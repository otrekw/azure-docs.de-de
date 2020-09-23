---
title: Lokales Entwickeln und Debuggen von Azure Stream Analytics-Aufträgen
description: In diesem Artikel erfahren Sie, wie Sie Azure Stream Analytics-Aufträge auf dem lokalen Computer entwickeln und testen, bevor Sie sie im Azure-Portal ausführen.
ms.author: mamccrea
author: mamccrea
ms.topic: conceptual
ms.date: 03/31/2020
ms.service: stream-analytics
ms.openlocfilehash: 797577ce976f71f41ece0d103bd0145775d48b56
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90885584"
---
# <a name="develop-and-debug-azure-stream-analytics-jobs-locally"></a>Lokales Entwickeln und Debuggen von Azure Stream Analytics-Aufträgen

Obwohl Azure Stream Analytics-Aufträge im Azure-Portal erstellt und getestet werden können, bevorzugen viele Entwickler jedoch dafür eine lokale Entwicklungsumgebung. Mit Stream Analytics können Sie einfach den Code-Editor und die Entwicklungstools Ihrer Wahl zum Erstellen und Testen von Aufträgen mit Liveereignisstreams aus Azure Event Hub, IoT Hub und Blob Storage mit einer lokalen Laufzeit für einzelne Knoten mit vollem Funktionsumfang verwenden. Sie können Aufträge auch direkt aus Ihrer lokalen Entwicklungsumgebung an Azure senden.

## <a name="local-development-environments"></a>Lokale Entwicklungsumgebungen

Die Art und Weise, wie Sie Stream Analytics Aufträge auf dem lokalen Computer entwickeln, hängt von den Einstellungen der jeweiligen Tools und der Verfügbarkeit der Features ab. Unter [Vergleich der Features von Azure Stream Analytics](feature-comparison.md) erfahren Sie, welche Features für die jeweilige Entwicklungsumgebung unterstützt werden.

Die lokale Entwicklung wird in den in der folgenden Tabelle aufgeführten Umgebungen unterstützt:

|Environment                              |BESCHREIBUNG    |
|-----------------------------------------|------------|
|[Visual Studio Code](visual-studio-code-explore-jobs.md)| Mit der [Azure Stream Analytics-Tools-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-bigdatatools.vscode-asa) für Visual Studio Code können Sie Ihren Stream Analytics-Auftrag sowohl lokal als auch in der Cloud erstellen, verwalten und testen, und Sie können dabei auf eine umfassende IntelliSense- und native Quellcodeverwaltung vertrauen. Die Entwicklung unter Linux, macOS und Windows wird unterstützt. Weitere Informationen finden Sie unter [Erstellen eines Azure Stream Analytics-Auftrags in Visual Studio Code](quick-create-visual-studio-code.md). Die Erweiterung unterstützt auch [Visual Studio Codespaces](https://visualstudio.microsoft.com/services/visual-studio-codespaces/), wobei es sich um eine in der Cloud gehostete Entwicklungsumgebung handelt.|
|[Visual Studio 2019](stream-analytics-tools-for-visual-studio-install.md) |Die Stream Analytics-Tools sind Teil der Workloads Azure-Entwicklung und Datenspeicherung und -verarbeitung in Visual Studio. Sie können mithilfe von Visual Studio benutzerdefinierte C#-Funktionen und -Deserialisierer schreiben. Weitere Informationen finden Sie unter [Erstellen eines Azure Stream Analytics-Auftrags mithilfe von Visual Studio](stream-analytics-quick-create-vs.md).|
|[Eingabeaufforderung oder Terminal](stream-analytics-tools-for-visual-studio-cicd.md)|Das Azure Stream Analytics-CI/CD-NuGet-Paket bietet Tools für Visual Studio-Projektbuilds und lokale Tests auf einem beliebigen Computer. Das Azure Stream Analytics-CI/CD-npm-Paket bietet Tools für Visual Studio Code-Projektbuilds (wobei eine Azure Resource Manager-Vorlage erstellt wird) auf einem beliebigen Computer.|

## <a name="next-steps"></a>Nächste Schritte

* [Lokales Testen von Stream Analytics-Abfragen mit Beispieldaten mithilfe von Visual Studio Code](visual-studio-code-local-run.md)
* [Lokales Testen von Stream Analytics-Abfragen unter Verwendung einer Livestreameingabe mithilfe von Visual Studio Code](visual-studio-code-local-run-live-input.md)
* [Lokales Testen von Stream Analytics-Abfragen mit Visual Studio](stream-analytics-vs-tools-local-run.md)
* [Lokales Testen von Livedaten mithilfe von Azure Stream Analytics-Tools für Visual Studio](stream-analytics-live-data-local-testing.md)
