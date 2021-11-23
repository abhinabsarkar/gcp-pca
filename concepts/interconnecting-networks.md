# Interconnecting Networks
* [Interconnecting Networks - slides](slides/InterconnectingNetworks.pdf)

## Cloud VPN
Cloud VPN securely connects your on-premises network to your GCP VPC network through an [IPsec](https://en.wikipedia.org/wiki/IPsec) [VPN](https://en.wikipedia.org/wiki/Virtual_private_network) [tunnel](https://en.wikipedia.org/wiki/Tunneling_protocol). Traffic traveling between the two networks is encrypted by one VPN gateway, then decrypted by the other VPN gateway. This protects your data as it travels over the public internet, and that’s why Cloud VPN is useful for low-volume data connections.
* Supports Site-to-Site VPN
* Static & Dynamic routes
* IKEv1 & IKEv2 ciphers
* SLA 99.9%

[Cloud VPN - Official documentation](https://cloud.google.com/vpn/docs/concepts/overview)

![alt text](/images/vpn-topology.png)

> In order to create a connection between two VPN gateways, you must establish two VPN tunnels. Each tunnel defines the connection from the perspective of its gateway, and traffic can only pass when the pair of tunnels is established.

When using Cloud VPN is that the maximum transmission unit, or MTU, for your on-premises VPN gateway cannot be greater than 1460 bytes. This is because of the encryption and encapsulation of packets. For more information about this MTU consideration, see the link - https://cloud.google.com/vpn/docs/concepts/mtu-considerations

### Dynamic routing with Cloud Router

![alt text](/images/dynamic-routing-cloud-router.png)

Cloud Router can manage routes for a Cloud VPN tunnel using Border Gateway Protocol, or BGP. This routing method allows for routes to be updated and exchanged without changing the
tunnel configuration.

For example, this diagram shows two different regional subnets in a VPC network, namely Test and Prod. The on-premises network has 29 subnets, and the two networks are connected through Cloud VPN tunnels. Now, how would you handle adding new subnets? For example, how would you add a new Staging” subnet in the GCP network and a new on-premises 10.0.30.0/24 subnet to handle growing traffic in
your data center? 

To automatically propagate network configuration changes, the VPN tunnel uses Cloud Router to establish a BGP session between the VPC and the on-premises VPN gateway, which must support BGP. The new subnets are then seamlessly advertised between networks. This means that instances in the new subnets can start sending and receiving traffic immediately.

To set up BGP, an additional IP address has to be assigned to each end of the VPN tunnel. These two IP addresses must be [link-local IP addresses](https://datatracker.ietf.org/doc/html/rfc3927), belonging to the IP address range 169.254.0.0/16. These addresses are not part of IP address space of either network and are used exclusively for establishing a BGP session. In this example, the IP address assigned are 169.254.1.1 & 169.254.1.2.

> 169.254.0.0/16 IP Addresses - When a host fails to dynamically acquire an address, it can optionally assign itself a link-local IPv4 address in accordance with RFC 3927

## HA VPN 
HA VPN is a high availability Cloud VPN solution that lets you securely connect your on-premises network to your Virtual Private Cloud (VPC) network through an IPsec VPN connection in a single region. HA VPN provides an SLA of 99.99% service availability.

## Cloud Interconnect

### Dedicated Interconnect
Dedicated Interconnect provides direct physical connections between your on-premises network and Google’s network. This enables you to transfer large amounts of data between networks, which can be more cost-effective than purchasing additional bandwidth over the public internet.

![alt text](/images/dedicated-interconnect.png)

In order to use Dedicated Interconnect, you need to provision a cross connect between the Google network and your own router in a common colocation facility, as shown in this diagram. To exchange routes between the  networks, you configure a BGP session over the interconnect between the Cloud Router and the on-premises router. This will allow user traffic from the on-premises network to reach GCP resources on the VPC network, and vice versa.

Dedicated Interconnect can be configured to offer a 99.9% or a 99.99% uptime SLA.

### Partner Interconnect
Partner Interconnect provides connectivity between your on-premises network and your VPC network through a supported service provider. This is useful if your data center is in a physical location that cannot reach a Dedicated Interconnect colocation facility or if your data needs don't warrant a Dedicated Interconnect.

![alt text](/images/partner-interconnect.png)

In order to use Partner Interconnect, you work with a supported service provider to connect your VPC and on-premises networks. These service providers have existing physical connections to Google's network that they make available for their customers to use. After you establish connectivity with a service provider, you can request a Partner Interconnect connection from your service provider. Then, you establish a BGP session between your Cloud Router and on-premises router to start passing traffic between your networks via the service
provider's network.

Partner Interconnect can be configured to offer a 99.9% or a 99.99% uptime SLA between Google and the service provider.

## Interconnect options comparison

![alt text](/images/interconnect-comparison.png)

## Cloud Peering
Direct Peering and Carrier Peering services are useful when you require access to Google and Google Cloud properties.

### Direct Peering

Google allows you to establish a Direct Peering connection between your business network and Google’s. With this connection you will be able to exchange internet traffic between your network and Google’s at one of Google’s broad-reaching edge network locations.

Direct Peering with Google is done by exchanging BGP routes between Google and the peering entity. After a Direct Peering connection is in place, you can use it to reach all of Google’s services, including the full suite of Google Cloud Platform products. Unlike Dedicated Interconnect, Direct Peering does not have an SLA.

GCP’s Edge Points of Presence, or PoPs, are where Google's network connects to the rest of the internet via peering. PoPs are present on over 90 internet exchanges and at over 100 interconnection facilities around the world.

### Career Peering
If you are nowhere closer to these locations, then you can use Carrier Peering. Just like Direct Peering, Carrier Peering does not have an SLA.

### Cloud peering options comparison
![alt text](/images/carrier-peering-comparison.png)

## GCP network connectivity options summary
![alt text](/images/gcp-network-connectivity-options.png)

### Choosing a network connection option

![alt text](/images/choosing-network-connectivity-options.png)

### Network connectivity decision tree

![alt text](/images/network-connectivity-decision-tree.png)

## Shared VPC Networks
Shared VPC allows an organization to connect resources from multiple projects to a common VPC network. This allows the resources to communicate with each other securely and efficiently using internal IPs from that network.

When you use shared VPC, you designate a project as a host project and attach one or more other service projects to it. In this case, the Web Application Server’s project is the host project, and the three other projects are the service projects. The overall VPC network is called the shared VPC network.

## VPC Peering
VPC Network Peering, in contrast, allows private RFC 1918 connectivity across two VPC networks, regardless of whether they belong to the same project or the same organization. Now, remember that each VPC network will have firewall rules that define what traffic is allowed or denied between the networks.

VPC Network Peering is a decentralized or distributed approach to multi-project networking, because each VPC network may remain under the control of separate administrator groups and maintains its own global firewall and routing tables. Historically, such projects would consider external IP addresses or VPNs to facilitate private communication between VPC networks. However, VPC Network Peering does not incur the network latency, security, and cost drawbacks that are present when using external IP addresses or VPNs.

![alt text](/images/shared-vpc-vs-peered-vpc.png)

The biggest difference between the two configurations is the network administration models. Shared VPC is a centralized approach to multi-project networking, because security and network policy occurs in a single designated VPC network. In contrast, VPC Network Peering is a decentralized approach, because each VPC network can remain under the control of separate administrator groups and maintains its own global firewall and routing tables.