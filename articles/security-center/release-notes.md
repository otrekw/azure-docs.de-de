---
title: Versionshinweise für Azure Security Center
description: Enthält eine Beschreibung der Neuerungen und Änderungen in Azure Security Center.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/21/2020
ms.author: memildin
ms.openlocfilehash: 3fa9de1057160340fdf10d72809a104cae985162
ms.sourcegitcommit: 8017209cc9d8a825cc404df852c8dc02f74d584b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2020
ms.locfileid: "84248157"
---
# <a name="whats-new-in-azure-security-center"></a>Neuerungen in Azure Security Center

Azure Security befindet sich in der aktiven Entwicklung und wird ständig verbessert. Damit Sie bezüglich der aktuellen Entwicklungen immer auf dem neuesten Stand sind, enthält diese Seite Informationen zu den folgenden Bereichen:

- Neue Funktionen
- Behebung von Programmfehlern
- Veraltete Funktionen

Es ist ratsam, diese Seite regelmäßig zu besuchen, da sie immer wieder aktualisiert wird. Elemente, die älter als sechs Monate sind, finden Sie im [Archiv zu den Neuerungen in Azure Security Center](release-notes-archive.md).


## <a name="may-2020"></a>Mai 2020


### <a name="alert-suppression-rules-preview"></a>Regeln zur Warnungsunterdrückung (Vorschau)

Dieses neue Feature (derzeit in der Vorschauphase) trägt zur Reduzierung der „Warnungsmüdigkeit“ bei. Verwenden Sie Regeln, damit Warnungen, die auf harmlose Fehler hinweisen oder sich auf normale Aktivitäten Ihrer Organisation beziehen, automatisch ausgeblendet werden. Auf diese Weise können Sie sich auf die relevantesten Bedrohungen konzentrieren. 

Warnungen, die den aktivierten Unterdrückungsregeln entsprechen, werden zwar weiterhin generiert, aber ihr Status lautet „Geschlossen“. Der Status wird im Azure-Portal bzw. an dem Ort angezeigt, über den Sie auf die Security Center-Sicherheitswarnungen zugreifen.

Mit Unterdrückungsregeln werden die Kriterien definiert, nach denen Warnungen automatisch geschlossen werden sollen. Normalerweise verwenden Sie in folgenden Fällen eine Unterdrückungsregel:

- Unterdrücken von Warnungen, die Sie als falsch positiv identifiziert haben

- Unterdrücken von Warnungen, die zu oft ausgelöst werden und daher nicht nützlich sind

Weitere Informationen finden Sie unter [Unterdrücken von Warnungen aus dem Threat Protection-Modul von Azure Security Center](alerts-suppression-rules.md).


### <a name="virtual-machine-vulnerability-assessment-is-now-generally-available"></a>Sicherheitsrisikobewertung für virtuelle Computer ist jetzt allgemein verfügbar

Der Standardtarif von Security Center enthält jetzt eine integrierte Sicherheitsrisikobewertung, für die keine zusätzlichen Gebühren anfallen. Diese Erweiterung basiert auf Qualys, aber die Ergebnisse werden direkt an Security Center gemeldet. Sie benötigen keine Qualys-Lizenz und auch kein Qualys-Konto – alles erfolgt nahtlos innerhalb von Security Center.

Mit der neuen Lösung können Ihre virtuellen Computer fortlaufend gescannt werden, um Sicherheitsrisiken zu ermitteln, und die Ergebnisse werden in Security Center bereitgestellt. 

Verwenden Sie zum Bereitstellen der Lösung die neue Sicherheitsempfehlung:

„Integrierte Lösung zur Sicherheitsrisikobewertung (unterstützt von Qualys) auf virtuellen Computern aktivieren“

Informieren Sie sich über die [integrierte Sicherheitsrisikobewertung für virtuelle Computer von Security Center](built-in-vulnerability-assessment.md).



