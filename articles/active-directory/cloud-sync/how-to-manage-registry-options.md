---
title: 'Agent für die Azure AD Connect-Cloudbereitstellung: Verwalten von Registrierungsoptionen | Microsoft-Dokumentation'
description: In diesem Artikel wird das Verwalten von Registrierungsoptionen des Agents für die Azure AD Connect-Cloudbereitstellung beschrieben.
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
ms.date: 12/11/2020
ms.subservice: hybrid
ms.reviewer: chmutali
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1f39ef611e2ea15ef3bc3dbfcf09e9624cbcf8b3
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2021
ms.locfileid: "98678220"
---
# <a name="manage-agent-registry-options"></a>Verwalten von Registrierungsoptionen für den Agent

In diesem Abschnitt werden Registrierungsoptionen beschrieben, mit denen Sie das Verarbeitungsverhalten des Azure AD Connect-Bereitstellungs-Agents steuern können. 

## <a name="configure-ldap-connection-timeout"></a>Konfigurieren des LDAP-Verbindungstimeouts
Beim Ausführen von LDAP-Vorgängen für konfigurierte Active Directory-Domänencontroller verwendet der Bereitstellungs-Agent standardmäßig den Standardwert für Verbindungstimeouts von 30 Sekunden. Wenn Ihr Domänencontroller mehr Zeit für eine Antwort benötigt, enthält die Protokolldatei des Agents möglicherweise die folgende Fehlermeldung: 

`
System.DirectoryServices.Protocols.LdapException: The operation was aborted because the client side timeout limit was exceeded.
`

LDAP-Suchvorgänge können länger dauern, wenn das Suchattribut nicht indiziert ist. Wenn Sie den obigen Fehler erhalten, sollten Sie als ersten Schritt überprüfen, ob das Suchattribut [indiziert](/windows/win32/ad/indexed-attributes) ist. Wenn die Suchattribute indiziert sind und der Fehler weiterhin auftritt, können Sie das LDAP-Verbindungstimeout anhand der folgenden Schritte erhöhen: 

1. Melden Sie sich auf dem Windows-Server, auf dem der Azure AD Connect-Bereitstellungs-Agent ausgeführt wird, als Administrator an.
1. Verwenden Sie das Menüelement *Ausführen*, um den Registrierungs-Editor zu öffnen („regedit.exe“). 
1. Suchen Sie den Schlüsselordner **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure AD Connect Agents\Azure AD Connect Provisioning Agent**.
1. Klicken Sie mit der rechten Maustaste, und wählen Sie „Neu“ und dann „Zeichenfolgenwert“ aus.
1. Geben Sie als Namen `LdapConnectionTimeoutInMilliseconds` an.
1. Doppelklicken Sie auf **Wertname**, und geben Sie als Wert `60000` Millisekunden ein.
    > [!div class="mx-imgBorder"]
    > ![LDAP-Verbindungstimeout](media/how-to-manage-registry-options/ldap-connection-timeout.png)
1. Starten Sie den Azure AD Connect-Bereitstellungsdienst über die Konsole *Dienste* neu.
1. Wenn Sie mehrere Bereitstellungs-Agents bereitgestellt haben, wenden Sie diese Registrierungsänderung aus Konsistenzgründen auf alle Agents an. 

## <a name="configure-referral-chasing"></a>Konfigurieren der Empfehlungsverfolgung
Standardmäßig verfolgt der Azure AD Connect-Bereitstellung-Agent [Empfehlungen](/windows/win32/ad/referrals) nicht. Möglicherweise möchten Sie die Empfehlungsverfolgung aber aktivieren, um bestimmte eingehende Personalbereitstellungsszenarien unterstützen, z. B.: 
* Überprüfen der Eindeutigkeit des UPN über mehrere Domänen hinweg
* Auflösen von domänenübergreifenden Empfehlungen von Vorgesetzten

Führen Sie die folgenden Schritte aus, um die Empfehlungsverfolgung zu aktivieren:

