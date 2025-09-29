# Equinor CycleCloud SLURM project

<!-- 

The source of this file is in https://github.com/equinor/hpc-azure-cyclecloud-slurm/blob/development/README-SLURM-Equinor.md

If changed, please copy to Subops repo under docs/implementation

Assuming that subops is a sibling repo, cut & paste this command:

cp -v README-SLURM-Equinor.md ../subops/docs/implementation/Azure-SLURM.md
 -->

Our [CycleCloud SLURM](https://github.com/equinor/hpc-azure-cyclecloud-slurm) repository is a fork of [Azure/cyclecloud-pbspro](https://github.com/Azure/cyclecloud-slurm), and is used in addition to our [main Azure CycleCloud repository](https://github.com/equinor/hpc-azure-cyclecloud)

In CycleCloud we use this OpenPBS component (a.k.a project in CC terms) on top of our main CycleCloud repository in [hpc-azure-cyclecloud](https://github.com/equinor/hpc-azure-cyclecloud) where the *shared-equinor* project is maintained

We have changed this from being a project in our main [hpc-azure-cyclecloud](https://github.com/equinor/hpc-azure-cyclecloud) into a separate fork to be able to pull updates from Azure master branch a lot easier. We can also issue pull requests to Azure developers.

**NOTE:** Never update the master branch. That will prevent us to merge in a safe and convenient way from the upstream Azure repository. The default/target branch to use for our fork is the *development* branch

## Features

**NOTE:** We also add a number of cluster init scripts -  not described here.

Our changes and additions provide the below for OpenPBS project as delivered from Azure:

- Use builtin python3.11 - 3.12 and beyond fails on `from requests.exceptions import ConnectionError`

- Skip package installs that are in our OS images

## Implementation 

From a CycleCloud server, you will need to use a python3.12 for the package builds:

```bash
# Create the venv
mkdir -p ~/venv
/usr/bin/python3.12 -m venv ~/venv/python3.12
./source ~/venv/python3.12/bin/activate

# Then build the packages - latest is 4.0.3 for SLURM 25.05.2
./util/build.sh

# Upload to CycleCloud blobs
cyclecloud project upload azure-storage
