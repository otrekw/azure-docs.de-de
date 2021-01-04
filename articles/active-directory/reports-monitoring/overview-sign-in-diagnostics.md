---
title: Welche Funktion hat die Anmeldediagnose in Azure AD? | Microsoft-Dokumentation
description: Dieser Artikel enthält eine allgemeine Übersicht über die Anmeldediagnose in Azure AD.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: e2b3d8ce-708a-46e4-b474-123792f35526
ms.service: active-directory
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 12/15/2020
ms.author: markvi
ms.reviewer: tspring
ms.collection: M365-identity-device-management
ms.openlocfilehash: d6aedf41fbf1ed0d70467a2efe97431fdecaa4fa
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/16/2020
ms.locfileid: "97585910"
---
# <a name="what-is-sign-in-diagnostic-in-azure-ad"></a>Welche Funktion hat die Anmeldediagnose in Azure AD?

Azure AD stellt ein flexibles Sicherheitsmodell bereit, mit dem Sie den Zugang der Benutzer zu den verwalteten Ressourcen und den Berechtigungsumfang steuern können. Für die Steuerung des Zugriffs auf diese Ressourcen gibt es zwei Kriterien: **wer** und **wie**. In der Regel führt die Flexibilität aufgrund der Anzahl der verfügbaren Konfigurationsoptionen zu einem gewissen Maß an Komplexität. Und Komplexität bringt ihrerseits ein höheres Fehlerrisiko mit sich.

Als IT-Administrator benötigen Sie eine Lösung, mit der Sie die richtigen Einblicke in die Aktivitäten in Ihrem System erhalten und auftretende Probleme leichter diagnostizieren und beheben können. Die Anmeldediagnose für Azure AD ist ein Beispiel für eine solche Lösung. Analysieren Sie mithilfe der Diagnose, was bei einer Anmeldung passiert ist und welche Aktionen Sie ausführen können, um die Probleme ohne Inanspruchnahme des Microsoft-Supports zu beheben.

In diesem Artikel erhalten Sie einen Überblick über die Funktionsweise der Lösung und deren Verwendung.


## <a name="requirements"></a>Anforderungen

Die Anmeldediagnose ist in allen Editionen von Azure AD verfügbar.<br> Um sie verwenden zu können, müssen Sie ein globaler Administrator Ihres Azure AD-Mandanten sein.

## <a name="how-it-works"></a>Funktionsweise

In Azure AD ist die Antwort auf einen Anmeldeversuch daran gebunden, **wer** auf Ihren Mandanten zugegriffen hat und  **wie** der Zugriff erfolgte. Als Administrator beispielsweise können Sie in der Regel alle Aspekte Ihres Mandanten konfigurieren, wenn Sie sich aus Ihrem Unternehmensnetzwerk anmelden. Sie werden unter Umständen aber sogar blockiert, wenn Sie sich mit demselben Konto aus einem nicht vertrauenswürdigen Netzwerk anmelden.
 
Aufgrund der größeren Flexibilität des Systems, auf einen Anmeldeversuch zu reagieren, kann es zu Szenarien kommen, in denen Sie eine Problembehandlung für Anmeldungen durchführen müssen. Die Anmeldediagnose hat folgende Funktionen:

- Analyse von Anmeldedaten 

- Ereignisanzeige und Empfehlungen zur Problembehebung 

Die Anmeldediagnose für Azure AD wurde für die Selbstdiagnose von Anmeldefehlern entwickelt. Zum Ausführen des Diagnosevorgangs müssen Sie folgende Schritte ausführen:

![Anmeldediagnose – Vorgehensweise](./media/overview-sign-in-diagnostics/process.png)
 
1. **Definieren** des Bereichs der in Frage kommenden Anmeldeereignisse

2. **Auswählen** der zu überprüfenden Anmeldung

3. **Überprüfen** des Diagnoseergebnisses

4. **Ausführen** von Aktionen

 
### <a name="define-scope"></a>Definieren des Bereichs

