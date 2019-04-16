# k8s-setup

## Description
Ansible playbooks for setting up a k8s cluster with Rancher in high availability setup.

## Prerequisites
### Nodes
* **L4 load balancer (TCP)** (e.g., AWS NLB and nginx) - For instructions see https://rancher.com/docs/rancher/v2.x/en/installation/ha/create-nodes-lb/nlb/ and https://rancher.com/docs/rancher/v2.x/en/installation/ha/create-nodes-lb/nginx/.
* **Three nodes** running **Ubuntu** - To these nodes the load balancer should forward TCP/80 and TCP/443.
* **SSH access** to the three nodes.
* **Client host** or **remote host** (in the following named **commander node**) running **Ubuntu** and **Ansible**.

**Notes:** The current version of the playbooks requires **Ubuntu** to be running on **nodes**.

### Ansible
* **Ansible** must be installed on the **commander node** (see https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-ansible-on-ubuntu-18-04).

    ```
    sudo apt update
    sudo apt install software-properties-common
    sudo apt-add-repository ppa:ansible/ansible
    sudo apt update
    sudo apt install ansible
    ```

## Execution of the Playbooks
### Prepare the inventory
Set the **values** for the **variables** in the **inventory** files **hosts**.

| Variable                       | Description                                      |
| -------------------------------|:-------------------------------------------------|
| ansible_host                 | The public ip address of the node                  |
| internal                     | The private ip address of the node                 |
| ansible_user                 | The user to be logged in as via SSH                |
| ansible_ssh_private_key_file | The directory where your private SSH key is placed |
| hostname                     | The public DNS record for the kubernetes cluster   |

### Run the playbooks
Run the playbooks from the directory where they are placed.

```
ansible-playbook -i hosts setup.yml
```

For debug mode, run:

```
ansible-playbook -i hosts setup.yml -vvv
```

## Architecture
More details on the architecture of the Kubernetes cluster can be found at https://rancher.com/docs/rancher/v2.x/en/installation/ha/.

## Results
* All the Kubernetes **nodes** must be in **available** state.
* All **pods** must be **running** and **ready**.
* You are able to point a browser to the hostname you picked and yould see the login page.

## References
* https://rancher.com/docs/rancher/v2.x/en/installation/ha/