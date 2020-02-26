---
title: Häufig gestellte Fragen zu Azure Monitor für VMs (allgemein verfügbare Version, GA) | Microsoft-Dokumentation
description: Azure Monitor for VMs ist eine Lösung in Azure, die Integritäts- und Leistungsüberwachung des Azure VM-Betriebssystems mit der automatischen Erkennung von Anwendungskomponenten und Abhängigkeiten mit anderen Ressourcen kombiniert und die Kommunikation unter ihnen als Zuordnung darstellt. In diesem Artikel werden häufig gestellte Fragen zum allgemein verfügbaren Release beantwortet.
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/31/2020
ms.openlocfilehash: 673cda4005d9c985d8d6ee5ef1d28a3d8c241ac0
ms.sourcegitcommit: 98a5a6765da081e7f294d3cb19c1357d10ca333f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2020
ms.locfileid: "77482883"
---
# <a name="azure-monitor-for-vms-generally-available-ga-frequently-asked-questions"></a>Häufig gestellte Fragen zu Azure Monitor für VMs in der allgemein verfügbaren Version (GA)

Diese häufig gestellten Fragen zur allgemeinen Verfügbarkeit betreffen Änderungen, die im Zug unserer Vorbereitung auf die Version für allgemeine Verfügbarkeit in Azure Monitor für VMs auftreten. 

## <a name="updates-for-azure-monitor-for-vms"></a>Updates für Azure Monitor für VMs

Wir haben eine neue Version von Azure Monitor für VMs veröffentlicht. Kunden, die Azure Monitor für VMs aktivieren, erhalten nun die neue Version. Bestandskunden, die Azure Monitor für VMs bereits verwenden, werden zum Upgrade aufgefordert. Diese häufig gestellten Fragen und unsere Dokumentation bieten Anleitungen zum Durchführen eines Upgrades in der erforderlichen Größenordnung, wenn Sie über große Bereitstellungen mit mehreren Arbeitsbereichen verfügen.

Mit diesem Upgrade werden Leistungsdaten von Azure Monitor für VMs in derselben Tabelle *InsightsMetrics* wie bei [Azure Monitor für Container](container-insights-overview.md) gespeichert. So können Sie diese beiden Datasets einfacher abfragen. Außerdem können Sie vielfältigere Datasets speichern, die in der zuvor verwendeten Tabelle nicht gespeichert werden konnten. 

In den Leistungsansichten werden nun die in der Tabelle *InsightsMetrics* gespeicherten Daten verwendet.  Wenn Sie noch kein Upgrade zur Nutzung der aktuellen VMInsights-Lösung in Ihrem Arbeitsbereich durchgeführt haben, werden in Ihren Diagrammen keine Informationen mehr angezeigt.  Sie können wie weiter unten beschrieben ein Upgrade über die Seite **Erste Schritte** ausführen.

Mit diesem Upgrade werden Leistungsdaten von Azure Monitor für VMs in derselben Tabelle *InsightsMetrics* wie bei [Azure Monitor für Container](container-insights-overview.md) gespeichert. So können Sie diese beiden Datasets einfacher abfragen. Außerdem können Sie vielfältigere Datasets speichern, die in der zuvor verwendeten Tabelle nicht gespeichert werden konnten. 

Uns ist bewusst, dass die Aufforderung zum Upgrade für bestehende Kunden eine Unterbrechung ihres Workflows bedeutet. Deshalb haben wir uns entschieden, dies zum jetzigen Zeitpunkt in der Public Preview und nicht erst später nach der allgemeinen Verfügbarkeit durchzuführen.


## <a name="what-is-changing"></a>Was ändert sich?

Wir haben eine neue Lösung mit dem Namen VMInsights veröffentlicht, die zusätzliche Funktionen für die Datensammlung sowie einen neuen Speicherort für diese Daten in Ihrem Log Analytics-Arbeitsbereich umfasst. 

