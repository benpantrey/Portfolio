---
linkTitle: Silence Alert Notifications
title: Silence Alert Notifications
description: "Silencing Rules prevent alerts from being sent to notification channels. You can use them to mute notifications, such as when you are aware of an issue and maintenance is already scheduled."
weight: "13"
---

Silencing Rules do not affect the evaluation of alert rules. If an alert rule is triggered during an active Silencing Rule, the alert occurrence will still be recorded as an [Event]({{< ref "Events" >}}) with an indication that it was silenced.

## Scenarios

Silencing rules can affect alert notifications and events in several ways depending on when the alert condition was met and resolved.

### Alert Rule Triggers and Resolves During Silencing Rule Window

If the alert rule triggers and resolves during the Silencing Rule window:
- Events Feed: A silenced alert occurrence appears in the event feed. Upon resolution, this alert occurrence is marked as resolved.
- Notification Channels: No alert notifications are forwarded to notification channels.


### Alert Rule Triggers Before Silencing Rule

If an alert rule is satisfied before the silencing rule, it will not be impacted by the silencing rule. You will still receive a notification when the alert resolves if the alert rule is configured to forward resolution notifications.
- Events Feed: An alert occurrence appears in the event feed. Upon resolution, this alert occurrence is marked as resolved.
- Notification Channels: An alert notification is forwarded to the user-specified notification channels when the alert triggers. If configured, a resolution notification is also forwarded upon resolution.

### Alert Rule Triggers During Silencing Rule but Resolves Later

An alert rule that is satisfied during a silencing rule window will trigger a notification at the end of the silencing rule window if the alert condition is still met.
- Events Feed: A silenced alert occurrence appears in the event feed. After the silencing rule has expired, a new alert occurrence will appear in the event feed. This alert occurrence is responsible for forwarding alert notifications to the user-specified notification channels.

- Notification Channels: A notification appears when the alert rule is satisfied after the silencing rule expires. If configured, a resolution notification is also forwarded upon resolution.

## Configure Silence Rule

You can enable Silencing Rules immediately or schedule them for the future. This provides the flexibility to plan ahead and avoid unnecessary disruptions.

To configure a Silencing Rule:

1.  Log in to Sysdig Monitor.

2.  From the left navigation bar, select **Alerts** > **Silence**.

    The page shows the list of all the existing silences.

3.  Click **Set a Silence**.

     The **Silencing Rule** modal appears.

4.  Specify the following:

    -   **Name**: Specify a name to identify the silence.

    -   **Silence Criteria**: Create a silence by alert and/or scope.

        - **By Scope**: Specify the entity, workload, or namespace you want to apply the scope as. For example, configuring a silence on the `region=us-east-1` scope will silence all alerts in the `us-east-1` region.

        - **By Alert**: Specify the alert that you want to mute. You can configure a silence for one or more alerts. Silencing the `Datacenter Down` alert instead of the `region=us-east-1` scope allows other alerts within the scope to continue notifying.

             For maximum specificity, combine both scope and alert.

    -   **Begins**: Select the day you want the silence to begin.

    -   **Duration**: Specify how long notifications should be muted.

    -   **Notify**: Select a notification channel that you want to notify when the Silence starts and ends. The available channels are Email and Slack.

5.  Click **Save**.

## Silence Alert Notifications from Alerts Page

To configure a Silence Rule for a specific alert, navigate to the **Alerts** page and select the alert you want to silence. You can then configure a Silence Rule specifically for that alert.

## Silence Alert Notifications from Events Feed

To configure a Silence Rule, access the Events feed. On the Events feed, find the **Alert Event** created when an alert is triggered. From the **Alert Event**, you can configure the Silence Rule to include both the scope and the alert that generated the event.

## Alert Events from Silenced Alerts

When an alert is silenced, it will not send notifications to your configured notification channels, but it will still generate Alert Events. These events will include information indicating that the alert was triggered during an active silence.

After the silencing window expires, the alert will trigger again if the alert rule continues to be satisfied.

By generating Alert Events, you can review the event history to see when the alert was silenced and when it resumed normal notifications. This helps provide visibility into what happened during the silence and can help with troubleshooting any issues that may arise.

{{% infobox type="note" %}}

Alerts without a notification channel won't be marked as silenced, and won't have the crossed bell icon or option to silence events in the events feed.

{{% /infobox %}}

## Manage Silencing Rules

You can manage silences individually, or as a group, by using the
checkboxes on the left side of the **Silence** UI and the customization bar. Select a group of silences and perform batch delete operations.

Select individual silences to perform tasks such as enabling, disabling, duplicating, and editing.

### Change States

Enable or disable a silence with the **State** toggle. Active Silences include both running silences and scheduled silences.

Completed silences cannot be re-enabled, but can be duplicated with a new silence period.

### Duplicate a Silence Rule

To duplicate a Silence Rule, click the duplicate/copy icon.

### Filter a Silence Rule

Filter silences by name using the search bar. Silencing Rules can also be filtered by the following categories: **Active**, **Scheduled**, **Completed**.

### Edit a Silence Rule

To edit a silence rule, click the edit icon.

You can edit Scheduled Silencing Rules before they begin.


### Extend the Time Duration

To extend the time duration of a silence, click the duration icon.

You can only extend or disable Active Silences. If you configure a notification channel for the Silencing Rule, the channel will be notified regarding the extended or disabled silencing rule.

## Disable Silencing Rule

To disable silencing rules, use the API call PATCH `/api/v1/silencingRules/disable` to provide a list of rule IDs The API call executes necessary operations to resolve associated alert occurrences with the alert rule and disable it immediately. Example:

```bash
curl --location --request PATCH 'http://{sysdig_url}/api/v1/silencingRules/disable' \
--header 'Content-Type: application/json' \
--data '{
    "silencingRules": {
        "ids": [9]
    }
}'
```

## Pause Alerts for Downtime

**Admin** users can temporarily pause all alert rule evaluations across every Sysdig team. This stops alert events from triggering and suspends notifications across all configured notification channels.

1. Log in to Sysdig Monitor as an Admin.

2. Select **Settings** > **Notification Channels**.

3. Toggle **Pause Alert Rule Evaluation** on. A confirmation dialog will appear.

4. Optional: Set a **Duration** to automatically resume alert evaluation after a specified time.

5. Optional: To inform others that alerts are paused, check **Notify via email or Slack**, and choose the notification channel to notify from the drop-down menu.