
[![Build Status](https://travis-ci.org/cdent/placecat.svg?branch=master)](https://travis-ci.org/cdent/placecat)

_The automated builds of placecat are triggered whenever there are
changes in this repo or whenever a new
[placedock](https://github.com/cdent/placedock) container image is
created. This build status indicates whether the features and bugs
of the placement service have caught up with what the tests here
want to do._

_Note: This requires version 1.41.0 of gabbi._

Placecat is a source of experiments with the OpenStack Placement
service, a standalone docker container, and some gabbi tests.

It exists to model some of the more esoteric functionality in
placement in a quick and (hopefully) comprehensible way.

The container is built from
[placedock](https://github.com/cdent/placedock). You can choose to
build that yourself, following the instructions there. Or, if you
prefer, use an already built container from the
[docker hub](https://hub.docker.com/r/cdent/placedock/).

The [gabbi](https://github.com/cdent/gabbi) tests use the command
line runner, `gabbi-run`, to run various YAML-based files. The YAML
files are extensively commented to describe what they are about.

A `dockerenv` file is present for starting up the container with the
right (and simple) configuration.

If the running container is replaced, all data is gone.

Start the container with:

```
docker run -t -d -p 127.0.0.1:8080:80 --env-file dockerenv cdent/placedock
```

(You may need to use `sudo`, depending on your environment.)

Check it is working with:

```
curl http://localhost:8080/
```

A response like this indicates it is working:

```json
{
   "versions" : [
      {
         "status" : "CURRENT",
         "max_version" : "1.29",
         "min_version" : "1.0",
         "id" : "v1.0",
         "links" : [
            {
               "href" : "",
               "rel" : "self"
            }
         ]
      }
   ]
}
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
* `nested-fridge.yaml`: Like `fridge.yaml`, but with some of the
  things in the fridge being nested resource providers, so that we can
  distinguish amongst them using traits.
* `cloud-capability.yaml`: A big hack, experimenting with using
  placement as a way to represent cloud capabilities amongst several
  clouds.

# Reminders

* If [placement
  master](https://git.openstack.org/cgit/openstack/nova) changes and
  those changes are desired in these tests, the container must be
  rebuilt. I regularly rebuild the container on docker hub, but it is
  not yet automated (coming soon!).
