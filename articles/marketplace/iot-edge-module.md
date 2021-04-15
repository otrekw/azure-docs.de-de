---
title: IoT Edge-Modulangebote in Azure Marketplace
description: Informationen über das Veröffentlichen von IoT Edge-Modulangeboten in Azure Marketplace
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: keferna
ms.author: keferna
ms.date: 08/18/2020
ms.openlocfilehash: 492571ac76acfcf388954a4b714b6603aced7338
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107107668"
---
# <a name="iot-edge-modules"></a>IoT Edge-Module

Die [Azure IoT Edge](https://azure.microsoft.com/services/iot-edge/)-Plattform wird von Microsoft Azure unterstützt.  Diese Plattform ermöglicht es Benutzern, Cloud-Workloads zum direkten Ausführen auf IoT-Geräten bereitzustellen.  Mit einem IoT Edge-Modul können Workloads offline ausgeführt und Datenanalysen lokal durchgeführt werden. Mit diesem Angebot können Sie Bandbreite sparen sowie lokale und vertrauliche Daten schützen. Zudem bietet es Antwortzeiten mit geringer Latenz.  Von diesen vorgefertigten Workloads können Sie ab sofort profitieren. Bislang sind nur wenige Erstanbieterlösungen von Microsoft verfügbar.  Und so mussten Sie bisher Zeit und Ressourcen in die Erstellung eigener benutzerdefinierter IoT-Lösungen investieren.

Dank der Einführung der [IoT Edge-Module im Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1) gibt es ab sofort einen zentralen Ort, an dem Herausgeber ihre Lösungen für die IoT-Zielgruppe anbieten und verkaufen können. Letztlich können IoT-Entwickler hier Funktionen zur Beschleunigung ihrer Lösungsentwicklung finden und erwerben.  

## <a name="key-benefits-of-iot-edge-modules-in-azure-marketplace"></a>Die wichtigsten Vorteile von IoT Edge-Modulen im Azure Marketplace

| **Für Herausgeber**    | **Für Kunden (IoT-Entwickler)**  |
| :------------------- | :-------------------|
| Sie erreichen Millionen von Entwicklern, die IoT Edge-Lösungen erstellen und bereitstellen möchten.  | Erstellen Sie eine IoT Edge-Lösung mit der Gewissheit, sichere und getestete Komponenten zu verwenden. |
| Sie müssen Ihre Lösung nur einmal veröffentlichen. Diese kann dann auf jeder IoT Edge-Hardware ausgeführt werden, die Container unterstützt. | Verkürzen Sie die Markteinführungszeit, indem Sie hier nach IoT Edge-Modulen von Erst- und Drittanbietern für spezifische Anforderungen suchen und diese bereitstellen. |
| Profitieren Sie von flexiblen Abrechnungsoptionen. <ul> <li> Kostenlos und BYOL (Bring Your Own License) </li> </ul> | Kaufen Sie mit den Abrechnungsmodellen Ihrer Wahl. <ul> <li> Kostenlos und BYOL (Bring Your Own License) </li> </ul> |

## <a name="what-is-an-iot-edge-module"></a>Was ist ein IoT Edge-Modul?

Azure IoT Edge ermöglicht Ihnen die Bereitstellung und Verwaltung von Geschäftslogik im Edge in Form von Modulen. Azure IoT Edge-Module sind die kleinste von IoT Edge verwaltete Recheneinheit und können Microsoft-Dienste (wie Azure Stream Analytics), Dienste von Drittanbietern oder Ihren eigenen lösungsspezifischen Code enthalten. Weitere Informationen zu IoT Edge-Modulen finden Sie unter [Grundlegendes zu Azure IoT Edge-Modulen](../iot-edge/iot-edge-modules.md).

**Was ist der Unterschied zwischen Container-Angebotstyp und einem IoT Edge-Modul-Angebotstyp?**

Beim IoT Edge-Modul-Angebotstyp handelt es sich um einen speziellen Containertyp, der auf einem IoT Edge-Gerät ausgeführt wird. Er wird mit Standardkonfigurationseinstellungen zur Ausführung im IoT Edge-Kontext ausgeliefert und verwendet optional das IoT Edge-Modul SDK zur Integration in die IoT Edge-Laufzeit.

## <a name="publishing-your-iot-edge-module"></a>Veröffentlichen eines IoT Edge-Moduls

**Auswählen des passenden Onlineshops**

IoT Edge-Module werden nur im Azure Marketplace veröffentlicht, in AppSource sind sie nicht verfügbar. Weitere Informationen zu den Unterschieden der jeweiligen Onlineshops finden Sie unter [Bestimmung Ihrer Veröffentlichungsoption](determine-your-listing-type.md).

**Abrechnungsoptionen**

Derzeit unterstützt der Marketplace **kostenlose** und an eigene Lizenzen gebundene Abrechnungsoptionen (**BYOL, Bring Your Own License**) für IoT Edge-Module.

### <a name="publishing-options"></a>Veröffentlichungsoptionen

Für IoT Edge-Module sollten Sie jedenfalls die Veröffentlichungsoption **Transact** auswählen.  Weitere Informationen zu Veröffentlichungsoptionen finden Sie unter [Auswählen einer Veröffentlichungsoption](determine-your-listing-type.md).  

## <a name="eligibility-criteria"></a>Wer kann IoT Edge-Module erwerben?

Für IoT Edge-Modul-Angebote gelten die Bestimmungen der Microsoft Azure Marketplace-Vereinbarungen und -Richtlinien.  Darüber hinaus müssen für IoT Edge-Module zusätzliche Voraussetzungen und technische Anforderungen erfüllt sein.  

### <a name="prerequisites"></a>Voraussetzungen

Wenn Sie ein IoT Edge-Modul im Azure Marketplace veröffentlichen möchten, müssen Sie folgende Voraussetzungen erfüllen:

- Sie benötigen Zugriff auf Partner Center. Weitere Informationen finden Sie unter [Erstellen eines Kontos im kommerziellen Marketplace in Partner Center](create-account.md).
- Sie müssen Ihr IoT Edge-Modul in einer Azure Container Registry hosten.
- Sie müssen unter anderem folgende Metadaten Ihres IoT Edge-Moduls bereithalten:
    - Titel
    - Beschreibung (im HTML-Format)
    - Logobild (in den Größen 48 × 48 (optional), 90 × 90 (optional) und von 216 × 216 bis 350 × 350 Pixel, alle im PNG-Format)
    - Nutzungsbedingungen und Datenschutzrichtlinie
    - Standardmodulkonfiguration (Route, gewünschte Eigenschaften von Modulzwillingen, createOptions, Umgebungsvariable)
    - Dokumentation
    - Supportkontakte

### <a name="technical-requirements"></a>Technische Anforderungen

Die wichtigsten technischen Voraussetzungen für die Zertifizierung und Veröffentlichung eines IoT Edge-Moduls im Azure Marketplace finden Sie unter [Vorbereiten der technischen Ressourcen für das IoT Edge-Modul](./partner-center-portal/create-iot-edge-module-asset.md).

## <a name="next-steps"></a>Nächste Schritte

- melden Sie sich bei [Partner Center](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership) an, um Ihr Angebot zu erstellen oder abzuschließen.
- [Erstellen eines IoT Edge-Modulangebots](./partner-center-portal/azure-iot-edge-module-creation.md) in Partner Center
