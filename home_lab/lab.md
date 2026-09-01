# Home Lab

## Current Infrastructure
The lab currently consists of a single Raspberry Pi connected to my ISP router


                         Internet
                            │
                            │
                     ┌──────▼──────┐
                     │ ISP Router  │
                     └──────┬──────┘
                            │
                            │ Ethernet
                            │
                     ┌──────▼──────┐
                     │ Raspberry Pi│
                     │             │
                     │   Docker    │
                     └──────┬──────┘
                            │
          ┌─────────────────┼─────────────────┐
          │                 │                 │
    ┌─────▼─────┐     ┌─────▼──────┐    ┌────▼────────┐
    │   Node    │     │ Prometheus │    │ Uptime Kuma │
    │  Exporter │────►│            │    │             │
    └───────────┘     └─────┬──────┘    └─────────────┘
                            │
                            │ PromQL
                            │
                     ┌──────▼──────┐
                     │   Grafana   │
                     │ Dashboards  │
                     └─────────────┘

I currently use several monitoring components as you can see in the diagram to collect, store and visualize the health of my Raspberry Pi

- **Node Exporter** - This runs as a container on the pi that exposes system-level metrics such as CPU, Memory and Disk utilization
  as well as network traffic and other host information

- **Prometheus** - This collects and stores the metrics exposed by **Node Exporter** providing Time-Series data that can later be queried and analyzed

- **Grafana** - Connects to **Prometheus** to visualize the collected metrics through a dashboard.
  I currently use this to visualize Memory, CPU and Disk usage as well as network TX and RX statistics

- **Uptime Kuma** - This provides uptime and availability monitoring. I use this to check that my containers are running as well as performing
  basic ping tests to my ISP router and external internet routes. This provides useful statistics about the health of my network.
