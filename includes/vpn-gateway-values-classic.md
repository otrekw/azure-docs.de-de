---
title: include file
description: Includedatei
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/08/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 11d2172d085fe9b47587f4084908f99d7b54437e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "92103236"
---
Beim Erstellen von klassischen VNETs im Azure-Portal ist der angezeigte Name nicht der vollständige Name, den Sie für PowerShell verwenden. Beispielsweise kann ein VNET, das im Azure-Portal als **TestVNet1** angezeigt wird, in der Netzwerkkonfigurationsdatei einen viel längeren Namen haben. Für ein VNET in der Ressourcengruppe „ClassicRG“ kann der Name wie folgt aussehen: **Gruppe ClassicRG TestVNet1**. Bei der Erstellung Ihrer Verbindungen ist es wichtig, dass Sie die in der Netzwerkkonfigurationsdatei angezeigten Werte verwenden.

In den folgenden Schritten stellen Sie eine Verbindung zu Ihrem Azure-Konto her. Zudem laden Sie die Netzwerkkonfigurationsdatei herunter und zeigen diese an, um die für Ihre Verbindungen erforderlichen Werte abzurufen.

1. Laden Sie die aktuelle Version der PowerShell-Cmdlets der Azure-Dienstverwaltung herunter, und installieren Sie sie. In den meisten Bereitstellungen sind die Resource Manager-Module lokal installiert, sie verfügen aber nicht über Dienstverwaltungsmodule. Bei Dienstverwaltungsmodulen handelt es sich um Legacy-Software, daher müssen sie separat installiert werden. Weitere Informationen finden Sie unter [Installieren von Cmdlets der Dienstverwaltung](/powershell/azure/servicemanagement/install-azure-ps).

1. Öffnen Sie die PowerShell-Konsole mit erhöhten Rechten, und stellen Sie eine Verbindung mit Ihrem Konto her. Verwenden Sie die folgenden Beispiele, um eine Verbindung herzustellen. Sie müssen diese Befehle lokal mit dem PowerShell-Dienstverwaltungsmodul ausführen. Stellen Sie eine Verbindung mit Ihrem Konto her. Verwenden Sie das folgende Beispiel, um eine Verbindung herzustellen:

   ```powershell
   Add-AzureAccount
   ```
1. Überprüfen Sie die Abonnements für das Konto.

   ```powershell
   Get-AzureSubscription
   ```
1. Wenn Sie über mehr als ein Abonnement verfügen, wählen Sie das Abonnement aus, das Sie verwenden möchten.

   ```powershell
   Select-AzureSubscription -SubscriptionId "Replace_with_your_subscription_ID"
   ```
1. Erstellen Sie ein Verzeichnis auf Ihrem Computer. Beispiel: C:\AzureVNet
1. Exportieren Sie die Netzwerkkonfigurationsdatei in das Verzeichnis. In diesem Beispiel wird die Netzwerkkonfigurationsdatei in das Verzeichnis **C:\AzureNet** exportiert.

   ```powershell
   Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
   ```
1. Öffnen Sie die Datei mit einem Texteditor und zeigen Sie die Namen für Ihre VNETs und Standorte an. Dies sind die Namen, die Sie beim Erstellen Ihrer Verbindungen verwenden.<br>**VNET-Namen** werden unter **VirtualNetworkSite name =** aufgelistet.<br>**Standortnamen** werden unter **LocalNetworkSiteRef name =** aufgelistet.