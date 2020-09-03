---
title: Selbstdiagnose und Lösung von Problemen in Azure Spring Cloud
description: Erfahren Sie mehr über Selbstdiagnose und Lösung von Problemen in Azure Spring Cloud.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: how-to
ms.date: 05/29/2020
ms.custom: devx-track-java
ms.openlocfilehash: d8f3b4d57f00ef33bbf566c36babafd83be9a5df
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89299625"
---
# <a name="how-to-self-diagnose-and-solve-problems-in-azure-spring-cloud"></a>Selbstdiagnose und Lösung von Problemen in Azure Spring Cloud
Die Azure Spring Cloud-Diagnose ist ein interaktives Verfahren, das Sie bei der Problembehandlung Ihrer App unterstützt. Es ist keine Konfiguration erforderlich. Wenn Sie Probleme feststellen, zeigt die Azure Spring Cloud-Diagnose, wo der Fehler liegt, und stellt Informationen breit, die Ihnen helfen, das Problem zu beheben und zu lösen.

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Tutorial benötigen Sie Folgendes:

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.
* Eine bereitgestellte Azure Spring Cloud-Dienstinstanz. Informationen zu den ersten Schritten finden Sie unter [Schnellstart: Starten einer Java Spring-Anwendung mit der Azure CLI](spring-cloud-quickstart.md).
* Mindestens eine bereits in Ihrer Dienstinstanz erstellte Anwendung.

## <a name="navigate-to-the-diagnostics-page"></a>Navigieren zur Diagnoseseite
1. Melden Sie sich beim Azure-Portal an.
2. Navigieren Sie zur Seite **Übersicht** von Azure Spring Cloud.
3. Öffnen Sie **Probleme diagnostizieren und lösen** im Menü links auf der Seite.

 ![Dialogfeld für Diagnose und Lösung](media/spring-cloud-diagnose/diagnose-solve-dialog.png)

 ## <a name="search-logged-issues"></a>Suchen nach protokollierten Problemen
Um ein Problem zu finden, können Sie entweder ein Schlüsselwort eingeben oder auf eine Lösungsgruppe klicken, um alle Lösungen in dieser Kategorie zu untersuchen.

 ![Suchen nach Problemen](media/spring-cloud-diagnose/search-detectors.png)

Die Auswahl von **Integritätsüberprüfung des Konfigurationsservers**, **Integritätsstatus des Konfigurationsservers** oder **Updateverlauf des Konfigurationsservers** zeigt verschiedene Ergebnisse an.

![Problemoptionen](media/spring-cloud-diagnose/detectors-options.png)

Suchen Sie nach der Zielerkennung, und klicken Sie darauf, um sie auszuführen. Nach der Ausführung der Erkennung wird eine Zusammenfassung der Diagnose angezeigt. Sie können **Vollständigen Bericht anzeigen** auswählen, um Diagnosedetails zu überprüfen, oder klicken Sie auf die Schaltfläche **Kachelmenü anzeigen**, um zur Erkennungsliste zurückzukehren.

 ![Diagnosezusammenfassung](media/spring-cloud-diagnose/summary-diagnostics.png)

Auf der Seite mit den Diagnosedetails können Sie den Diagnosezeitraum mit dem Controller in der oberen rechten Ecke ändern. Um weitere Metriken oder Protokolle anzuzeigen, schalten Sie jede Diagnose um. Es kann eine Verzögerung von 15 Minuten für Metriken und Protokolle auftreten.

 ![Diagnosedetails](media/spring-cloud-diagnose/diagnostics-details.png)

Einige Ergebnisse enthalten zugehörige Dokumentation.

 ![Zugehörige Details](media/spring-cloud-diagnose/related-details.png)

## <a name="next-steps"></a>Nächste Schritte
* [Überwachen von Spring Cloud-Ressourcen mithilfe von Warnungen und Aktionsgruppen](spring-cloud-tutorial-alerts-action-groups.md)
* [Sicherheitskontrollen für den Azure Spring Cloud-Dienst](spring-cloud-concept-security-controls.md)