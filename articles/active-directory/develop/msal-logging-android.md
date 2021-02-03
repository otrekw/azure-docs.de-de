---
title: Protokollieren von Fehlern und Ausnahmen in MSAL für Android
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie Fehler und Ausnahmen in MSAL für Android protokollieren.
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
ms.openlocfilehash: ce0929adbb2b0213cfd4ee61fe795a2d5f33c648
ms.sourcegitcommit: 4e70fd4028ff44a676f698229cb6a3d555439014
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2021
ms.locfileid: "98954883"
---
# <a name="logging-in-msal-for-android"></a>Protokollierung in MSAL für Android

[!INCLUDE [MSAL logging introduction](../../../includes/active-directory-develop-error-logging-introduction.md)]

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

## <a name="next-steps"></a>Nächste Schritte

Weitere Codebeispiele finden Sie unter [Microsoft Identity Platform-Codebeispiele](sample-v2-code.md).