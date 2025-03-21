# Two-vendor example

## Deploy a KNE Cluster

The first step when using KNE is to deploy a Kubernetes cluster. This can be done using the `kne deploy` command after cloning the KNE repo: `git clone https://github.com/openconfig/kne/` (...and installing `kne` and `kubectl` cli).

```bash
$ kne deploy kne/deploy/kne/external-multinode.yaml
I0306 17:50:32.420836  449937 deploy.go:195] Deploying cluster...
I0306 17:50:32.421104  449937 deploy.go:381] Deploy is a no-op for the external cluster type
I0306 17:50:32.421118  449937 deploy.go:199] Cluster deployed
I0306 17:50:32.475856  449937 run.go:26] (kubectl): Kubernetes control plane is running at https://cto.bloomberg.com:8443
I0306 17:50:32.475883  449937 run.go:26] (kubectl): CoreDNS is running at https://cto.bloomberg.com:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
I0306 17:50:32.475891  449937 run.go:26] (kubectl): To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
I0306 17:50:36.754937  449937 deploy.go:1348] Waiting on deployment "metallb-system" to be healthy
I0306 17:50:52.641096  449937 deploy.go:1375] Deployment "metallb-system" healthy
I0306 17:50:52.646997  449937 deploy.go:933] Applying metallb ingress config
I0306 18:18:01.303808  459343 deploy.go:1348] Waiting on deployment "metallb-system" to be healthy
...
```

## Create topology

