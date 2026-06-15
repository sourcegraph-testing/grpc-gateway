# How to contribute

## Getting started

This repository is a Go project that can be built and tested with either the
standard Go toolchain or with [Bazel](https://bazel.build) (the version pinned
in `.bazelversion`, currently `5.3.0`).

Prerequisites:

- Go (matching the version in `go.mod`; CI tests against Go 1.17, 1.18 and 1.19).
- Optionally, Bazel, if you want to reproduce the Bazel-based CI checks.
- Optionally, [`buf`](https://buf.build) if you want to regenerate protobuf code
  (`make install` will install a pinned version into `$GOBIN`).

Common workflows (run from the repository root):

```sh
# Build everything with the Go toolchain.
go build ./...

# Run the unit tests.
go test ./...

# Run the test target used by the Makefile, including the integration tests.
make test

# Build and test everything with Bazel (matches the Bazel CI job).
bazel build //...
bazel test //...

# Keep Bazel BUILD files and repositories.bzl in sync after Go changes.
bazel run //:gazelle
bazel run //:gazelle -- update-repos -from_file=go.mod -to_macro=repositories.bzl%go_repositories
bazel run //:buildifier
```

If you change `.proto` files or other generated inputs, regenerate the
checked-in artifacts by following the steps in
[I want to regenerate the files after making changes](#i-want-to-regenerate-the-files-after-making-changes)
below, then commit the resulting diff.

## Code reviews

All submissions, including submissions by project members, require review.

## I want to regenerate the files after making changes

### Using Docker

It should be as simple as this (run from the root of the repository):

```bash
docker run -v $(pwd):/grpc-gateway -w /grpc-gateway --rm ghcr.io/grpc-ecosystem/grpc-gateway/build-env:1.17 \
    /bin/bash -c 'make install && \
        make clean && \
        make generate'
docker run -itv $(pwd):/grpc-gateway -w /grpc-gateway --entrypoint /bin/bash --rm \
    ghcr.io/grpc-ecosystem/grpc-gateway/build-env:1.17 -c '\
        bazel run :gazelle -- update-repos -from_file=go.mod -to_macro=repositories.bzl%go_repositories && \
        bazel run :gazelle && \
        bazel run :buildifier'
```

You may need to authenticate with GitHub to pull `ghcr.io/grpc-ecosystem/grpc-gateway/build-env`.
You can do this by following the steps on the [GitHub Package docs](https://help.github.com/en/packages/using-github-packages-with-your-projects-ecosystem/configuring-docker-for-use-with-github-packages#authenticating-to-github-packages).

### Using Visual Studio Code dev containers

This repo contains a `devcontainer.json` configuration that sets up the build environment in a container using
[VS Code dev containers](https://code.visualstudio.com/docs/remote/containers). If you're using the dev container,
you can run the commands directly in your terminal:

```sh
$ make install && make clean && make generate
```

```sh
$ bazel run :gazelle -- update-repos -from_file=go.mod -to_macro=repositories.bzl%go_repositories && \
    bazel run :gazelle && \
    bazel run :buildifier
```

Note that the above-listed docker commands will not work in the dev container, since volume mounts from
nested docker container is not possible.

If this has resulted in some file changes in the repo, please ensure you check those in with your merge request.

## Making a release

To make a release, follow these steps:

1. Decide on a release version. The `gorelease` job can
   recommend whether the new release should be a patch or minor release.
1. Tag the release on `master`.
   1. The release can be created using the command line, or also through GitHub's [releases
      UI](https://github.com/grpc-ecosystem/grpc-gateway/releases/new).
   1. If you create a release using the web UI you can publish it as a draft and have it
      reviewed by another maintainer.
   1. Update the release description. Try to include some of the highlights of this release,
      ideally with links to the PRs and crediting the contributors.
1. Update the gorelease job in .github/ci.yaml to point to the new release version.
1. Sit back and pat yourself on the back for a job well done :clap:.
