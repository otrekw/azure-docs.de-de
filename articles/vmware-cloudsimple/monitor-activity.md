---
title: 'Azure VMware Solutions (AVS): Überwachen der Aktivität in einer privaten AVS-Cloud'
description: Beschreibt die Informationen, die zu Aktivitäten in der AVS-Umgebung (Azure VMware Solutions) verfügbar sind, einschließlich Warnungen, Ereignissen, Aufgaben und Überwachung.
titleSuffix: Azure VMware Solutions (AVS)
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/13/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 5d6fd1b92db62ab7cc9edd47c601910b8148bb95
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77019671"
---
# <a name="monitor-vmware-solutions-avs-activity"></a>Überwachen von Aktivitäten in VMware Solutions (AVS)

AVS-Aktivitätsprotokolle ermöglichen Erkenntnisse zu Vorgängen, die im AVS-Portal durchgeführt werden. Die Liste enthält Warnungen, Ereignisse, Aufgaben und Überwachung. Verwenden Sie die Aktivitätsprotokolle, um zu bestimmen, wann welche Vorgänge ausgeführt wurden, und wer sie ausgeführt hat. Aktivitätsprotokolle enthalten keine Lesevorgänge, die von einem Benutzer durchgeführt werden.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="access-the-avs-portal"></a>Zugreifen auf das AVS-Portal

Greifen Sie auf das [AVS-Portal](access-cloudsimple-portal.md) zu.

## <a name="activity-information"></a>Informationen zur Aktivität

Um auf die Aktivitätsseiten zuzugreifen, wählen Sie im seitlichen Menü **Aktivität** aus.

![Übersicht zur Aktivitätsseite](media/activity-page-overview.png)

Um Details zu Aktivitäten auf der Aktivitätsseite anzuzeigen, wählen Sie die Aktivität aus. Ein Detailbereich wird auf der rechten Seite geöffnet. Die Aktionen im Bereich sind vom Typ der Aktivität abhängig. Klicken Sie auf **X**, um den Bereich zu schließen.

Klicken Sie auf eine Spaltenüberschrift, um die Anzeige zu sortieren. Sie können Spalten nach bestimmten Werten filtern, die Sie anzeigen möchten. Laden Sie den Aktivitätsbericht durch Klicken auf das Symbol **Als CSV herunterladen** herunter.

## <a name="alerts"></a>Alerts

Bei Warnungen handelt es sich um Benachrichtigungen zu wichtigen Aktivitäten in der AVS-Umgebung. Warnungen enthalten Ereignisse, die sich auf die Abrechnung oder den Benutzerzugriff auswirken.

Wenn Sie Warnungen bestätigen und aus der Liste entfernen möchten, wählen Sie mindestens eine in der Liste aus, und klicken Sie auf **Bestätigen**.

Die folgenden Spalten mit Informationen sind für Warnungen verfügbar. Klicken Sie auf **Spalten bearbeiten**, und wählen Sie die Spalten aus, die Sie anzeigen möchten.

| Column | Beschreibung |
------------ | ------------- |
| Warnungstyp | Kategorie der Warnung.|
| Time | Zeitpunkt, zu dem die Warnung auftrat. |
| severity | Bedeutung der Warnung.|
| Ressourcenname | Name, der der Ressource zugewiesen ist, z. B. der Name der privaten AVS-Cloud. |
| Ressourcentyp | Kategorie der Ressource: Private AVS-Cloud, Cloudrack. |
| Ressourcen-ID | Bezeichner der Ressource. |
| Beschreibung | Beschreibung des Auslösers der Warnung. |
| Bestätigt | Angabe, ob die Warnung bestätigt wurde. |

## <a name="events"></a>Events

Mit Ereignissen werden Benutzer- und Systemaktivitäten im AVS-Portal angezeigt. Auf der Seite „Ereignisse“ werden die mit einer bestimmten Ressource verknüpften Aktivitäten sowie der Schweregrad der Auswirkungen angezeigt.

