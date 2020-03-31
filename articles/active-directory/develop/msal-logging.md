---
title: Protokollierung in MSAL-Apps | Azure
titleSuffix: Microsoft identity platform
description: Hier finden Sie Informationen zur Protokollierung in Microsoft Authentication Library-Anwendungen (MSAL-Anwendungen).
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/11/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 58697cc535357710c6889f05060b5e04e129ae7d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "77084896"
---
# <a name="logging-in-msal-applications"></a>Protokollierung in MSAL-Anwendungen

Mit Microsoft Authentication Library-Apps (MSAL-Apps) werden Protokollmeldungen generiert, die Sie beim Diagnostizieren von Problemen unterstützen. Eine App kann die Protokollierung mit wenigen Codezeilen konfigurieren und hat benutzerdefinierte Kontrolle über den Detailgrad und darüber, ob personenbezogene Daten und Organisationsdaten protokolliert werden oder nicht. Es wird empfohlen, einen MSAL-Protokollierungsrückruf festzulegen und Benutzern eine Möglichkeit zu bieten, Protokolle zu übermitteln, wenn Authentifizierungsprobleme auftreten.

## <a name="logging-levels"></a>Protokolliergrade

MSAL bietet mehrere Grade für Protokollierungsdetails:

- Error: Gibt an, dass ein Problem aufgetreten ist und ein Fehler generiert wurde. Verwenden Sie diesen Grad zum Debuggen und Identifizieren von Problemen.
- Warnung: Es muss nicht unbedingt zu einem Fehler oder Ausfall gekommen sein. Dieser Grad ist für das Diagnostizieren und Ermitteln von Problemen vorgesehen.
- Info: MSAL protokolliert Ereignisse, die für Informationszwecke und nicht unbedingt für das Debuggen vorgesehen sind.
- Verbose: Standard. Msal protokolliert die vollständigen Details des Bibliotheksverhaltens.

## <a name="personal-and-organizational-data"></a>Personenbezogene Daten und Organisationsdaten

Die MSAL-Protokollierung erfasst standardmäßig keine streng vertraulichen personenbezogenen Daten oder Organisationsdaten. Die Bibliothek bietet jedoch die Option, die Protokollierung von personenbezogenen Daten und Organisationsdaten zu aktivieren.

Ausführliche Informationen zur MSAL-Protokollierung in einer bestimmten Sprache finden Sie auf der entsprechenden Registerkarte:

## <a name="net"></a>[.NET](#tab/dotnet)

## <a name="logging-in-msalnet"></a>Protokollierung in MSAL.NET

 > [!NOTE]
 > Beispiele für die MSAL.NET-Protokollierung und weitere Infos finden Sie unter [MSAL.NET Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki).

In MSAL 3.x wird die Protokollierung bei der App-Erstellung pro Anwendung mit dem Generatormodifizierer `.WithLogging` festgelegt. Bei dieser Methode können optionale Parameter verwendet werden:

- Mit `Level` können Sie selbst über die gewünschte Protokollierungsebene entscheiden. Wenn Sie den Parameter auf „Errors“ festlegen, werden nur Fehler protokolliert.
- Mit `PiiLoggingEnabled` können Sie personenbezogene und organisationsbezogene Daten protokollieren, wenn die Option auf „true“ festgelegt ist. Dieser Parameter ist standardmäßig auf „false“ festgelegt, damit Ihre Anwendung keine personenbezogenen Daten protokolliert.
- `LogCallback` ist auf einen Delegate festgelegt, der die Protokollierung ausführt. Wenn `PiiLoggingEnabled`auf „true“ festgelegt ist, wird die Meldung von dieser Methode zweimal empfangen: einmal mit dem Parameter `containsPii`gleich „false“ und die Meldung enthält keine personenbezogenen Daten, und ein zweites Mal mit dem Parameter `containsPii`gleich „true“, wobei die Meldung personenbezogene Daten enthalten kann. In einigen Fällen (wenn die Nachricht keine personenbezogenen Daten enthält) ist die Nachricht dieselbe.
- `DefaultLoggingEnabled`Aktiviert die standardmäßige Protokollierung für die Plattform. Der Parameter ist standardmäßig auf „false“ festgelegt. Wenn Sie den Parameter auf „true“ festlegen, wird in Desktop-/UWP-Anwendungen die Ereignisablaufverfolgung, unter iOS NSLog und unter Android Logcat verwendet.

