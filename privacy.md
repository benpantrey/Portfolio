---
linkTitle: "Privacy Settings"
title: "Privacy Settings"
weight: "12"
description: "Sysdig facilitates customized privacy options, both for organizations and individuals. In the Privacy Settings page, you can opt in or out of sharing usage data with Sysdig. This may be required to comply with enterprise policy or security standards."
---

{{% infobox type="note" %}}

Opting-out of Data Analytics may prevent the usage of certain features, such as Okta MLand the "Chat with Us" button in the left navigation bar.

{{% /infobox %}}


## Review Privacy Settings

To review and modify your privacy settings:

1. From the user menu, navigate to **Settings** > **Privacy Settings**.

2. Toggle the sliders for the options listed below. Administrators can change data settings globally, while individual users can adjust the data being sent from their accounts.

### Global User Settings  

Sign in as an administrator to access these settings. You can enable sending **Usage Data** and **Crash Reporting** from all users back to Sysdig.

You can individually override this by opting out.

### Individual User Settings

Non-admin users can access these settings. If administrators have opted in through the global settings, individuals may opt out of sharing their own data using this option. You can enable or disable the sending of **Usage Data** and **Crash Reporting**.

Disabling **Usage Data** may hinder the **Chat with Us** button from reaching Customer Success. See [Sysdig Support]({{< ref "Support" >}}).

### Customer Data Analytics (Secure Only)

Sign in as an administrator to enable and disable the transfer of **Data Analytics**.

Note that this data is used for Machine Learning (ML) policies, such as Okta ML, AWS ML, and Workload ML. Opting out will prevent the usage of such policies.

This option is only available in Sysdig Secure.

### Sysdig Sage Message Collection

Sysdig may use Sysdig Sage messages and conversation data. Use the **Sysdig Sage Messages** toggle to enable or disable this collection on your account.

### Hard Opt-Out Option

Contact Sysdig Support to opt out of data sharing in a way that can't be overridden even by admins, if needed to comply with particular enterprise security standards.

{{% infobox type="note" %}}


If you are logged in to Sysdig Monitor as an administrator, you can see two pairs of sliders. The first pair are your individual user settings, and the second are the global settings.

{{% /infobox %}}
