---
author: russell-cooks
ms.service: media-services
ms.topic: include
ms.date: 10/05/2020
ms.author: russellcooks
ms.openlocfilehash: 5dd823c618a0d49bef29c473e6c293762b859149
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "94358242"
---
1. Navigieren Sie in Visual Studio Code zu „src/edge“. Hier befinden sich die erstellte ENV-Datei sowie einige Bereitstellungsvorlagendateien.

    Die Bereitstellungsvorlage verweist mit einigen Platzhalterwerten auf das Bereitstellungsmanifest für das Edgegerät. Die ENV-Datei enthält die Werte für diese Variablen.
1. Navigieren Sie als Nächstes zum Ordner „src/cloud-to-device-console-app“. Hier befindet sich die Datei „appsettings.json“, die Sie zusammen mit folgenden anderen Dateien erstellt haben:

   * „operations.json“: In dieser Datei sind die verschiedenen Vorgänge aufgelistet, die im Programm ausgeführt werden sollen.
   * main.py: Dies ist der Beispielprogrammcode, über den folgende Vorgänge durchgeführt werden:
    
        * Laden der App-Einstellungen
        * Aufruf der direkten Methoden des Moduls Live Video Analytics in IoT Edge, um eine Topologie zu erstellen, den Graphen zu instanziieren und den Graphen zu aktivieren
        * Anhalten der Ausführung, damit Sie die Ausgabe des Graphen im **Terminalfenster** und die an IoT Hub gesendeten Ereignisse im **Ausgabefenster** untersuchen können
        * Deaktivieren der Graphinstanz, Löschen der Graphinstanz und Löschen der Graphtopologie
