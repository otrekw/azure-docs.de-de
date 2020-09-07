---
title: Überwachen der Sicherheit Ihrer Container in Azure Security Center
description: Erfahren Sie, wie Sie den Sicherheitsstatus Ihrer Container über Azure Security Center überprüfen.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 06/30/2020
ms.author: memildin
ms.openlocfilehash: 6f5479ad31f45a92b0e4cd03f07bdd9e7930f543
ms.sourcegitcommit: c94a177b11a850ab30f406edb233de6923ca742a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2020
ms.locfileid: "89277288"
---
# <a name="monitor-the-security-of-your-containers"></a>Überwachen der Sicherheit Ihrer Container

Auf dieser Seite wird erläutert, wie Sie die Sicherheitsfunktionen für Container verwenden, die im Konzeptartikel [Containersicherheit](container-security.md) beschrieben werden.


## <a name="scan-your-arm-based-container-registries-for-vulnerabilities"></a>Überprüfen von ARM-basierten Containerregistrierungen auf Sicherheitsrisiken 

1. So aktivieren Sie die Überprüfung auf Sicherheitsrisiken für Ihre Azure Container Registry-Images:

    1. Stellen Sie sicher, dass Sie den Standard-Tarif von Azure Security Center nutzen.

    1. Aktivieren Sie auf der Seite **Preise und Einstellungen** das optionale Paket „Containerregistrierungen“ für Ihr Abonnement: ![Aktivieren des Pakets „Containerregistrierungen“](media/monitor-container-security/enabling-container-registries-bundle.png)

        Security Center ist jetzt bereit, die Images zu überprüfen, die per Pushvorgang in die Registrierung übertragen werden. 

        >[!NOTE]
        >Dieses Feature wird pro Image abgerechnet.


1. Um die Überprüfung eines Images auszulösen, übertragen Sie es per Push in Ihre Registrierung. 

    Wenn die Überprüfung abgeschlossen ist (in der Regel nach ungefähr zwei Minuten, sie kann aber auch bis zu 15 Minuten dauern), sind die Ergebnisse als Security Center-Empfehlungen wie diese verfügbar:

1. Auf der Seite **Empfehlungen** können Sie sich die Ergebnisse ansehen. Wenn Probleme gefunden wurden, wird die folgende Empfehlung angezeigt:

    ![Empfehlung zum Beheben von Problemen ](media/monitor-container-security/acr-finding.png)

1. Wählen Sie die Empfehlung aus. 
    Die Detailseite der Empfehlung wird geöffnet und zeigt zusätzliche Informationen an. Diese Informationen umfassen die Liste der Registrierungen mit anfälligen Images („Betroffene Ressourcen“) und die Schritte zum Beheben des Problems. 

1. Wählen Sie eine bestimmte Registrierung aus, um die darin enthaltenen Repositorys mit möglicherweise gefährdeten Images anzuzeigen.

    ![Registrierung auswählen](media/monitor-container-security/acr-finding-select-registry.png)

    Die Detailseite der Registrierung wird geöffnet und zeigt eine Liste der betroffenen Repositorys an.

1. Wählen Sie ein bestimmtes Repository aus, um die darin enthaltenen Repositorys mit möglicherweise gefährdeten Images anzuzeigen.

    ![Repository auswählen](media/monitor-container-security/acr-finding-select-repository.png)

    Die Detailseite für das Repository wird geöffnet. Sie zeigt die möglicherweise gefährdeten Images sowie eine Bewertung des Schweregrads der Ergebnisse an.

1. Wählen Sie ein bestimmtes Image aus, um die Sicherheitsrisiken anzuzeigen.

    ![Image auswählen](media/monitor-container-security/acr-finding-select-image.png)

    Die Liste der Ergebnisse für das ausgewählte Image wird geöffnet.

    ![Liste der Ergebnisse](media/monitor-container-security/acr-findings.png)

