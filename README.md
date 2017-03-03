# Software AG webMethods CI Demo 

Continuous Integration Demo for webMethods

Currently includes:

* Integration Server demo packages and unit tests
* Integration Server demo environments
  * Dev
  * QA
  * Prod

## Requirements

* [Setup Command Central server](https://github.com/SoftwareAG/sagdevops-cc-server)
* [Setup CI infrastructure](https://github.com/SoftwareAG/sagdevops-ci-infra)

To get started clone this project

```bash
git clone https://github.com/SoftwareAG/sagdevops-ci-demo.git
cd sagdevops-ci-demo
git submodule init
git submodule update
```

Verify that your _antcc_ folder is not empty.


## Provisioning Dev Environment

Modify [environments/default/env.properties](environments/default/env.properties) file as needed
to point to your product and fix repositories as the license files:

```
# repositories
release=9.12
repo.product=products-${release}
repo.fix=fixes-${release}

# MUST HAVE a valid license key
is.license.key.alias=0000306067_Integration_Server912-lnxamd64
```

Provision Integration Server default/dev environment

```bash
ant up
```

## Build

```bash
ant build
```

## Deploy to Dev and Test

TODO:

```bash
ant deploy test
```


______________________
These tools are provided as-is and without warranty or support. They do not constitute part of the Software AG product suite. Users are free to use, fork and modify them, subject to the license agreement. While Software AG welcomes contributions, we cannot guarantee to include every contribution in the master project.
