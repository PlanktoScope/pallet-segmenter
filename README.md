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
that currently `forklift` is only tested for Linux computers.

### Deployment

The instructions below assume that you are using a version of the `forklift` tool which is greater
than or equal to v0.7.0 (but less than v0.8.0, which has not yet been released yet); other versions
of the `forklift` tool may behave differently and thus may require different commands than what is
described below:

#### First-time deployment

You can clone, stage, and apply the latest commit of this Forklift pallet to your computer, by
using the `forklift` tool:
```
sudo -E forklift pallet switch --apply github.com/PlanktoScope/pallet-segmenter@edge
```

Warning: this will replace all Docker containers on your Docker host with the package deployments
specified by this pallet and delete any Docker containers not specified by this pallet's package
deployments.

If your user is [in the `docker` group](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user)
(so that you don't need to use `sudo` when running `docker` commands), then you can avoid using
`sudo` with the `forklift` command:

```
forklift pallet switch --apply github.com/PlanktoScope/pallet-segmenter@edge
```

When this pallet is updated on GitHub and you want to apply the latest changes on your computer, you
can use the same command as above (either
`forklift pallet switch --apply github.com/PlanktoScope/pallet-segmenter@edge` or that command with
`sudo -E`) to clone, stage, and apply the updated version of the pallet.

#### Subsequent deployment

Because the `forklift` tool uses [Docker Compose](https://docs.docker.com/compose/) to manage the
Docker containers specified by this pallet, the containers will not be running after you restart
your computer (this is true each time you restart your computer); you will need to run a command to
start the containers again:

```
sudo -E forklift stage apply
```

Or if your user is in the `docker` group:

```
forklift stage apply
```

#### Operation

After you have applied the pallet so that the containers are running, you can access the Node-RED
dashboard from your web browser at <http://localhost:1880/ui>, or you can programmatically interact
with the segmenter over MQTT via <mqtt://localhost:1883> (e.g. using
[github.com/PlanktoScope/cli](https://github.com/PlanktoScope/cli)).
You can also access the Node-RED dashboard editor from your web browser at
<http://localhost:1880/admin>.

The segmenter loads input datasets - and saves output files - in folders within
`~/.local/share/planktoscope/data`, instead of the usual path on PlanktoScopes (`/home/pi/data`).
Similarly, logs are saved in `~/.local/share/planktoscope/device-backend-logs` instead of
`/home/pi/device-backend-logs`. The simplest way to add input datasets and download EcoTaxa export
archives (working around issues with file permissions between your user account and the `root` user
used for running the segmenter) will be to use the filebrowser app in your web browser, at
<http://localhost:9000>.

Before you can use the Node-RED dashboard, you will need to create a folder at
`~/.local/share/planktoscope/data/img` (which you can do by just using the filebrowser app to create
a new folder named `img` in the top level, at <http://localhost:9000/files/>), and then you should
upload/copy your input datasets into that folder. Then you can press the "Update acquisition's
folder list" button in the Node-RED dashboard, which should cause your input datasets to be listed
in the dashboard.

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
