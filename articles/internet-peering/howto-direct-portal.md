---
title: Erstellen oder Ändern einer Instanz für Direct Peering über das Azure-Portal
titleSuffix: Azure
description: Erstellen oder Ändern einer Instanz für Direct Peering über das Azure-Portal
services: internet-peering
author: derekolo
ms.service: internet-peering
ms.topic: how-to
ms.date: 5/19/2020
ms.author: derekol
ms.openlocfilehash: e49462612b58163c2ac51b78584761d0d8b8bd06
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84700564"
---
# <a name="create-or-modify-a-direct-peering-by-using-the-azure-portal"></a>Erstellen oder Ändern einer Instanz für Direct Peering über das Azure-Portal

Dieser Artikel erläutert, wie Sie im Azure-Portal ein Microsoft Direct Peering für einen Internetdienstanbieter oder einen Internet Exchange-Anbieter erstellen. Der Artikel zeigt auch, wie Sie den Status der Ressource prüfen und die Ressource aktualisieren, löschen oder ihre Bereitstellung aufheben.

Falls Sie es vorziehen, können Sie diese Anleitung auch mithilfe von Azure [PowerShell](howto-direct-powershell.md) ausführen.

## <a name="before-you-begin"></a>Voraussetzungen
* Lesen Sie vor dem Konfigurieren die Informationen zu den [Voraussetzungen](prerequisites.md) sowie die [Exemplarische Vorgehensweise für das Direct Peering](walkthrough-direct-all.md).
* Falls Sie bereits Direct Peering-Verbindungen mit Microsoft eingerichtet haben, die nicht in Azure-Ressourcen konvertiert werden, lesen Sie die Informationen unter [Konvertieren eines Legacy-Direct Peerings in eine Azure-Ressource mithilfe des Portals](howto-legacy-direct-portal.md).

## <a name="create-and-provision-a-direct-peering"></a>Erstellen und Bereitstellen eines direkten Peerings

### <a name="sign-in-to-the-portal-and-select-your-subscription"></a>Melden Sie sich beim Portal an, und wählen Sie Ihr Abonnement aus.
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="create-a-direct-peering"></a><a name=create></a>Erstellen eines direkten Peerings

Als Internetdienstanbieter oder Internet Exchange-Anbieter können Sie durch [Erstellen eines Peerings]( https://go.microsoft.com/fwlink/?linkid=2129593) eine neue Anforderung für ein Direct Peering erstellen.

1. Füllen Sie auf der Seite **Peering erstellen** auf der Registerkarte **Grundlagen** die Felder wie folgt aus:


    ![Registrieren von Peering Service](./media/setup-basics-tab.png)

2. Wählen Sie Ihr Azure-Abonnement aus.

3. Als Ressourcengruppe können Sie in der Dropdownliste eine vorhandene Ressourcengruppe auswählen oder eine neue Gruppe erstellen, indem Sie „Neu erstellen“ auswählen. Im Rahmen dieses Beispiels erstellen Sie eine neue Ressourcengruppe.

4. Der Name entspricht dem Ressourcennamen und ist frei wählbar.

5. Die Region wird automatisch ausgewählt, wenn Sie eine vorhandene Ressourcengruppe ausgewählt haben. Wenn Sie eine neue Ressourcengruppe erstellen möchten, müssen Sie auch die Azure-Region auswählen, in der sich die Ressource befinden soll.

    >[!NOTE]
    > Die Region, in der sich die Ressourcengruppe befindet, ist unabhängig vom Standort, an dem Sie das Peering mit Microsoft erstellen möchten. Es empfiehlt sich jedoch, Ihre Peeringressourcen innerhalb von Ressourcengruppen zu organisieren, die sich in nahegelegenen Azure-Regionen befinden. Beispiel: Für Peerings in Ashburn können Sie eine Ressourcengruppe in „USA, Osten“ oder in „USA, Osten 2“ erstellen.

6. Wählen Sie Ihre ASN im Feld **PeerASN** aus.

    >[!IMPORTANT]
    >Sie können nur eine ASN mit dem ValidationState „Genehmigt“ auswählen, bevor Sie eine Peeringanforderung übermitteln. Wenn Sie Ihre PeerAsn-Anforderung gerade erst übermittelt haben, warten Sie ca. 12 Stunden, damit die ASN-Zuordnung genehmigt werden kann. Wenn die Genehmigung der von Ihnen ausgewählten ASN noch aussteht, wird eine Fehlermeldung angezeigt. Wenn die benötigte ASN nicht angezeigt wird, überprüfen Sie, ob Sie das richtige Abonnement ausgewählt haben. Ist dies der Fall, überprüfen Sie mithilfe von **[Zuordnen der Peer-ASN zum Azure-Abonnement](https://go.microsoft.com/fwlink/?linkid=2129592)** , ob Sie bereits über eine PeerAsn verfügen.

7. Klicken Sie auf **Weiter: Konfiguration**, um fortzufahren.



    ![Registrieren von Peering Service](./media/setup-direct-basics-filled-tab.png)


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
