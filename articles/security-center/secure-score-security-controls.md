---
title: Secure Score in Azure Security Center
description: Beschreibung des Secure Scores von Azure Security Center und seiner Sicherheitskontrollen
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetd: c42d02e4-201d-4a95-8527-253af903a5c6
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/10/2020
ms.author: memildin
ms.openlocfilehash: d7eea9cd83e72b6ffeaae319a8e87c065015e6b5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79415836"
---
# <a name="enhanced-secure-score-preview-in-azure-security-center"></a>Erweiterter Secure Score (Vorschau) in Azure Security Center

## <a name="introduction-to-secure-score"></a>Einführung in Secure Score

Azure Security Center hat zwei Hauptziele: Unterstützung beim Ermitteln Ihrer aktuellen Sicherheitssituation sowie beim effizienten und effektiven Verbessern Ihrer Sicherheit. Der zentrale Aspekt von Security Center, mit dem Sie diese Ziele erreichen können, ist Secure Score.

Security Center führt eine ständige Bewertung Ihrer Ressourcen, Abonnements und Organisation in Bezug auf Sicherheitsprobleme durch. Anschließend werden alle Ergebnisse in einer einzigen Bewertung zusammengefasst, sodass Sie auf einen Blick Ihre aktuelle Sicherheitssituation erkennen können: je höher die Bewertung, desto geringer das ermittelte Risiko. Mithilfe der Bewertung können Sie Sicherheitsmaßnahmen und -projekte in Ihrer Organisation verfolgen. 

Die Seite „Secure Score“ von Security Center umfasst Folgendes:

