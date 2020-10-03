# UnRAID - pfELK Docker Container

This Docker image is based off of [sebp/elk](https://hub.docker.com/r/sebp/elk/) with the configuration of [pfelk](https://github.com/3ilson/pfelk). This will allow you to run pfELK on your UnRAID server.

## Requirements

- [x] [GeoIPUpdate MaxMind Container](docs/MaxMind-Container.md) UnRAID Configuration
- [x] [unraid-pfelk](docs/UnRAID-pfELK.md) UnRAID Configuration

### Documentation

- Documentation of the base container that this image is based on can be found on the [ELK Docker image documentation web page](http://elk-docker.readthedocs.io/).
- Documentation for pfELK can be found on `3ilson` GitHub page [here](https://github.com/3ilson/pfelk).

### Docker Hub

This image is hosted on Docker Hub at [https://hub.docker.com/r/noodlemctwoodle/unraid-pfelk/](https://hub.docker.com/r/noodlemctwoodle/unraid-pfelk/).

The following tags are available:

- `latest`

### About

Container written by [Sébastien Pujadas](https://pujadas.net), released under the [Apache 2 license](https://www.apache.org/licenses/LICENSE-2.0).
Pfelk written by [3ilson](https://github.com/3ilson), released under the [Apache 2 license](https://www.apache.org/licenses/LICENSE-2.0).
