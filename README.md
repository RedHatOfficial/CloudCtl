# CloudCTL
DevOps Services & Utilities Container [Pod] Infrastructure as Code [(IaC)] toolkit.
## Problem
Infrastructure deployment requires a wide set of user enablement & automation 
utilities, network services, and Infrastructure as Code resources. These can 
become difficult & inconsistent to architecht, maintain, and support.
## Solution
CloudCtl provides a [Podman] "Point of Origin" [Pod] to augment infrastructure
services with [UBI8] containerized, and [(IaC)] driven deployment dependencies.

#### Core Features
  - Extensable
  - Portable 
  - Stateful
  - Dynamic
  - Consistent
  - Zero external dependencies (No yum/dnf rpm downloads)
  - Airgap Supported (with Koffer collector)
  - Behaves the same whether connected or disconnected from the internet
#### Supported Distributions
  - [RedHat] 8.0+
  - [RedHat CoreOS] 4.7+
#### Supported Services
  - Konductor User Space Container
  - Ansible Runner Service API
  - Docker Image Registry
  - Nginx File Server
  - Haproxy TCP Load Balancer
  - CoreDNS
  - ISC-DHCP
  - Tftpd
----------------------
## Getting Started
>  Run as User: `root`
>    
    
####  0. Install Dependencies
```
On RHEL 8+ / CentOS 8+ / Fedora 32+
  dnf install -y podman

On Ubuntu 20.04+
  apt install -y podman
```
####  1. Clone Repo
```
 mkdir -p /root/cloudctl && \
 podman run -it --rm --volume /root/cloudctl:/clone:z \
   quay.io/cloudctl/git \
 https://github.com/CloudCtl/CloudCtl.git && \
 cd ~/cloudctl
```
####  2. Initialize CloudCtl Pod
```
./init.sh
```
####  3. Exec into Konductor Userspace
```
podman exec -it konductor connect
```
####  4. Verify API Access
```
curl -ks \
    --key  /root/platform/secrets/cloudctl/certs/ssl/server/cloudctl.pem \
    --cert /root/platform/secrets/cloudctl/certs/ssl/server/cloudctl.crt \
  https://localhost:5001/api/v1/playbooks -X GET
```
>
>  sample output:
>
```
{"status": "OK", "msg": "0 playbook found", "data": {"playbooks": []}}
```
####  5. SSH to Konductor over port 2222
```
ssh -p 2222 root@10.88.0.1
```
####  6. Review pod status
```
sudo podman pod ps
sudo podman ps --all
```
>
>  sample output:
>
```
36668681f233  Up 18 hours ago    coredns
596dbaa4520b  Up 18 hours ago    haproxy
44d434359e33  Up 18 hours ago    nginx
1efcd0fafb40  Up 18 hours ago    registry
3598bab7c7ed  Up 18 hours ago    runner
11305c527abe  Up 18 hours ago    konductor
```
----------------------------------------------------------------------------------
####  Cleanup
```
podman pod rm --force cloudctl
podman image prune --all
rm -rf /root/{cloudctl,platform} /tmp/konductor
```
[Pod]:https://kubernetes.io/docs/concepts/workloads/pods/pod
[UBI8]:https://www.redhat.com/en/blog/introducing-red-hat-universal-base-image
[(IaC)]:https://www.ibm.com/cloud/learn/infrastructure-as-code
[Konductor]:https://github.com/redshiftofficial/Konductor
[Podman]:https://docs.podman.io/en/latest
[Install Podman]:https://podman.io/getting-started/installation
[Fedora]:https://getfedora.org
[Ubuntu]:https://ubuntu.com/download
[CentOS]:https://www.centos.org/download
[RedHat]:https://access.redhat.com/downloads
[Fedora CoreOS]:https://getfedora.org/en/coreos?stream=stable
[RedHat CoreOS]:https://coreos.com/