### <a name="changes-to-just-in-time-jit-virtual-machine-vm-access"></a>Änderungen am JIT-VM-Zugriff (Just-In-Time)

Security Center enthält ein optionales Feature zum Schützen der Verwaltungsports Ihrer virtuellen Computer. Dies ist eine Verteidigungsmaßnahme gegen die häufigste Form von Brute-Force-Angriffen.

Mit diesem Update werden die folgenden Änderungen an diesem Feature vorgenommen:

- Die Empfehlung, in der Ihnen zur JIT-Aktivierung auf einer VM geraten wird, wurde umbenannt. „Just-In-Time-Netzwerkzugriffssteuerung auf virtuelle Computer anwenden“ wurde geändert in: „Verwaltungsports virtueller Computer müssen mit der Just-In-Time-Netzwerkzugriffssteuerung geschützt werden“.

- Für die Empfehlung wurde festgelegt, dass sie nur ausgelöst wird, wenn offene Verwaltungsports vorhanden sind.

Erfahren Sie mehr zum [Feature für JIT-Zugriff](security-center-just-in-time.md).


### <a name="custom-recommendations-have-been-moved-to-a-separate-security-control"></a>Benutzerdefinierte Empfehlungen wurden in ein separates Sicherheitssteuerelement verschoben

Eines der Sicherheitssteuerelemente, die im Rahmen der erweiterten Sicherheitsbewertung eingeführt wurden, ist „Bewährte Sicherheitsmethoden implementieren“. Alle benutzerdefinierten Empfehlungen, die für Ihre Abonnements erstellt wurden, wurden automatisch in dieses Steuerelement eingefügt. 

Um Ihnen die Suche nach Ihren benutzerdefinierten Empfehlungen zu erleichtern, haben wir diese in das dedizierte Sicherheitssteuerelement „Benutzerdefinierte Empfehlungen“ verschoben. Dieses Steuerelement wirkt sich nicht auf Ihre Sicherheitsbewertung aus.

Weitere Informationen zu Sicherheitssteuerelementen finden Sie unter [Erweiterte Sicherheitsbewertung (Vorschau) in Azure Security Center](secure-score-security-controls.md).


### <a name="toggle-added-to-view-recommendations-in-controls-or-as-a-flat-list"></a>Umschalter zum Anzeigen von Empfehlungen in Steuerelementen oder als flache Liste hinzugefügt

Bei Sicherheitssteuerelementen handelt es sich um logische Gruppen mit zusammengehörigen Sicherheitsempfehlungen. Diese spiegeln Ihre anfälligen Angriffsflächen wider. Eine Sicherheitskontrolle umfasst eine Reihe von Sicherheitsempfehlungen mit Anweisungen, mit denen Sie diese Empfehlungen implementieren können.

Um sofort zu sehen, wie gut jede einzelne Angriffsfläche in Ihrer Organisation geschützt ist, sehen Sie sich die Bewertungen für die einzelnen Sicherheitskontrollen an.

Ihre Empfehlungen werden standardmäßig in den Sicherheitssteuerelementen angezeigt. Nach diesem Update können Sie sie auch als Liste anzeigen. Verwenden Sie den neuen Umschalter, mit dem Sie „Nach Steuerelementen gruppieren“ können, um sie als einfache Liste anzuzeigen, die nach dem Integritätsstatus der betroffenen Ressourcen sortiert ist. Der Umschalter befindet sich oberhalb der Liste im Portal.

Die Sicherheitssteuerelemente – und dieser Umschalter – sind Teil der neuen Benutzeroberfläche für die Sicherheitsbewertung. Denken Sie daran, uns über das Portal Ihr Feedback hierzu zu senden.

Weitere Informationen zu Sicherheitssteuerelementen finden Sie unter [Erweiterte Sicherheitsbewertung (Vorschau) in Azure Security Center](secure-score-security-controls.md).


