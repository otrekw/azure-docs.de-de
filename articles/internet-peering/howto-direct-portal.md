---
title: Erstellen oder Ändern einer Instanz für Direct Peering über das Azure-Portal
titleSuffix: Azure
description: Erstellen oder Ändern einer Instanz für Direct Peering über das Azure-Portal
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: dcd6aaf584691005dd071a7aba5958070f598978
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "81681054"
---
# <a name="create-or-modify-a-direct-peering-by-using-the-azure-portal"></a>Erstellen oder Ändern einer Instanz für Direct Peering über das Azure-Portal

In diesem Artikel wird beschrieben, wie Sie über das Azure-Portal ein direktes Microsoft-Peering erstellen. Der Artikel veranschaulicht darüber hinaus, wie Sie den Status der Ressource prüfen und die Ressource aktualisieren, löschen oder ihre Bereitstellung aufheben.

Falls Sie es vorziehen, können Sie diese Anleitung auch mithilfe von Azure [PowerShell](howto-direct-powershell.md) ausführen.

## <a name="before-you-begin"></a>Voraussetzungen
* Lesen Sie vor dem Konfigurieren die Informationen zu den [Voraussetzungen](prerequisites.md) sowie die [Exemplarische Vorgehensweise für das Direct Peering](walkthrough-direct-all.md).
* Falls Sie bereits Direct Peering-Verbindungen mit Microsoft eingerichtet haben, die nicht in Azure-Ressourcen konvertiert werden, lesen Sie die Informationen unter [Konvertieren eines Legacy-Direct Peerings in eine Azure-Ressource mithilfe des Portals](howto-legacy-direct-portal.md).

## <a name="create-and-provision-a-direct-peering"></a>Erstellen und Bereitstellen eines direkten Peerings

### <a name="sign-in-to-the-portal-and-select-your-subscription"></a>Melden Sie sich beim Portal an, und wählen Sie Ihr Abonnement aus.
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="create-a-direct-peering"></a><a name=create></a>Erstellen eines direkten Peerings

Sie können mithilfe der **Peering**ressource eine neue Peeringanforderung erstellen.

#### <a name="launch-resource-and-configure-basic-settings"></a>Starten der Ressource und Konfigurieren grundlegender Einstellungen
[!INCLUDE [direct-peering-basic](./includes/direct-portal-basic.md)]

#### <a name="configure-connections-and-submit"></a>Konfigurieren von Verbindungen und Übermitteln
[!INCLUDE [direct-peering-configuration](./includes/direct-portal-configuration.md)]

### <a name="verify-direct-peering"></a><a name=get></a>Überprüfen des direkten Peerings
[!INCLUDE [peering-direct-get-portal](./includes/direct-portal-get.md)]

## <a name="modify-a-direct-peering"></a><a name="modify"></a>Ändern des direkten Peerings
[!INCLUDE [peering-direct-modify-portal](./includes/direct-portal-modify.md)]

## <a name="deprovision-a-direct-peering"></a><a name="delete"></a>Aufheben der Bereitstellung eines direkten Peerings
[!INCLUDE [peering-direct-delete-portal](./includes/delete.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen oder Ändern einer Instanz für Exchange Peering über das Portal](howto-exchange-portal.md)
* [Konvertieren einer Legacy-Instanz für Exchange Peering in eine Azure-Ressource über das Portal](howto-legacy-exchange-portal.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Weitere Informationen finden Sie unter [Internetpeering: häufig gestellte Fragen](faqs.md).
