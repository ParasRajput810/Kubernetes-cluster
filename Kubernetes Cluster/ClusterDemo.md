# Creating Kubernetes Cluster and Deploying nginx pod on Ubuntu


### Create Ubuntu instance and turn off the swap using :-

- `sudo swapoff -a`

#### Run apt get update

- `sudo apt-get update`

#### Install container runtime in this we are using docker.io

- `sudo apt install docker.io`
- `sudo chmod 666 /var/run/docker.sock`

#### Enter root user and execute following commands

- `sudo su -`
- `apt-get update && apt-get install -y apt-transport-https curl`
- `curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -`
- `cat <<EOF >/etc/apt/sources.list.d/kubernetes.list`
`deb https://apt.kubernetes.io/ kubernetes-xenial` `main`
`EOF`

#### Update the ubuntun again and install kubeadm , kubectl and kubelet

- `apt-get update`
- `apt-get install -y kubelet kubeadm kubectl`

#### Run below command in the master node 

- `kubeadm init --apiserver-advertise-address=<Ip -address-of-master-node> --pod-network-cidr=192.168.0.0/16 --ignore-preflight-errors=NumCPU`

**Please save the command output for reference. You will get "kubeadm join" command that you need to execute on worker node**

#### On the master, exit to the normal user, and execute the following commands:

- `mkdir -p $HOME/.kube`
- `sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config`
- `sudo chown $(id -u):$(id -g) $HOME/.kube/config`

#### Pod network policy installation (Here we use calico there are others also available)

- `kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.25.1/manifests/tigera-operator.yaml`

- `kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.25.1/manifests/custom-resources.yaml`

- `watch kubectl get pods -n calico-system`

#### Execute command which we got by initializing kubeadm in master node

- eg. `kubeadm join 171.31.11.81:6443 --token .....`

#### Check if the nodes are running perfectly or not by :-

- `Kubectl get nodes`

#### Deploy the nginx pod by following command 

- `kubectl run <name_of_pod> --image nginx`

