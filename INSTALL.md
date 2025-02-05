# Installing KNE

I'm running this on a RHEL 8.10 system.

```bash
$ cat /etc/os-release 
NAME="Red Hat Enterprise Linux"
VERSION="8.10 (Ootpa)"
```

## Requirements
### Install Docker

Add Docker repo.

```bash
...
```

Install Docker and let your user run `docker` commands (without `sudo`)

```bash
sudo yum install -y yum-utils docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo systemctl enable docker --now
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker
```

### Install kubectl

Kubernetes CLI.

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
export PATH=$PATH:/usr/local/bin/
```

### Install KIND

KIND is a tool for running local Kubernetes clusters using Docker container “nodes”.

```bash
export PATH=$PATH:$HOME/go/bin
go install sigs.k8s.io/kind@v0.26.0
```

### Install Go

We need Go to compile KNE's code.

```bash
sudo dnf install golang -y
```

### Clone KNE repository

```bash
git clone https://github.com/openconfig/kne.git
```

Install KNE

```bash
cd kne && make install
```

```bash
$ kne help
Kubernetes Network Emulation CLI.  Works with meshnet to create
layer 2 topology used by containers to layout networks in a k8s
environment.

Usage:
  kne [command]

Available Commands:
  completion  Generate the autocompletion script for the specified shell
  create      Create Topology
  delete      Delete Topology
  deploy      Deploy cluster.
  help        Help about any command
  show        Show Topology
  teardown    Teardown cluster deployment.
  topology    Topology commands.
```

### Deploy a KNE Cluster

The first step when using KNE is to deploy a Kubernetes cluster. This can be done using the `kne deploy` command.

```bash
$ kne deploy deploy/kne/kind-bridge.yaml
I0127 12:45:58.372269  599289 deploy.go:195] Deploying cluster...
I0127 12:45:58.376165  599289 deploy.go:619] kind version valid: got 0.26.0 want 0.24.0
I0127 12:45:58.376208  599289 deploy.go:630] Attempting to recycle existing cluster "kne"...
I0127 12:45:58.422484  599289 run.go:26] (kubectl): Kubernetes control plane is running at https://127.0.0.1:33595
I0127 12:45:58.422509  599289 run.go:26] (kubectl): CoreDNS is running at https://127.0.0.1:33595/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
I0127 12:45:58.422513  599289 run.go:26] (kubectl): To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
I0127 12:45:58.424016  599289 deploy.go:632] Recycling existing cluster "kne"
I0127 12:45:58.424060  599289 deploy.go:690] Found manifest "/home/nleiva2/kne/manifests/kind/bridge.yaml"
I0127 12:45:58.542765  599289 run.go:26] (kubectl): clusterrole.rbac.authorization.k8s.io/kindnet unchanged
I0127 12:45:58.555136  599289 run.go:26] (kubectl): clusterrolebinding.rbac.authorization.k8s.io/kindnet unchanged
I0127 12:45:58.613993  599289 run.go:26] (kubectl): serviceaccount/kindnet unchanged
I0127 12:45:58.646119  599289 run.go:26] (kubectl): daemonset.apps/kindnet configured
I0127 12:45:58.648321  599289 deploy.go:716] Waiting for potential manifest-issued deployments to complete
W0127 12:45:58.694451  599289 run.go:29] (kubectl): No resources found in default namespace.
I0127 12:45:58.696462  599289 deploy.go:199] Cluster deployed
I0127 12:45:58.738851  599289 run.go:26] (kubectl): Kubernetes control plane is running at https://127.0.0.1:33595
I0127 12:45:58.738881  599289 run.go:26] (kubectl): CoreDNS is running at https://127.0.0.1:33595/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
I0127 12:45:58.738893  599289 run.go:26] (kubectl): To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
I0127 12:45:58.740565  599289 deploy.go:203] Cluster healthy
I0127 12:45:58.741678  599289 deploy.go:214] Validating kubectl version
I0127 12:45:58.787409  599289 deploy.go:246] Deploying ingress...
I0127 12:45:58.787506  599289 deploy.go:885] Creating metallb namespace
I0127 12:45:58.787516  599289 deploy.go:904] Deploying MetalLB from: /home/nleiva2/kne/manifests/metallb/manifest.yaml
...
I0127 12:46:11.521051  599289 run.go:26] (kubectl): namespace/lemming-operator created
I0127 12:46:11.534582  599289 run.go:26] (kubectl): customresourcedefinition.apiextensions.k8s.io/lemmings.lemming.openconfig.net created
I0127 12:46:11.542651  599289 run.go:26] (kubectl): serviceaccount/lemming-controller-manager created
I0127 12:46:11.549103  599289 run.go:26] (kubectl): role.rbac.authorization.k8s.io/lemming-leader-election-role created
I0127 12:46:11.553738  599289 run.go:26] (kubectl): clusterrole.rbac.authorization.k8s.io/lemming-manager-role created
I0127 12:46:11.557467  599289 run.go:26] (kubectl): clusterrole.rbac.authorization.k8s.io/lemming-metrics-reader created
I0127 12:46:11.561325  599289 run.go:26] (kubectl): clusterrole.rbac.authorization.k8s.io/lemming-proxy-role created
I0127 12:46:11.566704  599289 run.go:26] (kubectl): rolebinding.rbac.authorization.k8s.io/lemming-leader-election-rolebinding created
I0127 12:46:11.571661  599289 run.go:26] (kubectl): clusterrolebinding.rbac.authorization.k8s.io/lemming-manager-rolebinding created
I0127 12:46:11.575505  599289 run.go:26] (kubectl): clusterrolebinding.rbac.authorization.k8s.io/lemming-proxy-rolebinding created
I0127 12:46:11.581728  599289 run.go:26] (kubectl): configmap/lemming-manager-config created
I0127 12:46:11.595391  599289 run.go:26] (kubectl): service/lemming-controller-manager-metrics-service created
I0127 12:46:11.604580  599289 run.go:26] (kubectl): deployment.apps/lemming-controller-manager created
I0127 12:46:11.606718  599289 deploy.go:1161] Lemming controller deployed
I0127 12:46:11.606767  599289 deploy.go:1348] Waiting on deployment "lemming-operator" to be healthy
I0127 12:46:21.955674  599289 deploy.go:1375] Deployment "lemming-operator" healthy
I0127 12:46:21.955706  599289 deploy.go:279] Controllers deployed and healthy
I0127 12:46:21.955995  599289 deploy.go:119] Deployment complete, ready for topology
Log files can be found in:
    /tmp/kne.dev-10-34-21-181.nleiva2.log.INFO.20250127-124558.599289
    /tmp/kne.dev-10-34-21-181.nleiva2.log.WARNING.20250127-124558.599289
