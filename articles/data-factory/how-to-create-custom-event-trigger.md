---
title: Erstellen von Auslösern für benutzerdefinierte Ereignisse in Azure Data Factory
description: Hier erfahren Sie, wie in Azure Data Factory ein Auslöser für benutzerdefinierte Ereignisse erstellt wird, der eine Pipeline als Reaktion auf ein in Event Grid veröffentlichtes benutzerdefiniertes Ereignis ausführt.
ms.service: data-factory
author: chez-charlie
ms.author: chez
ms.reviewer: jburchel
ms.topic: conceptual
ms.date: 03/11/2021
ms.openlocfilehash: 2d2f26b24e7fa10d9244de8f99d78c64a44b3d61
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104785647"
---
# <a name="create-a-trigger-that-runs-a-pipeline-in-response-to-a-custom-event-preview"></a>Erstellen eines Auslösers zum Ausführen einer Pipeline als Reaktion auf ein benutzerdefiniertes Ereignis (Vorschau)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

In diesem Artikel werden Auslöser für benutzerdefinierte Ereignisse erläutert, den Sie in Ihren Data Factory-Pipelines erstellen können.

Die ereignisgesteuerte Architektur (EDA) ist ein allgemeines Datenintegrationsmuster, das die Produktion, Erkennung, Verbrauch und Reaktion auf Ereignisse beinhaltet. In Datenintegrationsszenarien müssen Data Factory-Kunden häufig Pipelines bei Eintritt bestimmter Ereignisse auslösen. Die native Integration von Data Factory mit [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) deckt jetzt [Benutzerdefinierte Ereignisse](../event-grid/custom-topics.md) ab: Kunden senden beliebige Ereignisse an ein Event Grid-Thema, und Data Factory abonnieren und lauscht auf das Thema und löst entsprechend Pipelines aus.

