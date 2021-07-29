---
title: Verschieben eines Azure Batch-Kontos in eine andere Region
description: Erfahren Sie, wie Sie ein Azure Batch-Konto in eine andere Region verschieben.
ms.topic: how-to
ms.date: 05/05/2021
ms.custom: subject-moving-resources
ms.openlocfilehash: 9c0e45b62f6dd6152ab80beaa751925702b1fa37
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108776736"
---
# <a name="move-an-azure-batch-account-to-another-region"></a>Verschieben eines Azure Batch-Kontos in eine andere Region

Es gibt Szenarien, in denen es hilfreich sein kann, ein vorhandenes [Batch-Konto](accounts.md) von einer Region in eine andere zu verschieben. Möglicherweise möchten Sie im Rahmen der Planung einer Notfallwiederherstellung eine Verschiebung in eine andere Region durchführen.

Azure Batch-Konten können nicht direkt von einer Region in eine andere verschoben werden. Sie können jedoch eine Azure Resource Manager-Vorlage verwenden, um die vorhandene Konfiguration Ihres Batch-Kontos zu exportieren. Anschließend können Sie die Ressource in einer anderen Region stagen, indem Sie das Batch-Konto in eine Vorlage exportieren, die Parameter so ändern, dass sie der Zielregion entsprechen, und die Vorlage dann in der neuen Region bereitstellen. Anschließend können Sie Aufträge und andere Features im Konto neu erstellen.

 Weitere Informationen zu Resource Manager und Vorlagen finden Sie unter [Schnellstart: Erstellen und Bereitstellen von Azure Resource Manager-Vorlagen über das Azure-Portal](../azure-resource-manager/templates/quickstart-create-templates-use-the-portal.md).

In diesem Thema wird erläutert, wie Sie ein Batch-Konto mithilfe des Azure-Portals zwischen Regionen verschieben.

## <a name="prerequisites"></a>Voraussetzungen

- Verschieben Sie das Ihrem Batch-Konto zugeordnete Speicherkonto in die neue Zielregion, indem Sie die Schritte unter [Verschieben eines Azure Storage-Kontos in eine andere Region](../storage/common/storage-account-move.md) ausführen. Wenn Sie dies vorziehen, können Sie das Speicherkonto in der ursprünglichen Region belassen. Eine Verschiebung wird jedoch empfohlen, weil im Allgemeinen eine bessere Leistung erzielt werden kann, wenn sich das Speicherkonto in derselben Region befindet wie Ihr Batch-Konto. In den folgenden Anweisungen wird davon ausgegangen, dass Sie Ihr Speicherkonto bereits migriert haben.
- Stellen Sie sicher, dass die von Ihrem Konto verwendeten Dienste und Features in der Zielregion unterstützt werden.

## <a name="prepare"></a>Vorbereiten

Exportieren Sie zunächst eine Resource Manager-Vorlage, und ändern Sie sie anschließend.

### <a name="export-a-template"></a>Exportieren einer Vorlage

Exportieren Sie zunächst eine Vorlage, die Einstellungen und Informationen für Ihr Batch-Konto enthält.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Wählen Sie die Option **Alle Ressourcen** und dann Ihr Batch-Konto aus.

3. Wählen Si **Automation** > **Vorlage exportieren** aus.

4. Wählen Sie **Herunterladen** auf dem Blatt **Vorlage exportieren** aus.

5. Suchen Sie die aus dem Portal heruntergeladene ZIP-Datei, und entpacken Sie sie in einen Ordner Ihrer Wahl.

   Diese ZIP-Datei enthält die JSON-Dateien, in denen die Vorlage und die Skripts zum Bereitstellen der Vorlage enthalten sind.

### <a name="modify-the-template"></a>Ändern der Vorlage

Im nächsten Schritt laden und ändern Sie die Vorlage, damit Sie ein neues Batch-Konto in der Zielregion erstellen können.

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**.

1. Geben Sie in **Marketplace durchsuchen** den Begriff **Vorlagenbereitstellung** ein, und drücken Sie dann die **EINGABETASTE**.

1. Wählen Sie **Vorlagenbereitstellung (mit benutzerdefinierten Vorlagen bereitstellen)** aus.

1. Klicken Sie auf **Erstellen**.

1. Wählen Sie **Eigene Vorlage im Editor erstellen**.

1. Wählen Sie **Datei laden** aus, und befolgen Sie die Anweisungen zum Laden der Datei **template.json**, die Sie im vorherigen Abschnitt heruntergeladen haben.

