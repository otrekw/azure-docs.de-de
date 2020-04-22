---
title: Registerkarte „Angebotseinstellungen“ für virtuelle Computer im Cloud-Partnerportal für Azure Marketplace
description: Beschreibt die Registerkarte „Angebotseinstellungen“, die beim Erstellen eines VM-Angebots für den Azure Marketplace verwendet wird.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: dsindona
ms.openlocfilehash: 94be2e5d3c2c941ab17401a743ea86acbe8b6252
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273799"
---
# <a name="virtual-machine-offer-settings-tab"></a>Registerkarte „Angebotseinstellungen“ für virtuelle Computer

> [!IMPORTANT]
> Ab dem 13. April 2020 beginnen wir mit der Umstellung der Verwaltung Ihrer Azure Virtual Machines-Angebote auf Partner Center. Nach der Migration erstellen und verwalten Sie Ihre Angebote in Partner Center. Befolgen Sie zum Verwalten Ihrer migrierten Angebote die Anweisungen unter [Erstellen eines Azure Virtual Machines-Angebots](https://aka.ms/CreateAzureVMoffer).

Auf der Seite **Neues Angebot** für virtuelle Computer wird als Erstes die Registerkarte **Angebotseinstellungen** angezeigt.  

![Seite „Neues Angebot“ für virtuelle Computer](./media/publishvm_004.png)


## <a name="offer-settings-fields"></a>Felder von „Angebotseinstellungen“

Auf der Registerkarte **Angebotseinstellungen** müssen folgende Felder ausgefüllt werden.  Mit einem an den Feldnamen angefügten Sternchen (*) wird angegeben, dass eine Eingabe erforderlich ist. 

|  **Feld**       |     **Beschreibung**                                                          |
|  ---------       |     ---------------                                                          |
| **Angebots-ID\***   | Ein (innerhalb eines Herausgeberprofils) eindeutiger Bezeichner für das Angebot. Dieser Bezeichner ist in Produkt-URLs, Azure Resource Manager-Vorlagen und Abrechnungsberichten sichtbar. Er hat eine maximale Länge von 50 Zeichen und darf ausschließlich alphanumerische Kleinbuchstaben und Bindestriche (-) enthalten, aber nicht mit einem Bindestrich enden. Nachdem ein Angebot live geschaltet wurde, kann dieses Feld nicht mehr geändert werden. <br> Wenn also beispielsweise Contoso ein Angebot mit der Angebots-ID **sample-vm** veröffentlicht, wird ihm die Azure Marketplace-URL `https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sample-vm?tab=Overview` zugewiesen. |
| **Herausgeber\***  | Der eindeutige Bezeichner Ihrer Organisation im Azure Marketplace. Alle Ihre Angebote sollten mit Ihrer Herausgeber-ID verknüpft sein. Nachdem das Angebot gespeichert wurde, kann dieser Wert nicht mehr geändert werden. |
| **Name\***       | Der Anzeigenamen für Ihr Angebot. Dieser Name wird im Azure Marketplace und im Cloud-Partnerportal angezeigt. Er darf aus höchstens 50 Zeichen bestehen. Hier empfiehlt es sich, einen Markennamen mit gutem Wiedererkennungswert für Ihr Produkt zu verwenden. Verwenden Sie hier nicht den Namen Ihrer Organisation, es sei denn, das Produkt wird unter diesem Namen beworben. Wenn Sie dieses Angebot auf anderen Websites und in anderen Publikationen vermarkten, achten Sie darauf, überall exakt den gleichen Namen zu verwenden. |
|   |   |
 
Klicken Sie auf **Speichern**, nachdem Sie alle Felder ausgefüllt haben. 


## <a name="next-steps"></a>Nächste Schritte

Auf der nächsten Registerkarte fügen Sie Ihrem Angebot [SKUs](./cpp-skus-tab.md) hinzu.
