---
title: Übersicht über das Veröffentlichen von Azure IoT Edge-Modulangeboten | Azure Marketplace
description: Übersicht über die Vorgehensweise zum Veröffentlichen eines IoT Edge-Modulangebots im Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: dsindona
ms.openlocfilehash: 20c76cec82944568c1b16694bef2838626b90b03
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "80983346"
---
# <a name="iot-edge-module-offer-publishing-overview"></a>Übersicht über das Veröffentlichen von IoT Edge-Modulangeboten

>[!Important]
>Ab dem 13. April 2020 beginnen wir mit der Umstellung der Verwaltung Ihrer IoT Edge-Modulangebote auf Partner Center. Nach der Migration erstellen und verwalten Sie Ihre Angebote in Partner Center. Befolgen Sie zum Verwalten Ihrer migrierten Angebote die Anweisungen unter [Erstellen eines IoT Edge-Modulangebots](https://aka.ms/AzureCreateIoT).

<table> <tr> <td>In diesem Abschnitt wird beschrieben, wie Sie ein neues Angebot für ein Azure IoT Edge-Modul auf dem <a href="https://azuremarketplace.microsoft.com">Azure Marketplace</a> von Microsoft veröffentlichen. Ein IoT Edge-Modul ist ein Docker-kompatibler Container, der zur Ausführung auf einem IoT Edge-Gerät erstellt wurde. Azure IoT Edge-Module stellen die kleinste Berechnungseinheit dar, die mit IoT Edge verwaltet wird, und können Azure-Dienste oder benutzerdefinierten Lösungscode enthalten. </td> <td><img src="./media/iotedge-icon1.png"  alt="Azure IoT Edge module icon" /></td> </tr> </table>

## <a name="publishing-process"></a>Veröffentlichungsvorgang

Die Veröffentlichung eines IoT Edge-Modulangebots umfasst ganz allgemein folgende Schritte:

1. Erstellen des Angebots<br> Geben Sie detaillierte Angebotsinformationen an. Hierzu zählen unter anderem die Angebotsbeschreibung, Marketingmaterial, Supportinformationen und Ressourcenspezifikationen.

2. Erstellen der geschäftlichen und technischen Ressourcen<br> Erstellen Sie die geschäftlichen Ressourcen (rechtlich relevante Dokumente und Marketingmaterial) und die technischen Ressourcen für die zugeordnete Lösung (die in einer Azure Container Registry-Instanz gehosteten IoT Edge-Modulimages).

3. Erstellen der SKU(s)<br> Erstellen Sie die dem Angebot zugeordneten SKUs. Für jedes Image, das Sie veröffentlichen möchten, ist eine eindeutige SKU erforderlich.

4. Zertifizieren und Veröffentlichen des Angebots <br>Nach Fertigstellung der Angebotsressourcen und der technischen Ressourcen kann das Angebot übermittelt werden. Durch diese Übermittlung wird der Veröffentlichungsprozess gestartet. Im Rahmen dieses Prozesses wird die Lösung getestet, überprüft, zertifiziert und anschließend im Azure Marketplace live geschaltet.

## <a name="parts-of-an-offer"></a>Komponenten eines Angebots

In den folgenden Artikeln werden die wichtigsten Bestandteile eines IoT Edge-Modulangebots behandelt.

- [Voraussetzungen](./cpp-prerequisites.md) <br>In diesem Artikel erfahren Sie, welche technischen und geschäftlichen Anforderungen erfüllt werden müssen, um ein IoT Edge-Modulangebot erstellen oder veröffentlichen zu können.
- [Vorbereiten der technischen Ressourcen für das IoT Edge-Modul](./cpp-create-technical-assets.md) <br>In diesem Artikel erfahren Sie, wie Sie die technischen Ressourcen für ein IoT Edge-Modul vorbereiten. Diese Ressourcen müssen jeweils die erforderlichen technischen Kriterien erfüllen, damit das IoT Edge-Modul im Azure Marketplace veröffentlicht werden kann.
- [Erstellen eines IoT Edge-Modulangebots](./cpp-create-offer.md) <br>In diesem Artikel werden die Schritte aufgelistet, die ausgeführt werden müssen, um über das [Cloud-Partnerportal](https://cloudpartner.azure.com) ein neues IoT Edge Modulangebot zu erstellen.
- [Veröffentlichen eines IoT Edge-Modulangebots](./cpp-publish-offer.md)<br> In diesem Artikel erfahren Sie, wie Sie das Angebot zur Veröffentlichung im Azure Marketplace übermitteln.

## <a name="next-steps"></a>Nächste Schritte

Überprüfen Sie die [technischen und geschäftlichen Anforderungen](./cpp-prerequisites.md) für die Veröffentlichung eines IoT Edge-Moduls im Microsoft Azure Marketplace.
