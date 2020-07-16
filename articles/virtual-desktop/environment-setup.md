---
title: 'Windows Virtual Desktop-Umgebung: Azure'
description: Enthält eine Beschreibung der grundlegenden Elemente einer Windows Virtual Desktop-Umgebung.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: d189d5febe54a83cde4fe6cd6aa51e728aa882e6
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2020
ms.locfileid: "86259890"
---
# <a name="windows-virtual-desktop-environment"></a>Windows Virtual Desktop-Umgebung

>[!IMPORTANT]
>Dieser Artikel gilt für das Update vom Frühjahr 2020 mit Windows Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie das Windows Virtual Desktop-Release vom Herbst 2019 ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/environment-setup-2019.md).
>
> Das Windows Virtual Desktop-Update vom Frühjahr 2020 befindet sich derzeit in der öffentlichen Vorschauphase. Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. 
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Windows Virtual Desktop ist ein Dienst, mit dem Benutzer einfachen und sicheren Zugriff auf ihre virtualisierten Desktops und RemoteApps erhalten. In diesem Thema wird die allgemeine Struktur der Windows Virtual Desktop-Umgebung näher erläutert.

## <a name="host-pools"></a>Hostpools

Ein Hostpool ist eine Sammlung mit virtuellen Azure-Computern, die unter Windows Virtual Desktop als Sitzungshosts registriert werden, wenn Sie den Windows Virtual Desktop-Agent ausführen. Alle virtuellen Computer von Sitzungshosts in einem Hostpool sollten über dasselbe Image erstellt werden, um eine einheitliche Benutzeroberfläche zu erhalten.

Bei einem Hostpool kann es sich um einen von zwei Typen handeln:

- Persönlich, wobei jeder Sitzungshost einzelnen Benutzern zugewiesen wird.
- In einem Pool, wobei Sitzungshosts Verbindungen von jedem Benutzer akzeptieren können, der im Hostpool für eine App-Gruppe autorisiert ist.

Sie können für den Hostpool zusätzliche Eigenschaften festlegen, um Folgendes zu ändern: Verhalten beim Lastenausgleich, Anzahl von Sitzungen, die für einen Sitzungshost jeweils verwendet werden können, und welche Schritte Benutzer für Sitzungshosts im Hostpool ausführen können, während sie an ihren Windows Virtual Desktop-Sitzungen angemeldet sind. Sie steuern die Ressourcen, die für Benutzer veröffentlicht werden, über App-Gruppen.

## <a name="app-groups"></a>App-Gruppen

Eine App-Gruppe ist eine logische Gruppierung von Anwendungen, die auf Sitzungshosts im Hostpool installiert sind. Eine App-Gruppe kann einen von zwei Typen aufweisen:

- RemoteApp, über die Benutzer auf die RemoteApps zugreifen, die Sie einzeln auswählen und für die App-Gruppe veröffentlichen
- Desktop, über den Benutzer auf den vollständigen Desktop zugreifen

Standardmäßig wird jeweils automatisch eine Desktop-App-Gruppe (mit dem Namen „Desktopanwendungsgruppe“) erstellt, wenn Sie einen Hostpool erstellen. Sie können diese App-Gruppe jederzeit entfernen. Es ist aber nicht möglich, im Hostpool eine weitere Desktop-App-Gruppe zu erstellen, während eine Desktop-App-Gruppe vorhanden ist. Sie müssen eine RemoteApp-App-Gruppe erstellen, um RemoteApps veröffentlichen zu können. Sie können mehrere RemoteApp-App-Gruppen erstellen, um unterschiedliche Workerszenarien abzudecken. Unterschiedliche RemoteApp-App-Gruppen können auch sich überlappende RemoteApps enthalten.

Zum Veröffentlichen von Ressourcen für Benutzer müssen Sie sie App-Gruppen zuweisen. Beachten Sie Folgendes, wenn Sie Benutzer App-Gruppen zuweisen:

- Ein Benutzer kann im gleichen Hostpool sowohl einer Desktop-App-Gruppe als auch einer RemoteApp-App-Gruppe zugewiesen sein. Benutzer können jedoch nur einen Typ von App-Gruppe pro Sitzung starten. Benutzer können nicht beide Typen von App-Gruppen gleichzeitig in einer einzigen Sitzung starten.
- Ein Benutzer kann in einem Hostpool mehreren App-Gruppen zugewiesen sein, und der Feed gilt dann gemeinsam für beide App-Gruppen.

## <a name="workspaces"></a>Arbeitsbereiche

Ein Arbeitsbereich ist eine logische Gruppierung von Anwendungsgruppen in Windows Virtual Desktop. Jede Windows Virtual Desktop-Anwendungsgruppe muss einem Arbeitsbereich zugeordnet sein, damit Benutzer die Remote-Apps und Desktops anzeigen können, die für sie veröffentlicht wurden.  

## <a name="end-users"></a>Endbenutzer

Nachdem Sie Benutzer ihren App-Gruppen zugewiesen haben, können diese mit beliebigen Windows Virtual Desktop-Clients eine Verbindung mit einer Windows Virtual Desktop-Bereitstellung herstellen.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich unter [Delegierter Zugriff in Windows Virtual Desktop](delegated-access-virtual-desktop.md) weiter über den delegierten Zugriff und das Zuweisen von Rollen zu Benutzern.

Informationen zum Einrichten des Windows Virtual Desktop-Hostpools finden Sie unter [Erstellen eines Hostpools mit dem Azure-Portal](create-host-pools-azure-marketplace.md).

Informationen zum Herstellen einer Verbindung mit Windows Virtual Desktop finden Sie in einem der folgenden Artikel:

- [Herstellen einer Verbindung mit Windows 10 oder Windows 7](connect-windows-7-10.md)
- [Herstellen einer Verbindung mit einem Webbrowser](connect-web.md)
- [Herstellen einer Verbindung mit dem Android-Client](connect-android.md)
- [Herstellen einer Verbindung mit dem macOS-Client](connect-macos.md)
- [Herstellen einer Verbindung mit dem iOS-Client](connect-ios.md)