---
title: 'Agent für die Azure AD Connect-Cloudbereitstellung: automatisches Upgrade | Microsoft-Dokumentation'
description: In diesem Artikel wird das integrierte automatische Upgradefeature im Agent für die Azure AD Connect-Cloudbereitstellung beschrieben.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/02/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 26c0b47dd358826b843143aaf23c469d852e93b3
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98612676"
---
# <a name="azure-ad-connect-cloud-provisioning-agent-automatic-upgrade"></a>Agent für die Azure AD Connect-Cloudbereitstellung: automatisches Upgrade

Mit dem automatischen Upgradefeature können Sie ganz leicht sicherstellen, dass Ihre Installation des Agents für die Azure AD Connect-Cloudbereitstellung (Azure Active Directory) immer auf dem neuesten Stand ist.

Hier ist der Agent installiert: „Programme\Azure AD Connect Provisioning Agent\AADConnectProvisioningAgent.exe“.

Klicken Sie zum Überprüfen Ihrer Version mit der rechten Maustaste auf die ausführbare Datei, und wählen Sie Eigenschaften sowie Details aus.

![Agent-Dateiversion](media/how-to-automatic-upgrade/agent-1.png)

Hier ist der Agent-Updater installiert: „Programme\Azure AD Connect Provisioning Agent Updater\AzureADConnectAgentUpdater.exe“.

Klicken Sie zum Überprüfen Ihrer Version mit der rechten Maustaste auf die ausführbare Datei, und wählen Sie Eigenschaften sowie Details aus.

![Agent-Updaterversion](media/how-to-automatic-upgrade/agent-2.png)

## <a name="uninstall-the-agent"></a>Deinstallieren des Agents
Navigieren Sie zu **Programm deinstallieren oder ändern**, um den Agent zu entfernen, und deinstallieren Sie Folgendes:

- **Microsoft Azure AD Connect Agent Updater**
- **Microsoft Azure AD Connect-Bereitstellungs-Agent**
- **Microsoft Azure AD Connect-Bereitstellungs-Agent-Paket**

![Entfernen des Agents](media/how-to-automatic-upgrade/agent-3.png)

## <a name="next-steps"></a>Nächste Schritte 

- [Was ist die Identitätsbereitstellung?](what-is-provisioning.md)
- [Worum handelt es sich bei der Azure AD Connect-Cloudsynchronisierung?](what-is-cloud-sync.md)

