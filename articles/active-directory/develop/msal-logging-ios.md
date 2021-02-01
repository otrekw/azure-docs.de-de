---
title: Protokollieren von Fehlern und Ausnahmen in MSAL für iOS/macOS
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie Fehler und Ausnahmen in MSAL für iOS/macOS protokollieren
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 01/25/2021
ms.author: marsma
ms.reviewer: saeeda, jmprieur
ms.custom: aaddev
ms.openlocfilehash: ee3837b75d586238e7ca6ac85434cc56f592929d
ms.sourcegitcommit: 3c3ec8cd21f2b0671bcd2230fc22e4b4adb11ce7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2021
ms.locfileid: "98763254"
---
# <a name="logging-in-msal-for-iosmacos"></a>Protokollierung in MSAL für iOS/macOS

[!INCLUDE [MSAL logging introduction](../../../includes/active-directory-develop-error-logging-introduction.md)]

## <a name="objective-c"></a>[Objective-C](#tab/objc)

## <a name="msal-for-ios-and-macos-logging-objc"></a>MSAL für iOS- und macOS-Protokollierung – ObjC

Legen Sie einen Rückruf fest, um die MSAL-Protokollierung zu erfassen und in die Protokollierung Ihrer eigenen Anwendung zu integrieren. Die Signatur für den Rückruf sieht folgendermaßen aus:

```objc
/*!
    The LogCallback block for the MSAL logger
 
    @param  level           The level of the log message
    @param  message         The message being logged
    @param  containsPII     If the message might contain Personally Identifiable Information (PII)
                            this will be true. Log messages possibly containing PII will not be
                            sent to the callback unless PIllLoggingEnabled is set to YES on the
                            logger.

 */
typedef void (^MSALLogCallback)(MSALLogLevel level, NSString *message, BOOL containsPII);
```

Beispiel:

```objc
[MSALGlobalConfig.loggerConfig setLogCallback:^(MSALLogLevel level, NSString *message, BOOL containsPII)
    {
        if (!containsPII)
        {
#if DEBUG
            // IMPORTANT: MSAL logs may contain sensitive information. Never output MSAL logs with NSLog, or print, directly unless you're running your application in debug mode. If you're writing MSAL logs to file, you must store the file securely.
            NSLog(@"MSAL log: %@", message);
#endif
        }
    }];
```

### <a name="personal-data"></a>Personenbezogene Daten

Standardmäßig erfasst bzw. protokolliert MSAL keine personenbezogenen Daten. Die Bibliothek erlaubt App-Entwicklern jedoch, diese Möglichkeit über eine Eigenschaft in der MSALLogger-Klasse zu aktivieren. Nach der Aktivierung von `pii.Enabled` übernimmt die App die Verantwortung für den sicheren Umgang mit hochgradig sensiblen Daten sowie für die Einhaltung gesetzlicher Anforderungen.

```objc
// By default, the `MSALLogger` doesn't capture any PII

// PII will be logged
MSALGlobalConfig.loggerConfig.piiEnabled = YES;

// PII will NOT be logged
MSALGlobalConfig.loggerConfig.piiEnabled = NO;
```

### <a name="logging-levels"></a>Protokolliergrade

Verwenden Sie einen der folgenden Werte, um den Protokolliergrad festzulegen, wenn Sie die Protokollierung mit MSAL für iOS und macOS ausführen:

|Ebene  |Beschreibung |
|---------|---------|
| `MSALLogLevelNothing`| Deaktivieren der gesamten Protokollierung |
| `MSALLogLevelError` | Standardgrad, gibt Informationen nur aus, wenn Fehler auftreten. |
| `MSALLogLevelWarning` | Warnungen |
| `MSALLogLevelInfo` |  Bibliothekseinstiegspunkte mit Parametern und verschiedenen Keychainvorgängen. |
|`MSALLogLevelVerbose`     |  API-Ablaufverfolgung |

Beispiel:

```objc
MSALGlobalConfig.loggerConfig.logLevel = MSALLogLevelVerbose;
 ```

 ### <a name="log-message-format"></a>Protokollieren des Nachrichtenformats

Der Nachrichtenteil der MSAL-Protokollmeldungen weist das Format `TID = <thread_id> MSAL <sdk_ver> <OS> <OS_ver> [timestamp - correlation_id] message` auf.

Beispiel:

`TID = 551563 MSAL 0.2.0 iOS Sim 12.0 [2018-09-24 00:36:38 - 36764181-EF53-4E4E-B3E5-16FE362CFC44] acquireToken returning with error: (MSALErrorDomain, -42400) User cancelled the authorization session.`