```

### Loading Docker images in KNE

Run these steps and re-run the previous command `kne deploy example/kind-bridge.yaml`

Pull the image requirements (from manifests):

```bash
docker pull us-west1-docker.pkg.dev/kne-external/kne/networkop/init-wait:ga
docker pull ghcr.io/srl-labs/init-wait:latest
docker pull ghcr.io/aojea/kindnetd:v1.7.0
docker pull quay.io/metallb/controller:main
docker pull quay.io/metallb/speaker:main
docker pull us-west1-docker.pkg.dev/kne-external/kne/networkop/meshnet:v0.3.2
docker pull gcr.io/kubebuilder/kube-rbac-proxy:v0.13.1
docker pull gcr.io/kubebuilder/kube-rbac-proxy:v0.11.0
docker pull gcr.io/kubebuilder/kube-rbac-proxy:v0.12.0
docker pull gcr.io/kubebuilder/kube-rbac-proxy:v0.8.0
docker pull ghcr.io/srl-labs/srl-controller:v0.6.1
docker pull ghcr.io/aristanetworks/arista-ceoslab-operator:v2.1.2
docker pull networkop/init-wait:latest
docker pull us-west1-docker.pkg.dev/openconfig-lemming/release/operator:v0.2.3
docker pull nleiva.com/ncptx:23.2R2.11
docker pull nleiva.com/nceos:4.28
docker pull nleiva.com/nxrd-control-plane:7.8.1
docker pull ghcr.io/nokia/srlinux
docker pull ghcr.io/open-traffic-generator/keng-operator:0.3.34
```


Tag the router images with the desired in-cluster name:

```bash
docker tag nleiva.com/ncptx:23.2R2.11 cptx:latest
docker tag nleiva.com/nceos:4.28 ceos:latest
docker tag nleiva.com/nxrd-control-plane:7.8.1 xrd:latest
# docker tag ghcr.io/nokia/srlinux ghcr.io/nokia/srlinux:latest
```

Load the image into the KIND cluster:

```bash
kind load docker-image ghcr.io/srl-labs/init-wait:latest --name=kne
kind load docker-image us-west1-docker.pkg.dev/kne-external/kne/networkop/init-wait:ga --name=kne
kind load docker-image ghcr.io/aojea/kindnetd:v1.7.0 --name=kne
kind load docker-image quay.io/metallb/controller:main --name=kne
kind load docker-image quay.io/metallb/speaker:main --name=kne
kind load docker-image us-west1-docker.pkg.dev/kne-external/kne/networkop/meshnet:v0.3.2 --name=kne
kind load docker-image gcr.io/kubebuilder/kube-rbac-proxy:v0.13.1 --name=kne
kind load docker-image gcr.io/kubebuilder/kube-rbac-proxy:v0.11.0 --name=kne
kind load docker-image gcr.io/kubebuilder/kube-rbac-proxy:v0.12.0 --name=kne
kind load docker-image gcr.io/kubebuilder/kube-rbac-proxy:v0.8.0 --name=kne
kind load docker-image ghcr.io/srl-labs/srl-controller:v0.6.1 --name=kne
kind load docker-image ghcr.io/aristanetworks/arista-ceoslab-operator:v2.1.2 --name=kne
kind load docker-image networkop/init-wait:latest --name=kne
kind load docker-image us-west1-docker.pkg.dev/openconfig-lemming/release/operator:v0.2.3 --name=kne
kind load docker-image ghcr.io/open-traffic-generator/keng-operator:0.3.34 --name=kne
kind load docker-image cptx:latest --name=kne
kind load docker-image ceos:latest --name=kne
kind load docker-image xrd:latest --name=kne
kind load docker-image ghcr.io/nokia/srlinux:latest --name=kne
```

You can check a full list of images loaded in your kind cluster using:

```bash
$ docker exec -it kne-control-plane crictl images
IMAGE                                                         TAG                  IMAGE ID            SIZE
docker.io/kindest/kindnetd                                    v20241108-5c6d2daf   50415e5d05f05       38.6MB
docker.io/kindest/local-path-helper                           v20230510-486859a6   be300acfc8622       3.05MB
docker.io/kindest/local-path-provisioner                      v20241108-5c6d2daf   27444efca312d       19.4MB
gcr.io/kubebuilder/kube-rbac-proxy                            v0.11.0              29589495df8d9       47.8MB
gcr.io/kubebuilder/kube-rbac-proxy                            v0.12.0              3b63df468a30f       62.6MB
gcr.io/kubebuilder/kube-rbac-proxy                            v0.13.1              eb5a02daef2fe       56.5MB
gcr.io/kubebuilder/kube-rbac-proxy                            v0.8.0               ad393d6a4d1b1       50.2MB
ghcr.io/aojea/kindnetd                                        v1.7.0               e5d39c396517a       115MB
ghcr.io/aristanetworks/arista-ceoslab-operator                v2.1.2               4c2fb319ebdff       51.2MB
ghcr.io/open-traffic-generator/keng-operator                  0.3.34               3c4f961cf2cb8       60.8MB
ghcr.io/srl-labs/srl-controller                               v0.6.1               f300b44269aad       55MB
quay.io/metallb/controller                                    main                 ebc4615715ca1       71.4MB
quay.io/metallb/speaker                                       main                 c1dd1c0a5c995       128MB
registry.k8s.io/coredns/coredns                               v1.11.1              cbb01a7bd410d       18.2MB
registry.k8s.io/etcd                                          3.5.15-0             2e96e5913fc06       56.9MB
registry.k8s.io/kube-apiserver-amd64                          v1.31.0              34127a45d246b       95.2MB
registry.k8s.io/kube-apiserver                                v1.31.0              34127a45d246b       95.2MB
registry.k8s.io/kube-controller-manager-amd64                 v1.31.0              7638c5918bd4c       89.4MB
registry.k8s.io/kube-controller-manager                       v1.31.0              7638c5918bd4c       89.4MB
registry.k8s.io/kube-proxy-amd64                              v1.31.0              aa32fdc5fd8ef       92.7MB
registry.k8s.io/kube-proxy                                    v1.31.0              aa32fdc5fd8ef       92.7MB
registry.k8s.io/kube-scheduler-amd64                          v1.31.0              7fca81e5b9fac       68.4MB
registry.k8s.io/kube-scheduler                                v1.31.0              7fca81e5b9fac       68.4MB
registry.k8s.io/pause                                         3.10                 873ed75102791       320kB
us-west1-docker.pkg.dev/kne-external/kne/networkop/meshnet    v0.3.2               83a71c50c4e4a       208MB
us-west1-docker.pkg.dev/openconfig-lemming/release/operator   v0.2.3               5a62790a5a87e       59.1MB
```

### Check KNE Cluster deployment 

```bash
$ kubectl get pods -A
NAMESPACE                        NAME                                                          READY   STATUS    RESTARTS   AGE
arista-ceoslab-operator-system   arista-ceoslab-operator-controller-manager-5f69445454-zlcgc   2/2     Running   0          155m
ixiatg-op-system                 ixiatg-op-controller-manager-849979569b-sc2z7                 2/2     Running   0          169m
kube-system                      coredns-6f6b679f8f-5kqwp                                      1/1     Running   0          3h7m
kube-system                      coredns-6f6b679f8f-q4phq                                      1/1     Running   0          3h7m
kube-system                      etcd-kne-control-plane                                        1/1     Running   0          3h7m
kube-system                      kindnet-9rkzd                                                 1/1     Running   0          3h7m
kube-system                      kube-apiserver-kne-control-plane                              1/1     Running   0          3h7m
kube-system                      kube-controller-manager-kne-control-plane                     1/1     Running   0          3h7m
kube-system                      kube-proxy-4n57p                                              1/1     Running   0          3h7m
kube-system                      kube-scheduler-kne-control-plane                              1/1     Running   0          3h7m
lemming-operator                 lemming-controller-manager-5d8fd8857c-n9kbx                   2/2     Running   0          155m
local-path-storage               local-path-provisioner-ccc7bf7fc-ktl5s                        1/1     Running   0          3h7m
meshnet                          meshnet-6jfpd                                                 1/1     Running   0          169m
metallb-system                   controller-8b7b6bf6b-86l5w                                    1/1     Running   0          3h7m
metallb-system                   speaker-rj48s                                                 1/1     Running   0          175m
srlinux-controller               srlinux-controller-controller-manager-f668d55cc-hznlg         2/2     Running   0          164m
```

## Launch Topology

```bash
kne create example/multivendor.pb.txt
```

```bash
$  kubectl get pods -n multivendor
NAME   READY   STATUS    RESTARTS   AGE
cptx   1/1     Running   0          6m32s
srl    1/1     Running   0          6m32s
```

```bash
$ kubectl get services -n multivendor
NAME           TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                                     AGE
service-cptx   LoadBalancer   10.96.134.55    172.18.0.50   443/TCP,22/TCP,9339/TCP,9340/TCP,9559/TCP   5m29s
service-srl    LoadBalancer   10.96.105.223   172.18.0.51   9340/TCP,9559/TCP,443/TCP,22/TCP,9339/TCP   5m29s
```

### Access routers

#### Juniper

```bash
$ kubectl exec -it cptx -n multivendor -- cli
Defaulted container "cptx" out of: cptx, init-cptx (init)

