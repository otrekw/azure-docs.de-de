---
title: Erstellen einer Azure Event Hubs-Schemaregistrierung
description: In diesem Artikel erfahren Sie, wie Sie eine Schemaregistrierung in einem Azure Event Hubs-Namespace erstellen.
ms.topic: how-to
ms.date: 09/22/2020
ms.custom: references_regions
ms.openlocfilehash: 90556e0843cda94dc79330321d027f8d28eb7d20
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91652192"
---
# <a name="create-an-azure-event-hubs-schema-registry-preview"></a>Erstellen einer Azure Event Hubs-Schemaregistrierung (Vorschau)
In diesem Artikel erfahren Sie, wie Sie eine Schemagruppe mit Schemas in einer Schemaregistrierung erstellen, die von Azure Event Hubs gehostet wird. Eine Übersicht über das Schemaregistrierung-Feature von Azure Event Hubs finden Sie unter [Azure-Schemaregistrierung in Event Hubs (Vorschau)](schema-registry-overview.md).

> [!NOTE]
> - Die Funktion **Schemaregistrierung** ist derzeit als **Vorschau** verfügbar und nicht für Produktionsworkloads vorgesehen.
> - Das Feature ist nur in den Tarifen **Standard** und **Dediziert** verfügbar, nicht im Tarif **Basic**.

## <a name="prerequisites"></a>Voraussetzungen
[Erstellen eines Event Hubs-Namespace](event-hubs-create.md#create-an-event-hubs-namespace). Sie können auch einen vorhandenen Namespace verwenden. 

## <a name="create-a-schema-group"></a>Erstellen einer Schemagruppe
1. Navigieren Sie zur Seite **Event Hubs-Namespace**. 
1. Wählen Sie im Menü auf der linken Seite **Schemaregistrierung** aus. Um eine Schemagruppe zu erstellen, wählen Sie auf der Symbolleiste **+ Schemagruppe** aus. 

    :::image type="content" source="./media/create-schema-registry/namespace-page.png" alt-text="Seite „Schemaregistrierung“":::
1. Führen Sie auf der Seite **Schemagruppe erstellen** die folgenden Schritte aus:
    1. Geben Sie einen **Namen** für die Schemagruppe ein.
    1. Wählen Sie für **Serialisierungstyp** ein Serialisierungsformat aus, das für alle Schemas in der Schemagruppe gilt. Da **Avro** derzeit der einzige unterstützte Typ ist, wählen Sie **Avro** aus. 
    1. Wählen Sie einen **Kompatibilitätsmodus** für alle Schemas in der Gruppe aus. Für **Avro** werden Aufwärts- und Abwärtskompatibilitätsmodi unterstützt. 
    1. Wählen Sie dann **Erstellen** aus, um die Schemagruppe zu erstellen. 
1. Wählen Sie in der Liste der Schemagruppen den Namen der **Schemagruppe** aus.

    :::image type="content" source="./media/create-schema-registry/select-schema-group.png" alt-text="Seite „Schemaregistrierung“":::    
1. Die Seite **Schemagruppe** wird für die Gruppe angezeigt.

    :::image type="content" source="./media/create-schema-registry/schema-group-page.png" alt-text="Seite „Schemaregistrierung“":::
    

## <a name="add-a-schema-to-the-schema-group"></a>Hinzufügen eines Schemas zur Schemagruppe
In diesem Abschnitt fügen Sie der Schemagruppe mithilfe des Azure-Portals ein Schema hinzu. 

1. Wählen Sie auf der Seite **Schemagruppe** auf der Symbolleiste **+ Schema** aus. 
1. Führen Sie auf der Seite **Schema erstellen** die folgenden Schritte aus:
    1. Geben Sie einen **Namen** für das Schema ein.
    1. Geben Sie das folgende **Schema** in das Textfeld ein. Sie können auch eine Datei mit dem Schema auswählen.
    
        ```json
        {
            "type": "record",
            "name": "AvroUser",
            "namespace": "com.azure.schemaregistry.samples",
            "fields": [
                {
                    "name": "name",
                    "type": "string"
                },
                {
                    "name": "favoriteNumber",
                    "type": "int"
                }
            ]
        }
        ```
    1. Klicken Sie auf **Erstellen**. 
1. Wählen Sie das **Schema** in der Liste der Schemas aus. 

    :::image type="content" source="./media/create-schema-registry/select-schema.png" alt-text="Seite „Schemaregistrierung“":::
1. Die Seite **Übersicht über Schemas** für das Schema wird angezeigt. 

    :::image type="content" source="./media/create-schema-registry/schema-overview-page.png" alt-text="Seite „Schemaregistrierung“":::    
1. Wenn mehrere Versionen eines Schemas vorhanden sind, werden Sie in der Dropdownliste **Versionen** angezeigt. Wählen Sie eine Version aus, um zu diesem Versionsschema zu wechseln. 

## <a name="create-a-new-version-of-schema"></a>Erstellen einer neuen Version des Schemas

1. Aktualisieren Sie das Schema im Textfeld, und wählen Sie **Überprüfen** aus. Im folgenden Beispiel wurde dem Schema ein neues Feld `id` hinzugefügt. 

    :::image type="content" source="./media/create-schema-registry/update-schema.png" alt-text="Seite „Schemaregistrierung“":::    
    
1. Überprüfen Sie den Überprüfungsstatus und Änderungen, und wählen Sie **Speichern** aus. 

    :::image type="content" source="./media/create-schema-registry/compare-save-schema.png" alt-text="Seite „Schemaregistrierung“":::     
1. Sie sehen, dass auf der Seite **Übersicht über Schemas** für die **Version** `2` ausgewählt ist. 

    :::image type="content" source="./media/create-schema-registry/new-version.png" alt-text="Seite „Schemaregistrierung“":::    
1. Wählen Sie `1` aus, um die Version 1 des Schemas anzuzeigen. 

    :::image type="content" source="./media/create-schema-registry/select-version.png" alt-text="Seite „Schemaregistrierung“":::    


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Schemaregistrierung finden Sie unter [Azure-Schemaregistrierung in Event Hubs (Vorschau)](schema-registry-overview.md).