### <a name="expanded-security-control-implement-security-best-practices"></a>Erweitertes Sicherheitssteuerelement „Bewährte Sicherheitsmethoden implementieren“ 

Eines der Sicherheitssteuerelemente, die im Rahmen der erweiterten Sicherheitsbewertung eingeführt wurden, ist „Bewährte Sicherheitsmethoden implementieren“. Wenn eine Empfehlung in diesem Steuerelement angeordnet ist, wirkt sie sich nicht auf die Sicherheitsbewertung aus. 

Mit diesem Update wurden drei Empfehlungen aus den Steuerelementen, in denen sie ursprünglich angeordnet waren, in dieses Steuerelement für die bewährte Vorgehensweise verschoben. Wir haben dies durchgeführt, weil wir festgestellt haben, dass das Risiko bei diesen drei Empfehlungen niedriger als anfänglich gedacht ist.

Außerdem wurden zwei neue Empfehlungen eingeführt und diesem Steuerelement hinzugefügt.

Die folgenden drei Empfehlungen wurden verschoben:

- **Für Konten mit Leseberechtigungen für Ihr Abonnement muss MFA aktiviert sein** (ursprünglich im Steuerelement „MFA aktivieren“ enthalten)
- **Externe Konten mit Leseberechtigungen müssen aus Ihrem Abonnement entfernt werden** (ursprünglich im Steuerelement „Zugriff und Berechtigungen verwalten“ enthalten)
- **Für Ihr Abonnement dürfen maximal 3 Besitzer festgelegt werden** (ursprünglich im Steuerelement „Zugriff und Berechtigungen verwalten“ enthalten)

Die beiden folgenden neuen Empfehlungen wurden dem Steuerelement hinzugefügt:

- **[Vorschau] Der Gastkonfigurations-Agent sollte installiert sein**: [Guest Configuration von Azure Policy](https://docs.microsoft.com/azure/governance/policy/concepts/guest-configuration) ermöglicht auf virtuellen Computern einen Einblick in Server- und Anwendungseinstellungen (nur Windows).

- **[Vorschau] Windows Defender Exploit Guard sollte aktiviert sein**: Für Windows Defender Exploit Guard wird der Guest Configuration-Agent von Azure Policy genutzt. Exploit Guard verfügt über vier Komponenten für die Absicherung von Geräten gegen viele verschiedene Angriffsvektoren und Blockierungsverhalten, mit denen bei Angriffen mit Schadsoftware häufig zu rechnen ist. Darüber hinaus ermöglichen diese Komponenten es Unternehmen, die richtige Balance zwischen Sicherheitsrisiken und Produktivitätsanforderungen zu finden (nur Windows).

Weitere Informationen zu Windows Defender Exploit Guard finden Sie unter [Erstellen und Bereitstellen einer Exploit Guard-Richtlinie](https://docs.microsoft.com/mem/configmgr/protect/deploy-use/create-deploy-exploit-guard-policy).

Weitere Informationen zu Sicherheitssteuerelementen finden Sie unter [Erweiterte Sicherheitsbewertung (Vorschau) in Azure Security Center](secure-score-security-controls.md).



### <a name="custom-policies-with-custom-metadata-are-now-generally-available"></a>Benutzerdefinierte Richtlinien mit benutzerdefinierten Metadaten sind nun allgemein verfügbar

Benutzerdefinierte Richtlinien sind nun Bestandteil der Security Center-Oberfläche mit den Empfehlungen, der Sicherheitsbewertung und des Dashboards mit den Standards zur Einhaltung gesetzlicher Bestimmungen. Dieses Feature ist jetzt allgemein verfügbar und ermöglicht es Ihnen, die Abdeckung in Bezug auf die Sicherheitsbewertung Ihrer Organisation in Security Center zu erweitern. 

Erstellen Sie in Azure Policy eine benutzerdefinierte Initiative, und fügen Sie ihr Richtlinien hinzu. Führen Sie anschließend das Onboarding in Azure Security Center und die Visualisierung in Form von Empfehlungen durch.

Wir haben jetzt auch die Option zum Bearbeiten der benutzerdefinierten Metadaten für die Empfehlungen hinzugefügt. Zu den Metadatenoptionen zählen Schweregrad, Problembehandlungsschritte, Bedrohungsinformationen und mehr.  

Informieren Sie sich über das [Verbessern der benutzerdefinierten Empfehlungen mit ausführlichen Informationen](custom-security-policies.md#enhancing-your-custom-recommendations-with-detailed-information).



### <a name="crash-dump-analysis-capabilities-migrating-to-fileless-attack-detection"></a>Migration der Funktionen für die Absturzabbildanalyse zur Erkennung dateiloser Angriffe 

Wir integrieren die Erkennungsfunktionen der Windows-Absturzabbildanalyse mit der [Erkennung dateiloser Angriffe](https://docs.microsoft.com/azure/security-center/threat-protection#windows-fileless). Die Analyse zur Erkennung dateiloser Angriffe verfügt über verbesserte Versionen der folgenden Sicherheitswarnungen für Windows-Computer: „Erkennung einer Codeinjektion“, „Erkennung eines Windows-Maskerademoduls“, „Erkennung von Shellcode“ und „Verdächtiges Codesegment erkannt“.

Diese Umstellung hat u. a. folgende Vorteile:

- **Proaktive und rechtzeitige Erkennung von Schadsoftware**: Bei der Absturzabbildanalyse wurde gewartet, bis es zu einem Absturz gekommen ist, und erst dann wurde die Analyse durchgeführt, um bösartige Artefakte zu ermitteln. Bei der Erkennung von dateilosen Angriffen werden In-Memory-Bedrohungen proaktiv während der Ausführung identifiziert. 

- **Erweiterte Warnungen**: Die Sicherheitswarnungen bei der Erkennung von dateilosen Angriffen verfügen über erweiterte Elemente, die bei der Absturzabbildanalyse nicht vorhanden sind, z. B. Informationen zu aktiven Netzwerkverbindungen. 

- **Warnungsaggregation**: Wenn bei der Absturzabbildanalyse in einem Absturzabbild mehrere Angriffsmuster erkannt wurden, wurden mehrere Sicherheitswarnungen ausgelöst. Bei der Erkennung von dateilosen Angriffen werden alle identifizierten Angriffsmuster eines Prozesses in einer Warnung zusammengefasst, damit nicht mehrere Warnungen korreliert werden müssen.

- **Verringerung der Anforderungen in Ihrem Log Analytics-Arbeitsbereich**: Absturzabbilder mit potenziell vertraulichen Daten werden nicht mehr in Ihren Log Analytics-Arbeitsbereich hochgeladen.



## <a name="april-2020"></a>April 2020

### <a name="dynamic-compliance-packages-are-now-generally-available"></a>Dynamische Compliancepakete sind jetzt allgemein verfügbar

Das Azure Security Center-Dashboard für die Einhaltung gesetzlicher Bestimmungen enthält jetzt **dynamische Compliancepakete** (allgemein verfügbar), um weitere branchenbezogene und gesetzliche Standards nachverfolgen zu können.

Dynamische Compliancepakete können über die Security Center-Seite mit den Sicherheitsrichtlinien Ihrem Abonnement oder Ihrer Verwaltungsgruppe hinzugefügt werden. Nachdem Sie das Onboarding für einen Standard oder einen Benchmarkwert durchgeführt haben, wird der Standard in Ihrem Dashboard für die Einhaltung gesetzlicher Bestimmungen mit allen zugeordneten Compliancedaten in Form von Bewertungen angezeigt. Ein zusammenfassender Bericht für alle Standards, für die das Onboarding durchgeführt wurde, wird als Download bereitgestellt.

Sie können nun beispielsweise folgende Standards hinzufügen:

- **NIST SP 800-53 R4**
- **SWIFT CSP CSCF-v2020**
- **UK Official und UK NHS**
- **Canada Federal PBMM**
- **Azure CIS 1.1.0 (neu)** (eine vollständigere Darstellung von Azure CIS 1.1.0)

Zusätzlich haben wir vor Kurzem den **Azure-Sicherheitsvergleichstest** hinzugefügt. Dies sind von Microsoft erstellte Azure-spezifische Richtlinien zu den bewährten Methoden für Sicherheit und Compliance, die auf allgemeinen Compliance-Frameworks basieren. Weitere Standards werden im Dashboard unterstützt, sobald sie verfügbar sind.  
 
Erfahren Sie mehr zum [Aktualisieren auf dynamische Compliancepakete in Ihrem Dashboard für die Einhaltung gesetzlicher Bestimmungen](update-regulatory-compliance-packages.md).


### <a name="identity-recommendations-now-included-in-azure-security-center-free-tier"></a>Empfehlungen zur Identität sind in Azure Security Center jetzt im Free-Tarif enthalten

Sicherheitsempfehlungen zur Identität und zum Zugriff sind im Free-Tarif von Azure Security Center nun allgemein verfügbar. Dies ist Teil der Maßnahme, mit der die Features für die Verwaltung des Cloudsicherheitsstatus kostenlos zugänglich gemacht werden sollen. Bisher waren diese Empfehlungen nur im Standard-Tarif verfügbar.

Beispiele für Empfehlungen zur Identität und zum Zugriff sind:

- „Für Konten mit Besitzerberechtigungen für Ihr Abonnement muss MFA aktiviert sein“
- „Für Ihr Abonnement dürfen maximal 3 Besitzer festgelegt werden“
- „Veraltete Konten müssen aus Ihrem Abonnement entfernt werden“

Bei Abonnements mit Free-Tarif wirkt sich diese Änderung auf deren Sicherheitsbewertungen aus, weil die Sicherheit in Bezug auf die Identität und den Zugriff hierfür bisher noch nicht bewertet wurde.

Weitere Informationen finden Sie in den [Empfehlungen zur Identität und zum Zugriff](recommendations-reference.md#recs-identity).

Erfahren Sie mehr zum [Überwachen der Identität und des Zugriffs](security-center-identity-access.md).


## <a name="march-2020"></a>März 2020

### <a name="workflow-automation-is-now-generally-available"></a>Workflowautomatisierung ist nun allgemein verfügbar

Das Feature „Workflowautomatisierung“ von Azure Security Center ist jetzt allgemein verfügbar. Sie können es verwenden, um Logic Apps bei Sicherheitswarnungen und Empfehlungen automatisch auszulösen. Darüber hinaus sind manuelle Trigger für Warnungen und alle Empfehlungen verfügbar, für die die „Schnellkorrektur“ verfügbar ist.

Jedes Sicherheitsprogramm umfasst mehrere Workflows für die Reaktion auf Vorfälle. Diese Prozesse können das Benachrichtigen relevanter Stakeholder, das Starten eines Change Management-Prozesses und das Anwenden spezifischer Korrekturschritte umfassen. Sicherheitsexperten empfehlen, möglichst viele Schritte dieser Verfahren zu automatisieren. Durch die Automatisierung wird der Aufwand reduziert. Außerdem können Sie die Sicherheit erhöhen, indem Sie sicherstellen, dass die Prozessschritte schnell, einheitlich und gemäß Ihren vordefinierten Anforderungen ausgeführt werden.

Weitere Informationen zu den automatischen und manuellen Security Center-Funktionen zum Ausführen von Workflows finden Sie unter [Workflowautomatisierung](workflow-automation.md).

Erfahren Sie mehr zum [Erstellen von Logik-Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview).


### <a name="integration-of-azure-security-center-with-windows-admin-center"></a>Integration von Azure Security Center mit Windows Admin Center

Es ist jetzt möglich, Ihre lokalen Windows-Server aus Windows Admin Center direkt in Azure Security Center zu verschieben. Security Center wird somit zu Ihrer zentralen Benutzeroberfläche für die Anzeige von Sicherheitsinformationen für Ihre gesamten Windows Admin Center-Ressourcen, z. B. lokale Server, virtuelle Computer und weitere PaaS-Workloads.

Nach dem Verschieben eines Servers aus Windows Admin Center in Azure Security Center haben Sie folgende Möglichkeiten:

- Anzeigen von Sicherheitswarnungen und -empfehlungen in der Security Center-Erweiterung von Windows Admin Center
- Anzeigen des Sicherheitsstatus und Abrufen weiterer detaillierter Informationen zu Ihren mit Windows Admin Center verwalteten Servern in Security Center im Azure-Portal (oder über eine API)

Erfahren Sie mehr zum [Integrieren von Azure Security Center mit Windows Admin Center](windows-admin-center-integration.md).


### <a name="protection-for-azure-kubernetes-service"></a>Schutz für Azure Kubernetes Service

Für Azure Security Center werden die Containersicherheitsfeatures als Schutz für Azure Kubernetes Service (AKS) erweitert.

Die beliebte Open-Source-Plattform Kubernetes wurde so stark genutzt, dass sie zu einem Branchenstandard für die Containerorchestrierung geworden ist. Trotz dieser weit verbreiteten Implementierung gibt es immer noch Defizite, was das Schützen einer Kubernetes-Umgebung betrifft. Zum Verteidigen der Angriffsflächen einer Containeranwendung sind bestimmte Kenntnisse erforderlich, um sicherstellen zu können, dass die Infrastruktur sicher konfiguriert ist und ständig auf potenzielle Bedrohungen überwacht wird.

Die Verteidigung für Security Center umfasst Folgendes:

- **Ermittlung und Transparenz**: Kontinuierliche Ermittlung von verwalteten AKS-Instanzen in den für Security Center registrierten Abonnements.
- **Sicherheitsempfehlungen**: Direkt umsetzbare Empfehlungen, damit Sie für AKS die bewährten Methoden in Bezug auf die Sicherheit anwenden können. Diese Empfehlungen sind in Ihrer Sicherheitsbewertung enthalten, um dafür zu sorgen, dass sie als Teil des Sicherheitsstatus Ihres Unternehmens angesehen werden. Ein Beispiel für eine Empfehlung mit AKS-Bezug, die ggf. angezeigt wird, ist „Die rollenbasierte Zugriffssteuerung sollte genutzt werden, um den Zugriff auf einen Kubernetes Service-Cluster einzuschränken“.
- **Bedrohungsschutz**: Security Center analysiert Ihre AKS-Bereitstellung ständig und warnt Sie vor Bedrohungen und schädlichen Aktivitäten, die auf dem Host und auf der AKS-Clusterebene erkannt werden.

Erfahren Sie mehr zur [Integration von Security Center in Azure Kubernetes Service](azure-kubernetes-service-integration.md).

Erfahren Sie mehr zu den [Containersicherheitsfeatures von Security Center](container-security.md).


### <a name="improved-just-in-time-experience"></a>Verbesserte Just-In-Time-Umgebung

Die Features, der Betrieb und die Benutzeroberfläche der Just-In-Time-Tools von Azure Security Center, mit denen Ihre Verwaltungsports geschützt werden, wurden wie folgt verbessert: 

- **Feld für Begründung**: Beim Anfordern des Zugriffs auf einen virtuellen Computer (VM) über die Just-In-Time-Seite des Azure-Portals ist ein neues optionales Feld verfügbar, in dem eine Begründung für die Anforderung eingegeben werden kann. Die in diesem Feld eingegebenen Informationen können im Aktivitätsprotokoll nachverfolgt werden. 
- **Automatische Bereinigung von redundanten Just-In-Time-Regeln (JIT)** : Bei jedem Update einer JIT-Richtlinie wird automatisch ein Bereinigungstool ausgeführt, um die Gültigkeit des gesamten Regelsatzes zu überprüfen. Das Tool sucht nach Konflikten zwischen den Regeln in Ihrer Richtlinie und den Regeln in der NSG. Wenn das Bereinigungstool einen Konflikt feststellt, ermittelt es die Ursache und entfernt integrierte Regeln, die nicht mehr benötigt werden – sofern dies auf sichere Weise möglich ist. Der Cleaner löscht niemals Regeln, die Sie erstellt haben. 

Erfahren Sie mehr zum [Feature für JIT-Zugriff](security-center-just-in-time.md).


### <a name="two-security-recommendations-for-web-applications-deprecated"></a>Zwei Sicherheitsempfehlungen für Webanwendungen als veraltet gekennzeichnet

Zwei Sicherheitsempfehlungen zu Webanwendungen wurden als veraltet gekennzeichnet: 

- Regeln für Webanwendungen in IaaS-NSGs müssen verstärkt werden.
    (Zugehörige Richtlinie: Für Webanwendungen in IaaS müssen die NSG-Regeln verstärkt werden)

- Zugriff auf App Services muss eingeschränkt sein.
    (Zugehörige Richtlinie: Zugriff auf App Services muss eingeschränkt sein [Vorschau])

Diese Empfehlungen werden nicht mehr in der Security Center-Liste mit den Empfehlungen angezeigt. Die zugehörigen Richtlinien sind nicht mehr in der Initiative mit dem Namen „Security Center-Standardrichtlinie“ enthalten.

Erfahren Sie mehr über [Sicherheitsempfehlungen](recommendations-reference.md).



## <a name="february-2020"></a>Februar 2020

### <a name="fileless-attack-detection-for-linux-preview"></a>Erkennung von dateilosen Angriffen für Linux (Vorschau)

Da Angreifer immer häufiger verdeckte Methoden nutzen, um einer Entdeckung zu entgehen, wird die Erkennung von dateilosen Angriffen für Azure Security Center zusätzlich zu Windows auch auf Linux erweitert. Bei dateilosen Angriffen werden Sicherheitsrisiken in der Software ausgenutzt, schädliche Nutzlasten in unkritische Systemprozesse eingeschleust und Angriffe im Arbeitsspeicher verborgen. Diese Vorgehensweise soll Folgendes bewirken:

- Verringerung oder Beseitigung der Spuren von Schadsoftware auf Datenträgern
- Erhebliche Verringerung der Wahrscheinlichkeit, dass Angreifer von datenträgerbasierten Schadsoftware-Scanlösungen erkannt werden

Als Reaktion auf diese Bedrohung wurde die Azure Security Center-Erkennung von dateilosen Angriffen für Windows im Oktober 2018 veröffentlicht und nun auch auf Linux ausgeweitet. 


## <a name="january-2020"></a>Januar 2020

### <a name="enhanced-secure-score-preview"></a>Erweiterte Sicherheitsbewertung (Vorschau)

Eine erweiterte Version der Sicherheitsbewertung von Azure Security Center ist jetzt verfügbar und befindet sich in der Vorschauphase. Bei dieser Version werden mehrere Empfehlungen zu Sicherheitssteuerelementen gruppiert, die Ihre anfälligen Angriffsflächen besser widerspiegeln (z. B. Einschränkung des Zugriffs auf Verwaltungsports).

Machen Sie sich mit den Änderungen vertraut, die während der Vorschauphase für die Sicherheitsbewertung vorgenommen werden, und ermitteln Sie andere Lösungen, mit denen Sie Ihre Umgebung noch besser schützen können.

Erfahren Sie mehr zu [Erweiterte Sicherheitsbewertung (Vorschau) in Azure Security Center](secure-score-security-controls.md).