```bash
$ cd 2vendors && kne create 2vendors.pb.txt
I0320 20:22:15.830898 1216185 topo.go:174] Trying in-cluster configuration
I0320 20:22:15.831141 1216185 topo.go:177] Falling back to kubeconfig: "/root/.kube/config"
I0320 20:22:15.832410 1216185 topo.go:453] Adding Link: srl:e1-7 cptx:eth12
I0320 20:22:15.832431 1216185 topo.go:453] Adding Link: srl:e1-8 cptx:eth13
I0320 20:22:15.832438 1216185 topo.go:453] Adding Link: srl:e1-9 cptx:eth14
I0320 20:22:15.832452 1216185 topo.go:499] Adding Node: srl:NOKIA
I0320 20:22:15.832572 1216185 topo.go:499] Adding Node: cptx:JUNIPER
I0320 20:22:15.888153 1216185 topo.go:580] Creating namespace for topology: "two-vendors"
I0320 20:22:15.896877 1216185 topo.go:593] Server Namespace: &Namespace{ObjectMeta:{two-vendors    0df03e3e-78f6-49b2-af22-b2a7513c36a8 4021535 0 2025-03-20 20:22:15 +0000 UTC <nil> <nil> map[kne-topology:true kubernetes.io/metadata.name:two-vendors] map[] [] [] [{kne Update v1 2025-03-20 20:22:15 +0000 UTC FieldsV1 {"f:metadata":{"f:labels":{".":{},"f:kne-topology":{},"f:kubernetes.io/metadata.name":{}}}} }]},Spec:NamespaceSpec{Finalizers:[kubernetes],},Status:NamespaceStatus{Phase:Active,Conditions:[]NamespaceCondition{},},}
I0320 20:22:15.896972 1216185 topo.go:640] Getting topology specs for namespace two-vendors
I0320 20:22:15.897058 1216185 topo.go:540] Getting topology specs for node srl
I0320 20:22:15.897082 1216185 topo.go:540] Getting topology specs for node cptx
I0320 20:22:15.897116 1216185 topo.go:647] Creating topology for meshnet node srl
I0320 20:22:15.903824 1216185 topo.go:647] Creating topology for meshnet node cptx
I0320 20:22:15.910802 1216185 topo.go:600] Creating Node Pods
I0320 20:22:15.910849 1216185 nokia.go:204] Creating Srlinux node resource srl
I0320 20:22:15.918294 1216185 nokia.go:209] Created SR Linux node srl configmap
I0320 20:22:15.971338 1216185 nokia.go:271] Created Srlinux resource: srl
I0320 20:22:16.022661 1216185 node.go:519] Created Service:
&Service{ObjectMeta:{service-srl  two-vendors  3deefa39-1cdf-4121-ac62-5102cbc5c1cd 4021556 0 2025-03-20 20:22:15 +0000 UTC <nil> <nil> map[pod:srl] map[] [] [] [{kne Update v1 2025-03-20 20:22:15 +0000 UTC FieldsV1 {"f:metadata":{"f:labels":{".":{},"f:pod":{}}},"f:spec":{"f:allocateLoadBalancerNodePorts":{},"f:externalTrafficPolicy":{},"f:internalTrafficPolicy":{},"f:ports":{".":{},"k:{\"port\":22,\"protocol\":\"TCP\"}":{".":{},"f:name":{},"f:port":{},"f:protocol":{},"f:targetPort":{}},"k:{\"port\":443,\"protocol\":\"TCP\"}":{".":{},"f:name":{},"f:port":{},"f:protocol":{},"f:targetPort":{}},"k:{\"port\":9339,\"protocol\":\"TCP\"}":{".":{},"f:name":{},"f:port":{},"f:protocol":{},"f:targetPort":{}},"k:{\"port\":9340,\"protocol\":\"TCP\"}":{".":{},"f:name":{},"f:port":{},"f:protocol":{},"f:targetPort":{}},"k:{\"port\":9559,\"protocol\":\"TCP\"}":{".":{},"f:name":{},"f:port":{},"f:protocol":{},"f:targetPort":{}}},"f:selector":{},"f:sessionAffinity":{},"f:type":{}}} }]},Spec:ServiceSpec{Ports:[]ServicePort{ServicePort{Name:gribi,Protocol:TCP,Port:9340,TargetPort:{0 57401 },NodePort:31840,AppProtocol:nil,},ServicePort{Name:p4rt,Protocol:TCP,Port:9559,TargetPort:{0 9559 },NodePort:30515,AppProtocol:nil,},ServicePort{Name:ssl,Protocol:TCP,Port:443,TargetPort:{0 443 },NodePort:31196,AppProtocol:nil,},ServicePort{Name:ssh,Protocol:TCP,Port:22,TargetPort:{0 22 },NodePort:30465,AppProtocol:nil,},ServicePort{Name:gnmi,Protocol:TCP,Port:9339,TargetPort:{0 57400 },NodePort:32259,AppProtocol:nil,},},Selector:map[string]string{app: srl,},ClusterIP:10.96.12.69,Type:LoadBalancer,ExternalIPs:[],SessionAffinity:None,LoadBalancerIP:,LoadBalancerSourceRanges:[],ExternalName:,ExternalTrafficPolicy:Cluster,HealthCheckNodePort:0,PublishNotReadyAddresses:false,SessionAffinityConfig:nil,IPFamilyPolicy:*SingleStack,ClusterIPs:[10.96.12.69],IPFamilies:[IPv4],AllocateLoadBalancerNodePorts:*true,LoadBalancerClass:nil,InternalTrafficPolicy:*Cluster,TrafficDistribution:nil,},Status:ServiceStatus{LoadBalancer:LoadBalancerStatus{Ingress:[]LoadBalancerIngress{},},Conditions:[]Condition{},},}
I0320 20:22:16.022848 1216185 topo.go:608] Node "srl" (vendor: "NOKIA", model: "ixrd2") resource created
I0320 20:22:16.022903 1216185 juniper.go:360] Creating cPTX node resource cptx model cptx
I0320 20:22:16.049257 1216185 juniper.go:517] Created cPTX node resource cptx pod model cptx
I0320 20:22:16.098636 1216185 node.go:519] Created Service:
&Service{ObjectMeta:{service-cptx  two-vendors  856d02a2-32df-44d0-bf52-5141d8c400cc 4021573 0 2025-03-20 20:22:16 +0000 UTC <nil> <nil> map[pod:cptx] map[] [] [] [{kne Update v1 2025-03-20 20:22:16 +0000 UTC FieldsV1 {"f:metadata":{"f:labels":{".":{},"f:pod":{}}},"f:spec":{"f:allocateLoadBalancerNodePorts":{},"f:externalTrafficPolicy":{},"f:internalTrafficPolicy":{},"f:ports":{".":{},"k:{\"port\":22,\"protocol\":\"TCP\"}":{".":{},"f:name":{},"f:port":{},"f:protocol":{},"f:targetPort":{}},"k:{\"port\":443,\"protocol\":\"TCP\"}":{".":{},"f:name":{},"f:port":{},"f:protocol":{},"f:targetPort":{}},"k:{\"port\":9339,\"protocol\":\"TCP\"}":{".":{},"f:name":{},"f:port":{},"f:protocol":{},"f:targetPort":{}},"k:{\"port\":9340,\"protocol\":\"TCP\"}":{".":{},"f:name":{},"f:port":{},"f:protocol":{},"f:targetPort":{}},"k:{\"port\":9559,\"protocol\":\"TCP\"}":{".":{},"f:name":{},"f:port":{},"f:protocol":{},"f:targetPort":{}}},"f:selector":{},"f:sessionAffinity":{},"f:type":{}}} }]},Spec:ServiceSpec{Ports:[]ServicePort{ServicePort{Name:gribi,Protocol:TCP,Port:9340,TargetPort:{0 32767 },NodePort:30262,AppProtocol:nil,},ServicePort{Name:p4rt,Protocol:TCP,Port:9559,TargetPort:{0 32767 },NodePort:30948,AppProtocol:nil,},ServicePort{Name:ssl,Protocol:TCP,Port:443,TargetPort:{0 443 },NodePort:32658,AppProtocol:nil,},ServicePort{Name:ssh,Protocol:TCP,Port:22,TargetPort:{0 22 },NodePort:31931,AppProtocol:nil,},ServicePort{Name:gnmi,Protocol:TCP,Port:9339,TargetPort:{0 32767 },NodePort:31423,AppProtocol:nil,},},Selector:map[string]string{app: cptx,},ClusterIP:10.96.31.255,Type:LoadBalancer,ExternalIPs:[],SessionAffinity:None,LoadBalancerIP:,LoadBalancerSourceRanges:[],ExternalName:,ExternalTrafficPolicy:Cluster,HealthCheckNodePort:0,PublishNotReadyAddresses:false,SessionAffinityConfig:nil,IPFamilyPolicy:*SingleStack,ClusterIPs:[10.96.31.255],IPFamilies:[IPv4],AllocateLoadBalancerNodePorts:*true,LoadBalancerClass:nil,InternalTrafficPolicy:*Cluster,TrafficDistribution:nil,},Status:ServiceStatus{LoadBalancer:LoadBalancerStatus{Ingress:[]LoadBalancerIngress{},},Conditions:[]Condition{},},}
I0320 20:22:16.098910 1216185 juniper.go:521] Created cPTX node resource cptx services
I0320 20:22:16.098920 1216185 topo.go:608] Node "cptx" (vendor: "JUNIPER", model: "cptx") resource created
I0320 20:22:16.098996 1216185 juniper.go:200] cptx - generating self signed certs
I0320 20:22:16.099001 1216185 juniper.go:201] cptx - waiting for pod to be running
I0320 20:22:19.351088 1216185 juniper.go:219] cptx - pod running.
```

