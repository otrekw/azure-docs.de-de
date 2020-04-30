---
title: Erstellen eines Azure IoT Edge-Modulangebots | Azure Marketplace
description: Hier erfahren Sie, wie Sie ein neues IoT Edge-Modul für den Marketplace veröffentlichen.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: dsindona
ms.openlocfilehash: d8f4300f45fc626c104055f987d123301ba8f39d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82148064"
---
# <a name="create-a-new-iot-edge-module-offer-with-the-cloud-partner-portal"></a>Erstellen eines neuen IoT Edge-Modulangebots im Cloud-Partnerportal

>[!Important]
>Ab dem 13. April 2020 beginnen wir mit der Umstellung der Verwaltung Ihrer IoT Edge-Modulangebote auf Partner Center. Nach der Migration erstellen und verwalten Sie Ihre Angebote im Partner Center. Befolgen Sie zum Verwalten Ihrer migrierten Angebote die Anweisungen unter [Erstellen eines IoT Edge-Modulangebots](https://docs.microsoft.com//azure/marketplace/partner-center-portal/azure-iot-edge-module-creation).

In diesem Artikel wird beschrieben, wie Sie ein IoT Edge-Modulangebot für den Azure Marketplace erstellen und veröffentlichen. Jedes Angebot wird als eigene Entität im Azure Marketplace angezeigt und mit mindestens einer SKU verknüpft.  Ein IoT Edge-Modulangebot setzt sich aus folgenden Gruppen von Ressourcen und unterstützenden Diensten zusammen:

|  **Ressourcengruppe**   |  **Beschreibung**  |
|  ---------------   |  ---------------  |
|    SKUs            |  Die kleinste bereitstellbare Einheit eines Angebots. Einem einzelnen Angebot (Produktklasse) können mehrere SKUs zugeordnet werden. Mithilfe von SKUs können Sie zwischen unterstützten Funktionen und Abrechnungsmodellen unterscheiden. |
|  Marketplace       | Enthält Ressourcen für Marketing, rechtliche Aspekte und Leadverwaltung sowie entsprechende Spezifikationen.  <ul><li> Zu Marketingressourcen zählen unter anderem der Name des Angebots, eine Beschreibung und Logos.</li> <li> Zu den rechtlichen Ressourcen zählen eine Datenschutzrichtlinie, Nutzungsbedingungen und andere rechtliche Dokumentationen.</li>  <li> Mit der Leadverwaltungsrichtlinie können Sie angeben, wie Leads aus dem Azure Marketplace-Endbenutzerportal verwendet werden sollen.</li> </ul> |
| Support            | Enthält Kontakt- und Richtlinieninformationen. |


## <a name="new-offer-form"></a>Formular „Neues Angebot“ 

Melden Sie sich beim [Cloud-Partnerportal](https://cloudpartner.azure.com/) an, und wählen Sie dann auf der linken Menüleiste **+ Neues Angebot** aus. Wählen Sie im Menü „Neues Angebot“ die Option **IoT Edge-Module** aus, um das Formular **Neues Angebot** anzuzeigen und dann mit dem Definieren von Ressourcen für ein neues IoT Edge-Modulangebot zu beginnen. 

![Neues IoT Edge-Modulangebot: Auswahl auf der Benutzeroberfläche](./media/new-iot-edge-module-offer.png)

## <a name="next-steps"></a>Nächste Schritte

Die Seite **Neues Angebot** für den IoT Edge-Modul-Angebotstyp umfasst eine Reihe von Registerkarten und Formularfeldern zum Erstellen eines neuen Angebots. In den folgenden Artikeln wird erläutert, wie Sie auf der jeweiligen Registerkarte die Ressourcengruppen und unterstützenden Dienste für Ihr neues IoT Edge-Modulangebot definieren.

- [Registerkarte „Angebotseinstellungen“](./cpp-offer-settings-tab.md)
- [Registerkarte „SKUs“](./cpp-skus-tab.md)
- [Registerkarte „Marketplace“](./cpp-marketplace-tab.md)
- [Registerkarte „Support“](./cpp-support-tab.md)
