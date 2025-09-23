---
linkTitle: Captures
title: Captures (Monitor)
description: "Sysdig capture files contain system calls and other operating system events. You can create captures manually, or configure certain Alerts, such as Threshold, Group Outlier, or Downtime, to take captures. The capture files can then be analyzed from Sysdig or opened with multiple open-source tools."
---

## Limitations

The Sysdig Agent can record only one capture per host at a time due to the volume of data collected. If multiple alerts, each configured to create a capture, are triggered simultaneously on the same host, only the first capture will be taken. This issue also occurs with overlapping captures, often caused by lengthy capture durations.

## Access Captures

To access the **Captures**:

1. Log in to Sysdig Monitor.

2. Select **Captures** from the left navigation bar.

## Navigate Captures

The Captures page contains a table listing the:

- **Status**: When the status is **Ok**, the file has been successfully transmitted from the Sysdig agent to the storage bucket, and is available for download and analysis.
- **Name**: The capture file name.
- **Time**: The time the capture was taken.
- **Duration**: The period of time captured.
- **Trigger**: The cause that triggered the capture.
- **Infrastructure**: The host the capture was retrieved from.

You can also search for captures using the search bar, or filter them by **Error** or **Expiring**.

## Take a Capture

There are two ways to create a capture file:
- Manually
- From an alert

### Take a Capture Manually

To take a capture manually:

1. Select **Captures** from the left navigation bar.

2. Select **Take Capture** from the top right corner.

    The **Take Capture** window appears.

3. Specify the following information:

   - **Name**: Define the Name of the capture, also used as capture file name.
   - **Host Name**: Specify the Host where you want to take a capture
   - **Container ID**: Configure the Container ID to set as a predefined filter when you open the Capture using Sysdig Inspect.
   - **Storage**: Choose your storage options. The default is **Sysdig Storage**, where captures are stored for 90 days. To configure alternative storage options, see [Configure Capture Storage](/en/capture-storage/).
   - **Duration**: Define the duration of the capture. The default time is 5 seconds; the maximum capture time available is 300 seconds (five minutes). The capture file size limit is 100MB.<br />Sysdig recommends using the default time to ensure captures are small and manageable.
   - **Filter**: Optionally, set a filter for the capture. This restricts the data streaming captured (syscalls, i/o streams), while it doesn't apply to the inspector tables (file descriptors, network connections, open ports, thread table, process list, containers). Applicable filters match the syntax for Falco conditions. See [Condition Syntax](https://falco.org/docs/reference/rules/supported-fields/).

4. Click **Start**.

   The capture is taken.

5. After the capture is complete, select whether to **Keep it** or **Discard** the capture. Capture you keep are displayed in the **Captures** report page.

### Take a Capture from an Alert

To configure a capture to be taken when an alert triggers:

1. Select **Alerts** from the left navigation bar.

2. Select **New Alert** and select **Threshold**, **Group Outlier** or **Downtime**.

   Captures cannot be configured for other alert types.

   Alternatively, edit an existing Threshold, Group Outlier, or Downtime alert.

3. Complete the configuration under the **Capture** section:

  - **Capture Enabled**: Toggle on or off.
  - **Capture Duration**: The period of time captured. The default time is 15 seconds; the maximum capture time available is 24 hours. The capture file size limit is 100MB. The capture time starts from the time the alert threshold was breached. It does not capture syscalls from before the alert was triggered. <br/> We recommend using the default time to ensure captures are small and manageable.
  - **Capture Storage**: The storage location for the capture files. The default is **Sysdig Storage**, where captures are stored for 90 days. To configure alternative storage options, see [Configure Capture Storage](/en/capture-storage/).
  - **Capture Name**: The name of the capture file. The default name includes the date and time stamp the capture was created.
  - **Capture Filter**: Restricts the amount of trace information collected. The filter uses [Falco syntax](https://falco.org/docs/reference/rules/supported-fields/). For example, you can use `container.id` or `container.name` as a filter in the format `container.id=123b12345678`. The resulting capture would contain syscalls only for the specified container. <br/>The filter restricts only the data streaming captured (syscalls, i/o streams), while it doesn't apply to the inspector tables (file descriptors, network connections, open ports, thread table, process list, containers).

When the alert triggers, a capture will be taken. The capture can be viewed in the Sysdig Monitor **Captures** page.

## Review a Capture

From the Captures page you can perform multiple operations on the previously taken Captures:
  - Open with Sysdig Inspect
  - Open Explore
  - Go to the triggering Notification
  - Download
  - Delete

### Review a Capture with Sysdig Inspect

To review the capture file with Sysdig Inspect:

1.  From the **Captures** page, navigate to the target capture file.

2.  Hover your cursor over the target capture file.

3.  Click on the **Sysdig Inspect** button to open Sysdig Inspect in a new browser tab.

### Explore a Capture File

To open a capture in Explore:

1. From the **Captures** page, navigate to the target capture file.

2. Select the three-dot menu icon from the right hand side of the capture listing.

3.  Select the **Open in Explore** button.

   You will be directed to the **Explore** tab view of the capture.

### Go to the Capture related Notification

To open the Notification related to the Capture:

1. From the **Captures** page, navigate to the target capture file.

2. Select the three-dot menu icon from the right hand side of the capture listing.

3.  Select the **Open Notification** button.

   You will be directed to the **Events** page, where you will see the Notification that triggered the Capture.

### Download a Capture File

To download a capture file:

1. From the **Captures** page, select the three-dot menu on the side of a capture listing.

2. Select **Download File**.

 The capture downloads in .scap format. You can open this with:
  - [Sysdig Inspect](https://github.com/draios/sysdig-inspect)
  - [sysdig or csysdig](https://github.com/draios/sysdig)

### Delete a Capture File

You can delete a single capture file or multiple files.

To delete a single file:

1. From the **Captures** page, navigate to the target capture file.

2. Hover your cursor over the target capture file.

3. Select the three-dot menu icon from the right hand side of the capture listing.

4. Select the **Delete** (trash can) button.

3. Click **Delete** to confirm deleting the capture, or **No** to cancel.

To delete multiple files:

1. From the **Captures** page, select the capture files to be deleted.

2. Select the option **Delete** next to the number of selected captures in the top right corner.

3.  On the **Delete Captures** prompt, click the **Yes** button to confirm, or the **No** button to cancel.

### Delete Capture Files

Captures saved in the Sysdig Storage older than 90 days will be automatically deleted. For longer storage, configure a custom S3 storage bucket. See [Configure AWS Capture File Storage](/en/docs/administration/administration-settings/storage-configure-options-for-capture-files/#storage-configure-options-for-capture-files).

You can also delete Captures manually.

## Disable Capture Functionality

Sometimes, security requirements dictate that capture functionality should not be triggered at all (for example, PCI compliance for payment information).

To disable Captures altogether, edit the agent configuration file as described in [Disable Captures](/en/disable-captures.html).

## Capture Files Storage

Sysdig capture files are stored in Sysdig's storage (for SaaS
environments), or in the Cassandra DB (for on-premises environments) by
default.
Captures saved in the Sysdig Storage (SaaS environments) for more than 90 days are automatically deleted.
Both environments have the option to use a S3-compatible custom storage, such as [Minio](https://min.io/) or [IBM Cloud Object Storage](https://www.ibm.com/cloud/object-storage). This lets you store captures for longer periods of time. To configure a custom storage, see [S3 Capture Storage](/en/capture-storage/).