To delete it: `kne delete 2vendors.pb.txt`

## Interacting with devices

```bash
$ kubectl get pod -n two-vendors
NAME   READY   STATUS    RESTARTS   AGE
cptx   1/1     Running   0          16m
srl    1/1     Running   0          16m
```

```bash
$ kubectl get svc -n two-vendors
NAME           TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)                                                                   AGE
service-cptx   LoadBalancer   10.96.26.144   198.18.0.50   9339:30227/TCP,9340:31074/TCP,9559:30841/TCP,443:32253/TCP,22:30726/TCP   16m
service-srl    LoadBalancer   10.96.62.67    198.18.0.51   443:32514/TCP,22:31194/TCP,9339:32193/TCP,9340:30470/TCP,9559:31675/TCP   16m
```

```bash
$ kubectl get nodes
NAME               STATUS   ROLES    AGE   VERSION
dev-10-34-13-105   Ready    worker   8d    v1.29.12
dev-10-34-29-242   Ready    worker   8d    v1.29.12
dev-10-34-31-43    Ready    worker   8d    v1.29.12
```

(This is only needed in our highly controlled environment without ghcr.io access) First manually change the SR-Linux hardcoded init image with: 

```bash
$ kubectl patch pod srl -n two-vendors -p '{"spec":{ "initContainers": [{"name": "init-srl", "image": "myrepo/kne/srl-labs/init-wait:latest" }]}}'
```

