---
title: Vorbereiten einer Änderung der IP-Adresse für eingehenden Datenverkehr
description: Erfahren Sie, wie Sie vorgehen, wenn Ihre eingehende IP-Adresse geändert werden soll, damit Ihre App nach der Änderung weiterhin funktioniert.
ms.topic: article
ms.date: 06/28/2018
ms.custom: seodec18
ms.openlocfilehash: 999511c4759ee761f028a61fbfaa0310ac93d9b4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "74672412"
---
# <a name="how-to-prepare-for-an-inbound-ip-address-change"></a>Vorbereitung auf eine Änderung der eingehenden IP-Adresse

Wenn Sie eine Benachrichtigung erhalten haben, dass die eingehende IP-Adresse Ihrer Azure App Service-App geändert wird, befolgen Sie die Anweisungen in diesem Artikel.

## <a name="determine-if-you-have-to-do-anything"></a>Ermitteln, ob Aktionen erforderlich sind

* Option 1: Wenn Ihre App Service-App über keine benutzerdefinierte Domäne verfügt, ist keine Aktion erforderlich.

* Option 2: Wenn in Ihrem Domänenregistrierungsportal (DNS-Drittanbieter oder Azure DNS) nur ein CNAME-Eintrag (DNS-Eintrag, der auf einen URI verweist) konfiguriert ist, ist keine Aktion erforderlich.

* Option 3: Wenn in Ihrem Domänenregistrierungsportal (DNS-Drittanbieter oder Azure DNS) ein A-Datensatz (DNS-Eintrag, der direkt auf Ihre IP-Adresse verweist) konfiguriert ist, ersetzen Sie die vorhandene IP-Adresse durch die neue IP-Adresse. Die neue IP-Adresse können Sie anhand der Anweisungen im nächsten Abschnitt suchen.

* Option 4: Wenn sich Ihre Anwendung hinter einem Load Balancer, IP-Filter oder einem anderen IP-Mechanismus befindet, für den die IP-Adresse der Anwendung erforderlich ist, ersetzen Sie die vorhandene IP-Adresse durch die neue IP-Adresse. Die neue IP-Adresse können Sie anhand der Anweisungen im nächsten Abschnitt suchen.

## <a name="find-the-new-inbound-ip-address-in-the-azure-portal"></a>Suchen der neuen eingehenden IP-Adresse im Azure-Portal

Die neue eingehende IP-Adresse, die Ihrer App zugewiesen wird, wird im Portal im Feld **Virtuelle IP-Adresse** angezeigt. Sowohl diese neue IP-Adresse als auch die alte IP-Adresse sind nun mit Ihrer App verbunden. Später wird die Verbindung mit der alten getrennt.

1.  Öffnen Sie das [Azure-Portal](https://portal.azure.com).

2.  Wählen Sie im linken Navigationsmenü die Option **App Services** aus.

3.  Wählen Sie Ihre App Service-App in der Liste aus.

1.  Wenn es sich bei der App um eine Funktions-App handelt, finden Sie weitere Informationen unter [Eingehende IP-Adresse einer Funktions-App](../azure-functions/ip-addresses.md#function-app-inbound-ip-address).

4.  Klicken Sie unter der Überschrift **Einstellungen** im linken Navigationsbereich auf **Eigenschaften**, und suchen Sie den Abschnitt mit der Bezeichnung **Virtuelle IP-Adresse**.

5. Kopieren Sie die IP-Adresse, und kopieren Sie den Domäneneintrag oder IP-Mechanismus neu.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde erläutert, wie Sie sich auf eine Änderung der IP-Adresse vorbereiten, die von Azure veranlasst wurde. Weitere Informationen zu IP-Adressen in Azure App Service finden Sie unter [Ein- und ausgehende IP-Adressen in Azure App Service](overview-inbound-outbound-ips.md).
