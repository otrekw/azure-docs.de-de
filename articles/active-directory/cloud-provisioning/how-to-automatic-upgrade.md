---
title: 'Agent für die Azure AD Connect-Cloudbereitstellung: Automatisches Upgrade | Microsoft-Dokumentation'
description: In diesem Artikel wird das integrierte automatische Upgradefeature im Agent für die Azure AD Connect-Cloudbereitstellung beschrieben.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/02/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: d3786386f75f4b85fe89562254eab63471de6cb2
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/31/2019
ms.locfileid: "75549396"
---
# <a name="azure-ad-connect-cloud-provisioning-agent-automatic-upgrade"></a>Agent für die Azure AD Connect-Cloudbereitstellung: automatische Upgrade

Mit dem automatischen Upgradefeature können Sie ganz leicht sicherstellen, dass Ihre Installation des Agents für die Azure AD Connect-Cloudbereitstellung (Azure Active Directory) immer auf dem neuesten Stand ist. Dieses Feature wird standardmäßig aktiviert und kann nicht deaktiviert werden.

Hier ist der Agent installiert: Programme\Azure AD Connect Provisioning Agent\AADConnectProvisioningAgent.exe.

Klicken Sie zum Überprüfen Ihrer Version mit der rechten Maustaste auf die ausführbare Datei, und wählen Sie Eigenschaften sowie Details aus.

![Agent-Dateiversion](media/how-to-automatic-upgrade/agent1.png)

Hier ist der Agent-Updater installiert: Programme\Azure AD Connect Provisioning Agent Updater\AzureADConnectAgentUpdater.exe.

Klicken Sie zum Überprüfen Ihrer Version mit der rechten Maustaste auf die ausführbare Datei, und wählen Sie Eigenschaften sowie Details aus.

![Agent-Updaterversion](media/how-to-automatic-upgrade/agent2.png)

## <a name="uninstall-the-agent"></a>Deinstallieren des Agents
Navigieren Sie zu **Programm deinstallieren oder ändern**, um den Agent zu entfernen, und deinstallieren Sie Folgendes:

- **Microsoft Azure AD Connect Agent Updater**
- **Microsoft Azure AD Connect-Bereitstellungs-Agent**
- **Microsoft Azure AD Connect-Bereitstellungs-Agent-Paket**

![Entfernen des Agents](media/how-to-automatic-upgrade/agent3.png)

## <a name="next-steps"></a>Nächste Schritte 

- [Was ist die Identitätsbereitstellung?](what-is-provisioning.md)
- [Was ist die Azure AD Connect-Cloudbereitstellung?](what-is-cloud-provisioning.md)

