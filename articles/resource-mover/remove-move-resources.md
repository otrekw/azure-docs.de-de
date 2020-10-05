---
title: Entfernen von Ressourcen aus einer Sammlung für die Verschiebung in Azure Resource Mover
description: Hier erfahren Sie, wie Ressourcen aus einer Sammlung für die Verschiebung in Azure Resource Mover entfernt werden.
manager: evansma
author: rayne-wiselman
ms.service: resource-move
ms.topic: how-to
ms.date: 09/08/2020
ms.author: raynew
ms.openlocfilehash: 241ccbda67f7a2518d0c44a0d362673922ad4284
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2020
ms.locfileid: "89652642"
---
# <a name="remove-resources-from-a-move-collection"></a>Entfernen von Ressourcen aus einer Sammlung für die Verschiebung

In diesem Artikel wird beschrieben, wie Ressourcen aus einer Sammlung für die Verschiebung in [Azure Resource Mover](overview.md) entfernt werden. Sammlungen für die Verschiebung werden beim Verschieben von Azure-Ressourcen zwischen Azure-Regionen verwendet.

## <a name="remove-a-resource-portal"></a>Entfernen einer Ressource (Portal)

Gehen Sie wie folgt vor, um im Resource Mover-Portal eine Ressource zu entfernen:

1. Wählen Sie unter **Regionsübergreifend** die Ressourcen aus, die Sie in der Sammlung > **entfernen** möchten.

    ![Schaltfläche, die Sie zum Entfernen auswählen](./media/remove-move-resources/portal-select-resources.png)

1. Klicken Sie unter **Ressourcen entfernen** auf **Entfernen**.

    ![Schaltfläche, die Sie auswählen, um Ressourcen in einer Sammlung für die Verschiebung zu entfernen](./media/remove-move-resources/remove-portal.png)

## <a name="remove-a-resource-powershell"></a>Entfernen einer Ressource (PowerShell)

Gehen Sie wie folgt vor, um eine Ressource (in unserem Beispiel die Computer „PSDemoVM“) in einer Sammlung mithilfe von PowerShell zu entfernen:

```azurepowershell-interactive
# Remove a resource using the resource ID
Remove-AzResourceMoverMoveResource -SubscriptionId  <subscription-id> -ResourceGroupName RegionMoveRG-centralus-westcentralus  -MoveCollectionName MoveCollection-centralus-westcentralus - Name PSDemoVM
```
**Erwartete Ausgabe**
![Ausgabetext nach dem Entfernen einer Ressource in einer Sammlung für die Verschiebung](./media/remove-move-resources/remove-resource.png)

## <a name="remove-a-collection-powershell"></a>Entfernen einer Sammlung (PowerShell)

Gehen Sie wie folgt vor, um eine ganze Sammlung für die Verschiebung mithilfe von PowerShell zu entfernen:

```azurepowershell-interactive
# Remove a resource using the resource ID
Remove-AzResourceMoverMoveResource -SubscriptionId  <subscription-id> -ResourceGroupName RegionMoveRG-centralus-westcentralus  -MoveCollectionName MoveCollection-centralus-westcentralus 
```
**Erwartete Ausgabe**
![Ausgabetext nach dem Entfernen einer Sammlung für die Verschiebung](./media/remove-move-resources/remove-collection.png)

## <a name="vm-resource-state-after-removing"></a>Zustand einer VM-Ressource nach dem Entfernen

Was geschieht, wenn Sie eine VM-Ressource in einer Sammlung für die Verschiebung entfernen, hängt vom Zustand der Ressource ab und ist in der Tabelle aufgeführt.

