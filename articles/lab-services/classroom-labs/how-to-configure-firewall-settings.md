---
title: Firewalleinstellungen für Azure Lab Services
description: Erfahren Sie, wie Sie die öffentliche IP-Adresse und den Portnummernbereich von virtuellen Computern in einem Lab ermitteln, damit den Firewallregeln Informationen hinzugefügt werden können.
author: emaher
ms.author: enewman
ms.date: 12/12/2019
ms.topic: article
ms.service: lab-services
ms.openlocfilehash: da1614e4a3e02ed91ef2d3c59ac4eb3eac0dcc7c
ms.sourcegitcommit: 2f8ff235b1456ccfd527e07d55149e0c0f0647cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/07/2020
ms.locfileid: "75692638"
---
# <a name="firewall-settings-for-azure-lab-services"></a>Firewalleinstellungen für Azure Lab Services

Jede Organisation oder Schule richtet ihr eigenes Netzwerk auf eine Weise ein, die Ihren Anforderungen am besten entspricht.  Dies umfasst manchmal auch das Festlegen von Firewallregeln, die RDP- (Remotedesktopprotokoll) oder SSH-Verbindungen (Secure Shell) mit Computern außerhalb ihres eigenen Netzwerks blockieren.  Da Azure Lab Services in der öffentlichen Cloud ausgeführt wird, ist möglicherweise eine zusätzliche Konfiguration erforderlich, damit Kursteilnehmer bei einer Verbindung über das Campusnetzwerk auf ihre VM zugreifen können.

Jedes Lab verwendet eine einzelne öffentliche IP-Adresse und mehrere Ports.  Diese öffentliche IP-Adresse wird von allen virtuellen Computern (Vorlagen-VM und Kursteilnehmer-VMs) verwendet.  Die öffentliche IP-Adresse wird während der Lebensdauer des Labs nicht geändert.  Jede VM nutzt jedoch eine andere Portnummer.  Der Portnummernbereich liegt zwischen 49152 und 65535.  Die Kombination aus öffentlicher IP-Adresse und Portnummer wird verwendet, um Kursleiter und Kursteilnehmer mit dem richtigen virtuellen Computer zu verbinden.  In diesem Artikel wird beschrieben, wie Sie die jeweilige öffentliche IP-Adresse ermitteln, die von einem Lab verwendet wird.  Mit diesen Informationen können Sie eingehende und ausgehende Firewallregeln anpassen, damit die Kursteilnehmer auf ihre VMs zugreifen können.

>[!IMPORTANT]
>Jedes Lab weist eine andere öffentliche IP-Adresse auf.

## <a name="find-public-ip-for-a-lab"></a>Ermitteln der öffentlichen IP-Adresse für ein Lab

Die öffentlichen IP-Adressen für die einzelnen Labs werden im Labkonto des Lab-Diensts auf dem Blatt **Alle Labs** aufgeführt.  Anweisungen zum Finden des Blatts **Alle Labs** finden Sie unter [Verwalten der Labs in einem Labkonto](how-to-manage-lab-accounts.md#view-and-manage-labs-in-the-lab-account).  

> [!div class="mx-imgBorder"]
> ![Blatt „Alle Labs“](../media/how-to-configure-firewall-settings/all-labs-properties.png)

>[!NOTE]
>Die öffentliche IP-Adresse wird nicht angezeigt, wenn der Vorlagencomputer für Ihr Lab noch nicht veröffentlicht wurde.

## <a name="conclusion"></a>Zusammenfassung

Sie kennen nun die öffentliche IP-Adresse für das Lab.  Eingangs- und Ausgangsregeln können in der Firewall der Organisation für die öffentliche IP-Adresse und den Portbereich 49152 – 65535 erstellt werden.  Nachdem die Regeln aktualisiert wurden, können Kursteilnehmer auf ihre VMs zugreifen, ohne dass die Netzwerkfirewall den Zugriff blockiert.
