---
title: 'Tutorial: Überprüfungen der Einhaltung gesetzlicher Bestimmungen: Azure Security Center'
description: 'Tutorial: Es wird beschrieben, wie Sie die Einhaltung gesetzlicher Bestimmungen verbessern, indem Sie Azure Security Center verwenden.'
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 5f50c4dc-ea42-418d-9ea8-158ffeb93706
ms.service: security-center
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/10/2021
ms.author: memildin
ms.openlocfilehash: fb8dc22c923b7b53a6263baa43046862af4d2f04
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100370262"
---
# <a name="tutorial-improve-your-regulatory-compliance"></a>Tutorial: Verbessern der Einhaltung gesetzlicher Vorschriften

Mit Azure Security Center kann der Prozess zur Einhaltung von Anforderungen gesetzlicher Bestimmungen optimiert werden, indem das **Dashboard zur Einhaltung gesetzlicher Bestimmungen** verwendet wird. 

Von Security Center wird Ihre Hybrid Cloud-Umgebung kontinuierlich bewertet, um die Risikofaktoren gemäß den Kontrollen und bewährten Methoden in den Standards, die auf Ihre Abonnements angewendet wurden, zu analysieren. Das Dashboard gibt den Status Ihrer Einhaltung dieser Standards wieder. 

