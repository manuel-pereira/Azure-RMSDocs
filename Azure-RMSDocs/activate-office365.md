---
# required metadata

title: Activate Azure RMS from the Microsoft 365 admin center - AIP
description: Learn how to activate the Azure Rights Management service from the Microsoft 365 admin center.
author: batamig
ms.author: bagol
manager: rkarlin
ms.date: 10/27/2020
ms.topic: how-to
ms.collection: M365-security-compliance
ms.service: information-protection
ms.assetid: a2b3e1a2-59a0-4191-bf4c-4485ae7a70a9

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.subservice: azurerms
ms.reviewer: esaggese
ms.suite: ems
#ms.tgt_pltfrm:
ms.custom: admin

---

# Activate Azure Rights Management protection from the Microsoft 365 admin center

>***Applies to**: [Azure Information Protection](https://azure.microsoft.com/pricing/details/information-protection), [Office 365](https://download.microsoft.com/download/E/C/F/ECF42E71-4EC0-48FF-AA00-577AC14D5B5C/Azure_Information_Protection_licensing_datasheet_EN-US.pdf)*
>
>***Relevant for**: [AIP unified labeling client and classic client](faqs.md#whats-the-difference-between-the-azure-information-protection-classic-and-unified-labeling-clients)*

[!INCLUDE [AIP classic client is deprecated - extended support customers](includes/classic-client-deprecation-extended-support.md)]

This article describes how global administrators with access to the Azure Rights Management service from the Microsoft 365 admin center can activate Azure Rights Management.

## Prerequisites

To activate the Azure Rights Management service, you must have either an [Azure Information Protection Premium plan](https://www.microsoft.com/cloud-platform/azure-information-protection-pricing) or an [Office 365 plan that includes Rights Management](https://download.microsoft.com/download/E/C/F/ECF42E71-4EC0-48FF-AA00-577AC14D5B5C/Azure_Information_Protection_licensing_datasheet_EN-US.pdf). For example, your organization has a plan for Office 365 E3 or Office 365 E5. 

If you have questions about the subscription requirements, or you need help activating the service, [contact Microsoft Support](information-support.md#to-contact-microsoft-support) or use your standard support channels.

## Activating Azure Rights Management

1. When you have confirmed that your organization has a plan that includes Azure Rights Management, go to the [Rights Management page](https://account.activedirectory.windowsazure.com/RmsOnline/Manage.aspx) in the Microsoft 365 admin center.
    
    If you are prompted to sign in, use an account that is a global administrator for Microsoft 365.
    
    > [!TIP]
    > For admin center help, see [About the Microsoft 365 admin center](/office365/admin/admin-overview/about-the-admin-center).
    
    If you prefer to navigate to the **rights management** page from the admin center: **Settings** > **Org settings** > **Services** tab > **Microsoft Azure Information Protection** > **Manage Microsoft Azure Information Protection settings**

2. On the **rights management** page, click **activate**.

3. When you see the message **Do you want to activate Rights Management?**, click **activate**.

You should now see **Rights management is activated** and the option to deactivate.

## Next steps

Resume reading [Activating the protection service from Azure Information Protection](activate-service.md#configuring-onboarding-controls-for-a-phased-deployment).

