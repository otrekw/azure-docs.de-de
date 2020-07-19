---
title: include file
description: include file
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 07/09/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 999f3fb054eedab64a1f7bcebd9788e1edbf29f9
ms.sourcegitcommit: f844603f2f7900a64291c2253f79b6d65fcbbb0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2020
ms.locfileid: "86220598"
---
In diesem Schritt erstellen Sie die Verbindung zwischen Ihrem Hub und einem VNET. Wiederholen Sie diese Schritte für jedes VNET, mit dem Sie eine Verbindung herstellen möchten.

1. Wählen Sie auf der Seite für Ihr virtuelles WAN **Virtuelle Netzwerkverbindungen** aus.
1. Wählen Sie auf der Seite für die VNET-Verbindung **+Add connection** (+Verbindung hinzufügen) aus.
1. Füllen Sie auf der Seite **Add connection** (Verbindung hinzufügen) die folgenden Felder aus:

    * **Verbindungsname**: Dies ist der Name Ihrer Verbindung.
    * **Hubs**: Wählen Sie den Hub aus, den Sie dieser Verbindung zuordnen möchten.
    * **Abonnement**: Überprüfen Sie das Abonnement.
    * **Virtuelles Netzwerk**: Wählen Sie das virtuelle Netzwerk aus, das Sie mit diesem Hub verbinden möchten. Für das virtuelle Netzwerk kann nicht bereits ein Gateway für virtuelle Netzwerke vorhanden sein.
1. Klicken Sie auf **OK**, um die Verbindung zu erstellen.