---
title: Vorbereiten einer Änderung der IP-Adresse für ausgehenden Datenverkehr
description: Erfahren Sie, wie Sie vorgehen, wenn Ihre ausgehende IP-Adresse geändert werden soll, damit Ihre App nach der Änderung weiterhin funktioniert.
ms.topic: article
ms.date: 06/28/2018
ms.custom: seodec18
ms.openlocfilehash: 2be4bc92dde278b054bd04f412f937440027ece7
ms.sourcegitcommit: 265f1d6f3f4703daa8d0fc8a85cbd8acf0a17d30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2019
ms.locfileid: "74671665"
---
# <a name="how-to-prepare-for-an-outbound-ip-address-change"></a>Vorbereitung auf eine Änderung der ausgehenden IP-Adresse

Wenn Sie eine Benachrichtigung erhalten haben, dass die ausgehenden IP-Adressen Ihrer Azure App Service-App geändert werden, befolgen Sie die Anweisungen in diesem Artikel.

## <a name="determine-if-you-have-to-do-anything"></a>Ermitteln, ob Aktionen erforderlich sind

* Option 1: Wenn in Ihrer App Service-App keine IP-Filterung, keine explizite Aufnahmeliste und keine spezielle Verarbeitung des ausgehenden Datenverkehrs (z.B. Routing oder eine Firewall) verwendet werden, ist keine Aktion erforderlich.

* Option 2: Wenn für Ihre App eine spezielle Verarbeitung für die ausgehenden IP-Adressen festgelegt ist (siehe Beispiele unten), fügen Sie die neuen ausgehenden IP-Adressen überall dort ein, wo die vorhandenen angezeigt werden. Ersetzen Sie die vorhandenen IP-Adressen nicht. Die neuen ausgehenden IP-Adressen können Sie anhand der Anweisungen im nächsten Abschnitt suchen.

  Eine ausgehende IP-Adresse kann beispielsweise explizit in einer Firewall außerhalb der App enthalten sein, oder ein externer Zahlungsdienst kann eine Liste zugelassener IP-Adressen festgelegt haben, die die ausgehende IP-Adresse für Ihre App enthält. Wenn die ausgehende IP-Adresse in einer Liste außerhalb Ihrer App konfiguriert ist, muss dies geändert werden.

## <a name="find-the-outbound-ip-addresses-in-the-azure-portal"></a>Suchen der ausgehenden IP-Adressen im Azure-Portal

Die neuen ausgehenden IP-Adressen werden im Portal angezeigt, bevor sie wirksam werden. Wenn die neuen IP-Adressen in Azure verwendet werden, werden die alten IP-Adressen nicht mehr verwendet. Es wird jeweils nur ein Satz von IP-Adressen verwendet. Die Einträge in Aufnahmelisten müssen daher sowohl die alten als auch die neuen IP-Adressen enthalten, um beim Wechsel einen Ausfall zu verhindern. 

1.  Öffnen Sie das [Azure-Portal](https://portal.azure.com).

2.  Wählen Sie im linken Navigationsmenü die Option **App Services** aus.

3.  Wählen Sie Ihre App Service-App in der Liste aus.

1.  Wenn es sich bei der App um eine Funktions-App handelt, finden Sie weitere Informationen unter [Ausgehende IP-Adressen einer Funktions-App](../azure-functions/ip-addresses.md#find-outbound-ip-addresses).

4.  Klicken Sie unter der Überschrift **Einstellungen** im linken Navigationsbereich auf **Eigenschaften**, und suchen Sie den Abschnitt mit der Bezeichnung **Ausgehende IP-Adressen**.

5. Kopieren Sie die IP-Adressen, und fügen Sie sie der speziellen Verarbeitung des ausgehenden Datenverkehrs (z.B. einem Filter oder einer Aufnahmeliste) hinzu. Löschen Sie die vorhandenen IP-Adressen in der Liste nicht.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde erläutert, wie Sie sich auf eine Änderung der IP-Adresse vorbereiten, die von Azure veranlasst wurde. Weitere Informationen zu IP-Adressen in Azure App Service finden Sie unter [Ein- und ausgehende IP-Adressen in Azure App Service](overview-inbound-outbound-ips.md).