1. Melden Sie sich auf dem Windows-Server, auf dem der Azure AD Connect-Bereitstellungs-Agent ausgeführt wird, als Administrator an.
1. Verwenden Sie das Menüelement *Ausführen*, um den Registrierungs-Editor zu öffnen („regedit.exe“). 
1. Suchen Sie den Schlüsselordner **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure AD Connect Agents\Azure AD Connect Provisioning Agent**.
1. Klicken Sie mit der rechten Maustaste, und wählen Sie „Neu“ und dann „Zeichenfolgenwert“ aus.
1. Geben Sie als Namen `ReferralChasingOptions` an.
1. Doppelklicken Sie auf **Wertname**, und geben Sie als Wert `96` ein. Dieser Wert entspricht dem konstanten Wert für `ReferralChasingOptions.All` und gibt an, dass der Agent Empfehlungen sowohl auf Ebene der Unterstruktur als auch auf Basisebene verfolgt. 
    > [!div class="mx-imgBorder"]
    > ![Empfehlungsverfolgung](media/how-to-manage-registry-options/referral-chasing.png)
1. Starten Sie den Azure AD Connect-Bereitstellungsdienst über die Konsole *Dienste* neu.
1. Wenn Sie mehrere Bereitstellungs-Agents bereitgestellt haben, wenden Sie diese Registrierungsänderung aus Konsistenzgründen auf alle Agents an.

## <a name="skip-gmsa-configuration"></a>Überspringen der GMSA-Konfiguration
Mit der Agent-Version 1.1.281.0 und höher werden Sie standardmäßig beim Ausführen des Konfigurations-Assistenten für den Agent aufgefordert, das [gruppenverwaltete Dienstkonto (Group Managed Service Account (GMSA))](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview) einzurichten. Das GMSA-Setup vom Assistenten wird zur Laufzeit für alle Synchronisierungs- und Bereitstellungsvorgänge verwendet. 

Wenn Sie ein Upgrade von einer früheren Version des Agents durchführen und ein benutzerdefiniertes Dienstkonto mit delegierten Berechtigungen auf Organisationseinheitsebene eingerichtet haben, die für Ihre Active Directory-Topologie spezifisch sind, können Sie die GMSA-Konfiguration überspringen/verschieben und diese Änderung planen. 

> [!NOTE]
> Diese Anleitung gilt speziell für Kunden, die die eingehende HR-Bereitstellung (Workday/SuccessFactors) mit Agent-Versionen vor 1.1.281.0 konfiguriert und ein benutzerdefiniertes Dienstkonto für Agent-Vorgänge eingerichtet haben. Langfristig empfiehlt es sich, als bewährte Vorgehensweise zum GMSA zu wechseln.  

In diesem Szenario können Sie weiterhin die Binärdateien für den Agent aktualisieren und die GMSA-Konfiguration mit den folgenden Schritten überspringen: 

1. Melden Sie sich auf dem Windows-Server, auf dem der Azure AD Connect-Bereitstellungs-Agent ausgeführt wird, als Administrator an.
1. Führen Sie das Agent-Installationsprogramm aus, um die neuen Binärdateien des Agents zu installieren. Schließen Sie den Agent-Konfigurations-Assistenten, der nach erfolgreicher Installation automatisch geöffnet wird. 
1. Verwenden Sie das Menüelement *Ausführen*, um den Registrierungs-Editor zu öffnen („regedit.exe“). 
1. Suchen Sie den Schlüsselordner **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure AD Connect Agents\Azure AD Connect Provisioning Agent**.
1. Führen Sie einen Rechtsklick aus, und wählen Sie „Neu > DWORD Value“ (DWORD-Wert) aus.
1. Geben Sie als Namen `UseCredentials` an.
1. Doppelklicken Sie auf **Wertname**, und geben Sie als Wert `1` ein.  
    > [!div class="mx-imgBorder"]
    > ![Verwenden von Anmeldeinformationen](media/how-to-manage-registry-options/use-credentials.png)
1. Starten Sie den Azure AD Connect-Bereitstellungsdienst über die Konsole *Dienste* neu.
1. Wenn Sie mehrere Bereitstellungs-Agents bereitgestellt haben, wenden Sie diese Registrierungsänderung aus Konsistenzgründen auf alle Agents an.
1. Führen Sie über das Desktopkontextmenü den Assistenten für die Agent-Konfiguration aus. Der Assistent überspringt die GMSA-Konfiguration. 


> [!NOTE]
> Sie können die Festlegung der Registrierungsoptionen überprüfen, indem Sie die [ausführliche Protokollierung](how-to-troubleshoot.md#log-files) aktivieren. Die beim Start des Agents ausgegebenen Protokolle enthalten die Konfigurationswerte, die aus der Registrierung übernommen wurden. 

## <a name="next-steps"></a>Nächste Schritte 

- [Was ist die Identitätsbereitstellung?](what-is-provisioning.md)
- [Worum handelt es sich bei der Azure AD Connect-Cloudsynchronisierung?](what-is-cloud-sync.md)

