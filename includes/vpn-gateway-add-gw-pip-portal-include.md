---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 07/16/2021
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: e259525bc8215b4542dba86cbe66aaa9ce6d9fbf
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114439652"
---
4. Geben Sie die Werte für **Öffentliche IP-Adresse** an. Mit diesen Einstellungen wird das Objekt für die öffentliche IP-Adresse angegeben, das dem VPN-Gateway zugeordnet wird. Die öffentliche IP-Adresse wird bei der Erstellung des VPN-Gateways diesem Objekt dynamisch zugewiesen. Die öffentliche IP-Adresse ändert sich nur, wenn das Gateway gelöscht und neu erstellt wird. Sie ändert sich nicht, wenn die Größe geändert wird, das VPN-Gateway zurückgesetzt wird oder andere interne Wartungs-/Upgradevorgänge für das VPN-Gateway durchgeführt werden.

   :::image type="content" source="./media/vpn-gateway-add-gw-pip-portal/pip-details.png" alt-text="Screenshot: Feld „Öffentliche IP-Adresse“":::

     * **Öffentliche IP-Adresse:** Lassen Sie **Neu erstellen** aktiviert.
     * **Öffentliche IP-Adresse**: Geben Sie im Textfeld einen Namen für die öffentliche IP-Adressinstanz ein.
     * **Zuweisung**: VPN Gateway unterstützt nur die dynamische Zuweisung.
     * **Aktiv/Aktiv-Modus aktivieren**: Aktivieren Sie **Aktiv/Aktiv-Modus aktivieren** nur, wenn Sie eine Aktiv/Aktiv-Gatewaykonfiguration erstellen. Lassen Sie diese Einstellung andernfalls **deaktiviert**.
     * Belassen Sie für **BGP konfigurieren** den Wert **Deaktiviert**, es sei denn, Ihre Konfiguration erfordert diese Einstellung ausdrücklich. Sollte diese Einstellung erforderlich sein, lautet die Standard-ASN 65515. (Dieser Wert kann jedoch geändert werden.)
5. Wählen Sie zum Ausführen der Validierung **Bewerten + erstellen** aus.
6. Wählen Sie nach Abschluss der Validierung **Erstellen** aus, um das VPN-Gateway bereitzustellen.