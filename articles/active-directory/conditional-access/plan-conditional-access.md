---
title: Planen einer Azure Active Directory-Bereitstellung mit bedingtem Zugriff
description: Erfahren Sie, wie Sie Richtlinien für den bedingten Zugriff entwerfen und in Ihrer Organisation effektiv bereitstellen.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 09/17/2019
ms.author: baselden
author: BarbaraSelden
manager: daveba
ms.reviewer: joflore
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5d4ae1c9926c7ea1d18bf5c87fbed837edc2a5d5
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83641485"
---
# <a name="plan--a-conditional-access-deployment"></a>Planen einer Bereitstellung für bedingten Zugriff

Die Planung Ihrer Bereitstellung für bedingten Zugriff ist wichtig, um sicherzustellen, dass Sie die Zugriffsstrategie Ihrer Organisation für Apps und Ressourcen umsetzen.

In einer von Mobilgeräten und der Cloud geprägten Welt können Ihre Benutzer mithilfe einer Vielzahl von Geräten und Apps von überall aus auf Ressourcen Ihrer Organisation zugreifen. Daher reicht es nicht mehr aus, sich darauf zu konzentrieren, wer Zugriff auf eine Ressourcen besitzt. Sie müssen auch berücksichtigen, wo sich der Benutzer befindet, welches Gerät verwendet wird, auf welche Ressource zugegriffen wird und vieles mehr. 

Der bedingte Zugriff (Conditional Access, CA) von Azure Active Directory (Azure AD) analysiert Signale wie Benutzer, Gerät und Standort, um Entscheidungen zu automatisieren und Zugriffsrichtlinien des Unternehmens für Ressourcen durchzusetzen. Mithilfe von Richtlinien für bedingten Zugriff können Sie Zugriffssteuerungen wie Multi-Factor Authentication (MFA) anwenden. Richtlinien für bedingten Zugriff ermöglichen es Ihnen, Benutzer zur mehrstufigen Authentifizierung aufzufordern, wenn diese aus Sicherheitsgründen benötigt wird, und sie Benutzern zu ersparen, wenn sie nicht benötigt wird.

![Bedingter Zugriff – Überblick](./media/plan-conditional-access/conditional-access-overview-how-it-works.png)

