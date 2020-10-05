# UnRAID - pfELK Docker Container

**Version** 0.0.1 Alhpa

This project is a Frankenstein of open source projects, the config of [pfelk](https://github.com/3ilson/pfelk) and the container of [sebp/elk](https://hub.docker.com/r/sebp/elk/) with a few modifications to pull in the pfELK configuration files. I've not built a public container before so please bare with me.

I am open to suggestions for fixing any bugs. 👍 This will allow you to run pfELK on your UnRAID server.

## Container Requirements

- [x] [GeoIPUpdate MaxMind Container](docs/MaxMind-Container.md) UnRAID Configuration

- [x] [UnRAID-pfELK](docs/UnRAID-pfELK.md) UnRAID Configuration

![image](https://raw.githubusercontent.com/3ilson/pfelk/master/Images/dashboards.gif)

### Documentation

- Documentation of the base container that this image is based on can be found on the [ELK Docker image documentation web page](http://elk-docker.readthedocs.io/).
- Documentation for pfELK can be found on `3ilson` GitHub page [here](https://github.com/3ilson/pfelk).

### Docker Hub

This image is hosted on Docker Hub at [https://hub.docker.com/r/noodlemctwoodle/unraid-pfelk/](https://hub.docker.com/r/noodlemctwoodle/unraid-pfelk/).

The following tags are available:

- `latest`

### Known Issues with container

- The Container is showing `not available` in UnRAID for update, I've added it to my bugs, not sure how to fix it..
- Some Graphs are not loading `Embeddable factory [embeddableFactoryId = map] does not exist.` and `Embeddable factory [embeddableFactoryId = lens] does not exist.`
- logstash-plain.log - `[WARN ][logstash.outputs.elasticsearch][main][pfelk] Could not index event to Elasticsearch. {:status=>400, :action=>["index", {:_id=>nil, :_index=>"pfelk-2020.10.05", :routing=>nil, :_type=>"_doc"}, #<LogStash::Event:0x3096a57f>], :response=>{"index"=>{"_index"=>"pfelk-2020.10.05", "_type"=>"_doc", "_id"=>"c9Gc93QBUbTd2xKRryWC", "status"=>400, "error"=>{"type"=>"illegal_argument_exception", "reason"=>"Limit of total fields [1000] in index [pfelk-2020.10.05] has been exceeded"}}}}`

### Known Issues with guide

- For some reason `png` images are not showing in the guide or in my git, I'm not sure why I've added images a million times to Git but today they aren't working.

### About

Container written by [Sébastien Pujadas](https://pujadas.net), released under the [Apache 2 license](https://www.apache.org/licenses/LICENSE-2.0).
Pfelk written by [3ilson](https://github.com/3ilson), released under the [Apache 2 license](https://www.apache.org/licenses/LICENSE-2.0).
