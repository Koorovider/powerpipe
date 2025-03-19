# Powerpipe Setup Guide

## Installing Powerpipe
[Download Powerpipe](https://powerpipe.io/downloads?install=linux)

## Initializing Powerpipe Module
[Module Initialization](https://powerpipe.io/docs/reference/cli/mod)
```sh
powerpipe mod init
```

## Installing Plugins
[Managing Plugins](https://hub.steampipe.io/)
```sh
powerpipe mod install github.com/turbot/steampipe-mod-gcp-compliance
powerpipe mod install github.com/turbot/steampipe-mod-gcp-insights
powerpipe mod install github.com/turbot/steampipe-mod-aws-insights
powerpipe mod install github.com/turbot/steampipe-mod-aws-compliance
powerpipe mod install github.com/turbot/steampipe-mod-aws-well-architected
powerpipe mod install github.com/turbot/steampipe-mod-aws-thrifty
```

## Managing Workspaces
[Workspace Management](https://powerpipe.io/docs/run/workspaces#managing-workspaces)
```hcl
workspace "default" {
  listen        = "network"
  port          = 8080
#  memory_max_mb = 2048
}
```

# Firewall and Kernel Network Configuration

## Firewall Configuration
Execute the following commands to open specific ports and set up port forwarding in the firewall.

```sh
# Allow TCP traffic on port 80 in the public zone
firewall-cmd --zone=public --add-port=80/tcp --permanent

# Forward incoming traffic on port 80 to 127.0.0.1:8080
firewall-cmd --add-forward-port=port=80:proto=tcp:toport=8080:toaddr=127.0.0.1 --permanent

# Apply firewall settings
firewall-cmd --reload
```

## Kernel sysctl Configuration
Add the following lines to `/etc/sysctl.conf` to enable IP forwarding and local network routing.

```ini
net.ipv4.ip_forward = 1
net.ipv4.conf.eth0.route_localnet = 1
```

Apply the settings with:

```sh
sysctl -p
```