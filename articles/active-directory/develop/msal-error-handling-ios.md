---
title: Behandeln von Fehlern und Ausnahmen in MSAL für iOS/macOS
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie Fehler und Ausnahmen, Anspruchsaufforderungen für den bedingten Zugriff und Wiederholversuche in MSAL für iOS/macOS-Anwendungen verarbeiten.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/26/2020
ms.author: marsma
ms.reviewer: saeeda, oldalton
ms.custom: aaddev
ms.openlocfilehash: f7f2abadb7586e1b19168eb46a54bad53caa05cc
ms.sourcegitcommit: 3c3ec8cd21f2b0671bcd2230fc22e4b4adb11ce7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2021
ms.locfileid: "98761096"
---
# <a name="handle-errors-and-exceptions-in-msal-for-iosmacos"></a>Behandeln von Fehlern und Ausnahmen in MSAL für iOS/macOS

[!INCLUDE [Active directory error handling introduction](../../../includes/active-directory-develop-error-handling-introduction.md)]

## <a name="error-handling-in-msal-for-iosmacos"></a>Fehlerbehandlung in MSAL für iOS/macOS

Die vollständige Liste der Fehler in MSAL für iOS und macOS finden Sie unter [MSALError-Enumeration](https://github.com/AzureAD/microsoft-authentication-library-for-objc/blob/master/MSAL/src/public/MSALError.h#L128).

Alle von MSAL erzeugten Fehler werden mit der `MSALErrorDomain`-Domain zurückgegeben.

Bei Systemfehlern gibt MSAL den ursprünglichen `NSError` von der System-API zurück. Wenn der Tokenabruf beispielsweise aufgrund fehlender Netzwerkkonnektivität fehlschlägt, gibt MSAL einen Fehler mit der `NSURLErrorDomain`-Domain und `NSURLErrorNotConnectedToInternet`-Code zurück.

Es wird empfohlen, mindestens die folgenden beiden MSAL-Fehler auf der Clientseite zu behandeln:

- `MSALErrorInteractionRequired`: Der Benutzer muss eine interaktive Anforderung ausführen. Es gibt viele Bedingungen, die zu diesem Fehler führen können, z.B. eine abgelaufene Authentifizierungssitzung oder die Notwendigkeit zusätzlicher Authentifizierungsanforderungen. Rufen Sie die interaktive MSAL-Tokenabruf-API auf, um die Wiederherstellung durchzuführen. 

- `MSALErrorServerDeclinedScopes`: Einige oder alle Bereiche wurden abgelehnt. Entscheiden Sie, ob der Vorgang mit nur den gewährten Bereichen fortgesetzt werden soll, oder beenden Sie den Anmeldevorgang.

> [!NOTE]
> Die `MSALInternalError`-Enumeration sollte nur zu Referenzzwecken und zum Debuggen verwendet werden. Versuchen Sie nicht, diese Fehler zur Laufzeit automatisch zu behandeln. Wenn Ihre App auf einen der Fehler stößt, die unter `MSALInternalError` fallen, sollten Sie eine generische Meldung für den Benutzer anzeigen, die beschreibt, was passiert ist.

`MSALInternalErrorBrokerResponseNotReceived` bedeutet beispielsweise, dass der Benutzer die Authentifizierung nicht fertiggestellt hat und manuell zur App zurückgekehrt ist. In diesem Fall sollte Ihre App eine generische Fehlermeldung anzeigen, die erläutert, dass die Authentifizierung nicht abgeschlossen wurde, und dem Benutzer empfehlen, sich erneut zu authentifizieren.

Der folgende Objective-C-Beispielcode zeigt bewährte Methoden für die Behandlung einiger allgemeiner Fehlerbedingungen.

```objc
    MSALInteractiveTokenParameters *interactiveParameters = ...;
    MSALSilentTokenParameters *silentParameters = ...;
    
    MSALCompletionBlock completionBlock;
    __block __weak MSALCompletionBlock weakCompletionBlock;
    
    weakCompletionBlock = completionBlock = ^(MSALResult *result, NSError *error)
    {
        if (!error)
        {
            // Use result.accessToken
            NSString *accessToken = result.accessToken;
            return;
        }
        
        if ([error.domain isEqualToString:MSALErrorDomain])
        {
            switch (error.code)
            {
                case MSALErrorInteractionRequired:
                {
                    // Interactive auth will be required
                    [application acquireTokenWithParameters:interactiveParameters
                                            completionBlock:weakCompletionBlock];
                    
                    break;
                }
                    
                case MSALErrorServerDeclinedScopes:
                {
                    // These are list of granted and declined scopes.
                    NSArray *grantedScopes = error.userInfo[MSALGrantedScopesKey];
                    NSArray *declinedScopes = error.userInfo[MSALDeclinedScopesKey];
                    
                    // To continue acquiring token for granted scopes only, do the following
                    silentParameters.scopes = grantedScopes;
                    [application acquireTokenSilentWithParameters:silentParameters
                                                  completionBlock:weakCompletionBlock];
                    
                    // Otherwise, instead, handle error fittingly to the application context
                    break;
                }
                    
                case MSALErrorServerProtectionPoliciesRequired:
                {
                    // Integrate the Intune SDK and call the
                    // remediateComplianceForIdentity:silent: API.
                    // Handle this error only if you integrated Intune SDK.
                    // See more info here: https://aka.ms/intuneMAMSDK
                    
                    break;
                }
                    
                case MSALErrorUserCanceled:
                {
                    // The user cancelled the web auth session.
                    // You may want to ask the user to try again.
                    // Handling of this error is optional.
                    
                    break;
                }
                    
                case MSALErrorInternal:
                {
                    // Log the error, then inspect the MSALInternalErrorCodeKey
                    // in the userInfo dictionary.
                    // Display generic error message to the end user
                    // More detailed information about the specific error
                    // under MSALInternalErrorCodeKey can be found in MSALInternalError enum.
                    NSLog(@"Failed with error %@", error);
                    
                    break;
                }
                    
                default:
                    NSLog(@"Failed with unknown MSAL error %@", error);
                    
                    break;
            }
            
            return;
        }
        
        // Handle no internet connection.
        if ([error.domain isEqualToString:NSURLErrorDomain] && error.code == NSURLErrorNotConnectedToInternet)
        {
            NSLog(@"No internet connection.");
            return;
        }
        
        // Other errors may require trying again later,
        // or reporting authentication problems to the user.
        NSLog(@"Failed with error %@", error);
    };
    
    // Acquire token silently
    [application acquireTokenSilentWithParameters:silentParameters
                                  completionBlock:completionBlock];

     // or acquire it interactively.
     [application acquireTokenWithParameters:interactiveParameters
                             completionBlock:completionBlock];
```

```swift
    let interactiveParameters: MSALInteractiveTokenParameters = ...
    let silentParameters: MSALSilentTokenParameters = ...
            
    var completionBlock: MSALCompletionBlock!
    completionBlock = { (result: MSALResult?, error: Error?) in
                
        if let result = result
        {
            // Use result.accessToken
            let accessToken = result.accessToken
            return
        }

        guard let error = error as NSError? else { return }

        if error.domain == MSALErrorDomain, let errorCode = MSALError(rawValue: error.code)
        {
            switch errorCode
            {
                case .interactionRequired:
                    // Interactive auth will be required
                    application.acquireToken(with: interactiveParameters, completionBlock: completionBlock)

                case .serverDeclinedScopes:
                    let grantedScopes = error.userInfo[MSALGrantedScopesKey]
                    let declinedScopes = error.userInfo[MSALDeclinedScopesKey]

                    if let scopes = grantedScopes as? [String] {
                        silentParameters.scopes = scopes
                        application.acquireTokenSilent(with: silentParameters, completionBlock: completionBlock)
                    }
                        
                    case .serverProtectionPoliciesRequired:
                        // Integrate the Intune SDK and call the
                        // remediateComplianceForIdentity:silent: API.
                        // Handle this error only if you integrated Intune SDK.
                        // See more info here: https://aka.ms/intuneMAMSDK
                        break
                        
                    case .userCanceled:
                       // The user cancelled the web auth session.
                       // You may want to ask the user to try again.
                       // Handling of this error is optional.
                       break
                        
                    case .internal:
                        // Log the error, then inspect the MSALInternalErrorCodeKey
                        // in the userInfo dictionary.
                        // Display generic error message to the end user
                        // More detailed information about the specific error
                        // under MSALInternalErrorCodeKey can be found in MSALInternalError enum.
                        print("Failed with error \(error)");
                        
                    default:
                        print("Failed with unknown MSAL error \(error)")
            }
        }
                
        // Handle no internet connection.
        if error.domain == NSURLErrorDomain && error.code == NSURLErrorNotConnectedToInternet
        {
            print("No internet connection.")
            return
        }
                
        // Other errors may require trying again later,
        // or reporting authentication problems to the user.
        print("Failed with error \(error)");    
    }
   
    // Acquire token silently
    application.acquireToken(with: interactiveParameters, completionBlock: completionBlock)
 
    // or acquire it interactively.
    application.acquireTokenSilent(with: silentParameters, completionBlock: completionBlock)
```

---

[!INCLUDE [Active directory error handling claims challenges](../../../includes/active-directory-develop-error-handling-claims-challenges.md)]

MSAL für iOS und macOS ermöglicht es Ihnen, bestimmte Ansprüche sowohl in interaktiven als auch in automatischen Tokenabrufszenarien anzufordern.

Geben Sie `claimsRequest` in `MSALSilentTokenParameters` oder `MSALInteractiveTokenParameters` an, um benutzerdefinierte Ansprüche anzufordern.

Weitere Informationen finden Sie unter [Anfordern benutzerdefinierter Ansprüche mit MSAL für iOS und macOS](request-custom-claims.md).

[!INCLUDE [Active directory error handling retries](../../../includes/active-directory-develop-error-handling-retries.md)]

## <a name="next-steps"></a>Nächste Schritte

Erwägen Sie die Aktivierung der [Protokollierung in MSAL für iOS/macOS](msal-logging-ios.md), um Probleme besser diagnostizieren und debuggen zu können.
