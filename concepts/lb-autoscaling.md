# Load Balancing & Autoscaling
* [Load Balancing and Autoscaling - slides](slides/LoadBalancingandAutoscaling.pdf)

## Global and regional load balancers
GCP offers different types of load balancers that can be divided into two categories: 
* Global
* Regional

[Google Cloud load balancers](https://cloud.google.com/load-balancing/docs/load-balancing-overview#summary-of-google-cloud-load-balancers)

[Load balancer decision tree](https://cloud.google.com/load-balancing/docs/load-balancing-overview#choosing_a_load_balancer)

[Underlying technology of Google Cloud load balancers](https://cloud.google.com/load-balancing/docs/load-balancing-overview#tech-load-balancing)

## Managed Instance Groups
* Deploy identical instances based on instance template
* Instance group can be resized
* Manager ensures all instances are RUNNING
* Typically used with autoscaler
* Can be single zone or regional
    * Regional managed instance groups are generally recommended over zonal managed instance groups because they allow you to spread the application load across multiple zones instead of confining your application to a single zone or you having to manage multiple instance groups across different zones. This replication protects against zonal failures and unforeseen scenarios where an entire group of instances in a single zone malfunctions. If that happens, your application can continue serving traffic from instances running in another zone of the same region.

> Managed instance group is like creating virtual machines, but you're applying more rules to that instance group.

![alt text](/images/mi-autoscaling.png)

## HTTP(S) load balancing
* Global load balancing
* Anycast IP address
* HTTP or port 80 or 8080
* HTTPs on port 443
* IPv4 or IPv6
* Autoscaling
* URL maps
    * URL based routing (content based like url/audio & url/video). Routing based on user location, serving capacity, zone, and instance health

![alt text](/images/architecture-https-lb.png)

The backends themselves contain an instance group, a balancing mode and a capacity scaler.
* An instance group contains virtual machine instances. The instance group may be a managed instance group with or without autoscaling or an unmanaged instance group.
* A balancing mode tells the load balancing system how to determine when the backend is at full usage. If all the backends for the backend service in a region are at full usage, new requests are automatically routed to the nearest region that can still handle requests. The balancing mode can be based on CPU utilization or requests per second (RPS).
* A capacity setting is an additional control that interacts with the balancing mode setting. For example, if you normally want your instances to operate at a maximum of 80% CPU utilization, you would set your balancing mode to 80% CPU utilization and your capacity to 100%. If you want to cut instance utilization in half, you could leave the balancing mode at 80% CPU utilization and set capacity to 50%.

An HTTP(S) load balancer has the same basic structure as an HTTP load balancer, but differs in the following ways:
* An HTTP(S) load balancer uses a target HTTPS proxy instead of a target HTTP proxy.
* An HTTP(S) load balancer requires at least one signed SSL certificate installed on the target HTTPS proxy for the load balancer.
* The client SSL session terminates at the load balancer.
* HTTP(S) load balancers support the QUIC transport layer protocol.
    * [QUIC](https://www.chromium.org/quic) is a transport layer protocol that allows faster client connection initiation, eliminates head-of-line blocking in multiplexed streams, and supports connection migration when a client's IP address changes.

## Network endpoint groups
A network endpoint group (NEG) is a configuration object that specifies a group of backend endpoints or services. A common use case for this configuration is deploying services in containers.

[NEG - Official documentation](https://cloud.google.com/load-balancing/docs/negs)

## SSL proxy load balancing
* Global load balancing for encrypted, non-HTTP traffic
* Terminates SSL session at load balancing layer
*  IPv4 or IPv6 clients
* Benefits:
    * Intelligent routing - load balances can route requests to backend locations where there is capacity.
    * Certificate management
    * Security patching -  GCP will apply patches at the load balancer automatically in order to keep instances safe
    * SSL policies

### Network diagram illustrates SSL proxy load balancing
![alt text](/images/ssl-proxy-lb.png)

> The user in Boston would reach the us east region, and the user in Iowa would
reach the us central region, if there is enough capacity.

## TCP proxy load balancing
* Global load balancing for unencrypted, non-HTTP traffic
* Terminates TCP sessions at load balancing layer
* IPv4 or IPv6 clients
* Benefits:
    * Intelligent routing
    * Security patching

![alt text](/images/tcp-proxy-lb.png)

> The user in Boston would reach the us east region, and the user in Iowa would
reach the us central region, if there is enough capacity.

## Network load balancing
* Regional, non-proxied load balancer
    * All traffic is passed through the load balancer, instead of being proxied, and traffic can only be balanced between VM instances that are in the same region, unlike a global load balancer
* Forwarding rules (IP protocol data)
* Traffic:
    * UDP
    * TCP/SSL ports
* Architecture:
    * Backend service-based
    * Target pool-based

## Internal TCP/UDP load balancing
* Regional, private load balancing
    * VM instances in same region
    * RFC 1918 IP addresses
* TCP/UDP traffic
* Reduced latency, simpler configuration
* Software-defined, fully distributed load balancing

Google Cloud internal load balancing is not based on a device or a VM instance. Instead, it is a software-defined, fully distributed load balancing solution.

In the traditional proxy model of internal load balancing, as shown on the left, you configure an internal IP address on a load balancing device or instances, and your client instance connects to this IP address. Traffic coming to the IP address is terminated at the load balancer, and the load balancer selects a backend to establish a new connection to. Essentially, there are two connections: one between the Client and the Load Balancer, and one between the Load Balancer and the Backend.

Google Cloud internal load balancing distributes client instance requests to the backends using a different approach, as shown on the right. It uses lightweight load balancing built on top of Andromeda (Google’s network virtualization stack) to provide software-defined load balancing that directly delivers the traffic from the client instance to a backend instance.

![alt text](/images/benefit-slb-internal.png)

## Internal HTTP(S) load balancing
* Regional Layer 7, private load balancing
    * VM instances in same region
    * RFC 1918 IP addresses
* HTTP, HTTPS, or HTTP/2 protocols
* Based on open source Envoy proxy