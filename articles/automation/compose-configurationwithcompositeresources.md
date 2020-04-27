---
title: Zusammenstellen von DSC-Konfigurationen in Azure Automation State Configuration mit zusammengesetzten Ressourcen
description: Es wird beschrieben, wie Sie Konfigurationen in Azure Automation State Configuration mit zusammengesetzten Ressourcen zusammenstellen.
keywords: PowerShell DSC, Desired State Configuration, Konfiguration des gewünschten Zustands, PowerShell DSC Azure, zusammengesetzte Ressourcen
services: automation
ms.subservice: dsc
ms.date: 08/21/2018
ms.topic: conceptual
ms.openlocfilehash: 7e7625ae95d5355ae1122a16ea4828eed5f78c73
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "81682929"
---
# <a name="composing-dsc-configurations-in-azure-automation-state-configuration-using-composite-resources"></a>Zusammenstellen von DSC-Konfigurationen in Azure Automation State Configuration mit zusammengesetzten Ressourcen

Wenn Sie eine Ressource mit mehr als nur einer Konfiguration des gewünschten Zustands (DSC) verwalten müssen, ist die beste Vorgehensweise die Nutzung von [zusammengesetzten Ressourcen](/powershell/scripting/dsc/resources/authoringresourcecomposite). Eine zusammengesetzte Ressource ist eine geschachtelte und parametrisierte Konfiguration, die als DSC-Ressource in einer anderen Konfiguration verwendet wird. Die Verwendung von zusammengesetzten Ressourcen ermöglicht Ihnen die Erstellung von komplexen Konfigurationen, während die zugrunde liegenden zusammengesetzten Ressourcen einzeln verwaltet und erstellt werden können.

Mit Azure Automation können Sie [zusammengesetzte Ressourcen importieren und kompilieren](automation-dsc-compile.md). Nachdem Sie zusammengesetzte Ressourcen in Ihr Automation-Konto importiert haben, können Sie Azure Automation State Configuration über die Funktion **State Configuration (DSC)** im Azure-Portal verwenden.

## <a name="composing-a-configuration-from-composite-resources"></a>Zusammenstellen einer Konfiguration aus zusammengesetzten Ressourcen

Bevor Sie eine aus zusammengesetzten Ressourcen erstellte Konfiguration im Azure-Portal zuweisen können, müssen Sie dies Konfiguration zusammenstellen. Bei der Zusammenstellung verwenden Sie auf der Seite „State Configuration (DSC)“ die Option **Konfiguration zusammenstellen**, während Sie sich auf der Registerkarte **Konfigurationen** oder **Kompilierte Konfigurationen** befinden.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Klicken Sie links auf **Alle Ressourcen** und dann auf den Namen des Automation-Kontos.
1. Wählen Sie auf der Seite „Automation-Konto“ unter **Konfigurationsverwaltung** die Option **State Configuration (DSC)** aus.
1. Klicken Sie auf der Seite „State Configuration (DSC)“ auf eine der Registerkarten **Konfigurationen** oder **Kompilierte Konfigurationen**, und klicken Sie dann im Menü oben auf der Seite auf **Konfiguration zusammenstellen**.
1. Geben Sie im Schritt **Grundlagen** den neuen Konfigurationsnamen ein (erforderlich), und klicken Sie jeweils auf eine beliebige Stelle der Zeile für eine zusammengesetzte Ressource, die Sie in Ihre neue Konfiguration einfügen möchten. Klicken Sie anschließend auf **Weiter** oder auf den Schritt **Quellcode**. Für die folgenden Schritte haben wir die zusammengesetzten Ressourcen `PSExecutionPolicy` und `RenameAndDomainJoin` ausgewählt.
   ![Screenshot: Schritt „Grundlagen“ der Seite „Konfiguration zusammenstellen“](./media/compose-configurationwithcompositeresources/compose-configuration-basics.png)
1. Im Schritt **Quellcode** wird angezeigt, wie die zusammengestellte Konfiguration der ausgewählten zusammengesetzten Ressourcen aussieht. Sie sehen, wie alle Parameter zusammengeführt und an die zusammengesetzte Ressource übergeben werden. Wenn Sie den neuen Quellcode geprüft haben, können Sie auf **Weiter** oder auf den Schritt **Parameter** klicken.
   ![Screenshot: Schritt „Quellcode“ der Seite „Konfiguration zusammenstellen“](./media/compose-configurationwithcompositeresources/compose-configuration-sourcecode.png)
1. Im Schritt **Parameter** wird der Parameter für jede einzelne zusammengesetzte Ressource verfügbar gemacht, damit Werte angegeben werden können. Wenn ein Parameter über eine Beschreibung verfügt, wird diese neben dem Parameterfeld angezeigt. Wenn ein Parameter vom Typ `PSCredential` ist, bietet das Dropdown eine Liste der **Anmeldeinformations**objekte im aktuellen Automation-Konto. Außerdem ist die Option **+ Anmeldeinformationen hinzufügen** verfügbar. Nachdem alle erforderlichen Parameter angegeben wurden, können Sie auf **Save and compile** (Speichern und kompilieren) klicken.
   ![Screenshot: Schritt „Parameter“ der Seite „Konfiguration zusammenstellen“](./media/compose-configurationwithcompositeresources/compose-configuration-parameters.png)

Nachdem die neue Konfiguration gespeichert wurde, wird sie für die Kompilierung übermittelt. Sie können den Status des Kompilierungsauftrags wie für jede andere importierte Konfiguration anzeigen. Weitere Informationen finden Sie unter [Anzeigen eines Kompilierungsauftrags](automation-dsc-getting-started.md#viewing-a-compilation-job).

Nachdem die Kompilierung erfolgreich abgeschlossen wurde, wird die neue Konfiguration auf der Registerkarte **Kompilierte Konfigurationen** angezeigt. Sie können die Konfiguration dann einem verwalteten Knoten zuweisen, indem Sie die Schritte unter [Neuzuweisen eines Knotens zu einer anderen Knotenkonfiguration](automation-dsc-getting-started.md#reassigning-a-node-to-a-different-node-configuration) verwenden.

## <a name="next-steps"></a>Nächste Schritte

- Eine Einführung finden Sie unter [Erste Schritte mit Azure Automation State Configuration](automation-dsc-getting-started.md).
- Weitere Informationen zum Onboarding von Knoten finden Sie unter [Onboarding von Computern zur Verwaltung durch Azure Automation DSC](automation-dsc-onboarding.md).
- Wie Sie DSC-Konfigurationen kompilieren und anschließend Zielknoten zuweisen, erfahren Sie unter [Kompilieren von DSC-Konfigurationen in Azure Automation DSC](automation-dsc-compile.md).
- Eine PowerShell-Cmdlet-Referenz ist unter [Azure Automation State Configuration-Cmdlets](/powershell/module/azurerm.automation/#automation) verfügbar.
- Eine Preisübersicht finden Sie unter [Automation – Preise](https://azure.microsoft.com/pricing/details/automation/).
- Ein Verwendungsbeispiel für Azure Automation State Configuration in einer Continuous Deployment-Pipeline finden Sie unter [Continuous Deployment mit Azure Automation State Configuration und Chocolatey](automation-dsc-cd-chocolatey.md).
