---
linkTitle: "Notification Channels"
title: "Manage Notification Channels"
description: "You can configure alerts to send through various supported notification channels. Alerts are triggered in Sysdig Monitor when event thresholds are crossed, while in Sysdig Secure, they can be linked to runtime policies and actions in Automations. This page explains how to configure and manage notification channels, and how to disable notifications when they are not needed, such as during scheduled downtime."
---

## Supported Notification Channels

The tables below show which notification channel types are supported in Sysdig Secure and Sysdig Monitor, and for which use cases within each product.

### Supported Channels in Sysdig Secure


| Notification Channel | Threat Detection Policies | Vulnerability Policies | Automations | Reports |
| -------------------- | ------- | ------------------------- | ------------ | --- | --- |
| Amazon SNS           | X       |   |       |     |
| Email                | X       | X |    X  | X    |
| Microsoft Teams      | X       | X |   X    |     |
| OpsGenie             | X       |   |       |     |
| PagerDuty            | X       |   |   X    |    |
| Prometheus Alertmanager  | X   |   |       |     |
| Slack                | X       | X |    X   |  X   |
| Sysdig Team Email    | X       |   |       |     |
| VictorOps            | X       |   |       |     |
| Webhook              | X       | X |   X    |     |

### Supported Channels in Sysdig Monitor

| Notification Channel | Alerts | Silence Rule Notifications | Cost Advisor Reports |
| -------------------- | ------ | ------------------------- | ------------ |
| Amazon SNS           | X  |   |       |     |
| Custom Webhook       | X  |   |       |     |
| Email                | X  |   |       |     |
| Google Chat          | X  |   |       |     |
| IBM Cloud Functions  | X  |   |       |     |
| IBM Event Notifications | (IBM Only)   |   |       |     |
| Microsoft Teams      | X  |   |       |     |
| OpsGenie             | X  |   |       |     |
| PagerDuty            | X  |   |       |     |
| Prometheus Alertmanager | X  |   |      |     |
| Slack                | X  | X |    X   |  X   |
| Sysdig Team Email    | X  |   |       |     |
| VictorOps            | X  |   |       |     |
| Webhook              | X  |   |       |     |

## Rate Limits

Sysdig provides rate limiting of incoming notification requests in order to prevent overloading of the destination channels. 

Rate limiting applies to all notification channels and counts notifications on a per customer, per notification channel type, per notification type basis. Hitting a limit on one channel won't affect the limit of other channels, even if they are the same type (for example, Slack) and owned by the same customer.

If the number of notifications triggering exceeds the per-minute or per-hour limit, the excess notifications are discarded.

