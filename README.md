# sou-lab-cni

# Set with Vagrant and Ansible Monitoring and Load Balancing with HAProxy, Prometheus, and Grafana on container using Podman

## Overview
This project use Vagrant, Ansible and Podman to set up a monitoring and load balancing stack with **HAProxy** as the reverse proxy and load balancer, **Prometheus** for monitoring and collecting metrics, and **Grafana** for visualizing those metrics. #to edit The setup also includes SSL configuration and logging for troubleshooting.

## Prerequisites

- **Vagrant** installed for automated VMs creation.
- **VirtualBox** installed for run the vms.

## Architecture


## How to use it
By default vagrant will launch 1 vm frontend(HaProxy) and 1 vm backend(grafana and prometheus).
To launch the default setting follow the guidelines below.

### Launch
Ansible need to be version <2.17 in order to correctly works with VagrantBox generic/oracle8 and a podman collection to use the Roles in the project.
So to launch the environment is suggested to use a Python Venv to correctly set the ansible version.

1. **Create and activate a virtual environment:**
   ```bash
   python3 -m venv venv 
   source venv/bin/activate
   ```
2. **Install the compatible version of Ansible and the Podman collection:**

```bash
pip3 install "ansible-core<2.17"
ansible-galaxy collection install containers.podman
vagrant up
```
3. **Start the environment using Vagrant:**

```bash
vagrant up
```

### Custom option
| --be-node-number | Set the number of backend nodes to deploy           | 1    | int          |
|------------------|-----------------------------------------------------|------|--------------|
| --https          | Set whether to deploy HaProxy in https or http mode | true | [true/false] |

**Example:**
```bash
vagrant --be-node-number=2 --https=true up
```

### Connect to instance

By default instance will be launched as per below:
HaProxy: 192.168.56.2:8404
Grafana: 192.168.56.[66:126]:3000 
Prometheus: 192.168.56.[66:126]:9090

The following Cname are mapped to the services
prometheus.sou.local
grafana.sou.local

Grafana and Prometheus can be tested passing Ip and port address on browser URL.
To test HAProxy add in /etc/host

```bash
192.168.56.2    grafana.sou.local
192.168.56.2    prometheus.sou.local
```

####HTTP
You can then access the services via browser:

Grafana: http://grafana.sou.local:8404
Prometheus: http://prometheus.sou.local:8404

or via curl:
```bash
curl -v -H "Host: grafana.sou.local" http://grafana.sou.local:8404
curl -v -H "Host: prometheus.sou.local" http://prometheus.sou.local:8404
```
####HTTPS
To connect via browser crt need to added to trusted certificate of your browser os, crt can be found in sou_haProxy/certs/haproxy.crt inside this project

To connect via curl cert path need to be specified:

```bash
curl -v --cacert sou_haProxy/certs/haproxy.crt -H "Host: prometheus.sou.local" https://prometheus.sou.local:8443
curl -v --cacert sou_haProxy/certs/haproxy.crt -H "Host: grafana.sou.local" https://grafana.sou.local:8443
```
