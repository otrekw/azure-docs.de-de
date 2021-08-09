---
title: Problembehandlung bei Azure Virtual Desktop-Dienstverbindungen (klassisch) – Azure
description: Informationen zum Beheben von Problemen beim Einrichten von Clientverbindungen in einer Azure Virtual Desktop-Mandantenumgebung (klassisch)
author: Heidilohr
ms.topic: troubleshooting
ms.date: 05/20/2020
ms.author: helohr
manager: femila
ms.openlocfilehash: 856d6e65273b15c843bffe40c818b07c60f00fb0
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111753933"
---
# <a name="azure-virtual-desktop-classic-service-connections"></a>Azure Virtual Desktop-Dienstverbindungen (klassisch)

>[!IMPORTANT]
>Dieser Inhalt gilt für Azure Virtual Desktop (klassisch). Der Dienst unterstützt keine Azure Virtual Desktop-Objekte in Azure Resource Manager. Wenn Sie Azure Virtual Desktop-Objekte in Azure Resource Manager verwalten möchten, helfen Ihnen die Informationen in [diesem Artikel](../troubleshoot-service-connection.md) weiter.

Verwenden Sie diesen Artikel zum Beheben von Problemen mit Azure Virtual Desktop-Clientverbindungen.

## <a name="provide-feedback"></a>Feedback geben

In der [Azure Virtual Desktop Tech Community](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) können Sie uns Feedback geben und sich mit dem Produktteam und anderen aktiven Communitymitgliedern über den Azure Virtual Desktop-Dienst austauschen.

## <a name="user-connects-but-nothing-is-displayed-no-feed"></a>Der Benutzer stellt eine Verbindung her, aber es wird nichts angezeigt (kein Feed).

Ein Benutzer kann Remotedesktopclients starten und sich authentifizieren, ihm werden jedoch im Websuchfeed keine Symbole angezeigt.

Vergewissern Sie sich, dass der Benutzer, der die Probleme meldet, mithilfe dieser Befehlszeile Anwendungsgruppen zugewiesen wurde:

```PowerShell
Get-RdsAppGroupUser <tenantname> <hostpoolname> <appgroupname>
```

Vergewissern Sie sich, dass sich der Benutzer mit den richtigen Anmeldeinformationen anmeldet.

Wenn der Webclient verwendet wird, vergewissern Sie sich, dass keine Probleme mit zwischengespeicherten Anmeldeinformationen vorliegen.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über die Problembehandlung von Azure Virtual Desktop und die Eskalationspfade finden Sie unter [Überblick über Problembehandlung, Feedback und Support](troubleshoot-set-up-overview-2019.md).
- Informationen zur Problembehandlung beim Erstellen eines Mandanten- und Hostpools in einer Azure Virtual Desktop-Umgebung finden Sie unter [Mandanten- und Hostpoolerstellung](troubleshoot-set-up-issues-2019.md).
- Informationen zur Problembehandlung bei der Konfiguration einer VM in Azure Virtual Desktop finden Sie unter [Konfiguration von Sitzungshost-VMs](troubleshoot-vm-configuration-2019.md).
- Informationen zur Problembehandlung bei der Verwendung von PowerShell mit Azure Virtual Desktop finden Sie unter [Azure Virtual Desktop – PowerShell](troubleshoot-powershell-2019.md).
- Ein Tutorial zur Problembehandlung finden Sie unter [Tutorial: Problembehandlung von Bereitstellungen der Resource Manager-Vorlage](../../azure-resource-manager/templates/template-tutorial-troubleshoot.md).
