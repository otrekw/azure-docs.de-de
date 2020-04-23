---
title: Erstellen einer Azure Shared Image Gallery-Instanz über das Portal
description: Erfahren Sie, wie Sie mit dem Azure-Portal VM-Images erstellen und freigeben.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/06/2019
ms.author: cynthn
ms.custom: ''
ms.openlocfilehash: 83cdae95d43884647e257cbf1808222a542a212e
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81458095"
---
# <a name="create-an-azure-shared-image-gallery-using-the-portal"></a>Erstellen einer Azure Shared Image Gallery-Instanz über das Portal

Der [Katalog mit freigegebenen Images](shared-image-galleries.md) vereinfacht das Freigeben benutzerdefinierter Images in Ihrer Organisation. Benutzerdefinierte Images sind wie Marketplace-Images, Sie erstellen sie jedoch selbst. Benutzerdefinierte Images können zum Laden von Bereitstellungsaufgaben verwendet werden, z.B. zum Vorabladen von Anwendungen sowie für Anwendungskonfigurationen und andere Betriebssystemkonfigurationen. 

Der Katalog mit geteilten Images gestattet es Ihnen, Ihre benutzerdefinierten VM-Images mit anderen Personen in Ihrer Organisation, innerhalb einer oder zwischen Regionen, innerhalb eines AAD-Mandanten zu teilen. Wählen Sie aus, welche Images Sie teilen möchten, in welchen Regionen Sie sie verfügbar machen möchten, und mit wem Sie sie teilen möchten. Sie können mehrere Kataloge erstellen, damit Sie geteilte Images logisch gruppieren können. 

Der Katalog ist eine Ressource der obersten Ebene, die vollständige rollenbasierte Zugriffssteuerung (RBAC) bereitstellt. Images bieten Versionsverwaltung, und Sie können sich entschließen, jede Imageversion in eine andere Gruppe von Azure-Regionen zu replizieren. Der Katalog funktioniert nur mit verwalteten Images.

Die Funktion „Katalog mit freigegebenen Images“ verfügt über mehrere Ressourcentypen. Wir werden diese in diesem Artikel verwenden oder erstellen:

