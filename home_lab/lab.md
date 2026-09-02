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


## PromQL Queries


| Metric | PromQL Query | Unit |
|---|---|---|
| **CPU Utilization** | `100 * (1 - rate(node_cpu_seconds_total{mode="idle"}[1m]))` | % |
| **Memory Total** | `node_memory_MemTotal_bytes / 1024^3` | GiB |
| **Memory Available** | `node_memory_MemAvailable_bytes / 1024^3` | GiB |
| **Memory Utilization** | `100 * (1 - node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes)` | % |
| **Network RX** | `rate(node_network_receive_bytes_total[5m])` | bytes/s |
| **Network TX** | `rate(node_network_transmit_bytes_total[5m])` | bytes/s |
| **Disk Space Available** | `node_filesystem_avail_bytes{device="/dev/mmcblk0p2", mountpoint="/"} / 1024^3` | GiB |
| **Disk Utilization** | `100 * (1 - (node_filesystem_avail_bytes{device="/dev/mmcblk0p2", mountpoint="/"} / node_filesystem_size_bytes{device="/dev/mmcblk0p2", mountpoint="/"}))` | % |

### CPU Utilization

`100 * (1 - rate(node_cpu_seconds_total{mode="idle"}[1m]))`
calculates ther percentage of CPU time for each core that is not idle over the previous 1 minute window
**Metric**: node_cpu_seconds_total
**Filter**: mode="idle" selects idle CPU time
**Function**: rate(..\[1m\]) calculates ther per-second rate of change over one minute
**Calculation**: 100*(1 - rate(..))
    - if the CPU is idle 30% of the time then (1 - 0.30) = 0.70
    - this means the CPU is 70% utilized after * 100

### Memory

#### Total Memory

`node_memory_MemTotal_bytes / 1024^3`
Returns the total amount of system memory. `/ 1024^3` converts the the value from bytes to GiB.

#### Available Memory

`node_memory_MemAvailable_bytes / 1024^3`
Returns the amount of memory currently available on the system. The value is converted to GiB.

#### Memory Utilization

`100 * (1 - node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes)`
calculates the percentage of memory that is currently being used

### Network Traffic

#### RX

`rate(node_network_receive_bytes_total\[5m\])`
calculates the average recevied network traffic rate over the previous 5 minutes.
- **Metric**: node_network_receive_bytes_total
- **Function**: rate(..\[5m\]) calculates the per-second rate of change

#### TX

`rate(node_network_receive_bytes_total\[5m\])`
calculates the average transmitted network traffic rate over the previous 5 minutes.
- **Metric**: node_network_transmit_bytes_total
- **Function**: rate(..\[5m\]) calculates the per-second rate of change

### Disk

#### Disk Space Available

`node_filesystem_avail_bytes{device="/dev/mmcblk0p2", mountpoint="/"} / 1024^3`
Returns the amount of available space on the root filesystem "/".
This is filtered to the drive "/dev/mmcblk0p2" where root is mounted
It is also converted to GiB

#### Disk Utilization

`100 * (1 - (node_filesystem_avail_bytes{device="/dev/mmcblk0p2", mountpoint="/"} / node_filesystem_size_bytes{device="/dev/mmcblk0p2", mountpoint="/"}))`
Calculates the percentage of the root filesystem currently in use

- **Available Space**: node_filesystem_avail_bytes
- **Total Space**: node_filesystem_size_bytes
- **Calculation**: 100*(1 - avail/total)
