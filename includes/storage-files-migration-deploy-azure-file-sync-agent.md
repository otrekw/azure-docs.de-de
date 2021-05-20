---
title: include file
description: include file
services: storage
author: fauhse
ms.service: storage
ms.topic: include
ms.date: 2/20/2020
ms.author: fauhse
ms.custom: include file
ms.openlocfilehash: 544bbaa358f0ca158c757524406ffd046ad43af7
ms.sourcegitcommit: ba8f0365b192f6f708eb8ce7aadb134ef8eda326
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2021
ms.locfileid: "109645401"
---
In diesem Abschnitt installieren Sie den Azure-Dateisynchronisierungs-Agent auf Ihrer Windows Server-Instanz.

Im [Bereitstellungsleitfaden](../articles/storage/file-sync/file-sync-deployment-guide.md) wird beschrieben, dass Sie die **verstärkte Sicherheitskonfiguration für Internet Explorer** deaktivieren müssen. Diese Sicherheitsmaßnahme ist nicht auf die Azure-Dateisynchronisierung anwendbar. Wenn Sie sie deaktivieren, können Sie sich problemlos bei Azure authentifizieren.

Öffnen Sie PowerShell. Installieren Sie die erforderlichen PowerShell-Module mithilfe der folgenden Befehle. Installieren Sie das vollständige Modul und den NuGet-Anbieter, wenn Sie dazu aufgefordert werden.

```powershell
Install-Module -Name Az -AllowClobber
Install-Module -Name Az.StorageSync
```

Wenn auf Ihrem Server Probleme mit der Internetverbindung auftreten, sollten Sie diese Probleme jetzt beheben. Bei der Azure-Dateisynchronisierung wird eine beliebige verfügbare Internetverbindung verwendet. Konfigurationen, in denen für die Internetverbindung ein Proxyserver erforderlich ist, werden ebenfalls unterstützt. Sie können entweder jetzt einen computerweiten Proxy konfigurieren oder während der Agent-Installation einen Proxy angeben, der ausschließlich für die Azure-Dateisynchronisierung verwendet wird.

Wenn die Konfiguration eines Proxy erfordert, dass Sie Ihre Firewalls für diesen Server öffnen, könnte dieser Ansatz für Sie geeignet sein. Am Ende der Serverinstallation und nach abgeschlossener Serverregistrierung werden in einem Bericht zur Netzwerkkonnektivität die genauen Endpunkt-URLs in Azure angezeigt, mit denen die Azure-Dateisynchronisierung für Ihre ausgewählte Region kommunizieren muss. Der Bericht enthält zudem Informationen dazu, weshalb diese Kommunikation erforderlich ist. Sie können den Bericht verwenden, um die Firewalls auf diesem Server zu sperren und nur bestimmte URLs zuzulassen.

Sie können auch einen konservativeren Ansatz wählen, bei dem die Firewalls nicht vollständig geöffnet werden. Sie können stattdessen die Kommunikation des Servers auf übergeordnete DNS-Namespaces beschränken. Weitere Informationen finden Sie unter [Proxy- und Firewalleinstellungen der Azure-Dateisynchronisierung](../articles/storage/file-sync/file-sync-firewall-and-proxy.md). Halten Sie sich an ihre eigenen bewährten Methoden für Netzwerke.

Am Ende des Assistenten für die Serverinstallation wird ein Assistent zur Serverregistrierung geöffnet. Registrieren Sie den Server bei der zuvor genannten Azure-Speichersynchronisierungsdienst-Ressource.

Im Bereitstellungshandbuch sind diese Schritte näher beschrieben. Dabei wird auch auf die PowerShell-Module eingegangen, die zuerst installiert werden sollten: [Installation des Azure-Dateisynchronisierungs-Agents](../articles/storage/file-sync/file-sync-deployment-guide.md).

Verwenden Sie den neuesten Agent. Sie können ihn aus dem Microsoft Download Center herunterladen: [Azure-Dateisynchronisierungs-Agent](https://aka.ms/AFS/agent "Download des Azure-Dateisynchronisierungs-Agents").

Nach der Installation und Registrierung des Servers können Sie überprüfen, ob dieser Schritt erfolgreich ausgeführt wurde. Wechseln Sie im Azure-Portal zur Storage Sync Service-Ressource. Wechseln Sie im linken Menü zu **Registrierte Server**. Ihr Server sollte in dieser Liste enthalten sein.
