---
no_list: true
linkTitle: "Integrations"
title: "Integrations for Sysdig Monitor"
weight: "25"
description: "Integrations for Sysdig Monitor lets you monitor infrastructure, applications, and cloud accounts. Sysdig supports metrics from Prometheus, JMX, StatsD, Kubernetes, and a number of applications to provide a 360-degree view of your environment. Many metrics are collected as soon as the agent is installed."
---

The Integrations module of Sysdig Monitor features four **Data Sources**:

- **Monitoring Integrations**: Manage the collection of metrics across multiple environments via Monitoring Integrations.
- **Cloud Accounts**: Collect metrics from cloud providers such as AWS, GCP, and Azure.
- **Sysdig Platform Audit**: Review audit logs of activity on Sysdig Platform.
- **Sysdig Agents**: Check the status of Sysdig agents. 

This page covers the basic concepts behind Integrations.

## What is an Integration?

An integration refers to the process of connecting Sysdig with other applications and platforms. An integration enables the Sysdig agent to scrape an application, such as Nginx, Kubelet, or HAProxy, for metrics. Integrations can also refer to cloud accounts, the Activity Audit, or the Sysdig Agents pages.

## What is a Metric?

A metric is a logical collection of time series sharing the same name, such as `workqueue_depth`, `kube_job_complete`, or `http_requests`. Many applications make dozens of metrics available for collection. Sysdig Integrations prioritize collection of the most valuable metrics for use in modules such as **Dashboards**, and **Alerts**.

## What is a Time Series?

A metric is identified by its unique name, such as `http_requests`. A time series is defined by both the metric name and a distinct set of label key-value pairs, for example `http_requests{method="GET", endpoint="/api/v2/users"}`. Every time series is unique, since no two can have an identical combination of labels.

|  Metric    | Times Series | Value  | Description |
| -------- | --------------- | ------------- | -------- | 
| `http_requests`  | `http_requests{method="GET", endpoint="/api/v2/users"}`   | 100          |  number of `GET HTTP` requests to `/api/v2/users`          | 
|  `http_requests` | `http_requests{method="POST", endpoint="/api/v2/users"}`    | 250   |  number of `POST HTTP` requests to `/api/v2/users`   | 
| `http_requests`  | `http_requests{status="500", endpoint="/api/v2/users"}` | 10     | Number of `HTTP requests` returning 500 to `/api/v2/users`  | 

### Default and Custom Metrics

Sysdig Agent collects thousand of metrics at no additional cost, such as `sysdig_container_net_server_in_bytes` or `kube_pod_info`; these are called default metrics.

On the other hand, there are custom metrics, which do count towards your Time Series entitlement limit. They can be identified with the `_sysdig_custom_metric="true"` label. Any metric without this label is a default metrics that doesn’t count towards your entitlement limit.

## How Are Metrics Collected?

The Sysdig Agent will begin collecting and reporting metrics as soon as it is installed.

For applications in environments where the agent is not installed, you can send metrics to Sysdig with Prometheus Remote Write.

Cloud accounts start sending metrics as soon as they are connected to Sysdig.

##  Customize Metrics Collection

Once an integration is set up, the Sysdig agent will begin to collect metrics. To configure metrics collection:

- Enable an integration to collect metrics from that integration.
- For metrics collected by the Sysdig Agent, you can exclude clusters from collection through the UI.
- For metrics collected via Prometheus Remote Write, edit the `prometheus.yaml` file to exclude the collection of certain metrics.
- Edit the annotation of a pod to prevent the collection of metrics by the Sysdig Agent.
- For Cloud Account metrics, modify metrics collection from the **Cloud Accounts** module.

These methods let you add new metrics or drop metrics from being collected.

### Default and Custom Integrations

There are two types of integrations: default integrations and custom integrations. 

Default integrations exist for applications found in Sysdig's Integrations Library: Application integrations, cloud integrations, and infrastructure Integrations all belong under the umbrella of default integrations.

Custom integrations are integrations not natively included by Sysdig. You use custom integrations to collect metrics from a source that is not found in the Integrations Library. This normally involves additional configuration.

## Manage Default Integrations

To access **Monitoring Integrations**, log in to Sysdig Monitor, and select **Integrations** > **Monitoring Integrations** from the left navigation bar. 

Browse the list on the **Monitoring Integrations** page to see which workloads Sysdig has detected on your connected environments. Click a listing to find out about the workload in detail. Here, depending on the status of the integration, you can see what metrics are available, configure or manage an integration, and view associated dashboards and alerts.

