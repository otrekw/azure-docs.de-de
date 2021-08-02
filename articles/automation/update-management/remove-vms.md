---
title: Entfernen von VMs aus der Updateverwaltung in Azure Automation
description: In diesem Artikel erfahren Sie, wie Sie mit der Updateverwaltung verwaltete Computer entfernen.
services: automation
ms.topic: conceptual
ms.date: 06/03/2021
ms.custom: mvc
ms.openlocfilehash: 621367ba04893e7a8030b4a284125a6247c5d091
ms.sourcegitcommit: 67cdbe905eb67e969d7d0e211d87bc174b9b8dc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111854970"
---
# <a name="remove-vms-from-update-management"></a>Entfernen virtueller Computer aus der Updateverwaltung

Wenn Sie die Verwaltung von Updates auf den VMs in Ihrer Umgebung abgeschlossen haben, können Sie aufhören, die VMs mit der Funktion [Updateverwaltung](overview.md) zu verwalten. Um die Bearbeitung zu beenden, bearbeiten Sie die gespeicherte Suchabfrage `MicrosoftDefaultComputerGroup` in Ihrem Log Analytics-Arbeitsbereich, der mit Ihrem Automation-Konto verknüpft ist.

## <a name="sign-into-the-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="to-remove-your-vms"></a>So entfernen Sie Ihre VMs

1. Starten Sie **Cloud Shell** über den oberen Navigationsbereich im Azure-Portal. Wenn Sie mit Azure Cloud Shell nicht vertraut sind, finden Sie weitere Informationen unter [Übersicht über Azure Cloud Shell](../../cloud-shell/overview.md).

2. Ermitteln Sie mit dem folgenden Befehl die UUID eines Computers, den Sie aus der Verwaltung entfernen möchten.

    ```azurecli
    az vm show -g MyResourceGroup -n MyVm -d
    ```

3. Navigieren Sie im Azure-Portal zu **Log Analytics-Arbeitsbereiche**. Wählen Sie Ihren Arbeitsbereich in der Liste aus.

4. Wählen Sie in Ihrem Log Analytics-Arbeitsbereich im linken Menü **Computergruppen** aus.

5. Im rechten Bereich unter **Computergruppen** wird standardmäßig die Registerkarte **Gespeicherte Gruppen** angezeigt.

6. Klicken Sie in der Tabelle auf das Symbol **Abfrage ausführen** rechts neben dem Element **MicrosoftDefaultComputerGroup** mit dem **Legacykategorie**-Wert **Updates**.

7. Überprüfen Sie die Abfrage im Abfrage-Editor, und suchen Sie die UUID für den virtuellen Computer. Entfernen Sie die UUID für den virtuellen Computer, und wiederholen Sie die Schritte für alle anderen VMs, die Sie entfernen möchten.

   > [!NOTE]
   > Erstellen Sie als zusätzlichen Schutz unbedingt eine Kopie der Abfrage, bevor Sie Bearbeitungen vornehmen. Auf diese Weise können Sie eine Wiederherstellung vornehmen, falls ein Problem auftritt.

   Wenn Sie mit der ursprünglichen Abfrage beginnen und Computer zur Unterstützung einer Bereinigungs- oder Wartungsaktivität erneut hinzufügen möchten, kopieren Sie die folgende Abfrage:

   ```kusto
   Heartbeat
   | where Computer in~ ("") or VMUUID in~ ("")
   | distinct Computer
   ```

8. Speichern Sie die gespeicherte Suche, wenn Sie die Bearbeitung abgeschlossen haben, indem Sie in der oberen Leiste **Speichern > Als Funktion speichern** auswählen. Geben Sie bei Aufforderung Folgendes an:

    * **Name**: Updates__MicrosoftDefaultComputerGroup
    * **Als Computergruppe speichern** ist ausgewählt
    * **Legacykategorie**: Updates

>[!NOTE]
>Computer werden weiterhin angezeigt, nachdem Sie die Registrierung aufgehoben haben, da wir alle in den letzten 24 Stunden bewerteten Computer melden. Nachdem Sie den Computer entfernt haben, müssen Sie 24 Stunden warten, bis er nicht mehr aufgelistet wird.

## <a name="next-steps"></a>Nächste Schritte

Informationen zur erneuten Aktivierung der Verwaltung Ihres virtuellen Computers finden Sie unter [Aktivieren der Updateverwaltung mittels Durchsuchen im Azure-Portal](enable-from-portal.md) oder [Aktivieren der Updateverwaltung über einen virtuellen Azure-Computer](enable-from-vm.md).