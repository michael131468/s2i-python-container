Python container images
=======================
[![Build and push container images to Quay.io registry](https://github.com/sclorg/s2i-python-container/actions/workflows/build-and-push.yml/badge.svg)](https://github.com/sclorg/s2i-python-container/actions/workflows/build-and-push.yml)

Images available on Quay are:
* CentOS 7 [python-27](https://quay.io/repository/centos7/python-27-centos7)
* CentOS 7 [python-38](https://quay.io/repository/centos7/python-27-centos7)
* RHEL 8 [python-39-minimal](https://quay.io/repository/sclorg/python-39-minimal-el8)
* CentOS Stream 9 [python-39-c9s](https://quay.io/repository/sclorg/python-39-c9s)
* Fedora [nginx-39](https://quay.io/repository/fedora/python-39)
* Fedora [nginx-310](https://quay.io/repository/fedora/python-310)

This repository contains the source for building various versions of
the Python application as a reproducible container image using
[source-to-image](https://github.com/openshift/source-to-image).
Users can choose between RHEL, Fedora and CentOS based builder images.
The resulting image can be run using [podman](https://github.com/containers/libpod) or
[docker](http://docker.io).

For more information about using these images with OpenShift, please see the
official [OpenShift Documentation](https://docs.okd.io/latest/openshift_images/using_images/using-s21-images.html).

For more information about concepts used in these container images, see the
[Landing page](https://github.com/sclorg/welcome).

Note: while the examples in this README are calling `podman`, you can replace any such calls by `docker` with the same arguments

Contributing
---------------
In this repository [distgen](https://github.com/devexp-db/distgen/) > 1.0 is used for generating directories for Python versions. Also make sure distgen imports the jinja2 package >= 2.10.

Files in directories for a specific Python version are generated from templates in the src directory with values from specs/multispec.yml.

A typical way how to contribute is:

1. Add a feature or fix a bug in templates (src directory) or values (specs/multispec.yml file).
1. Commit the changes.
1. Regenerate all files via `make generate-all`.
1. Commit generated files.
1. Test changes via `make test TARGET=fedora VERSIONS=3.9` which will `build`, `tag` and `test` an image in one step.
1. Open a pull request!

For more information about contributing, see
[the Contribution Guidelines](https://github.com/sclorg/welcome/blob/master/contribution.md).

Versions
---------------
Python versions currently provided are:
* [python-2.7](2.7)
* [python-3.6](3.6)
* [python-3.8](3.8)
* [python-3.9](3.9)
* [python-3.9 Minimal (tech-preview)](3.9-minimal)
* [python-3.10](3.10)

RHEL versions currently supported are:
* RHEL 7 ([catalog.redhat.com](https://catalog.redhat.com/software/containers/search))
* RHEL 8 ([catalog.redhat.com](https://catalog.redhat.com/software/containers/search))

CentOS versions currently supported are:
* CentOS 7 ([quay.io/centos7](https://quay.io/organization/centos7))

Fedora versions currently supported are:
* Fedora 36 ([registry.fedoraproject.org](https://quay.io/organization/fedora))

Download
--------
To download one of the base Python images, follow the instructions you find in registries mentioned above.

For example, Centos image can be downloaded via:

```
$ podman pull quay.io/centos7/python-38-centos7
```

Build
-----
To build a Python image from scratch run:

```
$ git clone https://github.com/sclorg/s2i-python-container.git
$ cd s2i-python-container
$ make build TARGET=centos7 VERSIONS=3.8
```

Where `TARGET` might be one of the supported platforms mentioned above.

**Notice: By omitting the `VERSIONS` parameter, the build/test action will be performed
on all provided versions of Python.**

Usage
-----
For information about usage of S2I Python images, see the documentation for each version in its folder.

Test
----
This repository also provides a [S2I](https://github.com/openshift/source-to-image) test framework,
which launches tests to check functionality of simple Python applications built on top of the s2i-python-container image.

```
$ cd s2i-python-container
$ make test TARGET=centos7 VERSIONS=3.8
```

Where `TARGET` might be one of the supported platforms mentioned above.

**Notice: By omitting the `VERSIONS` parameter, the build/test action will be performed
on all provided versions of Python.**


Repository organization
-----------------------
* **`<python-version>`**

    * **Dockerfile**

        CentOS based Dockerfile.

    * **Dockerfile.fedora**

        Fedora based Dockerfile.

    * **Dockerfile.rhel7** & **Dockerfile.rhel8**

        RHEL 7/8 based Dockerfile. In order to perform build or test actions on this
        Dockerfile you need to run the action on a properly subscribed RHEL machine.

    * **`s2i/bin/`**

        This folder contains scripts that are run by [S2I](https://github.com/openshift/source-to-image):

        *   **assemble**

            Used to install the sources into the location where the application
            will be run and prepare the application for deployment (eg. installing
            dependencies, etc.)

        *   **run**

            This script is responsible for running the application by using the
            application web server.

        *   **usage***

            This script prints the usage of this image.

    * **`test/`**

        This folder contains a [S2I](https://github.com/openshift/source-to-image)
        test framework with multiple test aplications testing different approaches.

        * **run**

            Script that runs the [S2I](https://github.com/openshift/source-to-image) test framework.

