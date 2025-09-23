---
linkTitle: "Manage Threat Detection Rules"
title: "Manage Threat Detection Rules"
weight: "2"
description: "Rules are the fundamental building blocks you use to compose your security policies. This page guides you through the rule library, and how to create and manage rules."
---

## Access the Rules Library

The Rules Library includes all created rules which can be referenced in policies. Out of the box, it provides a comprehensive runtime security library with container-specific rules (and predefined policies) developed by Sysdig Threat Research team, Falco open source community rules, and international security benchmarks such as [CIS](https://www.cisecurity.org/cis-benchmarks/) or [MITRE ATT&CK](https://attack.mitre.org/).

To access the Rules Library:
1. Log in to Sysdig Secure.
2. Select **Policies** > **Rules** > **Rules Library**.

## Navigate the UI

- Groups: Rules are grouped by source and you can collapse or open the groupings as needed.

### Understand the Columns

You can examine rules through the columns on the rules table:

- **Usage**: Shows the number of policies where the rule is used, and whether the policies are enabled. Click the rule to see the policy names in the Rule Detail panel.

- **Type**: The policy type to which the rule refers to. Corresponds to the option you select in the **Select rule source** dropdown.

- **Managed Type**: The managed type indicates who published the rule. Default (Falco) rules, published by Sysdig appear as **Managed**. Rules created by users in the Secure UI appear as **Custom**.

- **Modified**: This column indicates whether the rule has been modified through an append or an exception. See [Edit a Rule](#edit-a-rule) and [Rule Exceptions](#rule-exceptions).

- **Last Updated:** When the rule was last updated. Badges highlight rules that were added or changed in the past seven days.  

- **Tags**: Rules are categorized by tags, so you can group them by functionality, security standard, target, or whatever schema makes sense for your organization. Various tags are predefined and can help you organize rules into logical groups when creating or editing policies. Click the colored tag boxes to filter by tags.

### Available Filters

You can use various filters to find rules in the Rules Library. Select a term from the drop-down, or enter custom text. 

You can modify the expression of each filter. Available expressions include `in` (include), `not in`, `equal to` (exact match) and `different than`.

The available filters include:

- **Rule Name**: The name of the rule.

- **Type**: The policy type. For example, **List Matching** or **Kubernetes Audit**.

- **Usage**: Find rules that are **Enabled**, **Disabled** or **Not Used** in a policy.

- **Managed Type**: Indicates whether the rule was created by Sysdig (**Managed**) or created by a user in the UI (**Custom**).

- **MITRE Tags**: Filter by a specific MITRE tag. These are labels added to rules to indicate their alignment with tactics from the MITRE ATT&CK framework.

- **Tags**:  Filter items by tag by typing them into this field, selecting them from the drop-down, or selecting the colored tag boxes from the **Tags** column.

You can also filter with the buttons **New**, **Updated** and **Modified** (edited with an exception or append).

Select the **Reset** button at any time to arrange the currently applied filters.

## Review a Rule Detail Panel

From the Rules Library list, select a rule to see its details.

From this panel, you can see:

- **What**: Review the rule definition, including clicking embedded macros to open their details in a pop-up window.
- **Usage**: Check all policies in which the rule is used and see whether those policies are enabled or disabled.
- **Tags**: Tags attached to this rule.

- Select **Edit** to modify the rule and **Duplicate** to create a copy of the rule. You can use this copy to edit freely, for example. rule diff icon to compare the two versions of the rule.

## View Recent Changes to a Rule

When rules are changed, either by the Sysdig Threat Detection team or by users, an **Updated** badge is displayed next to the rule name for seven days.

There are several ways to view recent changes to a rule.

### From the Rules Library

1. Use the **Review changes** filter to find a list of recently changed rules. From the drop-down, you can find the rules changed by Sysdig, Tuner, or you.

2. Open the detail panel and select **View as Diff** to check changes made to a rule.

3. Select **View as Default** to return to the default view.

4. You can select another row without closing the rule diff window to see changes to that rule.

## Add Rules

You can select existing rules from the Library or create new rules on the fly and add them to a policy.

The Policy Editor interface provides many flexible ways to add rules to or remove rules from a Policy - the following steps describe one of these methods.

### Import from Library

1. From Sysdig Secure, click **Policies** > **Threat Detection** > **Runtime Policies**.

2. Click **Add Policy** in the top right corner, and select the policy type.

   Do not choose **Container Drift**, **Machine Learning**, or **AWS ML**, as these types do not support imports from the rules library.

   The **New Policy** page opens.

3. From the **New Policy** (or Edit Policy) page, click **Manage & Import**.

   The **Import from Rules Library** page appears.

4. Select the checkboxes by the rules to import.

   You can pre-sort a collection of rules by searching for particular keywords or tags, or clicking a colored tag icon.

   You can filter the list for rules you have already selected with the **Selected** button.

5. Select **Import Rules**.

   The Policy page with the selected rules listed appears.

   To remove a rule from a Policy, click the X next to the rule in the list.

6. Fill out the remaining fields and **Save** your policy.

### Create a List-Matching Rule: Container Type Example

Suppose you want detect whenever someone uses a specific container image that has known problems. In this case, a Container rule would be appropriate. To create this rule:

1. Log in to Sysdig Secure, and navigate to **Policies** > **Rules** > **Rules Library**.

2. From the **Rules Library** page, click **+Add Rule** and select **Container** from the drop-down.

   The **New Rule** page for the Container rule type is displayed.

3. Enter the parameters:

   **Name:** Enter a name, such as "Problematic Images".

   **Description:** Enter a Description, for example, "Images that shouldn't be used".

   **If Matching/ If Not Matching:** Select **If Matching**. When added to a policy, if the rule conditions match, then the policy action you define (such as "send notification") is triggered.

   **Containers:** Add the container names that are problematic, for example: `cassandra:3.0.23`.

   **Tags:** Select relevant tags from the dropdown, for example: **database** and **container**.

4. Click **Save**.

The other list-matching rule types have similar entry fields, as appropriate to their type.

## Edit a Rule

Rules published by Sysdig are default and read-only. You can append their lists and macros, to craft exceptions, but you cannot edit or delete the core parameters.

You can freely edit any rules you create.

To display existing rules:

1. Select **Policies** > **Rules** > **Rules Library** and select a rule.

2. The Rule Details panel opens on the right. You can review the parameters and [append to macros and lists](#append-to-falco-macros-and-lists) inline.

3. Select **Edit** to open the Rule Details page.

### Falco Rules

For information on writing and editing rules, consult the Falco rules documentation:

- [Falco.org](Falco.org): Fields and details for writing rules are documented in Falco docs.
- [How to use operators](https://falco.org/docs/concepts/rules/conditions/#comparison-operators).
- [How to use exceptions](https://falco.org/docs/concepts/rules/exceptions/).

{{% infobox type="warning" %}}

Sysdig does not support [Overriding Rules](https://falco.org/docs/concepts/rules/overriding/). If you use one of these markers in the YAML, your rule will be rejected.

{{% /infobox %}}

To learn which fields are supported by Sysdig Secure, see the [Fields Library](/en/docs/sysdig-secure/rule-fields-library/).


### View and Edit Rules with YAML

You can edit exceptions and custom rules, and view rules in YAML format through the Rules Library:

1. Log in to Sysdig Secure.

2. Select **Policies** > **Rules** > **Rules Library**.

3. Select a rule.

   The Rules Detail panel appears on the right.

4. Select the edit (pencil) icon.

   The Rules Detail page opens.

5. Select **View as YAML**.

   The YAML editor opens.

6. Edit the rule in YAML format:
   - You can edit any part of a custom rule.
   - You can only edit appends/exceptions for default rules.

7. To save your changes, select **Save**.

### Append to Falco Macros and Lists

The default Falco rules have a variety of macros and lists embedded in them. You cannot delete these from a default rule, but can append additional information to them.

For example, consider the Policy `DB Program Spawned Process` in the image above. The embedded rule is used to check that databases have not spawned illicit processes. You can see the rule condition in the `db_server_binaries` Falco list.

{{% infobox type="note" %}}

You can append to the condition and output of a managed rule, but you cannot append to tags, description, and rule type.

{{% /infobox %}}

#### Append Items to the List

1. Click the text in the rule condition, or go to **Policies** > **Rules** > **Falco Lists** and search for it by name.

2. The list content appears. Click **Edit Append**.

    <img src="/image/rule_db_condition.png" width="500" />

3. Enter the additional items (for example, databases) you want to include in the rule and click **Save**.

#### Append Conditions to the Macro

1. Click the blue macro text in the rule condition, or go to **Policies > Falco Macros** and search for it by name.

    <img src="/image/macros_list.png" width="500" />

2. The macro content appears. Click **Append**.

    <img src="/image/macro_append.png" width="500" />

3. Enter the additional conditions you want to include with prepending logical `or` or `and` operators and click **Save**.

    <img src="/image/macro_save.png" width="500" />

### How to Use the Rules Editor

The Rules Editor lets you freely create custom Falco rules, lists,
and macros and can override the behavior of the defaults.

#### Understand the Interface

To access the Rules Editor interface, select **Policies** > **Rules Editor**:

![](/image/rules_editor.png)

**The Right Panel (Default)**

Displays the `rules_yamls` provided by Sysdig.

- Contains the default rules and macros

- Is read-only

**The Left Panel (Custom)**

Displays the custom rules and overrides you want to add to the selected
`rules_yaml`.

Note that many default Falco rules and macros have a parallel `placeholder` entry (commented out) in the YAML file. These have the prefix `user_known`. To change the behavior of a default rule, copy the placeholder equivalent into the custom rules panel and edit it there, rather than editing the default rule directly.

**To search the rules YAML files**

Click inside the Rules Editor right panel and use `CTRL + F` (in Windows. Use `Command + F` in Mac) to open an internal search field.

## Rule Exceptions

To reduce false positives, Sysdig uses Falco exceptions in many of the default rules, including adding exceptions to community rules. Rule exceptions are used as part of Runtime Policy Tuning.

To understand how rule exceptions in Sysdig build upon and differ from Falco, see [About Rule Exceptions](/en/sysdig-secure/threat-detection-policies/#about-rule-exceptions). You can manage rule exceptions in the Rules Editor UI or using YAML.

### Manage Rule Exceptions with the UI

The exception builder gives users an easy way to apply or add new exceptions to managed or custom rules.

#### Modify an Existing Exception

For managed rules, you:

- Cannot modify or remove default exceptions
- Can append new exceptions
- Modify any appended exceptions

After you define an exception with the **fields** (`container.name`) and **comparisons** (`endswith`) you cannot change, add, or remove it.

![](/image/exceptions-ui-modify.gif)

#### Create a Custom Exception

For managed and custom rules:

- You can create new exceptions.
- Just as with any of the exceptions, once you have applied an exception to a rule, you can only modify the values fields. You cannot modify or add to the fields or comps.

![](/image/exceptions-ui-create.png)

#### Apply Exceptions

After each creation, deletion, or modification of an exception, select the **Apply** button to verify the action.
![](/image/exceptions-ui-delete.gif)

### Manage Rule Exceptions with YAML

#### Define a New Exception

You can define a custom exception to configure optional values. This is useful when the values are not yet known. It also provides the policy tuner with fields to suggest new values for.

```
- rule: my custom rule
...
  exceptions:
    - name: cmdline_writer
      fields: [proc.cmdline, fd.directory]
      comps: [startswith, =]
```

#### Append Values to an Exception

You can append values to the exception by specifying the rule name and the exception name, without redefining the entire exception.

```
- rule: my custom rule
...
  exceptions:
    - name: cmdline_writer
      values: [httpd, /etc/shadow]
  append: true
```

After appending, the rule with the full exception now reads as:

```
- rule: my custom rule
...
  exceptions:
    - name: cmdline_writer
      fields: [proc.cmdline, fd.directory]
      comps: [startswith, =]
      values: [httpd, /etc/shadow]
```

## Use Cases: List-Matching Rules

It is more helpful to think of the rules as matching the activity, rather than using concepts of allowing or denying. The use cases are based on answering the question: What do I want to know?

**I WANT TO KNOW...**

**when any process other than web server programs are run:**

- Rule Type: `Process`

- `If Not Matching`

- Entries: `[apache, httpd, nginx]`

**if any of the following crypto-mining processes are run:**

- Rule Type: `Process`

- `If Matching`

- Entries:`[minerd, ccminer]`

**if any program reads any file containing password-related
information:**

- Rule Type: Filesystem

- Read Operations: `If Matching`

- Entries:
    `/etc/shadow, /etc/sudoers, /etc/pam.conf, /etc/security/pwquality.conf`

**if any program writes anywhere below binary directories:**

- Rule Type: `Filesystem`

- Read/Write Operations: `If Matching`

- Entries:`/usr, /usr/bin, /bin`

**if a program writes to anywhere other than /var/tmp:**

- Rule Type: `Filesystem`

- Read/Write Operations: `If Not Matching`

- Entries: `/var/tmp`

**if any container with an image from docker.io is started:**

- Rule Type: `Container`

- `If Matching`

- Entries: `[docker.io/]`

**if any container runs an Apache web server:**

- Rule Type: `Container`

- `If Matching`

- Entries: `[httpd, amd64/httpd]`

**I want to know if any container with a non-database image is started:**

- Rule Type: `Container`

- `If Not Matching`

- Entries `[percona/percona-server, mysql, postgres]`

**if any program accepts an inbound ssh connection:**

- Rule Type: Network

- Tcp,`"If Matching"`

- Entries: `[22]`

**if any program receives a DNS datagram:**

- Rule Type: Network

- UDP, `"If Matching"`

- Entries: `[53]`

**if any program accepts a connection on a port other than http/https**

- Rule Type: Network

- TCP, `"If Not Matching"`

- Entries: `[80, 443]`

**if any program accepts any inbound connection:**

- Rule Type: `Network`

- Inbound Connection: `Deny`

**if any program makes any outbound connection**

- Rule Type: `Network`

- Outbound Connection: `Deny`
