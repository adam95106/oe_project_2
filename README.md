# Error Detection and Self-Healing Mechanism for Containerized Architecture

The project is to design a fault detection and self-healing system that is capable of monitoring the status of services in real time, detecting anomalous behavior, and automatically intervening to resolve issues.
The system specifically handles critical anomalies, such as CPU overload, memory exhaustion, and unexpected container termination.

## Architecture Overview

**Components:**
- **Docker:** Containerization platform that hosts and isolates all the monitoring stack components.
- **Prometheus:** Metrics collection and monitoring.
- **Grafana:** Visualization and dashboarding for metrics.
- **Node Exporter:** Hardware and OS metrics.
- **cAdvisor:** Container resource usage and performance metrics.
- **Alertmanager:** Handles alerts sent by Prometheus.
- **Loki:** Log aggregation system optimized for storing and querying logs
- **Promtail:** Lightweight log forwarder that collects logs and pushes them to Loki.
- **Ansible EDA Controller:** Event-driven automation controller that executes predefined Ansible playbooks based on incoming alerts.

## Getting Started
### Prerequisites
- **Ansible:** Ensure that Ansible is installed on your local machine.
- **SSH:** SSH must be installed and configured on both your local and remote machines.

***NOTE:*** This project currently only works on **Ubuntu 24.04 LTS** operating systems.

### Installation
1. Clone the repository.
    ```bash
    git clone https://github.com/adam95106/oe_project_2.git
    ```
2. Navigate to the project directory.
    ```bash
    cd oe_project_2/
    ```

### Configuring the Remote Machine with `bootstrap.yml` Role

The `playbooks/bootstrap.yml` playbook must be executed first, as it prepares the target host for role execution by setting up the necessary prerequisites (e.g., SSH-key pair).

```bash
ansible-playbook playbooks/bootstrap.yml \
  --extra-vars "ansible_user=<your-ssh-username>" \
  --ask-pass \
  --ask-become-pass \
  --ssh-common-args='-o StrictHostKeyChecking=no'
```

**Note:** Replace `<your-ssh-username>` with the actual username that has SSH access to the target host.

### Running the Ansible Playbooks
Before executing the main playbooks, make sure to configure the inventory file with the IP addresses of the target hosts.  
This is usually done by editing the `inventory/inventory.ini` file and adding the appropriate IPs under the relevant groups (e.g., `[monitoring_server]`, `[monitoring_clients]`).

Once the inventory is configured, you can run the main playbooks to deploy the monitoring and automation components.
```bash
ansible-playbook site.yml
```

In the following steps, deploying additional clients only requires re-running the playbook with the updated inventory:
```bash
ansible-playbook playbooks/clients.yml
```

Upon successful completion of the playbook, the Grafana dashboard will be accessible at `http://monitoringserverip:3000`. You can navigate to this address in your web browser to monitor and visualize system metrics collected from your servers.
