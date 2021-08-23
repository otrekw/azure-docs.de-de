---
title: Azure Stream Analytics – Vorschaufeatures
description: In diesem Artikel werden die Azure Stream Analytics-Features beschrieben, die sich derzeit in der Vorschau befinden.
author: enkrumah
ms.author: ebnkruma
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/16/2021
ms.openlocfilehash: 3f6b46425954d8befaef396c66b023565310ec36
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122339793"
---
# <a name="azure-stream-analytics-preview-features"></a>Azure Stream Analytics – Vorschaufeatures

Dieser Artikel fasst alle Features zusammen, die sich derzeit in der Vorschau für Azure Stream Analytics befinden. Die Verwendung von Vorschaufeatures in einer Produktionsumgebung wird nicht empfohlen.

## <a name="authenticate-to-sql-database-output-with-managed-identities-preview"></a>Authentifizieren bei der SQL-Datenbank-Ausgabe mit verwalteten Identitäten (Vorschau)

Azure Stream Analytics unterstützt die [Authentifizierung über verwaltete Identitäten](../active-directory/managed-identities-azure-resources/overview.md) für Azure SQL-Datenbank-Ausgabesenken. Verwaltete Identitäten heben die Einschränkungen benutzerbasierter Authentifizierungsmethoden auf, wie etwa die Notwendigkeit einer erneuten Authentifizierung aufgrund von Kennwortänderungen. 

## <a name="real-time-high-performance-scoring-with-custom-ml-models-managed-by-azure-machine-learning"></a>Leistungsstarke Echtzeitbewertung mit benutzerdefinierten ML-Modellen, die von Azure Machine Learning verwaltet werden

Azure Stream Analytics unterstützt die leistungsstarke Echtzeitbewertung anhand benutzerdefinierter, bereits trainierter ML-Modelle, die von Azure Machine Learning verwaltet und in Azure Kubernetes Service (AKS) oder Azure Container Instances (ACI) gehostet werden; dabei wird ein Workflow verwendet, für den Sie keinen Code schreiben müssen. [Registrieren Sie sich](https://aka.ms/asapreview1) für das Vorschaufeature.

## <a name="c-custom-de-serializers"></a>Benutzerdefinierte C#-Deserialisierer
Entwickler können von der Leistung von Azure Stream Analytics profitieren und Daten in Protobuf, XML oder einem beliebigen benutzerdefinierten Format verarbeiten. Sie können [benutzerdefinierte Deserialisierer](custom-deserializer-examples.md) in C# implementieren, um Ereignisse zu deserialisieren, die von Azure Stream Analytics empfangen wurden.

## <a name="extensibility-with-c-custom-code"></a>Erweiterbarkeit durch benutzerdefinierten C#-Code

Entwickler, die Stream Analytics-Module in der Cloud oder in IoT Edge erstellen, können benutzerdefinierte C#-Funktionen schreiben oder wiederverwenden und diese über [benutzerdefinierte Funktionen](stream-analytics-edge-csharp-udf-methods.md) direkt in der Abfrage abrufen.

## <a name="debug-queries-locally-using-job-diagram-in-visual-studio-code"></a>Lokales Debuggen von Abfragen mithilfe eines Auftragsdiagramms in Visual Studio Code

Sie können das Auftragsdiagramm verwenden, während Sie Ihre Abfrage lokal testen, um das Zwischenresultset und die Zwischenmetriken für jeden Schritt zu überprüfen.

## <a name="explore-jobs-in-visual-studio-code"></a>Erkunden von Aufträgen in Visual Studio Code

Stream Analytics-Explorer in Visual Studio Code bietet Entwicklern eine einfache Verwaltung ihrer Stream Analytics-Aufträge. Im Stream Analytics-Explorer können Sie Ihre Aufträge problemlos verwalten, das Auftragsdiagramm anzeigen und im Auftragsmonitor debuggen.

## <a name="debug-query-steps-in-visual-studio"></a>Schritte zum Debuggen von Abfragen in Visual Studio

Sie können auf einfache Weise eine Vorschau des Zwischenrowsets für ein Datendiagramm aufrufen, wenn Sie lokale Tests in Azure Stream Analytics-Tools für Visual Studio ausführen. 


## <a name="live-data-testing-in-visual-studio"></a>Livedatentests in Visual Studio

Visual Studio-Tools für Azure Stream Analytics erweitern die lokale Testfunktion, mit der Sie Ihre Abfragen anhand von Liveereignisstreams aus Cloudquellen wie Event Hub oder IoT Hub testen können. Erfahren Sie mehr über [Lokales Testen von Livedaten mithilfe von Azure Stream Analytics-Tools für Visual Studio](stream-analytics-live-data-local-testing.md).