###  <a name="remove-vm-state"></a>Zustand einer VM-Ressource nach dem Entfernen
**Ressourcenzustand** | **VM** | **Netzwerk**
--- | --- | --- 
**Zur Sammlung für die Verschiebung hinzugefügt** | Wird in der Sammlung für die Verschiebung gelöscht. | Wird in der Sammlung für die Verschiebung gelöscht. 
**Abhängigkeiten aufgelöst/Vorbereitung steht aus** | Wird in der Sammlung für die Verschiebung gelöscht.  | Wird in der Sammlung für die Verschiebung gelöscht. 
**Vorbereitung wird durchgeführt**<br/> (oder ein anderer Zustand in Ausführung) | Beim Löschen tritt ein Fehler auf.  | Beim Löschen tritt ein Fehler auf.
**Fehler bei Vorbereitung** | Wird in der Sammlung für die Verschiebung gelöscht.<br/>Alle in der Zielregion erstellten Elemente werden gelöscht – auch Replikatdatenträger. <br/><br/> Beim Verschieben erstellte Infrastrukturressourcen müssen manuell gelöscht werden. | Wird in der Sammlung für die Verschiebung gelöscht.  
**Einleitung der Verschiebung ausstehend** | Wird in der Sammlung für die Verschiebung gelöscht.<br/><br/> Alle in der Zielregion erstellten Elemente werden gelöscht – auch der virtuelle Computer, Replikatdatenträger usw.  <br/><br/> Beim Verschieben erstellte Infrastrukturressourcen müssen manuell gelöscht werden. | Wird in der Sammlung für die Verschiebung gelöscht.
**Fehler beim Einleiten der Verschiebung** | Wird in der Sammlung für die Verschiebung gelöscht.<br/><br/> Alle in der Zielregion erstellten Elemente werden gelöscht – auch der virtuelle Computer, Replikatdatenträger usw.  <br/><br/> Beim Verschieben erstellte Infrastrukturressourcen müssen manuell gelöscht werden. | Wird in der Sammlung für die Verschiebung gelöscht.
**Commit ausstehend** | Es wird empfohlen, die Verschiebung zu verwerfen, sodass die Zielressourcen zuerst gelöscht werden.<br/><br/> Die Ressource wird wieder in den Zustand **Einleitung der Verschiebung ausstehend** versetzt, und Sie können den Vorgang von dort aus fortsetzen. | Es wird empfohlen, die Verschiebung zu verwerfen, sodass die Zielressourcen zuerst gelöscht werden.<br/><br/> Die Ressource wird wieder in den Zustand **Einleitung der Verschiebung ausstehend** versetzt, und Sie können den Vorgang von dort aus fortsetzen. 
**Fehler bei Commit** | Es wird empfohlen, die Verschiebung zu verwerfen, sodass die Zielressourcen zuerst gelöscht werden.<br/><br/> Die Ressource wird wieder in den Zustand **Einleitung der Verschiebung ausstehend** versetzt, und Sie können den Vorgang von dort aus fortsetzen. | Es wird empfohlen, die Verschiebung zu verwerfen, sodass die Zielressourcen zuerst gelöscht werden.<br/><br/> Die Ressource wird wieder in den Zustand **Einleitung der Verschiebung ausstehend** versetzt, und Sie können den Vorgang von dort aus fortsetzen.
**Verwerfen abgeschlossen** | Die Ressource wird wieder in den Zustand **Einleitung der Verschiebung ausstehend** versetzt.<br/><br/> Sie wird in der Sammlung für die Verschiebung zusammen mit allen am Ziel erstellten Elementen – virtuelle Computer, Replikatdatenträger, Tresore usw. – gelöscht.  <br/><br/> Beim Verschieben erstellte Infrastrukturressourcen müssen manuell gelöscht werden. <br/><br/> Beim Verschieben erstellte Infrastrukturressourcen müssen manuell gelöscht werden. |  Die Ressource wird wieder in den Zustand **Einleitung der Verschiebung ausstehend** versetzt.<br/><br/> Sie wird in der Sammlung für die Verschiebung gelöscht.
**Fehler beim Verwerfen** | Es wird empfohlen, die Verschiebung zu verwerfen, sodass die Zielressourcen zuerst gelöscht werden.<br/><br/> Danach wird die Ressource wieder in den Zustand **Einleitung der Verschiebung ausstehend** versetzt, und Sie können den Vorgang von dort aus fortsetzen. | Es wird empfohlen, die Verschiebung zu verwerfen, sodass die Zielressourcen zuerst gelöscht werden.<br/><br/> Danach wird die Ressource wieder in den Zustand **Einleitung der Verschiebung ausstehend** versetzt, und Sie können den Vorgang von dort aus fortsetzen.
**Löschung der Quelle ausstehend** | Wird in der Sammlung für die Verschiebung gelöscht.<br/><br/> Die in der Zielregion erstellten Elemente werden nicht gelöscht.  | Wird in der Sammlung für die Verschiebung gelöscht.<br/><br/> Die in der Zielregion erstellten Elemente werden nicht gelöscht.
**Fehler beim Löschen der Quelle** | Wird in der Sammlung für die Verschiebung gelöscht.<br/><br/> Die in der Zielregion erstellten Elemente werden nicht gelöscht. | Wird in der Sammlung für die Verschiebung gelöscht.<br/><br/> Die in der Zielregion erstellten Elemente werden nicht gelöscht.

