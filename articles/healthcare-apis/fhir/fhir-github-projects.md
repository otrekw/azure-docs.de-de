---
title: Verwandte GitHub-Projekte für die Azure-API für die Azure-API
description: Listet alle Open Source-Repository (GitHub) für die Azure-API für die Vollversion von Azure auf.
services: healthcare-apis
author: ginalee-dotcom
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 02/01/2021
ms.author: ginle
ms.openlocfilehash: 9977765183bd567377592631d65f3e548bef4b34
ms.sourcegitcommit: ed7376d919a66edcba3566efdee4bc3351c57eda
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2021
ms.locfileid: "105097633"
---
# <a name="related-github-projects"></a>Verwandte GitHub-Projekte

Wir haben viele Open-Source-Projekte auf GitHub, die Ihnen den Quellcode und Anweisungen zum Bereitstellen von Diensten für verschiedene Verwendungszwecke bereitstellen. Sie sind immer willkommen, wenn Sie unsere GitHub-Repository besuchen, um sich mit unseren Features und Produkten vertraut zu machen. 

## <a name="fhir-server"></a>F-Server
* [Microsoft/f-Server](https://github.com/microsoft/fhir-server/): Open-Source-Server für den Schutz von Servern, der die Grundlage für die Azure-API für die Azure-API ist
* Die neuesten Releases finden Sie in den Anmerkungen zu dieser [Version](https://github.com/microsoft/fhir-server/releases) .
* [Microsoft/f-Server-Samples](https://github.com/microsoft/fhir-server-samples): eine Beispiel Umgebung

## <a name="data-conversion--anonymization"></a>Datenkonvertierung & Anonymisierung

#### <a name="fhir-converter"></a>F-Konverter
* [Microsoft/fhir-Converter](https://github.com/microsoft/FHIR-Converter): ein Konvertierungs Hilfsprogramm zum Übersetzen von Legacy Datenformaten in fhir
* Integriert in die Azure-API für die Verwendung von Azure-APIs für die Verwendung von Azure-APIs für Azure in Form eines $Convert-Data-Vorgangs
* Fortlaufende Verbesserungen in OSS und Continuous Integration in die fhir-Server
 
#### <a name="fhir-converter---vs-code-extension"></a>"F"-vs Code Erweiterung
* [Microsoft/f-Tools-for-Anonymisierung](https://github.com/microsoft/FHIR-Tools-for-Anonymization): eine Reihe von Tools für die Unterstützung von Daten (im f-Format)
* Integriert in die Azure-API für die Verwendung von Azure-APIs für die Verwendung von "de-identifizierter Export"

#### <a name="fhir-tools-for-anonymization"></a>F-Tools für die Anonymisierung
* [Microsoft/vscode-azurehealthcareapis-Tools](https://github.com/microsoft/vscode-azurehealthcareapis-tools): eine vs Code Erweiterung, die eine Sammlung von Tools für die Arbeit mit Azure Healthcare-APIs enthält.
* Veröffentlicht für Visual Studio Marketplace
* Wird zum Erstellen von Liquid-Vorlagen verwendet, die im fhir-Konverter verwendet werden sollen.

## <a name="iot-connector"></a>IoT-Konnektor

#### <a name="integration-with-iot-hub-and-iot-central"></a>Integration in IOT Hub und IOT Central
* [Microsoft/iomt-fhir](https://github.com/microsoft/iomt-fhir): Integration mit IOT Hub oder IOT Central in fhir mit Daten Normalisierung und fhir-Konvertierung der normalisierten Daten
* Normalisierung: Gerätedaten Informationen werden zur weiteren Verarbeitung in ein gängiges Format extrahiert.
* Fhir-Konvertierung: normalisierte und gruppierte Daten sind fhir zugeordnet. Beobachtungen werden gemäß konfigurierten Vorlagen erstellt oder aktualisiert und mit dem Gerät und dem Patienten verknüpft.
* [Tools zum Erstellen der Konversations](https://github.com/microsoft/iomt-fhir/tree/master/tools/data-mapper)Zuordnung: visualisieren Sie die Zuordnungs Konfiguration für das Normalisieren der Geräte Eingabedaten, und wandeln Sie Sie in die fhir-Ressourcen um. Entwickler können dieses Tool verwenden, um die Zuordnungen, die Geräte Zuordnung und die Zuordnung von fhir zu bearbeiten und zu testen und Sie zum Hochladen in den IOT-Connector in der Azure-Portal zu exportieren.

#### <a name="healthkit-and-fhir-integration"></a>Healthkit-und f-Integration
* [Microsoft/healthkit-on-fhir](https://github.com/microsoft/healthkit-on-fhir): eine SWIFT-Bibliothek, die den Export von Apple healthkit-Daten auf einen fhir-Server automatisiert

 