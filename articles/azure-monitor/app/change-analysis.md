---
title: Verwenden der Anwendungsänderungsanalyse in Azure Monitor für die Suche nach Web-App-Problemen | Microsoft-Dokumentation
description: Verwenden der Anwendungsänderungsanalyse in Azure Monitor zum Behandeln von Anwendungsproblemen auf Live-Websites in Azure App Service.
ms.topic: conceptual
author: cawams
ms.author: cawa
ms.date: 05/04/2020
ms.openlocfilehash: c287a2315f2b2319a6873ce84ee0e4e48bec8444
ms.sourcegitcommit: 11572a869ef8dbec8e7c721bc7744e2859b79962
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "82836765"
---
# <a name="use-application-change-analysis-preview-in-azure-monitor"></a>Verwenden der Anwendungsänderungsanalyse (Vorschau) in Azure Monitor

Wenn es zu einem Problem oder Ausfall einer Live-Website kommt, ist es wichtig, die Grundursache schnell bestimmen zu können. Die Standardüberwachungslösungen machen Sie möglicherweise auf ein Problem aufmerksam. Sie geben unter Umständen sogar an, bei welcher Komponente der Fehler aufgetreten ist. In den meisten Fällen geht aus dieser Warnung jedoch die Fehlerursache nicht direkt hervor. Sie wissen, dass Ihre Website vor fünf Minuten noch funktioniert hat, und jetzt nicht mehr. Was hat sich in den letzten fünf Minuten geändert? Für die Beantwortung dieser Frage ist die Anwendungsänderungsanalyse in Azure Monitor konzipiert.

Basierend auf dem Funktionsumfang von [Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview) liefert die Änderungsanalyse Erkenntnisse über Azure-Anwendungsänderungen, um die Transparenz zu erhöhen und die durchschnittliche Reparaturzeit (Mean Time To Repair, MTTR) zu verkürzen.

> [!IMPORTANT]
> Die Änderungsanalyse befindet sich derzeit in der Vorschauphase. Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt. Diese Version wird nicht für die Produktion empfohlen. Manche Funktionen werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="overview"></a>Übersicht

Die Änderungsanalyse erkennt von der Infrastrukturebene bis hin zur Anwendungsbereitstellung verschiedene Arten von Änderungen. Es handelt sich um einen Azure-Ressourcenanbieter auf Abonnementebene, der Ressourcenänderungen im Abonnement überprüft. Die Änderungsanalyse stellt Daten für verschiedene Diagnosetools bereit, damit der Benutzer besser verstehen kann, welche Änderungen möglicherweise zu Problemen geführt haben.

Das folgende Diagramm zeigt die Architektur der Änderungsanalyse:

![Architekturdiagramm zum Abrufen von Änderungsdaten und Bereitstellung dieser Daten für Clienttools durch die Änderungsanalyse](./media/change-analysis/overview.png)

## <a name="data-sources"></a>Datenquellen

Anwendungsänderungsanalyse-Abfragen für von Azure Resource Manager nachverfolgte Eigenschaften, per Proxy übermittelte Konfigurationen und Web-App-Änderungen des Gastsystems. Außerdem verfolgt der Dienst Änderungen an Ressourcenabhängigkeiten, um Anwendungen End-to-End zu diagnostizieren und zu überwachen.

### <a name="azure-resource-manager-tracked-properties-changes"></a>Verfolgte Eigenschaftsänderungen in Azure Resource Manager

Mithilfe von [Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview) stellt die Änderungsanalyse in Form von Verlaufsdaten dar, wie sich die Azure-Ressourcen, die Ihre Anwendung hosten, im Laufe der Zeit verändert haben. Nachverfolgte Einstellungen wie verwaltete Identitäten, Plattformbetriebssystems-Upgrades und Hostnamen können erkannt werden.

### <a name="azure-resource-manager-proxied-setting-changes"></a>Per Proxy übermittelte Einstellungsänderungen in Azure Resource Manager

Einstellungen wie die IP-Konfigurationsregel, TLS-Einstellungen und Erweiterungsversionen sind in Azure Resource Graph noch nicht verfügbar. Daher fragt die Änderungsanalyse diese Änderungen ab und berechnet sie sicher, um weitere Details darüber bereitzustellen, was sich in der App geändert hat.

### <a name="changes-in-web-app-deployment-and-configuration-in-guest-changes"></a>Änderungen bei Bereitstellung und Konfiguration einer Web-App (Änderungen auf Gastsystemen)

Die Änderungsanalyse erfasst alle vier Stunden den Bereitstellungs- und Konfigurationsstatus der Anwendung. Sie kann z. B. Änderungen in den Umgebungsvariablen der Anwendung erkennen. Das Tool berechnet die Unterschiede und zeigt die Änderungen an. Im Gegensatz zu Resource Manager-Änderungen sind die Informationen zu Änderungen an der Codebereitstellung möglicherweise nicht sofort im Tool verfügbar. Wählen Sie zum Anzeigen der jüngsten Änderungen in der Änderungsanalyse **Aktualisieren** aus.