## <a name="sql-resource-state-after-removing"></a>Zustand einer SQL-Ressource nach dem Entfernen

Was geschieht, wenn Sie eine Azure SQL-Ressource in einer Sammlung für die Verschiebung entfernen, hängt vom Zustand der Ressource ab und ist in der Tabelle aufgeführt.

**Ressourcenzustand** | **SQL** 
--- | --- 
**Zur Sammlung für die Verschiebung hinzugefügt** | Wird in der Sammlung für die Verschiebung gelöscht. 
**Abhängigkeiten aufgelöst/Vorbereitung steht aus** | Wird in der Sammlung für die Verschiebung gelöscht. 
**Vorbereitung wird durchgeführt**<br/> (oder ein anderer Zustand in Ausführung)  | Beim Löschen tritt ein Fehler auf. 
**Fehler bei Vorbereitung** | Wird in der Sammlung für die Verschiebung gelöscht.<br/><br/>Alle in der Zielregion erstellten Elemente werden gelöscht. 
**Einleitung der Verschiebung ausstehend** |  Wird in der Sammlung für die Verschiebung gelöscht.<br/><br/>Alle in der Zielregion erstellten Elemente werden gelöscht. Die SQL-Datenbank ist zu diesem Zeitpunkt vorhanden und wird gelöscht. 
**Fehler beim Einleiten der Verschiebung** | Wird in der Sammlung für die Verschiebung gelöscht.<br/><br/>Alle in der Zielregion erstellten Elemente werden gelöscht. Die SQL-Datenbank ist zu diesem Zeitpunkt vorhanden und muss gelöscht werden. 
**Commit ausstehend** | Es wird empfohlen, die Verschiebung zu verwerfen, sodass die Zielressourcen zuerst gelöscht werden.<br/><br/> Die Ressource wird wieder in den Zustand **Einleitung der Verschiebung ausstehend** versetzt, und Sie können den Vorgang von dort aus fortsetzen.
**Fehler bei Commit** | Es wird empfohlen, die Verschiebung zu verwerfen, sodass die Zielressourcen zuerst gelöscht werden.<br/><br/> Die Ressource wird wieder in den Zustand **Einleitung der Verschiebung ausstehend** versetzt, und Sie können den Vorgang von dort aus fortsetzen. 
**Verwerfen abgeschlossen** |  Die Ressource wird wieder in den Zustand **Einleitung der Verschiebung ausstehend** versetzt.<br/><br/> Sie wird in der Sammlung für die Verschiebung zusammen mit allen am Ziel erstellten Elementen wie SQL-Datenbanken gelöscht. 
**Fehler beim Verwerfen** | Es wird empfohlen, die Verschiebung zu verwerfen, sodass die Zielressourcen zuerst gelöscht werden.<br/><br/> Danach wird die Ressource wieder in den Zustand **Einleitung der Verschiebung ausstehend** versetzt, und Sie können den Vorgang von dort aus fortsetzen. 
**Löschung der Quelle ausstehend** | Wird in der Sammlung für die Verschiebung gelöscht.<br/><br/> Die in der Zielregion erstellten Elemente werden nicht gelöscht. 
**Fehler beim Löschen der Quelle** | Wird in der Sammlung für die Verschiebung gelöscht.<br/><br/> Die in der Zielregion erstellten Elemente werden nicht gelöscht. 

## <a name="next-steps"></a>Nächste Schritte

Versuchen Sie, eine VM mit Resource Mover in eine andere Region zu [verschieben](tutorial-move-region-virtual-machines.md).