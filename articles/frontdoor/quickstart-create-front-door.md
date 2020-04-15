---
title: 'Schnellstart: Erstellen eines Front Door-Profils für Hochverfügbarkeit von Anwendungen'
description: In diesem Schnellstartartikel wird die Vorgehensweise zum Erstellen einer Front Door-Instanz für globale Webanwendungen mit Hochverfügbarkeit und hoher Leistung beschrieben.
services: front-door
documentationcenter: ''
author: sharad4u
editor: ''
ms.assetid: ''
ms.service: frontdoor
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/31/2018
ms.author: sharadag
ms.openlocfilehash: 2b44c0cdbe2d955efe20a5f9473a29bc9f500a07
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2020
ms.locfileid: "80521452"
---
# <a name="quickstart-create-a-front-door-for-a-highly-available-global-web-application"></a>Schnellstart: Erstellen Sie eine „Front Door“ für eine hoch verfügbare globale Webanwendung.

In diesem Schnellstart wird beschrieben, wie Sie ein Front Door-Profil erstellen, das Hochverfügbarkeit und hohe Leistung für Ihre globale Webanwendung sicherstellt. 

Das in dieser Schnellstartanleitung beschriebene Szenario umfasst zwei Instanzen einer Webanwendung, die in verschiedenen Azure-Regionen ausgeführt werden. Es wird eine Front Door-Konfiguration auf der Grundlage von [Back-Ends mit gleicher Gewichtung und Priorität](front-door-routing-methods.md) erstellt, die dabei hilft, Benutzerdatenverkehr an die nächstgelegenen Standort-Back-Ends, auf denen die Anwendung ausgeführt wird, weiterzuleiten. Front Door überwacht kontinuierlich die Webanwendung und bietet automatisches Failover zum nächsten verfügbaren Back-End, wenn der nächstgelegene Standort nicht verfügbar ist.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure 
Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="prerequisites"></a>Voraussetzungen
Für diesen Schnellstart müssen Sie zwei Instanzen einer Webanwendung bereitgestellt haben, die in verschiedenen Azure-Regionen (*USA, Osten* und *Europa, Westen*) ausgeführt werden. Beide Webanwendungsinstanzen werden im Aktiv/Aktiv-Modus ausgeführt, d.h., jede von ihnen kann jederzeit Datenverkehr annehmen. Im Gegensatz dazu dient in einer Aktiv/Standby-Konfiguration ein Knoten als Failover.

1. Klicken Sie links oben auf dem Bildschirm auf **Ressource erstellen** > **Web** > **Web-App**.
2. Geben Sie unter **Web-App** die folgenden Informationen ein, oder wählen Sie sie aus, und geben Sie Standardeinstellungen ein, wenn noch keine Angaben gemacht wurden:

     | Einstellung         | Wert     |
     | ---              | ---  |
     | Resource group          | Wählen Sie **Neu** aus, und geben Sie *myResourceGroupFD1* ein. |
     | Name           | Geben Sie einen eindeutigen Namen für Ihre Web-App ein.  |
     | Laufzeitstapel          | Wählen Sie einen Runtimestapel für Ihre App aus |
     |      Region  |   USA (Westen)        |
     | App Service-Plan/Standort         | Wählen Sie **Neu**aus.  Geben Sie als App Service-Plan *myAppServicePlanEastUS* ein, und klicken Sie dann auf **OK**.| 
     |SKU und Größe  | Wählen Sie **Größe ändern** aus. Wählen Sie **Standard S1, 100 ACU insgesamt, 1,75 GB Arbeitsspeicher** aus |
     
