This repository provides configuration files for setting up a public DNS resolver using **Unbound** and **dnsdist**.
The setup emphasizes security and privacy, with configurations optimized to minimize information leakage, enforce DNS security features, and protect against abuse.

[![Setup Guide](https://img.shields.io/badge/Setup%20Guide-264653?style=for-the-badge)](how-to-setup.md)

## Configuration Details

### unbound.conf

- **Purpose**: Configures Unbound as a recursive resolver.
- **Key Features**:
  - Listens on 127.0.0.1:1053 for queries from dnsdist.
  - Enforces DNSSEC validation and query minimization.
  - Hides server identity and version for privacy.
  - Blocks private IP address responses.
  - Optimizes caching and prefetching for performance.
  - Plain DNS is disabled.

### dnsdist.conf

- **Purpose**: Configures dnsdist as a DNS load balancer and frontend.
- **Key Features**:
  - Listens on all interfaces for DoT and DoQ (port 853), DoH and DoH3 (port 443).
  - Forwards queries to Unbound at 127.0.0.1:1053.
  - Drops CHAOS class and UPDATE opcode queries.
  - Implements dynamic rate-limiting for queries, NXDOMAIN, SERVFAIL, and ANY queries to prevent abuse.

### sysctl.conf

- **Purpose**: Optimizes kernel network settings.
- **Key Features**:
  - Increases socket buffer sizes for high DNS traffic.
  - Enables BBR congestion control for better TCP performance.
  - Hardens network stack against spoofing, redirects, and other attacks.

## Disclaimer

Running a public DNS resolver exposes your server to potential abuse.
Ensure you have adequate monitoring and other security measures in place.
I am not responsible for any misuse or damage caused by this configuration.