```csharp
class Program
 {
  private static void Log(LogLevel level, string message, bool containsPii)
  {
     if (containsPii)
     {
        Console.ForegroundColor = ConsoleColor.Red;
     }
     Console.WriteLine($"{level} {message}");
     Console.ResetColor();
  }

  static void Main(string[] args)
  {
    var scopes = new string[] { "User.Read" };

    var application = PublicClientApplicationBuilder.Create("<clientID>")
                      .WithLogging(Log, LogLevel.Info, true)
                      .Build();

    AuthenticationResult result = application.AcquireTokenInteractive(scopes)
                                             .ExecuteAsync().Result;
  }
 }
 ```

## <a name="android"></a>[Android](#tab/android)

## <a name="logging-in-msal-for-android-using-java"></a>Protokollierung in MSAL für Android mithilfe von Java

Aktivieren Sie die Protokollierung bei der App-Erstellung durch Erstellen eines Protokollierungsrückrufs. Der Rückruf übernimmt die folgenden Parameter:

- `tag` ist eine Zeichenfolge, die von der Bibliothek an den Rückruf übermittelt wird. Sie ist dem Protokolleintrag zugeordnet und kann zum Sortieren von Protokollierungsmeldungen verwendet werden.
- Mit `logLevel` können Sie selbst über die gewünschte Protokollierungsebene entscheiden. Die unterstützten Protokollierungsebenen sind: `Error`, `Warning`, `Info` und `Verbose`.
- `message` ist der Inhalt des Protokolleintrags.
- `containsPII` gibt an, ob Nachrichten, die personenbezogene Daten oder Organisationsdaten enthalten, protokolliert werden. Dieser Parameter ist standardmäßig auf „false“ festgelegt, damit Ihre Anwendung keine personenbezogenen Daten protokolliert. Wenn `containsPII` aber `true` ist, empfängt diese Methode die Meldung zweimal: einmal mit dem Parameter `containsPII` auf `false` festgelegt und `message` ohne personenbezogene Daten, und ein zweites Mal mit dem Parameter `containsPii` auf `true` festgelegt, wobei die Meldung personenbezogene Daten enthalten könnte. In einigen Fällen (wenn die Nachricht keine personenbezogenen Daten enthält) ist die Nachricht dieselbe.

```java
private StringBuilder mLogs;

mLogs = new StringBuilder();
Logger.getInstance().setExternalLogger(new ILoggerCallback()
{
   @Override
   public void log(String tag, Logger.LogLevel logLevel, String message, boolean containsPII)
   {
      mLogs.append(message).append('\n');
   }
});
```

Standardmäßig werden von der MSAL-Protokollierung keine personenbezogenen Informationen oder identifizierbaren Informationen für die Organisation erfasst.
So aktivieren Sie die Protokollierung von personenbezogenen Informationen (PII) und organisationsbezogenen Informationen (OII):

```java
Logger.getInstance().setEnablePII(true);
```

So deaktivieren Sie die Protokollierung persönlicher Daten und Organisationsdaten:

```java
Logger.getInstance().setEnablePII(false);
```

Standardmäßig ist die Protokollierung für logcat deaktiviert. So aktivieren Sie Firewallregeln:

```java
Logger.getInstance().setEnableLogcatLog(true);
```

## <a name="javascript"></a>[JavaScript](#tab/javascript)

 Aktivieren Sie die Protokollierung in MSAL.js (JavaScript), indem Sie beim Konfigurieren ein Protokollierungsobjekt zum Erstellen einer Instanz vom Typ `UserAgentApplication` übergeben. Dieses Protokollierungsobjekt weist die folgenden Eigenschaften auf:

- `localCallback`: Eine Rückrufinstanz, die vom Entwickler bereitgestellt werden kann, um Protokolle benutzerdefiniert zu verarbeiten und zu veröffentlichen. Implementieren Sie die „localCallback“-Methode abhängig davon, wie Sie Protokolle umleiten möchten.
- `level` (optional): Der konfigurierbare Protokolliergrad. Die unterstützten Protokollierungsebenen sind: `Error`, `Warning`, `Info` und `Verbose`. Der Standardwert lautet `Info`.
- `piiLoggingEnabled` (optional): protokolliert personenbezogene und organisationsbezogene Daten, wenn der Wert auf „true“ festgelegt wird. Dieser Parameter ist standardmäßig auf „false“ festgelegt, damit Ihre Anwendung keine personenbezogenen Daten protokolliert. Protokolle mit personenbezogenen Daten werden nie in Standardausgaben wie die Konsole, Logcat oder NSLog geschrieben.
- `correlationId` (optional): Ein eindeutiger Bezeichner, der die Anforderung der Antwort zu Debugzwecken zuordnet. Standardmäßig wird ein in RFC 4122 definierter GUID der Version 4 (128 Bit) verwendet.

```javascript
function loggerCallback(logLevel, message, containsPii) {
   console.log(message);
}

var msalConfig = {
    auth: {
        clientId: "<Enter your client id>",
    },
     system: {
             logger: new Msal.Logger(
                                loggerCallback ,{
                                     level: Msal.LogLevel.Verbose,
                                     piiLoggingEnabled: false,
                                     correlationId: '1234'
                                }
                        )
     }
}

var UserAgentApplication = new Msal.UserAgentApplication(msalConfig);
```

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

Standardmäßig erfasst und protokolliert MSAL keine personenbezogenen Informationen (PII). Die Bibliothek erlaubt App-Entwicklern jedoch, diese Möglichkeit über eine Eigenschaft in der MSALLogger-Klasse zu aktivieren. Nach der Aktivierung von `pii.Enabled` übernimmt die App die Verantwortung für den sicheren Umgang mit hochgradig sensiblen Daten sowie für die Einhaltung gesetzlicher Anforderungen.

```objc
// By default, the `MSALLogger` doesn't capture any PII

// PII will be logged
MSALGlobalConfig.loggerConfig.piiEnabled = YES;

// PII will NOT be logged
MSALGlobalConfig.loggerConfig.piiEnabled = NO;
```

### <a name="logging-levels"></a>Protokolliergrade

Verwenden Sie einen der folgenden Werte, um den Protokolliergrad festzulegen, wenn Sie die Protokollierung mit MSAL für iOS und macOS ausführen:

|Ebene  |BESCHREIBUNG |
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

Standardmäßig erfasst und protokolliert MSAL keine personenbezogenen Informationen (PII). Die Bibliothek erlaubt App-Entwicklern jedoch, diese Möglichkeit über eine Eigenschaft in der MSALLogger-Klasse zu aktivieren. Nach der Aktivierung von `pii.Enabled` übernimmt die App die Verantwortung für den sicheren Umgang mit hochgradig sensiblen Daten sowie für die Einhaltung gesetzlicher Anforderungen.

```swift
// By default, the `MSALLogger` doesn't capture any PII

// PII will be logged
MSALGlobalConfig.loggerConfig.piiEnabled = true

// PII will NOT be logged
MSALGlobalConfig.loggerConfig.piiEnabled = false
```

### <a name="logging-levels"></a>Protokolliergrade

Verwenden Sie einen der folgenden Werte, um den Protokolliergrad festzulegen, wenn Sie die Protokollierung mit MSAL für iOS und macOS ausführen:

|Ebene  |BESCHREIBUNG |
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

## <a name="java"></a>[Java](#tab/java)

## <a name="msal-for-java-logging"></a>MSAL für Java-Protokollierung

