---
title: Das Überprüfungstool „ArrInspector“
description: Benutzerhandbuch zum Tool „ArrInspector“
author: florianborn71
ms.author: flborn
ms.date: 03/09/2020
ms.topic: article
ms.openlocfilehash: 300e0ff26d643ae0263d21e604cb26da37a18841
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "97723824"
---
# <a name="the-arrinspector-inspection-tool"></a>Das Überprüfungstool „ArrInspector“

ArrInspector ist ein webbasiertes Tool zum Überprüfen einer laufenden Azure Remote Rendering-Sitzung. Das Tool dient zu Debuggingzwecken, zum Überprüfen der Struktur der gerenderten Szene, zum Anzeigen der Protokollmeldungen sowie zur Liveüberwachung der Leistung des Servers.

![ArrInspector](./media/arr-inspector.png)

## <a name="connecting-to-the-arrinspector"></a>Herstellen einer Verbindung mit dem ArrInspector

Stellen Sie nach dem Abrufen des Hostnamens Ihres ARR-Servers (der auf `mixedreality.azure.com` endet) eine Verbindung mithilfe von [ConnectToArrInspectorAsync](../../how-tos/frontend-apis.md#connect-to-arr-inspector) her. Mit dieser Funktion wird eine `StartArrInspector.html` auf dem Gerät erstellt, auf dem die Anwendung ausgeführt wird. Öffnen Sie zum Starten von ArrInspector diese Datei in einem Browser (Microsoft Edge, Firefox oder Chrome) auf einem PC. Die Datei ist nur 24 Stunden lang gültig.

Wenn die App, die `ConnectToArrInspectorAsync` aufruft, bereits auf einem PC ausgeführt wird:

* Wenn Sie die Unity-Integration verwenden, erfolgt der Start möglicherweise automatisch.
* Andernfalls finden Sie die Datei in *User Folders\\LocalAppData\\[Ihre_App]\\AC\\Temp*.

Wenn die App auf einer HoloLens ausgeführt wird:

1. Greifen Sie über das [Windows Device Portal](/windows/mixed-reality/using-the-windows-device-portal) auf die HoloLens zu.
1. Wechseln Sie zu *System > Datei-Explorer*.
1. Navigieren Sie zu *User Folders\\LocalAppData\\[Ihre_App]\\AC\\Temp*.
1. Speichern Sie *StartArrInspector.html* auf dem PC.
1. Öffnen Sie *StartArrInspector.html*, um den ArrInspector für die Sitzung zu laden.

## <a name="the-performance-panel"></a>Leistungsbereich

![Leistungsbereich](./media/performance-panel.png)

In diesem Bereich werden Diagramme aller Pro-Frame-Leistungswerte angezeigt, die vom Server bereitgestellt werden. Die Werte umfassen derzeit Framedauer, FPS, CPU- und Speicherauslastung sowie Arbeitsspeicherstatistiken wie die RAM-Gesamtauslastung, Objektanzahlen usw.

Um einen dieser Parameter zu visualisieren, klicken Sie auf die Schaltfläche **Neu hinzufügen**, und wählen Sie einen der im Dialogfeld verfügbaren Werte aus. Durch diese Aktion wird dem Bereich ein neues fortlaufendes Diagramm hinzugefügt, in dem die Werte in Echtzeit verfolgt werden. Rechts sehen Sie den *minimalen*, *maximalen* und *aktuellen* Wert.

Sie können das Diagramm schwenken, indem Sie seinen Inhalt mit der Maus ziehen; das horizontale Schwenken ist jedoch nur möglich, wenn sich ArrInspector im angehaltenen Zustand befindet.

Wenn Sie beim Ziehen STRG gedrückt halten, können Sie zoomen. Das horizontale Zoomen kann auch mit dem Schieberegler am unteren Rand gesteuert werden.

Der vertikale Bereich wird standardmäßig auf der Grundlage der aktuell angezeigten Werte berechnet, und die Minimal- und Maximalwerte werden in den Textfeldern rechts angezeigt. Wenn die Werte manuell festgelegt werden, entweder durch direkte Eingabe im Textfeld oder durch Schwenken/Zoomen, werden diese Werte vom Diagramm verwendet. Klicken Sie auf das Symbol in der oberen rechten Ecke, um den automatischen vertikalen Rahmen wiederherzustellen.

![vertikaler Bereich](./media/vertical-range.png)

## <a name="the-log-panel"></a>Protokollbereich

![Protokollbereich](./media/log-panel.png)

Im Protokollbereich wird eine Liste der serverseitig generierten Protokollmeldungen angezeigt. Bei bestehender Verbindung werden bis zu 200 frühere Protokollmeldungen angezeigt, und es werden neue Meldungen ausgegeben, sobald sie auftreten.

Sie können die Liste anhand des Protokolltyps `[Error/Warning/Info/Debug]` über die Schaltflächen am oberen Rand filtern.
![Schaltflächen zum Filtern des Protokolls](./media/log-filter.png)

## <a name="the-timing-data-capture-panel"></a>Zeitdatenerfassungsbereich

![Zeitdatenerfassung](./media/timing-data-capture.png)

In diesem Bereich werden Zeitsteuerungsdaten vom Server erfasst und heruntergeladen. Die Datei verwendet das [Chrome Tracing JSON-Format](https://docs.google.com/document/d/1CvAClvFfyA5R-PhYUmn5OOQtYMH4h6I0nSsKchNAySU/edit). Öffnen Sie zum Überprüfen der Daten Chrome mit der URL `Chrome://tracing`, und verschieben Sie die heruntergeladen Datei per Drag & Drop auf die Seite. Die Zeitsteuerungsdaten werden laufend in einem Ringpuffer mit fester Größe erfasst. Bei der Ausgabe enthält die Erfassung lediglich Informationen über den direkt zurückliegenden Zeitraum, d. h. für einige Sekunden bis zu einigen Minuten.

## <a name="the-scene-inspection-panel"></a>Szenenüberprüfungszeitraum

![Szenenüberprüfungszeitraum](./media/scene-inspection-panel.png)

In diesem Bereich wird die Struktur der gerenderten Szene veranschaulicht. Die Objekthierarchie befindet sich links, während sich der Inhalt des ausgewählten Objekts rechts befindet. Der Bereich ist schreibgeschützt und wird in Echtzeit aktualisiert.

## <a name="the-vm-debug-information-panel"></a>Bereich mit VM-Debuginformationen

![Bereich mit VM-Debuginformationen](./media/state-debugger-panel.png)

Dieser Bereich bietet einige Debuggingfunktionen.

### <a name="restart-service"></a>Dienst neu starten

Mit der Schaltfläche **Dienst neu starten** wird die Laufzeit auf dem virtuellen Computer neu gestartet, mit dem ArrInspector verbunden ist. Jeder angefügte Client wird getrennt, und die ArrInspector-Seite muss neu geladen werden, um eine Verbindung mit dem neu gestarteten Dienst herzustellen.

### <a name="collect-debug-information"></a>Sammeln von Debuginformationen

Über die Schaltfläche **Debuginformationen für VM sammeln** wird ein Dialogfeld geöffnet, in dem Sie die ARR-Instanz auslösen können, um Debuginformationen zur VM zu sammeln:

![Dialogfeld für VM-Debuginformationen](./media/state-debugger-dialog.png)

Anhand von Debuginformationen kann das Azure Remote Rendering-Team etwaige Probleme mit einer ausgeführten ARR-Instanz analysieren. Das Dialogfeld enthält ein Textfeld, in dem zusätzliche Details angegeben werden können, z. B. Schritte zum Reproduzieren eines Problems.

Wenn Sie auf die Schaltfläche **Sammlung starten** klicken, wird das Dialogfeld geschlossen, und der Sammlungsvorgang wird gestartet. Das Sammeln der Informationen auf der VM kann einige Minuten dauern.

![Laufende Sammlung von VM-Debuginformationen](./media/state-debugger-panel-in-progress.png)

Nach Abschluss der Sammlung wird im ArrInspector-Fenster eine Benachrichtigung ausgegeben. Diese Benachrichtigung enthält eine ID, welche die konkrete Sammlung identifiziert. Speichern Sie diese ID, damit Sie sie dem Azure Remote Rendering-Team mitteilen können.

![Erfolgreiche Sammlung von VM-Debuginformationen](./media/state-debugger-snackbar-success.png)

> [!IMPORTANT]
> VM-Debuginformationen können nicht heruntergeladen oder anderweitig abgerufen werden. Nur das Azure Remote Rendering-Team hat Zugriff auf die gesammelten Daten. Sie müssen sich an uns wenden und die Sammlungs-ID senden, damit wir das aufgetretene Problem untersuchen können.

## <a name="pause-mode"></a>Angehaltener Modus

In der oberen rechten Ecke können Sie mithilfe eines Schalters das Liveupdate des Bereichs anhalten. Dieser Modus kann hilfreich sein, wenn Sie einen bestimmten Zustand eingehend überprüfen möchten.

![Angehaltener Modus](./media/pause-mode.png)

Beim erneuten Aktivieren des Liveupdates werden alle Bereiche zurückgesetzt.