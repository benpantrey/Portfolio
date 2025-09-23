---
linkTitle: "Zones"
title: "Zones"
weight: "5"
description: "A zone, in Sysdig, is a collection of scopes that represent important areas of your business. Zones filter what data, such as events and findings, users or teams can see. For example, you can create a zone for your production environment, a staging environment, or a region."
---

## Prerequisites

Before you create a zone, connect a Data Source to scan your infrastructure for the necessary data.

If you intend to filter by team, ensure you [Enable Team Zones](#enable-team-zones).

## Default Zones

Sysdig provides two zones by default:

* **Entire Infrastructure**: This applies to [connected data sources](/en/docs/sysdig-secure/integrations-for-sysdig-secure/data-sources/). Create a new zone to update findings that are reported on the [Compliance](/en/docs/sysdig-secure/posture/compliance/) page.
* **Entire Git**: If you have configured [Infrastructure as Code (IaC) scanning with Git integrations](/en/docs/sysdig-secure/iac-security/git-iac-scanning/) to your development pipeline in Git, then the **Entire Git** zone is automatically applied to those source repositories. You can also create more targeted zones for selected Git sources.

You can create more Zones to suit your organization's needs.


## Create and Configure a Zone

A completed Zone includes:

* Zone name and description
* Zone scope (the area of business to be included)

To create a Zone:

1. Log in to Sysdig Secure, and navigate to **Inventory** > **Zones**.

2. Click **New Zone**.

3. Enter a **Name** for the new zone.

4. (Optional) Enter a **Description**.

5. In the **Scope** section, select **Add Scope**.

6. Select from the available platforms. These include AWS, GCP, Kubernetes, Git, and more.

7. Use the **All Resources** toggle to determine whether the zone should include all the resources within the selected platform. It is enabled by default.

8. To define a more granular scope, toggle **All Resources** off. Then, specify the resources to include as key-value pairs.
To define a more specific scope using key-value pairs, do the following:

   1. Select a key/attribute: 
      - **Resource**: These are pre-defined, and vary depending on the platform. For example, **Account**(AWS), **Subscription** (Azure),and  **Project** (GCP). These align with cloud provider best practices and help prevent visibility gaps. For available resources, see [Available Scope Rule Attributes](#available-scope-rule-attributes)
      - **Custom Label**: Scope with tags and labels. This option is less optimal than scoping with platform-native resources.
      
   2. Select an operator:
      - `in`: For exact matches.
      - `contains`: For substring matches.
      - For more details, see [Use Operators and Values](#use-operators-and-values)

   3.  Enter one or more values. 
      - You can define multiple scopes. They are combined using **OR**. Attributes within a scope are combined using **AND**.

9. Click **Save** to save the Zone.

Your saved zone will appear in the list on the main Zones page. Data from Zones may take up to 24 hours to appear in other modules, such as **Inventory** > **Resources**.

{{% infobox type="warning" %}}

Use of the **Region** scope may result in more data being shown to users in the **Posture** > **Identity Management** pages than defined in the Zone.

{{% /infobox %}}

### Available Scope Rule Attributes

Supported scope rule attributes vary according to platform:

**AWS**

* Organization
* Account
* Region
* Labels

**Azure**

* Organization
* Subscription
* Region
* Labels

**GCP**

* Organization
* Project
* Region
* Labels

**OCI**

* Tenancy
* Compartment
* Region
* Labels

**Host**

* Cluster
* Host Name (for Docker, Linux hosts)
* Shield Tags

**Kubernetes**

* Distribution (AKS, GKE, EKS, Vanilla Kubernetes)
* Cluster name
* Namespace
* Labels
* Shield Tags

**Git**

* Git integration
* Git source(s)

**Image**

* Registry
* Repository

### Use Operators and Values

After the key, you can use operators and values.

Sysdig supports two operators:

* `in`:
  * Matches exact values. Use this to scope specific cluster names.
  * For example, defining a scope, `Cluster` + `in` + `prd`, will only match the cluster `prd`, if it exists.
  * You can match multiple values. For example, use the scope, `Cluster` + `in` + `prd` + `demo`, to include the clusters `prd` and `demo`.
* `contains`:
  * Matches a value inside a string. Use this to scope cluster names containing a given value.
  * For example, defining a scope, `Cluster` + `contains` + `prd`, will include clusters such as `myApp-prd`, `prd1`, and `prd-sysdig`.

After the operator, select a value. Each value field has a limitation of 2048 characters per row. For longer values, consider adding scopes. This improves readability and maintenance of your scopes.

Auto-complete values will be based on resources that were scanned and listed in the [Inventory]({{< ref "Inventory" >}}).

## Known Limitations

There are limits to the number of platforms, keys, and values supported per zone, as well as the number of zones supported per team. For example, you can define up to 25 keys (for example, **Region**) per zone. Each key can have one or more specific values (for example: **Region** `in` `eu-central-1`). You can define up to 50 values per key. In summary:

- The maximum number of platforms that can be supported per zone is 20.
- The maximum number of keys that can be supported per zone is 25.
- The maximum number of values that can be supported per key is 50.

## Team Zones (CA)

{{% infobox type="note" %}}

Team Zones is in Controlled Availability. Contact Sysdig Support to request access. This is an experimental feature, and will not work for Vulnerability Management (VM) and Threat Detection.

{{% /infobox %}}

Team Zones allow you to limit the scope of certain teams, restricting what a team can see to what is strictly necessary. This boosts security, following the principle of minimal privilege.

Zones are designed to replace Team Scopes. Unlike Team Scopes, Zones only have to be defined once, and can then be applied at will. Zones work for agentless cloud resources, as well as agent resources.

### Enable Team Zones

Once Sysdig Support has granted you access to this feature, Admin users can enable Team Zones:

1. Log in to Sysdig Secure as an Admin.

2. Select **Settings** > **User Profile**.

3. Under **Sysdig Labs**, enable **Zones based team scoping** and **Zones scoping for all features**.

4. Click **Save**.

Zones based team scoping is now enabled.

### Apply a Team Zone

To apply zones to teams:

1. Log in to Sysdig Secure.

2. Select **Settings** > **Teams**

3. Select an existing team, or select **Add team** to create a new team.

  The Team configuration page appears.

4. Under **Zones**, select **All Zones**, or select one or more zones under **Selected Zones**.

  If you do not select a zone, the scope will default to the configuration entered under **Team Scopes**.
