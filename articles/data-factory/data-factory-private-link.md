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
ms.openlocfilehash: 2cd9f01404a4e33303356dd3f452cd7dbc47a747
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91328563"
---
# <a name="azure-private-link-for-azure-data-factory"></a>Azure Private Link für Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-xxx-md.md)]

Mithilfe von Azure Private Link können Sie eine Verbindung mit verschiedenen PaaS-Bereitstellungen (Platform as a Service) in Azure über einen privaten Endpunkt herstellen. Ein privater Endpunkt ist eine private IP-Adresse in einem bestimmten virtuellen Netzwerk und Subnetz. Eine Liste der PaaS-Bereitstellungen, die die Private Link-Funktionalität unterstützen, finden Sie in der [Dokumentation zu Private Link](https://docs.microsoft.com/azure/private-link/). 

## <a name="secure-communication-between-customer-networks-and-azure-data-factory"></a>Sichere Kommunikation zwischen dem Kundennetzwerk und Azure Data Factory 
Sie können Azure Virtual Network als logische Darstellung Ihres Netzwerks in der Cloud einrichten. Diese Vorgehensweise bietet folgende Vorteile:
* Sie schützen damit Ihre Azure-Ressourcen vor Angriffen in öffentlichen Netzwerken.
* Sie ermöglichen eine sichere Kommunikation zwischen den Netzwerken und Data Factory. 

Sie können ein lokales Netzwerk auch mit Ihrem virtuellen Netzwerk verbinden, indem Sie eine IPsec-VPN-Verbindung (Internetprotokollsicherheit, Site-to-Site) oder eine Azure ExpressRoute-Verbindung (privates Peering) einrichten. 

Sie können auch eine selbstgehostete Integration Runtime auf einem lokalen Computer oder einem virtuellen Computer im virtuellen Netzwerk installieren. Dies ermöglicht Folgendes:
* Ausführen von Kopieraktivitäten zwischen einem Clouddatenspeicher und einem Datenspeicher im privaten Netzwerk
* Verteilen von Transformationsaktivitäten für Computeressourcen in einem lokalen Netzwerk oder einem virtuellen Azure-Netzwerk 

Zwischen Azure Data Factory und dem virtuellen Netzwerk des Kunden sind mehrere Kommunikationskanäle erforderlich, wie in der folgenden Tabelle dargestellt:

| Domain | Port | BESCHREIBUNG |
| ---------- | -------- | --------------- |
| `adf.azure.com` | 443 | Eine Steuerungsebene, erforderlich für die Data Factory-Erstellung und -Überwachung |
| `*.{region}.datafactory.azure.net` | 443 | Erforderlich für die selbstgehostete Integration Runtime, um Verbindungen mit dem Azure Data Factory-Dienst herzustellen. |
| `*.servicebus.windows.net` | 443 | Erforderlich für die selbstgehostete Integration Runtime zur interaktiven Erstellung |
| `download.microsoft.com` | 443 | Erforderlich für die selbstgehostete Integration Runtime zum Herunterladen der Aktualisierungen. |

Mit der Unterstützung von Private Link für Azure Data Factory können Sie folgende Aktionen ausführen:
* Erstellen eines privaten Endpunkts in Ihrem virtuellen Netzwerk
* Aktivieren einer privaten Verbindung mit einer bestimmten Data Factory-Instanz 

Die Kommunikation mit dem Azure Data Factory-Dienst durchläuft Private Link, sodass eine geschützte private Konnektivität zur Verfügung steht. Sie müssen die oben genannte Domäne und den Port nicht in einem virtuellen Netzwerk oder in der Unternehmensfirewall konfigurieren, um Ihre Ressourcen besser zu schützen.  

![Diagramm von Private Link für die Azure Data Factory-Architektur](./media/data-factory-private-link/private-link-architecture.png)

Die Aktivierung des Private Link-Diensts für die genannten Kommunikationskanäle bietet die folgenden Funktionen:
- **Unterstützt:**
   - Sie können Erstellungs- und Überwachungsvorgänge der Data Factory in Ihrem virtuellen Netzwerk ausführen, selbst wenn Sie die gesamte ausgehende Kommunikation blockieren.
   - Die Befehlskommunikation zwischen einer selbstgehosteten Integration Runtime und dem Azure Data Factory-Dienst kann sicher in einer privaten Netzwerkumgebung stattfinden. Der Datenverkehr zwischen der selbstgehosteten Integration Runtime und dem Azure Data Factory-Dienst erfolgt über Private Link. 
- **Derzeit nicht unterstützt:**
   - Die interaktive Erstellung mithilfe der selbstgehosteten Integration Runtime durchläuft Private Link, z. B. für eine Testverbindung, das Durchsuchen der Ordner- und Tabellenliste, das Abrufen eines Schemas und das Anzeigen einer Vorschau von Daten.
   - Wenn Sie die automatische Aktualisierung aktivieren, kann die neue Version der selbstgehosteten Integration Runtime automatisch aus dem Microsoft Download Center heruntergeladen werden.

   > [!NOTE]
   > Für Funktionen, die derzeit nicht unterstützt werden, müssen Sie die oben genannte Domäne und den Port weiterhin im virtuellen Netzwerk oder der Unternehmensfirewall konfigurieren. 

> [!WARNING]
> Wenn Sie einen verknüpften Dienst erstellen, stellen Sie sicher, dass die Anmeldeinformationen in Azure Key Vault gespeichert werden. Andernfalls funktionieren die Anmeldeinformationen nicht, wenn Sie Private Link in Azure Data Factory aktivieren.

## <a name="set-up-private-link-for-azure-data-factory"></a>Einrichten von Private Link für Azure Data Factory
Private Endpunkte können über das [Azure-Portal](https://docs.microsoft.com/azure/private-link/create-private-endpoint-portal), mithilfe von PowerShell oder über die Azure-Befehlszeilenschnittstelle erstellt werden.

Sie können auch im Azure-Portal zu Ihrer Azure Data Factory-Instanz navigieren und einen privaten Endpunkt erstellen, wie im Folgenden dargestellt:

![Screenshot des Bereichs „Private Endpunktverbindungen“ zum Erstellen eines privaten Endpunkts](./media/data-factory-private-link/create-private-endpoint.png)


Wenn Sie den öffentlichen Zugriff auf diese Azure Data Factory-Instanz blockieren und nur den Zugriff über Private Link zulassen möchten, können Sie den Netzwerkzugriff auf Azure Data Factory im Azure-Portal deaktivieren, wie hier gezeigt:

![Screenshot des Bereichs „Netzwerkzugriff“ zum Erstellen eines privaten Endpunkts](./media/data-factory-private-link/disable-network-access.png)

> [!NOTE]
> Die Deaktivierung des Zugriffs auf öffentliche Netzwerke gilt nur für die selbstgehostete Integration Runtime, nicht für Azure Integration Runtime und SSIS Integration Runtime (SQL Server Integration Services).

> [!NOTE]
> Sie können nach dem Deaktivieren des Zugriffs auf öffentliche Netzwerke weiterhin über ein öffentliches Netzwerk auf das Azure Data Factory-Portal zugreifen.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen einer Data Factory über die Azure Data Factory-Benutzeroberfläche](quickstart-create-data-factory-portal.md)
- [Einführung in den Azure Data Factory-Dienst](introduction.md)
- [Visuelles Erstellen in Azure Data Factory](author-visually.md)

