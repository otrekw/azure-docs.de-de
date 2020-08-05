---
title: Troubleshooting für das Tool zum Kopieren von Daten in Azure Data Factory
description: In diesem Artikel wird beschrieben, wie Sie in Azure Data Factory ein Troubleshooting für das Tool zum Kopieren von Daten ausführen.
services: data-factory
documentationcenter: ''
author: dearandyxu
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: troubleshooting
ms.date: 07/28/2020
ms.author: yexu
ms.openlocfilehash: 428b8956b4a86009c946d9d8e9e9ba1c58d31403
ms.sourcegitcommit: 5b8fb60a5ded05c5b7281094d18cf8ae15cb1d55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87388304"
---
# <a name="troubleshoot-copy-data-tool-in-azure-data-factory"></a>Troubleshooting für das Tool zum Kopieren von Daten in Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

In diesem Artikel werden die gängigen Problembehandlungsmethoden für das Tool zum Kopieren von Daten in Azure Data Factory beschrieben.

## <a name="common-errors-and-messages"></a>Häufige Fehler und Meldungen

### <a name="error-code-unable-to-validate-in-copy-data-tool"></a>Fehlercode: Unable to validate in Copy Data tool (Das Tool zum Kopieren von Daten kann nicht überprüft werden)

- **Symptome:** Beim ersten auszuführenden Schritt im Tool zum Kopieren von Daten tritt die Warnung „Unable to validate“ (Kann nicht überprüft werden) auf.
- **Ursachen**: Dies könnte auftreten, wenn alle Drittanbietercookies deaktiviert sind.
- **Lösung:** 
    - Verwenden der Browser „Internet Explorer“ und „Microsoft Edge“.
    - Wenn Sie den Chrome-Browser verwenden, befolgen Sie die Anleitung unten, um unten Ausnahmen für Cookies für *microsoftonline.com* und *windows.net* hinzuzufügen.
        1.  Öffnen Sie den Chrome-Browser.
        2.  Klicken Sie auf das Schraubendrehersymbol bzw. auf die drei Linien auf der rechten Seite (Customize and control Google Chrome (Anpassen und Steuern von Google Chrome)).
        3.  Klicken Sie auf **Einstellungen**.
        4.  Suchen Sie nach **Cookies**, oder navigieren Sie unter „Advanced Settings“ (Erweiterte Einstellungen) zu **Privacy** (Datenschutz).
        5.  Klicken Sie auf **Content Settings** (Inhaltseinstellungen).    
        6.  Cookies sollten auf **allow local data to be set (recommended)** (lokale Daten dürfen festgelegt werden (empfohlen)) festgelegt werden.
        7.  Klicken Sie auf **Manage exceptions** (Ausnahmen verwalten). Geben Sie unter **hostname pattern** (Hostnamemuster) Folgendes ein, und sorgen Sie dafür, dass für das Verhalten **Allow** (Erlauben) festgelegt ist.
            - login.microsoftonline.com
            - login.windows.net
        8.  Schließen Sie den Browser, und starten Sie ihn neu.
    - Wenn Sie den Firefox-Browser verwenden, befolgen Sie die Anleitung unten, um Cookieausnahmen hinzuzufügen.
        1. Navigieren Sie im Firefox-Menü zu **Tools** > **Options** (Tools > Optionen).
        2. Unter **Privacy** > **History** (Datenschutz > Verlauf) wird möglicherweise angezeigt, dass die aktuelle Einstellung **Use Custom settings for history** (Benutzerdefinierte Einstellungen für den Verlauf verwenden) lautet.
        3. Unter **Accept third-party cookies** (Cookies von Drittanbietern akzeptieren) könnte aktuell die Einstellung **Never** (Nie) festgelegt sein. In diesem Fall sollten Sie rechts auf **Exceptions** (Ausnahmen) klicken, um folgende Websites hinzuzufügen.
            - https://login.microsoftonline.com
            - https://login.windows.net
        4.  Schließen Sie den Browser, und starten Sie ihn neu. 


### <a name="error-code-unable-to-open-login-page-and-enter-password"></a>Fehlercode: Unable to open login page and enter password (Das Öffnen der Anmeldeseite und das Eingeben eines Kennworts ist nicht möglich)

- **Symptome:** Das Tool zum Kopieren von Daten leitet Sie zur Anmeldeseite weiter. Diese wird jedoch nicht erfolgreich angezeigt.
- **Ursachen**: Dieses Problem kann auftreten, wenn Sie die Netzwerkumgebung von einem Büronetzwerk in ein privates Netzwerk geändert haben. In Browsern gibt es Zwischenspeicher. 
- **Lösung:** 
    1.  Schließen Sie den Browser, und versuchen Sie es noch mal. Fahren Sie mit dem nächsten Schritt fort, wenn das Problem weiterhin besteht.   
    2.  Wenn Sie den Internet Explorer verwenden, versuchen Sie, ihn im privaten Modus zu öffnen. Drücken Sie dazu STRG + UMSCHALT + P. Wenn Sie Chrome verwenden, versuchen Sie, ihn im Inkognitomodus zu öffnen. Drücken Sie dazu STRG + UMSCHALT + N. Fahren Sie mit dem nächsten Schritt fort, wenn das Problem weiterhin besteht. 
    3.  Verwenden Sie einen anderen Browser. 


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Problembehandlung finden Sie in diesen Ressourcen:
*  [Data Factory-Blog](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory-Funktionsanfragen](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure-Videos](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Frageseite von Microsoft Q&A (Fragen und Antworten)](https://docs.microsoft.com/answers/topics/azure-data-factory.html)
*  [Stack Overflow-Forum für Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Twitter-Informationen über Data Factory](https://twitter.com/hashtag/DataFactory)
*  [Anleitung zur Leistung und Optimierung der Mapping Data Flow-Funktion](concepts-data-flow-performance.md)
