---
title: 'Azure VMware Solutions (AVS): Zugreifen auf den vSphere-Client'
description: Beschreibt den Zugriff auf vCenter in Ihrer privaten AVS-Cloud
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/30/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: ad018ea89b194d42ab1867a0569725c4c3680f7d
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77022663"
---
# <a name="access-your-avs-private-cloud-vcenter-portal"></a>Zugreifen auf das vCenter-Portal Ihrer privaten AVS-Cloud

Sie können das vCenter-Portal Ihrer privaten AVS-Cloud über das Azure-Portal oder das AVS-Portal starten. Das vCenter-Portal ermöglicht Ihnen die Verwaltung der VMware-Infrastruktur für Ihre private AVS-Cloud.

## <a name="before-you-begin"></a>Voraussetzungen

Für den Zugriff auf das vCenter-Portal muss eine Netzwerkverbindung hergestellt und eine DNS-Namensauflösung aktiviert werden. Sie können mithilfe der folgenden Optionen eine Netzwerkverbindung mit Ihrer privaten AVS-Cloud herstellen.

* [Herstellen einer Verbindung aus der lokalen Umgebung mit AVS mithilfe von ExpressRoute](on-premises-connection.md)
* [Konfigurieren einer VPN-Verbindung mit Ihrer privaten AVS-Cloud](set-up-vpn.md)

Informationen zum Einrichten der DNS-Namensauflösung für die VMware-Infrastrukturkomponenten der privaten AVS-Cloud finden Sie unter [Konfigurieren von DNS für die Namensauflösung für den vCenter-Zugriff in der privaten AVS-Cloud über lokale Arbeitsstationen](on-premises-dns-setup.md)

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="access-vcenter-from-azure-portal"></a>Zugreifen auf vCenter über das Azure-Portal

Sie können das vCenter-Portal Ihrer privaten AVS-Cloud über das Azure-Portal starten.

1. Wählen Sie **Alle Dienste** aus.

2. Suchen Sie nach **AVS Services** (AVS-Dienste).

3. Wählen Sie den AVS-Dienst aus, über den Sie eine Verbindung mit Ihrer privaten AVS-Cloud herstellen möchten.

4. Klicken Sie auf der Seite **Overview** (Übersicht) auf **View VMware Private Clouds** (Private VMware-AVS-Clouds anzeigen).

    ![Übersicht über den AVS-Dienst](media/cloudsimple-service-overview.png)

5. Wählen Sie die gewünschte private AVS-Cloud in der Liste aus, und klicken Sie dann auf **Launch vSphere Client** (vSphere-Client starten).

    ![VSphere-Client starten](media/cloudsimple-service-launch-vsphere-client.png)

## <a name="access-vcenter-from-avs-portal"></a>Zugreifen auf vCenter über das AVS-Portal

Sie können das vCenter-Portal Ihrer privaten AVS-Cloud über das AVS-Portal starten.

1. Greifen Sie auf Ihr [AVS-Portal](access-cloudsimple-portal.md) zu.

2. Wählen Sie unter **Resources** (Ressourcen) die private AVS-Cloud aus, auf die Sie zugreifen möchten, und klicken Sie dann auf **Launch vSphere Client** (vSphere-Client starten).

    ![vSphere-Client starten – Ressourcen](media/cloudsimple-portal-resources-launch-vcenter.png)

3. Sie können das vCenter-Portal auch über den Zusammenfassungsbildschirm Ihrer privaten AVS-Cloud starten.

    ![vSphere-Client starten – Zusammenfassung](media/cloudsimple-resources-summary-launch-vcenter.png)

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen und Verwalten von VLANs/Subnetzen für private AVS-Clouds](create-vlan-subnet.md)
* [Berechtigungsmodell der privaten AVS-Cloud von VMware vCenter](learn-private-cloud-permissions.md)