---
title: Bedarfsorientierte Bereitstellung von Benutzern in Azure Active Directory
description: Synchronisierung erzwingen
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.topic: how-to
ms.date: 06/23/2020
ms.author: mimart
ms.reviewer: arvinh
ms.openlocfilehash: 78a56b6a848139c47d7934a47decb126afe00b7a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85297527"
---
# <a name="on-demand-provisioning"></a>Bedarfsorientierte Bereitstellung
Mit der bedarfsorientierten Bereitstellung können Sie einen Benutzer innerhalb von Sekunden in einer Anwendung bereitstellen. Mit der Funktion können Sie schnell Konfigurationsprobleme beheben, von Ihnen definierte Ausdrücke überprüfen, Gültigkeitsbereichsfilter testen und vieles mehr. 

## <a name="how-to-use-on-demand-provisioning"></a>Verwenden der bedarfsorientierten Bereitstellung 

1. Melden Sie sich beim **Azure-Portal** an.
2. Navigieren Sie zu **Unternehmensanwendungen**.
3. Wählen Sie Ihre Anwendung aus, und navigieren Sie zur Konfigurationsseite für die Bereitstellung.
4. Konfigurieren Sie die Bereitstellung, indem Sie Ihre Administratoranmeldeinformationen angeben.
5. Klicken Sie auf **Bedarfsorientierte Bereitstellung**.
6. Suchen Sie anhand von Vorname, Nachname, Anzeigename oder E-Mail-Adresse nach einem Benutzer.
7. Wählen Sie unten auf der Seite „Bereitstellen“ aus.

## <a name="understanding-the-provisioning-steps"></a>Grundlegendes zu den Bereitstellungsschritten
Die Funktion für die bedarfsorientierte Bereitstellung versucht, die Schritte anzuzeigen, die der Bereitstellungsdienst beim Bereitstellen eines Benutzers ausführt. Die Bereitstellung eines Benutzers besteht in der Regel aus fünf Schritten, und ein Schritt oder auch mehrere Schritte werden in der Umgebung für die bedarfsorientierte Bereitstellung angezeigt.

### <a name="step-1-test-connection"></a>Schritt 1: Verbindung testen
Der Bereitstellungsdienst versucht, den Zugriff auf die Zielanwendung zu autorisieren, indem er eine Anforderung für einen „Testbenutzer“ ausgibt. Der Bereitstellungsdienst erwartet eine Antwort, aus der hervorgeht, dass er autorisiert ist, die Bereitstellungsschritte fortzusetzen. Dieser Schritt wird nur angezeigt, wenn in diesem Schritt ein Fehler auftritt. Wenn der Schritt erfolgreich ist, wird er in der Umgebung für die bedarfsorientierte Bereitstellung nicht angezeigt. 

