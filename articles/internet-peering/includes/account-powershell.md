---
title: include file
titleSuffix: Azure
description: include file
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: include
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 3a5f7157ef8f3645dd03ec93684238dd8bbc067e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75773641"
---
Bevor Sie mit der Konfiguration beginnen, installieren und importieren Sie die erforderlichen Module. Zum Installieren der Module in PowerShell benötigen Sie Administratorberechtigungen.

1. Installieren und importieren Sie das Az-Modul.
    ```powershell
    Install-Module Az -AllowClobber
    Import-Module Az
    ```
1. Installieren und importieren Sie das Az.Peering-Modul.
    ```powershell
    Install-Module -Name Az.Peering -AllowClobber
    Import-Module Az.Peering
    ```
1. Überprüfen Sie mithilfe des folgenden Befehls, ob die Module ordnungsgemäß importiert wurden.
    ```powershell
    Get-Module
    ```
1. Melden Sie sich mithilfe des folgenden Befehls bei Ihrem Azure-Konto an.
    ```powershell
    Connect-AzAccount
    ```
1. Sehen Sie sich die Abonnements für das Konto an, und wählen Sie das Abonnement aus, in dem Sie ein Peering erstellen möchten.
    ```powershell
    Get-AzSubscription
    Select-AzSubscription -SubscriptionId "subscription-id"
    ```
1. Wenn Sie noch keine Ressourcengruppe besitzen, müssen Sie zuerst eine erstellen, bevor Sie ein Peering erstellen. Dazu können Sie den folgenden Befehl ausführen:

    ```powershell
    New-AzResourceGroup -Name "PeeringResourceGroup" -Location "Central US"
    ```
> [!IMPORTANT]
> Wenn Sie Ihr ASN und Ihr Abonnement noch nicht zugeordnet haben, führen Sie die Schritte zum [Zuordnen eines Peer-ASN](../howto-subscription-association-powershell.md) aus. Dies ist erforderlich, um ein Peering anzufordern.

> [!NOTE]
> Der Speicherort der Ressourcengruppe ist unabhängig von dem Speicherort, an dem Sie ein Peering einrichten möchten.
&nbsp;
