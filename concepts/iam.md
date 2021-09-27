# Cloud IAM (Identity & Access Management)
* [Cloud IAM - Official documentation](https://cloud.google.com/iam/docs/overview)
* [Cloud IAM - slides](slides/CloudIAM.pdf)

With IAM, you manage access control by defining who (identity) has what access (role) for which resource. 

![Alt text](/images/iam.png)

## Resource hierarchy & Access Control
Each policy contains a set of roles and role members, with resources inheriting
policies from their parent. Think of it like this: resource policies are a union of parent
and resource, where a less restrictive parent policy will always override a more
restrictive resource policy.

* [Resource hierarchy - Official documentation](https://cloud.google.com/resource-manager/docs/cloud-platform-resource-hierarchy)
* [Resource hierarchy for access control - Official documentation](https://cloud.google.com/iam/docs/resource-hierarchy-access-control)

![Alt text](/images/iam-resource-hierarchy.png)

## Organization
* [Organization management - Official documentation](https://cloud.google.com/resource-manager/docs/creating-managing-organization)

![Alt Text](/images/organization.png)

## Folders
* [Folder management - Official documentation](https://cloud.google.com/resource-manager/docs/creating-managing-folders)

![Alt Text](/images/folders.png)

## Access control roles
* [Access control for Organization - Official documentation](https://cloud.google.com/resource-manager/docs/access-control-org)
* [Access control for Folders - Official documentation](https://cloud.google.com/resource-manager/docs/access-control-folders)

![Alt Text](/images/basic-rm-roles.png)

## Roles
* [Roles - Official documentation](https://cloud.google.com/iam/docs/understanding-roles)

There are three types of roles in IAM:
### Basic roles
Include the Owner, Editor, and Viewer roles that existed prior to the introduction of IAM.

![Alt Text](/images/iam-basic-roles.png)
### Predefined roles
Provide granular access for a specific service and are managed by Google Cloud.

![Alt Text](/images/predefined-roles.png)

### Custom roles
Provide granular access according to a user-specified list of permissions.

![Alt Text](/images/custom-roles.png)

## Members
A member can be a Google Account (for end users), a service account (for apps and virtual machines), a Google group, or a Google Workspace or Cloud Identity domain that can access a resource. The identity of a member is an email address associated with a user, service account, or Google group; or a domain name associated with Google Workspace or Cloud Identity domains.

**Permission management in IAM**

![Alt Text](/images/members.png)

## Google Cloud Directory Sync

![Alt Text](/images/cloud-directory-sync.png)

## Single sign-on (SSO)

![Alt Text](/images/sso.png)

## Service Account
Applications use service accounts to make authorized API calls, authorized as either the service account itself, or as Google Workspace or Cloud Identity users through domain-wide delegation.

A service account is identified by its email address, which is unique to the account.

Service accounts differ from user accounts in a few key ways:
* Service accounts do not have passwords, and cannot log in via browsers or cookies.
* Service accounts are associated with private/public RSA key-pairs that are used for authentication to Google.
* You can let other users or service accounts impersonate a service account.
* Service accounts are not members of your Google Workspace domain, unlike user accounts.

Types of service accounts:
* Default service accounts - When you enable or use some Google Cloud services, they create user-managed service accounts that enable the service to deploy jobs that access other Google Cloud resources. These accounts are known as default service accounts.
* User-managed service accounts - You can create user-managed service accounts in your project using the IAM API, the Cloud Console, or the gcloud command-line tool. You are responsible for managing and securing these accounts.
* Google-managed service accounts - Some Google Cloud services need access to your resources so that they can act on your behalf. For example, when you use Cloud Run to run a container, the service needs access to any Pub/Sub topics that can trigger the container. To meet this need, Google creates and manages service accounts for many Google Cloud services. These service accounts are known as Google-managed service accounts. *Google-managed service accounts are not listed in the Service accounts page in the Cloud Console.*

![Alt text](/images/service-account-differences.png)

[Service Accounts - Official documentation](https://cloud.google.com/iam/docs/service-accounts)

## Cloud IAP (Identity-Aware Proxy)
* [Identity-Aware Proxy - Official documentation](https://cloud.google.com/iap/docs/concepts-overview)

IAP lets you establish a central authorization layer for applications accessed by HTTPS, so you can use an application-level access control model instead of relying on network-level firewalls.

IAP policies scale across your organization. You can define access policies centrally and apply them to all of your applications and resources. When you assign a dedicated team to create and enforce policies, you protect your project from incorrect policy definition or implementation in any application.

![Alt text](/images/iap.png)