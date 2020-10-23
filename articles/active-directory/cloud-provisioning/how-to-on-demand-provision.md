---
title: Azure AD Connect-Cloudbereitstellung – bedarfsorientierte Bereitstellung
description: In diesem Artikel wird die Funktion zur bedarfsorientierten Bereitstellung beschrieben.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 09/14/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 72623d69f9fb0cbc68758a362bb977e5e02d2c00
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91636930"
---
# <a name="azure-ad-connect-cloud-provisioning-on-demand-provisioning"></a>Azure AD Connect-Cloudbereitstellung – bedarfsorientierte Bereitstellung

Die Azure AD Connect-Cloudbereitstellung umfasst eine neue Funktion, die Ihnen das Testen von Konfigurationsänderungen erlaubt, indem diese Änderungen auf einen einzelnen Benutzer angewendet werden.  So können Sie überprüfen, ob die an der Konfiguration vorgenommenen Änderungen richtig angewendet wurden und ordnungsgemäß mit Azure AD synchronisiert werden.  

> [!IMPORTANT] 
> Wenn Sie die bedarfsorientierte Bereitstellung verwenden, werden die Bereichsfilter nicht auf den von Ihnen ausgewählten Benutzer angewendet.  Dies bedeutet, dass Sie die bedarfsorientierte Bereitstellung für Benutzer verwenden können, die sich nicht in den Organisationseinheiten befinden, die Sie angegeben haben.


## <a name="using-on-demand-provisioning"></a>Verwenden der bedarfsorientierten Bereitstellung
Führen Sie die folgenden Schritte aus, um die neue Funktion zu verwenden.


1.  Wählen Sie im Azure-Portal die Option **Azure Active Directory** aus.
2.  Wählen Sie **Azure AD Connect** aus.
3.  Wählen Sie **Bereitstellung verwalten** aus.

    ![Verwalten der Bereitstellung](media/how-to-configure/manage1.png)
4. Wählen Sie unter **Konfiguration** Ihre Konfiguration aus.
5. Klicken Sie unter **Validieren** auf die Schaltfläche **Benutzer bereitstellen**. 

 ![Bereitstellen eines Benutzers](media/how-to-on-demand-provision/on-demand2.png)

6. Führen Sie die folgenden Schritte auf dem Bildschirm für die bedarfsorientierte Bereitstellung aus.  Geben Sie den **Distinguished Name** eines Benutzers ein, und klicken Sie auf die Schaltfläche **Bereitstellen**.  
 
 ![Bedarfsorientierte Bereitstellung](media/how-to-on-demand-provision/on-demand3.png)
7. Sobald der Vorgang abgeschlossen ist, sollten ein Erfolgsbildschirm und vier grüne Kontrollkästchen angezeigt werden, die darauf hinweisen, dass die Bereitstellung erfolgreich war.  Fehler werden auf der linken Seite angezeigt.

  ![Erfolg](media/how-to-on-demand-provision/on-demand4.png)

Nun können Sie sich den Benutzer ansehen und überprüfen, ob die Änderungen, die Sie an der Konfiguration vorgenommen haben, angewendet wurden.  Im verbleibenden Teil dieses Dokuments werden die einzelnen Abschnitte beschrieben, die in den Details zu einem erfolgreich synchronisierten Benutzer angezeigt werden.

## <a name="import-user-details"></a>Benutzer importieren – Details
Dieser Abschnitt enthält Informationen zu dem Benutzer, der aus Active Directory importiert wurde.  Hier sehen Sie den Benutzer vor der Bereitstellung in Azure AD.  Klicken Sie auf den Link **Details anzeigen**, um diese Informationen anzuzeigen.

![Benutzer importieren](media/how-to-on-demand-provision/on-demand5.png)

Mithilfe dieser Informationen können Sie die verschiedenen Attribute und deren Werte sehen, die importiert wurden.  Wenn Sie eine benutzerdefinierte Attributzuordnung erstellt haben, wird der Wert hier angezeigt.
![Benutzer importieren – Details](media/how-to-on-demand-provision/on-demand6.png)

## <a name="determine-if-user-is-in-scope-details"></a>Gültigkeitsbereich des Benutzers ermitteln – Details
Dieser Abschnitt enthält Informationen über den Bereich des in Azure AD importierten Benutzers.  Klicken Sie auf den Link **Details anzeigen**, um diese Informationen anzuzeigen.

![Benutzerbereich](media/how-to-on-demand-provision/on-demand7.png)

Anhand dieser Informationen können Sie zusätzliche Informationen über den Bereich Ihrer Benutzer anzeigen.

![Benutzerbereich – Details](media/how-to-on-demand-provision/on-demand10a.png)

## <a name="match-user-between-source-and-target-system-details"></a>Benutzer zwischen Quell- und Zielsystem abgleichen – Details
In diesem Abschnitt finden Sie Informationen dazu, ob der Benutzer bereits in Azure AD vorhanden ist und ob eine Zusammenführung vorgenommen werden soll, statt einen neuen Benutzer bereitzustellen.  Klicken Sie auf den Link **Details anzeigen**, um diese Informationen anzuzeigen.
![Details anzeigen](media/how-to-on-demand-provision/on-demand8.png)

Anhand dieser Informationen können Sie feststellen, ob eine Entsprechung gefunden wurde oder ob ein neuer Benutzer erstellt wird.

![Benutzerinformationen](media/how-to-on-demand-provision/on-demand11.png)

In den entsprechenden Details wird eine Meldung mit einem der drei folgenden Vorgänge angezeigt.  Sie lauten wie folgt:
- Erstellen: Ein Benutzer wird in Azure AD erstellt.
- Aktualisieren: Ein Benutzer wird basierend auf einer an der Konfiguration vorgenommenen Änderung aktualisiert.
- Löschen: Ein Benutzer wird aus Azure AD entfernt.

Die genaue Meldung hängt vom Typ des ausgeführten Vorgangs ab.

## <a name="perform-action-details"></a>Aktion ausführen – Details
Dieser Abschnitt enthält Informationen zu dem Benutzer, der nach dem Anwenden der Konfiguration in Azure AD bereitgestellt oder exportiert wurde.  Hier sehen Sie den Benutzer nach der Bereitstellung in Azure AD.  Klicken Sie auf den Link **Details anzeigen**, um diese Informationen anzuzeigen.
![Aktion ausführen – Details](media/how-to-on-demand-provision/on-demand9.png)

Anhand dieser Informationen können Sie die Werte der Attribute ermitteln, nachdem die Konfiguration angewendet wurde.  Entsprechen Sie den importierten Werten, oder weichen sie ab?  Wird die Konfiguration erfolgreich angewendet?  

Mithilfe dieses Prozesses können Sie die Attributtransformation über die Cloud zum Azure AD-Mandanten nachverfolgen.

![Nachverfolgen von Attributen](media/how-to-on-demand-provision/on-demand12.png)

## <a name="next-steps"></a>Nächste Schritte 

- [Was ist die Azure AD Connect-Cloudbereitstellung?](what-is-cloud-provisioning.md)
- [Installieren des Agents für die Azure AD Connect-Cloudbereitstellung](how-to-install.md)
 