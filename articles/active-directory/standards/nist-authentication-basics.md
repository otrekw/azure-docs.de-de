---
title: Grundlagen der NIST-Authentifizierung in Azure Active Directory
description: Dieser Artikel definiert wichtige Begriffe und beschreibt Trusted Platform Module und Authentifizierungsfaktoren für NIST.
services: active-directory
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: how-to
author: barbaraselden
ms.author: baselden
manager: mtillman
ms.reviewer: martinco
ms.date: 4/26/2021
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: eacdb19dcd5121f1d7fecdf6a3cefbafd609c9a9
ms.sourcegitcommit: 34feb2a5bdba1351d9fc375c46e62aa40bbd5a1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111889830"
---
# <a name="nist-authentication-basics"></a>Grundlagen der NIST-Authentifizierung 

Um die Richtlinien des National Institute of Standards and Technology (NIST) zu verstehen, müssen Sie die Terminologie kennen. Darüber hinaus müssen Sie sich mit dem Trusted Platform Module (TPM) und Authentifizierungsfaktoren auskennen. Dieser Artikel bietet Information zu beiden Themen. 

## <a name="terminology"></a>Begriff

Die folgende Terminologie wird in diesen NIST-bezogenen Artikeln verwendet.

|Begriff| Definition. In dieser Tabelle werden *kursiv gesetzte* Begriffe definiert.|
| - | - |
| Assertion| Eine Anweisung von einem *Verifier* an eine *vertrauende Seite*, die Informationen über den *Abonnenten* enthält. Eine Assertion kann überprüfte Attribute enthalten. |
|Authentifizierung| Der Prozess der Überprüfung der Identität eines *Subjekts*. |
| Authentifizierungsfaktor| Etwas, das Sie haben, etwas, das Sie wissen oder etwas, das Sie sind. Jeder *Authentifikator* verfügt über mindestens einen Authentifizierungsfaktor. |
| Authentifikator| Etwas, das der *Anforderer* besitzt und steuert und das zum Authentifizieren der Identität des *Anforderers* verwendet wird. |
| Anforderer| Eine *Person*, deren Identität mit einem oder mehreren *Authentifizierungsprotokollen* überprüft werden soll. |
|Anmeldeinformationen| Ein Objekt oder eine Datenstruktur, die eine Identität autoritativ an mindestens einen *Authentifikator* bindet, der von einem *Abonnenten* besitzt und gesteuert wird. |
| Credential Service Provider (CSP)| Eine vertrauenswürdige Entität, die *Abonnentenauthenifikatoren* ausgibt oder registriert und elektronische *Anmeldeinformationen* für *Abonnenten* ausgibt. |
|Vertrauende Seite| Eine Entität, die die *Assertion eines Verifiers* oder *Authentifikatoren eines Anforderers* und *Anmeldeinformationen* verwendet, in der Regel um Zugriff auf ein System zu gewähren. |
|  Subject| Eine Person, Organisation, Gerät, Hardware, Netzwerk, Software oder Dienst. |
| Subscriber| Eine Partei, die *Anmeldeinformationen* oder einen  *Authentikator* von einem *CSP* erhalten hat. |
|Trusted Platform Module  | Ein TPM ist ein manipulationssicheres Modul, das kryptografische Vorgänge einschließlich der Schlüsselgenerierung ausführt. |
|  Verifier| Eine Entität, welche die Identität des *Anforderers* durch Prüfen des Besitzes und der Kontrolle von *Authentikatoren* prüft. |


## <a name="about-trusted-platform-module-technology"></a>Informationen zur Trusted Platform Module-Technologie

Die TPM-Technologie stellt hardwarebasierte, sicherheitsbezogene Funktionen bereit. Ein TPM-Chip oder Hardware-TPM ist ein sicherer Kryptografieprozessor, welcher Sie bei Aktionen wie dem Generieren, Speichern und Einschränken der Verwendung kryptografischer Schlüssel unterstützt. 

