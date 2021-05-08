---
title: Protokollieren von Fehlern und Ausnahmen in MSAL.NET
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie Fehler und Ausnahmen in MSAL.NET protokollieren
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
ms.openlocfilehash: b4eb89c9c30915f72f45ebe9a26e063c8a44bce2
ms.sourcegitcommit: 62e800ec1306c45e2d8310c40da5873f7945c657
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108164415"
---
# <a name="logging-in-msalnet"></a>Protokollierung in MSAL.NET

[!INCLUDE [MSAL logging introduction](../../../includes/active-directory-develop-error-logging-introduction.md)]

## <a name="configure-logging-in-msalnet"></a>Konfigurieren der Protokollierung in MSAL.NET

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

> [!TIP]
 > Beispiele für die MSAL.NET-Protokollierung und weitere Infos finden Sie unter [MSAL.NET Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki).

## <a name="next-steps"></a>Nächste Schritte

Weitere Codebeispiele finden Sie unter [Microsoft Identity Platform-Codebeispiele](sample-v2-code.md).