Bisher hatten wir die ServiceMap-Lösung in Ihrem Arbeitsbereich aktiviert und Leistungsindikatoren in Ihrem Log Analytics-Arbeitsbereich eingerichtet, um die Daten an die Tabelle *Perf* zu senden. Bei dieser neuen Lösung werden die Daten an die Tabelle *InsightsMetrics* gesendet, die auch von Azure Monitor für Container verwendet wird. Aufgrund dieses Tabellenschemas können wir zusätzliche Metriken und Dienstdatasets speichern, die nicht mit dem Format der Tabelle *Perf* kompatibel sind.

Die Leistungsdiagramme wurden so aktualisiert, dass sie die in der Tabelle *InsightsMetrics* gespeicherten Daten verwenden. Sie können wie nachfolgend beschrieben über die Seite **Erste Schritte** ein Update zur Verwendung der Tabelle *InsightsMetrics* ausführen.


## <a name="how-do-i-upgrade"></a>Wie führe ich ein Upgrade aus?
Wenn ein Log Analytics-Arbeitsbereich auf die neueste Version von Azure Monitor für VMs aktualisiert wird, wird der Dependency-Agent auf allen VMs aktualisiert, die mit diesem Arbeitsbereich verbunden sind. Jede VM, die ein Upgrade erfordert, wird auf der Registerkarte **Erste Schritte** in Azure Monitor für VMs im Azure-Portal angegeben. Wenn Sie eine VM aktualisieren, werden der Arbeitsbereich für diese VM sowie alle anderen VMs aktualisiert, die mit diesem Arbeitsbereich verbunden sind. Sie können eine einzelne VM oder mehrere VMs, Ressourcengruppen oder Abonnements auswählen. 

Verwenden Sie den folgenden Befehl, um einen Arbeitsbereich mit PowerShell zu aktualisieren:

```PowerShell
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName <resource-group-name> -WorkspaceName <workspace-name> -IntelligencePackName "VMInsights" -Enabled $True
```

## <a name="what-should-i-do-about-the-performance-counters-in-my-workspace-if-i-install-the-vminsights-solution"></a>Was soll ich mit den Leistungsindikatoren in meinem Arbeitsbereich machen, wenn ich die VMInsights-Lösung installiere?

Bei der aktuellen Methode zum Aktivieren von Azure Monitor für VMs werden Leistungsindikatoren in Ihrem Arbeitsbereich verwendet. Bei der neuen Methode werden diese Daten in einer neuen Tabelle namens `InsightsMetrics` gespeichert.

