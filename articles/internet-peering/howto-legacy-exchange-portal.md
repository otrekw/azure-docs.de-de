---
title: Konvertieren einer Legacy-Instanz für Exchange Peering in eine Azure-Ressource über das Azure-Portal
titleSuffix: Azure
description: Konvertieren einer Legacy-Instanz für Exchange Peering in eine Azure-Ressource über das Azure-Portal
services: internet-peering
author: derekolo
ms.service: internet-peering
ms.topic: how-to
ms.date: 5/21/2020
ms.author: derekol
ms.openlocfilehash: 6249d68486af754f2d2980aaed9d5fd8287dcb5e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "84700160"
---
# <a name="convert-a-legacy-exchange-peering-to-an-azure-resource-by-using-the-azure-portal"></a>Konvertieren einer Legacy-Instanz für Exchange Peering in eine Azure-Ressource über das Azure-Portal

In diesem Artikel wird beschrieben, wie Sie mithilfe des Azure-Portals ein vorhandenes Legacy-Exchange Peering in eine Azure-Ressource konvertieren.

Falls Sie es vorziehen, können Sie diese Anleitung auch mithilfe von [PowerShell](howto-legacy-exchange-powershell.md) ausführen.

## <a name="before-you-begin"></a>Voraussetzungen
* Lesen Sie vor dem Konfigurieren die Informationen zu den [Voraussetzungen](prerequisites.md) sowie die [Exemplarische Vorgehensweise für das Exchange Peering](walkthrough-exchange-all.md).

## <a name="convert-a-legacy-exchange-peering-to-an-azure-resource"></a>Konvertieren einer Legacy-Instanz für Exchange Peering in eine Azure-Ressource

Als Internet Exchange-Anbieter können Sie durch [Erstellen eines Peerings]( https://go.microsoft.com/fwlink/?linkid=2129593) eine Anforderung für ein Exchange Peering erstellen.

1. Füllen Sie auf der Seite **Peering erstellen** auf der Registerkarte **Grundlagen** die Felder wie hier gezeigt aus:

   ![Registrieren von Peering Service](./media/setup-basics-tab.png)

* Wählen Sie Ihr Azure-Abonnement aus.

* Als Ressourcengruppe können Sie in der Dropdownliste eine vorhandene Ressourcengruppe auswählen oder eine neue Gruppe erstellen, indem Sie „Neu erstellen“ auswählen. Im Rahmen dieses Beispiels erstellen Sie eine neue Ressourcengruppe.

* Der Name entspricht dem Ressourcennamen und ist frei wählbar.

* Die Region wird automatisch ausgewählt, wenn Sie eine vorhandene Ressourcengruppe ausgewählt haben. Wenn Sie eine neue Ressourcengruppe erstellen möchten, müssen Sie auch die Azure-Region auswählen, in der sich die Ressource befinden soll.

  >[!NOTE]
  >Die Region, in der sich die Ressourcengruppe befindet, ist unabhängig vom Standort, an dem Sie das Peering mit Microsoft erstellen möchten. Es empfiehlt sich jedoch, Ihre Peeringressourcen innerhalb von Ressourcengruppen zu organisieren, die sich in nahegelegenen Azure-Regionen befinden. Beispiel: Für Peerings in Ashburn können Sie eine Ressourcengruppe in „USA, Osten“ oder in „USA, Osten 2“ erstellen.

* Wählen Sie Ihre ASN im Feld **PeerASN** aus.

  >[!IMPORTANT]  
  >Sie können nur eine ASN mit dem ValidationState „Genehmigt“ auswählen, bevor Sie eine Peeringanforderung übermitteln. Wenn Sie Ihre PeerAsn-Anforderung gerade erst übermittelt haben, warten Sie ca. 12 Stunden, damit die ASN-Zuordnung genehmigt werden kann. Wenn die Genehmigung der von Ihnen ausgewählten ASN noch aussteht, wird eine Fehlermeldung angezeigt. Wenn die benötigte ASN nicht angezeigt wird, überprüfen Sie, ob Sie das richtige Abonnement ausgewählt haben. Ist dies der Fall, überprüfen Sie mithilfe von **[Zuordnen der Peer-ASN zum Azure-Abonnement](https://go.microsoft.com/fwlink/?linkid=2129592)** , ob Sie PeerAsn bereits erstellt haben.

* Klicken Sie auf **Weiter: Konfiguration**, um fortzufahren.


#### <a name="configure-connections-and-submit"></a>Konfigurieren von Verbindungen und Übermitteln
[!INCLUDE [exchange-peering-configuration](./includes/exchange-portal-configuration-legacy.md)]

### <a name="verify-exchange-peering"></a><a name=get></a>Überprüfen von Exchange Peering
[!INCLUDE [peering-exchange-get-portal](./includes/exchange-portal-get.md)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Weitere Informationen finden Sie unter [Internetpeering: häufig gestellte Fragen](faqs.md).

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen oder Ändern einer Instanz für Exchange Peering über das Portal](howto-exchange-portal.md)
