---
title: Voraussetzungen für das Einrichten von Peering mit Microsoft
titleSuffix: Azure
description: Voraussetzungen für das Einrichten von Peering mit Microsoft
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: conceptual
ms.date: 12/15/2020
ms.author: prmitiki
ms.openlocfilehash: bc45bc8809eabe75902b602835ea595b56ff3cf8
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "97586839"
---
# <a name="prerequisites-to-set-up-peering-with-microsoft"></a>Voraussetzungen für das Einrichten von Peering mit Microsoft

Stellen Sie sicher, dass die unten aufgeführten Voraussetzungen erfüllt sind, bevor Sie ein neues Peering anfordern oder ein Legacy-Peering in eine Azure-Ressource konvertieren.

## <a name="azure-related-prerequisites"></a>Voraussetzungen für Azure
* **Microsoft Azure-Konto:** Wenn Sie noch kein Microsoft Azure-Konto besitzen, erstellen Sie ein [Microsoft Azure-Konto](https://azure.microsoft.com/free). Ein gültiges und aktives Microsoft Azure-Abonnement ist erforderlich, um Peering einzurichten, da die Peerings als Ressourcen innerhalb von Azure-Abonnements modelliert werden. Folgendes muss unbedingt beachtet werden:
    * Die Azure-Ressourcentypen, die zum Einrichten von Peering verwendet werden, sind zeitlich unbegrenzt kostenlose Azure-Produkte, d. h. Ihnen wird die Erstellung eines Azure-Kontos oder Abonnements oder der Zugriff auf die Azure-Ressourcen **PeerAsn** und **Peering** zum Einrichten von Peering nicht in Rechnung gestellt. Dies ist nicht zu verwechseln mit der Peeringvereinbarung für Direct Peering zwischen Ihnen und Microsoft, deren Bedingungen explizit mit unserem Peering-Team erörtert werden. Wenden Sie sich an [Microsoft-Peering](mailto:peering@microsoft.com), wenn Sie Fragen in dieser Hinsicht haben.
    * Sie können dasselbe Azure-Abonnement für den Zugriff auf andere Azure-Produkte oder Clouddienste verwenden, gleich, ob diese kostenlos oder kostenpflichtig sind. Wenn Sie auf ein kostenpflichtiges Produkt zugreifen, fallen Gebühren an.
    * Wenn Sie ein neues Azure-Konto und/oder Abonnement erstellen, sind Sie möglicherweise zu kostenlosen Azure-Gutschriften während eines Testzeitraums berechtigt, die Sie verwenden dürfen, um Azure-Clouddienste zu testen. Wenn Sie interessiert sind, finden Sie weitere Informationen unter [Microsoft Azure-Konto](https://azure.microsoft.com/free).

* **Peer-ASN zuordnen:** Bevor Sie das Peering anfordern, ordnen Sie zunächst Ihre ASN und Kontaktinformationen Ihrem Abonnement zu. Befolgen Sie die Anweisungen in [Zuordnen der Peer-ASN zum Azure-Abonnement](howto-subscription-association-powershell.md).

## <a name="other-prerequisites"></a>Weitere Voraussetzungen
* **PeeringDB-Profil:** Von Peers wird erwartet, dass sie über ein umfassendes und aktuelles Profil auf [PeeringDB](https://www.peeringdb.com) verfügen. Wir verwenden diese Informationen in unserem Registrierungssystem, um die Detailangaben des Peers zu überprüfen, wie etwa die NOC-Informationen, Informationen zum technischen Kontakt, die Anwesenheit in den Peeringeinrichtungen usw.

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen oder Ändern einer Instanz für Direct Peering über das Portal](howto-direct-portal.md)
* [Konvertieren einer älteren Instanz für Direct Peering in eine Azure-Ressource über das Portal](howto-legacy-direct-portal.md)
* [Erstellen oder Ändern einer Instanz für Exchange Peering über das Portal](howto-exchange-portal.md)
* [Konvertieren einer älteren Instanz für Exchange Peering in eine Azure-Ressource über das Portal](howto-legacy-exchange-portal.md)