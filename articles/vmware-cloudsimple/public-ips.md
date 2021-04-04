---
title: 'Azure VMware Solution by CloudSimple: Zuordnen öffentlicher IP-Adressen'
description: Beschreibt, wie öffentliche IP-Adressen für virtuelle Computer in der privaten Cloudumgebung zugeordnet werden.
author: Ajayan1008
ms.author: v-hborys
ms.date: 08/15/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: bb46ad726cd3b99324e9bb96b998ed1b4da885de
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "97899183"
---
# <a name="allocate-public-ip-addresses-for-private-cloud-environment"></a>Zuordnen von öffentlichen IP-Adressen für die private Cloudumgebung

Öffnen Sie die Registerkarte „Public IPs“ (Öffentliche IP-Adressen) auf der Seite „Network“ (Netzwerk), um öffentliche IP-Adressen für virtuelle Computer in Ihrer privaten Cloudumgebung zuzuordnen.

1. [Greifen Sie auf das CloudSimple Portal zu](access-cloudsimple-portal.md), und wählen Sie **Network** (Netzwerk) im seitlichen Menü aus.
2. Wählen Sie **Public IPs** (Öffentliche IP-Adressen) aus.
3. Klicken Sie auf **New Public IP** (Neue öffentliche IP-Adresse).

    ![Seite „Öffentliche IP-Adressen“](media/public-ips-page.png)

4. Geben Sie einen Namen ein, um den IP-Adresseintrag zu identifizieren.
5. Behalten Sie den Standardort bei.
6. Verwenden Sie den Schieberegler, um das Leerlauftimeout bei Bedarf zu ändern.
7. Geben Sie die lokale IP-Adresse ein, für die Sie eine öffentliche IP-Adresse zuweisen möchten.
8. Geben Sie einen zugeordneten DNS-Namen ein.
9. Klicken Sie auf **Submit**(Senden).

![Zuordnen von öffentlichen IP-Adressen](media/network-public-ip-allocate.png)

Jetzt beginnt die Aufgabe des Zuordnens der öffentlichen IP-Adresse. Sie können den Status der Aufgabe auf der Seite **Aktivität > Aufgaben** überprüfen. Nach Abschluss der Zuordnung wird der neue Eintrag auf der Seite „Öffentliche IP-Adressen“ angezeigt.
