## Summary

This rockcraft.yaml implements an image for the [filebrowser tool](github.com/filebrowser/filebrowser).


## Developer notes

Upstream builds their image [here](https://github.com/filebrowser/filebrowser/blob/d49c3dfacfc0ff07e620b3ad2700e64927b06235/.github/workflows/main.yaml#L69), where they:

* [build the frontend](https://github.com/filebrowser/filebrowser/blob/d49c3dfacfc0ff07e620b3ad2700e64927b06235/.github/workflows/main.yaml#L87), really just doing [`cd frontend && npm ci && npm run build`](https://github.com/filebrowser/filebrowser/blob/d49c3dfacfc0ff07e620b3ad2700e64927b06235/Makefile#L12)
* [build the filebrowser executable](https://github.com/filebrowser/filebrowser/blob/d49c3dfacfc0ff07e620b3ad2700e64927b06235/.github/workflows/main.yaml#L94) (mentioned as the `backend`) using GoReleaser, which builds a [matrix of images](https://github.com/filebrowser/filebrowser/blob/d49c3dfacfc0ff07e620b3ad2700e64927b06235/.goreleaser.yml) and publishes them based on [this dockerfile](https://github.com/filebrowser/filebrowser/blob/d49c3dfacfc0ff07e620b3ad2700e64927b06235/Dockerfile) and other similar dockerfiles.  Because the frontend is built and present when building the executable backend, the frontend is embedded into the backend's executable.
* Add a `HEALTHCHECK` which executes `healthcheck.sh`, a script that probes the running filebrowser at /health to see if it is alive.  This we replicate by adding the healthcheck script and implementing the pebble `checks`
* define the filebrowser's default configuration by copying `docker_config.json` into the image as `/.filebrowser.json`, which filebrowser will look at by default
* provide an empty default /srv directory which will be the root of the filesystem shown to users, unless otherwise specified

To update this image, look at the changes to all these linked files and infer from those what we need to change for our rock.  
