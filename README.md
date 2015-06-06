# A container for running discourse_docker tools

## Background

The way the Discourse project currently uses Docker is kind of annoying. Containers are treated like VMs (sort of), being  provisioned and managed using some tooling on top of `docker exec` and `docker commit`. Importantly, for some of the dependencies provisioned in this way, important side effects happen to persistent data during the build process, thus rendering the container images non-portable and forcing you to have the toolchain on your Docker host instead of uploading images built elsewhere and simply launching them.

I use Atomic hosts exclusively in production, and among other things the discourse_docker `launcher` requires git to keep itself up to date. An SCM tool like git has no place in a container host OS, so this container exists to provide an environment where the tools from the discourse_docker project can be used, because I have better things to do than maintain a fork of Discourse's packaging methodology.

## Usage 

Launch this interactively with privileges, host networking, and the Docker socket and /var/discourse mounted from the host:

    sudo docker run -it --rm --privileged --net=host -v /var/run/docker.sock:/var/run/docker.sock -v /var/discourse:/var/discourse koinu/discourse-launcher-env bash
