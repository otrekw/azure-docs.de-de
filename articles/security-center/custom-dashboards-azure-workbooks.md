---
title: Arbeitsmappenkatalog in Azure Security Center
description: Erfahren Sie, wie Sie mit dem integrierten Azure Monitor-Arbeitsmappenkatalog umfassende interaktive Berichte zu ihren Azure Security Center-Daten erstellen.
author: memildin
ms.author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 03/04/2021
ms.openlocfilehash: 198702f619e490e8000e4430aab23a7f6bfb6d85
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102107126"
---
# <a name="create-rich-interactive-reports-of-security-center-data"></a>Erstellen umfassender, interaktiver Berichte zu Security Center-Daten

[Azure Monitor-Arbeitsmappen](../azure-monitor/visualize/workbooks-overview.md) bieten einen flexiblen Bereich für die Datenanalyse und die Erstellung umfassender visueller Berichte innerhalb des Microsoft Azure-Portals. Mit ihnen können Sie mehrere Datenquellen in Azure nutzen und sie zu vereinheitlichten interaktiven Oberflächen kombinieren.

Arbeitsmappen bieten eine Vielzahl von Funktionen zur Visualisierung Ihrer Azure-Daten. Detaillierte Beispiele für die einzelnen Visualisierungstypen finden Sie in den [Visualisierungsbeispielen und der Dokumentation](../azure-monitor/visualize/workbooks-text-visualizations.md). 

Im Azure Security Center können Sie die Sicherheitslage Ihres Unternehmens anhand der integrierten Berichte verfolgen. Sie können auch benutzerdefinierte Berichte erstellen, um eine breite Palette von Daten aus dem Security Center oder anderen unterstützten Datenquellen anzuzeigen.

:::image type="content" source="media/custom-dashboards-azure-workbooks/secure-score-over-time-snip.png" alt-text="Bericht „Sicherheitsbewertung im Zeitverlauf“":::

## <a name="availability"></a>Verfügbarkeit

| Aspekt                          | Details                                                                                                                                      |
|---------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------|
| Status des Release:                  | Vorschau<br>[!INCLUDE [Legalese](../../includes/security-center-preview-legal-text.md)]                                                       |
| Preise:                        | Kostenlos                                                                                                                                         |
| Erforderliche Rollen und Berechtigungen: | Um Arbeitsmappen zu speichern, müssen Sie mindestens die Berechtigung „Arbeitsmappenmitwirkender“ für die Zielressourcengruppe haben                                      |
| Clouds:                         | ![Ja](./media/icons/yes-icon.png) Kommerzielle Clouds<br>![Ja](./media/icons/yes-icon.png) National/Sovereign (US Gov, China Gov, andere Gov) |
|                                 |                                                                                                                                              |

## <a name="workbooks-gallery-in-azure-security-center"></a>Arbeitsmappenkatalog in Azure Security Center

Mit der integrierten Azure-Arbeitsmappenfunktionalität erleichtert das Azure Security Center die Erstellung eigener interaktiver Berichte. Security Center enthält auch einen Arbeitsmappenkatalog mit den folgenden Berichten, die Sie anpassen können:

- **Sicherheitsbewertung im Zeitverlauf**: Nutzen Sie die Nachverfolgung der Bewertungen Ihrer Abonnements und der Änderungen von Empfehlungen für Ihre Ressourcen.
- **Systemupdates**: Zeigen Sie fehlende Systemupdates nach Ressource, Betriebssystem, Schweregrad usw. an.
- **Ergebnisse der Sicherheitsrisikobewertung**: Anzeigen der Ergebnisse von Sicherheitsrisikoüberprüfungen ihrer Azure-Ressourcen

:::image type="content" source="media/custom-dashboards-azure-workbooks/workbooks-gallery-security-center.png" alt-text="Katalog der integrierten Arbeitsmappen im Azure Security Center":::

Wählen Sie einen der bereitgestellten Berichte aus, oder erstellen Sie einen eigenen.

> [!TIP]
> Verwenden Sie die Schaltfläche **Bearbeiten**, um die bereitgestellten Berichte entsprechend Ihren Anforderungen anzupassen. Wenn Sie mit der Bearbeitung fertig sind, wählen Sie **Speichern** aus, und Ihre Änderungen werden in einer neuen Arbeitsmappe gespeichert.
> 
> :::image type="content" source="media/custom-dashboards-azure-workbooks/editing-supplied-workbooks.png" alt-text="Bearbeiten der bereitgestellten Arbeitsmappen, um Sie an Ihre speziellen Anforderungen anzupassen":::

### <a name="use-the-secure-score-over-time-report"></a>Verwenden des Berichts „Sicherheitsbewertung im Zeitverlauf“

