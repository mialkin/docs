# Creating cluster steps

We are going to use [Ansible](../ansible/ansible.md) for running commands on cluster nodes.

1\. [↑ Install Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) on your Linux/macOS computer.

2\.  If after installation `/etc/ansible/hosts` file doesn't already exist on your Linux/macOS machine, create it:

```bash
sudo mkdir /etc/ansible
sudo vim /etc/ansible/hosts
```

3\. Setup Ansible inventory file.

Here we assume that you will have 2 worker nodes and 1 master node all runing Ubuntu 20.04.

Put your nodes inside `hosts` inventory file mentioned above:

```text
[nodes]
node1 ansible_ssh_host=178.154.235.101 ansible_ssh_user=aleksei
node2 ansible_ssh_host=178.154.235.102 ansible_ssh_user=aleksei

[masters]
master1 ansible_ssh_host=178.154.235.126 ansible_ssh_user=aleksei

[all:vars]
ansible_python_interpreter=/usr/bin/python3
```

The user `aleksei` will be used for connecting to your nodes and running different tools. You can change the name of this user to whatever you like, but don't forget to change it in all other `yaml` scripts that we will execute below.

The `all:vars` subgroup sets the `ansible_python_interpreter` host parameter that will be valid for all hosts included in this inventory. This parameter makes sure the remote server uses the `/usr/bin/python3` Python 3 executable instead of `/usr/bin/python` (Python 2.7), which is not present on recent Ubuntu versions.

Make sure that Ansible can read your `hosts` inventory file:

```bash
ansible-inventory --list -y
```

4\. Do initial Ubuntu setup of all of your nodes, i.e. 2 worker nodes and master:

```bash
ansible-playbook provisioning.yaml
```

Here is the [provisioning.yaml](../ansible/provisioning.yaml) file. Download it to your computer and run the above command iside the folder with this file.

5\. Install Docker tools on all of your nodes:

```bash
ansible-playbook docker.yaml
```

Here is the [docker.yaml](../ansible/docker.yaml) file.

6\. Insall Kubernetes tools:

```bash
ansible-playbook kubernetes.yaml
```

Here is the [kubernetes.yaml](../ansible/kubernetes.yaml) file.

7\. Initialize Kubernetes cluster:

```bash
ansible-playbook master.yaml
```

Before running the above command please substitute the value of the `master_ip` variable in master file with the public static IP address of your master node.

Here is the [master](../ansible/master.yaml) file.

8\. Make sure that your cluster set up and running well:

```bash
kubectl get nodes
```

You should see output like this:

```text
NAME      STATUS   ROLES    AGE     VERSION
master3   Ready    master   1h9m    v1.19.4
```

9\. SSH to master node and run:

```bash
sudo cat /root/cluster_initialized.txt
```

You will see at the end of the output instructions of how to join your worker nodes to your master node:

```text
You can now join any number of control-plane nodes by copying certificate authorities
and service account keys on each node and then running the following as root:

  kubeadm join 178.154.232.118:6443 --token o4mk7q.raxthdrm1le6t5s1 \
    --discovery-token-ca-cert-hash sha256:90601f6f97b051b992ad8e16b2b90 \
    --control-plane 

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 178.154.232.118:6443 --token o4mk7q.raxthdrm1le6t5s1 \
    --discovery-token-ca-cert-hash sha256:90601f6f97b051b992ad8e16b2b90
```

Follow those instructions and join 2 of your worker nodes. After that run `kubectl get nodes`. You should see otput like this:

```text
NAME      STATUS   ROLES    AGE      VERSION
master3   Ready    master   1h14m    v1.19.4
node1     Ready    <none>   2m       v1.19.4
node2     Ready    <none>   1m       v1.19.4
```

Now you have Kubernetes cluster from 2 worker nodes and 1 master node that is ready to host new deployments.