- **Score:** Die Sicherheitsbewertung wird als Prozentwert angezeigt, die zugrunde liegenden Werte sind aber ebenfalls klar:

    [![Sicherheitsbewertung als Prozentwert mit deutlich erkennbaren zugrunde liegenden Zahlen](media/secure-score-security-controls/secure-score-with-percentage.png)](media/secure-score-security-controls/secure-score-with-percentage.png#lightbox)

- **Sicherheitskontrollen:** Jede Sicherheitskontrolle ist eine logische Gruppe verwandter Sicherheitsempfehlungen, die anfällige Angriffsflächen widerspiegelt. Eine Sicherheitskontrolle umfasst eine Reihe von Sicherheitsempfehlungen mit Anweisungen, mit denen Sie diese Empfehlungen implementieren können. Ihre Bewertung verbessert sich nur, wenn Sie *alle* Empfehlungen für eine einzelne Ressource innerhalb einer Sicherheitskontrolle umsetzen.

    Um sofort zu sehen, wie gut jede einzelne Angriffsfläche in Ihrer Organisation geschützt ist, sehen Sie sich die Bewertungen für die einzelnen Sicherheitskontrollen an.

    Weitere Informationen finden Sie unter [Berechnen der Sicherheitsbewertung](secure-score-security-controls.md#how-the-secure-score-is-calculated). 


>[!TIP]
> Frühere Versionen von Security Center verteilten Punkte auf Empfehlungsebene: Wenn Sie eine Empfehlung für eine einzelne Ressource umgesetzt hatten, wurde Ihre Sicherheitsbewertung verbessert. Nun wird die Bewertung nur verbessert, wenn Sie *alle* Empfehlungen für eine einzelne Ressource innerhalb einer Sicherheitskontrolle umsetzen. Das bedeutet, dass sich Bewertung nur verbessert, wenn die Sicherheit einer Ressource verbessert wird.
> Während sich diese erweiterte Version noch in der Vorschauphase befindet, steht die frühere Secure Score-Funktion im Azure-Portal als Option zur Verfügung. 


## <a name="locating-your-secure-score"></a>Auffinden Ihrer Sicherheitsbewertung

Die Bewertung wird im Security Center hervorgehoben angezeigt und ist auf der Übersichtsseite als Erstes zu sehen. Wenn Sie sich zur dedizierten Seite von Secure Score durchklicken, wird die Bewertung nach Abonnement aufgeschlüsselt angezeigt. Klicken Sie auf ein einzelnes Abonnement, um die detaillierte Liste mit priorisierten Empfehlungen und die möglichen Auswirkungen anzuzeigen, die eine Umsetzung dieser Empfehlungen auf die Bewertung des Abonnements hat.

## <a name="how-the-secure-score-is-calculated"></a>Berechnen der Sicherheitsbewertung 

Der jeweilige Beitrag der einzelnen Sicherheitskontrollen zur gesamten Sicherheitsbewertung ist auf der Seite mit Empfehlungen eindeutig angegeben.

[![Mit dem erweiterten Secure Score (Vorschauversion) werden Sicherheitskontrollen eingeführt](media/secure-score-security-controls/security-controls.png)](media/secure-score-security-controls/security-controls.png#lightbox)

Um alle zu erzielenden Punkte für eine Sicherheitskontrolle zu erhalten, müssen alle Ihre Ressourcen allen Sicherheitsempfehlungen innerhalb der Sicherheitskontrolle entsprechen. Beispielsweise enthält Security Center mehrere Empfehlungen zum Schützen Ihrer Verwaltungsports. In der Vergangenheit konnten Sie einige dieser zusammengehörigen und voneinander abhängigen Empfehlungen umsetzen, während andere nicht berücksichtigt wurden, und Ihre Sicherheitsbewertung verbesserte sich trotzdem. Bei objektiver Betrachtung kann man sagen, dass sich die Sicherheit erst dann verbessert hat, wenn alle Empfehlungen umgesetzt wurden. Zum jetzigen Zeitpunkt müssen alle Empfehlungen umgesetzt werden, um eine Änderung bei der Sicherheitsbewertung zu erzielen.

Beispielsweise wird die Sicherheitskontrolle „Systemupdates anwenden“ mit maximal sechs Punkten bewertet. Dieser Wert ist in der QuickInfo zur potenziellen Steigerung der Sicherheitskontrolle angegeben:

[![Sicherheitskontrolle „Systemupdates anwenden“](media/secure-score-security-controls/apply-system-updates-control.png)](media/secure-score-security-controls/apply-system-updates-control.png#lightbox)

Als Verbesserungspotenzial für die Sicherheitskontrolle „Systemupdates anwenden“ ist im Screenshot oben „2 % (1 Punkt)“ angegeben. Das bedeutet, dass sich bei Umsetzung aller Empfehlungen in diesem Kontrollelement die Bewertung um 2 % erhöht (in diesem Fall um einen Punkt). Der Einfachheit halber werden Werte in der Spalte mit der potenziellen Steigerung in der Empfehlungsliste auf ganze Zahlen gerundet. In den QuickInfos sind die genauen Werte angegeben:

* **Maximale Bewertung**: Die maximale Anzahl von Punkten, die Sie durch Erfüllen aller Empfehlungen innerhalb eines Kontrollelements erhalten können. Die maximale Bewertung für ein Kontrollelement gibt die relative Bedeutung dieses Kontrollelements an. Verwenden Sie die maximale Bewertung für die Selektierung der Probleme, die zuerst behandelt werden müssen. 
* **Potenzielle Steigerung**: Die Anzahl der noch verfügbaren Punkte innerhalb des Kontrollelements. Damit diese Punkte Ihrer Sicherheitsbewertung hinzugefügt werden, setzen Sie alle Empfehlungen der Sicherheitskontrolle um. Im Beispiel oben liegt der eine angezeigte Punkt für das Kontrollelement tatsächlich bei 0,96 Punkten.
* **Aktuelle Bewertung**: Die aktuelle Bewertung für dieses Kontrollelement. Jedes Kontrollelement trägt zur Gesamtbewertung bei. In diesem Beispiel trägt das Kontrollelement 5,04 Punkte zum Gesamtergebnis bei. 

### <a name="calculations---understanding-your-score"></a>Berechnungen – Verstehen der Bewertung

|Metrik|Formel und Beispiel|
|-|-|
|**Aktuelle Bewertung der Sicherheitskontrolle**|<br>![Gleichung zum Berechnen der aktuellen Bewertung einer Sicherheitskontrolle](media/secure-score-security-controls/security-control-scoring-equation.png)<br><br>Jede einzelne Sicherheitskontrolle trägt zur Sicherheitsbewertung bei. Jede Ressource, die von einer Empfehlung innerhalb des Kontrollelements betroffen ist, trägt zur aktuellen Bewertung des Kontrollelements bei. Die aktuelle Bewertung für jedes Kontrollelement ist ein Maß für den Status der Ressourcen *innerhalb* des Kontrollelements.<br>![QuickInfos mit den Werten, die beim Berechnen der aktuellen Bewertung der Sicherheitskontrolle verwendet werden](media/secure-score-security-controls/security-control-scoring-tooltips.png)<br>In diesem Beispiel wird die maximale Bewertung von 6 durch 78 dividiert, da dies die Summe aus fehlerfreien und fehlerhaften Ressourcen ist.<br>6 / 78 = 0,0769<br>Die Multiplikation dieses Ergebnisses mit der Anzahl der fehlerfreien Ressourcen (4) ergibt die aktuelle Bewertung:<br>0,0769 * 4 = **0,31**<br><br>|
|**Sicherheitsbewertung**<br>Ein Abonnement|<br>![Gleichung zum Berechnen der aktuellen Sicherheitsbewertung](media/secure-score-security-controls/secure-score-equation.png)<br><br>![Sicherheitsbewertung eines einzelnen Abonnements, wenn alle Kontrollen aktiviert sind](media/secure-score-security-controls/secure-score-example-single-sub.png)<br>In diesem Beispiel gibt es ein einzelnes Abonnement, für das alle Sicherheitskontrollen verfügbar sind (potenzielle Höchstbewertung sind 60 Punkte). Die Bewertung gibt 28 Punkte der möglichen 60 Punkte an. Die verbleibenden 32 Punkte sind in den Zahlen für die potenzielle Bewertungssteigerung der Sicherheitskontrollen enthalten.<br>![Liste der Kontrollen und potenziellen Bewertungssteigerung](media/secure-score-security-controls/secure-score-example-single-sub-recs.png)|
|**Sicherheitsbewertung**<br>Mehrere Abonnements|<br>Die aktuellen Bewertungen für alle Ressourcen in allen Abonnements werden addiert. Die anschließende Berechnung entspricht der für ein einzelnes Abonnement.<br><br>Beim Anzeigen mehrerer Abonnements werden von Secure Score alle Ressourcen in allen aktivierten Richtlinien ausgewertet und nach deren kombinierter Auswirkung auf die Höchstbewertung der einzelnen Sicherheitskontrollen gruppiert.<br>![Sicherheitsbewertung für mehrere Abonnements, wenn alle Sicherheitskontrollen aktiviert sind](media/secure-score-security-controls/secure-score-example-multiple-subs.png)<br>Die kombinierte Bewertung ist **kein** Durchschnittswert, sondern vielmehr die Auswertung des Status aller Ressourcen in allen Abonnements.<br>Wenn Sie die Seite mit den Empfehlungen aufrufen und die potenziell verfügbaren Punkte addieren, werden Sie feststellen, dass es sich hierbei um die Differenz zwischen der aktuellen Bewertung (24) und der verfügbaren Höchstbewertung (60) handelt.|
||||

## <a name="improving-your-secure-score"></a>Verbessern Ihrer Sicherheitsbewertung

Zum Verbessern Ihrer Sicherheitsbewertung setzen Sie die Sicherheitsempfehlungen in Ihrer Empfehlungsliste um. Sie können jede Empfehlung manuell für die einzelnen Ressourcen umsetzen oder die Option **Schnelle Problembehebung** verwenden (falls verfügbar), um eine Empfehlung für eine Gruppe von Ressourcen schnell anzuwenden. Weitere Informationen finden Sie unter [Umsetzen von Empfehlungen](security-center-remediate-recommendations.md).

>[!IMPORTANT]
> Nur integrierte Empfehlungen haben eine Auswirkung auf die Sicherheitsbewertung.

## <a name="security-controls-and-their-recommendations"></a>Sicherheitskontrollen und deren Empfehlungen

In der folgenden Tabelle sind die Sicherheitskontrollen in Azure Security Center aufgelistet. Für jede Sicherheitskontrolle ist die maximale Anzahl von Punkten angegeben, die Ihrer Sicherheitsbewertung hinzugefügt wird, wenn Sie *alle* in der Sicherheitskontrolle aufgeführten Empfehlungen für *alle* Ihre Ressourcen umsetzen. 

> [!TIP]
> Wenn Sie diese Liste anders filtern oder sortieren möchten, kopieren Sie die Liste, und fügen Sie sie in Excel ein.

|Sicherheitskontrolle|Maximale Punktzahl der Sicherheitsbewertung|Empfehlungen|
|----------------|:-------------------:|---------------|
|**Aktivieren der MFA**|10|- Für Konten mit Besitzerberechtigungen für Ihr Abonnement muss MFA aktiviert sein<br>- Für Konten mit Leseberechtigungen für Ihr Abonnement muss MFA aktiviert sein<br>- Für Konten mit Schreibberechtigungen für Ihr Abonnement sollte MFA aktiviert sein|
|**Verwaltungsports sichern**|8|- Die Just-in-Time-Netzwerkzugriffssteuerung muss auf virtuelle Computer angewendet werden<br>- Virtuelle Computer sollten einer Netzwerksicherheitsgruppe zugeordnet werden<br>- Verwaltungsports sollten auf Ihren VMs geschlossen werden|
|**Systemupdates anwenden**|6|- Überwachungs-Agent-Integritätsprobleme sollten auf Ihren Computern gelöst werden<br>- Der Überwachungs-Agent sollte für VM-Skalierungsgruppen installiert werden<br>- Der Überwachungs-Agent sollte auf Ihren Computern installiert werden<br>- Die Betriebssystemversion sollte für Ihre Clouddienstrollen aktualisiert werden<br>- Systemupdates für VM-Skalierungsgruppen sollten installiert werden<br>- Systemupdates müssen auf Ihren Computern installiert werden<br>- Ihre Computer sollten zur Anwendung von Systemupdates neu gestartet werden<br>- Für Kubernetes Service muss ein Upgrade auf eine Kubernetes-Version ohne Sicherheitsrisiko durchgeführt werden<br>- Der Überwachungs-Agent sollte auf Ihren virtuellen Computern installiert werden|
|**Sicherheitsrisiken beheben**|6|- Für Ihre SQL Server-Instanzen muss Advanced Data Security aktiviert sein<br>- Sicherheitsrisiken in Azure Container Registry-Images müssen behoben werden<br>- Sicherheitsrisiken in Ihren SQL-Datenbanken müssen entschärft werden<br>- Sicherheitsrisiken müssen durch eine Lösung zur Sicherheitsrisikobewertung entschärft werden<br>- Für Ihre verwalteten SQL-Instanzen muss eine Sicherheitsrisikobewertung aktiviert sein<br>- Für Ihre SQL Server-Instanzen muss eine Sicherheitsrisikobewertung aktiviert sein<br>- Die Lösung zur Sicherheitsrisikobewertung sollte auf Ihren virtuellen Computern installiert werden|
|**Verschlüsselung ruhender Daten aktivieren**|4|- Auf VMs muss die Datenträgerverschlüsselung angewendet werden<br>- Für SQL-Datenbanken muss Transparent Data Encryption aktiviert sein<br>- Automation-Kontovariablen müssen verschlüsselt werden<br>- Für Service Fabric-Cluster muss die ClusterProtectionLevel-Eigenschaft auf EncryptAndSign festgelegt sein<br>- SQL Server-TDE-Schutz muss mit eigenem Schlüssel verschlüsselt werden|
|**Daten während der Übertragung verschlüsseln**|4|- Zugriff auf API-App nur über HTTPS gestatten<br>- Zugriff auf Funktions-App nur über HTTPS gestatten<br>- Für Ihren Redis Cache dürfen nur sichere Verbindungen aktiviert sein<br>- Für Speicherkonten muss die sichere Übertragung aktiviert sein<br>- Zugriff auf Webanwendung nur über HTTPS gestatten|
|**Zugriff und Berechtigungen verwalten**|4|- Für Ihr Abonnement dürfen maximal 3 Besitzer festgelegt werden<br>- Veraltete Konten müssen aus Ihrem Abonnement entfernt werden (Vorschau)<br>- Veraltete Konten mit Besitzerberechtigungen müssen aus Ihrem Abonnement entfernt werden (Vorschau)<br>- Externe Konten mit Besitzerberechtigungen müssen aus Ihrem Abonnement entfernt werden (Vorschau)<br>- Externe Konten mit Leseberechtigungen müssen aus Ihrem Abonnement entfernt werden<br>- Externe Konten mit Schreibberechtigungen müssen aus Ihrem Abonnement entfernt werden (Vorschau)<br>- Ihrem Abonnement muss mehr als ein Besitzer zugewiesen sein<br>- Für Kubernetes Service muss die rollenbasierte Zugriffssteuerung (RBAC) verwendet werden (Vorschau)<br>- Service Fabric-Cluster dürfen nur Azure Active Directory für die Clientauthentifizierung verwenden|
|**Optimieren von Sicherheitskonfigurationen**|4|- Podsicherheitsrichtlinien für Kubernetes Service definieren<br>- Sicherheitsrisiken in Containersicherheitskonfigurationen sollten behoben werden<br>- Sicherheitsrisiken in der Sicherheitskonfiguration auf Ihren Computern müssen entschärft werden<br>- Sicherheitsrisiken in der Sicherheitskonfiguration Ihrer VM-Skalierungsgruppen müssen entschärft werden<br>- Der Überwachungs-Agent sollte auf Ihren virtuellen Computern installiert werden<br>- Der Überwachungs-Agent sollte auf Ihren Computern installiert werden<br>- Der Überwachungs-Agent sollte für VM-Skalierungsgruppen installiert werden<br>- Überwachungs-Agent-Integritätsprobleme sollten auf Ihren Computern gelöst werden|
|**Nicht autorisierten Netzwerkzugriff einschränken**|4|- IP-Weiterleitung für Ihre VM muss deaktiviert sein<br>- Für Kubernetes Service müssen autorisierte IP-Adressbereiche definiert werden (Vorschau)<br>– (VERALTET) Zugriff auf App Services muss eingeschränkt sein (Vorschau)<br>– (VERALTET) Regeln für Webanwendungen in IaaS-NSGs müssen verstärkt werden<br>- Virtuelle Computer sollten einer Netzwerksicherheitsgruppe zugeordnet werden<br>- Zugriff auf API-App über CORS nicht allen Ressourcen gestatten<br>- CORS darf nicht jeder Ressource Zugriff auf Ihre Funktions-App erteilen<br>- CORS darf nicht jeder Ressource Zugriff auf Ihre Webanwendungen erteilen<br>- Remotedebuggen muss für API-App deaktiviert sein<br>- Remotedebuggen muss für Funktions-Apps deaktiviert sein<br>- Remotedebuggen muss für Webanwendung deaktiviert sein<br>- Der Zugriff muss für freizügige Netzwerksicherheitsgruppen mit VMs mit Internetzugriff eingeschränkt werden<br>- Für VMs mit Internetzugriff müssen die NSG-Regeln verstärkt werden|
|**Adaptive Anwendungssteuerung anwenden**|3|- Für VMs muss die adaptive Anwendungssteuerung aktiviert sein<br>- Der Überwachungs-Agent sollte auf Ihren virtuellen Computern installiert werden<br>- Der Überwachungs-Agent sollte auf Ihren Computern installiert werden<br>- Überwachungs-Agent-Integritätsprobleme sollten auf Ihren Computern gelöst werden|
|**Datenklassifizierung anwenden**|2|- Sensible Daten in Ihren SQL-Datenbanken müssen klassifiziert werden (Vorschau)|
|**Anwendungen vor DDoS-Angriffen schützen**|2|- DDoS Protection Standard muss aktiviert sein|
|**Endpoint Protection aktivieren**|2|- Endpoint Protection-Integritätsfehler sollten für VM-Skalierungsgruppen behoben werden<br>- Endpoint Protection-Integritätsprobleme sollten auf Ihren Computern gelöst werden<br>- Auf VM-Skalierungsgruppen muss die Endpoint Protection-Lösung installiert sein<br>- Endpoint Protection-Lösung auf virtuellen Computern installieren<br>- Überwachungs-Agent-Integritätsprobleme sollten auf Ihren Computern gelöst werden<br>- Der Überwachungs-Agent sollte für VM-Skalierungsgruppen installiert werden<br>- Der Überwachungs-Agent sollte auf Ihren Computern installiert werden<br>- Der Überwachungs-Agent sollte auf Ihren virtuellen Computern installiert werden<br>- Endpoint Protection-Lösung auf Ihren Computern installieren|
|**Überwachung und Protokollierung aktivieren**|1|- Die Überwachung in SQL Server muss aktiviert werden<br>- In App Services müssen Diagnoseprotokolle aktiviert sein<br>- In Azure Data Lake Store müssen Diagnoseprotokolle aktiviert sein<br>- In Azure Stream Analytics müssen Diagnoseprotokolle aktiviert sein<br>- In Batch-Konten müssen Diagnoseprotokolle aktiviert sein<br>- In Data Lake Analytics müssen Diagnoseprotokolle aktiviert sein<br>- In Event Hub müssen Diagnoseprotokolle aktiviert sein<br>- In IoT Hub müssen Diagnoseprotokolle aktiviert sein<br>- In Key Vault müssen Diagnoseprotokolle aktiviert sein<br>- In Logic Apps müssen Diagnoseprotokolle aktiviert sein<br>-In den Suchdiensten müssen Diagnoseprotokolle aktiviert sein<br>- In Service Bus müssen Diagnoseprotokolle aktiviert sein<br>- In Virtual Machine Scale Sets müssen Diagnoseprotokolle aktiviert sein<br>- Für Batch-Konten müssen Warnungsregeln für Metriken konfiguriert sein<br>- Für SQL-Überwachungseinstellungen müssen Aktionsgruppen zum Erfassen kritischer Aktivitäten konfiguriert sein<br>- SQL Server-Instanzen müssen mit einer Überwachungsaufbewahrungsdauer von mehr als 90 Tagen konfiguriert werden|
|**Bewährte Sicherheitsmethoden implementieren**|0|- Der Zugriff auf Speicherkonten mit Firewall- und VNET-Konfigurationen sollte eingeschränkt werden<br>- Alle Autorisierungsregeln mit Ausnahme von RootManageSharedAccessKey müssen aus dem Event Hub-Namespace entfernt werden<br>- Für SQL Server-Instanzen muss ein Azure Active Directory-Administrator bereitgestellt werden<br>- Für die Event Hub-Instanz müssen Autorisierungsregeln definiert werden<br>- Speicherkonten müssen zu neuen Azure Resource Manager-Ressourcen migriert werden<br>- VMs müssen zu neuen Azure Resource Manager-Ressourcen migriert werden<br>- Advanced Data Security-Einstellungen für SQL Server-Instanz müssen eine E-Mail-Adresse zum Empfangen von Sicherheitswarnungen enthalten<br>- Für Ihre verwalteten Instanzen muss Advanced Data Security aktiviert sein<br>- In den Advanced Data Security-Einstellungen der verwalteten SQL Server-Instanz müssen alle Advanced Threat Protection-Typen aktiviert werden<br>- In den Advanced Data Security-Einstellungen für SQL Server müssen E-Mail-Benachrichtigungen an Administratoren und Abonnementbesitzer aktiviert sein<br>- Advanced Threat Protection-Typen müssen in den Advanced Data Security-Einstellungen der SQL Server-Instanz auf „Alle“ festgelegt werden<br>- Subnetze sollten einer Netzwerksicherheitsgruppe zugeordnet werden<br>- In den Advanced Data Security-Einstellungen der SQL Server-Instanz müssen alle Advanced Threat Protection-Typen aktiviert werden|
||||

## <a name="secure-score-faq"></a>Häufig gestellte Fragen zu Secure Score

### <a name="why-has-my-secure-score-gone-down"></a>Warum ist meine Sicherheitsbewertung niedriger ausgefallen?
Security Center wurde auf eine erweiterte Sicherheitsbewertung umgestellt (derzeit in der Vorschauphase), die Änderungen in der Art der Berechnung der Bewertung umfasst. Sie müssen nun alle Empfehlungen für eine Ressource umsetzen, um Punkte zu erhalten. Die Bewertungen wurden außerdem in eine Skala von 0 bis 10 geändert.

### <a name="if-i-address-only-three-out-of-four-recommendations-in-a-security-control-will-my-secure-score-change"></a>Ändert sich meine Sicherheitsbewertung, wenn ich nur drei von vier Empfehlungen in einer Sicherheitskontrolle umsetze?
Nein. Die Bewertung ändert sich erst, wenn Sie alle Empfehlungen für eine einzelne Ressource umgesetzt haben. Um die maximale Bewertung für ein Kontrollelement zu erhalten, müssen Sie alle Empfehlungen für alle Ressourcen umsetzen.

### <a name="is-the-previous-experience-of-the-secure-score-still-available"></a>Ist die vorherige Version von Secure Score noch verfügbar? 
Ja. Eine Zeit lang werden die beiden Bewertungen noch parallel ausgeführt, um den Übergang zu erleichtern. Sie sollten allerdings davon ausgehen, dass das ältere Modell mit der Zeit eingestellt wird. 

### <a name="if-a-recommendation-isnt-applicable-to-me-and-i-disable-it-in-the-policy-will-my-security-control-be-fulfilled-and-my-secure-score-updated"></a>Wenn eine Empfehlung für mich nicht anwendbar ist und ich sie in der Richtlinie deaktiviere, ist dann die Sicherheitskontrolle erfüllt, und wird meine Sicherheitsbewertung entsprechend aktualisiert?
Ja. Es wird empfohlen, Empfehlungen zu deaktivieren, wenn sie in Ihrer Umgebung nicht anwendbar sind. Anweisungen zum Deaktivieren einer bestimmten Empfehlung finden Sie unter [Deaktivieren von Sicherheitsrichtlinien](https://docs.microsoft.com/azure/security-center/tutorial-security-policy#disable-security-policies).

### <a name="if-a-security-control-offers-me-zero-points-towards-my-secure-score-should-i-ignore-it"></a>Wenn durch eine Sicherheitskontrolle keine Punkte für meine Sicherheitsbewertung erzielt werden, soll ich diese ignorieren?
In einigen Fällen wird als maximale Bewertung für ein Kontrollelement ein Wert größer 0 (null) angezeigt, jedoch ist die Auswirkung gleich null. Wenn der inkrementelle Wert für das Korrigieren von Ressourcen unerheblich ist, wird auf null gerundet. Ignorieren Sie diese Empfehlungen aber nicht, da Sie dennoch Sicherheitsverbesserungen bieten. Die einzige Ausnahme ist das Kontrollelement „Zusätzliche bewährte Methode“. Durch das Umsetzen dieser Empfehlungen erhöht sich die Bewertung nicht, doch Sie verbessern die Gesamtsicherheit.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurden Secure Score und die damit eingeführten Sicherheitskontrollen beschrieben. Weitere Informationen finden Sie in den folgenden Artikeln:

- [Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md)
- [Umsetzen von Empfehlungen in Azure Security Center](security-center-remediate-recommendations.md)