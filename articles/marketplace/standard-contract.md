---
title: Standardvertrag | Azure
description: Standardvertrag für Azure Marketplace und AppSource
services: Azure, Marketplace, Compute, Storage, Networking
author: ChJenk
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
ms.date: 11/19/2019
ms.author: v-chjen
ms.openlocfilehash: dc8edf2b6e4a1204e7edd515da9323896049eb13
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74228205"
---
# <a name="standard-contract-for-the-microsoft-commercial-marketplace"></a>Standardvertrag für den kommerziellen Microsoft-Marketplace

Zur Vereinfachung des Beschaffungsprozesses für Kunden und zur Verringerung der rechtlichen Komplexität für Softwareanbieter stellt Microsoft die Vorlage „Standardvertrag“ für den kommerziellen Microsoft-Marketplace bereit, um eine Transaktion im Marketplace zu erleichtern. Statt benutzerdefinierte Geschäftsbedingungen zu erstellen, können Herausgeber im kommerziellen Marketplace ihre Software wahlweise unter dem Standardvertrag anbieten, den Kunden nur einmal überprüfen und akzeptieren müssen. Der Standardvertrag ist hier zu finden: [https://go.microsoft.com/fwlink/?linkid=2041178](https://go.microsoft.com/fwlink/?linkid=2041178).

Die Geschäftsbedingungen für ein Angebot werden bei dessen Erstellung im Partner Center oder im Cloud-Partnerportal definiert. Sie können auswählen, ob Sie den Standardvertrag für den kommerziellen Microsoft-Marketplace verwenden möchten, anstatt Ihre eigenen benutzerdefinierten Geschäftsbedingungen bereitzustellen.

>[!Note]
>Nachdem Sie ein Angebot über den Standardvertrag für den kommerziellen Microsoft-Marketplace veröffentlicht haben, können Sie Ihre eigenen benutzerdefinierten Geschäftsbedingungen nicht mehr nutzen. Es handelt sich um ein „oder“-Szenario. Entweder bieten Sie Ihre Lösung im Rahmen des Standardvertrags *oder* Ihrer eigenen Geschäftsbedingungen an. Wenn Sie die Bedingungen des Standardvertrags ändern möchten, können Sie dies über Zusatzvereinbarungen für den Standardvertrag erreichen.

## <a name="standard-contract-amendments"></a>Zusatzvereinbarungen für den Standardvertrag

Mithilfe von Zusatzvereinbarungen für den Standardvertrag können Herausgeber der Einfachheit halber den Standardvertrag auswählen und mit angepassten Bedingungen für ihr Produkt oder Unternehmen versehen. Kunden, die bereits den Microsoft-Standardvertrag geprüft und akzeptiert haben, müssen lediglich die Zusatzvereinbarungen zum Vertrag prüfen.

Für kommerzielle Marketplace-Herausgeber stehen zwei Arten von Zusatzvereinbarungen zur Verfügung:

* Universelle Zusatzvereinbarungen: Diese Zusatzvereinbarungen werden universell auf den Standardvertrag für alle Kunden angewendet. Universelle Zusatzvereinbarungen werden jedem Kunden des Angebots im Rahmen der Kaufabwicklung angezeigt. Kunden müssen die Bedingungen des Standardvertrags und die Zusatzvereinbarungen akzeptieren, bevor sie Ihr Angebot nutzen können.

* Benutzerdefinierte Zusatzvereinbarungen: Diese Zusatzvereinbarungen sind spezielle Zusatzvereinbarungen für den Standardvertrag, die über Azure-Mandanten-IDs nur bestimmte Kunden betreffen. Herausgeber können den gewünschten Mandanten auswählen. Nur den Kunden des Mandanten werden die Bedingungen der Zusatzvereinbarungen im Rahmen der Kaufabwicklung für das Angebot präsentiert.  Kunden müssen die Bedingungen des Standardvertrags und die Zusatzvereinbarung(en) akzeptieren, bevor sie Ihr Angebot nutzen können.

>[!Note]
>Diese beiden Arten von Zusatzvereinbarungen sind übereinander angeordnet. Kunden, für die benutzerdefinierte Zusatzvereinbarungen gelten, erhalten beim Kauf auch die universellen Zusatzvereinbarung für den Standardvertrag.

Sie können den Standardvertrag für den kommerziellen Microsoft-Marketplace für die folgenden Angebotstypen nutzen:  Azure-Anwendungen (Lösungsvorlagen und verwaltete Anwendungen), virtuelle Computer, Container, Containeranwendungen, IoT Edge-Module und SaaS.

## <a name="customer-experience"></a>Benutzerfreundlichkeit

Während des Ermittlungsvorgangs im Azure-Marketplace oder in der AppSource können Kunden die mit dem Angebot verbundenen Bedingungen als Standardvertrag für den kommerziellen Microsoft-Marketplace und alle universellen Zusatzvereinbarungen einsehen.

![Ermittlungserlebnis der Kunden im Azure-Portal.](media/marketplace-publishers-guide/azure-discovery-process.png)

Während der Kaufabwicklung im Azure-Portal können Kunden die mit dem Angebot verbundenen Bedingungen als Standardvertrag für den kommerziellen Microsoft-Marketplace und alle universellen und/oder mandantenabhängigen Änderungen einsehen.

![Kauferlebnis der Kunden im Azure-Portal.](media/marketplace-publishers-guide/azure-purchase-process.png)

## <a name="api"></a>API

Kunden können die Bedingungen eines Angebots mithilfe von „Get-AzureRmMarketplaceTerms“ abrufen und akzeptieren. Der Standardvertrag und die zugehörigen Zusatzvereinbarungen werden in der Ausgabe des Cmdlets zurückgegeben.
