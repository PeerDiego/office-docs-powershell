---
title: "Configure advanced settings via PowerShell for the Microsoft Purview Information Protection Client"
f1.keywords:
- NOCSH
ms.author: v-katykoenen
author: kmkoenen
manager: laurawi
ms.date: 04/11/2024
audience: Admin
ms.topic: article
ms.service: purview
ms.localizationpriority: medium
ms.collection: 
- tier1
- purview-compliance
search.appverid: 
- MOE150
- MET150
description: "Learn how to configure advanced settings for the Microsoft Purview Information Protection Client using PowerShell."
---

# Configure advanced settings via PowerShell for the Microsoft Purview Information Protection Client

## Label advanced settings

Use the *AdvancedSettings* parameter with [New-Label](powershell/module/exchange/new-label) and [Set-Label](/powershell/module/exchange/set-label).

- color
- customPropertiesByLabel
- DefaultSubLabelId
- labelByCustomProperties
- SMimeEncrypt
- SMimeSign

## Label policy advanced settings

Use the *AdvancedSettings* parameter with [New-Label](powershell/module/exchange/new-label) and [Set-Label](/powershell/module/exchange/set-label).

- AdditionalPPrefixExtensions
- AttachmentAction
- AttachmentActionTip
- color
- customPropertiesByLabel
- DefaultSubLabelId
- DisableMandatoryInOutlook
- EnableAudit
- EnableContainerSupport
- EnableCustomPermissions
- EnableCustomPermissionsForCustomProtectedFiles
- EnableGlobalization
- EnableLabelByMailHeader
- EnableLabelBySharePointProperties
- EnableOutlookDistributionListExpansion
- EnableRevokeGuiSupport
- EnableTrackAndRevoke
- HideBarByDefault
- JustificationTextForUserText
- labelByCustomProperties
- LogMatchedContent
- OfficeContentExtractionTimeout
- OutlookBlockTrustedDomains
- OutlookBlockUntrustedCollaborationLabel
- OutlookCollaborationRule
- OutlookDefaultLabel
- OutlookGetEmailAddressesTimeOutMSProperty
- OutlookJustifyTrustedDomains
- OutlookJustifyUntrustedCollaborationLabel
- OutlookOverrideUnlabeledCollaborationExtensions
- OutlookRecommendationEnabled
- OutlookSkipSmimeOnReadingPaneEnabled
- OutlookUnlabeledCollaborationActionOverrideMailBodyBehavior
- OutlookWarnTrustedDomains
- OutlookWarnUntrustedCollaborationLabel
- PFileSupportedExtensions
- PostponeMandatoryBeforeSave
- PowerPointRemoveAllShapesByShapeName
- PowerPointShapeNameToRemove
- RemoveExternalContentMarkingInApp
- RemoveExternalMarkingFromCustomLayouts
- ReportAnIssueLink
- RunPolicyInBackground
- ScannerConcurrencyLevel
- ScannerFSAttributesToSkip
- ScannerMaxCPU
- ScannerMinCPU
- SharepointFileWebRequestTimeout
- SharepointWebRequestTimeout
- SMimeEncrypt
- SMimeSign
- UseCopyAndPreserveNTFSOwner

### AdditionalPPrefixExtensions

The information protection client supports changing \<EXT>.PFILE to P\<EXT> by using the advanced property, **AdditionalPPrefixExtensions**. This advanced property is supported from the File Explorer, PowerShell, and by the scanner. All apps have similar behavior.   

- Key: **AdditionalPPrefixExtensions**

- Value: **\<string value>** 

Use the following table to identify the string value to specify:

| String value| Client and Scanner|
|-------------|---------------|
|\*|All PFile extensions become P\<EXT>|
|\<null value>| Default value behaves like the default protection value.|
|ConvertTo-Json(".dwg", ".zip")|In addition to the previous list, ".dwg" and ".zip" become P\<EXT>| 

With this setting, the following extensions always become **P\<EXT>**: ".txt", ".xml", ".bmp", ".jt", ".jpg", ".jpeg", ".jpe", ".jif", ".jfif", ".jfi", ".png", ".tif", ".tiff", ".gif"). Notable exclusion is that "ptxt" does not become "txt.pfile". 

