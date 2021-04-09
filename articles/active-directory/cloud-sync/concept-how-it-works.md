---
title: Ausführliche Informationen zur Funktionsweise der Azure AD Connect-Cloudsynchronisierung
description: Dieses Thema enthält ausführliche Informationen zur Funktionsweise der Cloudsynchronisierung.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/05/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0e1079ab4a523fbbc99c1e9190aef960b3f0723f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98612686"
---
# <a name="cloud-sync-deep-dive---how-it-works"></a>Ausführliche Informationen zur Funktionsweise der Cloudsynchronisierung

## <a name="overview-of-components"></a>Übersicht der Komponenten

![Funktionsweise](media/concept-how-it-works/how-1.png)

Die Cloudsynchronisierung basiert auf den Azure AD-Diensten und umfasst zwei wichtige Komponenten:

- **Bereitstellungs-Agent**: Der Azure AD Connect-Cloudbereitstellungs-Agent ist derselbe Agent wie bei der eingehenden Workday-Bereitstellung und basiert auf der gleichen serverseitigen Technologie wie die App-Proxy- und Passthrough-Authentifizierung. Dafür ist nur eine ausgehende Verbindung erforderlich, und die Agents werden automatisch aktualisiert. 
- **Bereitstellungsdienst**: Derselbe Bereitstellungsdienst wie bei der ausgehenden Bereitstellung und bei der eingehenden Workday-Bereitstellung, bei der ein planungsbasiertes Modell verwendet wird. Bei der Cloudsynchronisierung werden die Änderungen alle zwei Minuten bereitgestellt.


## <a name="initial-setup"></a>Ersteinrichtung
Während der Ersteinrichtung werden ein paar Aufgaben ausgeführt, um die Cloudsynchronisierung zu ermöglichen.  Dies sind: 

- **Bei der Agent-Installation**: Konfigurieren Sie den Agent für die AD-Domänen, die Sie bereitstellen möchten.  Diese Konfiguration registriert die Domänen im Hybrididentitätsdienst und stellt eine ausgehende Verbindung mit der Service Bus-Instanz her, die auf Anforderungen lauscht.
- **Beim Aktivieren der Bereitstellung**: Wählen Sie die AD-Domäne aus, und aktivieren Sie die Bereitstellung, die alle zwei Minuten ausgeführt wird. Optional können Sie die Kennworthashsynchronisierung deaktivieren und Benachrichtigungs-E-Mails definieren. Sie können auch die Attributtransformation mithilfe von Microsoft Graph-APIs verwalten.


## <a name="agent-installation"></a>Installation des Agenten
Im Folgenden werden die Vorgänge bei der Installation des Cloudbereitstellungs-Agents erläutert.

- Zuerst installiert das Installationsprogramm die Agent-Binärdateien und den Agent-Dienst, der unter dem virtuellen Dienstkonto (NETWORK SERVICE\AADProvisioningAgent) ausgeführt wird.  Ein virtuelles Dienstkonto ist ein besonderer Kontotyp, der nicht über ein Kennwort verfügt und von Windows verwaltet wird.
- Dann startet das Installationsprogramm den Assistenten.
- Der Assistent fordert Sie zur Eingabe der Azure AD-Anmeldeinformationen auf, führt dann eine Authentifizierung durch und ruft ein Token ab.
- Anschließend fordert Sie der Assistent zur Eingabe der Anmeldeinformationen des Domänenadministrators für den aktuellen Computer auf.
- Mit diesen Anmeldeinformationen wird das allgemeine verwaltete Agent-Dienstkonto (General Managed Service Account, GMSA) für diese Domäne entweder erstellt oder gefunden und wiederverwendet, wenn es bereits vorhanden ist.
- Der Agent-Dienst wird jetzt für die Ausführung unter dem GMSA neu konfiguriert.
- Der Assistent fordert Sie jetzt zur Domänenkonfiguration zusammen mit dem Unternehmensadministrator-/Domänenadministratorkonto für jede Domäne auf, die der Agent bedienen soll.
- Das GMSA-Konto wird dann mit Berechtigungen aktualisiert, die den Zugriff auf jede zuvor eingegebene Domäne ermöglichen.
- Als Nächstes löst der Assistent die Agent-Registrierung aus.
- Der Agent erstellt ein Zertifikat und registriert sich und das Zertifikat mithilfe des Azure AD-Tokens beim Registrierungsdienst des Hybrididentitätsdiensts (Hybrid Identity Service, HIS).
- Der Assistent löst einen AgentResourceGrouping-Aufruf aus. Dieser Aufruf an den HIS-Administrationsdienst dient dazu, den Agent einer oder mehreren AD-Domänen in der HIS-Konfiguration zuzuweisen.
- Der Assistent startet jetzt den Agent-Dienst neu.
- Der Agent ruft beim Neustart (und danach alle 10 Minuten) einen Bootstrap-Dienst auf, um nach Konfigurationsaktualisierungen zu suchen.  Der Bootstrap-Dienst überprüft die Identität des Agents.  Außerdem aktualisiert er die letzte Bootstrap-Zeit.  Dies ist wichtig, weil Agents keine aktualisierten Service Bus-Endpunkte abrufen und möglicherweise keine Anforderungen empfangen können, wenn sie keinen Bootstrap-Vorgang ausführen. 


