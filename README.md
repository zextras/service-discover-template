# Carbonio Service Discover Template

Carbonio Service Discover Template provides template rendering, notifier, and supervisor capabilities for the Carbonio service-discover daemon. This package wraps HashiCorp's consul-template tool to enable dynamic configuration management based on Consul service discovery.

## Quick Start

### Prerequisites

- Docker or Podman installed
- Make

### Building Packages

```bash
# Build packages for Ubuntu 22.04
make build TARGET=ubuntu-jammy

# Build packages for Rocky Linux 9
make build TARGET=rocky-9

# Build packages for Ubuntu 24.04
make build TARGET=ubuntu-noble
```

### Supported Targets

- `ubuntu-jammy` - Ubuntu 22.04 LTS
- `ubuntu-noble` - Ubuntu 24.04 LTS
- `rocky-8` - Rocky Linux 8
- `rocky-9` - Rocky Linux 9

### Configuration

You can customize the build by setting environment variables:

```bash
# Use a specific container runtime
make build TARGET=ubuntu-jammy CONTAINER_RUNTIME=docker

# Use a different output directory
make build TARGET=rocky-9 OUTPUT_DIR=./my-packages
```

## Installation

This package is distributed as part of the [Carbonio platform](https://zextras.com/carbonio). To install:

### Ubuntu (Jammy/Noble)

```bash
apt-get install service-discover-template
```

### Rocky Linux (8/9)

```bash
yum install service-discover-template
```

## Usage

After installation, you can manage template instances using systemd:

### Managing Template Instances

```bash
# Enable and start a template instance for a specific service
systemctl enable service-discover-template@myservice.service
systemctl start service-discover-template@myservice.service

# Check status
systemctl status service-discover-template@myservice.service

# Manage multiple instances using the target
systemctl start service-discover-template.target
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for information on how to contribute to this project.

## License

The build scripts, patches, and configuration files in this repository are licensed under the GNU Affero General Public License v3.0 - see the [LICENSE.md](LICENSE.md) file for details.
