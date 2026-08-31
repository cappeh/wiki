## Prometheus

Prometheus is a **monitoring and alerting system** that collects and stores metrics as **time series data**.

**Time series data** is data recorded over time. Each sample has a value and a timestamp indicating when it was recorded.

A time series can also have **labels**, which are key:value pairs used to identify and differentiate different time series.

For example:

    http_requests_total{method="GET", status="200"}
    http_requests_total{method="POST", status="200"}

These are two different time series for the same metric because they have different label values.

## Metrics

A **metric** is basically a measurement of something, such as CPU usage, memory usage, request count, or request latency.

What we measure depends on the application or system we are monitoring.

For example:

- CPU usage of a VM
- Memory usage of a server
- Number of HTTP requests handled by a web server
- HTTP request latency
- Amount of storage being used

Metrics help us understand **what is happening inside a system** and can help us investigate why an application is behaving in a certain way.

For example, suppose users report that a web application is running slowly.

We could use metrics to investigate whether:

- The number of requests has increased
- Request latency has increased
- CPU usage has increased
- Memory usage has increased
- Some other resource is becoming a bottleneck

We might discover that request traffic has increased significantly and that this correlates with increased CPU usage and request latency.

This could lead us to scale the application by adding more servers/instances to handle the additional load.

### Important question when defining metrics

> **WHAT INFORMATION IS IMPORTANT TO DEBUG ANY ISSUES WITH THE APPLICATION?**

The metrics we collect should help us answer questions about the health and behaviour of the system.

## Basic Architecture

### Prometheus Server

The **Prometheus server** is responsible for:

- Scraping metrics from targets
- Storing the collected metrics as time series data
- Querying the stored metrics
- Evaluating alerting rules

### Targets

A **target** is something that Prometheus collects metrics from.

This could be:

- An application
- A server
- A VM
- A Raspberry Pi
- A database
- Another system that exposes Prometheus-compatible metrics

Prometheus typically collects metrics from a target by **scraping an HTTP endpoint** exposed by that target.

### Exporters

An **exporter** is a component that exposes metrics for a system that doesn't natively expose metrics in a format Prometheus can scrape.

For example, **node_exporter** exposes hardware and operating-system-level metrics from a machine, such as:

- CPU usage
- Memory
- Disk space
- Network statistics

Prometheus can then scrape `node_exporter` and store those metrics.

The basic flow is:

    System/Application
           |
           | exposes metrics
           v
        Target
           |
           | scraped by
           v
    Prometheus Server
           |
           | stores
           v
     Time Series Data

For systems that need an exporter:

    Machine
       |
       v
    node_exporter
       |
       | exposes metrics
       v
    Prometheus Server
       |
       v
    Time Series Data

## Basic Config

```yaml
global:
  scrape_interval: 15s      # how frequently Prometheus should scrape metrics

scrape_configs:
  - job_name: prometheus        # scraped metrics will have the job label: job="prometheus"
    static_configs:
      - targets: ["localhost:9090"]     # endpoint Prometheus scrapes metrics from: http://localhost:9090/metrics

```
