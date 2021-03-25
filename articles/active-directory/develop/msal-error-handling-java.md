---
title: Behandeln von Fehlern und Ausnahmen in MSAL4J
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie Fehler und Ausnahmen, Anspruchsaufforderungen für den bedingten Zugriff und Wiederholversuche in MSAL4J-Anwendungen verarbeiten.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/27/2020
ms.author: marsma
ms.reviewer: saeeda, nacanuma
ms.custom: aaddev
ms.openlocfilehash: 8563804a736c37acc9a96eb4a186933507f34200
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98760644"
---
# <a name="handle-errors-and-exceptions-in-msal-for-java"></a>Behandeln von Fehlern und Ausnahmen in MSAL für Java

[!INCLUDE [Active directory error handling introduction](../../../includes/active-directory-develop-error-handling-introduction.md)]

## <a name="error-handling-in-msal-for-java"></a>Fehlerbehandlung in MSAL für Java

In MSAL für Java gibt es drei Typen von Ausnahmen: `MsalClientException`, `MsalServiceException` und `MsalInteractionRequiredException`, die alle von `MsalException` erben.

- `MsalClientException` wird ausgelöst, wenn ein Fehler auftritt, der für die Bibliothek oder das Gerät lokal ist.
- `MsalServiceException` wird ausgelöst, wenn der Sicherheitstokendienst (Secure Token Service, STS) eine Fehlerantwort zurückgibt oder ein anderer Netzwerkfehler auftritt.
- `MsalInteractionRequiredException` wird ausgelöst, wenn eine Interaktion auf der Benutzeroberfläche erforderlich ist, damit die Authentifizierung erfolgreich ausgeführt werden kann.

### <a name="msalserviceexception"></a>MsalServiceException

`MsalServiceException` macht die in den Anforderungen an den STS zurückgegebene HTTP-Header verfügbar. Sie können über `MsalServiceException.headers()` darauf zugreifen.

### <a name="msalinteractionrequiredexception"></a>MsalInteractionRequiredException

Ein häufig von MSAL für Java zurückgegebener Statuscode beim Aufruf von `AcquireTokenSilently()` ist `InvalidGrantError`. Dies bedeutet, dass eine zusätzliche Benutzerinteraktion erforderlich ist, bevor ein Authentifizierungstoken ausgegeben werden kann. Die Anwendung sollte die Authentifizierungsbibliothek erneut anrufen, aber im interaktiven Modus, indem für öffentliche Clientanwendungen `AuthorizationCodeParameters` oder `DeviceCodeParameters` übermittelt wird.

Wenn bei `AcquireTokenSilently` ein Fehler auftritt, liegt dies in den meisten Fällen daran, dass der Tokencache kein Token für Ihre Anforderung hat. Zugriffstoken laufen nach einer Stunde ab, und `AcquireTokenSilently` versucht, basierend auf einem Aktualisierungstoken ein neues zu erhalten. Dies ist der Aktualisierungstokenflow in OAuth2. Bei diesem Flow kann auch aus verschiedenen Gründen ein Fehler auftreten, wenn z. B. ein Mandantenadministrator strengere Anmelderichtlinien konfiguriert.

Bestimmte Bedingungen, die diesen Fehler verursachen, können von den Benutzern einfach behoben werden. Möglicherweise müssen sie die Nutzungsbedingungen akzeptieren, oder die Anforderung kann nicht mit der aktuellen Konfiguration erfüllt werden, da der Computer eine Verbindung mit einem bestimmten Unternehmensnetzwerk herstellen muss.

MSAL macht ein Feld `reason` verfügbar, mit dem Sie die Handhabung für Benutzer verbessern können. Sie können dem Benutzer mit dem Feld `reason` z. B. mitteilen, dass sein Kennwort abgelaufen ist oder dass er der Verwendung einiger Ressourcen zustimmen muss. Die unterstützten Werte sind Teil der `InteractionRequiredExceptionReason`-Enumeration:

| `Reason` | Bedeutung | Empfohlene Behandlung |
|---------|-----------|-----------------------------|
| `BasicAction` | Die Bedingung kann während des interaktiven Authentifizierungsflows durch Benutzerinteraktion aufgelöst werden. | Aufrufen von `acquireToken` mit interaktiven Parametern. |
| `AdditionalAction` | Die Bedingung kann durch eine zusätzliche Korrekturmaßnahme mit dem System außerhalb des interaktiven Authentifizierungsflows aufgelöst werden. | Aufrufen von `acquireToken` mit interaktiven Parametern, um eine Nachricht anzuzeigen, die die Korrekturmaßnahme erklärt. Beim Aufrufen der App können Flows ausgeblendet werden, für die eine zusätzliche Aktion erforderlich ist, wenn der Benutzer die Korrekturmaßnahme wahrscheinlich nicht ausführen kann. |
| `MessageOnly` | Die Bedingung kann zu diesem Zeitpunkt nicht aufgelöst werden. Beim Starten des interaktiven Authentifizierungsflows wird eine Meldung angezeigt, in der die Bedingung erläutert wird. | Aufrufen von `acquireToken` mit interaktiven Parametern, um eine Nachricht anzuzeigen, die die Bedingung erklärt. `acquireToken` gibt den Fehler `UserCanceled` zurück, nachdem der Benutzer die Meldung gelesen und das Fenster geschlossen hat. Beim Aufrufen der App können Flows ausgeblendet werden, wenn sie zu einer Meldung führen, die für den Benutzer wahrscheinlich keinen Nutzen hat. |
| `ConsentRequired`| Die Benutzerzustimmung ist nicht vorhanden oder wurde widerrufen. |Aufrufen von `acquireToken` mit interaktiven Parametern, damit der Benutzer zustimmen kann. |
| `UserPasswordExpired` | Das Kennwort des Benutzers ist abgelaufen. | Aufrufen von `acquireToken` mit interaktivem Parameter, damit der Benutzer sein Kennwort zurücksetzen kann. |
| `None` |  Es werden weitere Informationen bereitgestellt. Die Bedingung kann während des interaktiven Authentifizierungsflows durch Benutzerinteraktion aufgelöst werden. | Aufrufen von `acquireToken` mit interaktiven Parametern. |

### <a name="code-example"></a>Codebeispiel

```java
        IAuthenticationResult result;
        try {
            PublicClientApplication application = PublicClientApplication
                    .builder("clientId")
                    .b2cAuthority("authority")
                    .build();

            SilentParameters parameters = SilentParameters
                    .builder(Collections.singleton("scope"))
                    .build();

            result = application.acquireTokenSilently(parameters).join();
        }
        catch (Exception ex){
            if(ex instanceof MsalInteractionRequiredException){
                // AcquireToken by either AuthorizationCodeParameters or DeviceCodeParameters
            } else{
                // Log and handle exception accordingly
            }
        }
```

[!INCLUDE [Active directory error handling claims challenges](../../../includes/active-directory-develop-error-handling-claims-challenges.md)]

[!INCLUDE [Active directory error handling retries](../../../includes/active-directory-develop-error-handling-retries.md)]

## <a name="next-steps"></a>Nächste Schritte

Erwägen Sie die Aktivierung der [Protokollierung in MSAL für Java](msal-logging-java.md), um Probleme besser diagnostizieren und debuggen zu können.
