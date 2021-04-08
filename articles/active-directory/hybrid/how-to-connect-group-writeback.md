---
title: 'Azure AD Connect: Gruppenrückschreiben'
description: In diesem Artikel wird das Gruppenrückschreiben in Azure AD Connect beschrieben.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.date: 06/11/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9c506c87ad5901754175f18e6b50bc6ed46a3c19
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98246909"
---
# <a name="azure-ad-connect-group-writeback"></a>Gruppenrückschreiben in Azure AD Connect

Das Gruppenrückschreiben ermöglicht Kunden die Nutzung von Cloudgruppen für ihre Hybridanforderungen. Bei Verwendung des Features „Microsoft 365-Gruppen“ können diese Gruppen in Ihrer lokalen Active Directory-Instanz dargestellt werden. Diese Option ist **nur** verfügbar, wenn Exchange in Ihrer lokalen Active Directory-Instanz vorhanden ist.

## <a name="pre-requisites"></a>Voraussetzungen
Damit das Gruppenrückschreiben aktiviert werden kann, ist Folgendes erforderlich:
- Azure Active Directory Premium-Lizenzen für Ihren Mandanten
- Eine konfigurierte (und ordnungsgemäß funktionierende) Hybridbereitstellung zwischen Ihrer lokalen Exchange-Organisation und Microsoft 365
- Lokale Installation einer unterstützten Version von Exchange
- Konfiguriertes einmaliges Anmelden mit Azure Active Directory Connect 

## <a name="enable-group-writeback"></a>Aktivieren des Gruppenrückschreibens
Gehen Sie zum Aktivieren des Gruppenrückschreibens wie folgt vor:

1. Öffnen Sie den Azure AD Connect-Assistenten, wählen Sie **Konfigurieren** aus, und klicken Sie auf **Weiter**.
2. Wählen Sie **Synchronisierungsoptionen anpassen** aus, und klicken Sie auf **Weiter**.
3. Geben Sie auf der Seite **Mit Azure AD verbinden** Ihre Anmeldeinformationen ein. Klicken Sie auf **Weiter**.
4. Vergewissern Sie sich auf der Seite **Optionale Features**, dass immer noch die zuvor konfigurierten Optionen ausgewählt sind.
5. Wählen Sie **Gruppenrückschreiben** aus, und klicken Sie auf **Weiter**.
6. Wählen Sie auf der Seite **Rückschreiben** eine Active Directory-Organisationseinheit (OE) zum Speichern von Objekten aus, die von Microsoft 365 mit Ihrer lokalen Organisation synchronisiert werden, und klicken Sie anschließend auf **Weiter**.
7. Klicken Sie auf der Seite **Bereit zur Konfiguration** auf **Konfigurieren**.
8. Klicken Sie nach Abschluss des Assistenten auf der Seite „Konfiguration abgeschlossen“ auf **Beenden**.
9. Öffnen Sie Windows PowerShell als Administrator auf dem Azure Active Directory Connect-Server, und führen Sie die folgenden Befehle aus.

```Powershell
$AzureADConnectSWritebackAccountDN =  <MSOL_ account DN>
Import-Module "C:\Program Files\Microsoft Azure Active Directory Connect\AdSyncConfig\AdSyncConfig.psm1"

# To grant the <MSOL_account> permission to all domains in the forest:
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountDN $AzureADConnectSWritebackAccountDN

# To grant the <MSOL_account> permission to specific OU (eg. the OU chosen to writeback Office 365 Groups to):
$GroupWritebackOU = <DN of OU where groups are to be written back to>
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountDN $AzureADConnectSWritebackAccountDN -ADObjectDN $GroupWritebackOU
```

Weitere Informationen zum Konfigurieren der Microsoft 365-Gruppen finden Sie unter [Konfigurieren von Microsoft 365-Gruppen mit lokaler Exchange-Hybridbereitstellung](/exchange/hybrid-deployment/set-up-microsoft-365-groups#enable-group-writeback-in-azure-ad-connect).

## <a name="disabling-group-writeback"></a>Deaktivieren des Gruppenrückschreibens
Gehen Sie zum Deaktivieren des Gruppenrückschreibens wie folgt vor: 


1. Starten Sie den Azure Active Directory Connect-Assistenten, und navigieren Sie zur Seite „Weitere Aufgaben“. Wählen Sie die Aufgabe **Synchronisierungsoptionen anpassen** aus, und klicken Sie auf **Weiter**.
2. Deaktivieren Sie auf der Seite **Optionale Features** die Option „Gruppenrückschreiben“.  Sie erhalten eine Warnung mit dem Hinweis, dass Gruppen gelöscht werden.  Klicken Sie auf **Ja**.
   >[!IMPORTANT]
   > Wenn Sie das Gruppenrückschreiben deaktivieren, werden alle zuvor von diesem Feature erstellten Gruppen im nächsten Synchronisierungszyklus aus Ihrer lokalen Active Directory-Instanz gelöscht. 

   ![Deaktivieren des Kontrollkästchens](media/how-to-connect-group-writeback/group2.png)
  
3. Klicken Sie auf **Weiter**.
4. Klicken Sie auf **Konfigurieren**.

 >[!NOTE]
 > Wenn Sie das Gruppenrückschreiben deaktivieren, werden die Flags für vollständiges Importieren und vollständiges Synchronisieren in Azure Active Directory Connector auf „true“ festgelegt. Dadurch werden die Regeländerungen beim nächsten Synchronisierungsvorgang verteilt, und die zuvor in Ihre Active Directory-Instanz zurückgeschriebenen Gruppen werden gelöscht.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](whatis-hybrid-identity.md).
