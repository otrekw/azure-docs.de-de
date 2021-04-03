---
title: Konvertieren einer Legacyinstanz für direktes Peering in eine Azure-Ressource über das Azure-Portal
titleSuffix: Azure
description: Konvertieren einer Legacyinstanz für direktes Peering in eine Azure-Ressource über das Azure-Portal
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: how-to
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 496774f034c861131624b552b1acefca9f399ea8
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "84700262"
---
# <a name="convert-a-legacy-direct-peering-to-an-azure-resource-by-using-the-azure-portal"></a>Konvertieren einer Legacyinstanz für direktes Peering in eine Azure-Ressource über das Azure-Portal

In diesem Artikel wird beschrieben, wie Sie mithilfe des Azure-Portals eine vorhandene Instanz für direktes Peering in eine Azure-Ressource konvertieren.

Sie können diese Anleitung auch mithilfe von [PowerShell](howto-legacy-direct-powershell.md) ausführen.

## <a name="before-you-begin"></a>Voraussetzungen
* Informieren Sie sich vor der Konfiguration zunächst über die [Voraussetzungen](prerequisites.md), und lesen Sie den Artikel [Exemplarische Vorgehensweise für direktes Peering](walkthrough-direct-all.md).


## <a name="convert-a-legacy-direct-peering-to-an-azure-resource"></a>Konvertieren einer Legacyinstanz für direktes Peering in eine Azure-Ressource

### <a name="sign-in-to-the-portal-and-select-your-subscription"></a>Melden Sie sich beim Portal an, und wählen Sie Ihr Abonnement aus.
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="convert-a-legacy-direct-peering"></a><a name=create></a> Konvertieren einer Legacyinstanz für direktes Peering

Als Internetdienstanbieter können Sie ältere direkte Peeringverbindungen mithilfe von [Creating a Peering]( https://go.microsoft.com/fwlink/?linkid=2129593) (Erstellen eines Peerings) konvertieren.

1. Füllen Sie auf der Seite **Create a Peering** (Peering erstellen) auf der Registerkarte **Grundlagen** die Felder wie hier gezeigt aus:

    > [!div class="mx-imgBorder"] 
    > ![Registrieren eines Peering Service](./media/setup-basics-tab.png)

*    Wählen Sie Ihr Azure-Abonnement aus.

* Als Ressourcengruppe können Sie in der Dropdownliste eine vorhandene Ressourcengruppe auswählen oder eine neue Gruppe erstellen, indem Sie „Neu erstellen“ auswählen. Im Rahmen dieses Beispiels erstellen Sie eine neue Ressourcengruppe.

* Der Name entspricht dem Ressourcennamen und ist frei wählbar.

* Die Region wird automatisch ausgewählt, wenn Sie eine vorhandene Ressourcengruppe ausgewählt haben. Wenn Sie eine neue Ressourcengruppe erstellen möchten, müssen Sie auch die Azure-Region auswählen, in der sich die Ressource befinden soll.

>[!NOTE]
>Die Region, in der sich die Ressourcengruppe befindet, ist unabhängig vom Standort, an dem Sie das Peering mit Microsoft erstellen möchten. Es empfiehlt sich jedoch, Ihre Peeringressourcen innerhalb von Ressourcengruppen zu organisieren, die sich in nahegelegenen Azure-Regionen befinden. Beispiel: Für Peerings in Ashburn können Sie eine Ressourcengruppe in „USA, Osten“ oder in „USA, Osten 2“ erstellen.

* Wählen Sie Ihre ASN im Feld **PeerASN** aus.

>[!IMPORTANT] 
>Sie können nur eine ASN mit dem ValidationState „Genehmigt“ auswählen, bevor Sie eine Peeringanforderung übermitteln. Wenn Sie Ihre PeerAsn-Anforderung gerade erst übermittelt haben, warten Sie ca. 12 Stunden, damit die ASN-Zuordnung genehmigt werden kann. Wenn die Genehmigung der von Ihnen ausgewählten ASN noch aussteht, wird eine Fehlermeldung angezeigt. Wenn die benötigte ASN nicht angezeigt wird, überprüfen Sie, ob Sie das richtige Abonnement ausgewählt haben. Ist dies der Fall, überprüfen Sie mithilfe von **[Zuordnen der Peer-ASN zum Azure-Abonnement](https://go.microsoft.com/fwlink/?linkid=2129592)** , ob Sie bereits eine Peer-ASN erstellt haben.

#### <a name="launch-the-resource-and-configure-basic-settings"></a>Starten der Ressource und Konfigurieren grundlegender Einstellungen
[!INCLUDE [direct-peering-basic](./includes/direct-portal-basic.md)]

#### <a name="configure-connections-and-submit"></a>Konfigurieren von Verbindungen und Übermitteln
[!INCLUDE [direct-peering-configuration](./includes/direct-portal-configuration-legacy.md)]

### <a name="verify-direct-peering"></a><a name=get></a>Überprüfen des direkten Peerings
[!INCLUDE [peering-direct-get-portal](./includes/direct-portal-get.md)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Weitere Informationen finden Sie unter [Internetpeering: häufig gestellte Fragen](faqs.md).

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen oder Ändern eines direkten Peerings über das Portal](howto-direct-portal.md)