Sobald wir unsere Benutzeroberfläche für die Verwendung der Daten in der Tabelle `InsightsMetrics` aktualisiert haben, aktualisieren wir auch unsere Dokumentation und verbreiten diese Ankündigung über mehrere Kanäle, einschließlich eines Banners im Azure-Portal. Zu diesem Zeitpunkt können Sie die [Leistungsindikatoren](vminsights-enable-overview.md#performance-counters-enabled) in Ihrem Arbeitsbereich deaktivieren, wenn Sie sie nicht mehr benötigen. 

>[!NOTE]
>Wenn Sie über Warnungsregeln verfügen, die auf diese Leistungsindikatoren in der Tabelle `Perf` verweisen, müssen Sie sie aktualisieren, damit sie auf die neuen Daten in der Tabelle `InsightsMetrics` verweisen. In unserer Dokumentation finden Sie Beispiele für Protokollabfragen, die Sie verwenden können und die auf diese Tabelle verweisen.
>

Wenn Sie die Leistungsindikatoren aktiviert lassen, werden Ihnen die erfassten und in der Tabelle `Perf` gespeicherten Daten basierend auf den [Log Analytics-Preisen](https://azure.microsoft.com/pricing/details/monitor/) in Rechnung gestellt.

## <a name="how-will-this-change-affect-my-alert-rules"></a>Wie wirkt sich diese Änderung auf meine Warnungsregeln aus?

Wenn Sie [Protokollwarnungen](../platform/alerts-unified-log.md) erstellt haben, die die Tabelle `Perf` für die im Arbeitsbereich aktivierten Leistungsindikatoren abfragen, sollten Sie diese Regeln aktualisieren, damit sie stattdessen auf die Tabelle `InsightsMetrics` verweisen. Diese Anweisung betrifft auch alle Regeln für die Protokollsuche, die `ServiceMapComputer_CL` und `ServiceMapProcess_CL` verwenden, da diese Datasets in die Tabellen `VMComputer` und `VMProcess` verschoben werden.

Diese häufig gestellten Fragen und unsere Dokumentation werden noch aktualisiert, damit Beispiele für Warnungsregeln für die Protokollsuche für die von uns gesammelten Datasets enthalten sind.

## <a name="how-will-this-affect-my-bill"></a>Welche Folgen hat das für meine Rechnung?

Die Abrechnung basiert weiterhin auf Daten, die erfasst und in Ihrem Log Analytics-Arbeitsbereich gespeichert werden.

Die von uns gesammelten Leistungsdaten auf Computerebene sind identisch, haben eine ähnliche Größe wie die in der Tabelle `Perf` gespeicherten Daten und weisen ungefähr dieselben Kosten auf.

## <a name="what-if-i-only-want-to-use-service-map"></a>Kann ich auch nur die Dienstzuordnung verwenden?

Das geht. Beim Anzeigen von Azure Monitor für VMs werden im Azure-Portal entsprechende Aufforderungen zum bevorstehenden Update angezeigt. Nach der Veröffentlichung erhalten Sie eine Aufforderung zum Update auf die neue Version. Wenn Sie nur das [Zuordnungsfeature](vminsights-maps.md) verwenden möchten, müssen Sie kein Upgrade durchführen und können das Zuordnungsfeature von Azure Monitor für VMs sowie die Dienstzuordnungslösung, auf die Sie über Ihren Arbeitsbereich oder die Dashboardkachel zugreifen, weiterhin verwenden.

Wenn Sie die Leistungsindikatoren in Ihrem Arbeitsbereich manuell aktivieren, können Sie möglicherweise Daten in einigen unserer Leistungsdiagramme sehen, die über Azure Monitor angezeigt werden. Nach der Veröffentlichung der neuen Lösung nehmen wir eine Aktualisierung unserer Leistungsdiagramme vor, um die in der `InsightsMetrics`-Tabelle gespeicherten Daten abzufragen. Wenn Sie Daten aus dieser Tabelle in diesen Diagrammen anzeigen möchten, müssen Sie ein Upgrade auf die neue Version von Azure Monitor für VMs durchführen.

Die Änderungen zum Verschieben von Daten aus `ServiceMapComputer_CL` und `ServiceMapProcess_CL` wirken sich sowohl auf die Dienstzuordnung als auch Azure Monitor für VMs aus, sodass Sie dieses Update trotzdem einplanen müssen.

Wenn Sie kein Upgrade auf die **VMInsights**-Lösung durchführen, werden wir weiterhin Legacyversionen unserer Arbeitsmappen zur Leistung bereitstellen, die sich auf Daten in der `Perf`-Tabelle beziehen.  

## <a name="will-the-service-map-data-sets-also-be-stored-in-insightsmetrics"></a>Werden die Datasets der Dienstzuordnung auch in InsightsMetrics gespeichert?

Die Datasets werden nicht dupliziert, wenn Sie beide Lösungen verwenden. Beide Angebote nutzen die Datasets gemeinsam, die in den Tabellen `VMComputer` (zuvor „ServiceMapComputer_CL“), `VMProcess` (zuvor „ServiceMapProcess_CL“), `VMConnection` und `VMBoundPort` zum Speichern der von uns gesammelten Zuordnungsdatasets gespeichert werden.  

In der Tabelle `InsightsMetrics` werden die von uns gesammelten VM-, Prozess- und Dienstdatasets gespeichert, und sie wird nur aufgefüllt, wenn Sie Azure Monitor für VMs verwenden. Die Dienstzuordnungslösung erfasst keine Daten und speichert nichts in der Tabelle `InsightsMetrics`.

## <a name="will-i-be-double-charged-if-i-have-the-service-map-and-vminsights-solutions-in-my-workspace"></a>Wenn in meinem Arbeitsbereich sowohl die Dienstzuordnungslösung als auch die VMInsights-Lösung vorhanden ist, werden mir die Daten dann doppelt in Rechnung gestellt?

Nein. Die beiden Lösungen nutzen die Zuordnungsdatasets gemeinsam, die wir in `VMComputer` (zuvor „ServiceMapComputer_CL“), `VMProcess` (zuvor „ServiceMapProcess_CL“), `VMConnection` und `VMBoundPort` speichern. Diese Daten werden Ihnen nicht doppelt in Rechnung gestellt, wenn beide Lösungen in Ihrem Arbeitsbereich vorhanden sind.

## <a name="if-i-remove-either-the-service-map-or-vminsights-solution-will-it-remove-my-data"></a>Wenn ich die Dienstzuordnungslösung oder die VMInsights-Lösung entferne, werden dann auch meine Daten entfernt?

Nein. Die beiden Lösungen nutzen die Zuordnungsdatasets gemeinsam, die wir in `VMComputer` (zuvor „ServiceMapComputer_CL“), `VMProcess` (zuvor „ServiceMapProcess_CL“), `VMConnection` und `VMBoundPort` speichern. Wenn Sie eine der Lösungen entfernen, erkennen diese Datasets, dass immer noch eine Lösung vorhanden ist, die die Daten verwendet, und die Daten verbleiben im Log Analytics-Arbeitsbereich. Sie müssen beide Lösungen aus Ihrem Arbeitsbereich entfernen, damit die Daten entfernt werden.

## <a name="when-will-this-update-be-released"></a>Wann wird dieses Update veröffentlicht?

Wir gehen davon aus, dass das Update für Azure Monitor für VMs Anfang Januar 2020 veröffentlicht wird. Wenn das Veröffentlichungsdatum näher rückt, werden wir hier Updates veröffentlichen und Benachrichtigungen im Azure-Portal einblenden, wenn Sie Azure Monitor öffnen.

## <a name="health-feature-is-in-limited-public-preview"></a>Integritätsfeature in der eingeschränkten Public Preview

Wir haben von Kunden viel wertvolles Feedback zu unserem VM-Integritätsfeature erhalten. Es besteht starkes Interesse an diesem Feature und seinen Möglichkeiten für die Unterstützung von Überwachungsworkflows. Wir planen eine Reihe von Änderungen, um Funktionen hinzuzufügen und das erhaltene Feedback umzusetzen. 

Um die Auswirkungen dieser Änderungen auf neue Kunden möglichst gering zu halten, haben wir dieses Feature in eine **eingeschränkte Public Preview** verschoben. Dieses Update fand im Oktober 2019 statt.

Wir planen, dieses Integritätsfeature 2020 erneut zu veröffentlichen, nachdem Azure Monitor für VMs allgemein verfügbar ist.

## <a name="how-do-existing-customers-access-the-health-feature"></a>Wie greifen vorhandene Kunden auf das Integritätsfeature zu?

Vorhandene Kunden, die das Integritätsfeature verwenden, können weiterhin darauf zugreifen, doch wird es neuen Kunden nicht angeboten.  

Für den Zugriff auf das Feature können Sie der Azure-Portal-URL [https://portal.azure.com](https://portal.azure.com) das Featureflag `feature.vmhealth=true` hinzufügen. Beispiel: `https://portal.azure.com/?feature.vmhealth=true`.

Sie können auch diese kurze URL verwenden, mit der das Featureflag automatisch gesetzt wird: [https://aka.ms/vmhealthpreview](https://aka.ms/vmhealthpreview).

Als vorhandener Kunde können Sie das Integritätsfeature weiterhin für VMs verwenden, die mit einer bestehenden Arbeitsbereichseinrichtung mit Integritätsfunktion verbunden sind.  

## <a name="i-use-vm-health-now-with-one-environment-and-would-like-to-deploy-it-to-a-new-one"></a>Ich verwende VM-Integrität derzeit für eine Umgebung und möchte das Feature für eine neue bereitstellen

Wenn Sie bereits ein Kunde sind, der das Integritätsfeature verwendet, und Sie das Feature für ein neues Rollout verwenden möchten, nehmen Sie unter vminsights@microsoft.com Kontakt mit uns auf, um Anweisungen anzufordern.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den Anforderungen und Methoden, die Ihnen beim Überwachen Ihrer virtuellen Computer helfen, finden Sie unter [Bereitstellen von Azure Monitor für VMs](vminsights-enable-overview.md).
