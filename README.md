
[![Build Status](https://travis-ci.org/cdent/placecat.svg?branch=master)](https://travis-ci.org/cdent/placecat)

_Note: This requires version 1.41.0 of gabbi._

Placecat is a source of experiments with the OpenStack Placement
service, a standalone docker container, and some gabbi tests.

It exists to model some of the more esoteric functionality in
placement in a quick and dirty way.

The container is built from
[placedock](https://github.com/cdent/placedock). Build that so you
have a 'placedock:1.0' container around. The README in _placedock_
can help with that aspect of things.

Or, if you prefer, use an already built container from the [docker
hub](https://hub.docker.com/r/cdent/placedock/).

The [gabbi](https://github.com/cdent/gabbi) tests use the command
line runner, `gabbi-run`, to run various YAML-based files. The YAML
files are extensively commented to describe what they are about.

A `dockerenv` file is present for starting up the container with the
right (and simple) configuration.

If the running container is replaced, all data is gone.

Start the container with:

```
sudo docker run -t -p 127.0.0.1:8080:80 --env-file dockerenv placedock:1.0
```

Check it is working with:

```
curl http://localhost:8080/
```

A response like this indicates it is working:

```json
{"versions": [{"id": "v1.0", "max_version": "1.21", "min_version": "1.0"}]}
````

Accomplish the same thing by running:

```
gabbi-run http://127.0.0.1:8080 -- gabbits/version.yaml
```

# The Scenarios

The files within `gabbits` represent a few different self-contained
scenarios for working with placement.

* `version.yaml`: Show the output of the service version discovery
  document.
* `base_urls.yaml`: Explore each of the main URLs presented by the
  service.
* `two-computes.yaml`: The common usage scenario of placing a
  workload in a simple cloud.
* `fridge.yaml`: Making sandwiches with placement and a fridge, just
  to show what's possible from an entirely different point of view.

# Reminders

* If [placement
  master](https://git.openstack.org/cgit/openstack/nova) changes and
  those changes are desired in these tests, the container must be
  rebuilt.