![Screenshot der Schaltfläche „Jetzt auf Änderungen prüfen“](./media/change-analysis/scan-changes.png)

### <a name="dependency-changes"></a>Abhängigkeitsänderungen

Änderungen an Ressourcenabhängigkeiten können ebenfalls zu Problemen in einer Web-App führen. Wenn eine Web-App beispielsweise Aufrufe an einen Redis Cache richtet, kann die Web-App-Leistung durch die Redis Cache-SKU beeinträchtigt werden. Zum Erkennen von Änderungen bei Abhängigkeiten überprüft die Änderungsanalyse dem DNS-Eintrag der Web-App. Auf diese Weise identifiziert sie Änderungen in allen App-Komponenten, die Probleme verursachen könnten.
Derzeit werden die folgenden Abhängigkeiten unterstützt:
- Web-Apps
- Azure Storage
- Azure SQL

## <a name="application-change-analysis-service"></a>Anwendungsänderungsanalyse-Dienst

Der Anwendungsänderungsanalyse-Dienst berechnet und aggregiert Änderungsdaten aus den oben erwähnten Datenquellen. Er bietet eine Reihe von Analysen für Benutzer, um bequem durch alle Ressourcenänderungen zu navigieren und zu ermitteln, welche Änderung im Kontext der Problembehandlung oder Überwachung relevant ist.
Der Ressourcenanbieter „Microsoft.ChangeAnalysis“ muss bei einem Abonnement registriert werden, damit die Daten zu nachverfolgten Änderungen und per Proxy übermittelten Einstellungsänderungen von Azure Resource Manager verfügbar sind. Wenn Sie das Diagnose- und Problembehandlungstool in der Web-App öffnen oder die eigenständige Registerkarte „Änderungsanalyse“ aufrufen, wird dieser Ressourcenanbieter automatisch registriert. Es gibt keine Leistungs- oder Kostenimplementierungen für Ihr Abonnement. Wenn Sie die Änderungsanalyse für Web-Apps aktivieren (oder die Aktivierung im Diagnose- und Problembehandlungstool vornehmen) hat dies nur geringfügige Auswirkungen auf die Leistung der Web-App und bewirkt keinerlei Abrechnungskosten.
Bei Web-App-Änderungen, die das Gastbetriebssystem betreffen, ist eine separate Aktivierung erforderlich, um Codedateien in einer Web-App zu überprüfen. Weitere Informationen finden Sie unter [Änderungsanalyse im Tool „Diagnose und Problembehandlung“](https://docs.microsoft.com/azure/azure-monitor/app/change-analysis#application-change-analysis-in-the-diagnose-and-solve-problems-tool) weiter unten in diesem Artikel.

## <a name="visualizations-for-application-change-analysis"></a>Visualisierungen für die Anwendungsänderungsanalyse

### <a name="standalone-ui"></a>Eigenständige Benutzeroberfläche

In Azure Monitor gibt es einen eigenständigen Bereich für die Änderungsanalyse, in dem alle Änderungen mit Erkenntnissen zu Anwendungsabhängigkeiten und Ressourcen angezeigt werden können.

Suchen Sie im Azure-Portal in der Suchleiste nach „Änderungsanalyse“, um die Erfahrung zu starten.

![Screenshot der Suche nach „Änderungsanalyse“ im Azure-Portal](./media/change-analysis/search-change-analysis.png)

Alle Ressourcen unter einem ausgewählten Abonnement werden mit Änderungen der letzten 24 Stunden angezeigt. Zur Optimierung der Seitenladeleistung zeigt der Dienst jeweils 10 Ressourcen gleichzeitig an. Klicken Sie auf die nächsten Seiten, um weitere Ressourcen anzuzeigen. Wir arbeiten derzeit daran, diese Einschränkung zu beseitigen.

![Screenshot des Blatts „Änderungsanalyse“ im Azure-Portal](./media/change-analysis/change-analysis-standalone-blade.png)

Klicken Sie auf eine Ressource, um alle ihre Änderungen anzuzeigen. Führen Sie bei Bedarf einen Drilldown in eine Änderung durch, um Änderungsdetails und Erkenntnisse im JSON-Format anzuzeigen.

![Screenshot von Änderungsdetails](./media/change-analysis/change-details.png)

Zum Senden von Feedback verwenden Sie die entsprechende Schaltfläche auf dem Blatt, oder senden Sie eine E-Mail an changeanalysisteam@microsoft.com.

![Screenshot der Feedbackschaltfläche auf dem Blatt „Änderungsanalyse“](./media/change-analysis/change-analysis-feedback.png)

### <a name="web-app-diagnose-and-solve-problems"></a>Diagnose und Behandlung von Problemen bei Web-Apps

In Azure Monitor ist die Änderungsanalyse auch in die Self-Service-Umgebung **Diagnose und Problembehandlung** integriert. Der Zugriff auf diese Umgebung erfolgt von der Seite **Übersicht** Ihrer App Service-Anwendung.

![Screenshot der Schaltflächen „Übersicht“ und „Diagnose und Problembehandlung“](./media/change-analysis/change-analysis.png)

### <a name="application-change-analysis-in-the-diagnose-and-solve-problems-tool"></a>Anwendungsänderungsanalyse im Tool „Diagnose und Problembehandlung“

Bei der Anwendungsänderungsanalyse handelt es sich um ein eigenständiges Erkennungsmodul in den Web-App-Tools für Diagnose und Problembehandlung. Es ist auch in den Erkennungsmodulen für **Anwendungsabstürze** und  **Web-App-Ausfälle** aggregiert. Wenn Sie das Diagnose- und Problembehandlungstool öffnen, wird der Ressourcenanbieter **Microsoft.ChangeAnalysis** automatisch registriert. Befolgen Sie diese Anweisungen, um die Nachverfolgung von Web-App-Änderungen des Gastsystems zu aktivieren.

1. Wählen Sie **Verfügbarkeit und Leistung** aus.

    ![Screenshot der Problembehandlungsoptionen von „Verfügbarkeit und Leistung“](./media/change-analysis/availability-and-performance.png)

2. Wählen Sie **Anwendungsänderungen** aus. Diese Funktion ist ebenfalls unter **Anwendungsabstürze** verfügbar.

   ![Screenshot der Schaltfläche „Anwendungsabstürze“](./media/change-analysis/application-changes.png)

3. Wählen Sie zum Aktivieren der Änderungsanalyse die Option **Jetzt aktivieren** aus.

   ![Screenshot der Optionen von „Anwendungsabstürze“](./media/change-analysis/enable-changeanalysis.png)

4. Aktivieren Sie **Änderungsanalyse**, und wählen Sie **Speichern** aus. Das Tool zeigt alle Web-Apps unter einem App Service-Plan an. Sie können den Schalter auf Planebene verwenden, um die Änderungsanalyse für alle Web-Apps in einem Plan zu aktivieren.

    ![Screenshot der Benutzeroberfläche zum Aktivieren der Änderungsanalyse](./media/change-analysis/change-analysis-on.png)

5. Wählen Sie **Diagnose und Problembehandlung**> **Verfügbarkeit und Leistung**> **Anwendungsabstürze** aus, um auf die Änderungsanalyse zuzugreifen. Im angezeigten Diagramm sind die Arten der Änderungen im Laufe der Zeit und Details zu diesen Änderungen zusammengefasst. Standardmäßig werden Änderungen der letzten 24 Stunden angezeigt, um bei unmittelbaren Problemen sofort reagieren zu können.

     ![Screenshot der Gegenüberstellung der Änderungen](./media/change-analysis/change-view.png)

### <a name="enable-change-analysis-at-scale"></a>Aktivieren der Änderungsanalyse in größerem Umfang

Wenn Ihr Abonnement zahlreiche Web-Apps enthält, wäre das Aktivieren des Diensts auf Web-App-Ebene nicht sehr effizient. Führen Sie das folgende Skript aus, um alle Web-Apps in Ihrem Abonnement zu aktivieren.

Voraussetzungen:

- PowerShell Az-Modul. Folgen Sie den Anweisungen unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-2.6.0).