In diesem Bericht werden Sicherheitsbewertungsdaten aus Ihrem Log Analytics-Arbeitsbereich verwendet. Diese Daten müssen aus dem Tool für den fortlaufenden Export exportiert werden, wie unter [Konfigurieren des fortlaufenden Exports über die Security Center-Seiten im Azure-Portal](continuous-export.md?tabs=azure-portal) beschrieben.

Wenn Sie den fortlaufenden Export einrichten, legen Sie die Exporthäufigkeit sowohl auf **Streamingupdates** als auch auf **Momentaufnahmen** fest.

:::image type="content" source="media/custom-dashboards-azure-workbooks/export-frequency-both.png" alt-text="Für die Arbeitsmappe „Sicherheitsbewertung im Zeitverlauf“ müssen Sie diese beiden Optionen aus den Einstellungen für die Exporthäufigkeit in der Konfiguration für den fortlaufenden Export auswählen.":::

> [!NOTE]
> Momentaufnahmen werden wöchentlich exportiert, sodass Sie mindestens eine Woche warten müssen, bis die erste Momentaufnahme exportiert wird, bevor Sie die Daten in diesem Bericht anzeigen können.

> [!TIP]
> Verwenden Sie zum Konfigurieren des fortlaufenden Exports in Ihrer gesamten Organisation die bereitgestellten Azure Policy „DeployIfNotExist“-Richtlinien, die unter [Konfigurieren des fortlaufenden Exports in großem Maßstab](continuous-export.md?tabs=azure-policy) beschrieben werden.

Der Bericht „Sicherheitsbewertung im Zeitverlauf“ enthält fünf Diagramme für die Abonnements, die an die ausgewählten Arbeitsbereiche Bericht erstatten:


|Graph  |Beispiel  |
|---------|---------|
|**Bewertungstrends für die letzte Woche und den Monat**<br>Verwenden Sie diesen Abschnitt, um die aktuelle Bewertung und allgemeine Bewertungstrends für Ihre Abonnements zu überwachen.|:::image type="content" source="media/custom-dashboards-azure-workbooks/secure-score-over-time-table-1.png" alt-text="Sicherheitsbewertungstrends im integrierten Bericht":::|
|**Aggregierte Bewertung für alle ausgewählten Abonnements**<br>Bewegen Sie die Maus über einen beliebigen Punkt in der Trendlinie, um die aggregierte Bewertung an einem bestimmten Datum im ausgewählten Zeitraum anzuzeigen.|:::image type="content" source="media/custom-dashboards-azure-workbooks/secure-score-over-time-table-2.png" alt-text="Aggregierte Bewertung für alle ausgewählten Abonnements":::|
|**Empfehlungen mit den fehlerhaftesten Ressourcen**<br>Diese Tabelle hilft Ihnen bei der Auswahl der Empfehlungen, bei denen die meisten Ressourcen über den ausgewählten Zeitraum in „fehlerhaft“ geändert wurden.|:::image type="content" source="media/custom-dashboards-azure-workbooks/secure-score-over-time-table-3.png" alt-text="Empfehlungen mit den fehlerhaftesten Ressourcen":::|
|**Bewertungen für bestimmte Sicherheitskontrollen**<br>Die Security Center-Sicherheitskontrollen sind logische Gruppierungen von Empfehlungen. Dieses Diagramm zeigt Ihnen auf einen Blick die wöchentlichen Bewertungen für alle Ihre Sicherheitskontrollen.|:::image type="content" source="media/custom-dashboards-azure-workbooks/secure-score-over-time-table-4.png" alt-text="Bewertungen für Ihre Sicherheitskontrollen im ausgewählten Zeitraum":::|
|**Ressourcenänderungen**<br>Hier werden die Empfehlungen mit den meisten Ressourcen aufgeführt, deren Status (fehlerfrei, fehlerhaft oder nicht zutreffend) sich im ausgewählten Zeitraum geändert hat. Wählen Sie eine beliebige Empfehlung aus der Liste aus, um eine neue Tabelle mit den spezifischen Ressourcen zu öffnen.|:::image type="content" source="media/custom-dashboards-azure-workbooks/secure-score-over-time-table-5.png" alt-text="Empfehlungen mit den meisten Ressourcen, deren Integritätsstatus sich geändert hat":::|

### <a name="use-the-system-updates-report"></a>Verwenden des Berichts „Systemupdates“

Dieser Bericht basiert auf der Sicherheitsempfehlung „Auf Ihren Computern sollten Systemupdates installiert werden“.

Der Bericht unterstützt Sie bei der Identifizierung von Computern mit ausstehenden Updates.

Sie können die Situation für die ausgewählten Abonnements entsprechend den folgenden Informationen anzeigen:

