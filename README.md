docker-sbt-extras
====

This project creates a container that is capable of running paulp's
excellent sbt replacement script
(https://github.com/paulp/sbt-extras).

This container uses Java 1.8 (OpenJDK build 8u92) from the Alpine
Linux 3.4 distribution.

Building the Container
=====

Check out the repo and update submodules:

    $ git clone --recursive https://github.com/tozny/docker-sbt-extras

Build the container:

    $ docker build -t tozny/docker-sbt-extras .

Using SBT
====

The container exposes two volumes so JARs downloaded by SBT can be
cached: `/root/.sbt` and `/root/.ivy2`.

Source files should be mounted at `/root/project`. The `.sbt` and
`.ivy2` volumes are optional; source files are not. Assuming your
project is at `~/project`, build it like so:

    docker run -v ~/.sbt:/root/.sbt -v ~/.ivy2:/root/.ivy2 -v \
      ~/project:/root/project tozny/docker-sbt-extras sbt package

Note that build outputs will be written to the `/root/project` volume
(`~/project` on the host), as that is the default work directory.

Motivation
====

Many, many Scala projects contain a version of paulp's script in their
own repo. We didn't want to do that, because DRY. So instead we
include the sbt-extras repository as a submodule and copy it in when
building. As a bonus, it's easy to re-build the container with new
versions of the script!


