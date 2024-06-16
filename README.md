## Kubernetes Master Node Setup using Ansible

This repository provides Ansible playbooks to automate the setup of a Kubernetes cluster on master nodes. The playbooks cover the installation of Docker, the configuration of kubeadm, and the initialization of the Kubernetes master.

### Prerequisites

Before you begin, ensure you have the following prerequisites:

- Ansible installed on your local machine.
- SSH access to the target machines where Kubernetes will be installed.
- A non-root user with sudo privileges on each target machine.
- kubeadm, kubelet, and kubectl versions are compatible. This guide uses versions suitable for Kubernetes 1.24+.
- At least one master node.

### Files Description

- hosts.ini: An inventory file that lists all the hosts (servers) where the playbooks will be executed. This includes the master node(s).

- docker.yaml: An Ansible playbook that installs Docker on the target hosts. Docker is the container runtime used by Kubernetes.

- kubeadm.yaml: An Ansible playbook that sets up kubeadm and other necessary tools (kubelet, kubectl) on the master node(s).

- master.yaml: An Ansible playbook that initializes the Kubernetes master node using kubeadm init.

### Setup Instructions

#### 1. Update the Hosts File

The hosts.ini file needs to be configured with the IP addresses or hostnames of your master node(s). An example hosts.ini file is provided below:

    ```
    [masters]
    master1 ansible_host=192.168.1.100 ansible_user=your_username

    [kube-nodes]
    master1
    ```

Replace 192.168.1.100 with the IP address or hostname of your master node and your_username with the username used to SSH into your servers.

#### 2. Run the Playbooks

To set up your Kubernetes master node(s), execute the following steps

- Install Docker:
  Run the docker.yaml playbook to install Docker on all nodes listed in the hosts.ini file.

      ```
      ansible-playbook -i hosts.ini docker.yaml
      ```

- Install Kubernetes Components:

Run the kubeadm.yaml playbook to install kubeadm, kubelet, and kubectl on the master node(s).

```
ansible-playbook -i hosts.ini kubeadm.yaml
```

- Initialize the Kubernetes Master:
  Run the master.yaml playbook to initialize the Kubernetes master node.

```
ansible-playbook -i hosts.ini master.yaml
```

- Verifying the Setup
  After running the playbooks, verify the setup by logging into the master node and checking the status of the Kubernetes components:

```
ssh your_username@master1
kubectl get nodes
```

You should see the master node in a "Ready" state.

Troubleshooting
If you encounter any issues during the setup, consider the following:

Ensure all nodes are reachable via SSH.
Check that the versions of kubeadm, kubelet, and kubectl are compatible.
Review the Ansible playbook output for any errors and consult the Kubernetes or Docker documentation for troubleshooting tips.
Contributing
Contributions are welcome! If you find any issues or have suggestions for improvements, feel free to open an issue or a pull request.
