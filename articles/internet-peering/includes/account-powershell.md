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
ms.openlocfilehash: beffb2babefd86c2807e21e9337cba66f42fcfc2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "81678487"
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
1. Überprüfen Sie mithilfe des folgenden Befehls, ob die Module ordnungsgemäß importiert wurden:
    ```powershell
    Get-Module
    ```
1. Melden Sie sich mit diesem Befehl bei Ihrem Azure-Konto an:
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
> Wenn Sie Ihre ASN und Ihr Abonnement noch nicht zugeordnet haben, führen Sie die Schritte zum [Zuordnen einer Peer-ASN](../howto-subscription-association-powershell.md) aus. Diese Aktion ist erforderlich, um ein Peering anzufordern.

> [!NOTE]
> Der Speicherort einer Ressourcengruppe ist unabhängig vom Speicherort, an dem Sie ein Peering einrichten möchten.
&nbsp;
