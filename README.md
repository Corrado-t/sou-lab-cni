# sou-lab-cni

# Set with Vagrant and Ansible Monitoring and Load Balancing with HAProxy, Prometheus, and Grafana on container using Podman

## Overview
This project use Vagrant, Ansible and Podman to set up a monitoring and load balancing stack with **HAProxy** as the reverse proxy and load balancer, **Prometheus** for monitoring and collecting metrics, and **Grafana** for visualizing those metrics. #to edit The setup also includes SSL configuration and logging for troubleshooting.

## Prerequisites

- **Vagrant** installed for automated VMs creation.
- **VirtualBox** installed for run the vms.

## Architecture


## How to use it

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

### Connect to instance

By default instance will be launched as per below:
HaProxy: 192.168.56.2:8404
Grafana: 192.168.56.66:3000
Prometheus: 192.168.56.66:9090

The following Cname are mapped to the services
prometheus.local:8404
grafana.local:8404

Grafana and Prometheus can be tested passing Ip and port address on browser URL.
To test HAProxy add in /etc/host

```bash
192.168.56.2    grafana.local
192.168.56.2    prometheus.local
```
You can then access the services via:

Grafana: http://grafana.local:8404
Prometheus: http://prometheus.local:8404


curl -v --cacert /Users/corradotiberio/Documents/Project/sou-lab-cni/sou_haProxy/certs/haproxy.crt -H "Host: prometheus.sou.local" https://prometheus.sou.local:8443

curl -v --cacert /Users/corradotiberio/Documents/Project/sou-lab-cni/sou_haProxy/certs/haproxy.crt -H "Host: grafana.sou.local" https://grafana.sou.local:8443