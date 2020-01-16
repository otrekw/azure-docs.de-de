---
title: Planen der HR-Cloudanwendung für die Azure Active Directory-Benutzerbereitstellung
description: In diesem Artikel wird der Bereitstellungsprozess der Integration von HR-Cloudsystemen wie Workday und Sucessfactors mit Azure Active Directory beschrieben. Die Integration von Azure AD in Ihr HR-Cloudsystem führt zu einem vollständigen Identity Lifecycle Management-System.
services: active-directory
author: martincoetzer
manager: CelesteDG
tags: azuread
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 11/22/2019
ms.author: martinco
ms.reviewer: arvindha
ms.openlocfilehash: 6f72371077aab813cc22c9bbbe755fdfaa9ac00a
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75433833"
---
# <a name="plan-cloud-hr-application-to-azure-active-directory-user-provisioning"></a>Planen der HR-Cloudanwendung für die Azure Active Directory-Benutzerbereitstellung

In der Vergangenheit haben sich IT-Mitarbeiter auf manuelle Methoden zum Erstellen, Aktualisieren und Löschen von Mitarbeitern verlassen, z. B. das Hochladen von CSV-Dateien oder benutzerdefinierte Skripts zum Synchronisieren von Mitarbeiterdaten. Diese Bereitstellungsprozesse sind jedoch fehleranfällig, unsicher und schwer zu verwalten.

Für ein problemloses End-to-End-Identity Lifecycle Management von Mitarbeitern, Anbietern oder vorübergehend Beschäftigten bietet der [Azure Active Directory-Bereitstellungsdienst (Azure AD)](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning) die Integration mit cloudbasierten Anwendungen für das Personalwesen (Human Resources, HR) wie Workday oder SuccessFactors.

Azure AD nutzt diese Integration, um die folgenden Workflows für HR-Cloudanwendungen (Apps) zu ermöglichen:

- **Bereitstellung von Benutzern in AD**: Sie können ausgewählte Gruppen von Benutzern aus einer HR-App in eine oder mehrere Active Directory-Domänen (AD) bereitstellen.
- **Bereitstellung von reinen Cloudbenutzern in Azure AD**: In Szenarien, in denen AD nicht verwendet wird, stellen Sie Benutzer direkt aus der HR-Cloud-App in Azure AD bereit.
- **Zurückschreiben in die HR-Cloud-App**: Schreiben Sie die E-Mail-Adressen und Benutzernamenattribute aus Azure AD zurück in die HR-Cloud-App.

