# Resource Management & Monitoring
* [Resource Manager - Official documentation](https://cloud.google.com/resource-manager/docs)
* [Cloud Operations Suite (formerly Stackdriver)](https://cloud.google.com/products/operations)
* [Resource Management - slides](slides/ResourceManagement.pdf)
* [Resource Monitoring - slides](slides/ResourceMonitoring.pdf)

## Resource Manager
* Allows to manage resources hierarchically

![alt text](/images/resource-management-hoerarchy.png)

Project accumulates the consumption of all its resources. It tracks resources & quota usage.

![alt text](/images/resource-hierarchy2.png)

### Quotas
* All projects are subject to project quotas or limits.
* Prevent runaway consumption in case of error or malicious attack.
* Prevent billing spikes.
* Forces sizing considerations & periodic review.

### Labels & Names
* Labels are key value pair that are attached to resources like VM, disk for organizing GCP resources
* Example of labels could be to give environment name, cost center, department, etc

### Labels Vs Tags
| Label |  Tag  |
| :-    |   :-  |
|User-defined strings in key-value format for organizing resources. They are used for billing. | Primarily used for networking to apply firewall rules |

### Budget
A budget enables you to track your actual Google Cloud spend against your planned spend. After you've set a budget amount, you set budget alert threshold rules that are used to trigger email notifications. Budget alert emails help you stay informed about how your spend is tracking against your budget. You can also use budgets to automate cost control responses.

* [Budget - Official documentation](https://cloud.google.com/billing/docs/how-to/budgets)
![alt text](/images/budget.png)

### Data Studio
You can get up-to-date Cloud Billing graphs throughout the day, and use labels to slice and dice your Google Cloud bill the way you want by combining Cloud Billing data export to BigQuery functionality with Google Data Studio.

* [Data Studio - Official documentation](https://cloud.google.com/billing/docs/how-to/visualize-data)

![alt text](/images/google-data-studio.png)

## Cloud Operations Suite (previously Stackdriver)
* Integrated monitoring, logging & diagnostics
* Manages across platforms (GCP & AWS)

![alt text](/images/cloud-operations-products.png)

* [Cloud Operations Suite (formerly Stackdriver)](https://cloud.google.com/products/operations)

### Cloud Monitoring
Cloud Monitoring collects metrics, events, and metadata from Google Cloud, Amazon Web Services (AWS), hosted uptime probes, and application instrumentation. Google Cloud's operations suite ingests that data and generates insights via dashboards, charts, and alerts. BindPlane is included with your Google Cloud project at no additional cost. Using the BindPlane service, you can also collect this data from over 150 common application components, on-premise systems, and hybrid cloud systems.

* [Cloud Monitoring - Official documentation](https://cloud.google.com/monitoring/docs)

![alt text](/images/cloud-monitoring.png)

Workspace is a single pane of glass.

### Cloud Logging
* Logging can be done for platform, system & application
* Logs are retained for 30 days
* Logs can be exported to Cloud Storage, BigQuery & Pub/Sub
* Analyze logs in BigQuery & visualize in Data Studio
* Logging agent is available to install on GCP Compute Engine & AWS EC2 instances

### Error Reporting
Aggregate & display errors for running cloud services

### Tracing
* Displays data in real time
* Latency reporting
* Collects latency data on App Engine, GCP HTTP(s) load balancers, etc

### Debugging
* Inspect an application in real time (i.e. no stopping or significant slowing down of app < 10 ms latency)