3. Klicken Sie auf **Überprüfen + erstellen**.
4. Überprüfen Sie die Informationen in der Zusammenfassung für die Web-App. Klicken Sie auf **Erstellen**.
5. Wenn die Web-App erfolgreich bereitgestellt wurde, wird nach ca. fünf Minuten eine Standardwebsite erstellt.
6. Wiederholen Sie die Schritte 1 bis 3, um eine zweite Website in einer anderen Azure-Region mit den folgenden Einstellungen zu erstellen:

     | Einstellung         | Wert     |
     | ---              | ---  |
     | Resource group          | Wählen Sie **Neu** aus, und geben Sie *myResourceGroupFD2* ein. |
     | Name           | Geben Sie einen eindeutigen Namen für Ihre Web-App ein.  |
     | Laufzeitstapel          | Wählen Sie einen Runtimestapel für Ihre App aus |
     |      Region  |   Europa, Westen      |
     | App Service-Plan/Standort         | Wählen Sie **Neu**aus.  Geben Sie als App Service-Plan *myAppServicePlanWestEurope* ein, und klicken Sie dann auf **OK**.|   
     |SKU und Größe  | Wählen Sie **Größe ändern** aus. Wählen Sie **Standard S1, 100 ACU insgesamt, 1,75 GB Arbeitsspeicher** aus |
    
## <a name="create-a-front-door-for-your-application"></a>Erstellen einer Front Door-Instanz für Ihre Anwendung
### <a name="a-add-a-frontend-host-for-front-door"></a>A. Hinzufügen eines Front-End-Hosts für Front Door
Erstellen Sie eine Front Door-Konfiguration, die den Benutzerdatenverkehr basierend auf der geringsten Wartezeit zwischen zwei Back-Ends weiterleitet.

1. Klicken Sie links oben auf dem Bildschirm auf **Ressource erstellen** > **Netzwerk** > **Front Door**.
2. Geben Sie unter **Frontdoor-Instanz erstellen** die folgenden Informationen ein, oder wählen Sie sie aus, und geben Sie Standardeinstellungen ein, wenn noch keine Angaben gemacht wurden:

     | Einstellung         | Wert     |
     | ---              | ---  |
     |Subscription  | Wählen Sie das gewünschte Abonnement aus, in dem die Front Door-Instanz erstellt werden soll.|
     | Resource group          | Wählen Sie **Neu** aus, und geben Sie *myResourceGroupFD0* ein |
     | Ressourcengruppenstandort  |   USA (Mitte)        |
     
     > [!NOTE]
     > Sie müssen keine neue Ressourcengruppe erstellen, um Front Door bereitzustellen.  Sofern möglich, können Sie auch eine vorhandene Ressourcengruppe auswählen.
     
3. Klicken Sie auf **Weiter: Konfiguration** aus.
4. Klicken Sie auf der Karte „Front-Ends/Domänen“ auf das Pluszeichen.  Geben Sie für **Hostname** Folgendes ein: `<Your Initials>frontend`. Dieser Hostname muss global eindeutig sein, dies wird durch Front Door geprüft.
5. Klicken Sie auf **Hinzufügen**.

### <a name="b-add-application-backend-and-backend-pools"></a>B. Hinzufügen des Anwendung-Back-Ends und von Back-End-Pools

Als Nächstes müssen Sie Ihre Front-Ends/Domänen und Anwendungs-Back-Ends in einem Back-End-Pool konfigurieren, damit Front Door weiß, wo sich Ihre Anwendung befindet. 

1. Klicken Sie auf der Karte „Back-End-Pools“ auf das Pluszeichen, um einen Back-End-Pool hinzuzufügen. Geben Sie für **Name** für Ihren Back-End-Pool `myBackendPool` ein.
2. Klicken Sie anschließend auf **Back-End hinzufügen**, um die zuvor erstellten Websites hinzuzufügen.
3. Wählen Sie für **Back-End-Hosttyp** die Option „App Service“ aus. Wählen Sie das Abonnement aus, in dem Sie die Website erstellt haben, und wählen Sie dann die erste Website in der Dropdownliste **Zielhostname** aus.
4. Lassen Sie die übrigen Felder zunächst unverändert, und klicken Sie auf **Hinzufügen**.
5. Wählen Sie für **Back-End-Hosttyp** die Option „App Service“ aus. Wählen Sie das Abonnement aus, in dem Sie die Website erstellt haben, und wählen Sie dann die **zweite** Website in der Dropdownliste **Zielhostname** aus.
6. Lassen Sie die übrigen Felder zunächst unverändert, und klicken Sie auf **Hinzufügen**.
7. Optional können Sie die Integritätstests und Lastenausgleichseinstellungen für den Back-End-Pool aktualisieren, aber die Standardwerte sollten ebenfalls funktionieren. Klicken Sie in einem solchen Fall auf **Hinzufügen**.


