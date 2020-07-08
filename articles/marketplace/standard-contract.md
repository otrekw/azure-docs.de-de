---
title: Standardvertrag für den kommerziellen Microsoft-Marketplace
description: Standardvertrag für den Azure Marketplace und AppSource in Partner Center
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: keferna
ms.author: keferna
ms.date: 05/20/2020
ms.openlocfilehash: f7327079e365bcd8a99a8274828f7c14e2337a34
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86121553"
---
# <a name="standard-contract-for-microsoft-commercial-marketplace"></a>Standardvertrag für den kommerziellen Microsoft-Marketplace

Microsoft bietet einen Standardvertrag für den kommerziellen Microsoft-Marketplace an. Dies vereinfacht den Beschaffungsprozess für Kunden, verringert die gesetzliche Komplexität für Softwareanbieter und erleichtert Transaktionen im Marketplace. Anstatt benutzerdefinierte Geschäftsbedingungen zu erstellen, können Sie als Herausgeber im kommerziellen Marketplace Ihre Software wahlweise unter dem [Standardvertrag](https://go.microsoft.com/fwlink/?linkid=2041178) anbieten, den Kunden nur einmal überprüfen und akzeptieren müssen.

Die Geschäftsbedingungen für ein Angebot werden bei dessen Erstellung im Partner Center definiert. Sie können auswählen, ob Sie den Standardvertrag für den kommerziellen Microsoft-Marketplace verwenden möchten, anstatt Ihre eigenen benutzerdefinierten Geschäftsbedingungen bereitzustellen.

>[!Note]
>Nachdem Sie ein Angebot über den Standardvertrag für den kommerziellen Microsoft-Marketplace veröffentlicht haben, können Sie Ihre eigenen benutzerdefinierten Geschäftsbedingungen nicht mehr nutzen. Es handelt sich um ein „oder“-Szenario. Entweder bieten Sie Ihre Lösung im Rahmen des Standardvertrags *oder* Ihrer eigenen Geschäftsbedingungen an. Wenn Sie die Bedingungen des Standardvertrags ändern möchten, können Sie dies über Zusatzvereinbarungen für den Standardvertrag erreichen.

## <a name="standard-contract-amendments"></a>Zusatzvereinbarungen für den Standardvertrag

Mithilfe von Zusatzvereinbarungen für den Standardvertrag können Herausgeber der Einfachheit halber den Standardvertrag auswählen und mit angepassten Bedingungen für ihr Produkt oder Unternehmen versehen. Kunden, die bereits den Microsoft-Standardvertrag geprüft und akzeptiert haben, müssen lediglich die Zusatzvereinbarungen zum Vertrag prüfen.

Für kommerzielle Marketplace-Herausgeber stehen zwei Arten von Zusatzvereinbarungen zur Verfügung:

* Universelle Zusatzvereinbarungen: Diese Zusatzvereinbarungen werden universell auf den Standardvertrag für alle Kunden angewendet. Universelle Zusatzvereinbarungen werden jedem Kunden des Angebots im Rahmen der Kaufabwicklung angezeigt. Kunden müssen die Bedingungen des Standardvertrags und die Zusatzvereinbarungen akzeptieren, bevor sie Ihr Angebot nutzen können.

* Benutzerdefinierte Zusatzvereinbarungen: Diese Zusatzvereinbarungen sind spezielle Zusatzvereinbarungen für den Standardvertrag, die über Azure-Mandanten-IDs nur bestimmte Kunden betreffen. Herausgeber können den gewünschten Mandanten auswählen. Nur den Kunden des Mandanten werden die Bedingungen der Zusatzvereinbarungen im Rahmen der Kaufabwicklung für das Angebot präsentiert.  Kunden müssen die Bedingungen des Standardvertrags und die Zusatzvereinbarung(en) akzeptieren, bevor sie Ihr Angebot nutzen können.

>[!Note]
>Diese beiden Arten von Zusatzvereinbarungen sind übereinander angeordnet. Kunden, für die benutzerdefinierte Zusatzvereinbarungen gelten, erhalten beim Kauf auch die universellen Zusatzvereinbarung für den Standardvertrag.

Sie können den Standardvertrag für den kommerziellen Microsoft-Marketplace für die folgenden Angebotstypen nutzen:  Azure-Anwendungen (Lösungsvorlagen und verwaltete Anwendungen), virtuelle Computer und SaaS.

## <a name="customer-experience"></a>Benutzerfreundlichkeit

Während des Ermittlungsvorgangs im Azure-Marketplace oder in AppSource können Kunden die mit dem Angebot verbundenen Bedingungen als Standardvertrag für den kommerziellen Microsoft-Marketplace und alle universellen Zusatzvereinbarungen einsehen.

![Ermittlungserlebnis der Kunden im Azure-Portal.](media/marketplace-publishers-guide/azure-discovery-process.png)

Während der Kaufabwicklung im Azure-Portal können Kunden die mit dem Angebot verbundenen Bedingungen als Standardvertrag für den kommerziellen Microsoft-Marketplace und alle universellen und/oder mandantenabhängigen Änderungen einsehen.

![Kauferlebnis der Kunden im Azure-Portal.](media/marketplace-publishers-guide/azure-purchase-process.png)

## <a name="api"></a>API

Kunden können die Bedingungen eines Angebots mithilfe von „Get-AzureRmMarketplaceTerms“ abrufen und akzeptieren. Der Standardvertrag und die zugehörigen Zusatzvereinbarungen werden in der Ausgabe des Cmdlets zurückgegeben.