### Nokia

Applying a router config

```bash
$ kne topology push 2vendors.pb.txt srl srlinux.cfg
I0320 20:36:09.446936 1222925 topo.go:174] Trying in-cluster configuration
I0320 20:36:09.447397 1222925 topo.go:177] Falling back to kubeconfig: "/root/.kube/config"
I0320 20:36:09.448257 1222925 topo.go:453] Adding Link: srl:e1-7 cptx:eth12
I0320 20:36:09.448269 1222925 topo.go:453] Adding Link: srl:e1-8 cptx:eth13
I0320 20:36:09.448272 1222925 topo.go:453] Adding Link: srl:e1-9 cptx:eth14
I0320 20:36:09.448849 1222925 topo.go:499] Adding Node: cptx:JUNIPER
I0320 20:36:09.448884 1222925 topo.go:499] Adding Node: srl:NOKIA
I0320 20:36:09.449390 1222925 nokia.go:144] srl - pushing config
I0320 20:36:13.588628 1222925 nokia.go:197] srl - finished pushing config
```

Connecting: `kubectl exec -it srl -n two-vendors -- sr_cli` 

```bash
$ kubectl exec -it srl -n two-vendors -- sr_cli
Defaulted container "srl" out of: srl, init-srl (init)
Using configuration file(s): ['/etc/opt/srlinux/srlinux.rc']
Welcome to the srlinux CLI.
Type 'help' (and press <ENTER>) if you need any help using this.
--{ running }--[  ]--
A:srl# show system lldp neighbor
  +--------------+-------------------+----------------------+---------------------+------------------------+----------------------+---------------+
  |     Name     |     Neighbor      | Neighbor System Name | Neighbor Chassis ID | Neighbor First Message | Neighbor Last Update | Neighbor Port |
  +==============+===================+======================+=====================+========================+======================+===============+
  | ethernet-1/7 | CA:7A:04:4C:05:D5 | cPTX                 | CA:7A:04:4C:05:D5   | 2 minutes ago          | 9 seconds ago        | 523           |
  +--------------+-------------------+----------------------+---------------------+------------------------+----------------------+---------------+
--{ running }--[  ]--
```

```bash
A:srl# info interface ethernet-1/7
    interface ethernet-1/7 {
        description cptx:et-0/0/8
        admin-state enable
        subinterface 0 {
            ipv4 {
                admin-state enable
                address 1.2.3.2/30 {
                }
            }
        }
    }
--{ running }--[  ]--
A:srl#
```

```bash
A:srl# show interface mgmt0.0
=====================================================================================================================================================================================================
  mgmt0.0 is up
    Network-instances:
      * Name: mgmt (ip-vrf)
    Encapsulation   : null
    Type            : None
    IPv4 addr    : 10.244.241.145/32 (dhcp, preferred)
    IPv6 addr    : fe80::e450:67ff:fedd:3726/64 (link-layer, preferred)
=====================================================================================================================================================================================================
--{ running }--[  ]--
A:srl#
```