Mit MSAL für Java können Sie die Protokollierungsbibliothek nutzen, die Sie bereits mit Ihrer App verwenden, sofern sie mit SLF4J kompatibel ist. MSAL für Java verwendet [Simple Logging Facade for Java](http://www.slf4j.org/) (SLF4J) als einfache Fassade oder Abstraktion für verschiedene Protokollierungsframeworks (beispielsweise [java.util.logging](https://docs.oracle.com/javase/7/docs/api/java/util/logging/package-summary.html), [Logback](http://logback.qos.ch/) und [Log4j](https://logging.apache.org/log4j/2.x/)). Mit SLF4J kann der Benutzer zur Bereitstellungszeit das gewünschte Protokollierungsframework einbinden.

Wenn Sie also beispielsweise Logback als Protokollierungsframework in Ihrer Anwendung verwenden möchten, fügen Sie der Maven-POM-Datei für Ihre Anwendung die Logback-Abhängigkeit hinzu:

```xml
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.2.3</version>
</dependency>
```

Fügen Sie anschließend die Logback-Konfigurationsdatei hinzu:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration debug="true">

</configuration>
```

SLF4J wird zur Bereitstellungszeit automatisch an Logback gebunden. MSAL-Protokolle werden in die Konsole geschrieben.

Eine Anleitung für die Bindung an andere Protokollierungsframeworks finden Sie im [SLF4J-Handbuch](http://www.slf4j.org/manual.html).

### <a name="personal-and-organization-information"></a>Personen- oder organisationsbezogene Informationen

Von der MSAL-Protokollierung werden standardmäßig keine personen- oder organisationsbezogenen Daten erfasst oder protokolliert. Im folgenden Beispiel ist die Protokollierung personen- oder organisationsbezogener Daten standardmäßig deaktiviert:

```java
    PublicClientApplication app2 = PublicClientApplication.builder(PUBLIC_CLIENT_ID)
            .authority(AUTHORITY)
            .build();
```

Aktivieren Sie die Protokollierung personen- oder organisationsbezogener Daten, indem Sie `logPii()` im Clientanwendungsersteller festlegen. Wenn Sie die Protokollierung personen- oder organisationsbezogener Daten aktivieren, muss Ihre App die Verantwortung für den sicheren Umgang mit hochgradig sensiblen Daten sowie für die Einhaltung gesetzlicher Anforderungen übernehmen.

Im folgenden Beispiel ist die Protokollierung personen- oder organisationsbezogener Daten aktiviert:

```java
PublicClientApplication app2 = PublicClientApplication.builder(PUBLIC_CLIENT_ID)
        .authority(AUTHORITY)
        .logPii(true)
        .build();
```

## <a name="python"></a>[Python](#tab/python)

## <a name="msal-for-python-logging"></a>MSAL für Python-Protokollierung

Bei der Protokollierung in MSAL Python wird der standardmäßige Python-Protokollierungsmechanismus verwendet (beispielsweise `logging.info("msg")`). Die MSAL-Protokollierung kann wie folgt konfiguriert werden. (Ein Praxisbeispiel finden Sie unter [username_password_sample](https://github.com/AzureAD/microsoft-authentication-library-for-python/blob/1.0.0/sample/username_password_sample.py#L31L32).)

### <a name="enable-debug-logging-for-all-modules"></a>Aktivieren der Debugprotokollierung für alle Module

Standardmäßig ist die Protokollierung in jedem Python-Skript deaktiviert. Wenn Sie die Debugprotokollierung für alle Module in Ihrem gesamten Python-Skript aktivieren möchten, verwenden Sie Folgendes:

```python
logging.basicConfig(level=logging.DEBUG)
```

### <a name="silence-only-msal-logging"></a>Nur Deaktivieren der MSAL-Protokollierung

Wenn Sie nur die MSAL-Protokollierung deaktivieren möchten, die Debugprotokollierung in allen anderen Modulen in Ihrem Python-Skript aber aktiviert sein soll, deaktivieren Sie die von MSAL Python verwendete Protokollierung:

```Python
logging.getLogger("msal").setLevel(logging.WARN)
```

### <a name="personal-and-organizational-data-in-python"></a>Personen- und organisationsbezogene Daten in Python

Von MSAL für Python werden keine personen- oder organisationsbezogenen Daten protokolliert. Es gibt keine Eigenschaft zum Aktivieren oder Deaktivieren der Protokollierung personen- oder organisationsbezogener Daten.

Sie können die Python-Standardprotokollierung verwenden, um die gewünschten Inhalte zu protokollieren. Sie sind jedoch für den sicheren Umgang mit sensiblen Daten sowie für die Einhaltung gesetzlicher Anforderungen verantwortlich.

Weitere Informationen zur Protokollierung in Python finden Sie in der [Protokollierungsanleitung](https://docs.python.org/3/howto/logging.html#logging-basic-tutorial) von Python.

---
