---
title: Entfernen von VMs aus der Updateverwaltung in Azure Automation
description: In diesem Artikel erfahren Sie, wie Sie VMs aus der Updateverwaltung entfernen.
services: automation
ms.topic: conceptual
ms.date: 05/10/2018
ms.custom: mvc
ms.openlocfilehash: 26a38c0851643fbd6446acddd99a05abf8d5b26c
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83836650"
---
# <a name="remove-vms-from-update-management"></a>Entfernen von VMs aus der Updateverwaltung

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

Wenn Sie die Bereitstellung von Änderungen an VMs in Ihrer Umgebung abgeschlossen haben, können Sie die VMs aus dem Feature [Updateverwaltung](automation-update-management.md) entfernen.

1. Wählen Sie in Ihrem Automation-Konto unter **Updateverwaltung** die Option **Updateverwaltung** aus.

2. Verwenden Sie den folgenden Befehl, um die UUID einer VM zu ermitteln, die Sie aus der Verwaltung entfernen möchten.

    ```azurecli
    az vm show -g MyResourceGroup -n MyVm -d
    ```

3. Greifen Sie in Ihrem Log Analytics-Arbeitsbereich unter **Allgemein** auf die gespeicherten Suchvorgänge zu.

4. Klicken Sie für die gespeicherte Suche `MicrosoftDefaultComputerGroup` auf die Auslassungspunkte rechts neben dem Eintrag, und wählen Sie **Bearbeiten** aus. 

5. Entfernen Sie die UUID für die VM.

6. Wiederholen Sie die Schritte für jede weitere VM, die Sie entfernen möchten.

7. Speichern Sie die gespeicherte Suche, wenn Sie die Bearbeitung abgeschlossen haben. 

## <a name="next-steps"></a>Nächste Schritte

* Informationen zur Arbeit mit der Updateverwaltung finden Sie unter [Verwalten von Updates und Patches für Ihre Azure-VMs](automation-tutorial-update-management.md).
* Informationen zum Behandeln allgemeiner Featureprobleme finden Sie unter [Beheben von Problemen mit der Updateverwaltung](troubleshoot/update-management.md).
* Informationen zum Behandeln von Problemen mit dem Windows Update-Agent finden Sie unter [Beheben von Problemen mit dem Windows Update-Agent](troubleshoot/update-agent-issues.md).
* Informationen zum Behandeln von Problemen mit dem Linux Update-Agent finden Sie unter [Beheben von Problemen mit dem Linux Update-Agent](troubleshoot/update-agent-issues-linux.md).