Microsoft stellt wichtige Informationen zur Funktionsweise von TPMs mit Windows zur Verfügung. Weitere Informationen finden Sie unter [Trusted Platform Module](/windows/security/information-protection/tpm/trusted-platform-module-top-node). 

Ein Software-TPM ist ein Emulator, der die Funktionalität von Hardware-TPM imitiert. 

 ## <a name="authentication-factors-and-their-strengths"></a>Authentifizierungsfaktoren und ihre Stärken

Authentifizierungsfaktoren können in drei Kategorien unterteilt werden:

![Grafik mit Beispielen für Authentifizierungsfaktoren, unterteilt nach etwas, das Sie wissen, etwas, das Sie haben, und etwas, das Sie sind.](media/nist-authentication-basics/nist-authentication-basics-0.png)

Die Stärke eines Authentifizierungsfaktors wird davon bestimmt, wie sicher Sie sein können, dass es sich um etwas handelt, das nur der Abonnent kennt, besitzt oder ist.

NIST bietet knappe Hinweise zur relativen Stärke von Authentifizierungsfaktoren. Im restlichen Teil dieses Abschnitts wird beschrieben, wie wir diese Stärken bei Microsoft bewerten. 

**Etwas, das Sie wissen** Kennwörter sind das gängigste *etwas, das Sie wissen* und stellen die größte Angriffsfläche dar. Folgende Abhilfemaßnahmen verbessern das Vertrauen in die Affinität zum Abonnenten. Sie sind effektive Gegenmaßnahmen gegen Kennwortangriffe wie Brute-Force-Angriffe, Lauschangriffe und Social Engineering:

* [Anforderungen an die Komplexität von Kennwörtern](https://www.microsoft.com/research/wp-content/uploads/2016/06/Microsoft_Password_Guidance-1.pdf)

* [Gesperrte Kennwörter](../authentication/tutorial-configure-custom-password-protection.md)

* [Kompromittierte Identifikation mit Anmeldeinformationen](../identity-protection/overview-identity-protection.md)

* [Sicherer Hashspeicher](https://aka.ms/AADDataWhitepaper)

* [Kontosperrung](../authentication/howto-password-smart-lockout.md)

**Etwas, das Sie haben** Die Stärke von *etwas, das Sie haben* basiert darauf, wie wahrscheinlich es ist, dass der Abonnent es im Besitz behält, und wie schwierig es für Angreifer ist, Zugriff darauf zu erlangen. Wenn Sie beispielsweise versuchen, sich vor internen Bedrohungen zu schützen, hat ein persönliches mobiles Gerät oder ein Hardwareschlüssel eine höhere Affinität. Es ist also sicherer als ein Desktopcomputer in einem Büro.

**Etwas, das Sie sind** Die Einfachheit, mit der ein Angreifer eine Kopie von etwas erhalten kann, *das Sie sind*, oder biometrische Daten vortäuschen kann, ist wichtig. NIST entwirft ein Framework für biometrische Daten. Derzeit akzeptiert NIST keine biometrischen Daten als separate Authentifizierungsmethode. Dies muss ein Faktor bei der mehrstufigen Authentifizierung sein. Diese Vorsichtsmaßnahme besteht, da biometrische Daten probabilistisch sind. Das heißt, sie verwenden Algorithmen, welche die Wahrscheinlichkeit der Affinität bestimmen. Biometrie ist nicht notwendigerweise eine genaue Übereinstimmung, wie dies bei Kennwörtern der Fall ist. Weitere Informationen finden Sie unter [Stärke der Funktion für Authentifikatoren – Biometrie](https://pages.nist.gov/SOFA/SOFA.html) (SOFA-B). 

Mit SOFA-B wird versucht, ein Framework zur Quantifizierung der Stärke der Biometrie für folgende Kriterien zu erstellen:
- Rate falscher Übereinstimmungen
- Rate falscher Fehlschläge
- Fehlerrate bei der Erkennung von Präsentationsangriffen
- Aufwand für die Durchführung eines Angriffs 

## <a name="single-factor-authentication"></a>Einstufige Authentifizierung

Die einstufige Authentifizierung kann mithilfe eines einstufigen Authentifikators implementiert werden, der etwas überprüft, *das Sie kennen* oder *das Sie sind*. Ein *etwas, das Sie sind*-Faktor wird als Authentifizierungsfaktor akzeptiert, aber nicht als Authentifikator selbst. 

![Grafik, die die Funktionsweise der einstufigen Authentifizierung veranschaulicht](media/nist-authentication-basics/nist-authentication-basics-1.png)

## <a name="multifactor-authentication"></a>Mehrstufige Authentifizierung

Sie können eine mehrstufige Authentifizierung implementieren, indem Sie entweder einen mehrstufigen Authentifikator oder zwei einstufige Authentifikatoren verwenden. Ein mehrstufiger Authentifikator erfordert zwei Authentifizierungsfaktoren, um eine einzelne Authentifizierungstransaktion abzuschließen.

### <a name="multifactor-authentication-by-using-two-single-factor-authenticators"></a>Mehrstufige Authentifizierung mit zwei einstufigen Authentifikatoren

Die mehrstufige Authentifizierung erfordert zwei verschiedene Authentifizierungsfaktoren. Diese Authentifikatoren können unabhängig sein. Beispiel: 

* Auswendig gelerntes Geheimnis (Kennwort) und Out-of-Band (SMS)

* Auswendig gelerntes Geheimnis (Kennwort) und einmaliges Kennwort (Hardware oder Software)

Diese Methoden führen zwei unabhängige Authentifizierungstransaktionen mit Azure Active Directory (Azure AD) aus.

![Grafik, die die mehrstufige Authentifizierung über zwei separate Authentifikatoren beschreibt](media/nist-authentication-basics/nist-authentication-basics-2.png)


### <a name="multifactor-authentication-by-using-a-single-multifactor-authenticator"></a>Mehrstufige Authentifizierung mit einem einzigen mehrstufigen Authentifikator

Die mehrstufige Authentifizierung erfordert einen Authentifizierungsfaktor (*etwas, das Sie wissen* oder *etwas, das Sie sind*), um einen zweiten Authentifizierungsfaktor zu entsperren. Die Benutzererfahrung ist in der Regel einfacher als die bei mehreren unabhängigen Authentifikatoren.

![Grafik, die die mehrstufige Authentifizierung mit einem einzigen mehrstufigen Authentifikator zeigt](media/nist-authentication-basics/nist-authentication-basics-3a.png)

Ein Beispiel ist die im kennwortlosen Modus verwendete Microsoft Authenticator-App. Mit dieser Methode versucht der Benutzer, auf eine gesicherte Ressource (vertrauende Seite) zu zugreifen, und erhält eine Benachrichtigung über die Authentifikator-App. Der Benutzer reagiert auf die Benachrichtigung, indem er entweder ein biometrisches Merkmal (*etwas,das Sie sind*) oder eine PIN (*etwas, das Sie wissen*) präsentiert. Durch diesen Faktor wird der kryptografische Schlüssel auf dem Telefon entsperrt (*etwas, das Sie haben*), der dann vom Verifier überprüft wird.

## <a name="next-steps"></a>Nächste Schritte 

[NIST-Übersicht](nist-overview.md)

[Weitere Informationen zu AALs](nist-about-authenticator-assurance-levels.md)

[Authentifizierungsszenarien für Azure AD](nist-authentication-basics.md)

[NIST-Authentifikatortypen](nist-authenticator-types.md)

[Erreichen von NIST AAL1 mit Azure AD](nist-authenticator-assurance-level-1.md)

[Erreichen von NIST AAL2 mit Azure AD](nist-authenticator-assurance-level-2.md)

[Erreichen von NIST AAL3 mit Azure AD](nist-authenticator-assurance-level-3.md)