Beyond Sysdig's own rate limit mechanism described here, third party services may apply their own rate limit mechanism. Hitting this limit may be reflected in the notification response. For example:
- [Jira](https://developer.atlassian.com/cloud/jira/platform/rate-limiting/) and Slack send a `429` error code.
- Email sends `421` or `450` error code. 
- Webhook sends an HTTP `429` code.

For more details, consult the documentation for the relevant third-party service.

On-prem userse can use standard configuration to configure limits per environment. 
The default limits are:

| Channel | Type | Per minute limit | Per hour limit |
| ------- | ---- | ---------------- | -------------- |
| Any     | Any  |  1,200           |   20,000       |
| Email   | Any  |  1,000           |  20,000        |
| Email   | Monitor Alerts | 1,200  |  72,000        |
| Email   | Runtime Policy |   500  |  1,000         | 
| Email   | New Findings* |    500  |  1,000         |
| PagerDuty  | Monitor Alerts | 1,200 | 30,000       |
| Custom Webhook | Monitor Alerts | 1,200 | 50,000   |
| Slack   | Monitor Alerts | 9,000 |  500,000        |
| Slack   | Runtime Policy | 500   |  1,000          |
| Slack   | New Findings* |  500   |   1,000         |
| Team Email | Any |  1,000        | 20,000          |
| Team Email  |  Runtime Policy | 500 |  1,000       |
| Team Email  |  New Findings* | 500 |  1,000        |
| Webhook | Monitor Alerts | 5,000 |   300,000       |

* *New Findings* refers to Vulnerability Findings.

## Control Access to Channels

Notification channel management can be fine-tuned by role-based access as follows:

-   Notification channels can be global or limited to a particular team.

-   Global channels can be managed by admins and can be viewed/used by
    other roles, while team-limited channels are available only to team
    members.

-   *Team Manager* , *Advanced User*, and *Service Manager* (Secure) roles can create/update/delete team-scoped notification channels. They can also read and use the global ones.

-   *Standard* and *View Only* roles can read team-limited and global notification channels.

-   Admins can create global notification channels and migrate channels from "global" to "team-limited", and also from one team to another.

## Add a Notification Channel

To add a new notification channel for alerting:

1. Log in to Sysdig Monitor or Sysdig Secure as Administrator.

2. Select **Integrations** > **Notification Channels**.

    The Notifications main page is displayed.

3. Select **Add Notification Channel**.

4.  Follow the channel-specific steps to complete the configuration process.

## Share a Notification Channel

When you configure a notification channel as Administrator, you can choose whether the channel should be **Shared With** the **Current Team** you are logged in as, or with **All Teams**.

Notification channels created by one team cannot be shared with just with one other team. To use a channel created by another team, ensure the channel is **Shared With** **All Teams** in the channel configuration.

When a non-Admin user creates a notification channel, it can only be shared with their **Current Team**.

Notification channels that have a team assigned cannot be converted to be shared with **All Teams**. Instead, you should create a new channel to be shared with **All Teams**.

## Edit a Notification Channel

To edit a notification channel:

1.  Log in to Sysdig Monitor or Sysdig Secure as Administrator.

2.  Select **Integrations** > **Notification Channels**.

3.  Click the target channel.

4.  Make the edits and click **Save**.

## Test a Notification Channel

To test a notification channel:

1.  Log in to Sysdig Monitor or Sysdig Secure as Administrator.

2.  Select **Integrations** > **Notification Channels**.

3.  Select the three dots next to a created Notification Channel and click **Test Channel**.

{{% infobox type="note" %}}

If a notification is not received within 10 minutes, the notification
channel is not working, and the configuration should be reviewed.

{{% /infobox %}}

## Report Unsuccessful Notification Attempts

When an unsuccessful notification has been attempted on a given notification channel, [Sysdig Events](/en/docs/sysdig-monitor/events/event-types/#sysdig-events) are generated to warn you about it. At the fifth failed notification attempt, the notification channel will be disabled and a corresponding Sysdig Event will be generated. To view the list of Sysdig Events:

1. Log in to Sysdig Monitor and select **Events**.

2. On the **Events** page, select **Sysdig** from the **All Types** drop-down.

## Disable a Notification Channel

Sometimes, a notification channel has outlived its use, or must be temporarily disabled due to noise while an underlying issue is investigated.

To temporarily disable a notification channel:

1. Log in to Sysdig Monitor or Sysdig Secure as Administrator.

2. Select **Integrations** > **Notification Channels**.

3. Identify a channel, and toggle the **Enabled** slider off.

To re-enable the channel, toggle the slider on.

## Delete a Notification Channel

1. Log in to Sysdig Monitor or Sysdig Secure as Administrator.

2. Select **Integrations** > **Notification Channels**.

3. Select the three-dot menu icon on the right side of a channel listing.

3. Select **Delete Channel**.

## Configure an Alert Start-Up Delay (On-Premises Only)

In Sysdig Monitor, alert jobs begin immediately at start-up. However, in instances where Sysdig goes down unexpectedly, or without proper shutdown/startup procedures implemented, data can be missing, triggering alert notifications.

A start-up delay in alert jobs can be configured in on-premises
environments, by setting the `draios.alerts.startupDelay` parameter. The
parameter requires a duration value; the example below shows a duration
of 10 minutes:

```yaml
draios.alerts.startupDelay=10m
```

This parameter can be configured for Kubernetes
environments:

- For Kubernetes environments, add the parameter to the `sysdigcloud.jvm.worker.options` parameter in the `configmap`.