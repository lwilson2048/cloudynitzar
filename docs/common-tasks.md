## Deploying a basic cloud

### Preparing the hardware

Currently Cloudy will work on Ubuntu 18.04, Debian 10, and Raspbian.

The Kubernetes cluster is only working on Arm64 and x86-64 Ubuntu 20.04.

### Build an inventory

The Ansible inventory file defines which hosts we will deploy to. You must be able to SSH into each remote host via public keys from the host you call `ansible-playbook` on. The property `k3s_control_node` is set on each node in order to build a high availibility cluster. The `docker` property is set because we want to use a docker based cluster. A `containerd` based cluster is possible but untested.

### Deploying Cloudy

Cloudy is deployed alongside the Kubernetes cluster and is independent of it. It is possible to deploy any combination of Cloudy and Kubernetes by (un)commenting lines in the file `playbook.yml`.

`ansible-playbook -i inventory.yml playbook.yml`

### Starting the Kubernetes cluster

Cloudy is deployed alongside the Kubernetes cluster and is independent of it. It is possible to deploy any combination of Cloudy and Kubernetes by (un)commenting lines in the file `playbook.yml`.

`ansible-playbook -i inventory.yml playbook.yml`

### Deploying Rook and Ceph

From this point on all paths are given relative to `{project_root}/kubernetes/`.

The order of application is important. The cluster must be applied *after* the operator.

`kubectl apply -f ceph/crds.yml -f ceph/common.yml -f ceph/operator.yml`

`kubectl apply -f ceph/cluster.yml`

`kubectl apply -f ceph/storageclass.yml`

### Deploying mysql

mysql is needed for Nextcloud and many other services. We deploy a single shared mysql server but it is possible for each pod to contain its own seperate instance. The reason for centralizing mysql usage is to reduce resource consumption.

mysql's persistant volume claim is backed by a `rook-ceph-block`.

`kubectl apply -f mysql/`

To check the status of the PVC:

`kubectl -n cloudy get pvc`

### Deploying Nextcloud

Nextcloud is an open source suite of collaboration, cloud storage, and file sharing software. It is a flexible and multi-user system that can be adapted to many use cases.

As of Commit 2e71fd9b3d3e96408d26c220c1783e44416acc3b exposing services is done via a NodePort. This will not scale but is useful for quickly exposing a service.

`kubectl apply -f nextcloud/`

Get the NodePort:

`kubectl -n cloudy get svc`

Look for a line like this:

`nextcloud-server   NodePort    10.43.107.82    <none>        80:31880/TCP        27m`

`31880` is the port we'll use to access nextcloud.

Find the IP of one of the Cloud/Kubernetes nodes and go to `<node ip>:31880` in your browser.

### Deploying Mosquitto MQTT

Mosquitto MQTT is a message broker. Many IoT and Home Automation tools use MQTT message brokers to communicate. This pod is therefore not required.

### Create the Rook toolbox pod

Rook includes a 'toolbox' pod that can be used to directly access Ceph. 

Create the container:

`kubectl create -f ceph/toolbox.yaml`

Wait for it to be ready and then join:

`kubectl -n rook-ceph exec -it deploy/rook-ceph-tools -- bash`

Some of the tools available:

`ceph status`

Sometimes commands will hang which indicates an unhealthy Ceph cluster. See [here](https://github.com/rook/rook/blob/master/Documentation/ceph-common-issues.md#cluster-failing-to-service-requests) for troubleshooting information.

[source](https://github.com/rook/rook/blob/master/Documentation/ceph-toolbox.md)

### Run a busy box container for network diagnoses

`kubectl run -it --rm --restart=Never busybox --image=busybox:1.28 -- ping 8.8.8.8`