1. Benennen Sie das Batch-Zielkonto in der hochgeladenen Datei **template.json**, indem Sie einen neuen **defaultValue** als Batch-Kontonamen eingeben. In diesem Beispiel wird der **defaultValue** für den Batch-Kontonamen auf `mytargetaccount` festgelegt und die Zeichenfolge in **defaultValue** durch die Ressourcen-ID für `mytargetstorageaccount` ersetzt.

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "batchAccounts_mysourceaccount_name": {
                "defaultValue": "mytargetaccount",
                "type": "String"
            }
        },
   ```

1. Als Nächstes aktualisieren Sie den **defaultValue** des Speicherkontos mit der Ressourcen-ID Ihres migrierten Speicherkontos. Navigieren Sie zum Abrufen dieser Werte zum Speicherkonto im Azure-Portal, wählen Sie oben auf dem Bildschirm **JSON-Ansicht** aus, und kopieren Sie dann den unter **Ressourcen-ID** angezeigten Wert. Im folgenden Beispiel wird die Ressourcen-ID für ein Speicherkonto namens `mytargetstorageaccount` in der Ressourcengruppe `mytargetresourcegroup` zurückgegeben.

   ```json
           "storageAccounts_mysourcestorageaccount_externalid": {
            "defaultValue": "/subscriptions/{subscriptionID}/resourceGroups/mytargetresourcegroup/providers/Microsoft.Storage/storageAccounts/mytargetstorageaccount",
            "type": "String"
        }
    },
   ```

1. Abschließend bearbeiten Sie die Eigenschaft **location**, um Ihre Zielregion zu verwenden. Dieses Beispiel legt den Zielbereich auf `centralus` fest.

```json
    {
        "resources": [
            {
                "type": "Microsoft.Batch/batchAccounts",
                "apiVersion": "2021-01-01",
                "name": "[parameters('batchAccounts_mysourceaccount_name')]",
                "location": "centralus",  
```

Informationen zum Abrufen von Regionsstandortcodes finden Sie unter [Azure-Standorte](https://azure.microsoft.com/global-infrastructure/locations/).  Der Code für eine Region ist der Regionsname ohne Leerzeichen. Beispiel: **USA, Mitte** = **centralus**

## <a name="move"></a>Move

Stellen Sie die Vorlage bereit, um ein neues Batch-Konto in der Zielregion zu erstellen.

1. Nachdem Sie ihre Änderungen vorgenommen haben, wählen Sie unterhalb der Datei **template.json** den Befehl **Speichern** aus.

1. Geben Sie die Eigenschaftswerte ein oder wählen Sie sie aus:
   - **Abonnement**: Wählen Sie ein Azure-Abonnement aus.
   - **Ressourcengruppe**: Wählen Sie die Ressourcengruppe aus, die Sie beim Verschieben des zugeordneten Speicherkontos erstellt haben.
   - **Region**: Wählen Sie die Azure-Region aus, in die Sie das Konto verschieben.

1. Wählen Sie **Überprüfen und erstellen** und dann **Erstellen** aus.

### <a name="configure-the-new-batch-account"></a>Konfigurieren des neuen Batch-Kontos

Einige Features werden nicht in eine Vorlage exportiert, daher müssen Sie sie im neuen Batch-Konto neu erstellen. Dabei handelt es sich z. B. um:

- Aufträge
- Auftragszeitpläne
- Zertifikate
- Anwendungspakete

Achten Sie darauf, diese im neuen Konto nach Bedarf zu konfigurieren. Sie können sich als Referenz ansehen, wie Sie diese Features in Ihrem Batch-Quellkonto konfiguriert haben.

## <a name="discard-or-clean-up"></a>Verwerfen oder Bereinigen

Nachdem Sie bestätigt haben, dass Ihr neues Batch-Konto in der neuen Region ordnungsgemäß funktioniert und Sie die erforderlichen Features wiederhergestellt haben, können Sie das Batch-Quellkonto löschen.

So entfernen Sie ein Batch-Konto über das Azure-Portal

1. Erweitern Sie im Azure-Portal das Menü auf der linken Seite, um das Menü mit den Diensten zu öffnen. Wählen Sie dann **Batch-Konten** aus.

2. Suchen Sie nach dem zu löschenden Batch-Konto, und klicken Sie mit der rechten Maustaste rechts neben der Auflistung auf die Schaltfläche **Mehr** ( **...** ). Stellen Sie sicher, dass dies das ursprüngliche Batch-Quellkonto ist, nicht das neue, das Sie erstellt haben.

3. Wählen Sie **Löschen** aus, und bestätigen Sie den Vorgang.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../azure-resource-manager/management/move-resource-group-and-subscription.md)
- Weitere Informationen zum [Verschieben von Azure-VMs in eine andere Region](../site-recovery/azure-to-azure-tutorial-migrate.md)
