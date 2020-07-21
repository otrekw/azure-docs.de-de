---
title: Erstellen eines benutzerdefinierten Azure DevTest Labs-Images aus einem virtuellen Computer | Microsoft Docs
description: Erfahren Sie, wie Sie mit dem Azure-Portal aus einem bereitgestellten virtuellen Computer ein benutzerdefiniertes Image in Azure DevTest Labs erstellen.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 597ef49b667cf613b721c645e09f8e6dcb157011
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85476596"
---
# <a name="create-a-custom-image-from-a-vm"></a>Erstellen eines benutzerdefinierten Images von einem virtuellen Computer

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

## <a name="step-by-step-instructions"></a>Schrittweise Anweisungen

Sie können ein benutzerdefiniertes Image aus einem bereitgestellten virtuellen Computer erstellen und es anschließend zum Erstellen identischer virtuelle Computer verwenden. Die folgenden Schritte beschreiben, wie Sie ein benutzerdefiniertes Image von einem virtuellen Computer erstellen:

1. Melden Sie sich beim [Azure-Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) an.

1. Wählen Sie **Alle Dienste** und dann in der Liste die Option **DevTest Labs**.

1. Wählen Sie in der Liste der Labs das gewünschte Lab aus.  

1. Wählen Sie im Hauptbereich des Labs die Option **Meine virtuellen Computer**.
 
1. Wählen Sie im Bereich **Meine virtuellen Computer** den virtuellen Computer aus, von dem Sie das benutzerdefinierte Image erstellen möchten.

1. Wählen Sie im Verwaltungsbereich des virtuellen Computers unter **VORGÄNGE** die Option **Benutzerdefiniertes Image erstellen (VHD)** .

    :::image type="content" source="./media/devtest-lab-create-template/create-custom-image.png" alt-text="Menüelement: Benutzerdefiniertes Bild erstellen":::
1. Geben Sie im Bereich **Benutzerdefiniertes Image** einen Namen und eine Beschreibung für Ihr benutzerdefiniertes Image ein. Diese Informationen werden in der Liste der Basisimages angezeigt, wenn Sie einen virtuellen Computer erstellen. Das benutzerdefinierte Image umfasst den Betriebssystemdatenträger und alle Datenträger, die an den virtuellen Computer angefügt sind.

    :::image type="content" source="./media/devtest-lab-create-template/create-custom-image-blade.png" alt-text="Bereich „Benutzerdefiniertes Image erstellen“":::
1. Wählen Sie aus, ob „sysprep“ auf dem virtuellen Computer ausgeführt wurde. Wenn „sysprep“ auf dem virtuellen Computer nicht ausgeführt wurde, geben Sie an, ob „sysprep“ auf dem virtuellen Computer ausgeführt werden soll, wenn das benutzerdefinierte Image erstellt wird.
1. Wählen Sie **OK** , wenn Sie mit der Erstellung des benutzerdefinierten Images fertig sind.

    Nach wenigen Minuten wird das benutzerdefinierte Image erstellt und im Speicherkonto des Labs gespeichert. Das Image ist in der Liste mit den Basisimages verfügbar, falls ein Lab-Benutzer einen neuen virtuellen Computer erstellen möchte.

    :::image type="content" source="./media/devtest-lab-create-template/custom-image-available-as-base.png" alt-text="Verfügbares benutzerdefiniertes Image in der Liste mit den Basisimages":::

## <a name="related-blog-posts"></a>Verwandte Blogbeiträge

- [Custom images or formulas? (Benutzerdefinierte Images oder Formeln?)](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Copying Custom Images between Azure DevTest Labs (Kopieren benutzerdefinierter Images zwischen Azure DevTest Labs)](https://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Nächste Schritte

- [Hinzufügen eines virtuellen Computers zum Lab](devtest-lab-add-vm.md)