> [!NOTE]
> Dieser Bereitstellungsplan zeigt Ihnen, wie Sie Ihre Workflows der HR-Cloud-App mithilfe der Azure AD-Benutzerbereitstellung bereitstellen. Informationen zur Bereitstellung der automatischen Benutzerbereitstellung für SaaS-Apps finden Sie unter [Planen einer automatischen Benutzerbereitstellung](https://aka.ms/deploymentplans/provisioning).

## <a name="enabled-hr-scenarios"></a>Unterstützte HR-Szenarien

Der Azure AD-Benutzerbereitstellungsdienst ermöglicht die Automatisierung der folgenden HR-basierten Identity Lifecycle Management-Szenarien:

- **Einstellen neuer Mitarbeiter**: Wenn ein neuer Mitarbeiter zur HR-Cloud-App hinzugefügt wird, wird in AD und Azure AD automatisch ein Benutzerkonto mit der Option erstellt, die Attribute für E-Mail-Adresse und Benutzername in die HR-Cloud-App zurückzuschreiben.
- **Aktualisierungen von Mitarbeiterattributen und -profilen**: Wenn ein Mitarbeiterdatensatz in der HR-Cloud-App aktualisiert wird (z. B. Name, Titel oder Vorgesetzter), wird das entsprechende Benutzerkonto automatisch in AD und Azure AD aktualisiert.
- **Kündigungen von Mitarbeitern**: Wenn ein Mitarbeiter in der HR-Cloud-App gekündigt wird, wird das entsprechende Benutzerkonto in AD und Azure AD automatisch deaktiviert.
- **Erneute Einstellung von Mitarbeitern**: Wenn ein Mitarbeiter in der HR-Cloud-App erneut eingestellt wird, kann sein altes Konto in AD und Azure AD automatisch reaktiviert oder erneut bereitgestellt werden.

## <a name="who-is-this-integration-best-suited-for"></a>Für wen ist diese Integration am besten geeignet?

Die Integration der HR-Cloud-App mit der Azure AD-Benutzerbereitstellung ist ideal für Unternehmen geeignet, für die Folgendes gilt:

- Organisationen, die eine vorgefertigte, cloudbasierte Lösung für die HR-Cloud-Benutzerbereitstellung verwenden möchten.
- Organisationen, bei denen Benutzer direkt aus der HR-Cloud-App in AD oder Azure AD bereitgestellt werden müssen.
- Organisationen, bei denen Benutzer mithilfe von Daten bereitgestellt werden müssen, die aus der HR-Cloud-App abgerufen werden.
- Organisationen, bei denen Benutzer beim Beitreten, Verschieben und Verlassen nur auf Grundlage von Änderungsinformationen, die in der HR-Cloud-App erkannt werden, mit einer oder mehreren AD-Gesamtstrukturen, -Domänen und -Organisationseinheiten synchronisiert werden müssen.
- Organisationen, die Office 365 für E-Mail-Funktionen verwenden.

## <a name="learn"></a>Lernen

Bei der Benutzerbereitstellung wird die Grundlage für eine fortlaufende Identitätsgovernance geschaffen und die Qualität von Geschäftsprozessen verbessert, die auf autoritativen Identitätsdaten basieren.

### <a name="terms"></a>Begriffe

In diesem Artikel werden die folgenden Begriffe verwendet:

- **Quellsystem**: Das Repository von Benutzern, aus dem die Azure AD-Instanz bereitstellt (z. B. eine HR-Cloud-App wie Workday und SuccessFactors).
- **Zielsystem**: Das Repository von Benutzern, in das die Azure AD-Instanz bereitstellt (z. B. AD, Azure AD, Office 365 oder andere SaaS-Apps).
- **Prozess für Einstellungen/Wechsel/Kündigungen**: Eine Beschreibung, die für Neueinstellungen, Versetzungen und Kündigungen unter Verwendung einer HR-Cloud-App als Datensatzsystem verwendet wird. Der Prozess ist abgeschlossen, wenn der Dienst die erforderlichen Attribute für das Zielsystem erfolgreich bereitstellt.

### <a name="key-benefits"></a>Hauptvorteile

Diese Funktion der vom Personalwesen gesteuerten IT-Bereitstellung bietet erhebliche Geschäftsvorteile, wie unten aufgeführt:

- **Produktivitätssteigerung**: Sie können jetzt die Zuweisung von Benutzerkonten und Office 365-Lizenzen automatisieren und den Zugriff auf Schlüsselgruppen ermöglichen. Die Automatisierung von Aufgaben bietet Neueinstellungen sofortigen Zugriff auf ihre Arbeitstools und sie erhöht die Produktivität.
- **Risikomanagement**: Sie können die Sicherheit erhöhen, indem Sie Änderungen basierend auf dem Mitarbeiterstatus oder den Gruppenmitgliedschaften automatisieren, wobei die Daten aus der HR-Cloud-App einfließen. Die Automatisierung von Änderungen stellt sicher, dass Benutzeridentitäten und der Zugriff auf wichtige Anwendungen automatisch aktualisiert werden, wenn Benutzer innerhalb des Unternehmen wechseln oder es verlassen.
- **Erfüllung von Compliance und Governance**: Azure AD unterstützt native Überwachungsprotokolle für Anforderungen zur Benutzerbereitstellung, die von Apps von Quell- und Zielsystemen ausgeführt werden. Durch die Überwachung können Sie über einen einzigen Bildschirm nachverfolgen, wer Zugriff auf die Apps hat.
- **Kostenmanagement**: Die automatische Bereitstellung reduziert Kosten, indem Ineffizienzen und menschliche Fehler vermieden werden, die bei einer manuellen Bereitstellung entstehen. Es reduziert den Bedarf an benutzerdefiniert entwickelten Lösungen für die Benutzerbereitstellung, die im Laufe der Zeit mithilfe älterer und veralteter Plattformen erstellt wurden.

### <a name="licensing"></a>Lizenzierung

Um die HR-Cloud-App für die Integration der Azure AD-Benutzerbereitstellung zu konfigurieren, benötigen Sie eine gültige [Azure AD Premium-Lizenz](https://azure.microsoft.com/pricing/details/active-directory/) und eine Lizenz für die HR-Cloud-App wie Workday oder SuccessFactors.

Sie benötigen zudem eine gültige Azure AD Premium P1- oder höhere Abonnementlizenz für jeden Benutzer, der aus der HR-Cloud-App erstellt und in einer AD- oder Azure AD-Instanz bereitgestellt wird. Eine nicht ausreichende Anzahl von Lizenzen, die sich im Besitz der HR-Cloud-App befinden, kann zu Fehlern bei der Benutzerbereitstellung führen.

### <a name="prerequisites"></a>Voraussetzungen

- Zugriff eines globalen Azure AD-Administrators zum Konfigurieren des Bereitstellungs-Agents von Azure AD Connect
- Eine Test- und Produktionsinstanz der HR-Cloud-App
- Administratorberechtigungen in der HR-Cloud-App zum Erstellen eines Systemintegrationsbenutzers für Testzwecke und Vornehmen von Änderungen zum Testen von Mitarbeiterdaten
- Für die Benutzerbereitstellung in AD einen Server mit mindestens Windows Server 2012 und der .NET-Runtime 4.7.1 und höher zum Hosten des [Bereitstellungs-Agents von Azure AD Connect](https://go.microsoft.com/fwlink/?linkid=847801)
- [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-azure-ad-connect) für die Synchronisierung von Benutzern zwischen AD und Azure AD

### <a name="training-resources"></a>Schulungsressourcen

| **Ressourcen** | **Link und Beschreibung** |
|:-|:-|
| Videos | [Was ist die Benutzerbereitstellung in Azure Active Directory?](https://youtu.be/_ZjARPpI6NI) |
| | [Implementieren der Benutzerbereitstellung in Azure Active Directory](https://youtu.be/pKzyts6kfrw) |
| Lernprogramme | Weitere Informationen finden Sie in der [Liste der Tutorials zur Integration von SaaS-Anwendungen in Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list). |
| | [Tutorial: Konfigurieren von Workday für die automatische Benutzerbereitstellung](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#frequently-asked-questions-faq) |
| Häufig gestellte Fragen | [Automatisierte Benutzerbereitstellung](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning#what-applications-and-systems-can-i-use-with-azure-ad-automatic-user-provisioning) |
| | [Bereitstellung aus Workday in Azure AD](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#frequently-asked-questions-faq) |

### <a name="solution-architecture"></a>Lösungsarchitektur

Im folgenden Beispiel wird die Lösungsarchitektur der End-to-End-Benutzerbereitstellung für häufige Hybridumgebungen beschrieben, die Folgendes umfasst:

- **Autoritativer HR-Datenfluss – aus der HR-Cloud-App in AD**: In diesem Flow wird das HR-Ereignis (Prozess für Einstellungen/Wechsel/Kündigungen) im Mandanten der HR-Cloud-App eingeleitet. Der Azure AD-Bereitstellungsdienst und der Azure AD Connect-Bereitstellungs-Agent stellen die Benutzerdaten aus der HR-Cloud-App in AD zur Verfügung. Abhängig vom Ereignis kann dies dann in Active Directory zu Erstellungs-, Aktualisierungs-, Aktivierungs- oder Deaktivierungsvorgängen führen.
- **Synchronisieren mit Azure AD und Rückschreiben von E-Mail-Adresse und Benutzername aus lokalem AD in die HR-Cloud-App**: Sobald die Konten in AD aktualisiert wurden, wird es über Azure AD Connect mit Azure AD synchronisiert, und die E-Mail-Adressen und Benutzernamenattribute können an den Mandanten der HR-Cloud-App rückgeschrieben werden.

![Workflowdiagramm](./media/plan-cloudhr-provisioning/plan-cloudhr-provisioning-img1.png)

#### <a name="description-of-workflow"></a>Beschreibung des Workflows

Die im Diagramm angegebenen Hauptschritte sind:  

1. Das **HR-Team** führt die Transaktionen im Mandanten der HR-Cloud-App durch.
2. Der **Azure AD-Bereitstellungsdienst** führt die geplanten Zyklen vom Mandanten der HR-Cloud-App aus und erkennt Änderungen, die zur Synchronisierung mit AD verarbeitet werden müssen.
3. Der **Azure AD-Bereitstellungsdienst** ruft den Azure AD Connect-Bereitstellungs-Agent mit einer Anforderungsnutzlast auf, die die Erstellungs-, Aktualisierungs-, Aktivierungs- oder Deaktivierungsvorgänge für das AD-Konto enthält.
4. Der **Azure AD Connect-Bereitstellungs-Agent** verwendet ein Dienstkonto zum Verwalten von AD-Kontodaten.
5. **Azure AD Connect** führt eine [Deltasynchronisierung](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-whatis) aus, um Updates in AD zu pullen.
6. **AD**-Updates werden mit Azure AD synchronisiert.
7. Der **Azure AD-Bereitstellungsdienst** schreibt E-Mail-Attribut und Benutzername aus Azure AD in den Mandanten der HR-Cloud-App zurück.

## <a name="plan-the-deployment-project"></a>Planen des Bereitstellungsprojekts

Berücksichtigen Sie die Anforderungen Ihrer Organisation, während Sie die Strategie für diese Bereitstellung in Ihrer Umgebung festlegen.

### <a name="engage-the-right-stakeholders"></a>Einbeziehen der richtigen Beteiligten

Fehler in Technologieprojekten sind in der Regel auf nicht erfüllte Erwartungen auf den Gebieten Auswirkungen, Ergebnisse und Zuständigkeiten zurückzuführen. Um diese Fallstricke zu vermeiden, [achten Sie darauf, die richtigen Beteiligten einzubeziehen](https://aka.ms/deploymentplans) und die Rolle der Beteiligten präzise zu definieren, indem Sie die Beteiligten, ihren Projektbeitrag und ihre Zuständigkeiten dokumentieren.

Sie müssen einen Mitarbeiter der Personalabteilung einbeziehen, der Eingaben zu bestehenden Geschäftsprozessen der Personalabteilung und zur Mitarbeiteridentität sowie zu Anforderungen an die Auftragsdatenverarbeitung bereitstellen kann.

### <a name="plan-communications"></a>Planen der Benachrichtigungen

Kommunikation ist ein kritischer Faktor für den Erfolg jedes neuen Diensts. Sie sollten proaktiv mit Ihren Benutzern darüber kommunizieren, wie sich die Nutzung ändern wird, wann sie sich ändert und wie sie Unterstützung erhalten, wenn sie auf Probleme stoßen.

### <a name="plan-a-pilot"></a>Planen eines Pilotprojekts

Die Integration von Geschäftsprozessen der Personalabteilung und von Identitätsworkflows aus der HR-Cloud-App in die Zielsysteme erfordert ein erhebliches Maß an Datenüberprüfung, Datentransformation, Datenbereinigung und End-to-End-Tests, bevor die Lösung in der Produktionsumgebung bereitgestellt wird.

Es wird empfohlen, die anfängliche Konfiguration in einer [Pilotumgebung](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-deployment-plans#best-practices-for-a-pilot) auszuführen, bevor sie für alle Benutzer in der Produktionsumgebung skaliert wird.

## <a name="select-cloud-hr-provisioning-connector-apps"></a>Auswählen von HR-Cloudbereitstellungsconnector-Apps

Um Azure AD-Bereitstellungsworkflows zwischen der HR-Cloud-App und AD zu ermöglichen, gibt es mehrere Bereitstellungsconnector-Apps, die Sie über den Azure AD-App-Katalog hinzufügen können:

- **HR-Cloud-App zur AD-Benutzerbereitstellung**: Diese Bereitstellungsconnector-App ermöglicht die Bereitstellung von Benutzerkonten von der HR-Cloud-App für eine einzelne AD-Domäne. Wenn Sie über mehrere Domänen verfügen, können Sie eine Instanz dieser App aus dem Azure AD-App-Katalog für jede AD-Domäne hinzufügen, in der die Bereitstellung erfolgen soll.
- **HR-Cloud-App zur Azure AD-Benutzerbereitstellung**: Obwohl Azure AD Connect das Tool ist, das Sie zum Synchronisieren von AD-Benutzern mit Azure AD verwenden sollten, kann auch diese Bereitstellungsconnector-App verwendet werden, um die Bereitstellung reiner Cloudbenutzer aus der HR-Cloud-App in einem einzelnen Azure AD-Mandanten zu erleichtern.
- **Rückschreiben der HR-Cloud-App**: Diese Bereitstellungsconnector-App ermöglicht das Rückschreiben der E-Mail-Adressen des Benutzers aus Azure AD in die HR-Cloud-App.

In der folgenden Abbildung sind z. B. die Workday-Connector-Apps aufgelistet, die im Azure AD-App-Katalog verfügbar sind.

![Azure Active Directory-Portal – App-Katalog](./media/plan-cloudhr-provisioning/plan-cloudhr-provisioning-img2.png)

### <a name="decision-flowchart"></a>Entscheidungsflussdiagramm

Verwenden Sie das untenstehende Entscheidungsflussdiagramm, um zu ermitteln, welche HR-Cloudbereitstellungs-App für Ihr Szenario relevant sind.

![Entscheidungsflussdiagramm](./media/plan-cloudhr-provisioning/plan-cloudhr-provisioning-img3.png)

## <a name="design-azure-ad-connect-provisioning-agent-deployment-topology"></a>Entwerfen der Bereitstellungstopologie des Azure AD Connect-Bereitstellungs-Agents

Die Bereitstellungsintegration zwischen der HR-Cloud-App und AD erfordert die folgenden vier Komponenten:

- Mandant der HR-Cloud-App
- Bereitstellungsconnector-App
- Azure AD Connect-Bereitstellungs-Agent
- AD-Domäne

Die Bereitstellungstopologie des Azure AD Connect-Bereitstellungs-Agents hängt von der Anzahl der HR-Cloud-Apps und untergeordneten AD-Domänen ab, die Sie integrieren möchten. Wenn Sie über mehrere AD-Domänen verfügen, hängt es davon ab, ob die AD-Domänen zusammenhängend oder [nicht zusammenhängend](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/disjoint-namespace) sind.

Wählen Sie auf der Grundlage Ihrer Entscheidung eines der Bereitstellungsszenarien aus:

- Einzelner HR-Cloud-App-Mandant -> ausgerichtet auf einzelne oder mehrere untergeordnete AD-Domänen in einer vertrauenswürdigen Gesamtstruktur
- Einzelner HR-Cloud-App-Mandant -> ausgerichtet auf mehrere untergeordnete Domänen in einer nicht zusammenhängenden AD-Gesamtstruktur

### <a name="single-cloud-hr-app-tenant---single-or-multiple-target-ad-child-domains-in-a-trusted-forest"></a>Einzelner HR-Cloud-App-Mandant -> einzelne oder mehrere untergeordnete AD-Zieldomänen in einer vertrauenswürdigen Gesamtstruktur

Folgende Produktionskonfigurationen werden empfohlen:

|Anforderung|Empfehlung|
|:-|:-|
|Anzahl der bereitzustellenden Azure AD Connect-Bereitstellungs-Agents|2 (für Hochverfügbarkeit und Failover)
|Anzahl der zu konfigurierenden Bereitstellungsconnector-Apps|Eine App pro untergeordneter Domäne|
|Serverhost für Azure AD Connect-Bereitstellungs-Agent|Windows 2012 R2 oder höher mit „Sichtverbindung“ zu geografischen AD-Domänencontrollern</br>Koexistenz mit dem Azure AD Connect-Dienst ist möglich|

![Flow zu lokalen Agents](./media/plan-cloudhr-provisioning/plan-cloudhr-provisioning-img4.png)

### <a name="single-cloud-hr-app-tenant---target-multiple-child-domains-in-a-disjoint-ad-forest"></a>Einzelner HR-Cloud-App-Mandant -> ausgerichtet auf mehrere untergeordnete Domänen in einer nicht zusammenhängenden AD-Gesamtstruktur

Dieses Szenario umfasst die Bereitstellung von Benutzern aus der HR-Cloud-App in Domänen in nicht zusammenhängenden AD-Gesamtstrukturen.

Folgende Produktionskonfigurationen werden empfohlen:

|Anforderung|Empfehlung|
|:-|:-|
|Anzahl der lokal bereitzustellenden Azure AD Connect-Bereitstellungs-Agents|2 pro nicht zusammenhängender AD-Gesamtstruktur|
|Anzahl der zu konfigurierenden Bereitstellungsconnector-Apps|Eine App pro untergeordneter Domäne|
|Serverhost für Azure AD Connect-Bereitstellungs-Agent|Windows 2012 R2 oder höher mit „Sichtverbindung“ zu geografischen AD-Domänencontrollern</br>Koexistenz mit dem Azure AD Connect-Dienst ist möglich|

![Einzelne nicht zusammenhängende AD-Gesamtstruktur für den HR-Cloud-App-Mandanten](./media/plan-cloudhr-provisioning/plan-cloudhr-provisioning-img5.png)

### <a name="azure-ad-connect-provisioning-agent-requirements"></a>Anforderungen des Azure AD Connect-Bereitstellungs-Agents

Die Lösung für die Benutzerbereitstellung von der HR-Cloud-App in AD erfordert das Bereitstellen mindestens eines Azure AD Connect-Bereitstellungs-Agents auf Servern unter Windows 2012 R2 oder höher mit mindestens 4 GB RAM und der .NET-Runtime 4.7.1 oder höher. Stellen Sie sicher, dass der Hostserver Netzwerkzugriff auf die AD-Zieldomäne hat.

Um die lokale Umgebung vorzubereiten, registriert der Konfigurationsassistent des Azure AD Connect-Bereitstellungs-Agents den Agent bei Ihrem Azure AD-Mandanten, [öffnet Ports](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application#open-ports), [gestattet den Zugriff auf URLs](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application#allow-access-to-urls) und unterstützt die [ausgehende HTTPS-Proxy-Konfiguration](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#how-do-i-configure-the-provisioning-agent-to-use-a-proxy-server-for-outbound-http-communication).

Der Bereitstellungs-Agent verwendet ein Dienstkonto für die Kommunikation mit den AD-Domänen. Vor der Installation des Agents empfehlen wir Ihnen, in „AD-Benutzer und -Computer“ ein Dienstkonto zu erstellen, das die folgenden Anforderungen erfüllt:

- Ein nicht ablaufendes Kennwort
- Delegierte Steuerungsberechtigungen zum Lesen, Erstellen, Löschen und Verwalten von Benutzerkonten

Sie können Domänencontroller auswählen, die Bereitstellungsanforderungen verarbeiten sollen. Wenn Sie über mehrere geografisch verteilte Domänencontroller verfügen, Installieren Sie den Bereitstellungs-Agent am selben Standort wie Ihre bevorzugten Domänencontroller. Damit steigern Sie die Zuverlässigkeit und Leistung der End-to-End-Lösung.

Für Hochverfügbarkeit können Sie auch mehrere Azure AD Connect-Bereitstellungs-Agents bereitstellen und registrieren, die dann den gleichen Satz von lokalen AD-Domänen behandeln.

## <a name="plan-scoping-filters-and-attribute-mapping"></a>Planen von Bereichsfiltern und Attributzuordnung

Wenn Sie die Bereitstellung von der HR-Cloud-App in AD oder Azure AD aktivieren, steuert das Azure-Portal die Attributwerte durch Attributzuordnung.

### <a name="define-scoping-filters"></a>Definieren von Bereichsfiltern

Verwenden Sie [Bereichsfilter](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters), um die attributbasierten Regeln zu definieren, die bestimmen, welche Benutzer von der HR-Cloud-App in AD oder Azure AD bereitgestellt werden sollen.

Wenn Sie den Einstellungsprozess starten, erfassen Sie die folgenden Anforderungen:

- Wird die HR-Cloud-App sowohl für Mitarbeiter als auch für vorübergehend Beschäftigte verwendet?
- Planen Sie, die HR-Cloud-App für die Azure AD-Benutzerbereitstellung zu verwenden, um sowohl Mitarbeiter als auch vorübergehend Beschäftigte zu verwalten?
- Planen Sie, den Rollout der HR-Cloud-App auf die Azure AD-Bereitstellung nur für eine Teilmenge der Benutzer der HR-Cloud-App (z. B. nur Mitarbeiter) durchzuführen?

Abhängig von Ihren Anforderungen können Sie bei der Konfiguration von Attributzuordnungen das Feld **Quellobjektbereich** so festlegen, dass Sie auswählen können, welche Benutzergruppen in der HR-Cloud-App für die Bereitstellung in AD in Frage kommen. Informationen zu häufig verwendeten Bereichsfiltern finden Sie im Tutorial zur HR-Cloud-App.

### <a name="determine-matching-attributes"></a>Ermitteln von übereinstimmenden Attributen

Mit der Bereitstellung erhalten Sie die Möglichkeit, bestehende Konten zwischen Quell- und Zielsystem abzugleichen. Wenn Sie die HR-Cloud-App in den Azure AD-Bereitstellungsdienst integrieren, können Sie die [Attributzuordnung konfigurieren](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-automatic-user-provisioning-portal#mappings), um zu bestimmen, welche Benutzerdaten von der HR-Cloud-App zu AD oder Azure AD gesendet werden sollen.

Wenn Sie den Einstellungsprozess starten, erfassen Sie die folgenden Anforderungen:

- Welche ist die eindeutige ID in dieser HR-Cloud-App, mit der die einzelnen Benutzer identifiziert werden?
- Wie gehen Sie aus der Sicht des Identitätslebenszyklus mit Wiedereinstellungen um? Behalten Wiedereinstellungen ihre alte Mitarbeiter-ID?
- Werden von Ihnen zukunftsorientierte Einstellungen verarbeitet und AD-Konten für sie im Voraus erstellt?
- Wie gehen Sie aus der Sicht des Identitätslebenszyklus mit der Umwandlung von einem Mitarbeiter zu einem vorübergehend Beschäftigtem oder ähnlichem um?
- Behalten umgewandelte Benutzer ihr altes AD-Konto oder erhalten sie neue Konten?

In Abhängigkeit von Ihren Anforderungen unterstützt Azure AD durch die direkte Zuordnung von Attribut zu Attribut, die Bereitstellung konstanter Werte oder das [Schreiben von Ausdrücken für Attributzuordnungen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-writing-expressions-for-attribute-mappings). Durch diese Flexibilität können Sie genau steuern, was im Attribut der Ziel-App aufgefüllt wird. Mit der [Microsoft Graph-API](https://docs.microsoft.com/azure/active-directory/manage-apps/export-import-provisioning-configuration) und dem Graph-Tester können Sie die Attributzuordnungen und das Schema Ihrer Benutzerbereitstellung in eine JSON-Datei exportieren und wieder in Azure AD importieren.

Das Attribut in der HR-Cloud-App, das die eindeutige Mitarbeiter-ID darstellt, wird **standardmäßig** als passendes Attribut verwendet, *das dem eindeutigen Attribut in AD zugeordnet wird.* Im Szenario „Workday-App“ wird z. B. das *Workday*-Attribut *WorkerID* dem AD-Attribut *employeeID* zugeordnet.

Sie können mehrere übereinstimmende Attribute festlegen und geeignete Prioritäten zuweisen. Sie werden nach „Rangfolge für Abgleich“ bewertet. Sobald eine Übereinstimmung gefunden wird, werden keine weiteren Attribute für den Abgleich mehr ausgewertet.

Sie können auch die [Standardattributzuordnungen anpassen](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes#understanding-attribute-mapping-types), z. B. bestehende Attributzuordnungen ändern oder löschen oder neue Attributzuordnungen entsprechend Ihren Geschäftsanforderungen erstellen. Eine Liste der benutzerdefinierten Attribute für die Zuordnung finden Sie im Tutorial der HR-Cloud-App (z. B. [Workday](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#planning-workday-to-active-directory-user-attribute-mapping-and-transformations)).

### <a name="determine-user-account-status"></a>Bestimmen des Benutzerkontostatus

Standardmäßig ordnet die Bereitstellungsconnector-App den **HR-Benutzerprofilstatus** dem **Benutzerkontostatus** in AD/Azure AD zu, um zu bestimmen, ob das Benutzerkonto aktiviert oder deaktiviert werden soll.

Wenn Sie den Prozess für Einstellungen/Kündigungen starten, erfassen Sie die folgenden Anforderungen:

| Prozess | Requirements (Anforderungen) |
| - | - |
| **Einstellungen** | Wie gehen Sie aus der Sicht des Identitätslebenszyklus mit Wiedereinstellungen um? Behalten Wiedereinstellungen ihre alte Mitarbeiter-ID? |
| | Werden von Ihnen zukunftsorientierte Einstellungen verarbeitet und AD-Konten für sie im Voraus erstellt? Werden diese Konten im aktivierten/deaktivierten Zustand erstellt? |
| | Wie gehen Sie aus der Sicht des Identitätslebenszyklus mit der Umwandlung von einem Mitarbeiter zu einem vorübergehend Beschäftigtem oder ähnlichem um? |
| | Behalten umgewandelte Benutzer ihr altes AD-Konto oder erhalten sie neue Konten? |
| **Kündigungen** | Werden Kündigungen von Mitarbeitern und vorübergehend Beschäftigten in AD unterschiedlich behandelt? |
| | Welche Stichtage werden für die Verarbeitung der Benutzerkündigung berücksichtigt? |
| | Wie wirken sich die Umwandlungen von Mitarbeitern und vorübergehend Beschäftigten auf bestehende AD-Konten aus? |
| | Wie bearbeiten Sie den Vorgang „Widerruf“ in AD? Widerrufvorgänge müssen behandelt werden, wenn im Rahmen des Einstellungsprozesses in der Zukunft datierte Einstellungen in AD erstellt werden. |

Abhängig von Ihren Anforderungen können Sie die Zuordnungslogik mithilfe von [Azure AD-Ausdrücken](https://docs.microsoft.com/azure/active-directory/manage-apps/functions-for-customizing-application-data) so anpassen, dass das AD-Konto basierend auf einer Kombination von Datenpunkten aktiviert oder deaktiviert wird.

### <a name="map-cloud-hr-app-to-ad-user-attributes"></a>Zuordnen der HR-Cloud-App zu AD-Benutzerattributen

Jede HR-Cloud-App wird mit standardmäßigen Zuordnungen von HR-Cloud-App zu AD ausgeliefert.

Wenn Sie den Prozess für Einstellungen/Wechsel/Kündigungen starten, erfassen Sie die folgenden Anforderungen:

| Prozess | Requirements (Anforderungen) |
| - | - |
| **Einstellungen** | Ist der AD-Kontoerstellungsprozess manuell, automatisiert oder teilweise automatisiert? |
| | Planen Sie, benutzerdefinierte Attribute von der HR-Cloud-App an AD zu verteilen? |
| **Wechsel** | Welche Attribute möchten Sie verarbeiten, wenn in der HR-Cloud-App ein „Wechsel“-Vorgang stattfindet? |
| | Führen Sie zum Zeitpunkt der Benutzeraktualisierung bestimmte Attributprüfungen durch? Wenn dies der Fall ist, geben Sie Details an. |
| **Kündigungen** | Werden Kündigungen von Mitarbeitern und vorübergehend Beschäftigten in AD unterschiedlich behandelt? |
| | Welche Stichtage werden für die Verarbeitung der Benutzerkündigung berücksichtigt? |
| | Wie wirken sich die Umwandlungen von Mitarbeitern und vorübergehend Beschäftigten auf bestehende AD-Konten aus? |

Je nach Ihren Anforderungen können Sie die Zuordnungen an Ihre Integrationsziele anpassen. Eine Liste der benutzerdefinierten Attribute für die Zuordnung finden Sie im jeweiligen Tutorial der HR-Cloud-App (z. B. [Workday](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#planning-workday-to-active-directory-user-attribute-mapping-and-transformations)).

### <a name="generate-unique-attribute-value"></a>Generieren von eindeutigen Attributwerten

Wenn Sie den Einstellungsprozess starten, müssen Sie möglicherweise eindeutige Attributwerte generieren, wenn Sie Attribute wie „CN“, „samAccountName“ und „UPN“ festlegen, die eindeutige Einschränkungen aufweisen.

Die Azure AD-Funktion [SelectUniqueValues](https://docs.microsoft.com/azure/active-directory/manage-apps/functions-for-customizing-application-data#selectuniquevalue) wertet jede Regel aus. Anschließend wird der Wert überprüft, der generiert wurde, um die Eindeutigkeit im Zielsystem sicherzustellen. Weitere Informationen finden Sie unter dem Beispiel [Generieren eines eindeutigen Werts für das Attribut „userPrincipalName (UPN)“](https://docs.microsoft.com/azure/active-directory/manage-apps/functions-for-customizing-application-data#generate-unique-value-for-userprincipalname-upn-attribute).

> [!NOTE]
> Diese Funktion wird derzeit nur für die „Benutzerbereitstellung von Workday in Active Directory“ unterstützt. Sie kann nicht für andere Bereitstellungs-Apps verwendet werden.

### <a name="configure-ad-ou-container-assignment"></a>Konfigurieren der Containerzuweisung für Organisationseinheiten in AD

Es ist eine häufige Anforderung, AD-Benutzerkonten basierend auf Geschäftseinheiten, Standorten und Abteilungen in Containern zu platzieren. Wenn Sie einen Wechselprozess initiieren und eine Änderung der Aufsichtsorganisation vorhanden ist, müssen Sie den Benutzer möglicherweise in AD von einer Organisationseinheit in eine andere verschieben.

Verwenden Sie die Funktion [Switch()](https://docs.microsoft.com/azure/active-directory/manage-apps/functions-for-customizing-application-data#switch), um die Geschäftslogik für die Zuordnung der Organisationseinheit zu konfigurieren und sie dem AD-Attribut *parentDistinguishedName* zuzuordnen.

Wenn Sie z. B. Benutzer in der Organisationseinheit auf der Grundlage des Personalabteilungsattributs „Municipality“ (Ort) erstellen möchten, dann können Sie den folgenden Ausdruck verwenden.

`
Switch([Municipality], "OU=Default,OU=Users,DC=contoso,DC=com", "Dallas", "OU=Dallas,OU=Users,DC=contoso,DC=com", "Austin", "OU=Austin,OU=Users,DC=contoso,DC=com", "Seattle", "OU=Seattle,OU=Users,DC=contoso,DC=com", "London", "OU=London,OU=Users,DC=contoso,DC=com")
`

Wenn der Wert für „Municipality“ Dallas, Austin, Seattle oder London entspricht, wird das Benutzerkonto in der entsprechenden Organisationseinheit erstellt. Wenn es keine Übereinstimmung gibt, wird das Konto in der Standardorganisationseinheit erstellt.

## <a name="plan-for-password-delivery-of-new-user-accounts"></a>Planen Sie die Zustellung von Kennwörtern für neue Benutzerkonten.

Wenn Sie den Einstellungsprozess starten, müssen Sie ein temporäres Kennwort für neue Benutzerkonten festlegen und zustellen. Mit der Benutzerbereitstellung von der HR-Cloud zu Azure AD können Sie den Rollout der SSPR-Funktion ([Self-Service-Kennwortzurücksetzung](https://docs.microsoft.com/azure/active-directory/authentication/quickstart-sspr)) von Azure AD für den Benutzer am ersten Tag durchführen.

SSPR ist für IT-Administratoren eine einfache Möglichkeit, Benutzern das Zurücksetzen ihrer Kennwörter oder das Entsperren ihrer Konten zu ermöglichen. Sie können das Attribut für die **Mobiltelefonnummer** von der HR-Cloud-App in AD bereitstellen und es mit Azure AD synchronisieren. Sobald das Attribut für die **Mobiltelefonnummer** in Azure AD vorhanden ist, können Sie SSPR für das Konto des Benutzers aktivieren, sodass die neuen Benutzer am ersten Tag die registrierte und verifizierte Mobilfunknummer zur Authentifizierung verwenden können.

## <a name="plan-for-initial-cycle"></a>Planen des erster Zyklus

Bei der ersten Ausführung des Azure AD-Bereitstellungsdiensts führt dieser einen [ersten Zyklus](https://docs.microsoft.com/azure/active-directory/manage-apps/how-provisioning-works#initial-cycle) für die HR-Cloud-App durch, um eine Momentaufnahme aller Benutzerobjekte in der HR-Cloud-App zu erstellen. Die für die ersten Zyklen erforderliche Zeit ist direkt davon abhängig, wie viele Benutzer im Quellsystem vorhanden sind. Der erste Zyklus für einige HR-Cloud-Apps mit über 100.000 Benutzern kann sehr lange dauern.

**Für große HR-Cloud-Apps (> 30.000 Benutzer)** empfehlen wir, den ersten Zyklus schrittweise durchzuführen und die inkrementellen Updates erst dann zu starten, wenn überprüft wurde, ob die richtigen Attribute in AD für verschiedene Benutzerbereitstellungsszenarien festgelegt sind. Befolgen Sie die nachfolgende Reihenfolge:

1. Führen Sie den ersten Zyklus nur für eine begrenzte Anzahl von Benutzern durch, indem Sie den [Bereichsfilter](#plan-scoping-filters-and-attribute-mapping) festlegen.
2. Überprüfen Sie die AD-Kontobereitstellung und die für die Benutzer festgelegten Attributwerte, die für die erste Ausführung ausgewählt wurden. Wenn das Ergebnis Ihren Erwartungen entspricht, erweitern Sie den Bereichsfilter, um schrittweise weitere Benutzer einzubeziehen, und überprüfen Sie dann die Ergebnisse für die zweite Ausführung.

Nachdem Sie mit den Ergebnissen des ersten Zyklus für Testbenutzer zufrieden sind, können Sie die [inkrementellen Updates](https://docs.microsoft.com/azure/active-directory/manage-apps/how-provisioning-works#incremental-cycles) starten.

## <a name="plan-testing-and-security"></a>Planen von Tests und der Sicherheit

Stellen Sie in jeder Phase der Bereitstellung (vom ersten Pilotprojekt bis zur Aktivierung der Benutzerbereitstellung) sicher, dass Sie testen, ob die Ergebnisse erwartungsgemäß ausfallen, und überwachen Sie die Bereitstellungszyklen.

### <a name="plan-testing"></a>Planen von Tests

Sobald Sie die HR-Cloud-App für die Azure AD-Benutzerbereitstellung konfiguriert haben, müssen Sie Testfälle ausführen, um zu überprüfen, ob diese Lösung den Anforderungen Ihres Unternehmens entspricht.

|Szenarien|Erwartete Ergebnisse|
|:-|:-|
|Einstellung neuer Mitarbeiter in der HR-Cloud-App| - Das Benutzerkonto wird im AD bereitgestellt.</br>- Benutzer können sich bei AD-Domänen-Apps anmelden und die gewünschten Aktionen durchführen.</br>- Wenn die AAD Connect-Synchronisierung konfiguriert ist, wird das Benutzerkonto auch in Azure AD erstellt.
|Der Benutzer wird in der HR-Cloud-App beendet|- Das Benutzerkonto ist in AD deaktiviert.</br>- Der Benutzer kann sich nicht bei den durch AD geschützten Unternehmens-Apps anmelden.
|Die Benutzeraufsichtsorganisation wird in der HR-Cloud-App aktualisiert|Basierend auf der Attributzuordnung wechselt das Benutzerkonto in AD von einer Organisationseinheit zu einer anderen.|
|HR aktualisiert den Vorgesetzten des Benutzers in der HR-Cloud-App|Das Feld für den Vorgesetzten in AD wird aktualisiert, um den Namen des neuen Vorgesetzten wiederzugeben.|
|Die Personalabteilung stellt einen Mitarbeiter in einer neuen Position ein.|Das Verhalten hängt davon ab, wie die HR-Cloud-App konfiguriert ist, um Mitarbeiter-IDs zu generieren:</br>- Wenn die alte Mitarbeiter-ID für Wiedereinstellungen wiederverwendet wird, dann aktiviert der Connector das bestehende AD-Konto für den Benutzer.</br>- Wenn Wiedereinstellungen eine neue Mitarbeiter-ID erhalten, dann erstellt der Connector ein neues AD-Konto für den Benutzer.|
|Die Personalabteilung wandelt den Mitarbeiter in einen vorübergehend Beschäftigten um oder umgekehrt|Es wird ein neues AD-Konto für die neue Persona erstellt und das alte Konto wird mit Inkrafttreten der Umwandlung deaktiviert.|

Verwenden Sie die obigen Ergebnisse, um zu bestimmen, wie Sie Ihre automatische Implementierung der Benutzerbereitstellung in der Produktionsumgebung basierend auf Ihren festgelegten Zeitrahmen umsetzen können.

> [!TIP]
> Es wird empfohlen, Verfahren wie Datenreduzierung und Datenbereinigung zu verwenden, wenn die Testumgebung mit Produktionsdaten aktualisiert wird, um vertrauliche Daten mit personenbezogenen Informationen zu entfernen/maskieren und so die Datenschutz- und Sicherheitsstandards einzuhalten.

### <a name="plan-security"></a>Planen der Sicherheit

Es ist üblich, dass im Rahmen der Bereitstellung eines neuen Diensts eine Sicherheitsüberprüfung erforderlich ist. Wenn eine Sicherheitsüberprüfung erforderlich ist oder noch nicht ausgeführt wurde, überprüfen Sie die vielen Azure AD-[Whitepaper](https://www.microsoft.com/download/details.aspx?id=36391), die eine Übersicht über die Identität als Dienst bereitstellen.

### <a name="plan-rollback"></a>Planen eines Rollbacks

Wenn die Implementierung der HR-Cloudbenutzerbereitstellung in der Produktionsumgebung nicht wie gewünscht funktioniert, können Sie die folgenden Rollbackschritte ausführen, um einen früheren bekannten fehlerfreien Zustand wiederherzustellen:

1. Überprüfen Sie den [Zusammenfassungsbericht für die Bereitstellung](check-status-user-account-provisioning.md#getting-provisioning-reports-from-the-azure-portal) und die [Bereitstellungsprotokolle](check-status-user-account-provisioning.md#provisioning-logs-preview) (siehe [Verwalten der HR-Cloud-App-Benutzerbereitstellung](#manage-your-configuration)), um zu ermitteln, welche fehlerhaften Vorgänge für die betroffenen Benutzer und/oder Gruppen durchgeführt wurden.
2. Der letzte bekannte fehlerfreie Zustand der betroffenen Benutzer und/oder Gruppen kann durch die Überwachungsprotokolle der Bereitstellung oder durch Überprüfen der Zielsysteme (Azure AD oder AD) ermittelt werden.
3. Arbeiten Sie mit dem App-Besitzer zusammen, um die betroffenen Benutzer und/oder Gruppen direkt in der App mit den letzten bekannten funktionierenden Statuswerten zu aktualisieren.

## <a name="deploy-the-cloud-hr-app"></a>Bereitstellen der HR-Cloud-App

Wählen Sie die HR-Cloud-App aus, die auf Ihre Lösungsanforderungen ausgerichtet ist.

**Workday** – [Tutorial: Konfigurieren von Workday für die automatische Benutzerbereitstellung](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#planning-your-deployment) – In diesem Tutorial werden die Schritte vorgestellt, die Sie zum Importieren von Workerprofilen aus Workday in AD und Azure AD ausführen müssen, wobei einige E-Mail-Adressen und Benutzernamen optional in Workday zurückgeschrieben werden.

## <a name="manage-your-configuration"></a>Verwalten Ihrer Konfiguration

Azure AD kann für Ihre Organisation zusätzliche Erkenntnisse zur Benutzerbereitstellung und betrieblichen Integrität liefern, indem Überwachungsprotokolle und Berichte verwendet werden.

### <a name="gain-insights-from-reports-and-logs"></a>Gewinnen von Erkenntnissen aus Berichten und Protokollen

Nach einem erfolgreichen [ersten Zyklus](https://docs.microsoft.com/azure/active-directory/manage-apps/how-provisioning-works#initial-cycle) führt der Azure AD-Bereitstellungsdienst so lange aufeinanderfolgende inkrementelle Aktualisierungen in Intervallen aus, die für die einzelnen Apps spezifisch sind, bis eines der folgenden Ereignisse eintritt:

- Der Dienst wird manuell beendet, und es wird über das [Azure-Portal](https://portal.azure.com/) oder mithilfe des entsprechenden [Microsoft Graph-API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview)-Befehls ein neuer erster Zyklus ausgelöst.
- Ein neuer erster Zyklus wird aufgrund einer Änderung in den Attributzuordnungen oder Bereichsfiltern ausgelöst.
- Der Bereitstellungsprozess wird aufgrund einer hohen Fehlerrate unter Quarantäne gestellt und bleibt mehr als vier Wochen lang in Quarantäne, wodurch er automatisch deaktiviert wird.

Wenn Sie diese Ereignisse und alle anderen vom Bereitstellungsdienst ausgeführten Aktivitäten überprüfen möchten, erfahren Sie, [wie Sie Protokolle überprüfen und Berichte zur Bereitstellungsaktivität abrufen können](https://docs.microsoft.com/azure/active-directory/manage-apps/check-status-user-account-provisioning).

#### <a name="azure-monitor-logs"></a>Azure Monitor-Protokolle

Alle vom Bereitstellungsdienst ausgeführten Aktivitäten werden in den **Azure AD-Überwachungsprotokollen** erfasst. Sie können Azure AD-Überwachungsprotokolle zur weiteren Analyse an Azure Monitor-Protokolle weiterleiten. **Mit Azure Monitor-Protokollen (auch als Log Analytics-Arbeitsbereich bezeichnet)** können Sie Daten abfragen, um Ereignisse zu finden, Trends zu analysieren und datenquellenübergreifende Korrelationen vorzunehmen. Sehen Sie sich dieses [Video](https://youtu.be/MP5IaCTwkQg) an, um die Vorteile der Verwendung von Azure Monitor-Protokollen für Azure AD-Protokolle in praktischen Benutzerszenarien kennenzulernen.

Sie können die [Log Analytics-Ansichten für Azure AD-Aktivitätsprotokolle](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-install-use-log-analytics-views) installieren, um Zugriff auf [vorgefertigte Berichte](https://github.com/AzureAD/Deployment-Plans/tree/master/Log%20Analytics%20Views) zu Bereitstellungsereignissen in Ihrer Umgebung zu erhalten.

Weitere Informationen finden Sie unter [Analysieren der Azure AD-Aktivitätsprotokolle mit ihren Azure Monitor-Protokollen](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-analyze-activity-logs-log-analytics).

### <a name="manage-personal-data"></a>Verwalten persönlicher Daten

Der auf dem Windows-Server installierte Azure AD Connect-Bereitstellungs-Agent erstellt Protokolle im Windows-Ereignisprotokoll, die abhängig von den Zuordnungen Ihrer HR-Cloud-App zu AD-Attributen persönliche Daten enthalten können. Um den Datenschutzverpflichtungen der Benutzer nachzukommen, können Sie eine von Windows geplante Aufgabe einrichten, um das Ereignisprotokoll zu löschen und sicherzustellen, dass keine Daten länger als 48 Stunden gespeichert werden.

Der Azure AD-Bereitstellungsdienst erstellt keine Berichte, führt keine Analysen aus und bietet keine Einblicke über 30 Tage hinaus. Aus diesem Grund erfolgt im Azure AD-Bereitstellungsdienst keine Speicherung, Verarbeitung oder Beibehaltung von Daten über 30 Tage hinaus. 

### <a name="troubleshoot"></a>Problembehandlung

Unter den folgenden Links finden Sie Informationen zum Beheben von Problemen, die bei der Bereitstellung auftreten können:

- [Problem beim Konfigurieren der Benutzerbereitstellung für eine Azure AD-Kataloganwendung](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-config-problem)
- [Synchronisieren eines Attributs aus lokalen Active Directory Domain Services mit Azure AD für die Bereitstellung einer Anwendung](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning-sync-attributes-for-mapping)
- [Die Benutzerbereitstellung für eine Azure AD-Kataloganwendung dauert Stunden oder länger](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish)
- [Problem saving administrator credentials while configuring user provisioning to an Azure Active Directory Gallery application](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-config-problem-storage-limit) (Probleme beim Speichern von Administratoranmeldeinformationen während des Konfigurierens der Benutzerbereitstellung in einer Anwendung aus dem Azure Active Directory-Katalog)
- [Es werden keine Benutzer für eine Azure AD-Kataloganwendung bereitgestellt](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-config-problem-no-users-provisioned)
- [Der falsche Satz von Benutzern wird für eine Azure AD-Kataloganwendung bereitgestellt](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-config-problem-wrong-users-provisioned)
- [Einrichten der Windows-Ereignisanzeige für die Problembehandlung bei Agents](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#setting-up-windows-event-viewer-for-agent-troubleshooting)
- [Einrichten von Azure-Portal-Überwachungsprotokollen für die Problembehandlung bei einem Dienst](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#setting-up-azure-portal-audit-logs-for-service-troubleshooting)
- [Grundlegendes zu Protokollen für Erstellungsvorgänge für ein AD-Benutzerkonto](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#understanding-logs-for-ad-user-account-create-operations)
- [Grundlegendes zu Protokollen für Aktualisierungsvorgänge für Manager](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#understanding-logs-for-manager-update-operations)
- [Beheben von häufig auftretenden Fehlern](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#resolving-commonly-encountered-errors)

### <a name="next-steps"></a>Nächste Schritte

- [Schreiben von Ausdrücken für Attributzuordnungen](https://docs.microsoft.com/azure/active-directory/manage-apps/functions-for-customizing-application-data)
- [Azure AD synchronization API overview](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview) (Azure AD-Synchronisierung – API-Übersicht)
- [Überspringen des Löschens von Benutzerkonten außerhalb des gültigen Bereichs](https://docs.microsoft.com/azure/active-directory/manage-apps/skip-out-of-scope-deletions)
- [Azure AD Connect-Bereitstellungs-Agent: Verlauf der Versionsveröffentlichungen](https://docs.microsoft.com/azure/active-directory/manage-apps/provisioning-agent-release-version-history).
