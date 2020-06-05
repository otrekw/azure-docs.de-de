---
title: Erstellen oder Ändern einer Instanz für Exchange Peering über das Azure-Portal
titleSuffix: Azure
description: Erstellen oder Ändern einer Instanz für Exchange Peering über das Azure-Portal
services: internet-peering
author: derekolo
ms.service: internet-peering
ms.topic: article
ms.date: 5/2/2020
ms.author: derekol
ms.openlocfilehash: 259ced3032eb43a946de7f1cf4dad9abb99d4a11
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2020
ms.locfileid: "83845237"
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


Als Internet Exchange-Anbieter können Sie durch [Erstellen eines Peerings]( https://go.microsoft.com/fwlink/?linkid=2129593) eine Anforderung für ein Exchange Peering erstellen.

1. Füllen Sie auf der Seite **Peering erstellen** auf der Registerkarte **Grundlagen** die Felder wie hier gezeigt aus:

    > [!div class="mx-imgBorder"] 
    > ![Dienst zum Registrieren eines Peerings](./media/setup-basics-tab.png)

*    Wählen Sie Ihr Azure-Abonnement aus.

* Als Ressourcengruppe können Sie in der Dropdownliste eine vorhandene Ressourcengruppe auswählen oder eine neue Gruppe erstellen, indem Sie auf „Neu erstellen“ klicken. Im Rahmen dieses Beispiels erstellen Sie eine neue Ressourcengruppe.

* Der Name entspricht dem Ressourcennamen und ist frei wählbar.

* Die Region wird automatisch ausgewählt, wenn Sie eine vorhandene Ressourcengruppe ausgewählt haben. Wenn Sie eine neue Ressourcengruppe erstellen möchten, müssen Sie auch die Azure-Region auswählen, in der sich die Ressource befinden soll.

>[!NOTE]
>Die Region, in der sich die Ressourcengruppe befindet, ist unabhängig vom Standort, an dem Sie das Peering mit Microsoft erstellen möchten. Es empfiehlt sich jedoch, Ihre Peeringressourcen innerhalb von Ressourcengruppen zu organisieren, die sich in nahegelegenen Azure-Regionen befinden. Beispiel: Für Peerings in Ashburn können Sie eine Ressourcengruppe in „USA, Osten“ oder in „USA, Osten 2“ erstellen.

* Wählen Sie Ihre ASN im Feld **PeerASN** aus.

>[!IMPORTANT] 
>Sie können nur eine ASN mit dem ValidationState „Genehmigt“ auswählen, bevor Sie eine Peeringanforderung übermitteln. Wenn Sie Ihre PeerAsn-Anforderung gerade erst übermittelt haben, warten Sie ca. 12 Stunden, damit die ASN-Zuordnung genehmigt werden kann. Wenn die Genehmigung der von Ihnen ausgewählten ASN noch aussteht, wird eine Fehlermeldung angezeigt. Wenn die benötigte ASN nicht angezeigt wird, überprüfen Sie, ob Sie das richtige Abonnement ausgewählt haben. Ist dies der Fall, überprüfen Sie mithilfe von **[Zuordnen der Peer-ASN zum Azure-Abonnement](https://go.microsoft.com/fwlink/?linkid=2129592)** , ob Sie bereits über eine PeerAsn verfügen.

* Klicken Sie auf **Weiter: Konfiguration**, um fortzufahren.

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
