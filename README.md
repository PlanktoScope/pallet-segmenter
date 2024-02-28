# pallet-segmenter
An experimental Forklift pallet for running the PlanktoScope's segmenter separately on a computer

## Introduction

pallet-segmenter is a [Forklift](https://github.com/PlanktoScope/forklift) pallet
specifying a configuration of Forklift package deployments for running the PlanktoScope segmenter on
any Docker host.

## Usage

### Prerequisites

You will need to have the Docker Engine installed on your computer. Installation instructions are
available [here](https://docs.docker.com/engine/install/).

Then, you will need to set up the [`forklift`](https://github.com/PlanktoScope/forklift) tool on
your computer. Setup instructions are available
[here](https://github.com/PlanktoScope/forklift?tab=readme-ov-file#downloadinstall-forklift). Note
that currently `forklift` is only tested and built for Linux computers.

### Deployment

You can clone the latest commit of this Forklift pallet to your computer, by
using the `forklift` tool:
```
forklift plt clone github.com/PlanktoScope/pallet-segmenter@main
```

Then you can apply the cloned pallet on your computer using the following sequence of `forklift`
CLI commands:
```
forklift plt cache-repo
sudo -E forklift plt apply
```

Warning: this will replace all Docker containers on your Docker host with the package deployments
specified by this pallet and delete any Docker containers not specified by this pallet's package
deployments.

If your user is [in the `docker` group](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user)
(so that you don't need to use `sudo` when running `docker`
commands), then you can just run a single command instead of the two the commands listed above:

```
forklift plt switch github.com/PlanktoScope/pallet-segmenter@main
```

Then you can access the Node-RED dashboard from your web browser at
<http://localhost:1880/ui>, or you can programmatically interact with the segmenter over MQTT via
<mqtt://localhost:1883> (e.g. using [github.com/PlanktoScope/cli](https://github.com/PlanktoScope/cli)).
You can also access the Node-RED dashboard editor from your web browser at <http://localhost:1880/admin>.

The segmenter loads input datasets - and saves output files - in `~/.local/share/planktoscope/data`,
instead of the usual path on PlanktoScopes (`/home/pi/data`). Similarly, logs are saved in
`~/.local/share/planktoscope/device-backend-logs` instead of `/home/pi/device-backend-logs`.
The simplest way to add input datasets and download EcoTaxa export archives (working around issues
with file permissions between your user account and the `root` user used for running the segmenter)
will be to use the filebrowser in your web browser, at <http://localhost:9000>.

### Forking

To make your own copy of this repository for experimentation, you should fork this repository to a
new repository. Then, update the `path` fields of the `forklift-pallet.yml` and
`forklift-repository.yml` files to match the path of your new repository.

## Licensing

Forklift packages deployed by this pallet have their own software licenses, as specified in the
Forklift repositories where those packages are published. Any source code provided with this
Forklift pallet is covered by the following information, except where otherwise indicated:

Copyright Ethan Li and PlanktoScope project contributors

SPDX-License-Identifier: Apache-2.0 OR BlueOak-1.0.0

You can use the source code provided here either under the
[Apache 2.0 License](https://www.apache.org/licenses/LICENSE-2.0)
or under the [Blue Oak Model License 1.0.0](https://blueoakcouncil.org/license/1.0.0);
you get to decide. We are making the software available under the Apache license because it's
[OSI-approved](https://writing.kemitchell.com/2019/05/05/Rely-on-OSI.html),
but we like the Blue Oak Model License more because it's easier to read and understand.
