# 1. networkmanager
NetworkManager provides automatic network detection and configuration for the system.

## 1.1 Objective
* Provide core network configuration features
* Expose the features through on-disk text-based configuration
* Expose the features through D-Bus API
* Provide basic CLI and GUI (other CLI/GUI frontends can be built on top of NetworkManager)

## 1.2 Configuration
[wiki](https://developer-old.gnome.org/NetworkManager/stable/NetworkManager.conf.html)
default config `/etc/NetworkManager/NetworkManager.conf`
additional config directory `/etc/NetworkManager/conf.d`

The configuration file format is so-called key file. It consists of sections(groups) of key-value pairs. Sections are started by a header line containting the section name enclosed in []
1. main section
   1. dns
      1. default: NetworkManager will update /etc/resolv.conf to reflect the nameservers 
      2. dnsmasq NetworkNanager will run dnsmasq as a local caching nameserver
      3. systemd-resolved dns configuration will be pushed to systemd-resolved    