### <a name="c-add-a-routing-rule"></a>C. Hinzufügen einer Routingregel
1. Klicken Sie abschließend auf der Karte „Routingregeln“ auf das Pluszeichen, um eine Routingregel zu konfigurieren. Dies ist erforderlich, um die Front-End-Hosts dem Back-End-Adresspool zuzuordnen. Dabei wird letztlich konfiguriert, dass bei `myappfrontend.azurefd.net` eingehende Anforderungen an den Back-End-Pool `myBackendPool` weitergeleitet werden sollen. 
2. Geben Sie für **Name** „LocationRule“ ein.
3. Klicken Sie auf **Hinzufügen**, um die Routingregel für Ihre Front Door-Instanz hinzuzufügen. 
4. Klicken Sie auf **Überprüfen und erstellen**.
5. Überprüfen Sie die Einstellungen zum Erstellen der Front Door-Instanz. Klicken Sie auf **Erstellen**

>[!WARNING]
> Sie **müssen** sicherstellen, dass jeder Front-End-Host in Ihrer Front Door-Instanz eine Routingregel mit einem Standardpfad („/\*“) aufweist. Das bedeutet, für alle Ihre Routingregeln muss mindestens eine Routingregel für jeden Ihrer Front-End-Hosts unter dem Standardpfad („/\*“) definiert sein. Andernfalls wird der Datenverkehr Ihrer Endbenutzer möglicherweise nicht richtig weitergeleitet.

## <a name="view-front-door-in-action"></a>Anzeigen von Front Door in Aktion
Nachdem Sie eine Front Door-Instanz erstellt haben, dauert es einige Minuten, bis die Konfiguration global bereitgestellt ist. Greifen Sie dann auf den erstellten Front-End-Host zu, d.h., wechseln Sie zu einem Webbrowser, und rufen Sie die URL `myappfrontend.azurefd.net` auf. Ihre Anforderung wird automatisch an das Ihnen nächstgelegene Back-End aus den angegebenen Back-Ends im Back-End-Pool weitergeleitet. 

### <a name="view-front-door-handle-application-failover"></a>Anzeigen des Anwendungsfailovers für das Front Door-Handle
Wenn Sie das sofortige globale Front Door-Failover in Aktion testen möchten, können Sie zu einer der erstellten Websites wechseln und diese beenden. Entsprechend den definierten Integritätstesteinstellungen für den Back-End-Pool findet unverzüglich ein Failover des Datenverkehrs zur anderen Websitebereitstellung statt. Sie können das Verhalten auch testen, indem Sie das Back-End in der Back-End-Poolkonfiguration für Ihre Front Door-Instanz deaktivieren. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn die Ressourcengruppen **myResourceGroupFD1**, **myResourceGroupFD2** und **myResourceGroupFD0** nicht mehr benötigt werden, löschen Sie die Gruppen:

## <a name="next-steps"></a>Nächste Schritte
In diesem Schnellstart haben Sie eine Front Door-Instanz erstellt, mit der Sie den Benutzerdatenverkehr für Webanwendungen, die Hochverfügbarkeit und maximale Leistung erfordern, weiterleiten können. Weitere Informationen zum Weiterleiten von Datenverkehr finden unter den von Front Door verwendeten [Routingmethoden](front-door-routing-methods.md).