It won't know how to reach another in-cluster address (management) with configured address/default-route from DHCP (need to fix this). 

Change:

```bash
enter candidate
interface mgmt0 subinterface 0 ipv4 address 10.244.241.145/16
exit
delete dhcp-client
commit now
```

### Juniper

```bash
$ kubectl exec -it cptx -n two-vendors -- cli
Defaulted container "cptx" out of: cptx, init-cptx (init)
root@cPTX> show version 
Hostname: cPTX
Model: ptx10001-36mr
Junos: 23.2R2.11-EVO
Yocto: 3.0.2
Linux Kernel: 5.2.60-yocto-standard-g3c005ea
JUNOS-EVO OS 64-bit [junos-evo-install-ptx-fixed-x86-64-23.2R2.11-EVO]
```

```bash
root@cPTX> show lldp neighbors   

root@cPTX> 
```
```bash
root@cPTX> show arp             

root@cPTX> 
```

```bash
root@cPTX> show interfaces et-0/0/8  
Physical interface: et-0/0/8, Enabled, Physical link is Up
  Interface index: 1024, SNMP ifIndex: 523
  Description: srl:et-0/0/7
  Link-level type: Ethernet, MTU: 1514, LAN-PHY mode, Speed: 100Gbps, BPDU Error: None, Loop Detect PDU Error: None, Ethernet-Switching Error: None, MAC-REWRITE Error: None, Loopback: Disabled,
  Source filtering: Disabled, Flow control: Enabled, Auto-negotiation: Disabled, Media type: Fiber
  Device flags   : Present Running
  Interface flags: SNMP-Traps
  CoS queues     : 8 supported, 8 maximum usable queues
  Current address: ca:7a:04:4c:00:4f, Hardware address: ca:7a:04:4c:00:4f
  Last flapped   : 2025-03-21 15:58:01 UTC (00:17:11 ago)
  Input rate     : 112 bps (0 pps)      
  Output rate    : 208 bps (1 pps)
  Active alarms  : None
  Active defects : None
  PCS statistics                      Seconds
    Bit errors                             0
    Errored blocks                         0
  PRBS Mode : Disabled
  Interface transmit statistics: Disabled
  Link Degrade :                      
    Link Monitoring                   :  Disable

  Logical interface et-0/0/8.0 (Index 1003) (SNMP ifIndex 524)
    Flags: Up SNMP-Traps Encapsulation: ENET2 DF
    Input packets : 74
    Output packets: 457
    Protocol inet
    Max nh cache: 100000, New hold nh limit: 100000, Curr nh cnt: 0, Curr new hold cnt: 0, NH drop cnt: 0
    MTU: 1500
      Flags: Sendbcast-pkt-to-re, Is-Primary
      Addresses, Flags: Is-Preferred Is-Primary
        Destination: 1.2.3.0/30, Local: 1.2.3.1, Broadcast: 1.2.3.3
    Protocol iso, MTU: 1497
      Flags: Is-Primary
    Protocol multiservice, MTU: Unlimited
      Flags: Is-Primary
```

#### No data-plane connectivity

`et-0/0/8 `has SNMP ifIndex: `523`. LLDP neighbor visible from SR Linux.

```bash
root@cPTX> show configuration interfaces et-0/0/8
description srl:et-0/0/7;
speed 100g;
unit 0 {
    family inet {
        address 1.2.3.1/30;
    }
    family iso;
}
```

Can't ping it

```bash
root@cPTX> ping 1.2.3.2 
PING 1.2.3.2 (1.2.3.2) 56(84) bytes of data.
^C
--- 1.2.3.2 ping statistics ---
4 packets transmitted, 0 received, 100% packet loss, time 3071ms
```

