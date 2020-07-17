---
title: Erstellen eines Labs mit einer freigegebenen Ressource | Azure Lab Services
description: Erfahren Sie, wie Sie ein Lab erstellen, das eine Ressource erfordert, die von den Kursteilnehmern gemeinsam genutzt wird.
author: emaher
ms.topic: article
ms.date: 06/26/2020
ms.author: enewman
ms.openlocfilehash: 9cb5698f95aa220208fb02a35a52ff5363a173ac
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85443365"
---
# <a name="how-to-create-a-lab-with-a-shared-resource-in-azure-lab-services"></a>Erstellen eines Labs mit einer freigegebenen Ressource in Azure Lab Services

Manchmal arbeiten Sie beim Erstellen eines Classroom-Labs mit Ressourcen, die von allen Kursteilnehmern im Lab gemeinsam genutzt werden müssen.  Nehmen wir beispielsweise an, Sie verfügen über einen Lizenzierungsserver oder SQL Server für eine Datenbankklasse.  In diesem Artikel werden die Schritte beschrieben, mit denen Sie freigegebene Ressourcen für ein Lab aktivieren können.  Wir erläutern außerdem das Einschränken des Zugriffs auf die freigegebene Ressource.

## <a name="architecture"></a>Aufbau

Wie im Diagramm unten dargestellt, verfügen wir über ein Lab-Konto mit einem Lab.  Das Lab-Konto weist die VNET-Peeringeinstellungen auf, sodass das virtuelle Netzwerk für das Lab mit dem Netzwerk der freigegebenen Ressource verbunden ist.  Im Diagramm unten sind zwei virtuelle Netzwerke mit sich nicht überschneidenden IP-Bereichen zu sehen.  Diese IP-Adressbereiche sind lediglich Beispielbereiche.  Beachten Sie außerdem, dass sich das virtuelle Netzwerk der freigegebenen Ressource im gleichen Abonnement wie das Lab-Konto befindet.

![Architektur von Lab Services mit freigegebener Ressource](./media/how-to-create-a-lab-with-shared-resource/shared-resource-architecture.png)

## <a name="setup-shared-resource"></a>Einrichten der freigegebenen Ressource

Das virtuelle Netzwerk für die freigegebene Ressource muss vor dem Erstellen des Labs erstellt werden.  Weitere Informationen zum Erstellen eines virtuellen Netzwerks finden Sie unter [Erstellen eines virtuellen Netzwerks](../virtual-network/quick-create-portal.md).  Es ist wichtig, die Bereiche der virtuellen Netzwerke so zu planen, dass sie sich nicht mit den Adressbereichen der Lab-Computer überschneiden.  Weitere Informationen zum Planen von Netzwerken finden Sie im Artikel zum [Planen virtueller Netzwerke](../virtual-network/virtual-network-vnet-plan-design-arm.md). In unserem Beispiel befindet sich die freigegebene Ressource in einem virtuellen Netzwerk mit dem Bereich 10.2.0.0/16.  Wenn dies noch nicht geschehen ist, [erstellen Sie ein Subnetz](../virtual-network/virtual-network-manage-subnet.md#add-a-subnet) zur Aufnahme der freigegebenen Ressource.  Im Beispiel verwenden wir den Bereich 10.2.0.0/24, aber abhängig von den Anforderungen für Ihr Netzwerk kann Ihr Bereich abweichen.

Bei der freigegebenen Ressource kann es sich um Software handeln, die auf einem virtuellen Computer ausgeführt wird, oder um einen von Azure bereitgestellten Dienst. Die freigegebene Ressource sollte über eine private IP-Adresse zugänglich sein.  Indem Sie den Zugriff auf die freigegebene Ressource nur über eine private IP ermöglichen, schränken Sie den Zugriff auf sie ein.

Im Diagramm ist außerdem eine Netzwerksicherheitsgruppe (NSG) zu sehen, die verwendet werden kann, um den Datenverkehr von den VMs der Kursteilnehmer einzuschränken.  Beispielsweise können Sie eine Sicherheitsregel schreiben, die festlegt, dass Datenverkehr von den IP-Adressen der Kursteilnehmer-VMs nur auf eine freigegebene Ressource und sonst nichts zugreifen kann.  Weitere Informationen zum Festlegen von Sicherheitsregeln finden Sie unter [Verwalten der Netzwerksicherheitsgruppe](../virtual-network/manage-network-security-group.md#work-with-security-rules). Wenn Sie den Zugriff auf eine freigegebene Ressource auf ein bestimmtes Lab einschränken möchten, rufen Sie die IP-Adresse für das Lab aus den [Lab-Einstellungen des Lab-Kontos](manage-labs.md#view-labs-in-a-lab-account) ab, und legen Sie eine Eingangsregel fest, die den Zugriff nur von dieser IP-Adresse zulässt.  Vergessen Sie nicht, für diese IP-Adresse die Ports 49152 bis 65535 zuzulassen.  Optional können Sie die private IP-Adresse der Kursteilnehmer-VMs mithilfe der [Seite „VM-Pool“](how-to-set-virtual-machine-passwords.md) ermitteln.

Wenn es sich bei Ihrer freigegebenen Ressource um einen virtuellen Azure-Computer handelt, der erforderliche Software ausführt, müssen Sie möglicherweise die Firewall-Standardregeln für den virtuellen Computer ändern.

## <a name="lab-account"></a>Lab-Konto

Zum Verwenden einer freigegebenen Ressource muss das Lab-Konto für die Verwendung eines [virtuellen Peer-Netzwerks](how-to-connect-peer-virtual-network.md) eingerichtet sein.  In diesem Fall stellen wir eine Peeringverbindung mit dem virtuellen Netzwerk her, das die freigegebene Ressource enthält.

>[!WARNING]
>Das Lab für Ihre Klasse muss erstellt werden, **nachdem** eine Peering-Verbindung des Lab-Kontos mit dem virtuellen Netzwerk der freigegebenen Ressource hergestellt wurde.  
Vorlagencomputer

Nachdem das Peering zwischen Ihrem Lab-Konto und dem virtuellen Netzwerk eingerichtet wurde, sollte der Vorlagencomputer nun Zugriff auf die freigegebene Ressource haben.  Abhängig von der freigegebenen Ressource, auf die zugegriffen werden soll, müssen Sie möglicherweise die Firewallregeln aktualisieren.
