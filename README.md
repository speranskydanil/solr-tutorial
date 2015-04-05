# Solr Tutorial

This page describes installation of the Solr with Jetty (for many different projects on a server).

It's not multicore configuration: one project -- one Solr instance running with Jetty.

### Installing Java

    yum install java-1.7.0-openjdk-devel

### Installing Solr

    cd /usr/share
    wget http://apache-mirror.rbc.ru/pub/apache/lucene/solr/4.4.0/solr-4.4.0.zip
    unzip solr-4.4.0.zip
    rm solr-4.4.0.zip
    mv solr-4.4.0 solr

### Creating Instance Example

    cd /usr/share/solr
    mkdir instances
    cp -R example instances/example
    cd instances/example
    rm -R README.txt multicore example-DIH exampledocs example-schemaless
    cp /path/to/schema/from/repo/schema.xml solr/collection1/conf/schema.xml
    vim start.sh
    chmod +x start.sh
    vim stop.sh
    chmod +x stop.sh

Pay attention to `/path/to/schema/from/repo/schema.xml`

**start.sh**

    #!/bin/bash

    nohup java -jar start.jar > logs/jetty.log 2>&1 &
    echo $! > jetty.pid

**stop.sh**

    #!/bin/bash

    kill -9 `cat jetty.pid`
    rm jetty.pid

### Creating New Instance

    cd /usr/share/solr/instances
    cp -R example vsp
    cd vsp
    grep -rl '8983' ./ | xargs sed -i 's/8983/port/g'
    ./start.sh
    ./stop.sh

Pay attention to `port` (it can be 8981 for example)

## Rails Configuration

### config/sunspot.yml

```yaml
production:
  solr:
    hostname: localhost
    port: 8981
    log_level: WARNING
    path: /solr

development:
  solr:
    hostname: localhost
    port: 8981
    log_level: INFO
    path: /solr
```

### Reindexing

    rake sunspot:solr:reindex

**Author (Speransky Danil):**
[Personal Page](http://dsperansky.info) |
[LinkedIn](http://ru.linkedin.com/in/speranskydanil/en) |
[GitHub](https://github.com/speranskydanil?tab=repositories) |
[StackOverflow](http://stackoverflow.com/users/1550807/speransky-danil)