root@re0> show version 
Hostname: re0
Model: ptx10001-36mr
Junos: 23.2R2.11-EVO
Yocto: 3.0.2
Linux Kernel: 5.2.60-yocto-standard-g3c005ea
JUNOS-EVO OS 64-bit [junos-evo-install-ptx-fixed-x86-64-23.2R2.11-EVO]
```

#### Nokia

```bash
$ kubectl exec -it srl -n multivendor -- sr_cli
Defaulted container "srl" out of: srl, init-srl (init)
Using configuration file(s): ['/etc/opt/srlinux/srlinux.rc']
Welcome to the srlinux CLI.
Type 'help' (and press <ENTER>) if you need any help using this.
--{ running }--[  ]--

A:srl# show version
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Hostname             : srl
Chassis Type         : 7220 IXR-D2
Part Number          : Sim Part No.
Serial Number        : Sim Serial No.
System HW MAC Address: 02:70:57:FF:00:00
OS                   : SR Linux
Software Version     : v24.10.1
Build Number         : 492-gf8858c5836
Architecture         : x86_64
Last Booted          : 2025-01-27T21:26:55.304Z
Total Memory         : 31886089 kB
Free Memory          : 18267816 kB
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
--{ running }--[  ]--
```

(*) Need to add service definitions to enable SSH, etc.: https://github.com/openconfig/kne/blob/main/examples/nokia/srlinux-cert/2node-srl-with-cert.pbtxt

### Troubleshoot


```bash
$ kubectl describe pods cptx -n multivendor             
...
Events:
  Type     Reason     Age                   From               Message
  ----     ------     ----                  ----               -------
  Normal   Scheduled  4m2s                  default-scheduler  Successfully assigned multivendor/cptx to kne-control-plane
  Normal   Pulling    2m28s (x4 over 4m1s)  kubelet            Pulling image "us-west1-docker.pkg.dev/kne-external/kne/networkop/init-wait:ga"
  Warning  Failed     2m28s (x4 over 4m1s)  kubelet            Failed to pull image "us-west1-docker.pkg.dev/kne-external/kne/networkop/init-wait:ga": failed to pull and unpack image "us-west1-docker.pkg.dev/kne-external/kne/networkop/init-wait:ga": failed to resolve reference "us-west1-docker.pkg.dev/kne-external/kne/networkop/init-wait:ga": failed to do request: Head "https://us-west1-docker.pkg.dev/v2/kne-external/kne/networkop/init-wait/manifests/ga": tls: failed to verify certificate: x509: certificate signed by unknown authority
  Warning  Failed     2m28s (x4 over 4m1s)  kubelet            Error: ErrImagePull
  Warning  Failed     2m14s (x6 over 4m)    kubelet            Error: ImagePullBackOff
  Normal   BackOff    2m2s (x7 over 4m)     kubelet            Back-off pulling image "us-west1-docker.pkg.dev/kne-external/kne/networkop/init-wait:ga"

```

Delete: `kne delete multivendor.pb.txt`