## <a name="what-is-system-for-cross-domain-identity-management-scim"></a>Was ist das System für domänenübergreifendes Identitätsmanagement (SCIM)?

Die [SCIM-Spezifikation](https://tools.ietf.org/html/draft-scim-core-schema-01) ist ein Standard, der verwendet wird, um den Austausch von Benutzer- oder Gruppenidentitätsinformationen zwischen Identitätsdomänen wie Azure AD zu automatisieren. SCIM wird immer mehr zum De-facto-Standard für die Bereitstellung. Bei Verwendung zusammen mit Verbundstandards wie SAML oder OpenID Connect verfügen Administratoren über eine auf Standards basierende End-to-End-Lösung für die Zugriffsverwaltung.

Der Azure AD Connect-Cloudbereitstellungs-Agent verwendet SCIM mit Azure AD zum Bereitstellen und zum Aufheben der Bereitstellung von Benutzern und Gruppen.

## <a name="synchronization-flow"></a>Synchronisierungsflow
![Bereitstellung](media/concept-how-it-works/provisioning-4.png) Nachdem Sie den Agent installiert und die Bereitstellung aktiviert haben, erfolgt der folgende Flow.

1.  Nach der Konfiguration ruft der Azure AD-Bereitstellungsdienst den Azure AD-Hybriddienst auf, um dem Service Bus eine Anforderung hinzuzufügen. Der Agent hält ständig eine ausgehende Verbindung mit dem Service Bus aufrecht, der auf Anforderungen lauscht, und ruft sofort die Anforderung des Systems für die domänenübergreifende Identitätsverwaltung (System for Cross-Domain Identity Management, SCIM) ab. 
2.  Der Agent teilt die Anforderung basierend auf dem Objekttyp in separate Abfragen auf. 
3.  AD gibt das Ergebnis an den Agent zurück, und der Agent filtert diese Daten, bevor sie an Azure AD gesendet werden.  
4.  Der Agent gibt die SCIM-Antwort an Azure AD zurück.  Diese Antworten basieren auf der im Agent erfolgten Filterung.  Der Agent filtert die Ergebnisse mithilfe von Bereichsdefinitionen. 
5.  Der Bereitstellungsdienst schreibt die Änderungen in Azure AD.
6. Wenn es sich um eine Deltasynchronisierung und nicht um eine vollständige Synchronisierung handelt, wird ein Cookie/Wasserzeichen verwendet. Neue Abfragen erhalten die Änderungen ab diesem Cookie/Wasserzeichen.

## <a name="supported-scenarios"></a>Unterstützte Szenarien:
Für die Cloudsynchronisierung werden die folgenden Szenarien unterstützt.


- **Bestehender Hybridkunde mit einer neuen Gesamtstruktur**: Für primäre Gesamtstrukturen wird die Azure AD Connect-Synchronisierung verwendet. Für die Bereitstellung aus AD-Gesamtstrukturen (einschließlich getrennter AD-Gesamtstrukturen) wird die Cloudsynchronisierung verwendet. Weitere Informationen finden Sie in [diesem Tutorial](tutorial-existing-forest.md).

 ![Vorhandener Hybrid](media/tutorial-existing-forest/existing-forest-new-forest-2.png)
- **Neuer Hybridkunde**:      Die Azure AD Connect-Synchronisierung wird nicht verwendet. Für die Bereitstellung aus einer AD-Gesamtstruktur wird die Cloudsynchronisierung verwendet.  Weitere Informationen finden Sie in [diesem Tutorial](tutorial-single-forest.md).
 
 ![Neue Kunden](media/tutorial-single-forest/diagram-2.png)

- **Bestehender Hybridkunde**: Für primäre Gesamtstrukturen wird die Azure AD Connect-Synchronisierung verwendet. Für eine kleine Gruppe von Benutzern in den primären Gesamtstrukturen wird [hier](tutorial-existing-forest.md) eine Pilotcloudsynchronisierung durchgeführt.

 ![Pilot für vorhandene Gesamtstruktur](media/tutorial-migrate-aadc-aadccp/diagram-2.png)

Weitere Informationen finden Sie unter [Unterstützte Topologien](plan-cloud-sync-topologies.md).



## <a name="next-steps"></a>Nächste Schritte 

- [Was ist die Identitätsbereitstellung?](what-is-provisioning.md)
- [Worum handelt es sich bei der Azure AD Connect-Cloudsynchronisierung?](what-is-cloud-sync.md)
