---
title: 'Agent für die Azure AD Connect-Cloudbereitstellung: Automatisches Upgrade | Microsoft-Dokumentation'
description: In diesem Thema wird das integrierte automatische Upgradefeature im Agent für die Azure AD Connect-Cloudbereitstellung beschrieben.
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
ms.openlocfilehash: 193804064fbf6d1abb2ce06df1e923ec709ef6de
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74795517"
---
# <a name="azure-ad-connect-cloud-provisioning-agent-automatic-upgrade"></a>Agent für die Azure AD Connect-Cloudbereitstellung: automatische Upgrade

Noch nie konnten Sie so leicht sicherstellen, dass Ihre Installation des Agents für die Azure AD Connect-Cloudbereitstellung mit dem **automatischen Upgradefeature** immer auf dem neuesten Stand ist. Dieses Feature wird standardmäßig aktiviert und kann nicht deaktiviert werden.

Hier ist der Agent installiert:  **Programme\Azure AD Connect Provisioning Agent\AADConnectProvisioningAgent.exe**

Sie können Ihre Version durch einen Rechtsklick auf die ausführbare Datei und durch Auswählen der Eigenschaften sowie der Details überprüfen.

![Agent-Dateiversion](media/how-to-automatic-upgrade/agent1.png)

Hier ist der Agent-Updater installiert:  **Programme\Azure AD Connect Provisioning Agent Updater\AzureADConnectAgentUpdater.exe**

Sie können Ihre Version durch einen Rechtsklick auf die ausführbare Datei und durch Auswählen der Eigenschaften sowie der Details überprüfen.

![Agent-Updaterversion](media/how-to-automatic-upgrade/agent2.png)

## <a name="uninstalling-the-agent"></a>Deinstallieren des Agents
Navigieren Sie zu **Programm deinstallieren oder ändern**, um den Agent zu entfernen, und deinstallieren Sie Folgendes:

- Microsoft Azure AD Connect Agent Updater
- Microsoft Azure AD Connect-Bereitstellungs-Agent
- Microsoft Azure AD Connect-Bereitstellungs-Agent-Paket

![Entfernen des Agents](media/how-to-automatic-upgrade/agent3.png)

## <a name="next-steps"></a>Nächste Schritte 

- [Was ist die Identitätsbereitstellung?](what-is-provisioning.md)
- [Was ist die Azure AD Connect-Cloudbereitstellung?](what-is-cloud-provisioning.md)

