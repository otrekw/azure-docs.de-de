---
title: Konfigurieren von Azure Marketplace-Image-Einstellungen in Azure DevTest Labs
description: Konfigurieren Sie, welche Azure Marketplace-Images beim Erstellen eines virtuellen Computers in Azure DevTest Labs verwendet werden können.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 9fdb4e3a888e876f91b8af2e4854a9c101eea45c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85482716"
---
# <a name="configure-azure-marketplace-image-settings-in-azure-devtest-labs"></a>Konfigurieren von Azure Marketplace-Image-Einstellungen in Azure DevTest Labs
DevTest Labs unterstützt die Erstellung von virtuellen Computern (VMs, Virtual Machines) auf Basis von Azure Marketplace-Images, abhängig davon, wie Sie Azure Marketplace-Images zur Verwendung in Ihrem Lab konfiguriert haben. In diesem Artikel erfahren Sie, wie Sie ggf. angeben, welche Azure Marketplace-Images zum Erstellen virtueller Computer in einem Lab verwendet werden können. Dadurch wird sichergestellt, dass das Team nur Zugriff auf die Marketplace-Images hat, die es benötigt. 

## <a name="select-which-azure-marketplace-images-are-allowed-when-creating-a-vm"></a>Auswählen, welche Azure Marketplace-Images beim Erstellen eines virtuellen Computers zulässig sind
1. Melden Sie sich beim [Azure-Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) an.
2. Wählen Sie **Alle Dienste** und dann in der Liste die Option **DevTest Labs**.
3. Wählen Sie in der Liste der Labs das gewünschte Lab aus. 
4. Wählen Sie auf dem Blatt des Labs die Option **Konfiguration und Richtlinien** aus.
5. Wählen Sie auf dem Blatt **Konfiguration und Richtlinien** des Labs unter **Virtual Machine Bases** (VM-Basis) die Option **Marketplace-Images**.
6. Geben Sie an, ob alle qualifizierten Azure Marketplace-Images für die Verwendung als Basis einer neuen VM verfügbar sein sollen. Bei Auswahl von **Ja**sind alle Azure Marketplace-Images im Lab zulässig, die alle folgenden Kriterien erfüllen:
   
   * Das Image erstellt einen einzelnen virtuellen Computer, **und**
   * Das Image verwendet den Azure-Resource-Manager zur Bereitstellung von VMs, **und**
   * Das Image erfordert nicht den Erwerb eines zusätzlichen Lizenzplans.
     
     Wenn keine Images zulässig sein sollen, oder Sie angeben möchten, welche Bilder verwendet werden können, wählen Sie **Nein**.
     
     ![Option, die Verwendung aller Marketplace-Images als Basis-Images für virtuelle Computer zuzulassen](./media/devtest-lab-configure-marketplace-images/allow-all-marketplace-images.png)
7. Bei Auswahl von **Nein** im vorherigen Schritt ist das Kontrollkästchen **Allowed images/Select all** (Zulässige Images/Alle auswählen) aktiviert. 
   Sie können diese Option zusammen mit dem Suchfeld verwenden, um schnell alle in der Liste angezeigten Elemente auszuwählen oder ihre Auswahl aufzuheben.
   * Wählen Sie die Azure Marketplace-Images aus, die Sie für die Erstellung des virtuellen Computers zulassen möchten, indem Sie das Kontrollkästchen jedes entsprechenden Images aktivieren.
   * Wählen Sie nichts aus der Liste, wenn keine Azure Marketplace-Images im Lab verwendet werden sollen.
   
     ![Sie können angeben, welche Azure Marketplace-Images als Basis-Images für virtuelle Computer verwendet werden können.](./media/devtest-lab-configure-marketplace-images/select-marketplace-images.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie konfiguriert haben, auf welche Weise Azure Marketplace-Images beim Erstellen eines virtuellen Computers zulässig sind, ist der nächste Schritt das [Hinzufügen eines virtuellen Computers zu Ihrem Lab](devtest-lab-add-vm.md).

