---
title: Bereitstellen des Azure-Dateisynchronisierungs-Agents
description: Bereitstellen des Azure-Dateisynchronisierungs-Agents Ein gemeinsamer Textblock der Migrationsdokumentation.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 694becc49667204ef2071a140bb6330285089039
ms.sourcegitcommit: 5192c04feaa3d1bd564efe957f200b7b1a93a381
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2020
ms.locfileid: "78209277"
---
In diesem Abschnitt installieren Sie den Azure-Dateisynchronisierungs-Agent auf Ihrer Windows Server-Instanz.
Im [Bereitstellungshandbuch](../articles/storage/files/storage-sync-files-deployment-guide.md) erfahren Sie, dass Sie die **erweiterte IE-Sicherheit** deaktivieren müssen. Bei der erweiterten IE-Sicherheit handelt es sich um eine Sicherheitsmaßnahme, die für die Azure-Dateisynchronisierung nicht relevant ist. Wenn Sie sie deaktivieren, können Sie sich problemlos gegenüber Azure authentifizieren.

Öffnen Sie PowerShell, und installieren Sie die erforderlichen PowerShell-Module mit den folgenden Befehlen. Stellen Sie sicher, dass Sie das vollständige Modul und den NuGet-Anbieter installieren, wenn Sie dazu aufgefordert werden:

```powershell
Install-Module -Name Az -AllowClobber
Install-Module -Name Az.StorageSync
```

Wenn auf Ihrem Server Probleme mit der Internetverbindung auftreten, sollten Sie diese Probleme jetzt beheben. Bei der Azure-Dateisynchronisierung wird eine beliebige verfügbare Internetverbindung verwendet.
Konfigurationen, in denen für die Internetverbindung ein Proxyserver erforderlich ist, werden ebenfalls unterstützt. Sie können entweder jetzt einen computerweiten Proxy konfigurieren oder während der Agentinstallation einen Proxy angeben, der ausschließlich für die Dateisynchronisierung verwendet wird.

Möglicherweise müssen Sie Ihre Firewalls auf dem betroffenen Server öffnen. Am Ende der Serverinstallation und nach abgeschlossener Serverregistrierung wird ein Bericht zur Netzwerkkonnektivität angezeigt, der die genauen Endpunkt-URLs in Azure umfasst, mit denen die Dateisynchronisierung für Ihre ausgewählte Region kommunizieren muss. Der Bericht enthält zudem Informationen dazu, weshalb diese Kommunikation erforderlich ist. Sie können den Bericht darüber hinaus verwenden, um die Firewalls auf diesem Server zu sperren und nur bestimmte URLs zuzulassen.

Sie können auch eine konservativere Vorgehensweise wählen, bei der Sie die Firewalls nicht öffnen, sondern die Kommunikation der Server stattdessen auf DNS-Namespaces einer höheren Ebene beschränken. Weitere Informationen dazu finden Sie im Artikel [Proxy- und Firewalleinstellungen für die Azure-Dateisynchronisierung](../articles/storage/files/storage-sync-files-firewall-and-proxy.md). Halten Sie sich an ihre eigenen bewährten Methoden für Netzwerke.

Am Ende des Assistenten für die *Serverinstallation* wird ein Assistent zur *Serverregistrierung* geöffnet.
Registrieren Sie den Server bei der zuvor genannten Azure-Speichersynchronisierungsdienst-Ressource.

Im Bereitstellungshandbuch sind diese Schritte näher beschrieben. Dabei wird auch auf die oben stehenden PowerShell-Module eingegangen, die zuerst installiert werden sollten: [Installation des Azure-Dateisynchronisierungs-Agents](../articles/storage/files/storage-sync-files-deployment-guide.md).

Sie sollten die aktuelle Version des Agents verwenden, die im Microsoft Download Center heruntergeladen werden kann: [Azure-Dateisynchronisierungs-Agent](https://aka.ms/AFS/agent "Download des Azure-Dateisynchronisierungs-Agents").

Nach der Installation und Registrierung des Servers können Sie überprüfen, ob dieser Schritt erfolgreich ausgeführt wurde: Wechseln Sie im Azure-Portal zur Speichersynchronisierungsdienst-Ressource, und rufen Sie über das Menü links „Registrierte Server“ auf. Sie sollten umgehend Ihren Server in der Liste sehen.
