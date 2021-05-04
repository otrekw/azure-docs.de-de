---
title: Entfernen von DSC und eines Knotens aus Automation State Configuration
description: In diesem Artikel wird erläutert, wie Sie ein zugewiesenes Azure Automation State Configuration-Konfigurationsdokument (DSC) entfernen und die Registrierung eines verwalteten Knotens aufheben.
titleSuffix: Azure Automation
services: automation
ms.subservice: dsc
ms.date: 04/16/2021
ms.topic: how-to
ms.openlocfilehash: 341a29d3aa8376a6308582bba026fab8fcadca35
ms.sourcegitcommit: 2f322df43fb3854d07a69bcdf56c6b1f7e6f3333
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2021
ms.locfileid: "108021129"
---
# <a name="how-to-remove-a-configuration-and-node-from-automation-state-configuration"></a>Entfernen einer Konfiguration und eines Knotens aus Automation State Configuration

In diesem Artikel wird beschrieben, wie Sie die Registrierung eines von Automation State Configuration verwalteten Knotens aufheben und eine PowerShell DSC-Konfiguration (Desired State Configuration) sicher von verwalteten Knoten entfernen. Für Windows- wie auch für Linux-Knoten müssen Sie [die Registrierung des Knotens aufheben](#unregister-a-node) und [die Konfiguration löschen](#delete-a-configuration-from-the-azure-portal). Nur für Linux-Knoten können Sie optional auch die DSC-Pakete von den Knoten löschen. Weitere Informationen finden Sie unter [Entfernen des DSC-Pakets von einem Linux-Knoten](#remove-the-dsc-package-from-a-linux-node).

## <a name="unregister-a-node"></a>Aufheben der Registrierung eines Knotens

Wenn ein Knoten nicht mehr von State Configuration (DSC) verwaltet werden soll, können Sie seine Registrierung im Azure-Portal oder mit Azure PowerShell mithilfe der folgenden Schritte aufheben.

Durch das Aufheben der Registrierung eines Knotens beim Dienst werden die Einstellungen des lokalen Konfigurations-Managers lediglich so festgelegt, dass der Knoten keine Verbindung mehr mit dem Dienst herstellt. Dies wirkt sich nicht auf die Konfiguration aus, die derzeit auf den Knoten angewendet wird, und belässt die zugehörigen Dateien auf dem Knoten. Sie können diese Dateien optional bereinigen. Weitere Informationen finden Sie unter [Löschen einer Konfiguration](#delete-a-configuration).

### <a name="unregister-in-the-azure-portal"></a>Aufheben der Registrierung im Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie nach **Automation-Konten**, und wählen Sie diese Option aus.
1. Wählen Sie auf der Seite **Automation-Konten** in der entsprechenden Liste Ihr Automation-Konto aus.
1. Wählen Sie in Ihrem Automation-Konto unter **Konfigurationsverwaltung** die Option **Zustandskonfiguration (DSC)** aus.
1. Klicken Sie auf der Seite **Zustandskonfiguration (DSC)** auf die Registerkarte **Knoten**.
1. Wählen Sie auf der Registerkarte **Knoten** den Namen des Knotens aus, dessen Registrierung Sie aufheben möchten.
1. Klicken Sie im Bereich dieses Knotens auf **Registrierung aufheben**.

   :::image type="content" source="./media/remove-desired-state-configuration-package/unregister-node.png" alt-text="Screenshot der Seite „Knotendetails“ mit hervorgehobener Schaltfläche „Registrierung aufheben“." lightbox="./media/remove-desired-state-configuration-package/unregister-node.png":::

### <a name="unregister-using-powershell"></a>Aufheben der Registrierung mit PowerShell

Sie können die Registrierung eines Knotens auch mithilfe des PowerShell-Cmdlets [Unregister-AzAutomationDscNode](/powershell/module/az.automation/unregister-azautomationdscnode) aufheben.

>[!NOTE]
>Wenn Ihre Organisation noch die veralteten AzureRM-Module verwendet, können Sie [Unregister-AzureRmAutomationDscNode](/powershell/module/azurerm.automation/unregister-azurermautomationdscnode) verwenden.

## <a name="delete-a-configuration"></a>Löschen einer Konfiguration

Wenn Sie bereit sind, ein importiertes DSC-Konfigurationsdokument (wobei es sich um eine MOF-Datei (Managed Object Format) handelt) zu entfernen, das einem oder mehrere Knoten zugewiesen ist, führen Sie die folgenden Schritte aus.

### <a name="delete-a-configuration-from-the-azure-portal"></a>Löschen einer Konfiguration über das Azure-Portal

Im Azure-Portal können Sie Konfigurationen sowohl für Windows- als auch für Linux-Knoten löschen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie nach **Automation-Konten**, und wählen Sie diese Option aus.
1. Wählen Sie auf der Seite **Automation-Konten** in der entsprechenden Liste Ihr Automation-Konto aus.
1. Wählen Sie in Ihrem Automation-Konto unter **Konfigurationsverwaltung** die Option **Zustandskonfiguration (DSC)** aus.
1. Klicken Sie auf der Seite **Zustandskonfiguration (DSC)** auf die Registerkarte **Konfigurationen,** , und wählen Sie dann den Namen der Konfiguration aus, die Sie löschen möchten.

   :::image type="content" source="./media/remove-desired-state-configuration-package/configurations-tab.png" alt-text="Screenshot der Registerkarte „Konfigurationen“." lightbox="./media/remove-desired-state-configuration-package/configurations-tab.png":::

1. Klicken Sie auf der Detailseite der Konfiguration auf **Löschen**, um die Konfiguration zu entfernen.

   :::image type="content" source="./media/remove-desired-state-configuration-package/delete-extension.png" alt-text="Screenshot des Löschens einer Erweiterung." lightbox="./media/remove-desired-state-configuration-package/delete-extension.png":::

## <a name="manually-delete-a-configuration-file-from-a-node"></a>Manuelles Löschen einer Konfigurationsdatei von einem Knoten

Wenn Sie das Azure-Portal nicht verwenden möchten, können Sie die MOF-Konfigurationsdateien wie folgt manuell löschen.

### <a name="delete-a-windows-configuration-using-powershell"></a>Löschen einer Windows-Konfiguration mithilfe von PowerShell

Um ein importiertes DSC-Konfigurationsdokument (MOF) zu entfernen, verwenden Sie das Cmdlet [Remove-DscConfigurationDocument](/powershell/module/psdesiredstateconfiguration/remove-dscconfigurationdocument).

### <a name="delete-a-linux-configuration"></a>Löschen einer Linux-Konfiguration

Die Konfigurationsdateien werden in „/etc/opt/omi/conf/dsc/configuration/“ gespeichert. Entfernen Sie die MOF-Dateien in diesem Verzeichnis, um die Konfiguration des Knotens zu löschen.

## <a name="remove-the-dsc-package-from-a-linux-node"></a>Entfernen des DSC-Pakets von einem Linux-Knoten

Dieser Schritt ist optional. Wenn Sie die Registrierung eines Linux-Knotens bei State Configuration (DSC) aufheben, werden nicht die DSC- und OMI-Pakete vom Computer entfernt. Verwenden Sie die folgenden Befehle, um die Pakete sowie alle Protokolle und zugehörigen Daten zu entfernen.

Die Paketnamen und andere relevante Details finden Sie im GitHub-Repository [PowerShell Desired State Configuration für Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux).

### <a name="rpm-based-systems"></a>RPM-basierte Systeme

```bash
RPM -e <package name>
``` 

### <a name="dpkg-based-systems"></a>dpkg-basierte Systeme

```bash
dpkg -P <package name>
```

 ## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie den Knoten erneut registrieren oder einen neuen Knoten registrieren möchten, finden Sie Informationen hierzu unter [Registrieren eines virtuellen Computers für die Verwaltung durch State Configuration](/azure/automation/tutorial-configure-servers-desired-state#register-a-vm-to-be-managed-by-state-configuration).

- Wenn Sie die Konfiguration wieder hinzufügen und neu kompilieren möchten, finden Sie Informationen hierzu unter [Kompilieren von DSC-Konfigurationen in Azure Automation State Configuration](/azure/automation/automation-dsc-compile).