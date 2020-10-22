---
title: Entfernen von VMs aus der Updateverwaltung in Azure Automation
description: In diesem Artikel erfahren Sie, wie Sie mit der Updateverwaltung verwaltete Computer entfernen.
services: automation
ms.topic: conceptual
ms.date: 09/09/2020
ms.custom: mvc
ms.openlocfilehash: 774dbe29cbb6b4d063d3619d0c710efb1949b99a
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2020
ms.locfileid: "92221626"
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

4. Wählen Sie in Ihrem Log Analytics-Arbeitsbereich im oberen Aktionsmenü **Protokolle** und dann **Abfrage-Explorer** aus.

5. Erweitern Sie unter **Abfrage-Explorer** im rechten Bereich **Gespeicherte Abfragen > Updates**, und wählen Sie die gespeicherte Suchabfrage `MicrosoftDefaultComputerGroup`, um sie zu bearbeiten.

6. Überprüfen Sie die Abfrage im Abfrage-Editor, und suchen Sie die UUID für den virtuellen Computer. Entfernen Sie die UUID für den virtuellen Computer, und wiederholen Sie die Schritte für alle anderen VMs, die Sie entfernen möchten.

7. Speichern Sie die gespeicherte Suche, wenn Sie die Bearbeitung abgeschlossen haben, indem Sie auf der oberen Leiste **Speichern** auswählen.

>[!NOTE]
>Computer werden weiterhin angezeigt, nachdem Sie die Registrierung aufgehoben haben, da wir alle in den letzten 24 Stunden bewerteten Computer melden. Nachdem Sie den Computer entfernt haben, müssen Sie 24 Stunden warten, bis er nicht mehr aufgelistet wird.

## <a name="next-steps"></a>Nächste Schritte

Informationen zur erneuten Aktivierung der Verwaltung Ihres virtuellen Computers finden Sie unter [Aktivieren der Updateverwaltung mittels Durchsuchen im Azure-Portal](enable-from-portal.md) oder [Aktivieren der Updateverwaltung über einen virtuellen Azure-Computer](enable-from-vm.md).