Microsoft stellt bedingte Standardrichtlinien mit dem Namen [Sicherheitsstandards](https://docs.microsoft.com/azure/active-directory/fundamentals/concept-fundamentals-security-defaults) bereit, die ein grundlegendes Maß an Sicherheit sicherstellen. Ihre Organisation benötigt jedoch möglicherweise mehr Flexibilität als das, was die Sicherheitsstandards bieten. Sie können bedingten Zugriff verwenden, um Sicherheitsstandards präziserer anzupassen und neue Richtlinien zu konfigurieren, die Ihren Anforderungen entsprechen.

## <a name="learn"></a>Lernen

Stellen Sie vor Beginn sicher, dass Sie die Funktionsweise von [bedingtem Zugriff](https://docs.microsoft.com/azure/active-directory/conditional-access/overview) und dessen Verwendung verstehen.

### <a name="benefits"></a>Vorteile

Die Bereitstellung bedingten Zugriffs bietet folgende Vorteile:

* Produktivitätssteigerung. Unterbrechen Sie Benutzer nur dann mit einer Anmeldebedingung wie MFA, wenn bestimmte Aspekte dies verlangen. Mithilfe von Richtlinien für bedingten Zugriff können Sie steuern, wann Benutzer MFA verwenden müssen, wann der Zugriff blockiert wird und wann ein vertrauenswürdiges Gerät verwendet werden muss.

* Risikomanagement Die Automatisierung der Risikobewertung mit Richtlinienbedingungen bedeutet, dass riskante Anmeldungen sofort erkannt und behoben oder blockiert werden. Die Kopplung von bedingtem Zugriff mit [Identity Protection](https://docs.microsoft.com/azure/active-directory/identity-protection/overview) zum Erkennen von Anomalien und verdächtigen Ereignissen ermöglicht Ihnen Zielvorgaben, wann der Zugriff auf Ressourcen blockiert oder eingeschränkt wird. 

* Umsetzen von Compliance und Governance. Der bedingte Zugriff ermöglicht es Ihnen, den Zugriff auf Anwendungen zu überwachen, Nutzungsbedingungen zur Zustimmung bereitzustellen und den Zugriff auf Grundlage von Compliancerichtlinien einzuschränken.

* Kostenmanagement. Durch das Verschieben von Zugriffsrichtlinien in Azure AD wird die Abhängigkeit von benutzerdefinierten oder lokalen Lösungen für bedingten Zugriff und deren Infrastrukturkosten reduziert.

### <a name="license-requirements"></a>Lizenzanforderungen

Weitere Informationen finden Sie unter [Lizenzanforderungen für bedingten Zugriff](https://docs.microsoft.com/azure/active-directory/conditional-access/overview).

Sollten zusätzliche Features erforderlich sein, benötigen Sie ggf. auch entsprechende Lizenzen. Weitere Informationen finden Sie unter [Azure Active Directory – Preise](https://azure.microsoft.com/pricing/details/active-directory/).

### <a name="prerequisites"></a>Voraussetzungen

* Einen funktionierenden Azure AD-Mandanten mit aktivierter Premium- oder Testlizenz. Erstellen Sie ggf. [ein kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

* Ein Konto mit Administratorberechtigungen vom Typ „Bedingter Zugriff“.

* Ein Benutzer ohne Administratorrechte mit einem Ihnen bekannten Kennwort, z. B. „testuser“. Wenn Sie einen Benutzer erstellen müssen, finden Sie weitere Informationen unter [Schnellstart: Hinzufügen neuer Benutzer in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory) weiter.

* Eine Gruppe, der der Benutzer ohne Administratorrechte angehört. Wenn Sie eine Gruppe erstellen müssen, finden Sie weitere Informationen unter [Erstellen einer Gruppe und Hinzufügen von Mitgliedern in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-groups-create-azure-portal).

### <a name="training-resources"></a>Schulungsressourcen

Die folgenden Ressourcen können nützlich sein, wenn Sie sich über bedingten Zugriff informieren:


**Videos**
* [Was ist bedingter Zugriff?](https://youtu.be/ffMAw2IVO7A)
* [Wie wird bedingter Zugriff bereitgestellt?](https://youtu.be/c_izIRNJNuk)
* [Wie wird das Rollout von Richtlinien für bedingten Zugriff für Endbenutzer ausgeführt?](https://youtu.be/0_Fze7Zpyvc)
* [Bedingter Zugriff mit Gerätesteuerungen](https://youtu.be/NcONUf-jeS4)
* [Bedingter Zugriff mit Azure MFA](https://youtu.be/Tbc-SU97G-w)
* [Bedingter Zugriff in Enterprise Mobility + Security](https://youtu.be/A7IrxAH87wc)
* [Gerätebasierter bedingter Zugriff](https://in.video.search.yahoo.com/search/video;_ylt=AwrPiBX0yHRcZiMAhFa7HAx.;_ylu=X3oDMTB0N2poMXRwBGNvbG8Dc2czBHBvcwMxBHZ0aWQDBHNlYwNwaXZz?p=conditional+access+videos+microsoft&fr2=piv-web&fr=mcafee)

**Onlinekurse von Pluralsight**
* [Design Identity Management in Microsoft Azure](https://www.pluralsight.com/courses/microsoft-azure-identity-management-design) (Entwerfen der Identitätsverwaltung in Microsoft Azure)
* [Design Authentication for Microsoft Azure](https://www.pluralsight.com/courses/microsoft-azure-authentication-design) (Entwerfen der Authentifizierung für Microsoft Azure)
* [Design Authorization for Microsoft Azure](https://www.pluralsight.com/courses/microsoft-azure-authorization-design) (Entwerfen der Autorisierung für Microsoft Azure)

**Häufig gestellte Fragen**

[Häufig gestellte Fragen zum bedingten Zugriff mit AD](https://docs.microsoft.com/azure/active-directory/conditional-access/faqs)
## <a name="plan-the-deployment-project"></a>Planen des Bereitstellungsprojekts

Berücksichtigen Sie die Anforderungen Ihrer Organisation, während Sie die Strategie für diese Bereitstellung in Ihrer Umgebung festlegen.
### <a name="engage-the-right-stakeholders"></a>Einbeziehen der richtigen Beteiligten
Fehler in Technologieprojekten sind in der Regel auf nicht erfüllte Erwartungen auf den Gebieten Auswirkungen, Ergebnisse und Zuständigkeiten zurückzuführen. Um diese Fallstricke zu vermeiden, stellen Sie sicher, dass Sie die [richtigen Beteiligten hinzuziehen](https://aka.ms/deploymentplans) und dass die Projektrollen klar sind.

### <a name="plan-communications"></a>Planen der Benachrichtigungen
Kommunikation ist ein kritischer Faktor für den Erfolg jedes neuen Diensts. Kommunizieren Sie proaktiv mit Ihren Benutzern darüber, wie sich die Nutzung ändern wird, wann sie sich ändert und wie sie Unterstützung erhalten, wenn sie auf Probleme stoßen.

### <a name="plan-a-pilot"></a>Planen eines Pilotprojekts
Wenn neue Richtlinien für Ihre Umgebung bereit sind, stellen Sie sie in Phasen in der Produktionsumgebung bereit. Wenden Sie eine Richtlinie zunächst auf eine kleine Gruppe von Benutzern in einer Testumgebung an, und überprüfen Sie, ob sich die Richtlinie erwartungsgemäß verhält. Lesen Sie hierzu [Bewährte Methoden für einen Pilotversuch](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-deployment-plans).

> [!NOTE]
> Zum Rollout neuer Richtlinien, die nicht spezifisch für Administratoren sind, schließen Sie alle Administratoren aus. Dadurch wird sichergestellt, dass Administratoren weiterhin auf die Richtlinie zugreifen und Änderungen vornehmen oder Sie widerrufen können, wenn eine beträchtliche Auswirkung vorliegt. Überprüfen Sie die Richtlinie immer mit kleineren Benutzergruppen, bevor Sie sie auf alle Benutzer anwenden.

## <a name="understand-ca-policy-components"></a>Grundlagen der Komponenten von Richtlinien für bedingten Zugriff

Richtlinien für bedingten Zugriff sind If-Then-Anweisungen: Wenn eine zugewiesene Bedingung erfüllt ist, werden diese Zugriffssteuerungen angewendet. 

![Bedingter Zugriff – Überblick](media/plan-conditional-access/10.png)

Beim Konfigurieren von Richtlinien für bedingten Zugriff werden Bedingungen als *Zuweisungen* bezeichnet. Richtlinien für bedingten Zugriff ermöglichen das Durchsetzen von Zugriffssteuerungen für die Apps Ihrer Organisation auf der Grundlage bestimmter Zuweisungen.

![Zuweisungen und Zugriffssteuerungen ](media/plan-conditional-access/ca-policy-access.png)


Weitere Informationen finden Sie unter [Erstellen einer Richtlinie für bedingten Zugriff](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-policies).

[Zuweisungen](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-policies) definieren die

* [Benutzer und Gruppen](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-users-groups), die von der Richtlinie betroffen sind

* [Cloud-Apps oder Aktionen](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-cloud-apps), auf die die Richtlinie angewendet werden soll 

* [Bedingungen](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions), unter denen die Richtlinie angewendet wird. 
<p>

![Bildschirm „Richtlinie erstellen“](media/plan-conditional-access/create-policy.png)

[Zugriffssteuerungs](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-policies)einstellungen bestimmen, wie eine Richtlinie durchgesetzt werden soll:

* [Gewähren oder Blockieren](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-grant) des Zugriffs auf Cloud-Apps.

* [Sitzungssteuerungen ](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-session) ermöglichen das Einschränken der Benutzeroberfläche innerhalb bestimmter Cloud-Apps.

### <a name="ask-the-right-questions-to-build-your-policies"></a>Stellen Sie die richtigen Fragen zum Erstellen Ihrer Richtlinien

Richtlinien beantworten Fragen dazu, wer auf Ihre Ressourcen zugreifen soll, auf welche Ressourcen zugegriffen werden darf und unter welchen Bedingungen. Richtlinien können zum Gewähren von Zugriff oder zum Blockieren des Zugriffs entworfen werden. Stellen Sie sicher, dass Sie die richtigen Fragen dazu stellen, was Ihre Richtlinie zu erreichen versucht. 

Dokumentieren Sie die Antworten auf die einzelnen Fragen für jede Richtlinie, bevor Sie sie erstellen. 

#### <a name="common-questions-about-assignments"></a>Häufig gestellte Fragen zu Zuweisungen

[Benutzer und Gruppen](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-users-groups)

* Welche Benutzer und Gruppen werden in die Richtlinie eingeschlossen oder aus ihr ausgeschlossen?

* Umfasst diese Richtlinie alle Benutzer, bestimmte Benutzergruppen, Verzeichnisrollen oder externe Benutzer?

[Cloud-Apps oder -Aktionen](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-cloud-apps)

* Für welche Anwendung (en) gilt die Richtlinie?

* Welche Benutzeraktionen werden dieser Richtlinie unterliegen?

[Bedingungen](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions)

* Welche Geräteplattformen werden in die Richtlinie eingeschlossen oder aus ihr ausgeschlossen?

* Wo sind die vertrauenswürdigen Standorte der Organisation?

* Welche Standorte werden in die Richtlinie eingeschlossen oder aus ihr ausgeschlossen?

* Welche Client-App-Typen (Browser, mobile Desktopclients, Apps mit älteren Authentifizierungsmethoden) werden in die Richtlinie eingeschlossen bzw. aus dieser ausgeschlossen?

* Verfügen Sie über Richtlinien, die das Ausschließen von in Azure AD eingebundenen Geräten oder in Azure AD Hybrid eingebundenen Geräten aus Richtlinien erfordern? 

* Wenn Sie [Identity Protection](https://docs.microsoft.com/azure/active-directory/identity-protection/overview) verwenden, sollte Schutz vor Anmelderisiken einbezogen werden?

#### <a name="common-questions-about-access-controls"></a>Gängige Fragen zu Zugriffssteuerungen

[Gewähren oder Blockieren](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-grant) 

Möchten Sie Zugriff auf Ressourcen gewähren, indem Sie mindestens eine der folgenden Maßnahmen für erforderlich erklären?

* Anfordern von MFA

* Markieren des Geräts als kompatibel erforderlich

* Gerät mit Hybrid-Azure AD-Einbindung erforderlich

* Genehmigte Client-App erforderlich

* App-Schutzrichtlinie erforderlich

[Sitzungssteuerung](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-session)

Möchten Sie eine der folgenden Zugriffssteuerungen für Cloud-Apps erzwingen?

* Von der App erzwungene Berechtigungen verwenden

* App-Steuerung für bedingten Zugriff verwenden

* Anmeldehäufigkeit erzwingen

* Persistente Browsersitzungen verwenden

### <a name="access-token-issuance"></a>Ausstellung von Zugriffstoken

Es ist wichtig zu verstehen, wie Zugriffstoken ausgestellt werden. 

![Abbildung zur Ausstellung von Zugriffstoken](media/plan-conditional-access/CA-policy-token-issuance.png)

**Beachten Sie insbesondere Folgendes: Wenn keine Zuweisung erforderlich ist und keine Richtlinie für bedingten Zugriff aktiviert ist, besteht das Standardverhalten darin, ein Zugriffstoken auszustellen**. 

Stellen Sie sich z. B. eine Richtlinie vor. in der Folgendes gilt:

WENN sich der Benutzer in Gruppe 1 befindet, DANN MFA zum Zugriff auf App 1 erzwingen.

Wenn ein Benutzer, der kein Mitglied von Gruppe 1 ist, versucht, auf die App zuzugreifen, wird keine WENN-Bedingung erfüllt, und es wird ein Token ausgestellt. Zum Ausschließen von Benutzern außerhalb von Gruppe 1 ist eine separate Richtlinie erforderlich, um alle anderen Benutzer zu blockieren.

## <a name="follow-best-practices"></a>Bewährte Methode befolgen

Das Framework für bedingten Zugriff bietet Ihnen mehr Flexibilität bei der Konfiguration. Mehr Flexibilität bedeutet jedoch auch, dass Sie jede Konfigurationsrichtlinie vor dem Veröffentlichen sorgfältig prüfen sollten, um unerwünschte Ergebnisse zu vermeiden.

### <a name="apply-ca-policies-to-every-app"></a>Anwenden von Richtlinien für bedingten Zugriff auf jede App

Zugriffstoken werden standardmäßig ausgestellt, wenn eine Richtlinienbedingung für bedingten Zugriff keine Zugriffssteuerung auslöst. Stellen Sie sicher, dass für jede App mindestens eine Richtlinie für bedingten Zugriff angewendet wird.

> [!IMPORTANT]
> Gehen Sie bei der Verwendung von Blockieren und allen Apps in einer einzigen Richtlinie sehr vorsichtig vor. Dadurch können Administratoren für das Azure-Verwaltungsportal gesperrt werden, und Ausschlüsse können nicht für wichtige Endpunkte wie Microsoft Graph konfiguriert werden.

### <a name="minimize-the-number-of-ca-policies"></a>Minimieren der Anzahl der Richtlinien für bedingten Zugriff

Das Erstellen einer Richtlinie für jede App ist nicht effizient und führt zu einer schwierigen Verwaltung. Der bedingte Zugriff wendet nur die ersten 195 Richtlinien pro Benutzer an. Es wird empfohlen, dass Sie Ihre Apps analysieren und in Anwendungen gruppieren, die dieselben Ressourcenanforderungen für dieselben Benutzer aufweisen. Wenn beispielsweise alle Office 365-Apps oder alle HR-Apps dieselben Anforderungen für dieselben Benutzer aufweisen, erstellen Sie eine einzelne Richtlinie, und schließen Sie alle Apps in diese ein, auf die sie angewendet wird. 

### <a name="set-up-emergency-access-accounts"></a>Einrichten von Konten für den Notfallzugriff

Wenn Sie eine Richtlinie falsch konfigurieren, kann die Organisationen aus dem Azure-Portal ausgesperrt werden. Mildern Sie die Auswirkungen eines versehentlichen Aussperrens des Administrators ab, indem Sie mindestens zwei [Konten für den Notfallzugriff](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-emergency-access) in Ihrer Organisation erstellen.

* Erstellen Sie ein Benutzerkonto, das für die Richtlinienverwaltung reserviert ist und von allen Richtlinien ausgeschlossen wird.

* Glasbruchszenario für Hybridumgebungen:

  * Erstellen Sie eine lokale Sicherheitsgruppe, und synchronisieren Sie sie mit Azure AD. Die Sicherheitsgruppe sollte Ihr dediziertes Konto für die Richtlinienverwaltung enthalten. 

   * Diese Sicherheitsgruppe wird von allen Richtlinien für bedingten Zugriff AUSGENOMMEN.

   * Wenn ein Dienstausfall auftritt, fügen Sie Ihre anderen Administratoren nach Bedarf der lokalen Gruppe hinzu, und erzwingen Sie eine Synchronisierung. Dadurch wird ihr Ausschluss von Richtlinien für bedingten Zugriff veranlasst.

### <a name="set-up-report-only-mode"></a>Einrichten eines reinen Berichtsmodus

Es kann schwierig sein, die Anzahl und die Namen von Benutzern vorherzusagen, die von häufigen Bereitstellungsinitiativen betroffen sind, beispielsweise:

* Blockieren von Legacyauthentifizierung
* Erfordern von MFA
* Implementieren Richtlinien für Risiken bei der Anmeldung

Ein [reiner Berichtsmodus ](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-report-only) ermöglicht Administratoren das Auswerten der Auswirkungen von Richtlinien für bedingten Zugriff auswerten, bevor sie diese in ihrer Umgebung aktivieren.

Informieren Sie sich über das [Konfigurieren einer Richtlinie für bedingten Zugriff im reinen Berichtsmodus](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-report-only).

### <a name="plan-for-disruption"></a>Planen von Unterbrechungen

Wenn Sie den Schutz Ihrer IT-Systeme auf einer einzelnen Zugriffssteuerung wie Multi-Factor Authentication (MFA) oder einem Netzwerkstandort begründen, sind Sie anfällig für Zugriffsfehler, wenn diese einzelne Zugriffssteuerung nicht mehr verfügbar oder falsch konfiguriert ist. Um das Risiko einer Sperrung bei unvorhergesehenen Unterbrechungen zu verringern, [sehen Sie Strategien vor](https://docs.microsoft.com/azure/active-directory/authentication/concept-resilient-controls), die für Ihre Organisation übernommen werden sollen.

### <a name="set-naming-standards-for-your-policies"></a>Festlegen von Benennungsstandards für Ihre Richtlinien

Der Benennungsstandard erleichtert die Suche nach Richtlinien und gibt Aufschluss über ihren Zweck, ohne sie im Azure-Verwaltungsportal öffnen zu müssen. Es wird empfohlen, beim Benennen von Richtlinien die folgendem Angaben u berücksichtigen:

* Eine laufende Nummer

* Die Cloud-App(s), für die sie gilt

* Die Antwort

* Für wen sie gilt

* Wann sie gilt (falls zutreffend)

![Benennungsstandard](media/plan-conditional-access/11.png)

**Beispiel**: Eine Richtlinie, die erfordert, dass MFA für Marketingbenutzer, die auf die Dynamics CRP-App von externen Netzwerken aus zugreifen, verwendet wird:

![Benennungsstandard](media/plan-conditional-access/naming-example.png)

Ein beschreibender Name hilft Ihnen dabei, einen Überblick über die Implementierung von bedingtem Zugriff zu behalten. Die Sequenznummer ist hilfreich, wenn Sie auf eine Richtlinie in einer Konversation verweisen müssen. Wenn Sie beispielsweise mit einem anderen Administrator telefonieren, können Sie ihn auffordern, die Richtlinie CA01 zu öffnen, um ein Problem zu lösen.

#### <a name="naming-standards-for-emergency-access-controls"></a>Benennungsstandards für Notfallzugriffssteuerungen

Neben Ihren aktiven Richtlinien sollten Sie auch deaktivierte Richtlinien implementieren, die als sekundäre [resiliente Zugriffssteuerungen für Ausfall-/Notfallszenarien](https://docs.microsoft.com/azure/active-directory/authentication/concept-resilient-controls) fungieren. Ihr Benennungsstandard für die Notfallplan-Richtlinien sollte Folgendes enthalten:
* IM NOTFALL AKTIVIEREN am Anfang, damit sich der Name von denen anderer Richtlinien unterscheidet.

* Der Name der Störungen, auf die sie angewandt werden soll.

* Mithilfe einer Sequenznummer für die Sortierung wissen Administratoren sofort, in welcher Reihenfolge die Richtlinien aktiviert werden sollen.

**Beispiel**

Der folgende Name gibt an, dass diese Richtlinie die erste von vier Richtlinien ist, die im Fall von MFA-Unterbrechungen aktivieren soll:

EM01 – ENABLE IN EMERGENCY: MFA Disruption [1/4] – Exchange SharePoint: Require hybrid Azure AD join For VIP users.

### <a name="exclude-countries-from-which-you-never-expect-a-sign-in"></a>Ausschließen von Ländern, aus denen nie eine Anmeldung erwartet wird.

Mit Azure Active Directory können Sie [benannte Standorte](https://docs.microsoft.com/azure/active-directory/conditional-access/location-condition) erstellen. Erstellen Sie einen benannten Standort, der alle Länder enthält, von denen Sie niemals eine Anmeldung erwarten. Erstellen Sie dann eine Richtlinie für alle Apps, die die Anmeldung von diesem benannten Standort blockieren. **Stellen Sie sicher, dass Ihre Administratoren von dieser Richtlinie ausgenommen sind**.

### <a name="plan-your-policy-deployment"></a>Planen der Richtlinienbereitstellung

Wenn neue Richtlinien für Ihre Umgebung bereit sind, vergewissern Sie sich, dass Sie jede Richtlinie überprüfen, bevor Sie sie freigeben, um unerwünschte Ergebnisse zu vermeiden. In der folgenden Dokumentation finden Sie wichtige Informationen dazu, wie Richtlinien angewendet und wie Probleme vermieden werden.

* [Wichtige Informationen](https://docs.microsoft.com/azure/active-directory/conditional-access/best-practices)

* [Das sollten Sie vermeiden](https://docs.microsoft.com/azure/active-directory/conditional-access/best-practices)

## <a name="common-policies"></a>Allgemeine Richtlinien

Überlegen Sie sich bei der Planung Ihrer Richtlinienlösung für bedingten Zugriff, ob Sie Richtlinien erstellen müssen, um die folgenden Ergebnisse zu erzielen.

### <a name="require-mfa"></a>Anfordern von MFA

Gängige Anwendungsfälle für die MFA-Anforderung:

* [Zugriff durch Administratoren](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-policy-admin-mfa)

* [Zugriff auf bestimmte Apps](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-mfa)

* [Für alle Benutzer](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-policy-all-users-mfa)

* [Zugriff über nicht vertrauenswürdige Netzwerkadressen](https://docs.microsoft.com/azure/active-directory/conditional-access/untrusted-networks)

* [Für Azure-Verwaltung](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-policy-azure-management)

### <a name="respond-to-potentially-compromised-accounts"></a>Reagieren auf potenziell gefährdete Konten

Mit Richtlinien für bedingten Zugriff können Sie automatisierte Reaktionen auf Anmeldungen von potenziell gefährdeten Identitäten implementieren. Die Wahrscheinlichkeit, dass ein Konto gefährdet ist, wird in Form von Risikostufen ausgedrückt. Von Identity Protection werden zwei Risikostufen berechnet: Anmelderisiko und Benutzerrisiko. Die folgenden drei Standardrichtlinien, können aktiviert werden.

* [Festlegen, dass sich Benutzer bei der Anmeldung für MFA registrieren müssen](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-policy-risk)

* [Festlegen, dass Benutzer, die ein hohes Risiko darstellen, ihr Kennwort ändern müssen](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-policy-risk)

* [Festlegen, dass Benutzer mit einem mittleren oder hohen Anmelderisiko MFA verwenden müssen](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-policy-risk)

### <a name="require-managed-devices"></a>Vorschreiben der Verwendung verwalteter Geräte

Die zunehmende Verbreitung unterstützter Geräte für den Zugriff auf Ihre Cloudressourcen kommt der Produktivität Ihrer Benutzer zugute. Es gibt wahrscheinlich Ressourcen in Ihrer Umgebung, die nicht für Geräte mit unbekannter Schutzebene zugänglich sein sollen. Für diese betroffenen Ressourcen sollten Sie daher [sicherstellen, dass Benutzer nur unter Verwendung eines verwalteten Geräts auf sie zugreifen können](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices).

### <a name="require-approved-client-apps"></a>Vorschreiben der Verwendung genehmigter Client-Apps

Mitarbeiter verwenden ihre mobilen Geräte sowohl für private als auch für berufliche Zwecke. Bei BYOD-Szenarien müssen Sie entscheiden, ob Sie das gesamte Gerät oder nur die darin enthaltenen Daten verwalten möchten. Wenn Sie nur Daten und Zugriff verwalten, können Sie [genehmigte Cloud-Apps vorschreiben](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access), die Ihre Unternehmensdaten schützen können. Beispielsweise können Sie vorschreiben, dass auf E-Mails nur über Outlook Mobile zugegriffen werden darf und nicht über ein generisches E-Mail-Programm.

### <a name="block-access"></a>Zugriff blockieren

Die Option zum [Blockieren des gesamten Zugriffs](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-policy-block-access) ist leistungsstark. Sie kann z. B. verwendet werden, wenn Sie eine App zu Azure AD migrieren, aber noch nicht bereit sind, dass sich Benutzer bei ihr anmelden. Zugriff blockieren: 

* Besitzt hat Vorrang vor allen anderen Zuweisungen für einen Benutzer.

* Sie kann die Anmeldung bei Ihrem Mandanten für die gesamte Organisation blockieren.

> [!IMPORTANT]
> Wenn Sie eine Richtlinie erstellen, mit der der Zugriff für alle Benutzer blockiert wird, schließen Sie unbedingt Konten für den Notfallzugriff aus, und ziehen Sie den Ausschluss aller Administratoren aus der Richtlinie in Betracht.

Weitere häufige Szenarien, in denen Sie den Zugriff für Ihre Benutzer blockieren können:

* [Blockieren bestimmter Netzwerkadressen](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-policy-location) für den Zugriff auf Ihre Cloud-Apps. Mit dieser Richtlinie können Sie bestimmte Länder blockieren, aus denen kein Datenverkehr stammen sollte.

* Azure AD unterstützt Legacyauthentifizierung. Legacyauthentifizierung unterstützt jedoch keine MFA, und viele Umgebungen erfordern, dass sie Identitätssicherheit berücksichtigt. In diesem Fall können Sie verhindern, dass [Apps, die Legacyuthentifizierung verwenden](https://docs.microsoft.com/azure/active-directory/conditional-access/block-legacy-authentication), auf Ihre Mandantenressourcen zugreifen können.

## <a name="build-and-test-policies"></a>Kompilieren und Testen von Richtlinien

Stellen Sie in jeder Phase der Bereitstellung sicher, dass ausgewertet wird, ob die Ergebnisse den Erwartungen entsprechen. 

Wenn neue Richtlinien bereit sind, stellen Sie sie in Phasen in der Produktionsumgebung bereit:

* Informieren Sie die Endbenutzer intern über die Änderung.

* Beginnen Sie mit einer kleinen Gruppe von Benutzern, und vergewissern Sie sich, dass das Verhalten der Richtlinie den Erwartungen entspricht.

* Wenn Sie eine Richtlinie auf einen größeren Benutzerkreis erweitern, schließen Sie dabei weiterhin alle Administratoren aus. Durch den Ausschluss der Administratoren ist gewährleistet, dass weiterhin jemand auf die Richtlinie zugreifen kann, sollte eine Änderung erforderlich sein.

* Wenden Sie eine Richtlinie erst dann auf alle Benutzer an, nachdem sie gründlich getestet wurde. Stellen Sie sicher, dass Sie über mindestens ein Administratorkonto verfügen, für das eine Richtlinie nicht gilt.

### <a name="create-test-users"></a>Erstellen von Testbenutzern

Erstellen Sie eine Reihe von Testbenutzern, die die Benutzer in Ihrer Produktionsumgebung widerspiegeln. Mithilfe der erstellten Testbenutzer können Sie sich vergewissern, dass die Richtlinien erwartungsgemäß funktionieren, bevor Sie sie für echte Benutzer aktivieren und dadurch möglicherweise deren Zugriff auf Apps und Ressourcen unterbrechen.

Einige Organisationen nutzen hierzu Testmandanten. Es kann sich jedoch als schwierig erweisen, sämtliche Bedingungen und Apps in einem Testmandanten nachzustellen, um das Ergebnis einer Richtlinie umfassend testen zu können.

### <a name="create-a-test-plan"></a>Erstellen eines Testplans

Der Testplan ist wichtig, um die erwarteten Ergebnisse mit den tatsächlichen Ergebnissen vergleichen zu können. Sie sollten immer eine Erwartung haben, bevor Sie etwas testen. Die folgende Tabelle enthält einige Beispiele für Testfälle. Passen Sie die Szenarien und die erwarteten Ergebnisse auf der Grundlage der Konfiguration Ihrer Richtlinien für bedingten Zugriff an.

| Richtlinie| Szenario| Erwartetes Ergebnis |
| - | - | - |
| [Anfordern der MFA, wenn nicht bei der Arbeit](https://docs.microsoft.com/azure/active-directory/conditional-access/untrusted-networks)| Ein autorisierter Benutzer meldet sich bei der App an, während er sich an einem vertrauenswürdigen Ort/bei der Arbeit befindet.| Der Benutzer wird nicht zur Verwendung von MFA aufgefordert. |
| [Anfordern der MFA, wenn nicht bei der Arbeit](https://docs.microsoft.com/azure/active-directory/conditional-access/untrusted-networks)| Ein autorisierter Benutzer meldet sich bei der App an, während er sich nicht an einem vertrauenswürdigen Ort/bei der Arbeit befindet.| Der Benutzer wird zur Verwendung der MFA aufgefordert und kann sich erfolgreich anmelden. |
| [Anfordern der MFA für Administratoren](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-baseline-protect-administrators)| Ein globaler Administrator meldet sich bei der App an.| Der Administrator wird zur Verwendung der MFA aufgefordert. |
| [Riskante Anmeldungen](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-sign-in-risk-policy)| Ein Benutzer meldet sich bei der App über einen [Tor-Browser](https://microsoft.sharepoint.com/azure/active-directory/active-directory-identityprotection-playbook) an.| Der Administrator wird zur Verwendung der MFA aufgefordert. |
| [Geräteverwaltung](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices)| Ein autorisierter Benutzer versucht, sich über ein autorisiertes Gerät anzumelden.| Der Zugriff wird gewährt. |
| [Geräteverwaltung](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices)| Ein autorisierter Benutzer versucht, sich über ein nicht autorisiertes Gerät anzumelden.| Der Zugriff wird blockiert. |
| [Kennwortänderung für riskante Benutzer](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-user-risk-policy)| Ein autorisierter Benutzer versucht, sich mit gefährdeten Anmeldeinformationen anzumelden (Anmeldung mit hohem Risiko).| Der Benutzer wird aufgefordert, das Kennwort zu ändern, oder der Zugriff wird blockiert (auf der Grundlage Ihrer Richtlinie). |


 

### <a name="configure-the-test-policy"></a>Konfigurieren der Testrichtlinie

Im [Azure-Portal](https://portal.azure.com/) konfigurieren Sie Richtlinien für bedingten Zugriff unter „Azure Active Directory > Sicherheit > Bedingter Zugriff“.

Weitere Informationen zum Erstellen von Richtlinien für bedingten Zugriff finden Sie in diesem Beispiel: [Richtlinie für bedingten Zugriff, die zur Verwendung von MFA auffordert, wenn sich ein Benutzer am Azure-Portal anmeldet](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-enable-azure-mfa?toc=/azure/active-directory/conditional-access/toc.json&bc=/azure/active-directory/conditional-access/breadcrumb/toc.json). Dieser Schnellstart unterstützt Sie bei Folgendem:

* Vertrautwerden mit der Benutzeroberfläche

* Einführung in die Funktionsweise von bedingtem Zugriff

### <a name="enable-the-policy-in-report-only-mode"></a>Aktivieren der Richtlinie im reinen Berichtsmodus

Um die Auswirkungen Ihrer Richtlinie zu beurteilen, aktivieren Sie die Richtlinie zunächst im [reinen Berichtsmodus](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-report-only). Reine Berichtsrichtlinien werden bei der Anmeldung ausgewertet, Gewährungssteuerelemente und Sitzungssteuerelemente werden jedoch nicht erzwungen. Nachdem Sie die Richtlinie im reinen Berichtsmodus gespeichert haben, können Sie die Auswirkung auf Echtzeitanmeldungen in den Anmeldeprotokollen erkennen. Wählen Sie aus den Anmeldeprotokollen ein Ereignis aus, und navigieren Sie zu der Registerkarte, die nur für Berichte vorgesehen ist, um das Ergebnis jeder reinen Berichtsrichtlinie anzuzeigen.


![Reiner Berichtsmodus ](media/plan-conditional-access/report-only-mode.png)

Wenn Sie die Richtlinie auswählen, können Sie auch sehen, wie die Zuweisungen und Zugriffssteuerungen der Richtlinie mithilfe des Bildschirms für die Richtliniendetails ausgewertet wurden. Damit eine Richtlinie auf eine Anmeldung angewendet werden kann, muss jede der konfigurierten Zuweisungen erfüllt werden. 

### <a name="understand-the-impact-of-your-policies-using-the-insights-and-reporting-workbook"></a>Verstehen der Auswirkung Ihrer Richtlinien mithilfe der Arbeitsmappe „Insights und Berichte“

Sie können die aggregierten Auswirkungen Ihrer Richtlinien für bedingten Zugriff in der Arbeitsmappe „Insights und Berichte“ anzeigen. Um auf die Arbeitsmappe zuzugreifen, benötigen Sie ein Azure Monitor-Abonnement, und Sie müssen die [Anmeldeprotokolle in einen Log Analytics-Arbeitsbereich streamen](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics). 

### <a name="simulate-sign-ins-using-the-what-if-tool"></a>Simulieren von Anmeldungen mit dem Was-wäre-wenn-Tool

Eine weitere Möglichkeit zum Überprüfen Ihrer Richtlinie für den bedingten Zugriff ist die Verwendung des [Was-wäre-wenn-Tools](https://docs.microsoft.com/azure/active-directory/conditional-access/troubleshoot-conditional-access-what-if), das simuliert, welche Richtlinien für einen Benutzer gelten würden, der sich unter hypothetischen Umständen anmeldet. Wählen Sie die Anmeldeattribute aus, die Sie testen möchten (z. B. Benutzer, Anwendung, Geräteplattform und Speicherort), und sehen Sie sich an, welche Richtlinien angewendet würden.

> [!NOTE] 
> Eine simulierte Ausführung vermittelt zwar einen Eindruck von den Auswirkungen einer Richtlinie für bedingten Zugriff, ist aber kein Ersatz für einen echten Testlauf.

### <a name="test-your-policy"></a>Testen Ihrer Richtlinie

Führen Sie jeden Test im Testplan mit Testbenutzern aus.

**Testen Sie unbedingt die Ausschlusskriterien einer Richtlinie**. Sie können beispielsweise einen Benutzer oder eine Gruppe aus einer Richtlinie ausschließen, die eine mehrstufige Authentifizierung (MFA) erfordert. Testen Sie, ob ausgeschlossene Benutzer zur Verwendung der MFA aufgefordert werden, weil die Kombination anderer Richtlinien möglicherweise die Verwendung der MFA für diese Benutzer vorschreibt.

### <a name="roll-back-policies"></a>Rollback von Richtlinien

Sollte für Ihre neu implementierten Richtlinien ein Rollback erforderlich sein, verwenden Sie mindestens eine der folgenden Optionen:

* **Deaktivieren der Richtlinie.** Deaktivierte Richtlinien werden bei Anmeldeversuchen von Benutzern nicht angewendet. Die Richtlinie kann bei Bedarf jederzeit erneut aktiviert werden.

![Abbildung zum Aktivieren einer Richtlinie](media/plan-conditional-access/enable-policy.png)

* **Ausschließen eines Benutzers oder einer Gruppe aus einer Richtlinie.** Falls ein Benutzer nicht auf die App zugreifen kann, können Sie ihn aus der Richtlinie ausschließen.

![Ausschließen von Benutzern und Gruppen](media/plan-conditional-access/exclude-users-groups.png)

> [!NOTE]
>  Diese Option sollte nur für vertrauenswürdige Benutzer verwendet werden. Der Benutzer sollte der Richtlinie oder Gruppe baldmöglichst wieder hinzugefügt werden.

* **Löschen der Richtlinie.** [Löschen](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-enable-azure-mfa?toc=/azure/active-directory/conditional-access/toc.json&bc=/azure/active-directory/conditional-access/breadcrumb/toc.json) Sie die Richtlinie, wenn Sie sie nicht mehr benötigen.

## <a name="manage-access-to-cloud-apps"></a>Verwalten des Zugriffs auf Cloud-Apps

Verwenden Sie die folgenden Verwaltungsoptionen zum Steuern und Verwalten Ihrer Richtlinien für bedingten Zugriff:

![manage-access](media/plan-conditional-access/manage-access.png)


### <a name="named-locations"></a>Benannte Orte

Mit der Standortbedingung einer Richtlinie für bedingten Zugriff können Sie die Einstellungen der Zugriffssteuerung an die Netzwerkstandorte Ihrer Benutzer knüpfen. Mithilfe von [benannten Standorten](https://docs.microsoft.com/azure/active-directory/conditional-access/location-condition) können Sie logische Gruppierungen von IP-Adressbereichen oder Ländern und Regionen erstellen.

### <a name="custom-controls"></a>Benutzerdefinierte Steuerelemente

[Benutzerdefinierte Steuerelemente](https://docs.microsoft.com/azure/active-directory/conditional-access/controls) leiten Benutzer an kompatible Dienste um, um Authentifizierungsanforderungen außerhalb von Azure AD zu genügen. Um die Bedingungen dieses Steuerelements zu erfüllen, wird der Browser eines Benutzers an den externen Dienst umgeleitet, die erforderliche Authentifizierung wird durchgeführt, dann wird der Browser erneut an Azure AD umgeleitet. Azure AD überprüft die Antwort, und wenn der Benutzer erfolgreich authentifiziert oder überprüft wurde, verbleibt der Benutzer im Vorgangsfluss des bedingten Zugriffs.

### <a name="terms-of-use"></a>Nutzungsbedingungen

Vor dem Zugriff auf bestimmte Cloud-Apps in Ihrer Umgebung können Sie die Zustimmung von Benutzern erlangen, indem diese Ihre Nutzungsbedingungen akzeptieren. Befolgen Sie diesen [Schnellstart zum Erstellen der Nutzungsbedingungen](https://docs.microsoft.com/azure/active-directory/conditional-access/require-tou).

### <a name="classic-policies"></a>Klassische Richtlinien

Im [Azure-Portal](https://portal.azure.com/) finden Sie Ihre Richtlinien für bedingten Zugriff unter „Azure Active Directory > Sicherheit > Bedingter Zugriff“. In Ihrer Organisation können auch ältere Richtlinien für bedingten Zugriff vorhanden sein, die nicht mithilfe dieser Seite erstellt wurden. Diese Richtlinien werden als „klassische Richtlinien“ bezeichnet. Es wird empfohlen, diese [klassischen Richtlinien in das Azure-Portal zu migrieren](https://docs.microsoft.com/azure/active-directory/conditional-access/best-practices).

## <a name="troubleshoot-conditional-access"></a>Problembehandlung für bedingten Zugriff

Wenn ein Benutzer ein Problem mit Richtlinie für bedingten Zugriff hat, erfassen Sie die folgenden Informationen, um die Problembehandlung zu vereinfachen.

* Benutzerprinzipalname

* Anzeigename des Benutzers

* Betriebssystemname

* Zeitstempel (ungefähre Angabe ist OK)

* Zielanwendung

* Clientanwendungstyp (Browser oder Client)

* Korrelations-ID (diese ist für die Anmeldung eindeutig)

Wenn der Benutzer eine Nachricht mit einem Link zu weiteren Details erhalten hat, kann er die meisten dieser Informationen für Sie erfassen.

![App-Fehlermeldung kann nicht abgerufen werden](media/plan-conditional-access/cant-get-to-app.png)

Nachdem Sie diese Informationen erfasst haben, finden Sie weitere Beschreibungen in den folgenden Ressourcen:

* [Anmeldeprobleme bei bedingtem Zugriff](https://docs.microsoft.com/azure/active-directory/conditional-access/troubleshoot-conditional-access): Mithilfe von Fehlermeldungen und dem Azure AD-Anmeldeprotokoll können Sie Probleme bei unerwarteten Anmeldeergebnissen im Zusammenhang mit bedingtem Zugriff beheben.

* [Verwenden des Was-wäre-wenn-Tools](https://docs.microsoft.com/azure/active-directory/conditional-access/troubleshoot-conditional-access-what-if): Verstehen Sie, warum eine Richtlinie in einem bestimmten Szenario auf einen Benutzer angewendet oder nicht angewendet wurde oder ob eine Richtlinie in einem bekannten Zustand angewendet würde.

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr über Multi-Factor Authentication (MFA)](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks)

[Erfahren Sie mehr über Identity Protection](https://docs.microsoft.com/azure/active-directory/identity-protection/overview-identity-protection)

[Verwalten von Richtlinien für bedingten Zugriff mit der Microsoft Graph-API](https://docs.microsoft.com/graph/api/resources/conditionalaccesspolicy?view=graph-rest-beta)
