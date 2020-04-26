---
title: Erstellen oder Ändern einer Instanz für Exchange Peering über das Azure-Portal
titleSuffix: Azure
description: Erstellen oder Ändern einer Instanz für Exchange Peering über das Azure-Portal
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: e4f2ee72cbe17c094567aab5c7cc4720b02cde68
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "81680950"
---
# <a name="create-or-modify-an-exchange-peering-by-using-the-azure-portal"></a>Erstellen oder Ändern einer Instanz für Exchange Peering über das Azure-Portal

In diesem Artikel wird beschrieben, wie Sie über das Azure-Portal ein Microsoft-Austauschpeering erstellen. Der Artikel veranschaulicht darüber hinaus, wie Sie den Status der Ressource prüfen und die Ressource aktualisieren, löschen oder ihre Bereitstellung aufheben.

Falls Sie es vorziehen, können Sie diese Anleitung auch mithilfe von [PowerShell](howto-exchange-powershell.md) ausführen.

## <a name="before-you-begin"></a>Voraussetzungen
* Lesen Sie vor dem Konfigurieren die Informationen zu den [Voraussetzungen](prerequisites.md) sowie die [Exemplarische Vorgehensweise für das Exchange Peering](walkthrough-exchange-all.md).
* Falls Sie bereits Exchange Peerings mit Microsoft eingerichtet haben, die nicht in Azure-Ressourcen konvertiert werden, lesen Sie die Informationen unter [Konvertieren eines Legacy-Exchange Peerings in eine Azure-Ressource mithilfe des Portals](howto-legacy-exchange-portal.md).

## <a name="create-and-provision-an-exchange-peering"></a>Erstellen und Bereitstellen eines Exchange Peerings

### <a name="sign-in-to-the-portal-and-select-your-subscription"></a>Melden Sie sich beim Portal an, und wählen Sie Ihr Abonnement aus.
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="create-an-exchange-peering"></a><a name=create></a>Erstellen eines Exchange Peerings

Sie können mithilfe der **Peering**ressource eine neue Peeringanforderung erstellen.

#### <a name="launch-the-resource-and-configure-basic-settings"></a>Starten der Ressource und Konfigurieren grundlegender Einstellungen
[!INCLUDE [direct-peering-basic](./includes/direct-portal-basic.md)]

#### <a name="configure-connections-and-submit"></a>Konfigurieren von Verbindungen und Übermitteln
[!INCLUDE [exchange-peering-configuration](./includes/exchange-portal-configuration.md)]

### <a name="verify-an-exchange-peering"></a><a name=get></a>Überprüfen eines Austauschpeerings
[!INCLUDE [peering-exchange-get-portal](./includes/exchange-portal-get.md)]

## <a name="modify-an-exchange-peering"></a><a name="modify"></a>Ändern eines Austauschpeerings
[!INCLUDE [peering-exchange-modify-portal](./includes/exchange-portal-modify.md)]

## <a name="deprovision-an-exchange-peering"></a><a name="delete"></a>Aufheben der Bereitstellung eines Exchange Peerings
[!INCLUDE [peering-exchange-delete-portal](./includes/delete.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen oder Ändern einer Instanz für Direct Peering über das Portal](howto-direct-portal.md)
* [Konvertieren einer älteren Instanz für Direct Peering in eine Azure-Ressource mit dem Portal](howto-legacy-direct-portal.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Weitere Informationen finden Sie unter [Internetpeering: häufig gestellte Fragen](faqs.md).