Ziel dieses Schritts ist es, den Bereich für die Anmeldungen zu definieren, die Sie untersuchen möchten. Der Bereich basiert entweder auf einem Benutzer oder einem Bezeichner (correlationId, requestId) und einem Zeitbereich. Wenn Sie den Bereich weiter eingrenzen möchten, können Sie auch einen App-Namen angeben. Azure AD verwendet die Bereichsinformationen, um die richtigen Ereignisse für Sie zu finden.  

### <a name="select-sign-in"></a>Auswählen der Anmeldung  

Anhand Ihren Suchkriterien ruft Azure AD alle passenden Anmeldungen ab und zeigt sie in Listenform in einer Authentifizierungsübersicht an. 

![Authentifizierungsübersicht](./media/overview-sign-in-diagnostics/authentication-summary.png)
 
Sie können die in dieser Ansicht angezeigten Spalten anpassen.

### <a name="review-diagnostic"></a>Überprüfen der Diagnose 

Für das ausgewählte Anmeldeereignis stellt Azure AD ein Diagnoseergebnis bereit. 

![Diagnoseergebnisse](./media/overview-sign-in-diagnostics/diagnostics-results.png)

 
Das Ergebnis beginnt mit einer Bewertung. In der Bewertung wird in wenigen Sätzen erläutert, was passiert ist. Die Erläuterung hilft Ihnen, das Verhalten des Systems zu verstehen. 

Im nächsten Schritt erhalten Sie eine Übersicht über die entsprechenden Richtlinien für bedingten Zugriff, die auf die ausgewählte Anmeldung angewendet wurden. Dieser Teil wird durch empfohlene Schritte zur Problembehebung vervollständigt. Da es nicht immer möglich ist, Probleme ohne zusätzliche Hilfe zu beheben, empfiehlt es sich, in einem weiteren Schritt ein Supportticket zu öffnen. 

### <a name="take-action"></a>Ausführen einer Aktion 
An diesem Punkt sollten Sie über die Informationen verfügen, die Sie zum Beheben Ihres Problems benötigen.


## <a name="scenarios"></a>Szenarien

Dieser Abschnitt enthält eine Übersicht über die behandelten Diagnoseszenarien. Die folgenden Szenarien sind implementiert: 
 
- Blockiert durch bedingten Zugriff

- Fehlerhafte Anmeldung durch bedingten Zugriff

- Mehrstufige Authentifizierung (MFA) für bedingten Zugriff erforderlich

- Mehrstufige Authentifizierung (MFA) aufgrund sonstiger Anforderungen erforderlich

- MFA-Nachweis erforderlich

- MFA-Nachweis erforderlich für einen Benutzeranmeldeversuch von einem unsicheren Standort

- Erfolgreiche Anmeldung


### <a name="blocked-by-conditional-access"></a>Blockiert durch bedingten Zugriff

Dieses Szenario beruht auf einer Anmeldung, die durch eine Richtlinie für bedingten Zugriff blockiert wurde.

![Zugriff blockieren](./media/overview-sign-in-diagnostics/block-access.png)

Der Diagnoseabschnitt für dieses Szenario enthält Einzelheiten zur Benutzeranmeldung und zu den angewendeten Richtlinien.


### <a name="failed-conditional-access"></a>Fehler beim bedingten Zugriff

Dieses Szenario ist in der Regel auf eine Anmeldung zurückzuführen, bei der ein Fehler aufgetreten ist, da die Anforderungen einer Richtlinie für bedingten Zugriff nicht erfüllt wurden. Typische Beispiele:

![Steuerelemente erforderlich](./media/overview-sign-in-diagnostics/require-controls.png)

- Gerät mit Hybrid-Azure AD-Einbindung erforderlich

- Genehmigte Client-App erforderlich

- App-Schutzrichtlinie erforderlich   


Der Diagnoseabschnitt für dieses Szenario enthält Einzelheiten zur Benutzeranmeldung und zu den angewendeten Richtlinien.


### <a name="mfa-from-conditional-access"></a>Mehrstufige Authentifizierung (MFA) für bedingten Zugriff erforderlich

Dieses Szenario basiert auf einer Richtlinie für bedingten Zugriff, die für die Anmeldung eine mehrstufige Authentifizierung (MFA) erfordert.

