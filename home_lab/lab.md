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
          ┌────────────┼────────────┐
          │            │            │
    ┌─────▼─────┐ ┌────▼─────┐ ┌───▼────────┐
    │   Node    │ │Prometheus│ │ Uptime Kuma│
    │  Exporter │ │          │ │            │
    └───────────┘ └──────────┘ └────────────┘