Das Bereitstellen von Korrelations-IDs und Zeitstempeln ist hilfreich zum Nachverfolgen von Problemen. Informationen zum Zeitstempel und zur Korrelations-ID sind in der Protokollmeldung verfügbar. Der einzige zuverlässige Ort zum Abrufen dieser Informationen sind die MSAL-Protokollierungsmeldungen.

## <a name="swift"></a>[Swift](#tab/swift)

## <a name="msal-for-ios-and-macos-logging-swift"></a>MSAL für iOS- und macOS-Protokollierung – Swift

Legen Sie einen Rückruf fest, um die MSAL-Protokollierung zu erfassen und in die Protokollierung Ihrer eigenen Anwendung zu integrieren. Die Signatur (dargestellt in Objective-C) für den Rückruf sieht wie folgt aus:

```objc
/*!
    The LogCallback block for the MSAL logger
 
    @param  level           The level of the log message
    @param  message         The message being logged
    @param  containsPII     If the message might contain Personally Identifiable Information (PII)
                            this will be true. Log messages possibly containing PII will not be
                            sent to the callback unless PIllLoggingEnabled is set to YES on the
                            logger.

 */
typedef void (^MSALLogCallback)(MSALLogLevel level, NSString *message, BOOL containsPII);
```

Beispiel:

```swift
MSALGlobalConfig.loggerConfig.setLogCallback { (level, message, containsPII) in
    if let message = message, !containsPII
    {
#if DEBUG
    // IMPORTANT: MSAL logs may contain sensitive information. Never output MSAL logs with NSLog, or print, directly unless you're running your application in debug mode. If you're writing MSAL logs to file, you must store the file securely.
    print("MSAL log: \(message)")
#endif
    }
}
```

### <a name="personal-data"></a>Personenbezogene Daten

Standardmäßig erfasst bzw. protokolliert MSAL keine personenbezogenen Daten. Die Bibliothek erlaubt App-Entwicklern jedoch, diese Möglichkeit über eine Eigenschaft in der MSALLogger-Klasse zu aktivieren. Nach der Aktivierung von `pii.Enabled` übernimmt die App die Verantwortung für den sicheren Umgang mit hochgradig sensiblen Daten sowie für die Einhaltung gesetzlicher Anforderungen.

```swift
// By default, the `MSALLogger` doesn't capture any PII

// PII will be logged
MSALGlobalConfig.loggerConfig.piiEnabled = true

// PII will NOT be logged
MSALGlobalConfig.loggerConfig.piiEnabled = false
```

### <a name="logging-levels"></a>Protokolliergrade

Verwenden Sie einen der folgenden Werte, um den Protokolliergrad festzulegen, wenn Sie die Protokollierung mit MSAL für iOS und macOS ausführen:

|Ebene  |Beschreibung |
|---------|---------|
| `MSALLogLevelNothing`| Deaktivieren der gesamten Protokollierung |
| `MSALLogLevelError` | Standardgrad, gibt Informationen nur aus, wenn Fehler auftreten. |
| `MSALLogLevelWarning` | Warnungen |
| `MSALLogLevelInfo` |  Bibliothekseinstiegspunkte mit Parametern und verschiedenen Keychainvorgängen. |
|`MSALLogLevelVerbose`     |  API-Ablaufverfolgung |

Beispiel:

```swift
MSALGlobalConfig.loggerConfig.logLevel = .verbose
 ```

### <a name="log-message-format"></a>Protokollieren des Nachrichtenformats

Der Nachrichtenteil der MSAL-Protokollmeldungen weist das Format `TID = <thread_id> MSAL <sdk_ver> <OS> <OS_ver> [timestamp - correlation_id] message` auf.

Beispiel:

`TID = 551563 MSAL 0.2.0 iOS Sim 12.0 [2018-09-24 00:36:38 - 36764181-EF53-4E4E-B3E5-16FE362CFC44] acquireToken returning with error: (MSALErrorDomain, -42400) User cancelled the authorization session.`

Das Bereitstellen von Korrelations-IDs und Zeitstempeln ist hilfreich zum Nachverfolgen von Problemen. Informationen zum Zeitstempel und zur Korrelations-ID sind in der Protokollmeldung verfügbar. Der einzige zuverlässige Ort zum Abrufen dieser Informationen sind die MSAL-Protokollierungsmeldungen.

---

## <a name="next-steps"></a>Nächste Schritte

Weitere Codebeispiele finden Sie unter [Microsoft Identity Platform-Codebeispiele](sample-v2-code.md).