```bash
root@cPTX> show interfaces re0:mgmt-0.0          
  Logical interface re0:mgmt-0.0 (Index 1000) (SNMP ifIndex 194)
    Flags: Up Encapsulation: ENET2 DF
    Input packets : 10
    Output packets: 63
    Protocol inet
    Max nh cache: 0, New hold nh limit: 0, Curr nh cnt: 0, Curr new hold cnt: 0, NH drop cnt: 0
    MTU: 1486
      Flags: Sendbcast-pkt-to-re
      Addresses, Flags: Master-only Is-Preferred Is-Primary
        Local: 10.244.43.155
```

```bash
root@cPTX> show route forwarding-table family inet table default    
Routing table: default.inet
Internet:
Destination        Type RtRef Next hop           Type Index    NhRef Netif
default            perm     0                    rjct       36     1
0.0.0.0/32         perm     0                    dscd       34     1
1.2.3.0/30         intf     0                    rslv     5005     1 et-0/0/8.0
1.2.3.0/32         dest     0                    recv     5007     1 et-0/0/8.0
1.2.3.1/32         dest     0                    locl     5008     1 .local..0
1.2.3.3/32         dest     0                    bcst     5006     1 et-0/0/8.0
2.2.2.2/32         dest     0                    locl     5002     1 .local..0
10.244.43.155/32   dest     0                    locl     5001     1 .local..0
224.0.0.0/4        perm     0                    mdsc       35     1
224.0.0.1/32       perm     0                    mcst       31     1
255.255.255.255/32 perm     0                    bcst       32     1
```

If we configured `set system management-instance`

```bash
root@cPTX> show route forwarding-table table mgmt_junos family inet 
Routing table: mgmt_junos.inet
Internet:
Destination        Type RtRef Next hop           Type Index    NhRef Netif
default            perm     0                    rjct      616     1
0.0.0.0/32         perm     0                    dscd      614     1
10.244.43.155/32   dest     0                    locl     5011     1 .local..6
224.0.0.0/4        perm     0                    mdsc      615     1
224.0.0.1/32       perm     0                    mcst      611     1
255.255.255.255/32 perm     0                    bcst      612     1
```

Can ping to itself, but it wouldn't know how to reach another in-cluster address (management) with configured address/default-route from DHCP (need to fix this). 

```bash
root@cPTX> ping 10.244.241.145 routing-instance mgmt_junos  
/bin/ping: connect: No route to host
```

Change:

```bash
delete interfaces re0:mgmt-0 unit 0 family inet address 10.244.43.155/32   
set interfaces re0:mgmt-0 unit 0 family inet address 10.244.43.155/16 
```

#### Sporadic management connectivity 

After this, it works for a few seconds (9 pings go through, then it stops)

```bash
root@cPTX> ping 10.244.241.145 routing-instance mgmt_junos    
PING 10.244.241.145 (10.244.241.145) 56(84) bytes of data.
64 bytes from 10.244.241.145: icmp_seq=1 ttl=62 time=95.3 ms
64 bytes from 10.244.241.145: icmp_seq=2 ttl=62 time=2.22 ms
64 bytes from 10.244.241.145: icmp_seq=3 ttl=62 time=2.27 ms
64 bytes from 10.244.241.145: icmp_seq=4 ttl=62 time=2.14 ms
64 bytes from 10.244.241.145: icmp_seq=5 ttl=62 time=1.99 ms
64 bytes from 10.244.241.145: icmp_seq=6 ttl=62 time=1.91 ms
64 bytes from 10.244.241.145: icmp_seq=7 ttl=62 time=2.17 ms
64 bytes from 10.244.241.145: icmp_seq=8 ttl=62 time=2.21 ms
64 bytes from 10.244.241.145: icmp_seq=9 ttl=62 time=2.13 ms
^C
--- 10.244.241.145 ping statistics ---
30 packets transmitted, 9 received, 70% packet loss, time 29532ms
rtt min/avg/max/mdev = 1.911/12.485/95.329/29.289 ms
```
