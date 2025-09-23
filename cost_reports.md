---
linkTitle: Reports
title: Cost Reports
description: "Use Cost Reports to understand how much your organization spends on workloads, containers, and namespaces. You can automate periodic reports in JSON/CSV and send them out to Slack and email, or export them for analysis in third-party tools."
---

## Review Reports

To access the Reports page and review reports:

1. Log in to Sysdig Monitor.

2. Select **Costs** > **Reports**.

   The Reports page appears. Here, you can review your existing reports.

3. Use the search bar to find a report by name.

4. Use the **Select Report Type** dropdown to filter reports by type.

5. Select columns, such as **Name**, **Enabled** or **Report Type** to sort the list alphabetically, by enablement status, or by type.

## Create a Report

To create a Costs Report:

1. Log in to Sysdig Monitor, and select **Costs** | **Reports** or **Costs** | **Costs Explorer**.

2. From either **Reports** or **Cost Explorer**, select **Create New Report** in the top right corner.

   The **New Report** page opens.

3. Configure the report details:

  - **Name**: Enter a unique name for your report.
  - **Description**: Enter a meaningful description.
  - **Report type**: Select your preferred report type.
    - **Workload Cost Trends**: Analyze workload cost changes over time to track spending trends. This is useful for comparing costs between periods. See [Workload Cost Trends Report]({{< ref "Reports#workload-cost-trends-report" >}})
    - **Wasted Workload Spend**: Gain insight into expenses from oversized or underutilized workloads. This is useful for identifying inefficiencies in resource usage. See [Wasted Workload Spend Report]({{< ref "Reports#wasted-workload-spend-report" >}})
  - **Export Format**: Choose the format the report should be exported in: JSON or CSV. You can export reports for offline processing or integration with third-party tools.

4. Define the report. By default, the scope includes the **Team Scope** of the team your current login. You can see the form your report will take in the **Data Preview** below.

5. Filter the scope with labels. For example, to limit the report to the namespace `risk-flow`, use the labels `kube_namespace_name` `in` `risk-flow`. You can add multiple labels to build a highly specific report.

6. Use **Group By** to determine which grouping columns will appear in the report. **Wasted Workload Spend** reports have the groups `kube_cluster_name`, `kube_namespace_name`, and `kube_workload_name` by default.

7. **Time Window**: For **Wasted Workload Spend**, choose the **Time Window** of the report.

8. Set the frequency of the report. This determines the time frame of cost data included in the final report.

9. Select which notification channel, such as Slack or Email, should be notified when the report is ready.

You can observe a **Preview** of the data to export. You can also retrieve the latest report through our API and connect with your preferred tools.


## Types of Cost Reports

### Workload Cost Trends Report
Workload Cost Trends Reports reveal which resource consumption trends drive changes in your costs. Use these reports to compare costs between periods, such as week over week or month over month. By grouping data with labels like `kube_cluster_name` and `kube_namespace_name` you can identify whether workloads are becoming more or less expensive over time.

### Wasted Workload Spend Report
Wasted Workload Spend Reports provide insights into resource consumption at the workload level by calculating how much allocated CPU and memory a workload uses. By default, these reports group cost data by `kube_cluster_name`, `kube_namespace_name`, and `kube_workload_name`. You can add additional label groupings, but these defaults are the minimum required. The report calculates the following cost metrics at a workload level:

* Accrued Cost: Represents the CPU and memory costs allocated to a workload, based on its configured CPU and memory quotas. If usage exceeds these quotas, the actual usage determines the accrued cost. Costs from Persistent Volume Claims (PVC) are also included in this calculation.

* Estimated Rightsizing Spend: Represents the cost of resources based on actual usage of CPU and memory as an average over a specified time window. It provides insights into potential savings by aligning resource allocation with actual usage patterns.

* Wasted Spend: The cost of CPU and memory resources requested by a workload but not utilized. While some buffer is essential for handling spikes, excessive unused resources can result in unnecessary costs.

The relationship between these terms is represented in the formula:
`Accrued Costs = Estimated Rightsizing Spend + Wasted Spend`

### Workload Rightsizing Report

{{% infobox type="note" %}}
This feature is currently in beta.
{{% /infobox %}}

Workload Rightsizing Reports optimize resource allocation for Kubernetes workloads by comparing actual resource usage (CPU and memory) of containers within those workloads against historical usage. These reports help identify opportunities to downsize or upsize workloads based on real usage patterns, reducing resource waste and improving performance.

By default, rightsizing recommendations are grouped by `kube_cluster_name`, `kube_namespace_name`, and `kube_workload_name`. This grouping reflects how CPU and memory resources are allocated in Kubernetes (at the workload level) and cannot be modified.

To refine the scope of the report, you can configure a scope using workload labels only. This concentrates focus on specific workloads or subsets of workloads.

{{% infobox type="note" %}}
Rightsizing recommendations are based on usage data collected across the full time window. If you narrow the scope by adding extra labels, you might exclude some containers from the analysis, which can lead to inaccurate or incomplete recommendations if the label was applied inconsistently or only to a subset of containers.
{{% /infobox %}}

#### Algorithms
When you define the report, you can choose from these algorithms to determine how CPU and memory usage data is interpreted to generate rightsizing recommendations:

**Avg - Savings Focused**: Calculates the average CPU and memory usage over the selected time window. This approach tends to recommend lower resource requests, focusing on potential savings.

**Max - Reliability Optimized**: Uses the maximum CPU and memory usage observed during the time window. This is best when you want to avoid throttling or OOM (Out of Memory) risks, ensuring enough headroom for peak usage.

**P95 - Balanced**: Uses the 95th percentile of CPU and memory usage, excluding the top 5% of usage spikes. This provides a balance between cost efficiency and reliability.

#### Time Window

The selected time window determines the range of usage data considered by the algorithm.
For example, if you choose a 1-week window with the Max algorithm, the recommendation will reflect the highest resource usage seen in that week for each workload.

#### Report Fields

**Number of Pods**: The total number of Pods, including those in ReplicaSets, Deployments, DaemonSets, and StatefulSets, that contain the container associated with this recommendation.

**Monthly Cost**: An estimate of the workload's current monthly cost, based on its daily cost multiplied by 30.

**Monthly Potential Savings**: The estimated monthly savings if the workload is rightsized based on the suggested values. This can be negative if the workload is currently under-provisioned.

**Suggested CPU (m)**: The recommended CPU request in millicores (1 CPU = 1000m).

**Suggested Memory (Mi)**:The recommended memory request in mebibytes (1 Mi = 1,048,576 bytes)

## Download Reports

To download a report as a CSV or JSON, depending on the Report's configuration:

1. Log in to Sysdig Monitor and select **Costs** > **Reports**.

   The **Reports** page appears.

2. Optional: To download the latest report, select the download icon on a Report listing.

3. Select the report from the list.

   The detail panel opens with a full list of generated reports.

4. Identify the report you want, and click **Download**.

## Disable or Enable a Report

Once you create a report, it will generate according to the schedule you configured. To disable a scheduled Report:

1. Log in to Sysdig Monitor.

2. Navigate to **Costs** > **Reports**.

3. Under the **Enabled** column, toggle the Report off.

The Report schedule is now disabled. To re-enable it, toggle it back on.
