---
title: Einrichten von Warnungen
titleSuffix: Azure Digital Twins
description: Es wird beschrieben, wie Sie Warnungen in Azure Digital Twins-Metriken aktivieren.
author: baanders
ms.author: baanders
ms.date: 7/28/2020
ms.topic: troubleshooting
ms.service: digital-twins
ms.openlocfilehash: 1b296cd942e36817da2832467ab603ebd833f825
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87907598"
---
# <a name="troubleshooting-azure-digital-twins-alerts"></a>Problembehandlung von Azure Digital Twins: Alerts

Azure Digital Twins sammelt [Metriken](troubleshoot-metrics.md) für Ihre Dienstinstanz, die Informationen zum Zustand Ihrer Ressourcen bereitstellen. Mit diesen Metriken können Sie die allgemeine Integrität des Azure Digital Twins-Diensts und der damit verbundenen Ressourcen bewerten.

**Warnungen** informieren Sie proaktiv, wenn wichtige Bedingungen in Ihren Metrikdaten gefunden werden. Sie ermöglichen es Ihnen, Probleme zu identifizieren und zu beheben, bevor die Benutzer Ihres Systems sie bemerken. Weitere Informationen zu Warnungen finden Sie unter [*Überblick über Warnungen in Microsoft Azure*](../azure-monitor/platform/alerts-overview.md).

## <a name="turn-on-alerts"></a>Aktivieren von Warnungen

Hier ist beschrieben, wie Sie Warnungen für Ihre Azure Digital Twins-Instanz aktivieren:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu Ihrer Azure Digital Twins-Instanz. Sie finden sie, indem Sie ihren Namen auf der Suchleiste des Portals eingeben. 

2. Wählen Sie im Menü die Option **Warnungen** und dann **+ Neue Warnungsregel** aus.

    :::image type="content" source="media/troubleshoot-alerts/alerts-pre.png" alt-text="Screenshot: Seite „Warnungen“ und Schaltfläche zum Hinzufügen. Es sind noch keine Warnungen konfiguriert." lightbox="media/troubleshoot-alerts/alerts-pre.png":::

3. Auf der Seite *Warnungsregel erstellen*, die als Nächstes angezeigt wird, können Sie die Anweisungen befolgen, um Bedingungen, die auszulösenden Aktionen und die Warnungsdetails zu definieren.     
    * Die Details unter **Bereich** sollten zusammen mit den Details für Ihre Instanz automatisch eingefügt werden.
    * Sie definieren die Details für **Bedingung** und **Aktionsgruppe**, um Warnungstrigger und die zugehörigen Antworten anzupassen.

    :::image type="content" source="media/troubleshoot-alerts/create-alert-rule.png" alt-text="Screenshot: Seite „Warnungsregel erstellen“ mit Abschnitten für Bereich, Bedingung und Aktionsgruppe" lightbox="media/troubleshoot-alerts/create-alert-rule.png":::

Eine Anleitung zum Ausfüllen dieser Felder finden Sie unter [*Überblick über Warnungen in Microsoft Azure*](../azure-monitor/platform/alerts-overview.md). Unten sind einige Beispiele dafür angegeben, welche Schritte für Azure Digital Twins ausgeführt werden müssen.

Hier ist ein Auszug aus dem Prozess *Bedingung auswählen* angegeben, um zu veranschaulichen, welche Arten von Warnungssignalen für Azure Digital Twins verfügbar sind. Auf dieser Seite können Sie nach dem Typ des Signals filtern und das gewünschte Signal in einer Liste auswählen.

:::image type="content" source="media/troubleshoot-alerts/configure-signal-logic.png" alt-text="Screenshot: Erste Seite „Signallogik konfigurieren“. Das Feld „Signaltyp“ ist hervorgehoben, über das Metriken und Aktivitätsprotokolle ausgewählt werden können, und unterhalb davon können Sie eine Liste mit Metriken auswählen.":::

Nachdem Sie ein Signal ausgewählt haben, werden Sie aufgefordert, die Logik der Warnung zu konfigurieren. Sie können nach einer Dimension filtern und einen Schwellenwert für Ihre Warnung sowie die Häufigkeit der Überprüfungen für die Bedingung festlegen. Hier ist ein Beispiel für die Einrichtung einer Warnung für den Fall angegeben, dass der Durchschnittswert für die Metrik „Routingfehlerrate“ über 5 % steigt.

:::image type="content" source="media/troubleshoot-alerts/configure-signal-logic-2.png" alt-text="Screenshot: Zweite Seite „Signallogik konfigurieren“. Auf der Seite wird der Metrikverlauf angezeigt, sie verfügt über einen Bereich zum Filtern nach einer Dimension, z. B. Event Grid-Vorgänge, und es ist ein Abschnitt zum Definieren der Warnungslogik vorhanden (Beispiel: „Durchschnittswert größer als 5“).":::
 
Nach dem Einrichten von Warnungen werden diese auf der Seite *Warnungen* für Ihre Instanz angezeigt.
 
:::image type="content" source="media/troubleshoot-alerts/alerts-post.png" alt-text="Screenshot: Seite „Warnungen“ und Schaltfläche zum Hinzufügen. Eine Warnung ist konfiguriert." lightbox="media/troubleshoot-alerts/alerts-post.png":::


## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Warnungen mit Azure Monitor finden Sie unter [*Überblick über Warnungen in Microsoft Azure*](../azure-monitor/platform/alerts-overview.md).
* Informationen zu den Azure Digital Twins-Metriken finden Sie unter [*Problembehandlung: Anzeigen von Metriken mit Azure Monitor*](troubleshoot-metrics.md).
* Informationen zur Aktivierung der Diagnoseprotokollierung für Ihre Metriken finden Sie unter [*Problembehandlung: Einrichten der Diagnose*](troubleshoot-diagnostics.md).