**Tipps zur Problembehandlung**
* Stellen Sie sicher, dass Sie gültige Anmeldeinformationen (z. B. geheimes Token und Mandanten-URL) für die Zielanwendung bereitgestellt haben. Welche Anmeldeinformationen erforderlich sind, hängt von der jeweiligen Anwendung ab. Ausführliche Tutorials zur Konfiguration finden Sie [hier](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list). 
* Stellen Sie sicher, dass die Zielanwendung das Filtern anhand von übereinstimmenden Attributen (definiert auf der Attributzuordnungsseite) unterstützt. Unter Umständen müssen Sie in der vom Anwendungsentwickler bereitgestellten API-Dokumentation nachschlagen, um zu erfahren, welche Filter unterstützt werden.  
* Bei SCIM-Anwendungen können Sie mit einem Tool (z. B. Postman) sicherstellen, dass die Anwendung die Autorisierungsanforderungen entsprechend den Erwartungen des Azure AD-Bereitstellungsdiensts beantwortet. Eine Beispielanforderung finden Sie [hier](https://docs.microsoft.com/azure/active-directory/app-provisioning/use-scim-to-provision-users-and-groups#request-3).

### <a name="step-2-import-user"></a>Schritt 2: Benutzer importieren
Als Nächstes ruft der Bereitstellungsdienst den Benutzer aus dem Quellsystem ab. Anhand der vom Dienst abgerufenen Benutzerattributen wird später evaluiert, ob sich der Benutzer im Gültigkeitsbereich für die Bereitstellung befindet. Außerdem wird das Zielsystem auf einen vorhandenen Benutzer überprüft, und es wird bestimmt, welche Benutzerattribute in das Zielsystem exportiert werden sollen. 

**Details anzeigen**

Im Abschnitt „Details anzeigen“ werden die Eigenschaften des Benutzers angezeigt, die aus dem Quellsystem (z. B. Azure AD) importiert wurden.

**Tipps zur Problembehandlung**
* Beim Importieren des Benutzers kann ein Fehler auftreten, wenn im Benutzerobjekt im Zielsystem das übereinstimmende Attribut fehlt. Sie können diesen Fehler beheben, indem Sie das Benutzerobjekt mit dem Wert für das übereinstimmende Attribut aktualisieren oder das übereinstimmende Attribut in Ihrer Bereitstellungskonfiguration ändern.  
* Wenn ein von Ihnen erwartetes Attribut in der importierten Liste fehlt, vergewissern Sie sich, dass das Attribut im Benutzerobjekt im Zielsystem mit einem Wert versehen ist. Der Bereitstellungsdienst unterstützt derzeit keine Attribute mit Nullwert. 
* Stellen Sie sicher, dass die Attributzuordnungsseite der Bereitstellungskonfiguration das erwartete Attribut enthält. 

### <a name="step-3-determine-if-user-is-in-scope"></a>Schritt 3: Gültigkeitsbereich des Benutzers ermitteln
Als Nächstes ermittelt der Bereitstellungsdienst, ob sich der Benutzer im [Gültigkeitsbereich](https://docs.microsoft.com/azure/active-directory/app-provisioning/how-provisioning-works#scoping) für die Bereitstellung befindet. Der Dienst überprüft, ob der Benutzer der Anwendung zugewiesen und der Gültigkeitsbereich auf „Nur zugewiesene Benutzer und Gruppen synchronisieren“ oder „Alle synchronisieren“ festgelegt ist. Außerdem berücksichtigt er die Gültigkeitsbereichsfilter, die in Ihrer Bereitstellungskonfiguration definiert sind.  

**Details anzeigen**

Im Abschnitt „Details anzeigen“ werden die ausgewerteten Bedingungen für den Gültigkeitsbereich angezeigt. Mindestens eine der folgenden Eigenschaften wird angezeigt:
* **Aktiv im Quellsystem**: Für den Benutzer wurde in Azure AD die Eigenschaft „Ist aktiv“ auf „True“ eingestellt.
* **Der Anwendung zugewiesen**: Der Benutzer ist in Azure AD der Anwendung zugewiesen.
* **Gültigkeitsbereich „Alle synchronisieren“** : Diese Einstellung für den Gültigkeitsbereich lässt alle Benutzer und Gruppen im Mandanten zu.
* **Benutzer hat erforderliche Rolle**: Der Benutzer verfügt über die Rollen, die für eine Bereitstellung in der Anwendung erforderlich sind. 
* **Gültigkeitsbereichsfilter**: Diese Eigenschaft wird ebenfalls angezeigt, wenn Sie Gültigkeitsbereichsfilter für Ihre Anwendung definiert haben. Der Filter wird in folgendem Format angezeigt: {Titel des Gültigkeitsbereichsfilters} {Attribut des Gültigkeitsbereichsfilters} {Operator des Gültigkeitsbereichsfilters} {Wert des Gültigkeitsbereichsfilters}. 

**Tipps zur Problembehandlung**
* Stellen Sie sicher, dass Sie eine für den Gültigkeitsbereich gültige Rolle definiert haben. Vermeiden Sie beispielsweise den Operator [Größer als](https://docs.microsoft.com/azure/active-directory/app-provisioning/define-conditional-rules-for-provisioning-user-accounts#create-a-scoping-filter) mit einem nicht ganzzahligen Wert. 
* Wenn der Benutzer nicht über die notwendige Rolle verfügt, überprüfen Sie die [hier](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-config-problem-no-users-provisioned#provisioning-users-assigned-to-the-default-access-role) beschriebenen Tipps. 

### <a name="step-4-match-user-between-source-and-target"></a>Schritt 4: Benutzer zwischen Quell- und Zielsystem abgleichen
In diesem Schritt versucht der Dienst, den im Importschritt abgerufenen Benutzer mit einem Benutzer im Zielsystem abzugleichen. 

**Details anzeigen**

Die Seite „Details anzeigen“ enthält die Eigenschaften des Benutzers oder mehrerer Benutzer, der bzw. die mit dem Zielsystem abgeglichen wurde(n). Die im Kontextbereich angezeigten Eigenschaften variieren wie folgt:
* Wenn es im Zielsystem keine übereinstimmenden Benutzer gibt, werden keine Eigenschaften angezeigt.
* Wenn es einen übereinstimmenden Benutzer im Zielsystem gibt, werden die Eigenschaften des abgeglichenen Benutzers im Zielsystem angezeigt.
* Wenn es mehrere übereinstimmende Benutzer gibt, werden die Eigenschaften dieser abgeglichenen Benutzer angezeigt.
* Wenn Ihre Attributzuordnungen mehrere übereinstimmende Attribute enthalten, werden die einzelnen übereinstimmenden Attribute nacheinander ausgewertet, und die abgeglichenen Benutzer werden angezeigt. 

**Details zur Problembehandlung**
* Der Bereitstellungsdienst kann einen Benutzer im Quellsystem nicht eindeutig mit einem Benutzer im Zielsystem abgleichen. Dieses Problem kann gelöst werden, indem Sie sicherstellen, dass das übereinstimmende Attribut eindeutig ist. 
* Stellen Sie sicher, dass die Zielanwendung das Filtern anhand des als übereinstimmend definierten Attributs unterstützt.  

### <a name="step-5-perform-action"></a>Schritt 5: Aktion ausführen
Schließlich führt der Bereitstellungsdienst eine Aktion (z. B. Erstellen, Aktualisieren, Löschen oder Überspringen des Benutzers) aus. 

**Details anzeigen**

Im Abschnitt „Details anzeigen“ werden die Attribute angezeigt, die in der Zielanwendung geändert wurden. Diese Anzeige beinhaltet das endgültige Ergebnis der Aktivität des Bereitstellungsdiensts und die exportierten Attribute. Wenn bei diesem Schritt ein Fehler auftritt, handelt es sich bei den angezeigten Attributen um die Attribute, die der Bereitstellungsdienst zu ändern versucht hat.  

**Tipps zur Problembehandlung**
* Die Fehler, die beim Exportieren von Änderungen auftreten, können sehr unterschiedlich sein. In der [Dokumentation](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs#error-codes) zu den Bereitstellungsprotokollen sind häufige Fehler beschrieben.


## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen
**Muss man die Bereitstellung deaktivieren, um die bedarfsorientierte Bereitstellung zu verwenden?** Bei Anwendungen, die ein langlebiges Bearertoken oder die Kombination aus Benutzername und Kennwort für die Autorisierung verwenden, sind keine weiteren Schritte erforderlich. Bei Anwendungen, die OAuth für die Autorisierung verwenden, muss derzeit der Bereitstellungsauftrag angehalten werden, bevor die bedarfsorientierte Bereitstellung verwendet werden kann. Anwendungen wie G Suite, Box, Workplace by Facebook und Slack gehören zu dieser Kategorie. Wir arbeiten daran, für alle Anwendungen die Ausführung der bedarfsorientierten Bereitstellung ohne Anhalten des Bereitstellungsauftrags zu ermöglichen. 

**Wie lange dauert die bedarfsorientierte Bereitstellung?** Sie dauert im Allgemeinen weniger als 30 Sekunden. 

## <a name="known-limitations"></a>Bekannte Einschränkungen
Aktuell gibt es ein paar bekannte Einschränkungen. Verfassen Sie einen Beitrag auf [UserVoice](https://aka.ms/appprovisioningfeaturerequest), damit wir die als Nächstes anstehenden Optimierungen besser priorisieren können. Beachten Sie, dass sich diese Einschränkungen nur auf die Funktion für die bedarfsorientierte Bereitstellung beziehen. Einzelheiten zur Unterstützung von Bereitstellungsgruppen, Löschvorgängen usw. durch eine Anwendung finden Sie im Tutorial zur jeweiligen Anwendung. 

* Die Anwendungen Workday, AWS und SuccessFactors unterstützen die bedarfsorientierte Bereitstellung nicht.
* Die bedarfsorientierte Bereitstellung von Gruppen und Rollen wird nicht unterstützt.
* Das Deaktivieren oder Löschen von Benutzern und Gruppen wird nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

* [Behandeln von Bereitstellungsproblemen](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-config-problem)
