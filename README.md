metrics_reader
=====

[![Hex pm](http://img.shields.io/hexpm/v/metrics_reader.svg?style=flat)](https://hex.pm/packages/metrics_reader)

An application that exposes Folsom metrics in common standardised formats.

This is important for several reasons:
 
- Applications provide a common interface for the same purpose
- Exposing application metrics in standardised formats allows existing tools
  to scrape these metrics e.g. Prometheus
- Knowledge is transferable when the same problem is solved in a consistent
  manner.  

The metrics reader exposes the application metrics via an rpc call to the
nodetool:

```
./bin/metrics_reader metrics
```

## The metrics reader server

Metrics should be registered with the metrics reader server if they are to be
reported via the console:

    metrics_reader:register(subscriber_acks).

It is also possible to de-register a registered metric:

    metrics_reader:deregister(subscriber_acks).

Finally, in order to examine the registered metrics:

    metrics_reader:metrics().

## The metrics observer server

For scalar metrics, it is possible to periodically snapshot the values and
record the resulting values in a histogram.  This allows richer statistics to be
derived from simple scalar values. Be aware that observing a metric will cause
the server to clear the metric's current value every time it is sampled.

In order to observe a metric, it is necessary to provide a name for the
histogram that will be created:

    metrics_observer:observe(subscriber_acks, subscriber_acks_per_second).

It is also possible to unobserve a metric:

    metrics_observer:unobserve(subscriber_acks).

## Configuration

    {format, prometheus_format}                         # defaults to prometheus exposition format
    {histogram_acc_interval_sec, 10}                    # defaults to 1 second
    {histogram_slide_interval_sec, 60}                  # defaults to 60 second

## Export formats

An `export_format` behaviour is defined that consists of the following callbacks:

    callback histogram(Name :: binary(), Histogram :: #{}) -> 
       [binary()].

An default `prometheus_format` formatter is already provided that will convert
the metric values to the prometheus textual exposition format.

## Build

```bash
$ rebar3 compile
```

In order to run tests...{to do}

## Release
```
$ rebar3 release
```
