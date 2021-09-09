# Compute Engine
* [Compute Engine - Official documentation](https://cloud.google.com/compute/docs)
* [Virtual Machines - slides](slides/VirtualMachines.pdf)

Compute Engine is a computing and hosting service that lets you create and run virtual machines on Google infrastructure. Compute Engine offers scale, performance, and value that lets you easily launch large compute clusters on Google's infrastructure.

![Alt text](/images/compute-engine.png)

1 vCPU = 1 HyperThread 
> Hyper-Threading is Intel’s term for simultaneous multithreading (SMT). This is a process where a CPU splits each of its physical cores into virtual cores, which are known as threads. For example, most of Intel's CPUs with two cores use hyper-threading to provide four threads. Hyper-Threading allows each core to do two things simultaneously. It increases CPU performance by improving the processor’s efficiency, thereby allowing you to run multiple demanding apps at the same time or use heavily-threaded apps without the PC lagging.

![Alt text](/images/compute-engine-features.png)

![Alt text](/images/compute-engine-storage.png)

![Alt text](/images/compute-engine-networking.png)

![Alt text](/images/compute-engine-lifecycle.png)

## Preemptible VM
A preemptible VM is an instance that you can create and run at a much lower price than normal instances. However, Compute Engine might stop (preempt) these instances if it requires access to those resources for other tasks. Preemptible instances are excess Compute Engine capacity, so their availability varies with usage.
* [Preemptible VM instances - Official documentation](https://cloud.google.com/compute/docs/instances/preemptible)

## Sole-tenant node
Sole-tenancy lets you have exclusive access to a sole-tenant node, which is a physical Compute Engine server that is dedicated to hosting only your project's VMs. Use sole-tenant nodes to keep your VMs physically separated from VMs in other projects, or to group your VMs together on the same host hardware.
* [Sole tenant node - official documentation](https://cloud.google.com/compute/docs/nodes/sole-tenant-nodes)

![Alt text](/images/sole-tenant-node.png)

## Shielded VMs
* [Shielded VMs - official documentation](https://cloud.google.com/security/shielded-cloud/shielded-vm)

## Disk options
![Alt text](/images/compute-engine-disks.png)

## Snapshots
* [Snapshots - Official documentation](https://cloud.google.com/compute/docs/disks/snapshots)
* [Snapshots - blog](https://jayendrapatil.com/google-cloud-compute-engine-snapshots/)