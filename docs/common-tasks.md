### Starting the Kubernetes cluster

The Kubernetes cluster is created by the ansible playbook `playbook.yml`

### Deploying Rook and Ceph

`kubectl apply -f ceph/crds.yml -f ceph/common.yml -f ceph/operator.yml`

`kubectl apply -f ceph/cluster.yml`

`kubectl apply -f ceph/storageclass.yml`

### Deploying mysql

mysql is needed for Nextcloud and many other services. We deploy a single shared mysql server but it is possible for each pod to contain its own seperate instance. The reason for centralizing mysql usage is to reduce resource consumption.

mysql's persistant volume claim is backed by a `rook-ceph-block`.

`kubectl apply -f mysql/configmap.yml -f mysql/pvc.yml -f mysql/pod.yml`

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

### 