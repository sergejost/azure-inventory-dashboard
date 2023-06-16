# azure-inventory-dashboard

This repository contains templates to deploy Azure inventory dashboard.
The dashboard provides a visual summary and organized view of your Azure resources.

This project appeared as an attempt to resolve issues with automated deployment of Azure dashboards using DevOps tools. It provides ready-to-use templates that you can deploy as a code using pipelines or command line tools.


## Azure dashboards

Azure dashboards are a feature of Microsoft Azure portal that allows you to create interactive dashboards to monitor and visualize data from various Azure resources. Dashboards provide a consolidated visual view on resources, metrics, logs and other information that is relevant to your services and applications.

For each registered user the portal provides a default private dashboard as a starting point. You can edit the default dashboard and create and customize additional dashboards.

When you create a dashboard on Azure portal, it is private by default, which means you are the only one who can see it. To make dashboards available to others, you can share them (make the dashboard shared).


## Access control for dashboards

From an access control perspective, shared dashboards are similar to other Azure resources, such as virtual machines or storage accounts. Shared dashboards are implemented as Azure resources. Each dashboard exists as a manageable item contained in a resource group within your subscription.

You allow others to view your shared dashboard by using Azure role-based access control (Azure RBAC) to assign roles to either a single user or a group of users. You can select a role that allows them only to view the published dashboard, or a role that also allows them to modify it.

Within a dashboard, individual tiles enforce their own access control requirements, based on the access permissions to resources they display. The each individual user will only be able to see information for resources he/she has at least read access to. So you can share any dashboard broadly, even if some data on specific tiles might not be visible to all users. 


## Azure Resource Graph

Azure Resource Graph is used for this dashboard to provide data about queried Azure resources. Azure Resource Graph is an Azure service designed to complement Azure Resource Manager by providing efficient resource exploration with the ability to query across a given set of subscriptions.

To use Resource Graph you must have appropriate rights in Azure role-based access control (Azure RBAC) with at least read access to the resources you want to query. Without at least read permissions to the Azure object or object group, results won't be returned.

Resource Graph uses the subscriptions available to a principal during login. To see resources of a new subscription added during an active session, the principal must refresh the context. This action happens automatically when logging out and back in.


## Azure dashboards and DevOps

Azure portal allows you to easily create and customize dashboards interactively. You can also export and import dashboard templates using the portal. 

Shared dashboards, either created on portal and published, or imported using a template, are deployed as an Azure resource.

The challenges appear once you need to deploy dashboards in your infrastructure-as-code solution, either using DevOps pipelines or Azure command line tools.

Microsoft warns in documentation, that export of resource template is not guaranteed to succeed. Export is not a reliable way to turn pre-existing resources into templates that are usable in production. It is better to create resources from scratch using hand-written Bicep file, ARM template or Terraform.

While for many other Azure resources template export and following import works fine, with dashboards it does not. You still can create and customize your dashboard interactively and export a template. But when you will try to deploy the dashboard using exported template, you will either get a error during deployment or deployed dashboard will provide no data.


## Usage 

There are two ready-to-use templates that deploy the identical Azure inventory dashboards.

**ARM template**

```
az deployment group create -g dashboards --template-file dashboard-inventory.json
```

**Bicep file**

```
az deployment group create -g dashboards --template-file dashboard-inventory.bicep
```

**Parameters**

Optionally you can use parameters, either in command line or in parameter file:

- dashboard_name 
- dashboard_title

Examples:

```
az deployment group create -g dashboards --template-file dashboard-inventory.json --parameters dashboard_name='dashboard-inventory-2' dashboard_title='Inventory dashboard 2' 

az deployment group create -g dashboards --template-file dashboard-inventory.bicep --parameters dashboard_name='dashboard-inventory-2' dashboard_title='Inventory dashboard 2' 
```
