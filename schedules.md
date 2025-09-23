---
linkTitle: "Schedules"
title: "Schedules"
weight: "9"
description: "Sysdig Reporting is a highly scalable, powerful reporting platform. Use it to quickly create and schedule reports with large swathes of information. Reports are interactive, and can contain up to 30 days worth of data. You can export Reports in a variety of formats, secure data by filtering reports by Zone, and protect reports with a password."
---

The **Schedules** page lets you review, create, edit, enable and disable report schedules.

## Access Schedules

To access **Schedules**:

1. Log in to Sysdig Secure.

2. Select **Reporting** > **Schedules**.

  The **Schedules** page appears.

## Review Schedules

On the **Schedules** page, you can see a list of schedules associated with your account.

You can find specific reports with the search bar and the drop-down list.

For each report schedule listed, you can see:

- A toggle to enable or disable the schedule.
- **Name**: The name of the schedule.
- **Reports included**: Which report the schedule creates.
- **Frequency**: How often the report is generated.
- **Last Completed**: When this report schedule last created a report.

Click the three-dot icon beside any report schedule to **View History** for the report.

Select any schedule listing to edit the schedule. See [Configure a Schedule](#configure-a-schedule).

## Create a Schedule

To create a schedule:

1. Log in to Sysdig Secure.

2. Select **Reporting** > **Schedules**.

  The Schedules page appears.

3. Select **New Schedule** from the top right corner.

  The **New Schedule** page appears.

4. Fill out the configuration details. See [Configure a Schedule](#configure-a-schedule).

## Configure a Schedule

You can access the Schedule configuration page when you create a new schedule, or you edit an existing schedule. Here, you can configure the following:

- **Name**: The name of the schedule.
- **Description**: Enter any descriptive text for the schedule.
- **Enabled**: Toggle this to enable or disable the schedule.
- **Report**: From the drop-down, select the report you wish to schedule. Default reports include:
  - **Resource Posture Report** (RPR): Provides a comprehensive report of your resource posture in a CSV file. Always available and does not require specific zones or policies.
  - **Policy Compliance Report** (PCR): Reports on compliance with specific policies within a defined zone. Select this report from the dropdown to access the policy dropdown. You must select one policy and one zone for this report. The report is available as a PDF.
  - The dropdown also displays custom reports. To create your own, see [Create a Custom Report](/en/docs/sysdig-secure/reporting/reports-manager/#create-a-custom-report).
- **Scope**: After selecting the report, the scopes are automatically populated with the scope of the report and cannot be changed directly. If you choose to change the scope, you can update the scope from the report, which will affect all associated schedules. Or create a new report without affecting any associated schedules.
- **Timeframe**: Select the period of time you wish the report to gather data. The minimum is 1 day, and the maximum is 30 days.
- **Frequency**: Define how often you wish the schedule to run.
- **Notifications**: Optional: Select a notification channel if you wish to be notified when a report is created by a schedule, and ready to be downloaded. The available channels are [Email](/en/email-notifications.html) and [Slack](/en/slack-notifications). To create a notification channel, see [Set Up Notification Channels](/en/set-up-notifications/).
- **Format**: Select the format for you report. **PDF** is the default. **JSON** and **CSV** are only available when the report is a single table.
- **Compression Format**: Select the format compression for your report. **Gzip** (`.gz`) and **Zip** (`.zip`) are supported.
- **Password Protection**: Optionally, toggle this on to set a password on the report PDF that is created. Be sure to remember your password, as it cannot be reset once the report is created. This is supported for zip files, but is not supported for Gzip files.

## Delete a Schedule

To delete a schedule:

1. Log in to Sysdig Secure.

2. Select **Reporting** > **Schedules**.

  The Schedules page appears.

3. Select the three-dot icon of the schedule you wish to delete.

4. Select **Delete**.
