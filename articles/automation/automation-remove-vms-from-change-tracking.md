---
title: Entfernen von VMs aus Änderungsnachverfolgung und Bestand in Azure Automation
description: In diesem Artikel erfahren Sie, wie Sie VMs aus Änderungsnachverfolgung und Bestand entfernen.
services: automation
ms.topic: conceptual
ms.date: 05/10/2018
ms.custom: mvc
ms.openlocfilehash: 8e9b80c60c098e4daf247db07fa48baa322aa7f0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "84169453"
---
# <a name="remove-vms-from-change-tracking-and-inventory"></a>Entfernen virtueller Computer aus Änderungsnachverfolgung und Bestand

Wenn Sie die Bereitstellung von Änderungen an VMs in Ihrer Umgebung abgeschlossen haben, können Sie die VMs aus dem Feature [Änderungsnachverfolgung und Bestand](change-tracking.md) entfernen.

## <a name="to-remove-your-vms"></a>So entfernen Sie Ihre VMs

1. Wählen Sie in Ihrem Automation-Konto unter **Konfigurationsverwaltung** die Option **Änderungsnachverfolgung** oder **Bestand** aus.

2. Verwenden Sie den folgenden Befehl, um die UUID einer VM zu ermitteln, die Sie aus der Verwaltung entfernen möchten.

    ```azurecli
    az vm show -g MyResourceGroup -n MyVm -d
    ```

3. Greifen Sie in Ihrem Log Analytics-Arbeitsbereich unter **Allgemein** auf die gespeicherten Suchvorgänge für die Bereichskonfiguration `MicrosoftDefaultScopeConfig-ChangeTracking` zu.

4. Klicken Sie für die gespeicherte Suche `MicrosoftDefaultComputerGroup` auf die Auslassungspunkte rechts neben dem Eintrag, und wählen Sie **Bearbeiten** aus. 

5. Entfernen Sie die UUID für die VM.

6. Wiederholen Sie die Schritte für jede weitere VM, die Sie entfernen möchten.

7. Speichern Sie die gespeicherte Suche, wenn Sie die Bearbeitung abgeschlossen haben. 

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Arbeiten mit Änderungsnachverfolgung und Bestand finden Sie unter [Verwalten von Änderungsnachverfolgung und Bestand](change-tracking-file-contents.md).
* Informationen zum Behandeln allgemeiner Featureprobleme finden Sie unter [Problembehandlung bei Änderungsnachverfolgung und Bestand](troubleshoot/change-tracking.md).