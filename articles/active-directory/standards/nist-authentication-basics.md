---
title: Grundlagen der NIST-Authentifizierung in Azure Active Directory
description: Erläuterungen zur Terminologie und zu den Authentifizierungsfaktoren für NIST.
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
ms.openlocfilehash: 028ce5fc6ae3cef586803e1b04948006b58fd17e
ms.sourcegitcommit: fc9fd6e72297de6e87c9cf0d58edd632a8fb2552
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108294092"
---
# <a name="nist-authentication-basics"></a>Grundlagen der NIST-Authentifizierung 

Um die NIST-Richtlinien zu verstehen, müssen Sie über solide Grundlagenkenntnisse in der Terminologie und den Konzepten von vertrauenswürdigen Plattformmodulen (Trusted Platform Modules, TPMs) und Authentifizierungsfaktoren verfügen.

## <a name="terminology"></a>Begriff

Die folgende Terminologie wird in diesen NIST-bezogenen Artikeln verwendet.

|Begriff| Definition: In dieser Tabelle werden *kursiv gesetzte* Begriffe definiert|
| - | - |
| Assertion| Eine Anweisung von einem *Verifier* an eine *vertrauende Partei*, die Informationen über den *Abonnenten* enthält. Kann überprüfte Attribute enthalten. |
|Authentifizierung| Der Prozess der Überprüfung der Identität eines *Subjekts*. |
| Authentifizierungsfaktor| Etwas, das Sie kennen, etwas, das Sie haben, oder etwas, das Sie sind: Jeder *Authentifikator* hat mindestens einen Authentifizierungsfaktor. |
| Authentifikator| Etwas, das der *Anforderer* besitzt und steuert und das zum Authentifizieren der Identität *des Anforderers* verwendet wird. |
| Anforderer| Ein *Subjekt*, dessen Identität mit einem oder mehreren Authentifizierungsprotokollen überprüft werden soll. |
|Anmeldeinformationen| Ein Objekt oder eine Datenstruktur, die eine Identität autoritativ an mindestens einen *Authentifikator* bindet, der von einem *Abonnenten* besitzt und gesteuert wird. |
| Credential Service Provider (CSP)| Eine vertrauenswürdige Entität, die *Abonnentenauthenifikatoren* ausgibt oder registriert und elektronische *Anmeldeinformationen* für *Abonnenten* ausgibt. |
|Vertrauende Seite| Eine Entität, die auf der *Assertion eines Verifiers* oder *Authentikatoren* und *Anmeldeinformationen* eines Anforderers basiert, um in der Regel Zugriff auf ein System zu gewähren. |
|  Subject| Eine Person, Organisation, Gerät, Hardware, Netzwerk, Software oder Dienst. |
| Subscriber| Eine Partei, die *Anmeldeinformationen* oder einen  *Authentikator* von einem *CSP* erhalten hat. |
|Trusted Platform Module (TPM)  | Ein TPM ist ein manipulationssicheres Modul, das kryptografische Vorgänge einschließlich der Schlüsselgenerierung ausführt. |
|  Verifier| Eine Entität, welche die Identität des *Anforderers* durch Prüfen des Besitzes und der Kontrolle von *Authentikatoren* prüft. |


## <a name="about-trusted-platform-modules"></a>Über TPM (Trusted Platform Module) 

Die TPM-Technologie stellt hardwarebasierte, sicherheitsbezogene Funktionen bereit. Ein TPM-Chip oder Hardware-TPM ist ein sicherer Kryptografieprozessor, welcher Sie bei Aktionen wie dem Generieren, Speichern und Einschränken der Verwendung kryptografischer Schlüssel unterstützt. 

