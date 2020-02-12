---
title: 'Azure VMware Solutions (AVS): Verwenden der privaten AVS-Cloud als Notfallwiederherstellungsstandort für lokale Workloads'
description: In diesem Artikel wird beschrieben, wie Sie Ihre private AVS-Cloud als Notfallwiederherstellungsstandort für lokale VMware-Workloads einrichten.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 1f34c4fb89d28a001a4af4d21879403a9ac5e860
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024703"
---
# <a name="set-up-avs-private-cloud-as-a-disaster-recovery-site-for-on-premises-vmware-workloads"></a>Einrichten der privaten AVS-Cloud als Notfallwiederherstellungsstandort für lokale VMware-Workloads

Ihre private AVS-Cloud kann als Wiederherstellungsstandort für lokale Anwendungen eingerichtet werden, um im Notfall für Geschäftskontinuität zu sorgen. Die Wiederherstellungslösung basiert auf Zerto Virtual Replication als Replikations- und Orchestrierungsplattform. Die grundlegende Infrastruktur sowie wichtige virtuelle Computer (VMs) können kontinuierlich aus der lokalen vCenter-Instanz in Ihre private AVS-Cloud repliziert werden. Sie können Ihre private AVS-Cloud für Failovertests und zum Sicherstellen der Verfügbarkeit Ihrer Anwendung bei einem Ausfall verwenden. Auf ähnliche Weise können Sie Ihre private AVS-Cloud als primären Standort einrichten, der durch einen geografisch getrennten Wiederherstellungsstandort geschützt wird.

