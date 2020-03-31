---
title: Konfigurieren von Labkonten in Azure Lab Services | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie in Azure Lab Services ein Lab-Konto erstellen, alle Lab-Konten anzeigen oder ein Lab-Konto löschen.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2020
ms.author: spelluru
ms.openlocfilehash: fa9dba62b3b58687ec6a2bfc29e8722f7016b679
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79237262"
---
# <a name="configure-lab-accounts-in-azure-lab-services"></a>Konfigurieren von Labkonten in Azure Lab Services 
In Azure Lab Services ist ein Lab-Konto ein Container für verwaltete Labtypen (beispielsweise Classroom-Labs). Ein Administrator richtet ein Lab-Konto mit Azure Lab Services ein und gewährt Lab-Besitzern Zugriff, sodass sie Labs unter dem Konto erstellen können. 

In diesem Artikel wird erläutert, wie die folgenden Aufgaben ausgeführt werden: 

- Angeben eines Adressbereichs für VMs im Lab
- Konfigurieren des automatischen Herunterfahrens von VMs beim Trennen

## <a name="specify-an-address-range-for-vms-in-the-lab"></a>Angeben eines Adressbereichs für VMs im Lab
Das folgende Verfahren besteht aus Schritten zum Angeben eines Adressbereichs für VMs im Lab. Wenn Sie den Bereich aktualisieren, den Sie zuvor angegeben haben, gilt der geänderte Adressbereich nur für virtuelle Computer, die nach der Änderung erstellt werden. 

Hier werden einige Einschränkungen zur Angabe des Adressbereichs aufgeführt, die Sie beachten sollten. 

- Das Präfix muss kleiner als oder gleich 23 sein. 
- Wenn ein virtuelles Netzwerk mittels Peering mit dem Labkonto verbunden ist, darf sich der angegebene Adressbereich nicht mit dem Adressbereich des mittels Peering verbundenen virtuellen Netzwerks überschneiden.

1. Wählen Sie auf der Seite **Labkonto** im Menü auf der linken Seite die Option **Labeinstellungen**.
2. Geben Sie für das Feld **Adressbereich** den Adressbereich für virtuelle Computer an, die im Lab erstellt werden. Der Adressbereich muss in der CIDR-Notation (Classless Inter-Domain Routing) angegeben werden (Beispiel: 10.20.0.0/23). Virtuelle Computer im Lab werden in diesem Adressbereich erstellt.
3. Wählen Sie auf der Symbolleiste **Speichern** aus. 

    ![Konfigurieren des Adressenbereichs](../media/how-to-manage-lab-accounts/labs-configuration-page-address-range.png)


## <a name="automatic-shutdown-of-vms-on-disconnect"></a>Automatisches Herunterfahren von VMs beim Trennen
In diesem Artikel erfahren Sie, wie Sie das automatische Herunterfahren von virtuellen Windows-Lab-Computern (VM-Vorlage oder VM für Schüler/Studenten) aktivieren oder deaktivieren, nachdem eine Remotedesktopverbindung getrennt wurde. Sie können auch angeben, wie lange die VMs warten sollen, bis die Verbindung des Benutzers wieder hergestellt wird, bevor sie automatisch heruntergefahren werden.

![Einstellung für automatisches Herunterfahren in einem Labkonto](../media/how-to-configure-lab-accounts/automatic-shutdown-vm-disconnect.png)

Diese Einstellung gilt für alle im Labkonto erstellten Labs. Ein Labbesitzer kann diese Einstellung auf Labebene außer Kraft setzen. Die Änderung an dieser Einstellung im Labkonto wirkt sich nur auf Labs aus, die nach der Änderung erstellt werden.

Informationen dazu, wie ein Labbesitzer diese Einstellung auf Labebene konfigurieren kann, finden Sie in [diesem Artikel](how-to-enable-shutdown-disconnect.md)

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in folgenden Artikeln:

- [Zulassen der Auswahl des Labstandorts durch den Lab-Ersteller](allow-lab-creator-pick-lab-location.md)
- [Verbinden des Netzwerks Ihres Labs mit einem virtuellen Peernetzwerk](how-to-connect-peer-virtual-network.md)
- [Anfügen eines Katalogs mit freigegebenen Images an ein Lab](how-to-attach-detach-shared-image-gallery.md)
- [Hinzufügen eines Benutzers als Labbesitzer](how-to-add-user-lab-owner.md)
- [Anzeigen von Firewalleinstellungen für ein Lab](how-to-configure-firewall-settings.md)