## Scope, Search, and Filter Integrations

You can narrow down the integrations visible in the **Monitoring Integrations** list in three ways: by scope, by filter, and by search.

- Scope: Use the directory to the left of the workloads list to view workloads from either your **Entire Infrastructure**, or from an individual **Cluster** or **Namespace**.
- Search: Use the **Search** bar to search by instance name.
- Filter: Click **Filter by type** to open a dropdown, where you can filter the integrations identified in your environment.

## Integrations Status

The list of **All Instances**, where every workload meeting your scope, filter and search conditions is listed, is further divided into five categories:

### Reporting Metrics

These integrations are configured correctly and are reporting metrics. Click a listing to:
- See when this integration was last checked.
- Enable or disable it globally or per cluster.
- Review types of metrics collected
- Create exceptions to limit the data being collected.
- See the Dashboards using these metrics.
- See linked Alerts.
- View compatibility and installation details. 

### Needs Attention

These integrations are not reporting metrics. Click on a listing to begin troubleshooting. The issue may be:
- The integration was not correctly identified.
- The agent settings have been configured to disable the integration.
- Check the configuration of the integration and, if necessary, contact support.

### Pending Metrics

These integrations have recently been configured and are waiting to receive metrics. This process takes up to 10 minutes to complete.

### Requires Configuration

You must configure these integrations to collect metrics. Open the detail page to decide the next steps. 

There are two possibilities: 
- A wizard-assisted configuration is available from Sysdig.
- No integration has been identified, and you need to create a custom integration.

### Available to Enable

These integrations are ready to collect metrics but are not yet enabled. You might incur costs if you enable them.

## Manage Custom Integrations

Use custom integrations to collect metrics from applications not found in the Integrations Library.

### View Unidentified Workloads

Workloads that require custom integrations are found under the **Unidentified Workloads** section in the **Monitoring Integrations** page of the Sysdig Monitor UI.

When Sysdig does not recognize the application from which metrics have come, it categorizes them as “Unidentified workloads.” You can configure these integrations via custom integration steps.

To view unidentified workloads Sysdig has detected in your environments:
1. Log in to Sysdig Monitor.
2. Select **Integrations** > **Monitoring Integrations**.
3. Select the **Show Unidentified Workloads** toggle on the top right of the page.

In rare cases, Sysdig may have a default integration, but the backend did not correctly identify the workload. In this case, you can manually assign the correct Integration. See .

### Collect Metrics with Custom Integrations

There are several ways to collect metrics with custom integrations, depending on the location of the workload, and the type of metrics:

- Prometheus Metrics: See [Collect Prometheus Metrics](/en/collect-prometheus-metrics).

- Java Management Extensions (JMX) Metrics: See [Integrate JMX Metrics from Java Virtual Machines](/en/integrate-jmx-metrics-from-java-virtual-machines).

- Node.js Metrics: See [Integrate Node.js Application Metrics](/en/integrate-node-js-application-metrics).

- StatsD Metrics: See [Integrate StatsD Metrics](/en/integrate-statsd-metrics).

Once configured, custom integrations will not be visible from the **Monitoring Integrations** page. To view those metrics:

1. Select **Explore** > **Metrics Usage**.
2. Select the **All jobs** drop down to filter your metrics by job.

The metrics will also be available in modules such as **Dashboards** and **Alerts**.

## Third Party Integrations

### Platform Metrics (IBM)

For Sysdig instances deployed on [IBM Cloud Monitoring with Sysdig](https://www.ibm.com/cloud/sysdig), an additional form of metrics collection is offered: Platform metrics. Rather than being collected by the Sysdig agent, when enabled, Platform metrics are reported to Sysdig directly by the IBM Cloud infrastructure.

Platform metrics are metrics that are exposed by enabled services across the IBM Cloud platform. These services have made metrics and pre-defined dashboards for their services available by publishing metrics associated with the customer’s space or account. Customers can view these platform metrics alongside the metrics from their applications and other services within IBM Cloud monitoring.

Enable this feature by logging into the IBM Cloud console and selecting **Enable** for IBM Platform metrics under the **Configure your resource** section when creating a new IBM Cloud Monitoring with a Sysdig instance. See [Enabling a monitoring instance through the UI](https://cloud.ibm.com/docs/monitoring?topic=monitoring-platform_metrics_enabling#platform_metrics_enabling_step2).

### Keda

To integrate Keda for Horizontal Pod Autoscaler (HPA), see [Integrate Keda for HPA](/en/integrate-keda-for-HPA).