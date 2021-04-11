---
title: Bereitstellen der Azure Industrial IoT-Plattform
description: In diesem Tutorial erfahren Sie, wie Sie die IIoT-Plattform bereitstellen.
author: jehona-m
ms.author: jemorina
ms.service: industrial-iot
ms.topic: tutorial
ms.date: 3/22/2021
ms.openlocfilehash: 87a7295c785c08fcf3faffc20d34ceef45144848
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104787288"
---
# <a name="tutorial-deploy-the-azure-industrial-iot-platform"></a>Tutorial: Bereitstellen der Azure Industrial IoT-Plattform

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Informationen zu den Hauptkomponenten der IIoT-Plattform
> * Informationen zu den verschiedenen Installationsarten
> * Bereitstellen der Industrial IoT-Plattform

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement muss erstellt werden.
- Laden Sie [Git](https://git-scm.com/downloads) herunter.
- Für die AAD-App-Registrierungen (Azure Active Directory), die für die Authentifizierung verwendet werden, sind Rechte vom Typ „Globaler Administrator“, „Anwendungsadministrator“ oder „Cloudanwendungsadministrator“ erforderlich, um eine mandantenweite Administratoreinwilligung zu ermöglichen. (Weitere Optionen finden Sie weiter unten.)
- Für die Bereitstellung werden die Betriebssysteme Windows, Linux und Mac unterstützt.
- Von IoT Edge werden Windows 10 IoT Enterprise LTSC und Ubuntu Linux 16.08/18.04 LTS Linux unterstützt.

## <a name="main-components"></a>Hauptkomponenten

- Mindestens erforderliche Abhängigkeiten: IoT Hub, Cosmos DB, Service Bus, Event Hub, Key Vault, Storage
- Standardabhängigkeiten: SignalR-Dienst, AAD-App-Registrierungen, Device Provisioning Service, Time Series Insights, Arbeitsmappe, Log Analytics, Application Insights (zusätzlich zu den mindestens erforderlichen Abhängigkeiten)
- Microservices: App Service-Plan, App Service
- Benutzeroberfläche (Web-App): App Service-Plan (gemeinsam mit Microservices verwendet), App Service
- Simulation: Virtueller Computer, virtuelles Netzwerk, IoT Edge
- Azure Kubernetes Service

## <a name="installation-types"></a>Installationstypen

- Minimal: Mindestens erforderliche Abhängigkeiten
- Lokal: Mindestens erforderliche Abhängigkeiten und Standardabhängigkeiten
- Dienste: Lokal und Microservices
- Simulation: Mindestens erforderliche Abhängigkeiten Simulationskomponenten
- App: Dienste und Benutzeroberfläche
- Alle (Standard): App und Simulation

## <a name="deployment"></a>Bereitstellung

1. Klonen Sie zunächst das Repository über die Eingabeaufforderung oder über das Terminal, um mit der Bereitstellung der IIoT-Plattform zu beginnen.

    git clone https://github.com/Azure/Industrial-IoT  cd Industrial-IoT

2. Starten Sie die geführte Bereitstellung. Daraufhin werden durch das Skript die erforderlichen Informationen wie Azure-Konto, Abonnement, Zielressource sowie Gruppen- und Anwendungsname erfasst.

Unter Windows:
    ```
    .\deploy
    ```

Unter Linux oder auf einem Mac:
    ```
    ./deploy.sh
    ```

3. Die Microservices und die Benutzeroberfläche sind Webanwendungen, die eine Authentifizierung erfordern. Hierzu sind drei App-Registrierungen in AAD erforderlich. Sollten die erforderlichen Rechte fehlen, gibt es zwei Möglichkeiten:

- Bitten Sie den AAD-Administrator, der Anwendung eine mandantenweite Administratoreinwilligung zu erteilen.
- Ein AAD-Administrator kann die AAD-Anwendungen erstellen. Der Ordner „deploy/scripts“ enthält das Skript „aad-register.ps1“, mit dem die AAD-Registrierung getrennt von der Bereitstellung durchgeführt werden kann. Durch das Skript wird eine Datei mit den relevanten Informationen für die Bereitstellung ausgegeben, die unter Verwendung des Arguments „-aadConfig“ an das Skript „deploy.ps1“ im gleichen Ordner übergeben werden muss.
    ```bash
    cd deploy/scripts
    ./aad-register.ps1 -Name <application-name> -Output aad.json
    ./deploy.ps1 -aadConfig aad.json
    ```

Bei Produktionsbereitstellungen, die Staging, Rollback, Skalierung und Resilienz erfordern, kann die Plattform in [Azure Kubernetes Service (AKS)](https://github.com/Azure/Industrial-IoT/blob/master/docs/deploy/howto-deploy-aks.md) bereitgestellt werden.

Referenzen:
- [Bereitstellen der Azure Industrial IoT-Plattform](https://github.com/Azure/Industrial-IoT/tree/master/docs/deploy)
- [All-in-One-Bereitstellung](https://github.com/Azure/Industrial-IoT/blob/master/docs/deploy/howto-deploy-all-in-one.md)
- [Bereitstellen der Plattform in AKS](https://github.com/Azure/Industrial-IoT/blob/master/docs/deploy/howto-deploy-aks.md)


## <a name="next-steps"></a>Nächste Schritte
Nach dem Bereitstellen der IIoT-Plattform können Sie sich mit dem Anpassen der Komponentenkonfiguration befassen:

> [!div class="nextstepaction"]
> [Tutorial: Konfigurieren der Industrial IoT-Komponenten](tutorial-configure-industrial-iot-components.md)
