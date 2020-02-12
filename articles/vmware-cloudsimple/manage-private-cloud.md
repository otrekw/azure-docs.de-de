---
title: Verwalten einer privaten AVS-Cloud (Azure VMware Solutions)
description: Beschreibt die Funktionen, mit denen Ressourcen und Aktivitäten einer privaten AVS-Cloud verwaltet werden können
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 47bf2251f71204b99245c1a9d55ef87157c41dd8
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77014826"
---
# <a name="manage-avs-private-cloud-resources-and-activities"></a>Verwalten von Ressourcen und Aktivitäten in einer privaten AVS-Cloud

Private AVS-Clouds werden über das AVS-Portal verwaltet. Im AVS-Portal können Sie den Status, verfügbare Ressourcen, Aktivitäten in der privaten AVS-Cloud und weitere Einstellungen überprüfen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="access-the-avs-portal"></a>Zugreifen auf das AVS-Portal

Greifen Sie auf das [AVS-Portal](access-cloudsimple-portal.md) zu.

## <a name="view-the-list-of-avs-private-clouds"></a>Anzeigen der Liste privater AVS-Clouds

Auf der Seite **Resources** (Ressourcen) der Registerkarte **AVS Private Clouds** (Private AVS-Clouds) sind alle privaten AVS-Clouds in Ihrem Abonnement aufgeführt. Die Angaben umfassen den Namen, die Anzahl der vSphere-Cluster, den Standort, den aktuellen Status der privaten AVS-Cloud und Ressourceninformationen.

![Seite „AVS Private Cloud“](media/manage-private-cloud.png)

Wählen Sie eine private AVS-Cloud aus, um zusätzliche Informationen und Aktionen anzuzeigen.

## <a name="avs-private-cloud-summary"></a>Private AVS-Cloud – Zusammenfassung

Sie erhalten eine umfassende Übersicht über die ausgewählte private AVS-Cloud. Auf der Seite „Summary“ (Zusammenfassung) werden die in der privaten AVS-Cloud bereitgestellten DNS-Server aufgeführt. Sie können die DNS-Weiterleitung von lokalen DNS-Servern an Ihre DNS-Server in der privaten AVS-Cloud einrichten. Weitere Informationen zur DNS-Weiterleitung finden Sie im Thema zum [Konfigurieren der DNS-Namensauflösung für den lokalen Zugriff auf vCenter in einer privaten AVS-Cloud](https://docs.azure.cloudsimple.com/on-premises-dns-setup/).

![Private AVS-Cloud – Zusammenfassung](media/private-cloud-summary.png)

### <a name="available-actions"></a>Verfügbare Aktionen

* [Launch vSphere client](https://docs.azure.cloudsimple.com/vsphere-access/) (vSphere-Client starten): Ermöglicht den vCenter-Zugriff für diese private AVS-Cloud.
* [Purchase nodes](create-nodes.md) (Knoten kaufen): Fügt dieser privaten AVS-Cloud Knoten hinzu.
* [Expand](expand-private-cloud.md) (Erweitern): Fügt dieser privaten AVS-Cloud Knoten hinzu.
* **Refresh** (Aktualisieren): Aktualisiert die Informationen auf dieser Seite.
* **Löschen**: Sie können die private AVS-Cloud jederzeit löschen. **Stellen Sie vor dem Löschen sicher, dass Sie alle Systeme und Daten gesichert haben.** Beim Löschen einer privaten AVS-Cloud werden alle VMs, vCenter-Konfigurationseinstellungen und Daten gelöscht. Klicken Sie im Abschnitt „Summary“ (Zusammenfassung) der ausgewählten privaten AVS-Cloud auf **Delete** (Löschen). Nach dem Löschen werden alle Daten der privaten AVS-Cloud in einem sicheren, hochgradig konformen Vorgang entfernt.
* [Change vSphere privileges](escalate-private-cloud-privileges.md) (vSphere-Berechtigungen ändern): Erweitert Ihre Berechtigungen in dieser privaten AVS-Cloud.

## <a name="avs-private-cloud-vlanssubnets"></a>Private AVS-Cloud – VLANs/Subnetze

Sie können die Liste der für die ausgewählte private AVS-Cloud definierten VLANs/Subnetze anzeigen. Die Liste umfasst die bei der Erstellung der privaten AVS-Cloud konfigurierten Verwaltungs-VLANs/-Subnetze.

![Private AVS-Cloud – VLANs/Subnetze](media/private-cloud-vlans-subnets.png) 

### <a name="available-actions"></a>Verfügbare Aktionen

* [Add VLANS/Subnets](https://docs.azure.cloudsimple.com/create-vlan-subnet/) (VLANS/Subnetze hinzufügen): Fügt der privaten AVS-Cloud ein VLAN/Subnetz hinzu.

Wählen Sie für folgende Aktionen ein VLAN/Subnetz aus:
* [Attach firewall table](https://docs.azure.cloudsimple.com/firewall/) (Firewalltabelle anfügen): Fügt eine Firewalltabelle an diese private AVS-Cloud an.
* **Bearbeiten**
* **Delete** (Löschen): nur benutzerdefinierte VLANs/Subnetze

## <a name="avs-private-cloud-activity"></a>Private AVS-Cloud – Aktivitäten

Sie können folgende Informationen zur ausgewählten privaten AVS-Cloud anzeigen. Die Aktivitätsinformationen sind in einer gefilterten Liste aller Aktivitäten in der ausgewählten privaten AVS-Cloud zusammengefasst. Auf dieser Seite werden die letzten Aktivitäten (maximal 25) angezeigt.

* Recent alerts (Letzte Warnungen)
* Recent events (Letzte Ereignisse)
* Recent tasks (Letzte Aufgaben)
* Recent audit (Letzte Überprüfung)

![Private AVS-Cloud – Aktivitäten](media/private-cloud-activity.png)

## <a name="cloud-racks"></a>Cloud-Racks

Cloudracks sind die Bausteine Ihrer privaten AVS-Cloud. Jedes Rack umfasst eine Kapazitätseinheit. In AVS werden Cloudracks beim Erstellen oder Erweitern einer privaten AVS-Cloud basierend auf den ausgewählten Optionen automatisch konfiguriert. Sie können die vollständige Liste der Cloudracks anzeigen, einschließlich der privaten AVS-Cloud, der sie jeweils zugewiesen sind.

![Private AVS-Cloud – Cloudracks](media/private-cloud-cloudracks.png)

## <a name="vsphere-management-network"></a>vSphere Management Network

Die Liste der VMware-Verwaltungsressourcen und der virtuellen Computer, die derzeit in der privaten AVS-Cloud konfiguriert sind. Die Informationen umfassen die Softwareversion, den vollqualifizierten Domänennamen (FQDN) und die IP-Adresse der Ressourcen.

![Private AVS-Cloud – vSphere Management Network](media/private-cloud-vsphere-management-network.png)

## <a name="next-steps"></a>Nächste Schritte

* [Nutzen von virtuellen VMware-Computern in Azure](quickstart-create-vmware-virtual-machine.md)
* Informieren Sie sich über [private AVS-Clouds](cloudsimple-private-cloud.md).