| Resource | BESCHREIBUNG|
|----------|------------|
| **Verwaltetes Image** | Ein Basisimage, das eigenständig oder zum Erstellen einer **Imageversion** in einem Imagekatalog verwendet werden kann. Verwaltete Images werden aus [generalisierten](shared-image-galleries.md#generalized-and-specialized-images) virtuellen Computern erstellt. Ein verwaltetes Image ist ein spezieller VHD-Typ, mit dem mehrere virtuelle Computer und jetzt auch Versionen von freigegebenen Images erstellt werden können. |
| **Momentaufnahme** | Eine Kopie einer VHD, die zum Erstellen einer **Imageversion** verwendet werden kann. Momentaufnahmen können von einem [spezialisierten](shared-image-galleries.md#generalized-and-specialized-images) virtuellen Computer (einem virtuellen Computer, der nicht generalisiert wurde) erstellt und dann allein oder mit Momentaufnahmen von Datenträgern verwendet werden, um eine spezialisierte Imageversion zu erstellen.
| **Imagekatalog** | Wie der Azure Marketplace ist ein **Imagekatalog** ein Repository zum Verwalten und Teilen von Images, aber Sie kontrollieren, wer Zugriff hat. |
| **Imagedefinition** | Images sind innerhalb eines Katalogs definiert und enthalten Informationen über das jeweilige Image und die Anforderungen für dessen Verwendung in Ihrer Organisation. Sie können Informationen einbinden, etwa, ob das Image ein generalisiertes oder spezialisiertes Image ist, das Betriebssystem, Anforderungen hinsichtlich minimalem und maximalem Arbeitsspeicher und Versionshinweise. Es ist eine Definition eines Imagetyps. |
| **Imageversion** | Eine **Imageversion** ist, was Sie verwenden, um einen virtuellen Computer zu erstellen, wenn Sie einen Katalog verwenden. Sie können nach Bedarf mehrere Versionen eines Images für Ihre Umgebung haben. Wie bei einem verwalteten Image wird, wenn Sie eine **Imageversion** zum Erstellen einer VM verwenden, wird die Imageversion verwendet, um neue Datenträger für den virtuellen Computer zu erstellen. Imageversionen können mehrmals verwendet werden. |

<br>


> [!IMPORTANT]
> Spezialisierte Images befinden sich zurzeit in der öffentlichen Vorschauphase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>
> **Bekannte Einschränkungen der Vorschau**: Virtuelle Computer können nur mithilfe des Portals oder der API aus spezialisierten Images erstellt werden. In der Vorschauversion werden die CLI und PowerShell nicht unterstützt.

## <a name="before-you-begin"></a>Voraussetzungen

Für das Beispiel in diesem Artikel muss ein verwaltetes Image eines generalisierten virtuellen Computers oder eine Momentaufnahme eines spezialisierten virtuellen Computers vorhanden sein. Sie können im [Tutorial: Erstellen eines benutzerdefinierten Images eines virtuellen Azure-Computers mit Azure PowerShell](tutorial-custom-images.md) lesen, wie ein verwaltetes Image erstellt wird, oder [erstellen Sie eine Momentaufnahme](snapshot-copy-managed-disk.md) für einen spezialisierten virtuellen Computer. Für verwaltete Images und Momentaufnahmen darf der Datenträger nicht größer als 1 TB sein.

Ersetzen Sie beim Durcharbeiten dieses Artikels bei Bedarf den Namen der Ressourcengruppe und des virtuellen Computers.


[!INCLUDE [virtual-machines-common-shared-images-portal](../../../includes/virtual-machines-common-shared-images-portal.md)]
 
## <a name="create-vms"></a>Virtuelle Computer erstellen

Jetzt können Sie einen oder mehrere neue virtuelle Computer erstellen. Dieses Beispiel erstellt den virtuellen Computer *myVM* in *myResourceGroup* im Rechenzentrum *USA, Osten*.

1. Wechseln Sie zur Imagedefinition. Sie können den Ressourcenfilter verwenden, um alle verfügbaren Imagedefinitionen anzuzeigen.
1. Wählen Sie im Menü oben auf der Seite für die Imagedefinition **VM erstellen** aus.
1. Wählen Sie unter **Ressourcengruppe** die Option **Neu erstellen** aus, und geben Sie *myResourceGroup* als Namen ein.
1. Geben Sie unter **Name des virtuellen Computers** die Zeichenfolge *myVM* ein.
1. Wählen Sie als **Region** die Option *USA, Osten* aus.
1. Übernehmen Sie für **Verfügbarkeitsoptionen** den Standardwert *Keine Infrastrukturredundanz erforderlich*.
1. Der Wert für **Image** wird automatisch mit der neuesten Imageversion (`latest`) ausgefüllt, wenn Sie auf der Seite für die Imagedefinition begonnen haben.
1. Wählen Sie für **Größe** in der Liste der verfügbaren Größen eine VM-Größe aus und klicken Sie dann auf **Auswählen**.
1. Wenn das Image generalisiert wurde, müssen Sie unter **Administratorkonto** einen Benutzernamen, z. B. *azureuser*, und ein Kennwort angeben. Das Kennwort muss mindestens zwölf Zeichen lang sein und die [definierten Anforderungen an die Komplexität](faq.md#what-are-the-password-requirements-when-creating-a-vm) erfüllen. Wenn das Image spezialisiert ist, werden die Felder für Benutzername und Kennwort abgeblendet, da Benutzername und Kennwort für die Quell-VM verwendet werden.
1. Wenn Sie Remotezugriff auf den virtuellen Computer zulassen möchten, wählen Sie unter **Öffentliche Eingangsports** die Option **Ausgewählte Ports zulassen** aus, und wählen Sie dann in der Dropdownliste **RDP (3389)** aus. Wenn Sie keinen Remotezugriff auf den virtuellen Computer ermöglichen möchten, lassen Sie für ausgewählte **Öffentliche Eingangsports** die Option **Keine** ausgewählt.
1. Wählen Sie abschließend die Schaltfläche **Überprüfen + erstellen** im unteren Seitenbereich aus.
1. Nachdem die VM-Konfiguration erfolgreich überprüft wurde, wählen Sie im unteren Seitenbereich **Erstellen** aus, um die Bereitstellung zu starten.


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Ressourcengruppe, virtueller Computer und alle zugehörigen Ressourcen nicht mehr benötigt werden, können Sie sie löschen. Wählen Sie dazu die Ressourcengruppe für den virtuellen Computer aus, klicken Sie auf **Löschen**, und bestätigen Sie den Namen der zu löschenden Ressourcengruppe.

Wenn Sie einzelne Ressourcen löschen möchten, müssen Sie diese in umgekehrter Reihenfolge löschen. Um beispielsweise eine Imagedefinition zu löschen, müssen Sie alle Imageversionen löschen, die aus diesem Image erstellt wurden.

## <a name="next-steps"></a>Nächste Schritte

Sie können auch mithilfe von Vorlagen eine Ressource im Katalog für freigegebene Images erstellen. Es stehen mehrere Azure-Schnellstartvorlagen zur Verfügung: 

- [Erstellen eines Katalogs mit freigegebenen Images](https://azure.microsoft.com/resources/templates/101-sig-create/)
- [Erstellen einer Imagedefinition in einem Katalog mit freigegebenen Images](https://azure.microsoft.com/resources/templates/101-sig-image-definition-create/)
- [Erstellen einer Imageversion in einem Katalog mit freigegebenen Images](https://azure.microsoft.com/resources/templates/101-sig-image-version-create/)
- [Erstellen eines virtuellen Computers aus einer Imageversion](https://azure.microsoft.com/resources/templates/101-vm-from-sig/)

Weitere Informationen zu Katalogen mit geteilten Images finden Sie in der [Übersicht](shared-image-galleries.md). Sollten Probleme auftreten, sehen Sie unter [Problembehandlung für Kataloge mit freigegebenen Images](troubleshooting-shared-images.md) nach.