> [!NOTE]
> Richtlinien zum Festlegen der richtigen Größe für Ihre Notfallwiederherstellungsumgebung finden Sie im Zerto-Dokument [Sizing Considerations For Zerto Virtual Replication](https://s3.amazonaws.com/zertodownload_docs/5.5U3/Zerto%20Virtual%20Replication%20Sizing.pdf) (Überlegungen zum Festlegen der Größe für Zerto Virtual Replication).

Die Vorteile der AVS-Lösung:

* Sie müssen kein spezielles Rechenzentrum für die Notfallwiederherstellung einrichten.
* Sie können die Azure-Standorte nutzen, an denen AVS bereitgestellt ist und für weltweite geografische Resilienz sorgt.
* Sie können die Bereitstellungs- und Gesamtbetriebskosten für die Notfallwiederherstellung reduzieren.

Für die Lösung ist Folgendes erforderlich:

* Sie müssen Zerto in Ihrer privaten AVS-Cloud installieren, konfigurieren und verwalten.
* Sie müssen Ihre eigenen Lizenzen für Zerto bereitstellen, wenn die private AVS-Cloud der geschützte Standort ist. Sie können zur Lizenzierung Zerto-Instanzen am AVS-Standort mit Ihrem lokalen Standort koppeln.

Die folgende Abbildung zeigt die Architektur der Zerto-Lösung.

![Aufbau](media/cloudsimple-zerto-architecture.png)

## <a name="how-to-deploy-the-solution"></a>Bereitstellen der Lösung

In den folgenden Abschnitten wird beschrieben, wie Sie mit Zerto Virtual Replication eine Notfallwiederherstellungslösung in Ihrer privaten AVS-Cloud bereitstellen.

1. [Voraussetzungen](#prerequisites)
2. [Optionale Konfiguration in Ihrer privaten AVS-Cloud](#optional-configuration-on-your-avs-private-cloud)
3. [Einrichten von ZVM und VRA in Ihrer privaten AVS-Cloud](#set-up-zvm-and-vra-on-your-avs-private-cloud)
4. [Einrichten einer Zerto Virtual Protection-Gruppe](#set-up-zerto-virtual-protection-group)

### <a name="prerequisites"></a>Voraussetzungen

Erfüllen Sie die folgenden Voraussetzungen, um Zerto Virtual Replication über Ihre lokale Umgebung in Ihrer privaten AVS-Cloud zu aktivieren.

1. [Richten Sie eine Site-to-Site-VPN-Verbindung zwischen Ihrem lokalen Netzwerk und Ihrer privaten AVS-Cloud ein](set-up-vpn.md).
2. [Richten Sie ein DNS-Lookup ein, sodass die Verwaltungskomponenten Ihrer privaten AVS-Cloud an die DNS-Server der privaten AVS-Cloud weitergeleitet werden](on-premises-dns-setup.md). Erstellen Sie auf Ihrem lokalen DNS-Server einen Eintrag für eine Weiterleitungszone für `*.AVS.io` an AVS-DNS-Server, um die Weiterleitung für das DNS-Lookup zu aktivieren.
3. Richten Sie ein DNS-Lookup ein, sodass lokale vCenter-Komponenten an lokale DNS-Server weitergeleitet werden. Die DNS-Server müssen von Ihrer privaten AVS-Cloud aus über die Site-to-Site-VPN-Verbindung erreichbar sein. Wenn Sie Hilfe benötigen, senden Sie eine [Supportanfrage](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) mit den folgenden Informationen. 

    * Name der lokalen DNS-Domäne
    * IP-Adressen des lokalen DNS-Servers

4. Installieren Sie einen Windows-Server in Ihrer privaten AVS-Cloud. Auf diesem Server wird Zerto Virtual Manager installiert.
5. [Eskalieren Sie Ihre AVS-Berechtigungen](escalate-private-cloud-privileges.md).
6. Erstellen Sie in der vCenter-Instanz Ihrer privaten AVS-Cloud einen neuen Benutzer mit Administratorrolle, der als Dienstkonto für Zerto Virtual Manager verwendet wird.

### <a name="optional-configuration-on-your-avs-private-cloud"></a>Optionale Konfiguration in Ihrer privaten AVS-Cloud

1. Erstellen Sie einen oder mehrere Ressourcenpools in der vCenter-Instanz Ihrer privaten AVS-Cloud, die als Zielressourcenpools für VMs in Ihrer lokalen Umgebung dienen.
2. Erstellen Sie einen oder mehrere Ordner in der vCenter-Instanz Ihrer privaten AVS-Cloud, die als Zielordner für VMs in Ihrer lokalen Umgebung dienen.
3. Erstellen Sie VLANs als Failovernetzwerke, und richten Sie Firewallregeln ein. Wenn Sie Hilfe benötigen, senden Sie [eine Supportanfrage](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
4. Erstellen Sie verteilte Portgruppen für das Failover- und das Testnetzwerk, um das Failover von VMs zu testen.
5. Installieren Sie [DHCP- und DNS-Server](dns-dhcp-setup.md), oder verwenden Sie einen Active Directory-Domänencontroller in Ihrer privaten AVS-Cloudumgebung.

### <a name="set-up-zvm-and-vra-on-your-avs-private-cloud"></a>Einrichten von ZVM und VRA in Ihrer privaten AVS-Cloud

1. Installieren Sie Zerto Virtual Manager (ZVM) auf dem Windows-Server in Ihrer privaten AVS-Cloud.
2. Melden Sie sich mit dem in einem vorherigen Schritt erstellten Dienstkonto bei ZVM an.
3. Richten Sie die Lizenzierung für Zerto Virtual Manager ein.
4. Installieren Sie die Zerto Virtual Replication Appliance (VRA) auf den ESXi-Hosts Ihrer privaten AVS-Cloud.
5. Koppeln Sie die ZVM-Instanz in Ihrer privaten AVS-Cloud mit der ZVM-Instanz in Ihrer lokalen Umgebung.

### <a name="set-up-zerto-virtual-protection-group"></a>Einrichten einer Zerto Virtual Protection-Gruppe

1. Erstellen Sie eine neue Virtual Protection Group (VPG), und geben Sie die Priorität für diese VPG an.
2. Wählen Sie die virtuellen Computer aus, die aus Gründen der Geschäftskontinuität Schutz benötigen, und passen Sie bei Bedarf die Startreihenfolge an.
3. Wählen Sie als Wiederherstellungsstandort Ihre private AVS-Cloud und als Standardwiederherstellungsserver den privaten AVS-Cloudcluster oder die erstellte Ressourcengruppe aus. Wählen Sie **vsanDatastore** als Wiederherstellungsdatenspeicher in Ihrer privaten AVS-Cloud aus.

    ![VPG](media/cloudsimple-zerto-vpg.png)

    > [!NOTE]
    > Sie können die Hostoption für einzelne VMs in den VM-Einstellungen ändern.

4. Passen Sie die Speicheroptionen gemäß Ihren Anforderungen an.
5. Geben Sie die Wiederherstellungsnetzwerke an, die als Failover- und als Failovertestnetzwerk verwenden werden sollen (beziehen Sie dabei die in einem vorherigen Schritt erstellten Portgruppen ein), und passen Sie die Wiederherstellungsskripts entsprechend an.
6. Passen Sie bei Bedarf die Netzwerkeinstellungen für einzelne VMs an, und erstellen Sie die VPG.
7. Testen Sie das Failover, sobald die Replikation abgeschlossen ist.

## <a name="reference"></a>Verweis

[Zerto-Dokumentation](https://www.zerto.com/myzerto/technical-documentation/)
