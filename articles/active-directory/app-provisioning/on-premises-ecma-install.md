---
title: Installation des Azure AD-ECMA-Connectorhosts
description: In diesem Artikel wird das Installieren des Azure AD-ECMA-Connectorhosts beschrieben.
services: active-directory
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 05/28/2021
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: e12ce0d979b323f10104d345f86b77618e094ba6
ms.sourcegitcommit: ff1aa951f5d81381811246ac2380bcddc7e0c2b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2021
ms.locfileid: "111570374"
---
# <a name="installation-of-the-azure-ad-ecma-connector-host"></a>Installation des Azure AD-ECMA-Connectorhosts

>[!IMPORTANT]
> Die Vorschauversion der lokalen Bereitstellung ist derzeit nur auf Einladung verfügbar. Sie können [hier](https://aka.ms/onpremprovisioningpublicpreviewaccess) den Zugriff auf die Funktion anfordern. Wir öffnen die Vorschauversion im Rahmen der Vorbereitung auf die allgemeine Verfügbarkeit in den nächsten Monaten für weitere Kunden und Connectors.

Der Azure AD-ECMA-Connectorhost ist Bestandteil des Azure AD Connect-Bereitstellungs-Agent-Pakets.  Der Bereitstellungs-Agent und der Azure AD-ECMA-Connectorhost sind zwei separate Windows-Dienste, die mithilfe eines Installationsprogramms auf demselben Computer installiert werden. 

Die Installation und die Konfiguration des Azure AD-ECMA-Connectorhosts erfolgen in einem Prozess.  Verwenden Sie zur Durchführung des Prozesses den folgenden Flow.

 ![Installationsflow](./media/on-premises-ecma-install/flow-1.png)  

Weitere Informationen zur Installation und Konfiguration finden Sie unter:
   - [Voraussetzungen für den Azure AD-ECMA-Connectorhost](on-premises-ecma-prerequisites.md)
   - [Konfigurieren des Azure AD-ECMA-Connectorhosts und des Bereitstellungs-Agents](on-premises-ecma-configure.md)
    - [Konfigurieren eines generischen SQL-Connectors mit dem Azure AD-ECMA-Connectorhost](on-premises-sql-connector-configure.md)


## <a name="download-and-install-the-azure-ad-connect-provisioning-agent-package"></a>Herunterladen und Installieren des Azure AD Connect-Bereitstellungs-Agent-Pakets

 1. Anmelden beim Azure-Portal
 2. Navigieren Sie zu „Unternehmensanwendungen“ > Neue Anwendung hinzufügen“.
 3. Suchen Sie nach der Anwendung „Lokale Bereitstellung“, und fügen Sie Ihrem Mandantenimage die Anwendung hinzu.
 4. Navigieren Sie zum Blatt „Bereitstellung“.
 5. Klicken Sie auf „Lokale Konnektivität“.
 6.  Laden Sie das Agent-Installationsprogramm herunter.
 7. Führen Sie das Installationsprogramm für den Azure AD Connect-Bereitstellungs-Agent („AADConnectProvisioningAgentSetup.msi“) aus.
 8. Akzeptieren Sie auf dem Bildschirm **Microsoft Azure AD Connect-Bereitstellungs-Agent-Paket** die Lizenzbedingungen, und wählen Sie **Installieren** aus.
   ![Bildschirm „Microsoft Azure AD Connect-Bereitstellungs-Agent-Paket“](media/on-premises-ecma-install/install-1.png)</br>
 9. Nach Abschluss dieses Vorgangs wird der Konfigurations-Assistent gestartet. Klicken Sie auf **Weiter**.
   ![Bildschirm „Willkommen“](media/on-premises-ecma-install/install-2.png)</br>
 10. Wählen Sie auf dem Bildschirm **Erweiterung auswählen** die Option **Lokale Anwendungsbereitstellung (Azure AD in Anwendung)** aus, und klicken Sie auf **Weiter**. 
   ![Erweiterung auswählen](media/on-premises-ecma-install/install-3.png)</br>
 12. Verwenden Sie Ihr globales Administratorkonto, und melden Sie sich bei Azure AD an.
     ![Azure-Anmeldung](media/on-premises-ecma-install/install-4.png)</br>
 13.  Klicken Sie auf dem Bildschirm **Agent-Konfiguration** auf **Bestätigen**.
     ![Bestätigen der Installation](media/on-premises-ecma-install/install-5.png)</br>
 14.  Nach Abschluss der Installation sollte im unteren Bereich des Assistenten eine Meldung angezeigt werden.  Klicken Sie auf **Fertig stellen**.
     ![Auf „Fertig stellen“ klicken](media/on-premises-ecma-install/install-6.png)</br>
 15. Klicken Sie auf **Schließen**.

Nachdem das Agent-Paket erfolgreich installiert wurde, müssen Sie den Azure AD-ECMA-Connectorhost konfigurieren und Connectors erstellen oder importieren.  
## <a name="next-steps"></a>Nächste Schritte


- [Voraussetzungen für den Azure AD-ECMA-Connectorhost](on-premises-ecma-prerequisites.md)
- [Konfiguration des Azure AD-ECMA-Connectorhosts](on-premises-ecma-configure.md)
- [Generischer SQL-Connector](on-premises-sql-connector-configure.md)
