---
title: Aktivieren und Verwalten des vorläufigen Löschens für Container (Vorschau)
titleSuffix: Azure Storage
description: Aktivieren Sie das vorläufige Löschen für Container (Vorschau), sodass Sie Ihre Daten leichter wiederherstellen können, wenn diese irrtümlich geändert oder gelöscht wurden.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 03/05/2021
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 2097c1743e07b5563bc75d3d1cce48aa11b98e5f
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102216342"
---
# <a name="enable-and-manage-soft-delete-for-containers-preview"></a>Aktivieren und Verwalten des vorläufigen Löschens für Container (Vorschau)

Das Feature des vorläufigen Löschens von Containern (Vorschau) schützt Ihre Daten vor versehentlichen oder irrtümlichen Lösch- oder Änderungsvorgängen. Wenn das vorläufige Löschen für Container in einem Speicherkonto aktiviert ist, kann ein Container und dessen Inhalt nach dem Löschen innerhalb eines von Ihnen angegebenen Aufbewahrungszeitraums wiederhergestellt werden.

Wenn die Möglichkeit besteht, dass Ihre Daten von einer Anwendung oder einem anderen Benutzer des Speicherkontos versehentlich geändert oder gelöscht werden, empfiehlt Microsoft, das vorläufige Löschen für Container zu aktivieren. In diesem Artikel wird gezeigt, wie Sie das vorläufige Löschen für Container aktivieren. Weitere Informationen zum Feature des vorläufigen Löschens für Container sowie zur Registrierung für die Vorschau finden Sie unter [Vorläufiges Löschen für Container (Vorschau)](soft-delete-container-overview.md).

Um einen vollumfänglichen Datenschutz zu erzielen, empfiehlt Microsoft auch die Aktivierung des vorläufigen Löschens für Blobs sowie die Blobversionsverwaltung. Informationen zum Aktivieren des vorläufigen Löschens für Blobs finden Sie unter [Aktivieren und Verwalten des vorläufigen Löschens für Blobs](soft-delete-blob-enable.md). Informationen zum Aktivieren der Blobversionsverwaltung finden Sie unter [Blobversionsverwaltung](versioning-overview.md).

> [!IMPORTANT]
>
> Das vorläufige Löschen von Containern befindet sich zurzeit in der **VORSCHAU**. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten rechtliche Bedingungen. Sie gelten für diejenigen Azure-Features, die sich in der Beta- oder Vorschauversion befinden oder aber anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.

## <a name="enable-container-soft-delete"></a>Aktivieren des vorläufigen Löschens von Containern

Sie können das vorläufige Löschen von Containern für das Speicherkonto jederzeit im Azure-Portal oder mit einer Azure Resource Manager-Vorlage aktivieren oder deaktivieren.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Um das vorläufige Löschen von Containern für Ihr Speicherkonto im Azure-Portal zu aktivieren, führen Sie die folgenden Schritte aus:

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) zu Ihrem Speicherkonto.
1. Navigieren Sie unter **Blob-Dienst** zu den Einstellungen für den **Datenschutz**.
1. Legen Sie die Eigenschaft **Vorläufiges Löschen von Containern** auf *Aktiviert* fest.
1. Geben Sie unter **Aufbewahrungsrichtlinien** an, wie lange vorläufig gelöschte Container von Azure Storage aufbewahrt werden sollen.
1. Speichern Sie die Änderungen.

:::image type="content" source="media/soft-delete-container-enable/soft-delete-container-portal-configure.png" alt-text="Screenshot: Aktivieren des vorläufigen Löschens von Containern im Azure-Portal":::

# <a name="template"></a>[Vorlage](#tab/template)

Wenn Sie das vorläufige Löschen von Containern mithilfe einer Azure Resource Manager-Vorlage aktivieren möchten, erstellen Sie eine Vorlage, mit der die Eigenschaft **containerDeleteRetentionPolicy** festgelegt wird. Die folgenden Schritte beschreiben, wie eine Vorlage im Azure-Portal erstellt wird.

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**.
1. Geben Sie in **Marketplace durchsuchen** den Begriff **Vorlagenbereitstellung** ein, und drücken Sie dann die **EINGABETASTE**.
1. Wählen Sie **Vorlagenbereitstellung** aus, klicken Sie auf **Erstellen**, und wählen Sie dann **Eigene Vorlage im Editor erstellen** aus.
1. Fügen Sie folgenden JSON-Code im Vorlagen-Editor ein. Ersetzen Sie den Platzhalter `<account-name>` durch den Namen Ihres Speicherkontos.

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {},
      "variables": {},
      "resources": [
          {
              "type": "Microsoft.Storage/storageAccounts/blobServices",
              "apiVersion": "2019-06-01",
              "name": "<account-name>/default",
              "properties": {
                  "containerDeleteRetentionPolicy": {
                      "enabled": true,
                      "days": 7
                  }
              }
          }
      ]
    }
    ```

---

1. Geben Sie den Aufbewahrungszeitraum an. Der Standardwert ist 7.
1. Speichern Sie die Vorlage.
1. Geben Sie die Ressourcengruppe des Kontos an, und wählen Sie die Schaltfläche **Überprüfen und erstellen** aus, um die Vorlage bereitzustellen und das vorläufige Löschen für Container zu aktivieren.

## <a name="view-soft-deleted-containers"></a>Anzeigen von vorläufig gelöschten Containern

Wenn das vorläufige Löschen aktiviert ist, können Sie vorläufig gelöschte Container im Azure-Portal anzeigen. Vorläufig gelöschte Container sind während des angegebenen Aufbewahrungszeitraums sichtbar. Nach Ablauf des Aufbewahrungszeitraums wird ein vorläufig gelöschter Container endgültig gelöscht und ist nicht mehr sichtbar.

Um vorläufig gelöschte Container im Azure-Portal anzuzeigen, führen Sie die folgenden Schritte aus:

1. Navigieren Sie im Azure-Portal zu Ihrem Speicherkonto, und zeigen Sie die Liste Ihrer Container an.
1. Legen Sie den Schalter „Gelöschte Container anzeigen“ so fest, dass gelöschte Container in der Liste angezeigt werden.

    :::image type="content" source="media/soft-delete-container-enable/soft-delete-container-portal-list.png" alt-text="Screenshot: Anzeigen von vorläufig gelöschten Containern im Azure-Portal":::

## <a name="restore-a-soft-deleted-container"></a>Wiederherstellen eines vorläufig gelöschten Containers

Sie können einen vorläufig gelöschten Container und dessen Inhalte innerhalb des Aufbewahrungszeitraums wiederherstellen. Um einen vorläufig gelöschten Container im Azure-Portal wiederherzustellen, führen Sie die folgenden Schritte aus:

1. Navigieren Sie im Azure-Portal zu Ihrem Speicherkonto, und zeigen Sie die Liste Ihrer Container an.
1. Öffnen Sie das Kontextmenü für den Container, den Sie wiederherstellen möchten, und wählen Sie die Option **Wiederherstellen** aus.

    :::image type="content" source="media/soft-delete-container-enable/soft-delete-container-portal-restore.png" alt-text="Screenshot: Wiederherstellen eines vorläufig gelöschten Containers im Azure-Portal":::

## <a name="next-steps"></a>Nächste Schritte

- [Vorläufiges Löschen für Container (Vorschau)](soft-delete-container-overview.md)
- [Vorläufiges Löschen für Blobs](soft-delete-blob-overview.md)
- [Blobversionsverwaltung](versioning-overview.md)
