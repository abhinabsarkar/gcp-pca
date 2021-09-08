# Virtual Private Cloud (VPC)
* [Virtual Private Cloud (VPC) - Official documentation](https://cloud.google.com/vpc/docs)

Google Cloud Virtual Private Cloud (VPC) provides networking functionality to Compute Engine virtual machine (VM) instances, Google Kubernetes Engine (GKE) containers, and the App Engine flexible environment. VPC provides networking for your cloud-based services that is global, scalable, and flexible.

![alt text](/images/vpc-basics.png)

> The traffic between VMs C & D isn't actually touching the public internet, but they go through the GCP edge routers. This has different billing & security ramifications as compared to the VMs A & B which are internal.

VPN gateway to connect to peered VPC from on-premises.
![alt text](/images/vpc-network-vpn-gwy.png)

VMs on the same subnet can be in different zones. The first 2 IP addresses in the range (.0 & .1) are reserved for Network & Subnet Gateways respectively. The last 2 IP addresses are also reserved where the last IP address is the broadcast address.

![alt text](/images/vpc-subnetwork-cross-zones.png)

The VPCs allow to increase the IP address space of a subnet   without shutting down any instances or downtime.

![alt text](/images/vpc-subnet-expansion.png)

The external IP address of a VM is mapped to the internal IP address of the VM. The VM itself is not aware of the external IP address.
![alt text](/images/vm-ext-ip-mapping-internal-ip.png)

Compute Engine instances receive internal DNS resolution information as part of their DHCP leases. By default, the instance's metadata server (169.254.169.254) resolves internal DNS names. The FQDN for the VMs are "VM_NAME.ZONE.c.PROJECT_ID.internal". The metadata server handles the ns queries for all the local resources & routes all other queries to Google's Public DNS servers for public name resolution.

Google Cloud alias IP ranges let you assign ranges of internal IP addresses as aliases to a virtual machine's (VM) network interfaces. This is useful if you have multiple services running on a VM and you want to assign each service a different IP address. Alias IP ranges also work with GKE Pods. Refer [Alias IP Range](https://cloud.google.com/vpc/docs/alias-ip)

![Alt text](/images/alias-ip-range.png)

## IP address for default domains
Google publishes the complete list of IP ranges that it announces to the internet in goog.json.
Google also publishes a list of Google Cloud customer-usable global and regional external IP
addresses ranges in cloud.json.
* https://www.gstatic.com/ipranges/cloud.json provides a JSON representation of Cloud
IP addresses organized by region.
* https://www.gstatic.com/ipranges/cloud_geofeed is a standard geofeed formatted IP
geolocation file that we share with 3rd-party IP geo providers like Maxmind, Neustar, and
IP2Location.
* https://www.gstatic.com/ipranges/goog.json and https://www.gstatic.com/ipranges/goog.txt are TXT and JSON formatted files
respectively that include Google public prefixes in CIDR notation.

For more information as well as an example of how to use this information, refer to
https://cloud.google.com/vpc/docs/configure-private-google-access#ip-addr-defaults

## Routes & Firewall rules
**Route** - A route is a mapping of an IP range to a destination.
* Apply to traffic egressing a VM
* Forward traffic to most specific route
* Are created when a subnet is created
* Enable VMs on the same network to communicate
* Destination is in CIDR notation
* Traffic is delivered only if it also matches a firewall rule

**Firewall** - Firewall rules protect compute instances from unapproved connections.
* VPC network functions as a distributed firewall
* Firewall rules are applied to the network as a whole
* Connections are allowed or denied at the instance level
* Implied is deny all ingress & allow all egress
* Firewall rules are stateful i.e. they allow bi-directional communication once the session is established

## Cloud NAT
Cloud NAT configures the [Andromeda software](https://cloudplatform.googleblog.com/2014/04/enter-andromeda-zone-google-cloud-platforms-latest-networking-stack.html) that powers Virtual Private Cloud (VPC) network so that it provides source network address translation (source NAT or SNAT) for VMs without external IP addresses. Cloud NAT also provides destination network address translation (destination NAT or DNAT) for established inbound response packets.
* [Cloud NAT documentation](https://cloud.google.com/nat/docs/overview)

![Alt text](/images/cloud-nat.png)

## Private Google Access
Private Google Access permits access to Cloud and Developer APIs and most Google Cloud services, except for the following services:
* App Engine Memcache
* Filestore
* Memorystore

Instead, *Private Services Access* might support one or more of them.
* [Private Google Access documentation](https://cloud.google.com/vpc/docs/private-google-access)

![Alt text](/images/private-google-access.png)

## Private Services Access
Google and third parties (together known as service producers) can offer services with internal IP addresses that are hosted in a VPC network. Private services access enables you to reach those internal IP addresses. This is useful if you want your VM instances in your VPC network to use internal IP addresses instead of external IP addresses.

* [Private Services Access documentation](https://cloud.google.com/vpc/docs/private-services-access#private-services-supported-services)

## Identity-Aware Proxy (IAP) for TCP forwarding
IAP TCP forwarding allows you to establish an encrypted tunnel over which you can forward SSH, RDP, and other traffic to VM instances. IAP TCP forwarding also provides you fine-grained control over which users are allowed to establish tunnels and which VM instances users are allowed to connect to.
* [IAP for TCP forwarding](https://cloud.google.com/iap/docs/using-tcp-forwarding)
* [TCP forwarding overview](https://cloud.google.com/iap/docs/tcp-forwarding-overview)

When instances do not have external IP addresses, they can only be reached by other instances on the network via a managed VPN gateway or via a Cloud IAP tunnel. Cloud IAP enables context-aware access to VMs via SSH and RDP without bastion hosts. 
* [Cloud IAP enables context-aware access to VMs via SSH and RDP without bastion hosts](https://cloud.google.com/blog/products/identity-security/cloud-iap-enables-context-aware-access-to-vms-via-ssh-and-rdp-without-bastion-hosts)