Microsoft stellt wichtige Informationen zur Funktionsweise von TPMs mit Microsoft Windows zur Verfügung. Weitere Informationen finden Sie in diesem Artikel zum [Trusted Platform Module](https://docs.microsoft.com/windows/security/information-protection/tpm/trusted-platform-module-top-node). 

Ein Software-TPM ist ein Emulator, der diese Funktionalität imitiert. 

 ## <a name="authentication-factors-and-their-strengths"></a>Authentifizierungsfaktoren und ihre Stärken

Authentifizierungsfaktoren können in drei Kategorien unterteilt werden. Die folgende Tabelle enthält ein Beispiel für die Typen von Faktoren unter den einzelnen Gruppierungen.

![MFA kann etwas beinhalten, was Sie wissen, was Sie besitzen und was sie sind.](media/nist-authentication-basics/nist-authentication-basics-0.png)

Die Stärke eines Authentifizierungsfaktors wird davon bestimmt, wie sicher wir sein können, dass es sich um etwas handelt, das nur der Abonnent kennt, besitzt oder ist.

In NIST gibt es eingeschränkte Hinweise zur relativen Stärke von Authentifizierungsfaktoren. Hier bei Microsoft bewerten wir die Stärken wie unten dargestellt. 

**Etwas, das Sie kennen**: Kennwörter, das gängigste, was Sie wissen, stellen die größte Angriffsfläche dar. Die folgenden Gegenmaßnahmen verbessern das Vertrauen in die Affinität zum Abonnenten und sind effektiv, um Kennwortangriffe wie Brute-Force-Angriffe, Lauschangriffe und Social Engineering zu verhindern:

* [Anforderungen an die Komplexität von Kennwörtern](https://www.microsoft.com/research/wp-content/uploads/2016/06/Microsoft_Password_Guidance-1.pdf)

* [Gesperrte Kennwörter](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-configure-custom-password-protection)

* [Kompromittierte Identifikation mit Anmeldeinformationen](https://docs.microsoft.com/azure/active-directory/identity-protection/overview-identity-protection)

* [Sicherer Hashspeicher](https://aka.ms/AADDataWhitepaper)

* [Kontosperrung](https://docs.microsoft.com/azure/active-directory/authentication/howto-password-smart-lockout)

**Etwas, das Sie haben**: Die Stärke von etwas, das Sie haben, basiert darauf, wie wahrscheinlich der Abonnent ist, es im Besitz zu behalten, und wie schwierig es ist, dass ein Angreifer Zugriff darauf erlangen kann. Beispielsweise hat ein persönliches mobiles Gerät oder ein Hardwareschlüssel eine höhere Affinität und ist daher sicherer als ein Desktopcomputer in einem Büro, wenn versucht wird, sich vor internen Bedrohungen zu schützen.

**Etwas, das Sie** sind: Die Einfachheit, mit der ein Angreifer eine Kopie von etwas erhalten kann, das Sie sind, oder eine biometrische Datensyntmetrie spoofen kann, ist wichtig. NIST entwirft ein Framework für biometrische Daten. Heute akzeptiert NIST keine biometrischen Daten als separate Authentifizierungsmethode. Dies muss ein Faktor bei der mehrstufigen Authentifizierung sein. Dies liegt daran, dass biometrische Daten probabilistisch sind. Das heißt, sie verwenden Algorithmen, welche die Wahrscheinlichkeit bestimmen, dass es sich um dieselbe Person handelt. Es handelt sich nicht unbedingt um eine genaue Übereinstimmung, wie dies bei einem Kennwort der Fall ist. Weitere Informationen finden Sie in diesem Dokument zur [Stärke der Funktion für Authentifikatoren – Biometrie](https://pages.nist.gov/SOFA/SOFA.html) (SOFA-B). SOFA-B versucht, ein Framework zu präsentieren, um die Stärke der biometrischen Daten in Bezug auf falsche Übereinstimmungsrate, Falsch, Fehlerrate, Fehlerrate bei der Erkennung von Präsentationsangriffen und erforderliche Maßnahmen zum Starten eines Angriffs zu bewerten. 

## <a name="single-factor-authentication"></a>Einstufige Authentifizierung

Die einstufige Authentifizierung kann mithilfe eines einstufigen Authentifikators erreicht werden, der etwas darstellt, das Sie kennen oder das Sie sind. Obwohl ein Authentifizierungsfaktor, der „etwas ist, das Sie sind“, als Authentifizierungsfaktor akzeptiert wird, wird er an sich nicht als Authentifikator akzeptiert. 

![Konzeptionelle Darstellung der einstufigen Authentifizierung.](media/nist-authentication-basics/nist-authentication-basics-1.png)

## <a name="multi-factor-authentication"></a>Mehrstufige Authentifizierung

Die mehrstufige Authentifizierung kann entweder durch einen mehrstufigen Authentifikator oder durch eine Kombination aus zwei einstufigen Authentifikatoren erreicht werden. Ein mehrstufiger Authentifikator erfordert zwei Authentifizierungsfaktoren, um eine einzelne Authentifizierungstransaktion auszuführen.

### <a name="multi-factor-authentication-using-two-single-factor-authenticators"></a>Mehrstufige Authentifizierung mit zwei einstufigen Authentifikatoren

Die mehrstufige Authentifizierung erfordert zwei verschiedene Authentifizierungsfaktoren. Dabei kann es sich um zwei unabhängige Authentifikatoren handeln wie 

* Auswendig gelerntes Geheimnis [Kennwort] und Out-of-Band [SMS]

* Auswendig gelerntes Geheimnis [Kennwort] und einmaliges Kennwort [Hardware oder Software]

Diese Methoden führen zwei unabhängige Authentifizierungstransaktionen mit Azure AD aus.

![Konzeptionelle Darstellung der mehrstufigen Authentifizierung mit zwei separaten Authentifikatoren.](media/nist-authentication-basics/nist-authentication-basics-2.png)


### <a name="multi-factor-authentication-using-a-single-multi-factor-authenticator"></a>Mehrstufige Authentifizierung mit zwei einstufigen Authentifikatoren

Die mehrstufige Authentifizierung erfordert einen Authentifizierungsfaktor (etwas, das Sie kennen oder etwas, das Sie sind), um einen zweiten Authentifizierungsfaktor zu entsperren. Dies ist in der Regel eine einfachere Benutzeroberfläche als mehrere unabhängige Authentifikatoren.

![Konzeptionelle Abbildung der mehrstufigen Authentifizierung mit einem einzelnen mehrstufigen Authentifikator.](media/nist-authentication-basics/nist-authentication-basics-3a.png)

Ein Beispiel ist die Microsoft Authenticator-App, die im kennwortlosen Modus verwendet wird. Mit dieser Methode versucht der Benutzer, auf eine gesicherte Ressource (vertrauende Seite) zu zugreifen, und erhält eine Benachrichtigung über seine Authentifikator-App. Der Benutzer antwortet auf eine Benachrichtigung, indem er entweder eine biometrische (etwas, das Sie sind) oder eine PIN (etwas, das Sie wissen) bereitstellt, die dann den kryptografischen Schlüssel auf dem Telefon entsperrt (etwas, das Sie haben), das dann vom Verifier überprüft wird.

## <a name="next-steps"></a>Nächste Schritte 

[NIST-Übersicht](nist-overview.md)

[Weitere Informationen zu AALs](nist-about-authenticator-assurance-levels.md)

[Authentifizierungsszenarien für Azure AD](nist-authentication-basics.md)

[NIST-Authenticator-Typen](nist-authenticator-types.md)

[Erzielen von NIST AAL1 mit Azure AD](nist-authenticator-assurance-level-1.md)

[Erzielen von NIST AAL2 mit Azure AD](nist-authenticator-assurance-level-2.md)

[Erzielen von NIST AAL3 mit Azure AD](nist-authenticator-assurance-level-3.md) 