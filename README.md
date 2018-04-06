
Placecat is a source of experiments with the OpenStack Placement
service, a standalone docker container, and some gabbi tests.

It exists to model some of the more esoteric functionality in
placement in a quick and dirty way.

The container is built from
[placedock](https://github.com/cdent/placedock). Build that so you
have a 'placedock:1.0' container around. The README in _placedock_
can help with that aspect of things.

The [gabbi](https://github.com/cdent/gabbi) tests use the command
line runner, `gabbi-run`, to run various YAML-based files. The YAML
files are extensively commented to describe what they are about.

A `dockerenv` file is present for starting up the container with the
right (and simple) configuration.
