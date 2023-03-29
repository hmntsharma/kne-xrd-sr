# Kubernetes Network Emulation with Cisco XRd


## Introduction

This repository is an extension of [ansible-cisco-xrd-sr](https://github.com/hmntsharma/ansible-cisco-xrd-sr) deployed using KNE instead of docker-compose.

## Pre-Requisites

* Setup Cisco XRd as described [here](https://hmntsharma.github.io/cisco-xrd/base_setup/#observations)
* Setup KNE as described [here](https://github.com/openconfig/kne/blob/main/docs/setup.md)


## Deploy KNE Cluster
* Deploy cluster as described [here](https://github.com/openconfig/kne/blob/main/docs/create_topology.md)
  * While our topology does not require any controllers, we kept only the srl controller for completeness. Removed all other controller config from [kind-bridge.yaml](https://github.com/openconfig/kne/blob/main/deploy/kne/kind-bridge.yaml)

  <details>
  <summary><h4>Deployment logs</h4></summary>
    
  ```ruby
    $ kne deploy kne/deploy/kne/kind-bridge.yaml
    I0329 11:54:14.949825  277957 deploy.go:141] Deploying cluster...
    I0329 11:54:14.957473  277957 deploy.go:404] kind version valid: got v0.17.0 want v0.17.0
    I0329 11:54:14.957516  277957 deploy.go:411] Attempting to recycle existing cluster "kne"...
    W0329 11:54:15.052204  277957 deploy.go:52] (kubectl): error: context "kind-kne" does not exist
    I0329 11:54:15.055630  277957 deploy.go:436] Creating kind cluster with: [create cluster --name kne --image kindest/node:v1.26.0 --config /home/lab/github/kne/kind/kind-no-cni.yaml]
    W0329 11:54:15.532908  277957 deploy.go:52] (kind): Creating cluster "kne" ...
    W0329 11:54:15.532973  277957 deploy.go:52] (kind):  • Ensuring node image (kindest/node:v1.26.0) 🖼  ...
    W0329 11:54:15.776373  277957 deploy.go:52] (kind):  ✓ Ensuring node image (kindest/node:v1.26.0) 🖼
    W0329 11:54:15.776458  277957 deploy.go:52] (kind):  • Preparing nodes 📦   ...
    W0329 11:54:19.868960  277957 deploy.go:52] (kind):  ✓ Preparing nodes 📦
    W0329 11:54:20.094567  277957 deploy.go:52] (kind):  • Writing configuration 📜  ...
    W0329 11:54:21.308209  277957 deploy.go:52] (kind):  ✓ Writing configuration 📜
    W0329 11:54:21.308266  277957 deploy.go:52] (kind):  • Starting control-plane 🕹️  ...
    W0329 11:54:41.490830  277957 deploy.go:52] (kind):  ✓ Starting control-plane 🕹️
    W0329 11:54:41.490869  277957 deploy.go:52] (kind):  • Installing StorageClass 💾  ...
    W0329 11:54:42.839695  277957 deploy.go:52] (kind):  ✓ Installing StorageClass 💾
    W0329 11:54:44.094009  277957 deploy.go:52] (kind): Set kubectl context to "kind-kne"
    W0329 11:54:44.094080  277957 deploy.go:52] (kind): You can now use your cluster with:
    W0329 11:54:44.094106  277957 deploy.go:52] (kind): kubectl cluster-info --context kind-kne
    W0329 11:54:44.094135  277957 deploy.go:52] (kind): Not sure what to do next? 😅  Check out https://kind.sigs.k8s.io/docs/user/quick-start/
    I0329 11:54:44.096893  277957 deploy.go:440] Deployed kind cluster: kne
    I0329 11:54:44.096970  277957 deploy.go:454] Found manifest "/home/lab/github/kne/manifests/kind/kind-bridge.yaml"
    I0329 11:54:44.386376  277957 deploy.go:49] (kubectl): clusterrole.rbac.authorization.k8s.io/kindnet created
    I0329 11:54:44.393076  277957 deploy.go:49] (kubectl): clusterrolebinding.rbac.authorization.k8s.io/kindnet created
    I0329 11:54:44.403606  277957 deploy.go:49] (kubectl): serviceaccount/kindnet created
    I0329 11:54:44.420486  277957 deploy.go:49] (kubectl): daemonset.apps/kindnet created
    I0329 11:54:44.427546  277957 deploy.go:145] Cluster deployed
    I0329 11:54:44.521749  277957 deploy.go:49] (kubectl): Kubernetes control plane is running at https://127.0.0.1:37931
    I0329 11:54:44.521820  277957 deploy.go:49] (kubectl): CoreDNS is running at https://127.0.0.1:37931/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
    I0329 11:54:44.521845  277957 deploy.go:49] (kubectl): To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
    I0329 11:54:44.526543  277957 deploy.go:149] Cluster healthy
    I0329 11:54:44.529456  277957 deploy.go:160] Checking kubectl versions.
    I0329 11:54:44.624072  277957 deploy.go:192] Deploying ingress...
    I0329 11:54:44.624385  277957 deploy.go:696] Creating metallb namespace
    I0329 11:54:44.624488  277957 deploy.go:715] Deploying MetalLB from: /home/lab/github/kne/manifests/metallb/manifest.yaml
    I0329 11:54:44.955154  277957 deploy.go:49] (kubectl): namespace/metallb-system created
    I0329 11:54:44.975991  277957 deploy.go:49] (kubectl): customresourcedefinition.apiextensions.k8s.io/addresspools.metallb.io created
    I0329 11:54:44.987728  277957 deploy.go:49] (kubectl): customresourcedefinition.apiextensions.k8s.io/bfdprofiles.metallb.io created
    I0329 11:54:44.999590  277957 deploy.go:49] (kubectl): customresourcedefinition.apiextensions.k8s.io/bgpadvertisements.metallb.io created
    I0329 11:54:45.013310  277957 deploy.go:49] (kubectl): customresourcedefinition.apiextensions.k8s.io/bgppeers.metallb.io created
    I0329 11:54:45.023183  277957 deploy.go:49] (kubectl): customresourcedefinition.apiextensions.k8s.io/communities.metallb.io created
    I0329 11:54:45.034133  277957 deploy.go:49] (kubectl): customresourcedefinition.apiextensions.k8s.io/ipaddresspools.metallb.io created
    I0329 11:54:45.046096  277957 deploy.go:49] (kubectl): customresourcedefinition.apiextensions.k8s.io/l2advertisements.metallb.io created
    I0329 11:54:45.054886  277957 deploy.go:49] (kubectl): serviceaccount/controller created
    I0329 11:54:45.067451  277957 deploy.go:49] (kubectl): serviceaccount/speaker created
    I0329 11:54:45.076849  277957 deploy.go:49] (kubectl): role.rbac.authorization.k8s.io/controller created
    I0329 11:54:45.084043  277957 deploy.go:49] (kubectl): role.rbac.authorization.k8s.io/pod-lister created
    I0329 11:54:45.090552  277957 deploy.go:49] (kubectl): clusterrole.rbac.authorization.k8s.io/metallb-system:controller created
    I0329 11:54:45.096937  277957 deploy.go:49] (kubectl): clusterrole.rbac.authorization.k8s.io/metallb-system:speaker created
    I0329 11:54:45.105609  277957 deploy.go:49] (kubectl): rolebinding.rbac.authorization.k8s.io/controller created
    I0329 11:54:45.117455  277957 deploy.go:49] (kubectl): rolebinding.rbac.authorization.k8s.io/pod-lister created
    I0329 11:54:45.126849  277957 deploy.go:49] (kubectl): clusterrolebinding.rbac.authorization.k8s.io/metallb-system:controller created
    I0329 11:54:45.136013  277957 deploy.go:49] (kubectl): clusterrolebinding.rbac.authorization.k8s.io/metallb-system:speaker created
    I0329 11:54:45.147538  277957 deploy.go:49] (kubectl): secret/webhook-server-cert created
    I0329 11:54:45.162025  277957 deploy.go:49] (kubectl): service/webhook-service created
    I0329 11:54:45.173735  277957 deploy.go:49] (kubectl): deployment.apps/controller created
    I0329 11:54:45.190656  277957 deploy.go:49] (kubectl): daemonset.apps/speaker created
    I0329 11:54:45.203999  277957 deploy.go:49] (kubectl): validatingwebhookconfiguration.admissionregistration.k8s.io/metallb-webhook-configuration created
    I0329 11:54:45.226091  277957 deploy.go:720] Creating metallb secret
    I0329 11:54:45.231744  277957 deploy.go:1115] Waiting on deployment "metallb-system" to be healthy
    I0329 11:55:36.271322  277957 deploy.go:1142] Deployment "metallb-system" healthy
    I0329 11:55:36.274841  277957 deploy.go:744] Applying metallb ingress config
    W0329 11:55:36.282047  277957 deploy.go:782] Failed to create address polling (will retry 5 times)
    I0329 11:55:41.337187  277957 deploy.go:1115] Waiting on deployment "metallb-system" to be healthy
    I0329 11:55:41.340136  277957 deploy.go:1142] Deployment "metallb-system" healthy
    I0329 11:55:41.340170  277957 deploy.go:201] Ingress healthy
    I0329 11:55:41.340191  277957 deploy.go:202] Deploying CNI...
    I0329 11:55:41.340218  277957 deploy.go:845] Deploying Meshnet from: /home/lab/github/kne/manifests/meshnet/grpc/manifest.yaml
    I0329 11:55:42.220733  277957 deploy.go:49] (kubectl): namespace/meshnet created
    I0329 11:55:42.230037  277957 deploy.go:49] (kubectl): customresourcedefinition.apiextensions.k8s.io/topologies.networkop.co.uk created
    I0329 11:55:42.240547  277957 deploy.go:49] (kubectl): serviceaccount/meshnet created
    I0329 11:55:42.246403  277957 deploy.go:49] (kubectl): clusterrole.rbac.authorization.k8s.io/meshnet-clusterrole created
    I0329 11:55:42.254308  277957 deploy.go:49] (kubectl): clusterrolebinding.rbac.authorization.k8s.io/meshnet-clusterrolebinding created
    I0329 11:55:42.265365  277957 deploy.go:49] (kubectl): daemonset.apps/meshnet created
    I0329 11:55:42.269106  277957 deploy.go:849] Meshnet Deployed
    I0329 11:55:42.269148  277957 deploy.go:854] Waiting on Meshnet to be Healthy
    I0329 11:55:42.271818  277957 deploy.go:875] Meshnet Healthy
    I0329 11:55:42.271845  277957 deploy.go:212] CNI healthy
    I0329 11:55:42.271865  277957 deploy.go:214] Deploying controller...
    I0329 11:55:42.271879  277957 deploy.go:1017] Deploying SRLinux controller from: /home/lab/github/kne/manifests/controllers/srlinux/manifest.yaml
    I0329 11:55:42.591850  277957 deploy.go:49] (kubectl): namespace/srlinux-controller created
    I0329 11:55:42.603833  277957 deploy.go:49] (kubectl): customresourcedefinition.apiextensions.k8s.io/srlinuxes.kne.srlinux.dev created
    I0329 11:55:42.612469  277957 deploy.go:49] (kubectl): serviceaccount/srlinux-controller-controller-manager created
    I0329 11:55:42.622472  277957 deploy.go:49] (kubectl): role.rbac.authorization.k8s.io/srlinux-controller-leader-election-role created
    I0329 11:55:42.637936  277957 deploy.go:49] (kubectl): clusterrole.rbac.authorization.k8s.io/srlinux-controller-manager-role created
    I0329 11:55:42.643499  277957 deploy.go:49] (kubectl): clusterrole.rbac.authorization.k8s.io/srlinux-controller-metrics-reader created
    I0329 11:55:42.649319  277957 deploy.go:49] (kubectl): clusterrole.rbac.authorization.k8s.io/srlinux-controller-proxy-role created
    I0329 11:55:42.656643  277957 deploy.go:49] (kubectl): rolebinding.rbac.authorization.k8s.io/srlinux-controller-leader-election-rolebinding created
    I0329 11:55:42.663385  277957 deploy.go:49] (kubectl): clusterrolebinding.rbac.authorization.k8s.io/srlinux-controller-manager-rolebinding created
    I0329 11:55:42.671595  277957 deploy.go:49] (kubectl): clusterrolebinding.rbac.authorization.k8s.io/srlinux-controller-proxy-rolebinding created
    I0329 11:55:42.690661  277957 deploy.go:49] (kubectl): service/srlinux-controller-controller-manager-metrics-service created
    I0329 11:55:42.702556  277957 deploy.go:49] (kubectl): deployment.apps/srlinux-controller-controller-manager created
    I0329 11:55:42.709911  277957 deploy.go:1021] SRLinux controller deployed
    I0329 11:55:42.709963  277957 deploy.go:1115] Waiting on deployment "srlinux-controller" to be healthy
    I0329 11:56:13.081781  277957 deploy.go:1142] Deployment "srlinux-controller" healthy
    I0329 11:56:13.081823  277957 deploy.go:225] Controllers deployed and healthy
    I0329 11:56:13.081866  277957 deploy.go:109] Deployment complete, ready for topology
    Log files can be found in:
        /tmp/kne.xrdlab.lab.log.INFO.20230329-115414.277957
        /tmp/kne.xrdlab.lab.log.WARNING.20230329-115415.277957
  ```
  </details>


#### Verify
```ruby
$ docker ps -a
CONTAINER ID   IMAGE                  COMMAND                  CREATED       STATUS       PORTS                       NAMES
9334f6a851e9   kindest/node:v1.26.0   "/usr/local/bin/entr…"   3 hours ago   Up 3 hours   127.0.0.1:37931->6443/tcp   kne-control-plane
```

## Load the docker image in to a ```kind``` cluster

```ruby
$ docker images | grep xr
localhost/ios-xr                          7.7.1     dd8d741e50b2   8 months ago   1.15GB
```

```ruby
$ kind load docker-image localhost/ios-xr:7.7.1 --name=kne
Image: "" with ID "sha256:dd8d741e50b2fd91680f2fac666056cccb766ab9447eb4dfff14d74ac2df295d" not yet present on node "kne-control-plane", loading...
```

#### Verify
```ruby
$ docker exec -it kne-control-plane crictl images | grep xr
localhost/ios-xr                                               7.7.1                dd8d741e50b2f       1.18GB
```

## Deploy Topology
* There are multiple examples of topology file configuration in the  kne repository. This [kne-xrd-sr.pb.txt](https://github.com/hmntsharma/kne-xrd-sr/blob/main/kne-xrd-sr.pb.txt) file depicts the original topology mentioned in the introduction earlier.

    <details>
    <summary><h4>Deployment logs</h4></summary>

    ```ruby
      $ kne create kne-xrd-sr.pb.txt
      I0329 11:59:01.113437  283745 root.go:119] /home/lab/github/kne-xrd-sr
      I0329 11:59:01.114935  283745 topo.go:118] Trying in-cluster configuration
      I0329 11:59:01.114976  283745 topo.go:121] Falling back to kubeconfig: "/home/lab/.kube/config"
      I0329 11:59:01.117200  283745 topo.go:254] Adding Link: xrd-1:eth3 xrd-2:eth3
      I0329 11:59:01.117221  283745 topo.go:254] Adding Link: xrd-2:eth1 xrd-3:eth1
      I0329 11:59:01.117294  283745 topo.go:254] Adding Link: xrd-2:eth2 xrd-6:eth2
      I0329 11:59:01.117307  283745 topo.go:254] Adding Link: xrd-2:eth4 xrd-7:eth4
      I0329 11:59:01.117314  283745 topo.go:254] Adding Link: xrd-2:eth6 xrd-9:eth6
      I0329 11:59:01.117321  283745 topo.go:254] Adding Link: xrd-3:eth2 xrd-7:eth2
      I0329 11:59:01.117391  283745 topo.go:254] Adding Link: xrd-3:eth3 xrd-4:eth3
      I0329 11:59:01.117443  283745 topo.go:254] Adding Link: xrd-3:eth4 xrd-9:eth4
      I0329 11:59:01.117450  283745 topo.go:254] Adding Link: xrd-4:eth1 xrd-5:eth1
      I0329 11:59:01.117499  283745 topo.go:254] Adding Link: xrd-4:eth2 xrd-8:eth2
      I0329 11:59:01.117547  283745 topo.go:254] Adding Link: xrd-4:eth5 xrd-7:eth5
      I0329 11:59:01.117554  283745 topo.go:254] Adding Link: xrd-6:eth1 xrd-7:eth1
      I0329 11:59:01.117561  283745 topo.go:254] Adding Link: xrd-7:eth3 xrd-8:eth3
      I0329 11:59:01.117606  283745 topo.go:292] Adding Node: xrd-2:CISCO
      I0329 11:59:01.117888  283745 topo.go:292] Adding Node: xrd-4:CISCO
      I0329 11:59:01.118070  283745 topo.go:292] Adding Node: xrd-5:CISCO
      I0329 11:59:01.118703  283745 topo.go:292] Adding Node: xrd-6:CISCO
      I0329 11:59:01.118865  283745 topo.go:292] Adding Node: xrd-7:CISCO
      I0329 11:59:01.120540  283745 topo.go:292] Adding Node: ansible:HOST
      I0329 11:59:01.120638  283745 topo.go:292] Adding Node: xrd-1:CISCO
      I0329 11:59:01.120809  283745 topo.go:292] Adding Node: xrd-3:CISCO
      I0329 11:59:01.121166  283745 topo.go:292] Adding Node: xrd-8:CISCO
      I0329 11:59:01.121363  283745 topo.go:292] Adding Node: xrd-9:CISCO
      I0329 11:59:01.139570  283745 topo.go:359] Creating namespace for topology: "kne-xrd-sr"
      I0329 11:59:01.167280  283745 topo.go:369] Server Namespace: &Namespace{ObjectMeta:{kne-xrd-sr    af5afd55-833e-4b5e-9802-dadbd544b31c 1040 0 2023-03-29 11:59:01 +0000 UTC <nil> <nil> map[kubernetes.io/metadata.name:kne-xrd-sr] map[] [] [] [{kne Update v1 2023-03-29 11:59:01 +0000 UTC FieldsV1 {"f:metadata":{"f:labels":{".":{},"f:kubernetes.io/metadata.name":{}}}} }]},Spec:NamespaceSpec{Finalizers:[kubernetes],},Status:NamespaceStatus{Phase:Active,Conditions:[]NamespaceCondition{},},}
      I0329 11:59:01.167511  283745 topo.go:396] Getting topology specs for namespace kne-xrd-sr
      I0329 11:59:01.167546  283745 topo.go:325] Getting topology specs for node xrd-4
      I0329 11:59:01.167565  283745 topo.go:325] Getting topology specs for node xrd-5
      I0329 11:59:01.167591  283745 topo.go:325] Getting topology specs for node xrd-1
      I0329 11:59:01.167615  283745 topo.go:325] Getting topology specs for node xrd-3
      I0329 11:59:01.167636  283745 topo.go:325] Getting topology specs for node xrd-2
      I0329 11:59:01.167651  283745 topo.go:325] Getting topology specs for node xrd-6
      I0329 11:59:01.167680  283745 topo.go:325] Getting topology specs for node xrd-7
      I0329 11:59:01.167718  283745 topo.go:325] Getting topology specs for node ansible
      I0329 11:59:01.167741  283745 topo.go:325] Getting topology specs for node xrd-8
      I0329 11:59:01.167760  283745 topo.go:325] Getting topology specs for node xrd-9
      I0329 11:59:01.167785  283745 topo.go:403] Creating topology for meshnet node xrd-4
      I0329 11:59:01.348621  283745 topo.go:403] Creating topology for meshnet node xrd-2
      I0329 11:59:01.356477  283745 topo.go:403] Creating topology for meshnet node xrd-7
      I0329 11:59:01.361110  283745 topo.go:403] Creating topology for meshnet node xrd-8
      I0329 11:59:01.367605  283745 topo.go:403] Creating topology for meshnet node xrd-5
      I0329 11:59:01.371915  283745 topo.go:403] Creating topology for meshnet node xrd-1
      I0329 11:59:01.376515  283745 topo.go:403] Creating topology for meshnet node xrd-3
      I0329 11:59:01.384787  283745 topo.go:403] Creating topology for meshnet node xrd-6
      I0329 11:59:01.391599  283745 topo.go:403] Creating topology for meshnet node ansible
      I0329 11:59:01.395874  283745 topo.go:403] Creating topology for meshnet node xrd-9
      I0329 11:59:01.401506  283745 topo.go:376] Creating Node Pods
      I0329 11:59:01.401581  283745 cisco.go:60] Creating Cisco xrd node resource xrd-4
      I0329 11:59:01.406304  283745 cisco.go:65] Created Cisco xrd node xrd-4 configmap
      I0329 11:59:01.424470  283745 cisco.go:170] Created Cisco xrd node resource xrd-4 pod
      I0329 11:59:01.445361  283745 cisco.go:174] Created Cisco xrd node resource xrd-4 services
      I0329 11:59:01.445399  283745 topo.go:381] Node "xrd-4" resource created
      I0329 11:59:01.445409  283745 cisco.go:60] Creating Cisco xrd node resource xrd-5
      I0329 11:59:01.449246  283745 cisco.go:65] Created Cisco xrd node xrd-5 configmap
      I0329 11:59:01.455471  283745 cisco.go:170] Created Cisco xrd node resource xrd-5 pod
      I0329 11:59:01.468099  283745 cisco.go:174] Created Cisco xrd node resource xrd-5 services
      I0329 11:59:01.468133  283745 topo.go:381] Node "xrd-5" resource created
      I0329 11:59:01.468147  283745 cisco.go:60] Creating Cisco xrd node resource xrd-1
      I0329 11:59:01.471749  283745 cisco.go:65] Created Cisco xrd node xrd-1 configmap
      I0329 11:59:01.479030  283745 cisco.go:170] Created Cisco xrd node resource xrd-1 pod
      I0329 11:59:01.489721  283745 cisco.go:174] Created Cisco xrd node resource xrd-1 services
      I0329 11:59:01.489748  283745 topo.go:381] Node "xrd-1" resource created
      I0329 11:59:01.489761  283745 cisco.go:60] Creating Cisco xrd node resource xrd-3
      I0329 11:59:01.530531  283745 cisco.go:65] Created Cisco xrd node xrd-3 configmap
      I0329 11:59:01.733902  283745 cisco.go:170] Created Cisco xrd node resource xrd-3 pod
      I0329 11:59:01.943032  283745 cisco.go:174] Created Cisco xrd node resource xrd-3 services
      I0329 11:59:01.943077  283745 topo.go:381] Node "xrd-3" resource created
      I0329 11:59:01.943101  283745 cisco.go:60] Creating Cisco xrd node resource xrd-9
      I0329 11:59:02.129372  283745 cisco.go:65] Created Cisco xrd node xrd-9 configmap
      I0329 11:59:02.335536  283745 cisco.go:170] Created Cisco xrd node resource xrd-9 pod
      I0329 11:59:02.546398  283745 cisco.go:174] Created Cisco xrd node resource xrd-9 services
      I0329 11:59:02.546460  283745 topo.go:381] Node "xrd-9" resource created
      I0329 11:59:02.546496  283745 cisco.go:60] Creating Cisco xrd node resource xrd-2
      I0329 11:59:02.729986  283745 cisco.go:65] Created Cisco xrd node xrd-2 configmap
      I0329 11:59:02.949095  283745 cisco.go:170] Created Cisco xrd node resource xrd-2 pod
      I0329 11:59:03.144028  283745 cisco.go:174] Created Cisco xrd node resource xrd-2 services
      I0329 11:59:03.144099  283745 topo.go:381] Node "xrd-2" resource created
      I0329 11:59:03.144140  283745 cisco.go:60] Creating Cisco xrd node resource xrd-6
      I0329 11:59:03.330686  283745 cisco.go:65] Created Cisco xrd node xrd-6 configmap
      I0329 11:59:03.535052  283745 cisco.go:170] Created Cisco xrd node resource xrd-6 pod
      I0329 11:59:03.740565  283745 cisco.go:174] Created Cisco xrd node resource xrd-6 services
      I0329 11:59:03.740597  283745 topo.go:381] Node "xrd-6" resource created
      I0329 11:59:03.740608  283745 cisco.go:60] Creating Cisco xrd node resource xrd-7
      I0329 11:59:03.930473  283745 cisco.go:65] Created Cisco xrd node xrd-7 configmap
      I0329 11:59:04.134631  283745 cisco.go:170] Created Cisco xrd node resource xrd-7 pod
      I0329 11:59:04.348064  283745 cisco.go:174] Created Cisco xrd node resource xrd-7 services
      I0329 11:59:04.348096  283745 topo.go:381] Node "xrd-7" resource created
      I0329 11:59:04.348131  283745 node.go:250] Creating Pod:
       name:"ansible"  config:{command:"/bin/sh"  command:"-c"  command:"sleep 2000000000000"  image:"ghcr.io/hmntsharma/ansible-cisco-sr-lab:latest"  entry_command:"kubectl exec -it ansible -- sh"  config_path:"/etc"  config_file:"config"}  vendor:HOST  model:"linux"  os:"ubuntu"
      I0329 11:59:04.531268  283745 node.go:338] no services found
      I0329 11:59:04.531296  283745 topo.go:381] Node "ansible" resource created
      I0329 11:59:04.531312  283745 cisco.go:60] Creating Cisco xrd node resource xrd-8
      I0329 11:59:04.731122  283745 cisco.go:65] Created Cisco xrd node xrd-8 configmap
      I0329 11:59:04.935794  283745 cisco.go:170] Created Cisco xrd node resource xrd-8 pod
      I0329 11:59:05.140979  283745 cisco.go:174] Created Cisco xrd node resource xrd-8 services
      I0329 11:59:05.141027  283745 topo.go:381] Node "xrd-8" resource created
      I0329 11:59:12.730310  283745 topo.go:449] Node "xrd-5": Status RUNNING
      I0329 11:59:14.928807  283745 topo.go:449] Node "xrd-9": Status RUNNING
      I0329 11:59:17.578186  283745 topo.go:449] Node "xrd-8": Status RUNNING
      I0329 11:59:18.330353  283745 topo.go:449] Node "xrd-4": Status RUNNING
      I0329 11:59:18.533289  283745 topo.go:449] Node "xrd-1": Status RUNNING
      I0329 11:59:18.729497  283745 topo.go:449] Node "xrd-3": Status RUNNING
      I0329 11:59:19.268240  283745 topo.go:449] Node "xrd-6": Status RUNNING
      I0329 11:59:20.134138  283745 topo.go:449] Node "xrd-2": Status RUNNING
      I0329 11:59:20.328612  283745 topo.go:449] Node "xrd-7": Status RUNNING
      I0329 11:59:43.130984  283745 topo.go:449] Node "ansible": Status RUNNING
      I0329 11:59:43.231380  283745 topo.go:159] Topology "kne-xrd-sr" created
      Log files can be found in:
          /tmp/kne.xrdlab.lab.log.INFO.20230329-115901.283745
    ```


    </details>

```ruby
$ kne create kne-xrd-sr.pb.txt
```

#### Verify

```ruby
$ kubectl get pods -A
NAMESPACE            NAME                                                     READY   STATUS    RESTARTS   AGE
kne-xrd-sr           ansible                                                  1/1     Running   0          45s
kne-xrd-sr           xrd-1                                                    1/1     Running   0          48s
kne-xrd-sr           xrd-2                                                    1/1     Running   0          47s
kne-xrd-sr           xrd-3                                                    1/1     Running   0          48s
kne-xrd-sr           xrd-4                                                    1/1     Running   0          48s
kne-xrd-sr           xrd-5                                                    1/1     Running   0          48s
kne-xrd-sr           xrd-6                                                    1/1     Running   0          46s
kne-xrd-sr           xrd-7                                                    1/1     Running   0          45s
kne-xrd-sr           xrd-8                                                    1/1     Running   0          45s
kne-xrd-sr           xrd-9                                                    1/1     Running   0          47s
kube-system          coredns-787d4945fb-dfsxj                                 1/1     Running   0          4m56s
kube-system          coredns-787d4945fb-w4znm                                 1/1     Running   0          4m56s
kube-system          etcd-kne-control-plane                                   1/1     Running   0          5m8s
kube-system          kindnet-4km75                                            1/1     Running   0          4m56s
kube-system          kube-apiserver-kne-control-plane                         1/1     Running   0          5m8s
kube-system          kube-controller-manager-kne-control-plane                1/1     Running   0          5m8s
kube-system          kube-proxy-r2lt9                                         1/1     Running   0          4m56s
kube-system          kube-scheduler-kne-control-plane                         1/1     Running   0          5m8s
local-path-storage   local-path-provisioner-c8855d4bb-l4zqp                   1/1     Running   0          4m56s
meshnet              meshnet-kjlcp                                            1/1     Running   0          4m7s
metallb-system       controller-8bb68977b-nkrss                               1/1     Running   0          4m56s
metallb-system       speaker-88wl4                                            1/1     Running   0          4m45s
srlinux-controller   srlinux-controller-controller-manager-5bddb8b985-rskj4   2/2     Running   0          4m7s
```

```ruby
$ kubectl get services -n kne-xrd-sr
NAME            TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                        AGE
service-xrd-1   LoadBalancer   10.96.190.38    172.18.0.52   22:31566/TCP,57400:31205/TCP   91s
service-xrd-2   LoadBalancer   10.96.122.231   172.18.0.55   57400:30862/TCP,22:30329/TCP   89s
service-xrd-3   LoadBalancer   10.96.170.95    172.18.0.53   57400:30360/TCP,22:32311/TCP   91s
service-xrd-4   LoadBalancer   10.96.165.20    172.18.0.50   22:32425/TCP,57400:32621/TCP   91s
service-xrd-5   LoadBalancer   10.96.170.82    172.18.0.51   22:30277/TCP,57400:32491/TCP   91s
service-xrd-6   LoadBalancer   10.96.57.186    172.18.0.56   22:30482/TCP,57400:30822/TCP   89s
service-xrd-7   LoadBalancer   10.96.193.159   172.18.0.57   22:30336/TCP,57400:30530/TCP   88s
service-xrd-8   LoadBalancer   10.96.208.94    172.18.0.58   22:30540/TCP,57400:31636/TCP   87s
service-xrd-9   LoadBalancer   10.96.255.147   172.18.0.54   22:32611/TCP,57400:31850/TCP   90s
```


## Modify the ansible container accordingly

#### Find out mgmt IPs of the xrd containers
```ruby
lab@xrdlab:~$ kubectl describe services -A | grep -i -E "Name:|:22"
Name:              kubernetes
Name:                     service-xrd-1
Endpoints:                10.244.0.9:22
Name:                     service-xrd-2
Endpoints:                10.244.0.12:22
Name:                     service-xrd-3
Endpoints:                10.244.0.10:22
Name:                     service-xrd-4
Endpoints:                10.244.0.8:22
Name:                     service-xrd-5
Endpoints:                10.244.0.7:22
Name:                     service-xrd-6
Endpoints:                10.244.0.13:22
Name:                     service-xrd-7
Endpoints:                10.244.0.14:22
Name:                     service-xrd-8
Endpoints:                10.244.0.16:22
Name:                     service-xrd-9
Endpoints:                10.244.0.11:22
```

#### Change as per below
```ruby
$ kubectl exec -it -n kne-xrd-sr ansible -- sh
Defaulted container "ansible" out of: ansible, init-ansible (init)
/home/ansible-code #

/home/ansible-code # cat all_inclusive_play.yaml
---
# Revert topology to base stage
#- import_playbook: 0_revert_to_base_topology.yaml

# Lab step by step
- import_playbook: 1_getting_started.yaml
- import_playbook: 2_intro_segment_routing.yaml
- import_playbook: 3_sr_ldp_interworking.yaml
- import_playbook: 4_tilfa.yaml
- import_playbook: 5_zs_tilfa.yaml
- import_playbook: 6_ss_tilfa.yaml
- import_playbook: 7_ds_tilfa.yaml
- import_playbook: 8_microloop_avoidance.yaml
- import_playbook: 9_mpls_traffic_eng.yaml
- import_playbook: 10_tilfa_ns_prep.yaml
- import_playbook: 11_tilfa_ns_node.yaml
- import_playbook: 12_tilfa_ns_local_srlg.yaml
- import_playbook: 13_tilfa_ns_global_srlg.yaml
- import_playbook: 14_tilfa_ns_node_plus_srlg.yaml
- import_playbook: 15_tilfa_tiebrkr_link_node_srlg.yaml

/home/ansible-code # cat inventory/hosts.ini
###hosts.ini
[routers]
PE1    ansible_host=10.244.0.9
P2     ansible_host=10.244.0.12
P3     ansible_host=10.244.0.10
P4     ansible_host=10.244.0.8
PE5    ansible_host=10.244.0.7
P6     ansible_host=10.244.0.13
P7     ansible_host=10.244.0.14
P8     ansible_host=10.244.0.16
P9     ansible_host=10.244.0.11

[zstilfa]
P2     ansible_host=10.244.0.12
P6     ansible_host=10.244.0.13
P7     ansible_host=10.244.0.14
```

## Run Playbook
**Note**: Please don't run the ```0_revert_to_base_topology.yaml```. It is not apt for this setup.

```ruby
$ kubectl exec -it -n kne-xrd-sr ansible -- ansible-playbook 1_getting_started.yaml -e ansible_user="cisco" -e ansible_password="cisco"
```

<details>
<summary><h4>Playbook Output</h4></summary>

```ruby
$ kubectl exec -it -n kne-xrd-sr ansible -- ansible-playbook 1_getting_started.yaml -e ansible_user="cisco" -e ansible_password="cisco"
Defaulted container "ansible" out of: ansible, init-ansible (init)

PLAY [1. CISCO XRV9K SR WITH TI-LFA LAB PLAYBOOK - GETTING STARTED] ******************************************************************************************************************************************************

TASK [1.0 RENDER AND DISPLAY THE IP, ISIS & LDP CONFIGURATION] ***********************************************************************************************************************************************************
ok: [PE1] =>
  msg:
  - 'template: ip_igp_ldp_template.j2'
  - - ''
    - hostname PE1
    - root
    - ''
    - interface Loopback0
    - ' ipv4 address 1.1.1.1/32'
    - ' description System_Loopback_Interface'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - interface GigabitEthernet0/0/0/2
    - ' ipv4 address 12.0.0.1/24'
    - ' description PE1_to_P2'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - router isis IGP
    - ' is-type level-2-only'
    - ' net 49.0000.0000.0001.00'
    - ' log adjacency changes'
    - ' address-family ipv4 unicast'
    - '  mpls ldp auto-config'
    - '  metric-style wide level 2'
    - ' !'
    - ' interface Loopback0'
    - '  passive'
    - '  address-family ipv4 unicast'
    - '  !'
    - ' !'
    - '  interface GigabitEthernet0/0/0/2'
    - '  circuit-type level-2-only'
    - '  point-to-point'
    - '  hello-padding disable'
    - '  address-family ipv4 unicast'
    - '   metric 10'
    - '  !'
    - ' !'
    - '  !'
    - root
    - ''
    - mpls oam
    - '!'
    - mpls ldp
    - ' log'
    - '  neighbor'
    - ' !'
    - '!'
    - root
    - '!'
    - ''
    - ''
ok: [PE5] =>
  msg:
  - 'template: ip_igp_ldp_template.j2'
  - - ''
    - hostname PE5
    - root
    - ''
    - interface Loopback0
    - ' ipv4 address 5.5.5.5/32'
    - ' description System_Loopback_Interface'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - interface GigabitEthernet0/0/0/0
    - ' ipv4 address 45.0.0.5/24'
    - ' description PE5_to_P4'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - router isis IGP
    - ' is-type level-2-only'
    - ' net 49.0000.0000.0005.00'
    - ' log adjacency changes'
    - ' address-family ipv4 unicast'
    - '  mpls ldp auto-config'
    - '  metric-style wide level 2'
    - ' !'
    - ' interface Loopback0'
    - '  passive'
    - '  address-family ipv4 unicast'
    - '  !'
    - ' !'
    - '  interface GigabitEthernet0/0/0/0'
    - '  circuit-type level-2-only'
    - '  point-to-point'
    - '  hello-padding disable'
    - '  address-family ipv4 unicast'
    - '   metric 10'
    - '  !'
    - ' !'
    - '  !'
    - root
    - ''
    - mpls oam
    - '!'
    - mpls ldp
    - ' log'
    - '  neighbor'
    - ' !'
    - '!'
    - root
    - '!'
    - ''
    - ''
ok: [P6] =>
  msg:
  - 'template: ip_igp_ldp_template.j2'
  - - ''
    - hostname P6
    - root
    - ''
    - interface Loopback0
    - ' ipv4 address 6.6.6.6/32'
    - ' description System_Loopback_Interface'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - interface GigabitEthernet0/0/0/0
    - ' ipv4 address 67.0.0.6/24'
    - ' description P6_to_P7'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - interface GigabitEthernet0/0/0/1
    - ' ipv4 address 26.0.0.6/24'
    - ' description P6_to_P2'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - router isis IGP
    - ' is-type level-2-only'
    - ' net 49.0000.0000.0006.00'
    - ' log adjacency changes'
    - ' address-family ipv4 unicast'
    - '  mpls ldp auto-config'
    - '  metric-style wide level 2'
    - ' !'
    - ' interface Loopback0'
    - '  passive'
    - '  address-family ipv4 unicast'
    - '  !'
    - ' !'
    - '  interface GigabitEthernet0/0/0/0'
    - '  circuit-type level-2-only'
    - '  point-to-point'
    - '  hello-padding disable'
    - '  address-family ipv4 unicast'
    - '   metric 10'
    - '  !'
    - ' !'
    - '  interface GigabitEthernet0/0/0/1'
    - '  circuit-type level-2-only'
    - '  point-to-point'
    - '  hello-padding disable'
    - '  address-family ipv4 unicast'
    - '   metric 10'
    - '  !'
    - ' !'
    - '  !'
    - root
    - ''
    - mpls oam
    - '!'
    - mpls ldp
    - ' log'
    - '  neighbor'
    - ' !'
    - '!'
    - root
    - '!'
    - ''
    - ''
ok: [P2] =>
  msg:
  - 'template: ip_igp_ldp_template.j2'
  - - ''
    - hostname P2
    - root
    - ''
    - interface Loopback0
    - ' ipv4 address 2.2.2.2/32'
    - ' description System_Loopback_Interface'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - interface GigabitEthernet0/0/0/0
    - ' ipv4 address 23.0.0.2/24'
    - ' description P2_to_P3'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - interface GigabitEthernet0/0/0/1
    - ' ipv4 address 26.0.0.2/24'
    - ' description P2_to_P6'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - interface GigabitEthernet0/0/0/2
    - ' ipv4 address 12.0.0.2/24'
    - ' description P2_to_PE1'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - interface GigabitEthernet0/0/0/3
    - ' ipv4 address 27.0.0.2/24'
    - ' description P2_to_P7'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - interface GigabitEthernet0/0/0/5
    - ' ipv4 address 29.0.0.2/24'
    - ' description P2_to_P9'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - router isis IGP
    - ' is-type level-2-only'
    - ' net 49.0000.0000.0002.00'
    - ' log adjacency changes'
    - ' address-family ipv4 unicast'
    - '  mpls ldp auto-config'
    - '  metric-style wide level 2'
    - ' !'
    - ' interface Loopback0'
    - '  passive'
    - '  address-family ipv4 unicast'
    - '  !'
    - ' !'
    - '  interface GigabitEthernet0/0/0/0'
    - '  circuit-type level-2-only'
    - '  point-to-point'
    - '  hello-padding disable'
    - '  address-family ipv4 unicast'
    - '   metric 10'
    - '  !'
    - ' !'
    - '  interface GigabitEthernet0/0/0/1'
    - '  circuit-type level-2-only'
    - '  point-to-point'
    - '  hello-padding disable'
    - '  address-family ipv4 unicast'
    - '   metric 10'
    - '  !'
    - ' !'
    - '  interface GigabitEthernet0/0/0/2'
    - '  circuit-type level-2-only'
    - '  point-to-point'
    - '  hello-padding disable'
    - '  address-family ipv4 unicast'
    - '   metric 10'
    - '  !'
    - ' !'
    - '  interface GigabitEthernet0/0/0/3'
    - '  circuit-type level-2-only'
    - '  point-to-point'
    - '  hello-padding disable'
    - '  address-family ipv4 unicast'
    - '   metric 10'
    - '  !'
    - ' !'
    - '  interface GigabitEthernet0/0/0/5'
    - '  circuit-type level-2-only'
    - '  point-to-point'
    - '  hello-padding disable'
    - '  address-family ipv4 unicast'
    - '   metric 10'
    - '  !'
    - ' !'
    - '  !'
    - root
    - ''
    - mpls oam
    - '!'
    - mpls ldp
    - ' log'
    - '  neighbor'
    - ' !'
    - '!'
    - root
    - '!'
    - ''
    - ''
ok: [P4] =>
  msg:
  - 'template: ip_igp_ldp_template.j2'
  - - ''
    - hostname P4
    - root
    - ''
    - interface Loopback0
    - ' ipv4 address 4.4.4.4/32'
    - ' description System_Loopback_Interface'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - interface GigabitEthernet0/0/0/0
    - ' ipv4 address 45.0.0.4/24'
    - ' description P4_to_PE5'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - interface GigabitEthernet0/0/0/1
    - ' ipv4 address 48.0.0.4/24'
    - ' description P4_to_P8'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - interface GigabitEthernet0/0/0/2
    - ' ipv4 address 34.0.0.4/24'
    - ' description P4_to_P3'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - interface GigabitEthernet0/0/0/4
    - ' ipv4 address 47.0.0.4/24'
    - ' description P4_to_P7'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - router isis IGP
    - ' is-type level-2-only'
    - ' net 49.0000.0000.0004.00'
    - ' log adjacency changes'
    - ' address-family ipv4 unicast'
    - '  mpls ldp auto-config'
    - '  metric-style wide level 2'
    - ' !'
    - ' interface Loopback0'
    - '  passive'
    - '  address-family ipv4 unicast'
    - '  !'
    - ' !'
    - '  interface GigabitEthernet0/0/0/0'
    - '  circuit-type level-2-only'
    - '  point-to-point'
    - '  hello-padding disable'
    - '  address-family ipv4 unicast'
    - '   metric 10'
    - '  !'
    - ' !'
    - '  interface GigabitEthernet0/0/0/1'
    - '  circuit-type level-2-only'
    - '  point-to-point'
    - '  hello-padding disable'
    - '  address-family ipv4 unicast'
    - '   metric 10'
    - '  !'
    - ' !'
    - '  interface GigabitEthernet0/0/0/2'
    - '  circuit-type level-2-only'
    - '  point-to-point'
    - '  hello-padding disable'
    - '  address-family ipv4 unicast'
    - '   metric 10'
    - '  !'
    - ' !'
    - '  interface GigabitEthernet0/0/0/4'
    - '  circuit-type level-2-only'
    - '  point-to-point'
    - '  hello-padding disable'
    - '  address-family ipv4 unicast'
    - '   metric 10'
    - '  !'
    - ' !'
    - '  !'
    - root
    - ''
    - mpls oam
    - '!'
    - mpls ldp
    - ' log'
    - '  neighbor'
    - ' !'
    - '!'
    - root
    - '!'
    - ''
    - ''
ok: [P3] =>
  msg:
  - 'template: ip_igp_ldp_template.j2'
  - - ''
    - hostname P3
    - root
    - ''
    - interface Loopback0
    - ' ipv4 address 3.3.3.3/32'
    - ' description System_Loopback_Interface'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - interface GigabitEthernet0/0/0/0
    - ' ipv4 address 23.0.0.3/24'
    - ' description P3_to_P2'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - interface GigabitEthernet0/0/0/1
    - ' ipv4 address 37.0.0.3/24'
    - ' description P3_to_P7'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - interface GigabitEthernet0/0/0/2
    - ' ipv4 address 34.0.0.3/24'
    - ' description P3_to_P4'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - interface GigabitEthernet0/0/0/3
    - ' ipv4 address 39.0.0.3/24'
    - ' description P3_to_P9'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - router isis IGP
    - ' is-type level-2-only'
    - ' net 49.0000.0000.0003.00'
    - ' log adjacency changes'
    - ' address-family ipv4 unicast'
    - '  mpls ldp auto-config'
    - '  metric-style wide level 2'
    - ' !'
    - ' interface Loopback0'
    - '  passive'
    - '  address-family ipv4 unicast'
    - '  !'
    - ' !'
    - '  interface GigabitEthernet0/0/0/0'
    - '  circuit-type level-2-only'
    - '  point-to-point'
    - '  hello-padding disable'
    - '  address-family ipv4 unicast'
    - '   metric 10'
    - '  !'
    - ' !'
    - '  interface GigabitEthernet0/0/0/1'
    - '  circuit-type level-2-only'
    - '  point-to-point'
    - '  hello-padding disable'
    - '  address-family ipv4 unicast'
    - '   metric 10'
    - '  !'
    - ' !'
    - '  interface GigabitEthernet0/0/0/2'
    - '  circuit-type level-2-only'
    - '  point-to-point'
    - '  hello-padding disable'
    - '  address-family ipv4 unicast'
    - '   metric 10'
    - '  !'
    - ' !'
    - '  interface GigabitEthernet0/0/0/3'
    - '  circuit-type level-2-only'
    - '  point-to-point'
    - '  hello-padding disable'
    - '  address-family ipv4 unicast'
    - '   metric 10'
    - '  !'
    - ' !'
    - '  !'
    - root
    - ''
    - mpls oam
    - '!'
    - mpls ldp
    - ' log'
    - '  neighbor'
    - ' !'
    - '!'
    - root
    - '!'
    - ''
    - ''
ok: [P8] =>
  msg:
  - 'template: ip_igp_ldp_template.j2'
  - - ''
    - hostname P8
    - root
    - ''
    - interface Loopback0
    - ' ipv4 address 8.8.8.8/32'
    - ' description System_Loopback_Interface'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - interface GigabitEthernet0/0/0/1
    - ' ipv4 address 48.0.0.8/24'
    - ' description P8_to_P4'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - interface GigabitEthernet0/0/0/2
    - ' ipv4 address 78.0.0.8/24'
    - ' description P8_to_P7'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - router isis IGP
    - ' is-type level-2-only'
    - ' net 49.0000.0000.0008.00'
    - ' log adjacency changes'
    - ' address-family ipv4 unicast'
    - '  mpls ldp auto-config'
    - '  metric-style wide level 2'
    - ' !'
    - ' interface Loopback0'
    - '  passive'
    - '  address-family ipv4 unicast'
    - '  !'
    - ' !'
    - '  interface GigabitEthernet0/0/0/0'
    - '  circuit-type level-2-only'
    - '  point-to-point'
    - '  hello-padding disable'
    - '  address-family ipv4 unicast'
    - '   metric 10'
    - '  !'
    - ' !'
    - '  interface GigabitEthernet0/0/0/1'
    - '  circuit-type level-2-only'
    - '  point-to-point'
    - '  hello-padding disable'
    - '  address-family ipv4 unicast'
    - '   metric 10'
    - '  !'
    - ' !'
    - '  interface GigabitEthernet0/0/0/2'
    - '  circuit-type level-2-only'
    - '  point-to-point'
    - '  hello-padding disable'
    - '  address-family ipv4 unicast'
    - '   metric 10'
    - '  !'
    - ' !'
    - '  !'
    - root
    - ''
    - mpls oam
    - '!'
    - mpls ldp
    - ' log'
    - '  neighbor'
    - ' !'
    - '!'
    - root
    - '!'
    - ''
    - ''
ok: [P7] =>
  msg:
  - 'template: ip_igp_ldp_template.j2'
  - - ''
    - hostname P7
    - root
    - ''
    - interface Loopback0
    - ' ipv4 address 7.7.7.7/32'
    - ' description System_Loopback_Interface'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - interface GigabitEthernet0/0/0/0
    - ' ipv4 address 67.0.0.7/24'
    - ' description P7_to_P6'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - interface GigabitEthernet0/0/0/1
    - ' ipv4 address 37.0.0.7/24'
    - ' description P7_to_P7'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - interface GigabitEthernet0/0/0/2
    - ' ipv4 address 78.0.0.7/24'
    - ' description P7_to_P8'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - interface GigabitEthernet0/0/0/3
    - ' ipv4 address 27.0.0.7/24'
    - ' description P7_to_P2'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - interface GigabitEthernet0/0/0/4
    - ' ipv4 address 47.0.0.7/24'
    - ' description P7_to_P4'
    - ' no shutdown'
    - '!'
    - root
    - ''
    - router isis IGP
    - ' is-type level-2-only'
    - ' net 49.0000.0000.0007.00'
    - ' log adjacency changes'
    - ' address-family ipv4 unicast'
    - '  mpls ldp auto-config'
    - '  metric-style wide level 2'
    - ' !'
    - ' interface Loopback0'
    - '  passive'
    - '  address-family ipv4 unicast'
    - '  !'
    - ' !'
    - '  interface GigabitEthernet0/0/0/0'
    - '  circuit-type level-2-only'
    - '  point-to-point'
    - '  hello-padding disable'
    - '  address-family ipv4 unicast'
    - '   metric 10'
    - '  !'
    - ' !'
    - '  interface GigabitEthernet0/0/0/1'
    - '  circuit-type level-2-only'
    - '  point-to-point'
    - '  hello-padding disable'
    - '  address-family ipv4 unicast'
    - '   metric 10'
    - '  !'
    - ' !'
    - '  interface GigabitEthernet0/0/0/2'
    - '  circuit-type level-2-only'
    - '  point-to-point'
    - '  hello-padding disable'
    - '  address-family ipv4 unicast'
    - '   metric 10'
    - '  !'
    - ' !'
    - '  interface GigabitEthernet0/0/0/3'
    - '  circuit-type level-2-only'
    - '  point-to-point'
    - '  hello-padding disable'
    - '  address-family ipv4 unicast'
    - '   metric 10'
    - '  !'
    - ' !'
    - '  interface GigabitEthernet0/0/0/4'
    - '  circuit-type level-2-only'
    - '  point-to-point'
    - '  hello-padding disable'
    - '  address-family ipv4 unicast'
    - '   metric 10'
    - '  !'
    - ' !'
    - '  !'
    - root
    - ''
    - mpls oam
    - '!'
    - mpls ldp
    - ' log'
    - '  neighbor'
    - ' !'
    - '!'
    - root
    - '!'
    - ''
    - ''

TASK [1.1 RENDER AND SAVE THE IP, ISIS & LDP CONFIGURATION] **************************************************************************************************************************************************************
ok: [P3]
ok: [P2]
ok: [P6]
ok: [PE1]
ok: [P8]
ok: [P4]
ok: [P7]
ok: [PE5]
Pausing for 1 seconds
(ctrl+C then 'C' = continue early, ctrl+C then 'A' = abort)

TASK [1 secs PAUSE FOR CONVERGENCE] **************************************************************************************************************************************************************************************
ok: [PE1]

TASK [1.2 DISPLAY THE OUTPUT REGISTERED IN THE PREVIOUS TASK] ************************************************************************************************************************************************************
ok: [P2] =>
  output.dest: xrcfg/P2.ip_mpls.xrcfg
ok: [PE5] =>
  output.dest: xrcfg/PE5.ip_mpls.xrcfg
ok: [PE1] =>
  output.dest: xrcfg/PE1.ip_mpls.xrcfg
ok: [P6] =>
  output.dest: xrcfg/P6.ip_mpls.xrcfg
ok: [P4] =>
  output.dest: xrcfg/P4.ip_mpls.xrcfg
ok: [P8] =>
  output.dest: xrcfg/P8.ip_mpls.xrcfg
ok: [P3] =>
  output.dest: xrcfg/P3.ip_mpls.xrcfg
ok: [P7] =>
  output.dest: xrcfg/P7.ip_mpls.xrcfg

TASK [1.3 RENDER AND APPLY THE IP, ISIS & LDP CONFIGURATION] *************************************************************************************************************************************************************
[WARNING]: To ensure idempotency and correct diff the input configuration lines should be similar to how they appear if present in the running configuration on device including the indentation
changed: [PE1]
changed: [PE5]
changed: [P6]
changed: [P8]
changed: [P3]
changed: [P4]
changed: [P7]
changed: [P2]
Pausing for 120 seconds
(ctrl+C then 'C' = continue early, ctrl+C then 'A' = abort)

TASK [2 mins PAUSE FOR CONVERGENCE] **************************************************************************************************************************************************************************************
ok: [PE1]

TASK [1.4 RUN COMMANDS TO CAPTURE IP, ISIS & LDP STATE] ******************************************************************************************************************************************************************
ok: [PE1] => (item=show ipv4 interface brief)
ok: [PE1] => (item=ping 12.0.0.2)
ok: [PE1] => (item=show isis adjacency)
ok: [PE1] => (item=show route)
ok: [PE1] => (item=show isis route 5.5.5.5/32 detail)
ok: [PE1] => (item=show route 5.5.5.5/32 detail)
ok: [PE1] => (item=show mpls ldp neighbor brief)
ok: [PE1] => (item=show mpls label table)
ok: [PE1] => (item=show mpls forwarding)
ok: [PE1] => (item=show cef 5.5.5.5/32)
ok: [PE1] => (item=traceroute mpls ipv4 5.5.5.5/32)
ok: [PE1] => (item=traceroute mpls multipath ipv4 5.5.5.5/32 verbose)

TASK [1.5 DISPLAY THE OUTPUT REGISTERED IN THE PREVIOUS TASK] ************************************************************************************************************************************************************
ok: [PE1] => (item=show ipv4 interface brief) =>
  msg:
  - - - Interface                      IP-Address      Status          Protocol Vrf-Name
      - 'Loopback0                      1.1.1.1         Up              Up       default '
      - 'MgmtEth0/RP0/CPU0/0            10.244.0.9      Up              Up       default '
      - GigabitEthernet0/0/0/2         12.0.0.1        Up              Up       default
ok: [PE1] => (item=ping 12.0.0.2) =>
  msg:
  - - - Type escape sequence to abort.
      - 'Sending 5, 100-byte ICMP Echos to 12.0.0.2, timeout is 2 seconds:'
      - '!!!!!'
      - Success rate is 100 percent (5/5), round-trip min/avg/max = 2/2/4 ms
ok: [PE1] => (item=show isis adjacency) =>
  msg:
  - - - 'IS-IS IGP Level-2 adjacencies:'
      - System Id      Interface                SNPA           State Hold Changed  NSF IPv4 IPv6
      - '                                                                               BFD  BFD '
      - P2             Gi0/0/0/2                *PtoP*         Up    26   00:19:08 Yes None None
      - ''
      - 'Total adjacency count: 1'
ok: [PE1] => (item=show route) =>
  msg:
  - - - 'Codes: C - connected, S - static, R - RIP, B - BGP, (>) - Diversion path'
      - '       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area'
      - '       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2'
      - '       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP'
      - '       i - ISIS, L1 - IS-IS level-1, L2 - IS-IS level-2'
      - '       ia - IS-IS inter area, su - IS-IS summary null, * - candidate default'
      - '       U - per-user static route, o - ODR, L - local, G  - DAGR, l - LISP'
      - '       A - access/subscriber, a - Application route'
      - '       M - mobile route, r - RPL, t - Traffic Engineering, (!) - FRR Backup path'
      - ''
      - Gateway of last resort is not set
      - ''
      - L    1.1.1.1/32 is directly connected, 00:19:19, Loopback0
      - i L2 2.2.2.2/32 [115/10] via 12.0.0.2, 00:19:03, GigabitEthernet0/0/0/2
      - i L2 3.3.3.3/32 [115/20] via 12.0.0.2, 00:18:52, GigabitEthernet0/0/0/2
      - i L2 4.4.4.4/32 [115/30] via 12.0.0.2, 00:18:52, GigabitEthernet0/0/0/2
      - i L2 5.5.5.5/32 [115/40] via 12.0.0.2, 00:18:52, GigabitEthernet0/0/0/2
      - i L2 6.6.6.6/32 [115/20] via 12.0.0.2, 00:18:52, GigabitEthernet0/0/0/2
      - i L2 7.7.7.7/32 [115/20] via 12.0.0.2, 00:18:52, GigabitEthernet0/0/0/2
      - i L2 8.8.8.8/32 [115/30] via 12.0.0.2, 00:18:52, GigabitEthernet0/0/0/2
      - C    10.244.0.0/24 is directly connected, 01:10:05, MgmtEth0/RP0/CPU0/0
      - L    10.244.0.9/32 is directly connected, 01:10:05, MgmtEth0/RP0/CPU0/0
      - C    12.0.0.0/24 is directly connected, 00:19:19, GigabitEthernet0/0/0/2
      - L    12.0.0.1/32 is directly connected, 00:19:19, GigabitEthernet0/0/0/2
      - i L2 23.0.0.0/24 [115/20] via 12.0.0.2, 00:19:03, GigabitEthernet0/0/0/2
      - i L2 26.0.0.0/24 [115/20] via 12.0.0.2, 00:19:03, GigabitEthernet0/0/0/2
      - i L2 27.0.0.0/24 [115/20] via 12.0.0.2, 00:19:03, GigabitEthernet0/0/0/2
      - i L2 29.0.0.0/24 [115/20] via 12.0.0.2, 00:19:03, GigabitEthernet0/0/0/2
      - i L2 34.0.0.0/24 [115/30] via 12.0.0.2, 00:18:52, GigabitEthernet0/0/0/2
      - i L2 37.0.0.0/24 [115/30] via 12.0.0.2, 00:18:52, GigabitEthernet0/0/0/2
      - i L2 39.0.0.0/24 [115/30] via 12.0.0.2, 00:18:52, GigabitEthernet0/0/0/2
      - i L2 45.0.0.0/24 [115/40] via 12.0.0.2, 00:18:52, GigabitEthernet0/0/0/2
      - i L2 47.0.0.0/24 [115/30] via 12.0.0.2, 00:18:52, GigabitEthernet0/0/0/2
      - i L2 48.0.0.0/24 [115/40] via 12.0.0.2, 00:18:52, GigabitEthernet0/0/0/2
      - i L2 67.0.0.0/24 [115/30] via 12.0.0.2, 00:18:52, GigabitEthernet0/0/0/2
      - i L2 78.0.0.0/24 [115/30] via 12.0.0.2, 00:18:52, GigabitEthernet0/0/0/2
      - L    127.0.0.0/8 [0/0] via 0.0.0.0, 00:19:19
ok: [PE1] => (item=show isis route 5.5.5.5/32 detail) =>
  msg:
  - - - 'L2 5.5.5.5/32 [40/115] Label: None, medium priority'
      - '   Installed Mar 29 12:52:21.204 for 00:18:54'
      - '     via 12.0.0.2, GigabitEthernet0/0/0/2, P2, Weight: 0'
      - '     src PE5.00-00, 5.5.5.5'
ok: [PE1] => (item=show route 5.5.5.5/32 detail) =>
  msg:
  - - - Routing entry for 5.5.5.5/32
      - '  Known via "isis IGP", distance 115, metric 40, type level-2'
      - '  Installed Mar 29 12:52:21.204 for 00:18:57'
      - '  Routing Descriptor Blocks'
      - '    12.0.0.2, from 5.5.5.5, via GigabitEthernet0/0/0/2'
      - '      Route metric is 40'
      - '      Label: None'
      - '      Tunnel ID: None'
      - '      Binding Label: None'
      - '      Extended communities count: 0'
      - "      Path id:1\t      Path ref count:0"
      - '      NHID:0x1(Ref:20)'
      - '  Route version is 0x1 (1)'
      - '  No local label'
      - '  IP Precedence: Not Set'
      - '  QoS Group ID: Not Set'
      - '  Flow-tag: Not Set'
      - '  Fwd-class: Not Set'
      - '  Route Priority: RIB_PRIORITY_NON_RECURSIVE_MEDIUM (7) SVD Type RIB_SVD_TYPE_LOCAL'
      - '  Download Priority 1, Download Version 23'
      - '  No advertising protos.'
ok: [PE1] => (item=show mpls ldp neighbor brief) =>
  msg:
  - - - 'Peer               GR  NSR  Up Time     Discovery   Addresses     Labels    '
      - '                                        ipv4  ipv6  ipv4  ipv6  ipv4   ipv6 '
      - '-----------------  --  ---  ----------  ----------  ----------  ------------'
      - 2.2.2.2:0          N   N    00:18:59    1     0     7     0     22     0
ok: [PE1] => (item=show mpls label table) =>
  msg:
  - - - Table Label   Owner                           State  Rewrite
      - '----- ------- ------------------------------- ------ -------'
      - 0     0       LSD(A)                          InUse  Yes
      - 0     1       LSD(A)                          InUse  Yes
      - 0     2       LSD(A)                          InUse  Yes
      - 0     13      LSD(A)                          InUse  Yes
      - 0     24000   LDP(A)                          InUse  Yes
      - 0     24001   LDP(A)                          InUse  Yes
      - 0     24002   LDP(A)                          InUse  Yes
      - 0     24003   LDP(A)                          InUse  Yes
      - 0     24004   LDP(A)                          InUse  Yes
      - 0     24005   LDP(A)                          InUse  Yes
      - 0     24006   LDP(A)                          InUse  Yes
      - 0     24007   LDP(A)                          InUse  Yes
      - 0     24008   LDP(A)                          InUse  Yes
      - 0     24009   LDP(A)                          InUse  Yes
      - 0     24010   LDP(A)                          InUse  Yes
      - 0     24011   LDP(A)                          InUse  Yes
      - 0     24012   LDP(A)                          InUse  Yes
      - 0     24013   LDP(A)                          InUse  Yes
      - 0     24014   LDP(A)                          InUse  Yes
      - 0     24015   LDP(A)                          InUse  Yes
      - 0     24016   LDP(A)                          InUse  Yes
      - 0     24017   LDP(A)                          InUse  Yes
      - 0     24018   LDP(A)                          InUse  Yes
ok: [PE1] => (item=show mpls forwarding) =>
  msg:
  - - - 'Local  Outgoing    Prefix             Outgoing     Next Hop        Bytes       '
      - 'Label  Label       or ID              Interface                    Switched    '
      - '------ ----------- ------------------ ------------ --------------- ------------'
      - '24000  Pop         2.2.2.2/32         Gi0/0/0/2    12.0.0.2        2376        '
      - '24001  Pop         29.0.0.0/24        Gi0/0/0/2    12.0.0.2        0           '
      - '24002  Pop         27.0.0.0/24        Gi0/0/0/2    12.0.0.2        0           '
      - '24003  Pop         26.0.0.0/24        Gi0/0/0/2    12.0.0.2        0           '
      - '24004  Pop         23.0.0.0/24        Gi0/0/0/2    12.0.0.2        0           '
      - '24005  24002       3.3.3.3/32         Gi0/0/0/2    12.0.0.2        0           '
      - '24006  24001       6.6.6.6/32         Gi0/0/0/2    12.0.0.2        0           '
      - '24007  24000       7.7.7.7/32         Gi0/0/0/2    12.0.0.2        0           '
      - '24008  24010       4.4.4.4/32         Gi0/0/0/2    12.0.0.2        0           '
      - '24009  24012       8.8.8.8/32         Gi0/0/0/2    12.0.0.2        0           '
      - '24010  24011       5.5.5.5/32         Gi0/0/0/2    12.0.0.2        0           '
      - '24011  24007       39.0.0.0/24        Gi0/0/0/2    12.0.0.2        0           '
      - '24012  24006       67.0.0.0/24        Gi0/0/0/2    12.0.0.2        0           '
      - '24013  24008       37.0.0.0/24        Gi0/0/0/2    12.0.0.2        0           '
      - '24014  24005       47.0.0.0/24        Gi0/0/0/2    12.0.0.2        0           '
      - '24015  24009       34.0.0.0/24        Gi0/0/0/2    12.0.0.2        0           '
      - '24016  24004       78.0.0.0/24        Gi0/0/0/2    12.0.0.2        0           '
      - '24017  24014       48.0.0.0/24        Gi0/0/0/2    12.0.0.2        0           '
      - 24018  24013       45.0.0.0/24        Gi0/0/0/2    12.0.0.2        0
ok: [PE1] => (item=show cef 5.5.5.5/32) =>
  msg:
  - - - 5.5.5.5/32, version 39, internal 0x1000001 0x30 (ptr 0x87183b50) [1], 0x600 (0x87846a88), 0xa28 (0x89170ee8)
      - ' Updated Mar 29 12:52:21.655 '
      - ' local adjacency to GigabitEthernet0/0/0/2'
      - ''
      - ' Prefix Len 32, traffic index 0, precedence n/a, priority 3'
      - '  gateway array (0x876ae400) reference count 42, flags 0x68, source lsd (5), 1 backups'
      - '                [15 type 5 flags 0x8401 (0x891af4c8) ext 0x0 (0x0)]'
      - '  LW-LDI[type=5, refc=3, ptr=0x87846a88, sh-ldi=0x891af4c8]'
      - '  gateway array update type-time 1 Mar 29 12:52:21.655'
      - ' LDI Update time Mar 29 12:52:21.655'
      - ' LW-LDI-TS Mar 29 12:52:21.655'
      - '   via 12.0.0.2/32, GigabitEthernet0/0/0/2, 5 dependencies, weight 0, class 0 [flags 0x0]'
      - '    path-idx 0 NHID 0x1 [0x893caa10 0x0]'
      - '    next hop 12.0.0.2/32'
      - '    local adjacency'
      - '     local label 24010      labels imposed {24011}'
      - ''
      - '    Load distribution: 0 (refcount 15)'
      - ''
      - '    Hash  OK  Interface                 Address'
      - '    0     Y   GigabitEthernet0/0/0/2    12.0.0.2'
ok: [PE1] => (item=traceroute mpls ipv4 5.5.5.5/32) =>
  msg:
  - - - Tracing MPLS Label Switched Path to 5.5.5.5/32, timeout is 2 seconds
      - ''
      - 'Codes: ''!'' - success, ''Q'' - request not sent, ''.'' - timeout,'
      - '  ''L'' - labeled output interface, ''B'' - unlabeled output interface, '
      - '  ''D'' - DS Map mismatch, ''F'' - no FEC mapping, ''f'' - FEC mismatch,'
      - '  ''M'' - malformed request, ''m'' - unsupported tlvs, ''N'' - no rx label, '
      - '  ''P'' - no rx intf label prot, ''p'' - premature termination of LSP, '
      - '  ''R'' - transit router, ''I'' - unknown upstream index,'
      - '  ''X'' - unknown return code, ''x'' - return code 0'
      - ''
      - Type escape sequence to abort.
      - ''
      - '  0 12.0.0.1 MRU 1500 [Labels: 24011 Exp: 0]'
      - 'L 1 12.0.0.2 MRU 1500 [Labels: 24012 Exp: 0] 5 ms'
      - 'L 2 23.0.0.3 MRU 1500 [Labels: 24002 Exp: 0] 6 ms'
      - 'L 3 34.0.0.4 MRU 1500 [Labels: implicit-null Exp: 0] 8 ms'
      - '! 4 45.0.0.5 11 ms'
ok: [PE1] => (item=traceroute mpls multipath ipv4 5.5.5.5/32 verbose) =>
  msg:
  - - - Starting LSP Path Discovery for 5.5.5.5/32
      - ''
      - 'Codes: ''!'' - success, ''Q'' - request not sent, ''.'' - timeout,'
      - '  ''L'' - labeled output interface, ''B'' - unlabeled output interface, '
      - '  ''D'' - DS Map mismatch, ''F'' - no FEC mapping, ''f'' - FEC mismatch,'
      - '  ''M'' - malformed request, ''m'' - unsupported tlvs, ''N'' - no rx label, '
      - '  ''P'' - no rx intf label prot, ''p'' - premature termination of LSP, '
      - '  ''R'' - transit router, ''I'' - unknown upstream index,'
      - '  ''X'' - unknown return code, ''x'' - return code 0'
      - ''
      - Type escape sequence to abort.
      - ''
      - LLL!
      - 'Path 0 found, '
      - ' output interface GigabitEthernet0/0/0/2 nexthop 12.0.0.2'
      - ' source 12.0.0.1 destination 127.0.0.1'
      - '  0 12.0.0.1 12.0.0.2 MRU 1500 [Labels: 24011 Exp: 0] multipaths 0'
      - 'L 1 12.0.0.2 23.0.0.3 MRU 1500 [Labels: 24012 Exp: 0] ret code 8 multipaths 2'
      - 'L 2 23.0.0.3 34.0.0.4 MRU 1500 [Labels: 24002 Exp: 0] ret code 8 multipaths 1'
      - 'L 3 34.0.0.4 45.0.0.5 MRU 1500 [Labels: implicit-null Exp: 0] ret code 8 multipaths 1'
      - '! 4 45.0.0.5, ret code 3 multipaths 0'
      - LL!
      - 'Path 1 found, '
      - ' output interface GigabitEthernet0/0/0/2 nexthop 12.0.0.2'
      - ' source 12.0.0.1 destination 127.0.0.0'
      - '  0 12.0.0.1 12.0.0.2 MRU 1500 [Labels: 24011 Exp: 0] multipaths 0'
      - 'L 1 12.0.0.2 27.0.0.7 MRU 1500 [Labels: 24013 Exp: 0] ret code 8 multipaths 2'
      - 'L 2 27.0.0.7 47.0.0.4 MRU 1500 [Labels: 24002 Exp: 0] ret code 8 multipaths 1'
      - 'L 3 47.0.0.4 45.0.0.5 MRU 1500 [Labels: implicit-null Exp: 0] ret code 8 multipaths 1'
      - '! 4 45.0.0.5, ret code 3 multipaths 0'
      - ''
      - Paths (found/broken/unexplored) (2/0/0)
      - ' Echo Request (sent/fail) (7/0)'
      - ' Echo Reply (received/timeout) (7/0)'
      - ' Total Time Elapsed 59 ms'

TASK [1.6 RUN COMMANDS TO TRACE LABELS FROM PE1 TO PE5] ******************************************************************************************************************************************************************
ok: [PE1]
ok: [P2]
ok: [P3]
ok: [P4]
ok: [P7]

PLAY [1.x CISCO XRV9K SR WITH TI-LFA LAB PLAYBOOK - GETTING STARTED] *****************************************************************************************************************************************************

TASK [DISPLAY THE OUTPUT REGISTERED IN THE PREVIOUS TASK] ****************************************************************************************************************************************************************
ok: [PE1] =>
  msg:
  - show mpls forwarding prefix 5.5.5.5/32
  - - 'Local  Outgoing    Prefix             Outgoing     Next Hop        Bytes       '
    - 'Label  Label       or ID              Interface                    Switched    '
    - '------ ----------- ------------------ ------------ --------------- ------------'
    - 24010  24011       5.5.5.5/32         Gi0/0/0/2    12.0.0.2        0

PLAY [1.x CISCO XRV9K SR WITH TI-LFA LAB PLAYBOOK - GETTING STARTED] *****************************************************************************************************************************************************
ok: [P2] =>
  msg:
  - show mpls forwarding prefix 5.5.5.5/32
  - - 'Local  Outgoing    Prefix             Outgoing     Next Hop        Bytes       '
    - 'Label  Label       or ID              Interface                    Switched    '
    - '------ ----------- ------------------ ------------ --------------- ------------'
    - '24011  24012       5.5.5.5/32         Gi0/0/0/0    23.0.0.3        1536        '
    - '       24013       5.5.5.5/32         Gi0/0/0/3    27.0.0.7        792'

PLAY [1.x CISCO XRV9K SR WITH TI-LFA LAB PLAYBOOK - GETTING STARTED] *****************************************************************************************************************************************************
ok: [P3] =>
  msg:
  - show mpls forwarding prefix 5.5.5.5/32
  - - 'Local  Outgoing    Prefix             Outgoing     Next Hop        Bytes       '
    - 'Label  Label       or ID              Interface                    Switched    '
    - '------ ----------- ------------------ ------------ --------------- ------------'
    - 24012  24002       5.5.5.5/32         Gi0/0/0/2    34.0.0.4        1024

PLAY [1.x CISCO XRV9K SR WITH TI-LFA LAB PLAYBOOK - GETTING STARTED] *****************************************************************************************************************************************************
ok: [P7] =>
  msg:
  - show mpls forwarding prefix 5.5.5.5/32
  - - 'Local  Outgoing    Prefix             Outgoing     Next Hop        Bytes       '
    - 'Label  Label       or ID              Interface                    Switched    '
    - '------ ----------- ------------------ ------------ --------------- ------------'
    - 24013  24002       5.5.5.5/32         Gi0/0/0/4    47.0.0.4        528

PLAY [1.x CISCO XRV9K SR WITH TI-LFA LAB PLAYBOOK - GETTING STARTED] *****************************************************************************************************************************************************
ok: [P4] =>
  msg:
  - show mpls forwarding prefix 5.5.5.5/32
  - - 'Local  Outgoing    Prefix             Outgoing     Next Hop        Bytes       '
    - 'Label  Label       or ID              Interface                    Switched    '
    - '------ ----------- ------------------ ------------ --------------- ------------'
    - 24002  Pop         5.5.5.5/32         Gi0/0/0/0    45.0.0.5        3172

PLAY RECAP ***************************************************************************************************************************************************************************************************************
P2                         : ok=6    changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0
P3                         : ok=6    changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0
P4                         : ok=6    changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0
P6                         : ok=4    changed=1    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0
P7                         : ok=6    changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0
P8                         : ok=4    changed=1    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0
PE1                        : ok=10   changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
PE5                        : ok=4    changed=1    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0
```

</details>


## Changelog
#### [29/03/2023]
* Initial upload
