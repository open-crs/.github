<center>
    <img width="100" src="https://raw.githubusercontent.com/CyberReasoningSystem/wiki/main/logo/export.png"/>
    <h1>OpenCRS</h1>
</center>

## About

OpenCRS is an open-source [cyber reasoning system (CRS)](https://www.trailofbits.com/services/published-research/cyber-reasoning-system-crs) capable of detecting, exploiting, and patching vulnerabilities in `i386` ELF executables, built from C codebases.

## Repositories

### CRS Modules

- **[`dataset`](https://github.com/CyberReasoningSystem/dataset)** for compiling and managing vulnerable programs
- **[`attack_surface_approximation`](https://github.com/CyberReasoningSystem/attack_surface_approximation)** for discovering the attack surface in an executable
- **[`vulnerability_detection`](https://github.com/CyberReasoningSystem/vulnerability_detection)** for finding vulnerabilities in executables
- **[`vulnerability_analytics`](https://github.com/CyberReasoningSystem/vulnerability_analytics)** for analyzing found vulnerabilities to extract more information (for example, the root cause)
- **[`automatic_exploit_generation`](https://github.com/CyberReasoningSystem/automatic_exploit_generation)** for automatically generating exploits

### Helpers

- **[`opencrs_dataset`](https://github.com/CyberReasoningSystem/opencrs_dataset)** for storing 54k vulnerable ELF executables
- **[`nist_c_test_suite`](https://github.com/CyberReasoningSystem/nist_c_test_suite)** for storing NIST's "C Test Suite for Source Code Analyzer v2 - Vulnerable" dataset
- **[`vagrant_infra`](https://github.com/CyberReasoningSystem/vagrant_infra)** for creating VMs with OpenCRS's modules
- **[`commons`](https://github.com/CyberReasoningSystem/commons)**, with utility functions and classes, enums, and interfaces that are used in multiple CRS modules
- **[`zeratool_lib`](https://github.com/CyberReasoningSystem/zeratool_lib)**, a fork of Zeratool for migrating the CLI tool into a Python 3 library for exploiting executables on the local machine

### Meta

- **[`wiki`](https://github.com/CyberReasoningSystem/wiki)** as a non-functional, meta-repository for describing how OpenCRS works as an organization and storing miscellaneous information
- **[`awesome-binary-analysis`](https://github.com/CyberReasoningSystem/awesome-binary-analysis)** for helpful binary analysis tools and research materials

## Requirements

OpenCRS requires a Linux (or Linux-like) environment with [Docker](https://www.docker.com/) and [Python](https://www.python.org/) >= 3.10 available.
Windows with [Windows Subsystem for Linux (WSL)](https://learn.microsoft.com/en-us/windows/wsl/) may work.

The following packages are required:

- `build-essential` / `base-devel` / `@development-tools` (the meta-package that includes `make`, `gcc` and other development-related packages)
- `sudo`
- `git`
- `curl`
- `wget`
- `python3`
- `docker`, with common plugins `docker-buildx-plugin`, `docker-compose-plugin`

On Ubuntu/Debian or other `apt`-based distributions, use, as `root`, the following command to install the requirements:

```console
apt install -y --no-install-recommends \
  build-essential \
  sudo \
  git \
  curl \
  wget \
  gcc-multilib \
  python3 \
  python-is-python3
```

### Install Docker

To install Docker, follow the official instructions.
Install either [Docker Engine](https://docs.docker.com/engine/install/) or [Docker Desktop](https://www.docker.com/products/docker-desktop/).

For Ubuntu, you can run the following commands:

```console
sudo apt-get -y update
sudo apt-get -y install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get -y update
sudo apt-get -y install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Be sure to run the [Docker post-install steps](https://docs.docker.com/engine/install/linux-postinstall/).
Namely:

1. Add the `docker` group and add the current user to the group:

   ```console
   sudo groupadd docker
   sudo usermod -aG docker $USER
   ```

1. Log in to the new group, by doing one of the two steps.

   1. Log out of your session and log in again.
      This is the best option since it will then create future sessions with required privileges.

   1. Run the command below to get privileges for the current session:

      ```console
      newgrp docker
      ```

## Set Up Environment

The recommended setup is to have all repositories in a given directory.
Typically this means running the commands below:

```console
mkdir open-crs
cd open-crs
test -d dataset || git clone --recurse-submodules https://github.com/open-crs/dataset dataset
test -d attack_surface_approximation || git clone --recurse-submodules https://github.com/open-crs/attack_surface_approximation
test -d vulnerability_detection || git clone --recurse-submodules https://github.com/open-crs/vulnerability_detection
test -d vulnerability_analytics || git clone --recurse-submodules https://github.com/open-crs/vulnerability_analytics
test -d automatic_exploit_generation || git clone --recurse-submodules https://github.com/open-crs/automatic_exploit_generation
test -d signature_generation || git clone --recurse-submodules https://github.com/open-crs/signature_generation
test -d opencrs_dataset || git clone --recurse-submodules https://github.com/open-crs/opencrs_dataset
test -d nist_c_test_suite || git clone --recurse-submodules https://github.com/open-crs/nist_c_test_suite
test -d vagrant_infra || git clone --recurse-submodules https://github.com/open-crs/vagrant_infra
test -d commons || git clone --recurse-submodules https://github.com/open-crs/commons
test -d zeratool_lib || git clone --recurse-submodules https://github.com/open-crs/zeratool_lib
```

If everything is OK, it should all look like this:

```console
$ ls -F
attack_surface_approximation/  commons/  nist_c_test_suite/  signature_generation/  vulnerability_analytics/  zeratool_lib/
automatic_exploit_generation/  dataset/  opencrs_dataset/    vagrant_infra/         vulnerability_detection/
```

### Python Environment

CRS modules are using Python.
Because of potentially different requirements of Python and Python libraries, you want to have control over their versions.
For that we recommend that you:

- Install and configure [`pyenv`](https://github.com/pyenv/pyenv).
- Create a [Python virtual environment](https://docs.python.org/3/library/venv.html) in each module.
- Install [Poetry](https://python-poetry.org/) in the Python virtual environment for each module.
- Install packages using the Poetry configuration files for each module.

#### Install `pyenv`

`pyenv` lets you easily switch between multiple versions of Python.

Install `pyenv` using the [official instructions](https://github.com/pyenv/pyenv).
Typically, on a Linux system, this means running the command:

```console
curl https://pyenv.run | bash
```

Follow the install instructions to configure your shell to use `pyenv`.

Then, to check the current installed versions, use:

```console
pyenv versions
```

Before installing a new Python version (it is typically build from source) install development packages for OpenSSL, Bzip2, SQLite3 and Readline.
On a Debian / Ubuntu system, this means running:

```console
sudo apt install libssl-dev libreadline-dev libbz2-dev lisqlite3-dev
```

To install a new Python version, use:

```console
python install <version_id>
```

where `<version_id>` is the version you want to install (e.g. `3.8`, `3.10`, `3.11` etc.)

To switch to another Python version, use:

```console
pyenv global <version_id>
```

#### Create a Python Virtual Environment

A [Python virtual environment](https://docs.python.org/3/library/venv.html) allows to have an isolated build and run environment for the Python applications.
You should create a separate virtual environment in each module.
That is, in each module directory, run:

```console
python -m venv .venv
source .venv/bin/activate
```

The second command activates the Python virtual environment.
The command-line prompt should have a `(.venv)` prefix to signal it is using a virtual environment.

**Note**: You can, at any point in time, deactivate the Python virtual environment by using the command:

```console
deactivate
```

To re-activate the Python virtual environment use, again, while in the module directory, the command:

```console
source .venv/bin/activate
```

#### Install Poetry

Poetry is a Python package manager that we use for all modules.
It is used to manage package dependencies and install required packages.

With the virtual environment activated, install [Poetry](https://python-poetry.org/):

```console
pip install poetry
```

The `poetry.lock` and the `pyprojects.toml` file in each module are the configuration files used by Poetry.
`poetry.lock` is generated from `pyprojects.toml` and contains the exact versions of installed packages, that are known to work.
