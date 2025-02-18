---
title: Erstellen eines Katalogs mit freigegebenen Images mit Azure PowerShell
description: Erfahren Sie, wie Sie mit Azure PowerShell einen Katalog mit freigegebenen Images in Azure erstellen.
author: cynthn
ms.service: virtual-machines
ms.subservice: shared-image-gallery
ms.topic: how-to
ms.workload: infrastructure
ms.date: 05/04/2020
ms.author: cynthn
ms.reviewer: akjosh
ms.openlocfilehash: 4a79892669c746121a0699a5696dcb95d55d7572
ms.sourcegitcommit: abf31d2627316575e076e5f3445ce3259de32dac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2021
ms.locfileid: "114204395"
---
# <a name="create-a-shared-image-gallery-with-azure-powershell"></a>Erstellen eines Katalogs mit freigegebenen Images mit Azure PowerShell 

Der [Katalog mit freigegebenen Images](./shared-image-galleries.md) vereinfacht das Freigeben benutzerdefinierter Images in Ihrer Organisation. Benutzerdefinierte Images sind wie Marketplace-Images, Sie erstellen sie jedoch selbst. Benutzerdefinierte Images können zum Laden von Bereitstellungsaufgaben verwendet werden, z.B. zum Vorabladen von Anwendungen sowie für Anwendungskonfigurationen und andere Betriebssystemkonfigurationen. 

Der Katalog mit geteilten Images gestattet es Ihnen, Ihre benutzerdefinierten VM-Images mit anderen Personen in Ihrer Organisation, innerhalb einer oder zwischen Regionen, innerhalb eines AAD-Mandanten zu teilen. Wählen Sie aus, welche Images Sie teilen möchten, in welchen Regionen Sie sie verfügbar machen möchten, und mit wem Sie sie teilen möchten. Sie können mehrere Kataloge erstellen, damit Sie geteilte Images logisch gruppieren können. 

Der Katalog ist eine Ressource der obersten Ebene, die vollständige rollenbasierte Zugriffssteuerung in Azure (RBAC) bereitstellt. Images bieten Versionsverwaltung, und Sie können sich entschließen, jede Imageversion in eine andere Gruppe von Azure-Regionen zu replizieren. Der Katalog funktioniert nur mit verwalteten Images.

Die Funktion „Katalog mit freigegebenen Images“ verfügt über mehrere Ressourcentypen. 

[!INCLUDE [virtual-machines-shared-image-gallery-resources](../../includes/virtual-machines-shared-image-gallery-resources.md)]


[!INCLUDE [virtual-machines-common-shared-images-powershell](../../includes/virtual-machines-common-shared-images-powershell.md)]


## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie ein Image aus einer [VM](image-version-vm-powershell.md), einem [verwalteten Image](image-version-managed-image-powershell.md) oder einem [Image in einem anderen Katalog](image-version-another-gallery-powershell.md).

[Azure Image Builder (Vorschauversion)](./image-builder-overview.md) hilft beim Automatisieren der Erstellung von Imageversionen. Sie können den Dienst sogar zum Aktualisieren und [Erstellen einer neuen Imageversion aus einer vorhandenen](./windows/image-builder-gallery-update-image-version.md) verwenden. 

Sie können auch mithilfe von Vorlagen eine Ressource im Katalog für freigegebene Images erstellen. Es stehen mehrere Azure-Schnellstartvorlagen zur Verfügung: 

- [Erstellen eines Katalogs mit freigegebenen Images](https://azure.microsoft.com/resources/templates/sig-create/)
- [Erstellen einer Imagedefinition in einem Katalog mit freigegebenen Images](https://azure.microsoft.com/resources/templates/sig-image-definition-create/)
- [Erstellen einer Imageversion in einem Katalog mit freigegebenen Images](https://azure.microsoft.com/resources/templates/sig-image-version-create/)
