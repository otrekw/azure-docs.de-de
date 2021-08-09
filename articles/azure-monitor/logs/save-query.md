---
title: Speichern einer Abfrage in Azure Monitor Log Analytics (Vorschau)
description: Beschreibt das Speichern einer Abfrage in Log Analytics.
ms.subservice: logs
ms.topic: article
author: bwren
ms.author: bwren
ms.date: 05/20/2021
ms.openlocfilehash: 0334fa67849acdf628ec77a55fcf7d7f5435cbf6
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110482445"
---
# <a name="save-a-query-in-azure-monitor-log-analytics-preview"></a>Speichern einer Abfrage in Azure Monitor Log Analytics (Vorschau)
[Protokollabfragen](log-query-overview.md) sind Anforderungen in Azure Monitor, mit denen Sie Daten in einem Log Analytics-Arbeitsbereich verarbeiten und abrufen können. Das Speichern von Protokollabfragen ermöglicht Ihnen:

- Verwenden der Abfrage in allen Log Analytics-Kontexten, einschließlich arbeitsbereichs- und ressourcenbezogen.
- Ermöglichen, dass andere Benutzer dieselbe Abfrage ausführen.
- Erstellen einer Bibliothek mit häufigen Abfragen für Ihre Organisation.

## <a name="save-options"></a>Speicheroptionen
Wenn Sie eine Abfrage speichern, wird sie in einem Abfragepaket gespeichert, das gegenüber der vorherigen Methode zum Speichern der Abfrage in einem Arbeitsbereich die folgenden Vorteile hat. Das Speichern in einem Abfragepaket ist die bevorzugte Methode, die die folgenden Vorteile bietet:

- Einfachere Erkennbarkeit durch die Möglichkeit, Abfragen nach verschiedenen Eigenschaften zu filtern und zu gruppieren.
- Abfragen, die bei Verwendung eines Ressourcenbereichs in Log Analytics verfügbar sind.
- Stellen Sie Abfragen abonnementübergreifend zur Verfügung.
- Es sind weitere Daten verfügbar, um die Abfrage zu beschreiben und zu kategorisieren.


## <a name="save-a-query"></a>Speichern einer Abfrage
Um eine Abfrage in einem Abfragepaket zu speichern, wählen Sie in Log Analytics in der Dropdownliste **Speichern** die Option **Als Log Analytics-Abfrage speichern** aus.

[![Menü „Abfrage speichern“](media/save-query/save-query.png)](media/save-query/save-query.png#lightbox)

Wenn Sie eine Abfrage in einem Abfragepaket speichern, wird das folgende Dialogfeld angezeigt, in dem Sie Werte für die Abfrageeigenschaften bereitstellen können. Die Abfrageeigenschaften werden zum Filtern und Gruppieren ähnlicher Abfragen verwendet, damit Sie die gesuchte Abfrage finden können. Eine ausführliche Beschreibung der einzelnen Eigenschaften finden Sie unter [Abfrageeigenschaften](queries.md#query-properties).

Die meisten Benutzer sollten die Option **Im Standardabfragepaket speichern** ausgewählt lassen, wodurch die Abfrage im [Standardabfragepaket](query-packs.md#default-query-pack) gespeichert wird. Deaktivieren Sie dieses Kontrollkästchen, wenn Ihr Abonnement andere Abfragepakete enthält. Ausführliche Informationen zum Erstellen eines neuen Abfragepakets finden Sie unter [Abfragepakete in Azure Monitor-Protokollen (Vorschau)](query-packs.md).

[![Dialogfeld „Abfrage speichern“](media/save-query/save-query-dialog.png)](media/save-query/save-query-dialog.png#lightbox)

## <a name="edit-a-query"></a>Bearbeiten einer Abfrage
Möglicherweise möchten Sie eine Abfrage bearbeiten, die Sie bereits gespeichert haben. Dies kann sein, um die Abfrage selbst oder eine ihrer Eigenschaften zu ändern. Nachdem Sie eine vorhandene Abfrage in Log Analytics geöffnet haben, können Sie sie bearbeiten, indem Sie in der Dropdownliste **Speichern** die Option **Abfragedetails bearbeiten** auswählen. Dadurch können Sie die bearbeitete Abfrage mit den gleichen Eigenschaften speichern oder Eigenschaften ändern, bevor Sie sie speichern.

Wenn Sie die Abfrage mit einem anderen Namen speichern möchten, wählen Sie **Als Log Analytics-Abfrage speichern** genauso aus, als würden Sie eine neue Abfrage erstellen. 


## <a name="save-as-a-legacy-query"></a>Als Legacyabfrage speichern
Das Speichern als Legacyabfrage wird aufgrund der oben aufgeführten Vorteile von Abfragepaketen nicht empfohlen. Sie können eine Abfrage jedoch im Arbeitsbereich speichern, um sie mit anderen Abfragen zu kombinieren, die vor der Veröffentlichung von Abfragepaketen im Arbeitsbereich gespeichert wurden. 

Um eine Legacyabfrage zu speichern, wählen Sie in Log Analytics in der Dropdownliste **Speichern** die Option **Als Log Analytics-Abfrage speichern** aus. Wählen Sie die Option **Als Legacyabfrage speichern** aus. Die einzige verfügbare Option ist die Legacykategorie.


## <a name="next-steps"></a>Nächste Schritte

[Erste Schritte mit KQL-Abfragen](get-started-queries.md)
