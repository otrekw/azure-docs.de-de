---
title: Beispielanwendung für den Zugriff auf kommerzielle Marketplace-Analysedaten
description: Verwenden Sie die Beispielanwendung, um Ihre eigene Analyseanwendung für den kommerziellen Marketplace zu erstellen.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: sayantanroy83
ms.author: sroy
ms.date: 3/08/2021
ms.openlocfilehash: 8fe2301c4d4ed2a582774c65d5dbe68bab416aa3
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102583528"
---
# <a name="sample-application-for-accessing-commercial-marketplace-analytics-data"></a>Beispielanwendung für den Zugriff auf kommerzielle Marketplace-Analysedaten

Die Beispielanwendungen wurden in den Sprachen C# und Java erstellt und sind auf [GitHub](https://github.com/partneranalytics) verfügbar.

- [C#-Beispielanwendung](https://github.com/partneranalytics/ProgrammaticExportSampleAppISV)
- [Java-Beispielanwendung](https://github.com/partneranalytics/ProgrammaticExportSampleAppISV_Java)

Sie können sich an der Beispielanwendung orientieren und Ihre eigene Anwendung mit einer beliebigen Programmiersprache erstellen.

Die Beispielanwendung erzielt Folgendes:

- Sie generiert ein Azure Active Directory-Token.
- Sie ruft verfügbare Datasets ab.
- Sie erstellt benutzerdefinierte Abfragen.
- Sie ruft benutzerdefinierte Abfragen und Systemabfragen ab.
- Sie plant einen Bericht.

Die Beispielanwendung umfasst die Methode zum Aufrufen von APIs für andere Funktionalitäten nicht. Allerdings bleibt das Verfahren zum Aufrufen anderer APIs wie oben beschrieben.

## <a name="how-to-run-the-application"></a>Ausführen der Anwendung

1. Klonen Sie das Repository mithilfe des folgenden Befehls:

    `git clone https://github.com/partneranalytics/ProgrammaticExportSampleAppISV.git`

    > [!NOTE]
    > Weitere Anweisungen finden Sie in der Datei `ProgrammaticExportSampleAppISV/README.md` im [GitHub-Repository](https://github.com/partneranalytics/ProgrammaticExportSampleAppISV.git).

1. Ändern Sie die Client-ID und den geheimen Clientschlüssel in der Datei **appsettings.Development.json**, um die App schnell auszuführen.

    :::image type="content" source="./media/analytics-programmatic-access/appsettings-development-json.png" alt-text="Ausschnitt der Datei „appsettings.Development.json“":::

Wenn Sie die App ausführen, wird ein lokaler Webserver gestartet und eine Seite wird geöffnet (`https://localhost:44365/ProgrammaticExportSampleApp/sample`).

[![Seite „Schedule Report“ (Bericht planen)](./media/analytics-programmatic-access/schedule-report.png)](./media/analytics-programmatic-access/schedule-report.png#lightbox)

Diese Seite führt API-Aufrufe an den Webserver aus, der auf dem lokalen Computer betrieben wird, der wiederum die API-Aufrufe für den tatsächlichen programmgesteuerten Zugriff ausführt.

## <a name="code-snippets"></a>Codeausschnitte

Die grundlegende Struktur des C#-Codes zum Ausführen von API-Aufrufen für den programmgesteuerten Zugriff lautet wie folgt:

:::image type="content" source="./media/analytics-programmatic-access/code-snippets.png" alt-text="Screenshot: API-Aufrufe":::

## <a name="next-steps"></a>Nächste Schritte

- [APIs für den Zugriff auf kommerzielle Marketplace-Analysedaten](analytics-available-apis.md)
