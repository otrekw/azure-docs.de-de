---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 01/18/2021
ms.author: mimart
ms.openlocfilehash: 4f6ba0892438d49dcc982a01a6b30dfa36ed43b5
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "98674229"
---
## <a name="add-a-user-journey"></a>Hinzufügen einer User Journey 

Der Identitätsanbieter wurde nun eingerichtet, aber er ist noch auf keiner der Anmeldeseiten verfügbar. Wenn Sie nicht über eine eigene benutzerdefinierte User Journey verfügen, erstellen Sie ein Duplikat einer vorhandenen User Journey-Vorlage, und fahren Sie andernfalls mit dem nächsten Schritt fort. 

1. Öffnen Sie die Datei *TrustFrameworkBase.xml* aus dem Starter Pack.
1. Suchen und kopieren Sie den gesamten Inhalt des **UserJourney**-Elements, das `Id="SignUpOrSignIn"` enthält.
1. Öffnen Sie die Datei *TrustFrameworkExtensions.xml*, und suchen Sie nach dem **UserJourneys**-Element. Wenn das Element nicht vorhanden ist, fügen Sie ein solches hinzu.
1. Fügen Sie den gesamten Inhalt des kopierten **UserJourney**-Element als untergeordnetes Element des **UserJourneys**-Elements ein.
1. Benennen Sie die ID der User Journey um. Beispiel: `Id="CustomSignUpSignIn"`.

## <a name="add-the-identity-provider-to-a-user-journey"></a>Hinzufügen des Identitätsanbieters zu einer User Journey 

Nachdem Sie nun über eine User Journey verfügen, fügen Sie den neuen Identitätsanbieter der User Journey hinzu. Fügen Sie zunächst eine Anmeldeschaltfläche hinzu, und verknüpfen Sie dann die Schaltfläche mit einer Aktion. Die Aktion ist das technische Profil, das Sie zuvor erstellt haben.

1. Suchen Sie nach dem Orchestrierungsschrittelement, das `Type="CombinedSignInAndSignUp"` enthält, oder `Type="ClaimsProviderSelection"` in der User Journey. Dies ist in der Regel der erste Orchestrierungsschritt. Das **ClaimsProviderSelections**-Element enthält eine Liste mit Identitätsanbietern, mit denen sich ein Benutzer anmelden kann. Die Reihenfolge der Elemente gibt die Reihenfolge der Anmeldeschaltflächen vor, die dem Benutzer angezeigt werden. Fügen Sie ein **ClaimsProviderSelection**-XML-Element hinzu. Legen Sie für **TargetClaimsExchangeId** einen Anzeigenamen fest.

1. Fügen Sie im nächsten Orchestrierungsschritt ein **ClaimsExchange**-Element hinzu. Legen Sie die **ID** auf den Wert der Zielanspruchs-Austausch-ID fest. Ändern Sie den Wert von **TechnicalProfileReferenceId** in die ID des technischen Profils, das Sie zuvor erstellt haben.

Der folgende XML-Code veranschaulicht die ersten beiden Orchestrierungsschritte einer User Journey mit dem Identitätsanbieter: