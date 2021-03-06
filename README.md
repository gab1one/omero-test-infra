OMERO Test Infra
================

`omero-test-infra` provides simplified integration testing
for projects that are built on OMERO.server or OMERO.web.

Any repository that relies on an existing OMERO installation
can depend on this directory whether for testing in travis
or locally.

Setup
-----

The directory MUST BE located at .omero at the top-level of
your source code. This can be achieved most simply by:

```
git clone git://github.com/openmicroscopy/omero-test-infra .omero
```

but adding the repository as a submodule or downloading a
zip file is also permissible.

Basic usage
-----------

`.omero/compose up` will start a basic OMERO stack including
PostgreSQL, OMERO.server, and OMERO.web using the current
production docker images. The definition of the stack is available
in `docker-compose.yml` with a number of environment variables
being specified in `.env`.

All docker compose environment variables like COMPOSE_FILE will be
respected, except for COMPOSE_PROJECT_NAME which has special handling
via the PROJECT environment variable.

Docker commands
---------------

To run a full build and install the desire applications, it is
recommended to use

 * .omero/docker NAME

where NAME is `app`, `cli`, `lib` or `scripts`. The special `srv`
command will build the parent directory as a replacement for the
OMERO.server container.

It is also possible to combine several application e.g.

 * .omero/docker app scripts

but each of the applications must pass when it is run alone as well.

These commands should be invoked by the `script` step in the project's
.travis.yml file. Set any environment variables as necessary.

Hooks
-----

Each command runs a number of subcommands or tasks which can be
overridden by placing an executable file of the same name in an
`.omeroci` directory *beside* the `.omero` directory.

 * `app-deps` installs dependencies for apps
 * `app-srv` gives apps a chance to modify the server
 * `py-check` runs standard Python quality tools
 * `py-common` runs standard Python package build steps
 * `py-setup` builds and install the Python package

 Helper scripts
 --------------

 * `wait-on-login` periodically attempts to login to OMERO.server
 * `persist.sh` generates tarballs of the contents of the
   OMERO.server and PostgreSQL containers to disk as well as
   restores from these tar balls.
 * `download.sh` downloads and unpacks a zip file containing
   persisted tarballs.

 Environment variables
 ---------------------

 * `DOCKER_ARGS` will be passed to `docker run ... test` to allow
   mounting additional directories or exposing ports.