Die folgenden Spalten mit Informationen sind für Warnungen verfügbar. Klicken Sie auf **Spalten bearbeiten**, und wählen Sie die Spalten aus, die Sie anzeigen möchten.

| Column | Beschreibung |
------------ | ------------- |
| Time | Angabe des Zeitpunkts, zu dem das Ereignis aufgetreten ist, mit Datum und Uhrzeit. |
| Ereignistyp | Numerischer Code, der das Ereignis identifiziert. |
| severity | Ereignisschweregrad.|
| Ressourcenname | Name, der der Ressource zugewiesen ist, z. B. der Name der privaten AVS-Cloud. |
| Ressourcentyp | Kategorie der Ressource: Private AVS-Cloud, Cloudrack. |
| Beschreibung | Beschreibung des Auslösers der Warnung. |

## <a name="tasks"></a>Aufgaben

Bei Aufgaben handelt es sich um Aktivitäten in einer privaten AVS-Cloud, die voraussichtlich mindestens 30 Sekunden dauern. (Aktivitäten, die voraussichtlich weniger als 30 Sekunden dauern, werden nur als Ereignisse gemeldet.) Öffnen Sie die Seite „Aufgaben“, um den Fortschritt von Aufgaben für die private AVS-Cloud zu verfolgen.

Die folgenden Spalten mit Informationen sind für Warnungen verfügbar. Klicken Sie auf **Spalten bearbeiten**, und wählen Sie die Spalten aus, die Sie anzeigen möchten.

| Column | Beschreibung |
------------ | ------------- |
| Task-ID | Eindeutiger Bezeichner für die Aufgabe. |
| Vorgang | Aktion, die die Aufgabe ausführt. |
| Benutzer | Der Benutzer, dem die Durchführung der Aufgabe zugewiesen ist. |
| Ressourcenname | Name, der der Ressource zugewiesen ist. |
| Ressourcentyp | Kategorie der Ressource: Private AVS-Cloud, Cloudrack. |
| Ressourcen-ID | Bezeichner der Ressource. |
| Start | Startzeit für die Aufgabe. |
| Ende | Endzeit für die Aufgabe. |
| Status | Aktueller Aufgabenstatus. |
| Verstrichene Zeit | Zeitspanne, die die Durchführung der Aufgabe (sofern durchgeführt) in Anspruch nahm bzw. (bei laufender Durchführung) bisher in Anspruch genommen hat. |
| Beschreibung | Aufgabenbeschreibung. |

## <a name="audit"></a>Audit

Überwachungsprotokolle verfolgen die Benutzeraktivität nach. Überwachungsprotokolle können Sie zum Überwachen von Benutzeraktivitäten für alle Benutzer verwenden.

Die folgenden Spalten mit Informationen sind für Warnungen verfügbar. Klicken Sie auf **Spalten bearbeiten**, und wählen Sie die Spalten aus, die Sie anzeigen möchten.

| Column | Beschreibung |
------------ | ------------- |
| Time | Zeitpunkt des Überwachungseintrags. |
| Vorgang | Aktion, die die Aufgabe ausführt. |
| Benutzer | Benutzer, der der Aufgabe zugewiesen ist. |
| Ressourcenname | Name, der der Ressource zugewiesen ist. |
| Ressourcentyp | Kategorie der Ressource: Private AVS-Cloud, Cloudrack. |
| Ressourcen-ID | Bezeichner der Ressource. |
| Ergebnis | Ergebnis der Aktivität, z. B. **Erfolg**. |
| Benötigte Zeit | Zeitspanne zum Durchführen dieser Aufgabe. |
| Beschreibung | Beschreibung der Aktion. |

## <a name="next-steps"></a>Nächste Schritte

* [Nutzen von virtuellen VMware-Computern in Azure](quickstart-create-vmware-virtual-machine.md)
* Informieren Sie sich über [private AVS-Clouds](cloudsimple-private-cloud.md).
