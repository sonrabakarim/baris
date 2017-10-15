---
title: Jekyll, Docker, Windows, and 0.0.0.0
category: blog
---

Having used Jekyll with Docker on macOS successfully for a few months, I recently attempted to set up Jekyll using Docker on Windows. What I expected to be a relatively trivial exercise ended up involving issues with operating system networking, absolute URLs, and behaviour in Jekyll.

Here are a few things I learnt from the experience and how I finally got everything working.

## Jekyll, Docker, and 0.0.0.0

Running Jekyll inside a Docker container involves executing `jekyll serve` with the flag `--host=0.0.0.0` to allow Jekyll to be accessible outside the container on the host machine.

**Note:** The [official Jekyll Docker image](https://hub.docker.com/r/jekyll/jekyll/) passes `--host=0.0.0.0` by default, so it does not need to be set explicitly.
{: .notice--info}

This results in a server address of `http://0.0.0.0:4000/`, as shown in the `jekyll serve` output below:

```
Configuration file: _config.yml
            Source: /srv/jekyll
       Destination: /srv/jekyll/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
                    done in 5.556 seconds.
 Auto-regeneration: enabled for '/srv/jekyll'
    Server address: http://0.0.0.0:4000
  Server running... press ctrl-c to stop.
```

## Windows and 0.0.0.0

Unlike macOS, Windows does not allow connections to the 0.0.0.0 "any" address. The effect of this is the address `http://0.0.0.0:4000/` is not available on Windows.

Whether or not this is correct behaviour is subject to much discussion, which I won't cover here. However, the underlying cause is explained in the answer to this Stack Overflow question:
- [Socket.connect() to 0.0.0.0: Windows vs. Mac](https://stackoverflow.com/questions/11982562/socket-connect-to-0-0-0-0-windows-vs-mac/28230165#28230165)

> Unfortunately, connecting to the any address is not allowed on Windows.
> [...] as stated at the [Windows API Documentation](https://msdn.microsoft.com/en-us/library/windows/desktop/ms737625.aspx):
> > If the address member of the structure specified by the name parameter is filled with zeros, connect will return the error WSAEADDRNOTAVAIL.

On Windows, Jekyll should instead be accessed at `http://localhost:4000/`.

However, this will not solve the issue for a site with assets or links that use absolute URLs. These will still have a host of 0.0.0.0 and fail to be loaded, as can be seen in Chrome DevTools:

![Asset references with 0.0.0.0 hostname fail to load](/assets/images/jekyll-docker.png){: .align-center}

## `jekyll serve` and URLs

The cause of this is behaviour introduced in Jekyll 3.3:

> When you run `jekyll serve`, Jekyll will build your site with the value of the `host`, `port` [...]
> This happens by default when running Jekyll locally.
> <cite><a href="https://jekyllrb.com/news/2016/10/06/jekyll-3-3-is-here/#3-siteurl-is-set-by-the-development-server">site.url is set by the development server</a></cite>

When `jekyll serve` is run with `--host=0.0.0.0`, Jekyll sets the host portion of the site URL to 0.0.0.0 by default, regardless of the `url` setting in the config. This also affects any absolute URL that Jekyll generates.

This is not ideal for the combination of Jekyll, Docker, and Windows.

Fortunately, Jekyll provides a way to override this:

> If `JEKYLL_ENV` is any value except `development` (its default value), Jekyll will not overwrite the value of `url` in your config.

## Jekyll localhost workaround

The solution is to run `jekyll serve` so that it listens on 0.0.0.0 but without setting the server address to `http://0.0.0.0:4000/`.

This can be achieved with two changes:
1. Setting `JEKYLL_ENV` to a value other than `development`
2. Updating the configured `url` to an address that resolves locally eg. `http://localhost:4000`

An easy solution is to update the `url` value in `_config.yml` directly when running Jekyll locally. However, this adds the inconvenience of making sure to change the value when moving between local and production environments.

Jekyll's `--config` flag provides a way to override specific settings locally while allowing the default `_config.yml` to retain the production settings:

> Specify config files instead of using _config.yml automatically. Settings in later files override settings in earlier files.
> <cite><a href="https://jekyllrb.com/docs/configuration/#build-command-options">Build Command Options</a></cite>

First, create a file `_config.docker.yml` and set `url` to override the URL in `_config.yml`:

```yaml
url: "http://localhost:4000"
```

Then run Docker as follows:

```powershell
docker run --rm -e "JEKYLL_ENV=docker" -v ${PWD}:/srv/jekyll -p 4000:4000 -it jekyll/jekyll jekyll serve --config  _config.yml,_config.docker.yml
```

**Note:** The above command line is for PowerShell. For Windows Command Line (`cmd`), replace `${PWD}` with `%cd%`.
{: .notice--info}

The essential details are:

- `-e "JEKYLL_ENV=docker"` sets the environment variable `JEKYLL_ENV` in the container. This can be any value except `development`.
- `--config  _config.yml,_config.docker.yml` specifies the config files. Settings in `_config.docker.yml` override settings in `_config.yml`.

Instead of starting Docker with all these command line arguments, we can add this configuration to a `docker-compose.yml` file and use Docker Compose:

```yaml
version: '2'
services:
  site:
    environment:
      - JEKYLL_ENV=docker
    command: jekyll serve --config  _config.yml,_config.docker.yml
    image: jekyll/jekyll
    volumes:
      - .:/srv/jekyll
    ports:
      - 4000:4000
```

Then start the container with the command `docker-compose up`.

Using either of these methods, Jekyll will be available at `http://localhost:4000` with assets and links working correctly.

## References

- [Running Jekyll locally with Docker](https://kristofclaes.github.io/2016/06/19/running-jekyll-locally-with-docker/)
- [Jekyll GitHub repository](https://github.com/jekyll/jekyll)
- [Jekyll Docker GitHub repository](https://github.com/jekyll/docker)
- [Configuration - Jekyll](https://jekyllrb.com/docs/configuration/)
- [Jekyll Server >> Doesn't serve to host port expressly set, just localhost](https://github.com/jekyll/jekyll/issues/5599)
- [Cannot set site.url to a different address than the --host flag address in development](https://github.com/jekyll/jekyll/issues/5743)
