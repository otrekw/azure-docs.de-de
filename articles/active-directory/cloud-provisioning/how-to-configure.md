---
title: Konfiguration eines neuen Agent für die Azure AD Connect-Cloudbereitstellung
description: In diesem Artikel wird die Installation der Cloudbereitstellung beschrieben.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 02/26/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: c8b18629a776dd98950f49b1f607cbc876abcd9c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91628888"
---
# <a name="create-a-new-configuration-for-azure-ad-connect-cloud-based-provisioning"></a>Erstellen einer neuen Konfiguration für die cloudbasierte Azure AD Connect-Bereitstellung

Nachdem Sie den Agent installiert haben, müssen Sie sich beim Azure-Portal anmelden und die Azure Active Directory (Azure AD) Connect-Cloudbereitstellung konfigurieren. Führen Sie die folgenden Schritte aus, um den Agent zu aktivieren.

## <a name="configure-provisioning"></a>Konfigurieren der Bereitstellung
Gehen Sie zum Konfigurieren der Bereitstellung wie folgt vor.

 1. Wählen Sie im Azure-Portal die Option **Azure Active Directory** aus.
 2. Wählen Sie **Azure AD Connect** aus.
 3. Wählen Sie **Bereitstellung verwalten** aus.

 ![Verwalten der Bereitstellung](media/how-to-configure/manage1.png)
 
 4. Wählen Sie **Neue Konfiguration** aus.
 5. Wählen Sie auf dem Konfigurationsbildschirm Ihre Domäne aus, und geben Sie an, ob die Kennworthashsynchronisierung aktiviert werden soll.  Klicken Sie auf **Erstellen**.  
 
 ![Erstellen einer neuen Konfiguration](media/how-to-configure/configure1.png)


 6.  Der Bildschirm „Edit provisioning configuration“ (Bereitstellungskonfiguration bearbeiten) wird geöffnet.

   ![Bearbeiten der Konfiguration](media/how-to-configure/configure2.png)

 7. Geben Sie eine **E-Mail-Adresse für Benachrichtigungen** ein. An diese E-Mail-Adresse wird eine Benachrichtigung gesendet, wenn die Bereitstellung nicht fehlerfrei ist.
 8. Verschieben Sie den Auswahlregler auf Aktivieren, und wählen Sie Speichern aus.

## <a name="scope-provisioning-to-specific-users-and-groups"></a>Beschränken der Bereitstellung auf bestimmte Benutzer und Gruppen
Durch Verwenden lokaler Active Directory-Gruppen oder Organisationseinheiten können Sie den Agent so einrichten, dass nur bestimmte Benutzer und Gruppen synchronisiert werden. Gruppen und Organisationseinheiten können im Rahmen einer Konfiguration nicht konfiguriert werden. 

 1.  Wählen Sie im Azure-Portal die Option **Azure Active Directory** aus.
 2. Wählen Sie **Azure AD Connect** aus.
 3. Wählen Sie **Bereitstellung verwalten (Vorschau)** aus.
 4. Wählen Sie unter **Konfiguration** Ihre Konfiguration aus.

 ![Konfigurationsabschnitt](media/how-to-configure/scope1.png)
 
 5. Wählen Sie unter **Konfigurieren** die Option **Edit scoping filters** (Bereichsfilter bearbeiten) aus, um den Bereich der Konfigurationsregel zu ändern.
 ![Bearbeiten des Bereichs](media/how-to-configure/scope3.png)
 7. Auf der rechten Seite können Sie den Bereich ändern.  Klicken Sie abschließend auf **Fertig** und **Speichern**.
 8. Nachdem Sie den Bereich geändert haben, sollten Sie die [Bereitstellung erneut starten](#restart-provisioning), um eine sofortige Synchronisierung der Änderungen einzuleiten.

## <a name="attribute-mapping"></a>Attributzuordnung
Die Azure AD Connect-Cloudbereitstellung ermöglicht das einfache Zuordnen von Attributen zwischen lokalen Benutzer-/Gruppenobjekten und den Objekten in Azure AD.  Sie können die Standardattributzuordnungen den Anforderungen Ihres Unternehmens entsprechend anpassen. Dies bedeutet, dass Sie vorhandene Attributzuordnungen ändern oder löschen und neue Attributzuordnungen erstellen können.  Weitere Informationen finden Sie unter [Attributzuordnung bei der Azure AD Connect-Cloudbereitstellung](how-to-attribute-mapping.md).

## <a name="on-demand-provisioning"></a>Bedarfsorientierte Bereitstellung
Die Azure AD Connect-Cloudbereitstellung ermöglicht Ihnen das Testen von Konfigurationsänderungen, indem diese Änderungen auf einen einzelnen Benutzer oder eine einzelne Gruppe angewendet werden.  Dadurch können Sie überprüfen, ob die an der Konfiguration vorgenommenen Änderungen richtig angewendet wurden und ordnungsgemäß mit Azure AD synchronisiert werden.  Weitere Informationen finden Sie unter [Bedarfsorientierte Bereitstellung mit Azure AD Connect-Cloudbereitstellung](how-to-on-demand-provision.md).

## <a name="restart-provisioning"></a>Erneutes Starten der Bereitstellung 
Wenn Sie nicht auf die nächste geplante Ausführung warten möchten, können Sie mithilfe der Schaltfläche **Bereitstellung neu starten** die Ausführung der Bereitstellung auslösen. 
 1.  Wählen Sie im Azure-Portal die Option **Azure Active Directory** aus.
 2. Wählen Sie **Azure AD Connect** aus.
 3.  Wählen Sie **Bereitstellung verwalten (Vorschau)** aus.
 4. Wählen Sie unter **Konfiguration** Ihre Konfiguration aus.

   ![Konfigurationsauswahl zum Neustart der Bereitstellung](media/how-to-configure/scope1.png)

 5. Wählen Sie am oberen Rand **Bereitstellung neu starten** aus.

## <a name="remove-a-configuration"></a>Entfernen einer Konfiguration
Führen Sie die folgenden Schritte aus, um eine Konfiguration zu löschen.

 1.  Wählen Sie im Azure-Portal die Option **Azure Active Directory** aus.
 2. Wählen Sie **Azure AD Connect** aus.
 3. Wählen Sie **Bereitstellung verwalten (Vorschau)** aus.
 4. Wählen Sie unter **Konfiguration** Ihre Konfiguration aus.
   
   ![Konfigurationsauswahl zum Entfernen der Konfiguration](media/how-to-configure/scope1.png)

 5. Wählen Sie am oberen Rand des Konfigurationsbildschirms **Löschen** aus.

>[!IMPORTANT]
>Vor dem Löschen einer Konfiguration wird keine Bestätigung ausgegeben. Stellen Sie sicher, dass es sich um die Aktion handelt, die Sie ausführen möchten, bevor Sie **Löschen** auswählen.


## <a name="next-steps"></a>Nächste Schritte 

- [Was ist die Identitätsbereitstellung?](what-is-provisioning.md)
- [Was ist die Azure AD Connect-Cloudbereitstellung?](what-is-cloud-provisioning.md)