![Mehrstufige Authentifizierung erforderlich](./media/overview-sign-in-diagnostics/require-mfa.png)

Der Diagnoseabschnitt für dieses Szenario enthält Einzelheiten zur Benutzeranmeldung und zu den angewendeten Richtlinien.



### <a name="mfa-from-other-requirements"></a>Mehrstufige Authentifizierung (MFA) aufgrund sonstiger Anforderungen erforderlich

Dieses Szenario basiert auf der Anforderung einer mehrstufigen Authentifizierung, die nicht durch eine Richtlinie für bedingten Zugriff erzwungen wurde. Beispielsweise kann es sich um eine mehrstufige Authentifizierung auf Benutzerbasis handeln.


![Mehrstufige Authentifizierung pro Benutzer erforderlich](./media/overview-sign-in-diagnostics/mfa-per-user.png)


Dieses Diagnoseszenario soll weitere Einzelheiten zu folgenden Informationen bereitstellen:

- Quelle der MFA-Unterbrechung 
- Ergebnis der Clientinteraktion

Außerdem enthält dieser Abschnitt alle Einzelheiten zum Benutzeranmeldeversuch. 


### <a name="mfa-proof-up-required"></a>MFA-Nachweis erforderlich

Dieses Szenario basiert auf Anmeldungen, die von Anforderungen zum Einrichten der mehrstufigen Authentifizierung (MFA) unterbrochen wurden. Diese Einrichtung wird auch als „Nachweis“ bezeichnet.

„MFA-Nachweis erforderlich“ tritt auf, wenn ein Benutzer die mehrstufige Authentifizierung verwenden muss, sie aber noch nicht konfiguriert hat, oder wenn ein Administrator den Benutzer zum Konfigurieren der mehrstufigen Authentifizierung eingerichtet hat.

Dieses Diagnoseszenario soll Erkenntnisse zur Ursache der MFA-Unterbrechung bereitstellen und gibt an, dass die mehrstufige Authentifizierung (MFA) eingerichtet werden muss. Außerdem enthält es die Empfehlung, den Benutzer zum vollständigen Nachweis aufzufordern.

### <a name="mfa-proof-up-required-from-a-risky-sign-in"></a>MFA-Nachweis für eine riskante Anmeldung erforderlich

Dieses Szenario ist auf Anmeldungen zurückzuführen, die durch eine Anforderung zum Einrichten der mehrstufigen Authentifizierung (MFA) für eine riskante Anmeldung unterbrochen wurden. 

Dieses Diagnoseszenario soll Erkenntnisse zur Ursache der MFA-Unterbrechung bereitstellen und gibt an, dass die mehrstufige Authentifizierung (MFA) eingerichtet werden muss. Außerdem enthält es die Empfehlung, den Benutzer zum vollständigen Nachweis aufzufordern, und zwar von einem als nicht riskant eingestuften Netzwerkstandort. Wenn beispielsweise ein Unternehmensnetzwerk als benannter Standort definiert ist, versuchen Sie, den Nachweis stattdessen aus dem Unternehmensnetzwerk zu vervollständigen.


### <a name="successful-sign-in"></a>Erfolgreiche Anmeldung

Dieses Szenario basiert auf Anmeldungen, die weder durch den bedingten Zugriff noch von Anforderungen zum Einrichten der mehrstufigen Authentifizierung (MFA) unterbrochen wurden.

Dieses Diagnoseszenario stellt Erkenntnisse zu den Informationen bereit, die der Benutzer bei der Anmeldung bereitgestellt hat, und zwar für Anmeldungen, bei denen die Anwendung einer (oder mehrerer) Richtlinie für bedingten Zugriff erfolgen oder eine konfigurierte mehrstufige Authentifizierung zu einer Unterbrechung der Benutzeranmeldung führen sollte.



## <a name="next-steps"></a>Nächste Schritte

* [Was sind Azure Active Directory-Berichte?](overview-reports.md)
* [Was ist die Azure Active Directory-Überwachung?](overview-monitoring.md)