1. Um mehr über ein Ergebnis zu erfahren, wählen Sie es aus. 

    Der Detailbereich für das Ergebnis wird geöffnet.

    [![Detailbereich für Ergebnis](media/monitor-container-security/acr-finding-details-pane.png)](media/monitor-container-security/acr-finding-details-pane.png#lightbox)

    Dieser Bereich enthält eine detaillierte Beschreibung des Problems sowie Links zu externen Ressourcen, die bei der Minimierung der Bedrohungen helfen können.

1. Führen Sie die in diesem Bereich angegebenen Schritte zur Problembehebung aus.

1. Wenn Sie die Schritte zum Beheben des Sicherheitsproblems ausgeführt haben, ersetzen Sie das Image in Ihrer Registrierung:

    1. Übertragen Sie das aktualisierte Image per Pushvorgang. Damit wird eine Überprüfung ausgelöst. 
    
    1. Suchen Sie auf der Empfehlungsseite nach der Empfehlung „Sicherheitsrisiken in Azure Container Registry-Images müssen behoben werden“. 
    
        Wenn die Empfehlung weiterhin angezeigt wird und das bearbeitete Image sich immer noch in der Liste der anfälligen Images befindet, überprüfen Sie die Schritte zur Problembehebung erneut.

    1. Wenn Sie sicher sind, dass das aktualisierte Image per Push übertragen und überprüft wurde und nicht mehr in der Empfehlung angezeigt wird, löschen Sie das „alte“, anfällige Image aus der Registrierung.


## <a name="harden-your-containers-docker-hosts"></a>Härten der Docker-Hosts Ihrer Container

Security Center überwacht die Konfiguration Ihrer Docker-Hosts kontinuierlich und generiert Sicherheitsempfehlungen gemäß Branchenstandards.

So zeigen Sie die Sicherheitsempfehlungen von Azure Security Center für die Docker-Hosts Ihrer Container an:

1. Öffnen Sie auf der Navigationsleiste von Security Center die Option **Compute und Apps**, und wählen Sie die Registerkarte **Container** aus.

1. Optional können Sie die Liste Ihrer Containerressourcen nach Containerhosts filtern.

    ![Filter für Containerressourcen](media/monitor-container-security/container-resources-filter.png)

1. Wählen Sie in der Liste Ihrer Containerhosts einen Computer aus, den Sie genauer untersuchen möchten.

    ![Empfehlung für Containerhosts](media/monitor-container-security/container-resources-filtered-to-hosts.png)

    Die **Seite mit Informationen zum Containerhost** wird geöffnet und zeigt Details zum Host sowie eine Liste mit Empfehlungen an.

1. Wählen Sie in der Empfehlungsliste eine Empfehlung aus, um diese genauer zu untersuchen.

    ![Liste der Empfehlungen für Containerhosts](media/monitor-container-security/container-host-rec.png)

1. Optional können Sie die Beschreibung, weitere Informationen, Bedrohungen und Schritte zur Bereinigung lesen. 

1. Klicken Sie unten auf der Seite auf **Aktion ausführen**.

    [![Schaltfläche „Aktion ausführen“](media/monitor-container-security/host-security-take-action-button.png)](media/monitor-container-security/host-security-take-action.png#lightbox)

    Log Analytics wird geöffnet und zeigt einen benutzerdefinierten Vorgang, der direkt ausgeführt werden kann. Die standardmäßige benutzerdefinierte Abfrage enthält eine Liste aller fehlerhaften Regeln, die bewertet wurden, sowie Anleitungen zum Beheben der Probleme.

    [![Log Analytics-Aktion](media/monitor-container-security/log-analytics-for-action-small.png)](media/monitor-container-security/log-analytics-for-action.png#lightbox)

1. Optimieren Sie die Abfrageparameter bei Bedarf.

1. Wenn Sie sicher sind, dass der Befehl für Ihren Host geeignet und bereit ist, wählen Sie **Ausführen** aus.



## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie die Security Center-Features für die Containersicherheit verwenden. 

Weitere verwandte Informationen finden Sie auf den folgenden Seiten: 

- [Security Center-Empfehlungen für Container](recommendations-reference.md#recs-containers)
- [Warnungen auf AKS-Clusterebene](alerts-reference.md#alerts-akscluster)
- [Warnungen auf Containerhostebene](alerts-reference.md#alerts-containerhost)
