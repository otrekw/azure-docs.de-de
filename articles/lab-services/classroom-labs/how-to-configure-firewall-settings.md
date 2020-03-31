---
title: Firewalleinstellungen für Azure Lab Services
description: Erfahren Sie, wie Sie die öffentliche IP-Adresse und den Portnummernbereich von virtuellen Computern in einem Lab ermitteln, damit den Firewallregeln Informationen hinzugefügt werden können.
author: emaher
ms.author: enewman
ms.date: 02/14/2020
ms.topic: article
ms.service: lab-services
ms.openlocfilehash: fbd45af0c9b94f04fdaad9d9b5c8214a91a8db91
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "77443453"
---
# <a name="firewall-settings-for-azure-lab-services"></a>Firewalleinstellungen für Azure Lab Services

Jede Organisation oder Schule richtet ihr eigenes Netzwerk auf eine Weise ein, die Ihren Anforderungen am besten entspricht.  Dies umfasst manchmal auch das Festlegen von Firewallregeln, die RDP- (Remotedesktopprotokoll) oder SSH-Verbindungen (Secure Shell) mit Computern außerhalb ihres eigenen Netzwerks blockieren.  Da Azure Lab Services in der öffentlichen Cloud ausgeführt wird, ist möglicherweise eine zusätzliche Konfiguration erforderlich, damit Kursteilnehmer bei einer Verbindung über das Campusnetzwerk auf ihre VM zugreifen können.

Jedes Lab verwendet eine einzelne öffentliche IP-Adresse und mehrere Ports.  Diese öffentliche IP-Adresse wird von allen virtuellen Computern (Vorlagen-VM und Kursteilnehmer-VMs) verwendet.  Die öffentliche IP-Adresse wird während der Lebensdauer des Labs nicht geändert.  Jede VM nutzt jedoch eine andere Portnummer.  Der Portnummernbereich liegt zwischen 49152 und 65535.  Die Kombination aus öffentlicher IP-Adresse und Portnummer wird verwendet, um Kursleiter und Kursteilnehmer mit dem richtigen virtuellen Computer zu verbinden.  In diesem Artikel wird beschrieben, wie Sie die jeweilige öffentliche IP-Adresse ermitteln, die von einem Lab verwendet wird.  Mit diesen Informationen können Sie eingehende und ausgehende Firewallregeln anpassen, damit die Kursteilnehmer auf ihre VMs zugreifen können.

>[!IMPORTANT]
>Jedes Lab weist eine andere öffentliche IP-Adresse auf.

## <a name="find-public-ip-for-a-lab"></a>Ermitteln der öffentlichen IP-Adresse für ein Lab

Die öffentlichen IP-Adressen für die einzelnen Labs werden im Labkonto der Lab-Dienste auf der Seite **Alle Labs** aufgeführt.  Anweisungen zum Auffinden der Seite **Alle Labs** finden Sie unter [Verwalten der Labs in einem Labkonto](how-to-manage-lab-accounts.md#view-and-manage-labs-in-the-lab-account).  

> [!div class="mx-imgBorder"]
> ![Seite „Alle Labs“](../media/how-to-configure-firewall-settings/all-labs-properties.png)

>[!NOTE]
>Die öffentliche IP-Adresse wird nicht angezeigt, wenn der Vorlagencomputer für Ihr Lab noch nicht veröffentlicht wurde.

## <a name="conclusion"></a>Zusammenfassung

Sie kennen nun die öffentliche IP-Adresse für das Lab.  Eingangs- und Ausgangsregeln können in der Firewall der Organisation für die öffentliche IP-Adresse und den Portbereich 49152 – 65535 erstellt werden.  Nachdem die Regeln aktualisiert wurden, können Kursteilnehmer auf ihre VMs zugreifen, ohne dass die Netzwerkfirewall den Zugriff blockiert.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in folgenden Artikeln:

- [Zulassen der Auswahl des Labstandorts durch den Lab-Ersteller](allow-lab-creator-pick-lab-location.md)
- [Verbinden des Netzwerks Ihres Labs mit einem virtuellen Peernetzwerk](how-to-connect-peer-virtual-network.md)
- [Anfügen eines Katalogs mit freigegebenen Images an ein Lab](how-to-attach-detach-shared-image-gallery.md)
- [Hinzufügen eines Benutzers als Labbesitzer](how-to-add-user-lab-owner.md)
- [Anzeigen von Firewalleinstellungen für ein Lab](how-to-configure-firewall-settings.md)
- [Konfigurieren von anderen Einstellungen für ein Lab](how-to-configure-lab-accounts.md)
