---
title: Verbinden von Stream Analytics-Aufträgen mit Ressourcen in einem Azure Virtual Network (VNet)
description: In diesem Artikel wird beschrieben, wie Sie einen Azure Stream Analytics-Auftrag mit Ressourcen verbinden, die sich in einem VNet befinden.
author: sidramadoss
ms.author: sidram
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/04/2021
ms.custom: devx-track-js
ms.openlocfilehash: 4701cb4122b4196b08b2a427b34d49c7784b91a7
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98878238"
---
# <a name="connect-stream-analytics-jobs-to-resources-in-an-azure-virtual-network-vnet"></a>Verbinden von Stream Analytics-Aufträgen mit Ressourcen in einem Azure Virtual Network (VNet)

Ihre Stream Analytics-Aufträge stellen ausgehende Verbindungen mit Ihren Ein- und Ausgaberessourcen von Azure her, um Daten in Echtzeit zu verarbeiten und Ergebnisse zu erzeugen. Diese Ein- und Ausgaberessourcen (z. B. Azure Event Hubs und Azure SQL-Datenbank) können sich hinter einer Azure-Firewall oder in einem Azure Virtual Network (VNet) befinden. Der Stream Analytics-Dienst wird aus Netzwerken betrieben, die nicht direkt in Ihre Netzwerkregeln aufgenommen werden können.

Es gibt jedoch zwei Möglichkeiten, um Ihre Stream Analytics-Aufträge in solchen Szenarien sicher mit Ihren Ein- und Ausgaberessourcen zu verbinden.
* Verwenden privater Endpunkte in Stream Analytics-Clustern.
* Verwendung des Authentifizierungsmodus „Verwaltete Identität“, gekoppelt mit der Netzwerkeinstellung „Vertrauenswürdige Dienste zulassen“.

Ihr Stream Analytics-Auftrag akzeptiert keine eingehende Verbindung.

## <a name="private-endpoints-in-stream-analytics-clusters"></a>Private Endpunkte in Stream Analytics-Clustern.
[Stream Analytics-Cluster](./cluster-overview.md) sind dedizierte Computecluster mit einem einzelnen Mandanten, auf denen Sie Ihre Stream Analytics-Aufträge ausführen können. Sie können verwaltete private Endpunkte in Ihrem Stream Analytics-Cluster erstellen, wodurch alle Aufträge, die auf Ihrem Cluster ausgeführt werden, eine sichere ausgehende Verbindung mit Ihren Ein- und Ausgaberessourcen herstellen können.

Das Erstellen privater Endpunkte in Ihrem Stream Analytics-Cluster ist ein [zweistufiger Vorgang](./private-endpoints.md). Diese Option eignet sich am besten für mittlere bis große Streamingworkloads, da die Mindestgröße eines Stream Analytics-Clusters 36 SUs beträgt (obwohl die 36 SUs von verschiedenen Aufträgen in verschiedenen Abonnements oder Umgebungen wie Entwicklung, Test und Produktion gemeinsam genutzt werden können).

## <a name="managed-identity-authentication-with-allow-trusted-services-configuration"></a>Authentifizierung mit verwalteten Identitäten mit der Konfiguration „Vertrauenswürdige Dienste zulassen“
Einige Azure-Dienste bieten die Netzwerkeinstellung **Vertrauenswürdige Microsoft-Dienste zulassen**, die, wenn sie aktiviert ist, ihren Stream Analytics-Aufträgen gestattet, sich mithilfe starker Authentifizierung sicher mit Ihrer Ressource zu verbinden. Mit dieser Option können Sie Ihre Aufträge mit Ihren Ein- und Ausgaberessourcen verbinden, ohne einen Stream Analytics-Cluster und private Endpunkte zu benötigen. Das Konfigurieren Ihres Auftrags für die Verwendung dieser Methode ist ein zweistufiger Vorgang:
* Verwenden Sie den Authentifizierungsmodus „Verwaltete Identität“, wenn Sie die Ein- oder Ausgabe in Ihrem Stream Analytics-Auftrag konfigurieren.
* Gewähren Sie Ihren spezifischen Stream Analytics-Aufträgen explizit Zugriff auf Ihre Zielressourcen, indem Sie der systemseitig zugewiesenen verwalteten Identität des Auftrags eine Azure-Rolle zuweisen. 

Durch das Aktivieren von **Vertrauenswürdige Microsoft-Diensten zulassen** wird kein globaler Zugriff auf alle Aufträge gewährt. Dadurch erhalten Sie vollständige Kontrolle darüber, welche spezifischen Stream Analytics-Aufträge sicher auf Ihre Ressourcen zugreifen können. 

Ihre Aufträge können mit dieser Methode eine Verbindung mit den folgenden Azure-Diensten herstellen:
1. [Blob Storage oder Azure Data Lake Storage Gen2](./blob-output-managed-identity.md): Kann das Speicherkonto oder die Streamingein- oder -ausgabe Ihres Auftrags sein.
2. [Azure Event Hubs](./event-hubs-managed-identity.md): Kann die Streamingein- oder -ausgabe Ihres Auftrags sein.

Wenn Ihre Aufträge eine Verbindung mit anderen Eingabe- oder Ausgabetypen herstellen müssen, können Sie mit Azure Functions zuerst aus Stream Analytics in die Event Hubs-Ausgabe und dann in ein beliebiges Ziel schreiben. Wenn Sie direkt aus Stream Analytics in andere Ausgabetypen schreiben möchten, die in einem VNet oder einer Firewall geschützt sind, besteht die einzige Möglichkeit darin, private Endpunkte in Stream Analytics-Clustern zu verwenden.

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen und Entfernen privater Endpunkte in Stream Analytics-Clustern](./private-endpoints.md)
* [Herstellen einer Verbindung mit Event Hubs in einem VNet unter Verwendung der Authentifizierung mit verwalteten Identitäten](./event-hubs-managed-identity.md)
* [Herstellen einer Verbindung mit Blob Storage und ADLS Gen2 in einem VNet mithilfe der Authentifizierung mit verwalteten Identitäten](./blob-output-managed-identity.md)