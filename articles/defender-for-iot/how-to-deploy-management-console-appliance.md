---
title: Bereitstellen der Verwaltungskonsole in Azure Defender für IoT
description: Hier erfahren Sie, wie Sie die Verwaltungskonsole in Azure Defender für IoT bereitstellen können.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/14/2020
ms.author: rkarlin
ms.openlocfilehash: 6af943951b2dba0c4a11b08ad1bcd961cf1c0fc2
ms.sourcegitcommit: 30505c01d43ef71dac08138a960903c2b53f2499
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2020
ms.locfileid: "92094153"
---
# <a name="deploy-the-management-console"></a>Bereitstellen der Verwaltungskonsole
In diesem Artikel wird beschrieben, wie Sie die lokale Verwaltungskonsole von Azure Defender für IoT bereitstellen können.

## <a name="the-on-premises-management-console"></a>Die lokale Verwaltungskonsole

Die lokale Verwaltungskonsole stellt eine konsolidierte Ansicht aller Netzwerkressourcen bereit und bietet eine Echtzeitansicht der wichtigsten IoT- und OT-Risikoindikatoren sowie Warnungen für alle Ihre Einrichtungen. Die enge Integration in Ihre SOC-Workflows und Runbooks ermöglicht eine einfache Priorisierung von Entschärfungsaktivitäten und standortübergreifende Korrelation von Bedrohungen.

- Ganzheitlich – Reduzieren der Komplexität mit einer einzigen vereinheitlichten Plattform für Ressourcenverwaltung, Verwaltung von Risiken und Sicherheitsrisiken sowie Bedrohungsüberwachung mit Reaktion auf Vorfälle.

- Aggregation und Korrelation – Anzeigen, Aggregieren und Analysieren von Daten und Warnungen, die aus allen Standorten gesammelt wurden.

- Alle Sensoren steuern – Konfigurieren und Überwachen aller Sensoren von einem einzigen Ort aus.

   ![Azure Defender für IoT – Ansicht der Standortverwaltung](media/updates/image2.png)

## <a name="deploy-the-on-premises-management-console-appliance"></a>Bereitstellen der Appliance für die lokale Verwaltungskonsole

Für diesen Prozess müssen Sie Ihre eigene Hardware erwerben und Software installieren. Die Lösung wird auf zertifizierten Appliances ausgeführt. Informationen zum Kauf Ihrer zertifizierten Appliance finden Sie im [Azure Defender for IoT hardware specifications guide](https://aka.ms/AzureDefenderforIoTBareMetalAppliance) (Leitfaden zu den Hardwarespezifikationen für Azure Defender für IoT).

Ausführliche Informationen zum Herunterladen des ISO-Images und Installieren der Sensorsoftware finden Sie im [Azure Defender for IoT Installation Guide](https://aka.ms/AzureDefenderforIoTInstallSensorISO) (Azure Defender für IoT-Installationshandbuch).

**So stellen Sie die lokale Verwaltungskonsole bereit:**

1. Navigieren Sie zu Microsoft Azure Defender für IoT.

2. Wählen Sie **Lokale Verwaltungskonsole** aus.

   ![Ansicht der lokalen Verwaltungskonsole von Azure Defender für IoT](media/updates/image15.png)

3. Wählen Sie im Menü **Version auswählen** „Version 3.1“ aus.

4. Wählen Sie **Herunterladen** aus, und speichern Sie die Datei.

5. Nachdem die Software installiert wurde, führen Sie die Aufgaben zur Netzwerkeinrichtung aus. Ausführliche Informationen finden Sie im [Azure Defender for IoT Network Setup Guide](https://aka.ms/AzureDefenderForIoTNetworkSetup) (Handbuch zur Einrichtung des Azure Defender für IoT-Netzwerks).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Konfigurationsoptionen finden Sie in der Schrittanleitung für die Modulkonfiguration.
> [!div class="nextstepaction"]
> [Schrittanleitung für die Modulkonfiguration](./how-to-agent-configuration.md)