**AdditionalPPrefixExtensions** only works if protection of PFiles with the advanced property - [**PFileSupportedExtension**](#pfilesupportedextension) is enabled. 

**Example 1**: PowerShell command to behave like the default behavior where Protect ".dwg" becomes ".dwg.pfile":

```PowerShell
Set-LabelPolicy -AdvancedSettings @{ AdditionalPPrefixExtensions =""}
```

**Example 2**:  PowerShell command to change all PFile extensions from generic protection (dwg.pfile) to native protection (.pdwg) when the files are protected:

```PowerShell
Set-LabelPolicy -AdvancedSettings @{ AdditionalPPrefixExtensions ="*"}
```

**Example 3**: PowerShell command to change ".dwg"  to ".pdwg" when using this service protect this file:

```PowerShell
Set-LabelPolicy -AdvancedSettings @{ AdditionalPPrefixExtensions =ConvertTo-Json(".dwg")}
```

### AttachmentAction

The information protection client supports apply a label that matches the highest classification for email attachments.

This setting is for when users attach labeled documents to an email, and do not label the email message itself. In this scenario, a label is automatically selected for them, based on the classification labels that are applied to the attachments. The highest classification label is selected.

The attachment must be a physical file, and cannot be a link to a file (for example, a link to a file on Microsoft SharePoint or OneDrive).

You can configure this setting to **Recommended**, so that users are prompted to apply the selected label to their email message, with a customizable tooltip. Users can accept the recommendation or dismiss it. Or, you can configure this setting to **Automatic**, where the selected label is automatically applied but users can remove the label or select a different label before sending the email.

> [!NOTE]
> When the attachment with the highest classification label is configured for protection with the setting of user-defined permissions:
> 
> - When the label's user-defined permissions include Outlook (Do Not Forward), that label is selected and Do Not Forward protection is applied to the email.
> - When the label's user-defined permissions are just for Word, Excel, PowerPoint, and File Explorer, that label is not applied to the email message, and neither is protection.
> 

To configure this advanced setting, enter the following strings for the selected label policy:

- Key 1: **AttachmentAction**

- Key Value 1: **Recommended** or **Automatic**

- Key 2: **AttachmentActionTip**

- Key Value 2: "\<customized tooltip>"

The customized tooltip supports a single language only.

Example PowerShell command, where your label policy is named "Global":

```PowerShell
Set-LabelPolicy -Identity Global -AdvancedSettings @{AttachmentAction="Automatic"}
```

### AttachmentActionTip

For details, see [AttachmentAction](#attachmentaction).

### color

Use this advanced setting to set a color for a label. To specify the color, enter a hex triplet code for the red, green, and blue (RGB) components of the color. For example, #40e0d0 is the RGB hex value for turquoise.

If you need a reference for these codes, you'll find a helpful table from the [\<color>](https://developer.mozilla.org/docs/Web/CSS/color_value) page from the MSDN web docs. You also find these codes in many applications that let you edit pictures. For example, Microsoft Paint lets you choose a custom color from a palette and the RGB values are automatically displayed, which you can then copy.

To configure the advanced setting for a label's color, enter the following strings for the selected label:

- Key: **color**

- Value: **\<RGB hex value>**

Example PowerShell command, where your label is named "Public":

```PowerShell
Set-Label -Identity Public -AdvancedSettings @{color="#40e0d0"}
```

### customPropertiesByLabel

Use this advanced setting to set a color for a label. To specify the color, enter a hex triplet code for the red, green, and blue (RGB) components of the color. For example, #40e0d0 is the RGB hex value for turquoise.

If you need a reference for these codes, you'll find a helpful table from the [\<color>](https://developer.mozilla.org/docs/Web/CSS/color_value) page from the MSDN web docs. You also find these codes in many applications that let you edit pictures. For example, Microsoft Paint lets you choose a custom color from a palette and the RGB values are automatically displayed, which you can then copy.

To configure the advanced setting for a label's color, enter the following strings for the selected label:

- Key: **color**

- Value: **\<RGB hex value>**

Example PowerShell command, where your label is named "Public":

```PowerShell
Set-Label -Identity Public -AdvancedSettings @{color="#40e0d0"}
```
### DefaultSubLabelId

When you add a sublabel to a label, users can no longer apply the parent label to a document or email. By default, users select the parent label to see the sublabels that they can apply, and then select one of those sublabels. If you configure this advanced setting, when users select the parent label, a sublabel is automatically selected and applied for them: 

- Key: **DefaultSubLabelId**

- Value: **\<sublabel GUID>**

Example PowerShell command, where your parent label is named "Confidential" and the "All Employees" sublabel has a GUID of 8faca7b8-8d20-48a3-8ea2-0f96310a848e:

```PowerShell
Set-Label -Identity "Confidential" -AdvancedSettings @{DefaultSubLabelId="8faca7b8-8d20-48a3-8ea2-0f96310a848e"}
```

### DisableMandatoryInOutlook

By default, when you enable the label policy setting of **All documents and emails must have a label**, all saved documents and sent emails must have a label applied. When you configure the following advanced setting, the policy setting applies only to Office documents and not to Outlook messages.

For the selected label policy, specify the following strings:

- Key: **DisableMandatoryInOutlook**

- Value: **True**

Example PowerShell command, where your label policy is named "Global":

```PowerShell
Set-LabelPolicy -Identity Global -AdvancedSettings @{DisableMandatoryInOutlook="True"}
```

### EnableAudit

By default, the Azure Information Protection unified labeling client supports central reporting and sends its audit data to:

- [Azure Information Protection analytics](../reports-aip.md), if you've configured a [Log Analytics workspace](https://azure.microsoft.com/pricing/details/log-analytics)
- Microsoft 365, where you can view them in the [Activity Explorer](/microsoft-365/compliance/data-classification-activity-explorer)

To change this behavior, so that audit data is not sent, do the following:

1. Add the following policy [advanced setting](#configuring-advanced-settings-for-the-client-via-powershell) using the Office 365 Security & Compliance Center PowerShell:

    - Key: **EnableAudit**

    - Value: **False**

    For example, if your label policy is named "Global":

    ```PowerShell
    Set-LabelPolicy -Identity Global -AdvancedSettings @{EnableAudit="False"}
    ```

    > [!NOTE]
    > By default, this advanced setting is not present in the policy, and the audit logs are sent.
    >

1. In all Azure Information Protection client machines, delete the following folder: **%localappdata%\Microsoft\MSIP\mip**

To enable the client to send audit log data again, change the advanced setting value to **True**. You do not need to manually create the **%localappdata%\Microsoft\MSIP\mip** folder again on your client machines.

### EnableContainerSupport

When you configure this setting, the  [PowerShell](./clientv2-admin-guide-powershell.md) cmdlet **Set-AIPFileLabel** is enabled to allow removal of protection from PST, rar, and 7zip files.

- Key: **EnableContainerSupport**

- Value: **True**

Example PowerShell command where your policy is enabled:

```PowerShell
Set-LabelPolicy -Identity Global -AdvancedSettings @{EnableContainerSupport="True"}
```

### EnableCustomPermissions

y default, users see an option named **Protect with custom permissions** when they right-click in File Explorer and choose **Classify and protect**. This option lets them set their own protection settings that can override any protection settings that you might have included with a label configuration. Users can also see an option to remove protection. When you configure this setting, users do not see these options.

To configure this advanced setting, enter the following strings for the selected label policy:

- Key: **EnableCustomPermissions**

- Value: **False**

Example PowerShell command, where your label policy is named "Global":

```PowerShell
Set-LabelPolicy -Identity Global -AdvancedSettings @{EnableCustomPermissions="False"}
```
### EnableCustomPermissionsForCustomProtectedFiles

When you configure the advanced client setting to [turn off custom permissions in File Explorer](#turn-off-custom-permissions-in-file-explorer), by default, users are not able to see or change custom permissions that are already set in a protected document.

However, there's another advanced client setting that you can specify so that in this scenario, users can see and change custom permissions for a protected document when they use File Explorer and right-click the file.

To configure this advanced setting, enter the following strings for the selected label policy:

- Key: **EnableCustomPermissionsForCustomProtectedFiles**

- Value: **True**

Example PowerShell command:

```PowerShell
Set-LabelPolicy -Identity Global -AdvancedSettings @{EnableCustomPermissionsForCustomProtectedFiles="True"}
```

### EnableGlobalization

Use the **EnableGlobalization** setting to  inrease the detection accuracy of your sensitive infomration types, including increased accuracy for East Asian languages and support for double-byte characters. These enhancements are provided only for 64-bit processes, and are turned off by default.

Turn on these features for your policy specify the following strings:

- Key: **EnableGlobalization**

- Value: `True`

Example PowerShell command, where your label policy is named "Global":

```PowerShell
Set-LabelPolicy -Identity Global -AdvancedSettings @{EnableGlobalization="True"}
```

To turn off support again and revert to the default, set the **EnableGlobalization** advanced setting to an empty string.

### EnableLabelByMailHeader

You can use the configuration you've defined with the [*labelByCustomProperties*](#migrate-labels-from-secure-islands-and-other-labeling-solutions) advanced setting for Outlook emails, in addition to Office documents, by specifying an additional label policy advanced setting. 

However, this setting has a known negative impact on the performance of Outlook, so configure this additional setting only when you have a strong business requirement for it and remember to set it to a null string value when you have completed the migration from the other labeling solution.

To configure this advanced setting, enter the following strings for the selected label policy:

- Key: **EnableLabelByMailHeader**

- Value: **True**

Example PowerShell command, where your label policy is named "Global":

```PowerShell
Set-LabelPolicy -Identity Global -AdvancedSettings @{EnableLabelByMailHeader="True"}
```

### EnableLabelBySharePointProperties

[*labelByCustomProperties*](#migrate-labels-from-secure-islands-and-other-labeling-solutions)

This setting is supported when you use Word, Excel, and PowerPoint.

To configure this advanced setting, enter the following strings for the selected label policy:

- Key: **EnableLabelBySharePointProperties**

- Value: **True**

Example PowerShell command, where your label policy is named "Global":

```PowerShell
Set-LabelPolicy -Identity Global -AdvancedSettings @{EnableLabelBySharePointProperties="True"}
```
### EnableOutlookDistributionListExpansion

To extend support from other advanced settings to recipients inside Outlook distribution lists, set the **EnableOutlookDistributionListExpansion** advanced setting to **true**.

- Key: **EnableOutlookDistributionListExpansion**
- Value: **true**

For example, if you've configured the [OutlookBlockTrustedDomains](#to-implement-the-warn-justify-or-block-pop-up-messages-for-specific-labels), [OutlookBlockUntrustedCollaborationLabel](#implement-pop-up-messages-in-outlook-that-warn-justify-or-block-emails-being-sent) advanced settings, and then also configure the **EnableOutlookDistributionListExpansion** setting, Outlook is enabled to expand the distribution list to ensuring that a block message appears as needed.

The default timeout for expanding the distribution list is **2000** milliseconds.

To modify this timeout, create the following advanced setting for the selected policy:

- Key: **OutlookGetEmailAddressesTimeOutMSProperty**
- Value: *Integer, in milliseconds*

Example PowerShell command, where your label policy is named "Global":

```PowerShell
Set-LabelPolicy -Identity Global -AdvancedSettings @{
  EnableOutlookDistributionListExpansion="true"
  OutlookGetEmailAddressesTimeOutMSProperty="3000"
}
```

### EnableRevokeGuiSupport

If you do not want end-users to have the ability to revoke access to protected documents from their Office apps, you can remove the **Revoke Access** option from your Office apps.

> [!NOTE]
> Removing the **Revoke Access** option continues to keep your protected documents tracked in the background, and retains the admin ability to revoke access to documents [via PowerShell](/powershell/module/aipservice/set-aipservicedocumentrevoked).
> 

For the selected label policy, specify the following strings:

- Key: **EnableRevokeGuiSupport**

- Value: **False**

Example PowerShell command, where your label policy is named "Global":

```PowerShell
Set-LabelPolicy -Identity Global -AdvancedSettings @{EnableRevokeGuiSupport="False"}
```

### EnableTrackAndRevoke

By default, document tracking features are turned on for your tenant. To turn them off, such as for privacy requirements in your organization or region, set the **EnableTrackAndRevoke** value to **False**.

Once turned off, document tracking data will not longer be available in your organization, and users will no longer see the [**Revoke**](revoke-access-user.md#revoke-access-from-microsoft-office-apps) menu option in their Office apps.

For the selected label policy, specify the following strings:

- Key: **EnableTrackAndRevoke**

- Value: **False**

Example PowerShell command, where your label policy is named "Global":

```PowerShell
Set-LabelPolicy -Identity Global -AdvancedSettings @{EnableTrackAndRevoke="False"}
```

After setting this value to **False**, track and revoke is turned off as follows: 

- Opening protected documents with the AIP unified labeling client no longer registers the documents for track and revoke.
- End users will no longer see the [**Revoke**](revoke-access-user.md#revoke-access-from-microsoft-office-apps) menu option in their Office apps.

However, protected documents that are already registered for tracking will continue to be track, and administrators can still revoke access from PowerShell. To fully turn off track and revoke features, also run the [Disable-AipServiceDocumentTrackingFeature](/powershell/module/aipservice/disable-aipservicedocumenttrackingfeature) cmdlet.

This configuration uses a policy [advanced setting](#configuring-advanced-settings-for-the-client-via-powershell) that you must configure by using Office 365 Security & Compliance Center PowerShell.

> [!TIP]
> To turn track and revoke back on, set the **EnableTrackAndRevoke** to **True**, and also run the [Enable-AipServiceDocumentTrackingFeature](/powershell/module/aipservice/enable-aipservicedocumenttrackingfeature) cmdlet.
>

### HideBarByDefault

By default, users must select the **Show Bar** option from the **Sensitivity** button to display the Information Protection bar in Office apps. Use the **HideBarByDefault** key and set the value to **False** to automatically display this bar for users so that they can select labels from either the bar or the button. 

For the selected label policy, specify the following strings:

- Key: **HideBarByDefault**

- Value: **False**

Example PowerShell command, where your label policy is named "Global":

```PowerShell
Set-LabelPolicy -Identity Global -AdvancedSettings @{HideBarByDefault="False"}
```

### JustificationTextForUserText

Customize the justification prompts that are displayed in both Office and the AIP client, when end users change classification labels on documents and emails.

For example, as an administrator, you may want to remind your users not to add any customer identifying information into this field:

:::image type="content" source="../media/justification-office.png" alt-text="Customized justification prompt text":::

To modify the default **Other** text that's displayed, use the **JustificationTextForUserText** advanced property with the [Set-LabelPolicy](/powershell/module/exchange/set-labelpolicy) cmdlet. Set the value to the text you want to use instead.

Sample PowerShell command, when your label policy is named "Global":

``` PowerShell
Set-LabelPolicy -Identity Global -AdvancedSettings @{JustificationTextForUserText="Other (please explain) - Do not enter sensitive info"}
```

### labelByCustomProperties

The *labelByCustomProperties* setting can be used in a number of scenarios, as described in the following examples.

#### Migrate labels from Secure Islands and other labeling solutions

This configuration is not compatible with protected PDF files that have a .ppdf file name extension. These files cannot be opened by the client using File Explorer or PowerShell.

For Office documents that are labeled by Secure Islands, you can relabel these documents with a sensitivity label by using a mapping that you define. You also use this method to reuse labels from other solutions when their labels are on Office documents. 

As a result of this configuration option, the new sensitivity label is applied by the information protection client as follows:

- **For Office documents**: When the document is opened in the desktop app, the new sensitivity label is shown as set and is applied when the document is saved.

- **For PowerShell**: [Set-FileLabel](https://go.microsoft.com/fwlink/?linkid=2259827) and [Set-FileLabel -Autolabel](https://go.microsoft.com/fwlink/?linkid=2259827) can apply the new sensitivity label.

- **For File Explorer**: In the Azure Information Protection dialog box, the new sensitivity label is shown but isn't set.

This configuration requires you to specify an advanced setting named **labelByCustomProperties** for each sensitivity label that you want to map to the old label. Then for each entry, set the value by using the following syntax:

```PowerShell
[migration rule name],[Secure Islands custom property name],[Secure Islands metadata Regex value]
```

Specify your choice of a migration rule name. Use a descriptive name that helps you to identify how one or more labels from your previous labeling solution should be mapped to sensitivity label.

Note that this setting does not remove the original label from the document or any visual markings in the document that the original label might have applied. To remove headers and footers, see [Remove headers and footers from other labeling solutions](#remove-headers-and-footers-from-other-labeling-solutions).

Examples:

- [Example 1: One-to-one mapping of the same label name](#example-1-one-to-one-mapping-of-the-same-label-name)
- [Example 2: One-to-one mapping for a different label name](#example-2-one-to-one-mapping-for-a-different-label-name)
- [Example 3: Many-to-one mapping of label names](#example-3-many-to-one-mapping-of-label-names)
- [Example 4: Multiple rules for the same label](#example-4-multiple-rules-for-the-same-label)

For additional customization, see:

- [Extend your label migration rules to emails](#extend-your-label-migration-rules-to-emails)
- [Extend your label migration rules to SharePoint properties](#extend-your-label-migration-rules-to-sharepoint-properties)

> [!NOTE]
> If you are migrating from your labels across tenants, such as after a company merger, we recommend that you read our [blog post on mergers and spinoffs](https://techcommunity.microsoft.com/t5/microsoft-security-and/mergers-and-spinoffs/ba-p/910455) for more information.
>
