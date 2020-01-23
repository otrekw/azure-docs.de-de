---
title: Erstellen oder Ändern eines Austauschpeerings über das Portal
titleSuffix: Azure
description: Erstellen oder Ändern eines Austauschpeerings über das Portal
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 2c186decf68d167ab2c5ab7696c2dfb51d77a071
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75773749"
---
# <a name="create-or-modify-an-exchange-peering-using-the-portal"></a>Erstellen oder Ändern eines Austauschpeerings über das Portal

In diesem Artikel wird beschrieben, wie Sie über das Portal ein Microsoft-Austauschpeering erstellen. Der Artikel veranschaulicht auch, wie Sie den Status der Ressource prüfen und die Ressource aktualisieren, löschen oder ihre Bereitstellung aufheben.

Falls Sie es vorziehen, können Sie diese Anleitung auch mithilfe von [PowerShell](howto-exchange-powershell.md) ausführen.

## <a name="before-you-begin"></a>Voraussetzungen
* Lesen Sie vor dem Konfigurieren die Informationen zu den [Voraussetzungen](prerequisites.md) sowie die [exemplarische Vorgehensweise für das Austauschpeering](walkthrough-exchange-all.md).
* Falls Sie bereits Microsoft-Austauschpeerings eingerichtet haben, die nicht in Azure-Ressourcen konvertiert werden, lesen Sie die Informationen unter [Konvertieren eines älteren Austauschpeerings in eine Azure-Ressource über das Portal](howto-legacy-exchange-portal.md).

## <a name="create-and-provision-an-exchange-peering"></a>Erstellen und Bereitstellen eines Austauschpeerings

### <a name="sign-in-to-portal-and-select-your-subscription"></a>Melden Sie sich beim Portal an, und wählen Sie Ihr Abonnement aus.
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name=create></a>Erstellen eines Austauschpeerings

Sie können mithilfe einer **Peeringressource** eine neue Peeringanforderung erstellen.

#### <a name="launch-resource-and-configure-basic-settings"></a>Starten der Ressource und Konfigurieren grundlegender Einstellungen
[!INCLUDE [direct-peering-basic](./includes/direct-portal-basic.md)]

#### <a name="configure-connections-and-submit"></a>Konfigurieren von Verbindungen und Übermitteln
[!INCLUDE [exchange-peering-configuration](./includes/exchange-portal-configuration.md)]

### <a name=get></a>Überprüfen eines Austauschpeerings
[!INCLUDE [peering-exchange-get-portal](./includes/exchange-portal-get.md)]

## <a name="modify"></a>Ändern eines Austauschpeerings
[!INCLUDE [peering-exchange-modify-portal](./includes/exchange-portal-modify.md)]

## <a name="delete"></a>Aufheben der Bereitstellung eines Austauschpeerings
[!INCLUDE [peering-exchange-delete-portal](./includes/delete.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen oder Ändern eines direkten Peerings über das Portal](howto-direct-portal.md)
* [Konvertieren eines älteren direkten Peerings in eine Azure-Ressource über das Portal](howto-legacy-direct-portal.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Weitere Informationen finden Sie unter [Internetpeering: häufig gestellte Fragen](faqs.md).