Führen Sie folgendes Skript aus:

```PowerShell
# Log in to your Azure subscription
Connect-AzAccount

# Get subscription Id
$SubscriptionId = Read-Host -Prompt 'Input your subscription Id'

# Make Feature Flag visible to the subscription
Set-AzContext -SubscriptionId $SubscriptionId

# Register resource provider
Register-AzResourceProvider -ProviderNamespace "Microsoft.ChangeAnalysis"

# Enable each web app
$webapp_list = Get-AzWebApp | Where-Object {$_.kind -eq 'app'}
foreach ($webapp in $webapp_list)
{
    $tags = $webapp.Tags
    $tags[“hidden-related:diagnostics/changeAnalysisScanEnabled”]=$true
    Set-AzResource -ResourceId $webapp.Id -Tag $tags -Force
}

```

### <a name="virtual-machine-diagnose-and-solve-problems"></a>Diagnose und Problembehandlung bei virtuellen Computern

Wechseln Sie zum Diagnose- und Problembehandlungstool für einen virtuellen Computer.  Wechseln Sie zu **Problembehandlungstools**, scrollen Sie auf der Seite nach unten, und wählen Sie **Aktuelle Änderungen analysieren** aus, um Änderungen an dem virtuellen Computer anzuzeigen.

![Screenshot von „Diagnose und Problembehandlung bei virtuellen Computern“](./media/change-analysis/vm-dnsp-troubleshootingtools.png)

![Screenshot von „Diagnose und Problembehandlung bei virtuellen Computern“](./media/change-analysis/analyze-recent-changes.png)

## <a name="next-steps"></a>Nächste Schritte

- Aktivieren von Application Insights für [Azure App Services-Apps](azure-web-apps.md)
- Aktivieren von Application Insights für [in IIS gehostete virtuelle Azure-Computer und Azure-VM-Skalierungsgruppen](azure-vm-vmss-apps.md)
- Erfahren Sie mehr über [Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview), der die Änderungsanalyse unterstützt.