> [!NOTE]
> Die in diesem Artikel beschriebene Integration basiert auf [Azure Event Grid](https://azure.microsoft.com/services/event-grid/). Stellen Sie sicher, dass Ihr Abonnement für den Event Grid-Ressourcenanbieter registriert ist. Weitere Informationen finden Sie unter [Ressourcenanbieter und -typen](../azure-resource-manager/management/resource-providers-and-types.md#azure-portal). Sie müssen dazu in der Lage sein, die Aktion *Microsoft.EventGrid/eventSubscriptions/* * auszuführen. Diese Aktion ist Teil der integrierten Rolle EventGrid-EventSubscription-Mitwirkender.

Außerdem können Kunden durch Kombination von Pipelineparametern und Auslösern für benutzerdefinierte Ereignisse eine benutzerdefinierte _data_-Nutzlast in Pipelineausführungen analysieren und darauf verweisen. Das Feld _data_ in der Nutzlast eines benutzerdefinierten Ereignisses ist eine Freiform-Schlüssel-Wert-Struktur in JSON, die Kunden eine maximale Kontrolle über die ereignisgesteuerten Pipelineausführungen erlaubt.

> [!IMPORTANT]
> Zuweilen fehlt ein Schlüssel, auf den in der Parametrisierung verwiesen wird, in der Nutzlast des benutzerdefinierten Ereignisses. Die _Ausführung des Auslösers_ schlägt fehl, und es wird angegeben, dass der Ausdruck nicht ausgewertet werden kann, da die _keyName_-Eigenschaft nicht vorhanden ist. Das Ereignis löst __keine__ _Pipelineausführung_ aus.

## <a name="setup-event-grid-custom-topic"></a>Einrichten eines benutzerdefinierten Event Grid-Themas

Zum Verwenden eines Auslösers für ein benutzerdefiniertes Ereignis in Data Factory müssen Sie _zunächst_ ein [benutzerdefiniertes Thema in Event Grid](../event-grid/custom-topics.md) einrichten. Der Workflow unterscheidet sich von dem für einen Speicherereignisauslöser, bei dem Data Factory das Thema für Sie einrichtet. Hier müssen Sie im Azure Event Grid navigieren und das Thema selbst erstellen. Weitere Informationen zum Erstellen des benutzerdefinierten Themas finden Sie in den [Portal-Tutorials](../event-grid/custom-topics.md#azure-portal-tutorials) und den [CLI-Tutorials](../event-grid/custom-topics.md#azure-cli-tutorials) für Azure Event Grid.

Data Factorys erwarten, dass die Ereignisse dem [Event Grid-Ereignisschema](../event-grid/event-schema.md) folgen. Stellen Sie sicher, dass die Ereignisnutzlasten über folgende Felder verfügen.

```json
[
  {
    "topic": string,
    "subject": string,
    "id": string,
    "eventType": string,
    "eventTime": string,
    "data":{
      object-unique-to-each-publisher
    },
    "dataVersion": string,
    "metadataVersion": string
  }
]
```

## <a name="data-factory-ui"></a>Data Factory-Benutzeroberfläche

In diesem Abschnitt wird gezeigt, wie Sie über die Benutzeroberfläche von Azure Data Factory einen Trigger für Speicherereignisse erstellen können.

1. Wechseln Sie zur Registerkarte **Bearbeiten**, die mit einem Stiftsymbol angezeigt wird.

1. Wählen Sie im Menü **Trigger** und dann **Neu/Bearbeiten** aus.

1. Wählen Sie auf der Seite **Trigger hinzufügen** die Option **Trigger auswählen...** und dann **+Neu** aus.

1. Wählen Sie den Auslösertyp **Benutzerdefinierte Ereignisse** aus.

    :::image type="content" source="media/how-to-create-custom-event-trigger/custom-event-1-creation.png" alt-text="Screenshot der Seite „Autor“ zum Erstellen eines neuen Auslösers für benutzerdefinierte Ereignisse über die Data Factory-Benutzeroberfläche" lightbox="media/how-to-create-custom-event-trigger/custom-event-1-creation-expanded.png":::

1. Wählen Sie in der Dropdownliste „Azure-Abonnement“ das benutzerdefinierte Thema aus, oder geben Sie den Bereich für das Ereignisthema manuell ein.

   > [!NOTE]
   > Um einen neuen Auslöser für benutzerdefinierte Ereignisse zu erstellen oder einen vorhandenen zu ändern, muss das Azure-Konto, das für die Anmeldung bei Data Factory und die Veröffentlichung des Speicherereignisauslösers verwendet wird, über die entsprechende rollenbasierte Zugriffssteuerungsberechtigung (Azure RBAC) für das Thema verfügen. Zusätzliche Berechtigungen sind nicht erforderlich: Der Dienstprinzipal für Azure Data Factory benötigt _keine_ spezielle Berechtigung für Event Grid. Weitere Informationen zur Zugriffssteuerung finden Sie im Abschnitt [Rollenbasierte Zugriffssteuerung](#role-based-access-control).

1. Mit den Eigenschaften **Betreff beginnt mit** und **Betreff endet mit** können Sie Ereignisse filtern, für die die Pipeline ausgelöst werden soll. Beide Eigenschaften sind optional.

1. Verwenden Sie **+ Neu**, um **Ereignistypen** hinzuzufügen, nach denen Sie filtern möchten. Auslöser für benutzerdefinierte Ereignisse verwenden in der Liste eine ODER-Beziehung: Wenn ein benutzerdefiniertes Ereignis eine _eventType_-Eigenschaft aufweist, die mit einem hier aufgeführten Ereignistyp übereinstimmt, wird eine Pipelineausführung ausgelöst. Beim Ereignistyp wird die Groß-/Kleinschreibung nicht beachtet. Beispielsweise entspricht der Auslöser im folgenden Screenshot allen Ereignissen mit den Namen _copycompleted_ und _copysucceeded_, deren Betreff mit _factories_ beginnt.

    :::image type="content" source="media/how-to-create-custom-event-trigger/custom-event-2-properties.png" alt-text="Screenshot der Seite „Trigger bearbeiten“ zum Erläutern des Filterns nach Ereignistypen und Themen auf der Data Factory Benutzeroberfläche":::

1. Der Auslöser für benutzerdefinierte Ereignisse kann die benutzerdefinierte _data_-Nutzlast analysieren und an ihre Pipeline senden. Erstellen Sie zunächst die Pipelineparameter, und geben Sie die Werte auf der Seite **Parameter** ein. Verwenden Sie das Format **@triggerBody().event.data._keyName_** zum Analysieren der Datennutzlast, und übergeben Sie Werte an Pipelineparameter. Eine ausführliche Erläuterung finden Sie unter [Verweisen auf Triggermetadaten in Pipelineausführungen](how-to-use-trigger-parameterization.md) und [Systemvariablen in Auslösern für benutzerdefinierte Ereignisse](control-flow-system-variables.md#custom-event-trigger-scope).

    :::image type="content" source="media/how-to-create-custom-event-trigger/custom-event-4-trigger-values.png" alt-text="Screenshot der Einstellung „Pipelineparameter“":::

    :::image type="content" source="media/how-to-create-custom-event-trigger/custom-event-3-parameters.png" alt-text="Screenshot der Seite „Parameter“ zum Verweisen auf die Datennutzlast in einem benutzerdefinierten Ereignis":::

1. Zum Schluss klicken Sie auf **OK**.

## <a name="json-schema"></a>JSON-Schema

Die folgende Tabelle bietet eine Übersicht über die Schemaelemente im Zusammenhang mit Auslösern für benutzerdefinierte Ereignisse:

| **JSON-Element** | **Beschreibung** | **Typ** | **Zulässige Werte** | **Erforderlich** |
| ---------------- | --------------- | -------- | ------------------ | ------------ |
| **scope** | Die Azure Resource Manager-Ressourcen-ID des Event Grid-Themas. | String | Azure Resource Manager-ID | Ja |
| **events** | Der Ereignistyp, die diesen Trigger auslöst. | Array von Zeichenfolgen    |  | Ja, mindestens ein Wert wird erwartet. |
| **subjectBeginsWith** | Das Feld „Betreff“ muss mit dem angegebenen Muster beginnen, damit der Auslöser ausgelöst wird. Beispielsweise löst `factories` nur den Auslöser für ein Ereignis aus, dessen Betreff mit `factories` beginnt. | String   | | Nein |
| **subjectEndsWith** | Das Feld „Betreff“ muss mit dem angegebenen Muster enden, damit der Auslöser ausgelöst wird. | String   | | Nein |

## <a name="role-based-access-control"></a>Rollenbasierte Zugriffssteuerung

In Azure Data Factory wird die rollenbasierte Zugriffssteuerung von Azure (Azure RBAC) verwendet, um sicherzustellen, dass unbefugte Zugriffe zum Belauschen von, zum Abonnieren von Aktualisierungen von sowie zum Auslösen von Pipelines, die mit benutzerdefinierten Ereignissen verknüpft sind, strikt untersagt sind.

* Zum erfolgreichen Erstellen eines neuen oder Aktualisieren eines vorhandenen Auslösers für benutzerdefinierte Ereignisse muss das bei Data Factory angemeldete Azure-Konto über entsprechenden Zugriff auf das relevante Speicherkonto verfügen. Andernfalls schlägt der Vorgang mit der Meldung _Zugriff verweigert_ fehl.
* Data Factory benötigt keine spezielle Berechtigung für Ihr Event Grid, und Sie müssen dem Data Factory-Dienstprinzipal _keine_ spezielle Azure RBAC-Berechtigung für den Vorgang zuweisen.

Insbesondere benötigt der Kunde die Berechtigung _Microsoft.EventGrid/EventSubscriptions/Write_ für _/subscriptions/####/resourceGroups//####/providers/Microsoft.EventGrid/topics/someTopics_.

## <a name="next-steps"></a>Nächste Schritte

* Detaillierte Informationen zu Triggern finden Sie unter [Pipelineausführung und -trigger](concepts-pipeline-execution-triggers.md#trigger-execution).
* Informationen zum Verweisen auf Triggermetadaten in der Pipeline finden Sie unter [Verweisen auf Triggermetadaten in Pipelineausführungen](how-to-use-trigger-parameterization.md).
