---
title: Problembehandlung bei Windows Virtual Desktop-Dienstverbindungen – Azure
description: Informationen zum Beheben von Problemen beim Einrichten von Clientverbindungen in einer Windows Virtual Desktop-Mandantenumgebung
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 05/20/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 356506224a0273eeea65f0f901fbc79c338498d2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "83743602"
---
# <a name="windows-virtual-desktop-service-connections"></a>Windows Virtual Desktop-Dienstverbindungen

>[!IMPORTANT]
>Dieser Inhalt gilt für das Release vom Herbst 2019, das keine Windows Virtual Desktop-Objekte in Azure Resource Manager unterstützt. Wenn Sie Windows Virtual Desktop-Objekte in Azure Resource Manager verwalten möchten, die im Update vom Frühjahr 2020 eingeführt wurden, finden Sie weitere Informationen in [diesem Artikel](../troubleshoot-service-connection.md).

Verwenden Sie diesen Artikel zum Beheben von Problemen mit Windows Virtual Desktop-Clientverbindungen.

## <a name="provide-feedback"></a>Feedback geben

In der [Windows Virtual Desktop Tech Community](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) können Sie uns Feedback geben und sich mit dem Produktteam und anderen aktiven Communitymitgliedern über den Windows Virtual Desktop-Dienst austauschen.

## <a name="user-connects-but-nothing-is-displayed-no-feed"></a>Der Benutzer stellt eine Verbindung her, aber es wird nichts angezeigt (kein Feed).

Ein Benutzer kann Remotedesktopclients starten und sich authentifizieren, ihm werden jedoch im Websuchfeed keine Symbole angezeigt.

Vergewissern Sie sich, dass der Benutzer, der die Probleme meldet, mithilfe dieser Befehlszeile Anwendungsgruppen zugewiesen wurde:

```PowerShell
Get-RdsAppGroupUser <tenantname> <hostpoolname> <appgroupname>
```

Vergewissern Sie sich, dass sich der Benutzer mit den richtigen Anmeldeinformationen anmeldet.

Wenn der Webclient verwendet wird, vergewissern Sie sich, dass keine Probleme mit zwischengespeicherten Anmeldeinformationen vorliegen.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über die Problembehandlung von Windows Virtual Desktop und die Eskalationspfade finden Sie unter [Überblick über Problembehandlung, Feedback und Support](troubleshoot-set-up-overview-2019.md).
- Informationen zur Problembehandlung beim Erstellen eines Mandanten- und Hostpools in einer Windows Virtual Desktop-Umgebung finden Sie unter [Mandanten- und Hostpoolerstellung](troubleshoot-set-up-issues-2019.md).
- Informationen zur Problembehandlung bei der Konfiguration eines virtuellen Computers (VM) in Windows Virtual Desktop finden Sie unter [Konfiguration des virtuellen Sitzungshostcomputers](troubleshoot-vm-configuration-2019.md).
- Informationen zur Problembehandlung bei der Verwendung von PowerShell mit Windows Virtual Desktop finden Sie unter [Windows Virtual Desktop – PowerShell](troubleshoot-powershell-2019.md).
- Ein Tutorial zur Problembehandlung finden Sie unter [Tutorial: Problembehandlung von Bereitstellungen der Resource Manager-Vorlage](../../azure-resource-manager/templates/template-tutorial-troubleshoot.md).
