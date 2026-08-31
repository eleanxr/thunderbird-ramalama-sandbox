# Thunderbird Ramalama Sandbox

This repository includes necessary tools and setup for utilizing Ramalama for
local LLM-assisted development of Mozilla Thunderbird.

## Prerequisites

* Podman. On Linux systems, this can be installed using the normal mechanism
  for your distribution. On MacOS, I have found the easiest way to install
  podman is with [Podman Desktop](https://podman-desktop.io/).
* [Ramalama](https://github.com/containers/ramalama). On both MacOS and Linux,
  I have found the easiest way to
  install is with Homebrew.

## Set Up

Build the Goose container:

```
$ cd container/
$ podman build -t goose-thunderbird . -f Containerfile.ramalama
```

Choose a local model. I've found that qwen3.5:9b works reasonably well with the
Thunderbird codebase, but this is an area that should include experimentation:

```
$ ramalama pull qwen3.5:9b
```

Clone the repositories.

Follow the instructions on the [Thunderbird Developer](https://developer.thunderbird.net)
website for your platform to clone the Thunderbird repositories into the root
directory of this repository.  All of the agent instructions assume that the
source tree is cloned into a top-level subdirectory of this directory called
`source`

## Running

To start a development sandbox, run the following command in the directory
containing this file:

```
$ ramalama sandbox goose -w $PWD --goose-image goose-thunderbird qwen3.5:9b
```

This directory will be mounted in the container filesystem at `/work`.

## Working in the container

It is sometimes necessary for the human to execute commands in the container
environment, such as bootstrapping mach (although the agent can do this too).
To run a command in the Goose container, determine the name of the running
container with `podman ps` and run the following to obtain a shell in the
container:

```
$ podman exec -it <container-name> bash
```

## Ethical Considerations

The goal of this set up is to build an ethical way to utilize local LLMs
effectively to assist with Thunderbird development. Utilizing a local model
with a relatively small size is a step toward this goal, but the training of
the Qwen models is opaque. To further the goals of this set up, further
experimentation with models trained transparently and consensually on open data
is necessary.

## Future Directions

* Add more deterministic formal analysis tooling to the Goose container
  to enable Agents to call formal code analysis when performing tasks or
  answering questions.

