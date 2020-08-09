---
title: Entfernen von VMs aus der Updateverwaltung in Azure Automation
description: In diesem Artikel erfahren Sie, wie Sie mit der Updateverwaltung verwaltete Computer entfernen.
services: automation
ms.topic: conceptual
ms.date: 07/28/2020
ms.custom: mvc
ms.openlocfilehash: d7f7e4aa8b2c192688020b4449c8750f94af29f6
ms.sourcegitcommit: cee72954f4467096b01ba287d30074751bcb7ff4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2020
ms.locfileid: "87449581"
---
# <a name="remove-vms-from-update-management"></a>Entfernen virtueller Computer aus der Updateverwaltung

Wenn Sie die Verwaltung von Updates auf den VMs in Ihrer Umgebung abgeschlossen haben, können Sie aufhören, die VMs mit der Funktion [Updateverwaltung](update-mgmt-overview.md) zu verwalten.

## <a name="sign-into-the-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="to-remove-your-vms"></a>So entfernen Sie Ihre VMs

1. Wählen Sie in Ihrem Automation-Konto unter **Updateverwaltung** die Option **Updateverwaltung** aus.

2. Ermitteln Sie mit dem folgenden Befehl die UUID eines Computers, den Sie aus der Verwaltung entfernen möchten.

    ```azurecli
    az vm show -g MyResourceGroup -n MyVm -d
    ```

3. Greifen Sie in Ihrem Log Analytics-Arbeitsbereich unter **Allgemein** auf die gespeicherten Suchvorgänge für die Bereichskonfiguration `MicrosoftDefaultScopeConfig-Updates` zu.

4. Klicken Sie für die gespeicherte Suche `MicrosoftDefaultComputerGroup` auf die Auslassungspunkte rechts neben dem Eintrag, und wählen Sie **Bearbeiten** aus.

5. Entfernen Sie die UUID für die VM.

6. Wiederholen Sie die Schritte für jede weitere VM, die Sie entfernen möchten.

7. Speichern Sie die gespeicherte Suche, wenn Sie die Bearbeitung abgeschlossen haben.

>[!NOTE]
>Computer werden weiterhin angezeigt, nachdem Sie die Registrierung aufgehoben haben, da wir alle in den letzten 24 Stunden bewerteten Computer melden. Nachdem Sie den Computer getrennt haben, müssen Sie 24 Stunden warten, bis er nicht mehr aufgelistet wird.

## <a name="next-steps"></a>Nächste Schritte

Informationen zur erneuten Aktivierung der Verwaltung Ihres virtuellen Computers finden Sie unter [Aktivieren der Updateverwaltung mittels Durchsuchen im Azure-Portal](update-mgmt-enable-portal.md) oder [Aktivieren der Updateverwaltung über einen virtuellen Azure-Computer](update-mgmt-enable-vm.md).