- Liste der Ressourcen mit ausstehenden Updates
- Liste der Updates, die auf ihren Ressourcen fehlen

:::image type="content" source="media/custom-dashboards-azure-workbooks/system-updates-report.png" alt-text="Security Center-Bericht „Systemupdates“, basierend auf der Sicherheitsempfehlung zu fehlenden Updates":::

### <a name="use-the-vulnerability-assessment-findings-report"></a>Verwenden des Berichts „Ergebnisse der Sicherheitsrisikobewertung“

Security Center umfasst Sicherheitsrisikoüberprüfungen für Ihre Computer, Container in Containerregistrierungen und SQL Server-Instanzen.

Weitere Informationen zur Verwendung dieser Überprüfungen:

- [Überprüfen Ihrer Computer mit dem integrierten Sicherheitsrisikobewertungsscanner](deploy-vulnerability-assessment-vm.md)
- [Überprüfen Ihrer Registrierungsimages auf Sicherheitsrisiken](defender-for-container-registries-usage.md)
- [Überprüfen der SQL-Ressourcen auf Sicherheitsrisiken](defender-for-sql-on-machines-vulnerability-assessment.md)

Die Ergebnisse der einzelnen Überprüfungen werden in separaten Empfehlungen angezeigt:

- Sicherheitsrisiken für VMs müssen behoben werden
- Sicherheitsrisiken in Azure Container Registry-Images müssen behoben werden (unterstützt von Qualys).
- Ergebnisse der Sicherheitsrisikobewertung in Ihren SQL Datenbanken müssen beseitigt werden
- Ergebnisse der Sicherheitsrisikobewertung auf Ihren SQL-Servern auf Computern müssen beseitigt werden

Dieser Bericht sammelt diese Ergebnisse und organisiert sie nach Schweregrad, Ressourcentyp und Kategorie.

:::image type="content" source="media/custom-dashboards-azure-workbooks/vulnerability-assessment-findings-report.png" alt-text="Bericht mit den Ergebnissen der Sicherheitsrisikobewertung im Security Center":::


## <a name="import-workbooks-from-other-workbook-galleries"></a>Importieren von Arbeitsmappen aus anderen Arbeitsmappenkatalogen

Wenn Sie Arbeitsmappen in anderen Azure-Diensten erstellt haben und sie in Ihren Azure Security Center-Arbeitsmappenkatalog verschieben möchten:

1. Öffnen Sie die Zielarbeitsmappe.

1. Wählen Sie auf der Symbolleiste die Option **Bearbeiten** aus.

    :::image type="content" source="media/custom-dashboards-azure-workbooks/editing-workbooks.png" alt-text="Bearbeiten einer Azure Monitor-Arbeitsmappe":::

1. Wählen Sie auf der Symbolleiste die Option **</>** aus, um den erweiterten Editor zu öffnen.

    :::image type="content" source="media/custom-dashboards-azure-workbooks/editing-workbooks-advanced-editor.png" alt-text="Starten des erweiterten Editors zum Abrufen des JSON-Codes der Katalogvorlage":::

1. Kopieren Sie den JSON-Code der Katalogvorlage der Arbeitsmappe.

1. Öffnen Sie den Arbeitsmappenkatalog im Security Center, und wählen Sie in der Menüleiste **Neu** aus.
1. Wählen Sie **</>** aus, um den erweiterten Editor zu öffnen.
1. Fügen Sie gesamten JSON-Code der Katalogvorlage ein.
1. Wählen Sie **Übernehmen**.
1. Wählen Sie auf der Symbolleiste die Option **Speichern unter** aus.

    :::image type="content" source="media/custom-dashboards-azure-workbooks/editing-workbooks-save-as.png" alt-text="Speichern der Arbeitsmappe im Katalog im Security Center":::

1. Geben Sie die erforderlichen Details zum Speichern der Arbeitsmappe ein:
   1. Einen Namen für die Arbeitsmappe
   2. Die gewünschte Region
   3. Abonnement, Ressourcengruppe und Freigabe (nach Bedarf).

Sie finden Ihre gespeicherte Arbeitsmappe in der Kategorie **Kürzlich geänderte Arbeitsmappen**.


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde die Security Center-Seite der integrierten Azure Monitor Arbeitsmappen mit den integrierten Berichten und der Option zum Erstellen eigener benutzerdefinierter, interaktiver Berichte beschrieben.

- Weitere Informationen zu [Azure Monitor-Arbeitsmappen](../azure-monitor/visualize/workbooks-overview.md)
- Die integrierten Berichte rufen ihre Daten per Pull aus den Security Center-Empfehlungen ab. Weitere Informationen zu den zahlreichen Sicherheitsempfehlungen finden Sie unter [Sicherheitsempfehlungen – ein Referenzhandbuch](recommendations-reference.md).