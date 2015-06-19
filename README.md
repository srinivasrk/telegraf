# Telegraf - A native agent for InfluxDB

Telegraf is an agent written in Go for collecting metrics from the system it's running on or from other services and writing them into InfluxDB.

Design goals are to have a minimal memory footprint with a plugin system so that developers in the community can easily add support for collecting metrics from well known services and third party APIs.

We'll happily accept pull requests for new plugins and will manage the set of plugins that Telegraf supports. See the bottom of this doc for instructions on writing new plugins.

## Quickstart

* Build from source or download telegraf. Packages here:

```
http://get.influxdb.org/telegraf/telegraf_0.1.1_amd64.deb
http://get.influxdb.org/telegraf/telegraf-0.1.1-1.x86_64.rpm
```

* Run `telegraf -sample-config > telegraf.toml` to create an initial configuration
* Edit the configuration to match your needs
* Run `telegraf -config telegraf.toml -test` to output one full measurement sample to STDOUT
* Run `telegraf -config telegraf.toml` to gather and send metrics to InfluxDB

## Telegraf Options

Telegraf has a few options you can configure under the `agent` section of the config. If you don't see an `agent` section run `telegraf -sample-config > telegraf.toml` to create a valid initial configuration:

* **hostname**: The hostname is passed as a tag. By default this will be the value retured by `hostname` on the machine running Telegraf. You can override that value here.
* **interval**: How ofter to gather metrics. Uses a simple number + unit parser, ie "10s" for 10 seconds or "5m" for 5 minutes.
* **debug**: Set to true to gather and send metrics to STDOUT as well as InfluxDB.

## Plugin Options

There are 3 configuration options that are configurable per plugin:

* **pass**: An array of strings that is used to filter metrics generated by the current plugin. Each string in the array is tested as a prefix against metrics and if it matches, the metric is emitted.
* **drop**: The inverse of pass, if a metric matches, it is not emitted.
* **interval**: How often to gather this metric. Normal plugins use a single global interval, but if one particular plugin should be run less or more often, you can configure that here.
