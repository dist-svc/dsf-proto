# Getting Started

DSF consists of a service daemon that connects to and supports the DSF network, a Client (CLI) that allows users to interact with the daemon to manage services, and a client library supporting the implementation of DSF based services.


## Installation

- Daemon releases are available [here](https://github.com/dist-svc/dsf-daemon/releases/latest)
- Client releases are available [here](https://github.com/dist-svc/dsf-client/releases/latest)

## Usage

- First, run the daemon
  - If you installed the `.deb` package this provides a `dsfd.service` systemd unit that can be started with `systemctl start dsfd`
  - If you installed the daemon manually you will need to place this in a suitable location and ensure it is executed when required
  - `dsfd --help` will display available daemon options (and their defaults)

- Run `dsfc status` to check connectivity and fetch the daemon status
  - If this fails you may need to configure permissions on the unix socket used for communication with the daemon
  - The `.deb` package creates a `dsfd` user and group with permissions to access this socked, add your user to this group with `useradd USERNAME dsfd` where `USERNAME` is your username
- Run `dsfc --help` to display cli options