Wenn Sie Security Center für ein Azure-Abonnement aktivieren, wird diesem Abonnement automatisch der [Azure-Sicherheitsvergleichstest](../security/benchmarks/introduction.md) zugewiesen. Diese weit verbreitete Benchmark basiert auf den Kontrollen des [Center for Internet Security (CIS)](https://www.cisecurity.org/benchmark/azure/) und des [National Institute of Standards and Technology (NIST)](https://www.nist.gov/) und konzentriert sich auf cloudzentrierte Sicherheit.

Im Dashboard zur Einhaltung gesetzlicher Bestimmungen wird der Status aller Bewertungen angezeigt, die in Ihrer Umgebung für Ihre ausgewählten Standards und Bestimmungen durchgeführt werden. Wenn Sie aufgrund der Empfehlungen Maßnahmen ergreifen und die Risikofaktoren Ihrer Umgebung reduzieren, verbessert sich Ihr Konformitätsstatus.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Evaluieren der Einhaltung gesetzlicher Bestimmungen im entsprechenden Dashboard
> * Verbessern Ihres Konformitätsstatus durch das Ergreifen von Maßnahmen aufgrund von Empfehlungen
> * Einrichten von Warnungen zu Änderungen Ihres Konformitätsstatus
> * Exportieren Ihrer Konformitätsdaten als kontinuierlichen Datenstrom und als wöchentliche Momentaufnahmen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchlaufen der in diesem Tutorial behandelten Features müssen folgende Voraussetzungen erfüllt sein:

- [Azure Defender](azure-defender.md) muss aktiviert sein. Azure Defender kann 30 Tage lang kostenlos getestet werden.
- Sie müssen mit einem Konto angemeldet sein, das über Lesezugriff auf die Daten zur Richtlinienkonformität verfügt. (**Sicherheitsleseberechtigter** reicht nicht aus.) Hierzu kann die Rolle **Globaler Leser** für das Abonnement verwendet werden. Ihnen müssen mindestens die Rollen **Mitwirkender bei Ressourcenrichtlinien** und **Sicherheitsadministrator** zugewiesenen sein.

##  <a name="assess-your-regulatory-compliance"></a>Bewerten der Einhaltung gesetzlicher Bestimmungen

Auf dem Dashboard zur Einhaltung gesetzlicher Bestimmungen werden Ihre ausgewählten Konformitätsstandards mit allen zugehörigen Anforderungen angezeigt. Die unterstützten Anforderungen sind hierbei jeweils den entsprechenden Sicherheitsbewertungen zugeordnet. Der Status dieser Bewertungen spiegelt Ihre Einhaltung des Standards wider.

Verwenden Sie das Dashboard zur Einhaltung gesetzlicher Bestimmungen, um sich auf Konformitätslücken im Zusammenhang mit den von Ihnen ausgewählten Standards und Bestimmungen zu konzentrieren. Diese fokussierte Ansicht ermöglicht auch eine kontinuierliche Überwachung Ihrer Compliance in dynamischen Cloud- und Hybridumgebungen.

1. Wählen Sie im Security Center-Menü die Option **Einhaltung gesetzlicher Bestimmungen** aus.

    Im oberen Bildschirmbereich befindet sich ein Dashboard mit einer Übersicht über Ihren Konformitätsstatus mit den unterstützten Bestimmungen. Es sind Ihr Gesamtstatus der Konformität und die Anzahl von bestandenen und nicht bestandenen Bewertungen der einzelnen Standards angegeben.

    :::image type="content" source="./media/security-center-compliance-dashboard/compliance-dashboard.png" alt-text="Dashboard für die Einhaltung gesetzlicher Bestimmungen" lightbox="./media/security-center-compliance-dashboard/compliance-dashboard.png":::

1. Wählen Sie eine Registerkarte für einen Konformitätsstandard aus, der für Sie relevant ist (1). Sie sehen, auf welche Abonnements der Standard angewendet wird (2), und die Liste mit allen Kontrollen für den Standard (3) wird angezeigt. Sie können für eine Kontrolle jeweils die Details zu den bestandenen und nicht bestandenen Bewertungen (4) sowie die Anzahl betroffener Ressourcen (5) anzeigen. Einige Kontrollen sind abgeblendet. Diesen Kontrollen sind keine Security Center-Bewertungen zugeordnet. Überprüfen Sie deren Anforderungen, und bewerten Sie sie in Ihrer Umgebung. Einige sind möglicherweise prozessbezogen und nicht technischer Art.

    :::image type="content" source="./media/security-center-compliance-dashboard/compliance-drilldown.png" alt-text="Erkunden der Compliancedetails für einen bestimmten Standard":::

1. Wählen Sie **Bericht herunterladen** aus, um einen PDF-Bericht mit einer Zusammenfassung Ihres aktuellen Compliancestatus für einen bestimmten Standard zu generieren.

    Der Bericht enthält eine allgemeine Zusammenfassung des Konformitätsstatus für den ausgewählten Standard auf der Grundlage von Security Center-Bewertungsdaten. Der Bericht ist nach den Kontrollen dieses bestimmten Standards strukturiert. Der Bericht kann an Stakeholder weitergegeben und als Nachweis bei internen und externen Prüfungen genutzt werden.

    :::image type="content" source="./media/security-center-compliance-dashboard/download-report.png" alt-text="Herunterladen des Berichts zur Compliance":::

## <a name="improve-your-compliance-posture"></a>Verbessern Ihres Konformitätsstatus

Verbessern Sie mit den Informationen im Dashboard für die Einhaltung gesetzlicher Bestimmungen Ihren Konformitätsstatus, indem Sie direkt im Dashboard Maßnahmen zu den Empfehlungen ergreifen.

1.  Wählen Sie auf dem Dashboard eine beliebige der nicht bestandenen Bewertungen aus, um die Details zur jeweiligen Empfehlung anzuzeigen. Jede Empfehlung enthält verschiedene Problembehandlungsschritte.

1.  Wählen Sie eine bestimmte Ressource aus, um weitere Details anzuzeigen und die Ursache der Empfehlung für diese Ressource zu beheben. <br>Wählen Sie unter dem Standard **Azure CIS 1.1.0** beispielsweise die Empfehlung **Datenträgerverschlüsselung sollte auf virtuelle Computer angewendet werden** aus.

    :::image type="content" source="./media/security-center-compliance-dashboard/sample-recommendation.png" alt-text="Auswahl einer Empfehlung aus einem Standard führt direkt zur Seite mit den Details der Empfehlung":::

1. Wenn Sie in diesem Beispiel auf der Seite mit den Empfehlungsdetails die Option **Aktion ausführen** auswählen, gelangen Sie zu den Azure Virtual Machine-Seiten im Azure-Portal. Hier können Sie die Verschlüsselung auf der Registerkarte **Sicherheit** aktivieren:

    :::image type="content" source="./media/security-center-compliance-dashboard/encrypting-vm-disks.png" alt-text="Schaltfläche „Aktion ausführen“ auf der Seite mit den Details zur Empfehlung führt zu den Lösungsoptionen":::

    Weitere Informationen zur Anwendung der Empfehlungen finden Sie unter [Implementieren von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md).

1.  Nachdem Sie Schritte zur Umsetzung von Empfehlungen ausgeführt haben, wird das Ergebnis im Bericht des Compliancedashboards angezeigt, da sich Ihre Compliancebewertung verbessert hat.

    > [!NOTE]
    > Da die Bewertungen ungefähr alle zwölf Stunden durchgeführt werden, sehen die Auswirkungen auf Ihre Konformitätsdaten jeweils erst nach der nächsten Ausführung der relevanten Bewertung.


## <a name="export-your-compliance-status-data"></a>Exportieren Ihrer Daten zum Konformitätsstatus

Falls Sie Ihren Konformitätsstatus mit anderen Überwachungstools in Ihrer Umgebung nachverfolgen möchten, können Sie den einfachen Exportmechanismus von Security Center nutzen. Konfigurieren Sie den **fortlaufenden Export**, um ausgewählte Daten an eine Azure Event Hub-Instanz oder einen Log Analytics-Arbeitsbereich zu senden.

Verwenden Sie die Daten des fortlaufenden Exports an eine Azure Event Hub-Instanz oder einen Log Analytics-Arbeitsbereich wie folgt:

- Exportieren Sie alle Daten, die für die Einhaltung der gesetzlichen Vorgaben relevant sind, als **kontinuierlichen Datenstrom**:

    :::image type="content" source="media/security-center-compliance-dashboard/export-compliance-data-stream.png" alt-text="Fortlaufender Export von Daten zur Einhaltung gesetzlicher Bestimmungen" lightbox="media/security-center-compliance-dashboard/export-compliance-data-stream.png":::

- Exportieren Sie **wöchentliche Momentaufnahmen** Ihrer Daten zur Einhaltung gesetzlicher Bestimmungen:

    :::image type="content" source="media/security-center-compliance-dashboard/export-compliance-data-snapshot.png" alt-text="Fortlaufender Export einer wöchentlichen Momentaufnahme mit Daten zur Einhaltung gesetzlicher Bestimmungen" lightbox="media/security-center-compliance-dashboard/export-compliance-data-snapshot.png":::

Sie können auch direkt aus dem Dashboard für die Einhaltung gesetzlicher Bestimmungen einen **PDF-/CSV-Bericht** mit Ihren Konformitätsdaten exportieren:

:::image type="content" source="media/security-center-compliance-dashboard/export-compliance-data-report.png" alt-text="Exportieren Ihrer Daten für die Einhaltung gesetzlicher Bestimmungen als PDF- oder CSV-Bericht" lightbox="media/security-center-compliance-dashboard/export-compliance-data-report.png":::

Weitere Informationen finden Sie unter [Fortlaufendes Exportieren von Security Center-Daten](continuous-export.md).


## <a name="run-workflow-automations-when-there-are-changes-to-your-compliance"></a>Durchführen von Workflowautomatisierungen bei Änderungen Ihres Konformitätsstatus

Mit dem Security Center-Feature für die Workflowautomatisierung kann festgelegt werden, dass Logic Apps immer dann ausgelöst wird, wenn sich für eine Ihrer Bewertungen der Einhaltung gesetzlicher Bestimmungen der Status ändert.

Es kann beispielsweise sein, dass von Security Center eine E-Mail an einen bestimmten Benutzer gesendet werden soll, wenn dies aufgrund einer Bewertung der Konformität erforderlich ist. Sie müssen zunächst die Logik-App erstellen (mit [Azure Logic Apps](../logic-apps/logic-apps-overview.md)) und den Trigger dann für eine neue Workflowautomatisierung einrichten, wie unter [Automatisieren der Reaktionen auf Security Center-Trigger](workflow-automation.md) beschrieben.

:::image type="content" source="media/release-notes/regulatory-compliance-triggers-workflow-automation.png" alt-text="Verwenden von Änderungen bei Bewertungen der Einhaltung gesetzlicher Bestimmungen zum Auslösen der Workflowautomatisierung" lightbox="media/release-notes/regulatory-compliance-triggers-workflow-automation.png":::




## <a name="faq---regulatory-compliance-dashboard"></a>Häufig gestellte Fragen: Dashboard für die Einhaltung gesetzlicher Bestimmungen

- [Welche Standards werden auf dem Compliancedashboard unterstützt?](#what-standards-are-supported-in-the-compliance-dashboard)
- [Warum sind einige Kontrollen abgeblendet?](#why-do-some-controls-appear-grayed-out)
- [Wie kann ich einen integrierten Standard (beispielsweise PCI-DSS, ISO 27001 oder SOC2 TSP) aus dem Dashboard entfernen?](#how-can-i-remove-a-built-in-standard-like-pci-dss-iso-27001-or-soc2-tsp-from-the-dashboard)
- [Ich habe die in der Empfehlung vorgeschlagenen Änderungen vorgenommen, dies wird jedoch im Dashboard nicht widergespiegelt.](#i-made-the-suggested-changed-based-on-the-recommendation-yet-it-isnt-being-reflected-in-the-dashboard)
- [Welche Berechtigungen benötige ich für den Zugriff auf das Compliancedashboard?](#what-permissions-do-i-need-to-access-the-compliance-dashboard)
- [Das Dashboard für die Einhaltung gesetzlicher Bestimmungen lädt bei mir nicht.](#the-regulatory-compliance-dashboard-isnt-loading-for-me)
- [Wie kann ich einen nach Standard aufgeschlüsselten Bericht über bestandene und nicht bestandene Kontrollen auf meinem Dashboard anzeigen?](#how-can-i-view-a-report-of-passing-and-failing-controls-per-standard-in-my-dashboard)
- [Wie kann ich einen Bericht mit Konformitätsdaten in einem PDF-fremden Format herunterladen?](#how-can-i-download-a-report-with-compliance-data-in-a-format-other-than-pdf)
- [Wie kann ich Ausnahmen für einige der Richtlinien im Dashboard für die Einhaltung gesetzlicher Bestimmungen erstellen?](#how-can-i-create-exceptions-for-some-of-the-policies-in-the-regulatory-compliance-dashboard)
- [Welche Azure Defender-Pläne oder -Lizenzen benötige ich, um das Dashboard für die Einhaltung gesetzlicher Bestimmungen verwenden zu können?](#what-azure-defender-plans-or-licenses-do-i-need-to-use-the-regulatory-compliance-dashboard)

### <a name="what-standards-are-supported-in-the-compliance-dashboard"></a>Welche Standards werden auf dem Compliancedashboard unterstützt?
Standardmäßig wird auf dem Dashboard für die Einhaltung gesetzlicher Bestimmungen der Azure-Sicherheitsvergleichstest angezeigt. Beim Azure-Sicherheitsvergleichstest handelt es sich um von Microsoft erstellte, Azure-spezifische Richtlinien zu bewährten Methoden für Sicherheit und Compliance, die auf allgemeinen Complianceframeworks basieren. Weitere Informationen finden Sie in der [Einführung zum Vergleichstest für die Azure-Sicherheit](../security/benchmarks/introduction.md)

Wenn Sie Ihre Konformität mit einem anderen Standard nachverfolgen möchten, müssen Sie ihn explizit Ihrem Dashboard hinzufügen.
 
Sie können unter anderem folgende Standards hinzufügen: Azure CIS 1.1.0 (neu), NIST SP 800-53 R4, NIST SP 800-171 R2, SWIFT CSP CSCF-v2020, UK Official und UK NHS, HIPAA HITRUST, Canada Federal PBMM, ISO 27001, SOC2-TSP und PCI-DSS 3.2.1.  
 
Dem Dashboard werden weitere Standards hinzugefügt und in die Informationen unter [Anpassen der Standards in Ihrem Dashboard für die Einhaltung gesetzlicher Bestimmungen](update-regulatory-compliance-packages.md) eingeschlossen.

### <a name="why-do-some-controls-appear-grayed-out"></a>Warum sind einige Kontrollen abgeblendet?
Für jeden Konformitätsstandard auf dem Dashboard gibt es eine Liste mit den zugehörigen Kontrollen. Sie können für eine Kontrolle jeweils die Details zu den bestandenen und nicht bestandenen Bewertungen anzeigen.

Einige Kontrollen sind abgeblendet. Diesen Kontrollen sind keine Security Center-Bewertungen zugeordnet. Einige sind möglicherweise verfahrens- oder prozessbezogen und können daher nicht von Security Center überprüft werden. Für einige werden automatisierte Richtlinien oder Bewertungen erst noch implementiert. Und für einige Kontrollen ist ggf. die Plattform zuständig, wie unter [Gemeinsame Verantwortung in der Cloud](../security/fundamentals/shared-responsibility.md) erläutert. 

### <a name="how-can-i-remove-a-built-in-standard-like-pci-dss-iso-27001-or-soc2-tsp-from-the-dashboard"></a>Wie kann ich einen integrierten Standard (beispielsweise PCI-DSS, ISO 27001 oder SOC2 TSP) aus dem Dashboard entfernen? 
Wenn Sie das Dashboard für die Einhaltung gesetzlicher Bestimmungen anpassen und sich nur auf die für Sie relevanten Standards konzentrieren möchten, können Sie alle angezeigten gesetzlichen Standards entfernen, die für Ihre Organisation nicht relevant sind. Eine Anleitung zum Entfernen von Standards finden Sie unter [Entfernen eines Standards aus Ihrem Dashboard](update-regulatory-compliance-packages.md#remove-a-standard-from-your-dashboard).

### <a name="i-made-the-suggested-changed-based-on-the-recommendation-yet-it-isnt-being-reflected-in-the-dashboard"></a>Ich habe die in der Empfehlung vorgeschlagenen Änderungen vorgenommen, dies wird jedoch auf dem Dashboard nicht widergespiegelt.
Nach der Umsetzung von Empfehlungen kann es bis zu zwölf Stunden dauern, bis die Änderungen an Ihren Konformitätsdaten sichtbar werden. Bewertungen werden etwa alle 12 Stunden durchgeführt, und die Auswirkungen auf Ihre Konformitätsdaten sind jeweils erst nach Abschluss einer Bewertung sichtbar.
 
### <a name="what-permissions-do-i-need-to-access-the-compliance-dashboard"></a>Welche Berechtigungen benötige ich für den Zugriff auf das Compliancedashboard?
Zum Anzeigen von Konformitätsdaten benötigen Sie auch mindestens **Lesezugriff** auf die Konformitätsdaten der Richtlinie. „Sicherheitsleseberechtigter“ reicht nicht aus. Wenn Sie „Globaler Leser“ für das Abonnement sind, ist das ausreichend.

Für den Zugriff auf das Dashboard und die Verwaltung von Standards werden mindestens die Rollen **Mitwirkender bei Ressourcenrichtlinien** und **Sicherheitsadministrator** benötigt.


### <a name="the-regulatory-compliance-dashboard-isnt-loading-for-me"></a>Das Dashboard für die Einhaltung gesetzlicher Bestimmungen lädt bei mir nicht.
Um das Dashboard für die Einhaltung gesetzlicher Bestimmungen verwenden zu können, muss Azure Defender für Azure Security Center auf Abonnementebene aktiviert sein. Sollte das Dashboard nicht ordnungsgemäß geladen werden, versuchen Sie Folgendes:

1. Löschen Sie den Cache des Browsers.
1. Versuchen Sie es mit einem anderen Browser.
1. Öffnen Sie das Dashboard von einer anderen Netzwerkadresse aus.


### <a name="how-can-i-view-a-report-of-passing-and-failing-controls-per-standard-in-my-dashboard"></a>Wie kann ich einen nach Standard aufgeschlüsselten Bericht über bestandene und nicht bestandene Kontrollen auf meinem Dashboard anzeigen?
Auf dem Hauptdashboard können Sie einen Bericht über bestandene und nicht bestandene Kontrollen für (1) die Top 4 der niedrigsten Konformitätsstandards auf dem Dashboard erstellen. Wenn Sie den Status aller bestandenen/nicht bestandenen Kontrollen anzeigen möchten, wählen Sie (2) **Alle *x* anzeigen** aus, wobei „x“ die Anzahl der nachverfolgten Standards ist. Auf einer Kontextebene wird der Konformitätsstatus für jeden nachverfolgten Standard angezeigt.

:::image type="content" source="media/security-center-compliance-dashboard/summaries-of-compliance-standards.png" alt-text="Zusammenfassungsabschnitt des Dashboards für die Einhaltung gesetzlicher Bestimmungen":::


### <a name="how-can-i-download-a-report-with-compliance-data-in-a-format-other-than-pdf"></a>Wie kann ich einen Bericht mit Konformitätsdaten in einem PDF-fremden Format herunterladen?
Wenn Sie **Bericht herunterladen** auswählen, wählen Sie den Standard und das Format (PDF oder CSV) aus. Der resultierende Bericht umfasst die Abonnements, den Sie im Filter des Portals ausgewählt haben.

- Der PDF-Bericht enthält einen Zusammenfassungsstatus für den von Ihnen ausgewählten Standard.
- Der CSV-Bericht liefert detaillierte Ergebnisse pro Ressource, da er sich auf Richtlinien bezieht, die der jeweiligen Kontrolle zugeordnet sind.

Derzeit kann nur ein Bericht für die bereitgestellten gesetzlichen Standards heruntergeladen werden, nicht aber für eine benutzerdefinierte Richtlinie.


### <a name="how-can-i-create-exceptions-for-some-of-the-policies-in-the-regulatory-compliance-dashboard"></a>Wie kann ich Ausnahmen für einige der Richtlinien auf dem Dashboard für die Einhaltung gesetzlicher Bestimmungen erstellen?
Für Richtlinien, die in Security Center integriert und in der Sicherheitsbewertung enthalten sind, können Sie direkt im Portal Ausnahmen für einzelne oder mehrere Ressourcen erstellen, wie unter [Ausschließen von Ressource und Empfehlungen aus der Sicherheitsbewertung](exempt-resource.md) erläutert.

Für andere Richtlinien können Sie direkt in der Richtlinie eine Ausnahme erstellen. Eine entsprechende Anleitung finden Sie unter [Azure Policy-Ausnahmenstruktur](../governance/policy/concepts/exemption-structure.md).


### <a name="what-azure-defender-plans-or-licenses-do-i-need-to-use-the-regulatory-compliance-dashboard"></a>Welche Azure Defender-Pläne oder -Lizenzen benötige ich, um das Dashboard für die Einhaltung gesetzlicher Bestimmungen verwenden zu können?
Wenn für einen beliebigen Ihrer Azure-Ressourcentypen eines der Azure Defender-Pakete aktiviert ist, haben Sie in Security Center Zugriff auf das Dashboard für die Einhaltung gesetzlicher Bestimmungen mit sämtlichen zugehörigen Daten.





## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde beschrieben, wie Sie das Security Center-Dashboard zur Einhaltung gesetzlicher Bestimmungen für folgende Zwecke verwenden:

> [!div class="checklist"]
> * Anzeigen und Überwachen Ihres Konformitätsstatus in Bezug auf die für Sie wichtigen Standards und Bestimmungen
> * Verbessern Ihres Konformitätsstatus durch das Lösen relevanter Empfehlungen und Verfolgen der verbesserten Compliancebewertung

Mit dem Dashboard zur Einhaltung gesetzlicher Bestimmungen kann der Konformitätsprozess stark vereinfacht werden. Darüber hinaus kann eine erhebliche Verkürzung des Zeitraums erzielt werden, der für das Sammeln von Konformitätsnachweisen für Ihre Azure-, Hybrid- und Multicloudumgebung erforderlich ist.

Weitere Informationen finden Sie auf den folgenden verwandten Seiten:

- [Anpassen der Standards in Ihrem Dashboard für die Einhaltung gesetzlicher Bestimmungen](update-regulatory-compliance-packages.md): Es wird beschrieben, wie Sie auswählen, welche Standards in Ihrem Dashboard zur Einhaltung gesetzlicher Bestimmungen angezeigt werden. 
- [Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md): Hier erfahren Sie, wie Sie Empfehlungen in Security Center nutzen, um Ihre Azure-Ressourcen zu schützen.