---
title: Azure Private Link für Azure Data Factory
description: Erfahren Sie etwas über die Funktionsweise von Azure Private Link in Azure Data Factory.
services: data-factory
ms.author: abnarain
author: nabhishek
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 09/01/2020
ms.openlocfilehash: 1d560bd4a6b826e2bf0b3e23dde563cc39863537
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89322828"
---
# <a name="azure-private-link-for-azure-data-factory"></a>Azure Private Link für Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-xxx-md.md)]

Private Link ermöglicht das Herstellen von Verbindungen mit verschiedenen PaaS-Diensten in Azure über einen privaten Endpunkt. Eine Liste der PaaS-Dienste, die die Private Link-Funktionalität unterstützen, finden Sie auf der [Dokumentationsseite zu Private Link](https://docs.microsoft.com/azure/private-link/). Ein privater Endpunkt ist eine private IP-Adresse in einem bestimmten virtuellen Netzwerk und Subnetz.

## <a name="secure-communication-between-customer-network-and-azure-data-factory-service"></a>Sichere Kommunikation zwischen dem Kundennetzwerk und dem Azure Data Factory-Dienst
Um Ihre Azure-Ressourcen vor Angriffen im öffentlichen Netzwerk zu schützen oder ihre sichere Kommunikation untereinander zu ermöglichen, können Sie ein virtuelles Azure-Netzwerk als logische Darstellung Ihres Netzwerks in der Cloud einrichten. Sie können ein lokales Netzwerk auch mit Ihrem virtuellen Netzwerk verbinden, indem Sie ein IPsec-VPN (Site-to-Site) oder ExpressRoute (privates Peering) einrichten. Die selbstgehostete Integration Runtime kann auf einem lokalen Computer oder einem virtuellen Computer im virtuellen Netzwerk installiert werden, um Kopieraktivitäten zwischen einem Clouddatenspeicher und einem Datenspeicher in einem privaten Netzwerk auszuführen oder Transformationsaktivitäten für Computeressourcen in einem lokalen Netzwerk oder einem virtuellen Azure-Netzwerk zu verteilen. 

Zwischen Data Factory und dem virtuellen Netzwerk des Kunden sind mehrere Kommunikationskanäle erforderlich.


| **Domäne** | **Port** | **Beschreibung** |
| ---------- | -------- | --------------- |
| `pe-adf.azure.com` | 443 | Steuerungsebene. Erforderlich für die Data Factory-Erstellung und -Überwachung. |
| `*.{region}.datafactory.azure.net` | 443 | Erforderlich für die selbstgehostete Integration Runtime, um Verbindungen mit dem Data Factory-Dienst herzustellen. |
| `*.servicebus.windows.net` | 443 | Erforderlich für die selbstgehostete Integration Runtime zur interaktiven Erstellung. |
| `download.microsoft.com` | 443 | Erforderlich für die selbstgehostete Integration Runtime zum Herunterladen der Aktualisierungen. |


Mit der Unterstützung von Azure Private Link für Azure Data Factory können Sie einen privaten Endpunkt (PE) in Ihrem virtuellen Netzwerk erstellen und eine private Verbindung mit einer bestimmten Azure Data Factory-Instanz ermöglichen. Die Kommunikation mit dem Azure Data Factory-Dienst durchläuft Azure Private Link, sodass eine gesicherte private Konnektivität zur Verfügung steht. Sie müssen die oben genannte Domäne und den Port nicht im virtuellen Netzwerk oder in der Unternehmensfirewall konfigurieren, die eine sicherere Möglichkeit zum Schutz Ihrer Ressourcen bieten.  

![Architektur von Azure Data Factory mit Private Link](./media/data-factory-private-link/private-link-architecture.png)

Folgende Vorteile bietet die Aktivierung des Private Link-Diensts für die einzelnen oben genannten Kommunikationskanäle:
- (Unterstützt:) Sie können Erstellungs- und Überwachungsvorgänge mit Azure Data Factory in Ihrem virtuellen Netzwerk ausführen, selbst wenn Sie die gesamte ausgehende Kommunikation blockieren.
- (Unterstützt:) Die Befehlskommunikation zwischen einer selbstgehosteten Integration Runtime und dem Azure Data Factory-Dienst kann sicher in einer privaten Netzwerkumgebung stattfinden. Der Datenverkehr zwischen der selbstgehosteten Integration Runtime und dem Azure Data Factory-Dienst erfolgt über Private Link. 
- (Derzeit nicht unterstützt:) Die interaktive Erstellung mithilfe der selbstgehosteten Integration Runtime durchläuft Private Link, z. B. für eine Testverbindung, das Durchsuchen der Ordnerliste und Tabellenliste, das Abrufen eines Schemas und für eine Vorschau von Daten.
- (Derzeit nicht unterstützt:) Wenn Sie die automatische Aktualisierung aktivieren, kann die neue Version der selbstgehosteten Integration Runtime automatisch aus dem Downloadcenter heruntergeladen werden.

> [!NOTE]
> Für Funktionen, die derzeit nicht unterstützt werden, müssen Sie die oben genannte Domäne und den Port weiterhin im virtuellen Netzwerk oder der Unternehmensfirewall konfigurieren. 

> [!WARNING]
> Wenn Sie einen verknüpften Dienst erstellen, stellen Sie sicher, dass die Anmeldeinformationen in Azure Key Vault gespeichert werden. Andernfalls funktioniert er nicht, wenn Sie den Private Link-Dienst in Azure Data Factory aktivieren.

## <a name="how-to-set-up-private-link-for-azure-data-factory"></a>Einrichten von Private Link für Azure Data Factory
Private Endpunkte können über das Azure-Portal, mithilfe von PowerShell oder über die Azure CLI erstellt werden:

[Portal](https://docs.microsoft.com/azure/private-link/create-private-endpoint-portal)


Sie können auch im Azure-Portal zu Ihrer Azure Data Factory-Instanz navigieren und einen privaten Endpunkt (PE) erstellen:

![Erstellen eines privaten Endpunkts](./media/data-factory-private-link/create-private-endpoint.png)


Wenn Sie den öffentlichen Zugriff auf diese Azure Data Factory-Instanz blockieren und nur den Zugriff über Private Link zulassen möchten, können Sie den Netzwerkzugriff auf Azure Data Factory im Azure-Portal deaktivieren:

![Erstellen eines privaten Endpunkts](./media/data-factory-private-link/disable-network-access.png)

> [!NOTE]
> Die Deaktivierung des Zugriffs auf öffentliche Netzwerke gilt nur für die selbstgehostete Integration Runtime, nicht für Azure Integration Runtime und SSIS Integration Runtime.

> [!NOTE]
> Benutzer können nach dem Deaktivieren des Zugriffs auf das öffentliche Netzwerk weiterhin über das öffentliche Netzwerk auf das Azure Data Factory-Portal zugreifen.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen einer Data Factory über die Azure Data Factory-Benutzeroberfläche](quickstart-create-data-factory-portal.md)

- [Einführung in den Azure Data Factory-Dienst](introduction.md)

- [Visuelles Erstellen in Azure Data Factory](author-visually.md)

