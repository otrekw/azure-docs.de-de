---
title: Verwenden von freigegebenen VM-Images zum Erstellen von Skalierungsgruppen in Azure PowerShell
description: Erfahren Sie, wie Sie mit Azure PowerShell freigegebene VM-Images erstellen können, mit denen Sie VM-Skalierungsgruppen in Azure bereitstellen können.
author: cynthn
ms.service: virtual-machine-scale-sets
ms.subservice: shared-image-gallery
ms.topic: how-to
ms.date: 05/04/2020
ms.author: cynthn
ms.reviewer: mimckitt
ms.openlocfilehash: 351d905b45d6c1bae6e4fa52aa34d22df9de82f9
ms.sourcegitcommit: abf31d2627316575e076e5f3445ce3259de32dac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2021
ms.locfileid: "114204088"
---
# <a name="create-and-use-shared-images-for-virtual-machine-scale-sets-with-the-azure-powershell"></a>Erstellen und Verwenden von freigegebenen Images für VM-Skalierungsgruppen mit Azure PowerShell

Wenn Sie eine Skalierungsgruppe erstellen, geben Sie ein Image an, das beim Bereitstellen der VM-Instanzen verwendet wird. Der Katalog mit geteilten Images vereinfacht das Teilen benutzerdefinierter Images in Ihrer Organisation. Benutzerdefinierte Images sind wie Marketplace-Images, Sie erstellen sie jedoch selbst. Benutzerdefinierte Images können zum Starten von Konfigurationen verwendet werden, z.B. zum Vorabladen von Anwendungen, Anwendungskonfigurationen und anderen Betriebssystemkonfigurationen. 

Der Katalog mit geteilten Images gestattet es Ihnen, Ihre benutzerdefinierten VM-Images mit anderen Personen in Ihrer Organisation, innerhalb einer oder zwischen Regionen, innerhalb eines AAD-Mandanten zu teilen. Wählen Sie aus, welche Images Sie teilen möchten, in welchen Regionen Sie sie verfügbar machen möchten, und mit wem Sie sie teilen möchten. Sie können mehrere Kataloge erstellen, damit Sie geteilte Images logisch gruppieren können. 

Der Katalog ist eine Ressource der obersten Ebene, die vollständige rollenbasierte Zugriffssteuerung in Azure (RBAC) bereitstellt. Images bieten Versionsverwaltung, und Sie können sich entschließen, jede Imageversion in eine andere Gruppe von Azure-Regionen zu replizieren. Der Katalog funktioniert nur mit verwalteten Images. 

Die Funktion „Katalog mit freigegebenen Images“ verfügt über mehrere Ressourcentypen. 


[!INCLUDE [virtual-machines-shared-image-gallery-resources](../../includes/virtual-machines-shared-image-gallery-resources.md)]


## <a name="before-you-begin"></a>Voraussetzungen

In den folgenden Schritten wird erläutert, wie Sie eine vorhandene VM in ein wiederverwendbares benutzerdefiniertes Image umwandeln, mit dem Sie neue VM-Instanzen erstellen können.

Für das Beispiel in diesem Artikel muss ein verwaltetes Image vorhanden sein. Sie können im [Tutorial: Erstellen und Verwenden eines benutzerdefinierten Images für VM-Skalierungsgruppen mit Azure PowerShell](tutorial-use-custom-image-powershell.md) ggf. eines erstellen. Wenn das verwaltete Image einen Datenträger für Daten enthält, darf dieser nicht größer als 1 TB sein.

Ersetzen Sie beim Durcharbeiten des Artikels bei Bedarf den Namen der Ressourcengruppe und des virtuellen Computers.


[!INCLUDE [virtual-machines-common-shared-images-ps](../../includes/virtual-machines-common-shared-images-powershell.md)]




## <a name="next-steps"></a>Nächste Schritte

Sie können auch mithilfe von Vorlagen eine Ressource im Katalog für freigegebene Images erstellen. Es stehen mehrere Azure-Schnellstartvorlagen zur Verfügung: 

- [Erstellen eines Katalogs mit freigegebenen Images](https://azure.microsoft.com/resources/templates/sig-create/)
- [Erstellen einer Imagedefinition in einem Katalog mit freigegebenen Images](https://azure.microsoft.com/resources/templates/sig-image-definition-create/)
- [Erstellen einer Imageversion in einem Katalog mit freigegebenen Images](https://azure.microsoft.com/resources/templates/sig-image-version-create/)

Weitere Informationen zu Katalogen mit geteilten Images finden Sie in der [Übersicht](../virtual-machines/shared-image-galleries.md). Sollten Probleme auftreten, sehen Sie unter [Problembehandlung für Kataloge mit freigegebenen Images](../virtual-machines/troubleshooting-shared-images.md) nach.
