---
title: Aktivierungs- und Deaktivierungslebenszyklus des Azure Service Fabric-Hostings
description: In diesem Artikel wird der Lebenszyklus von Anwendungen und „ServicePackage“ auf einem Knoten erläutert.
author: tugup
ms.topic: conceptual
ms.date: 05/1/2020
ms.author: tugup
ms.openlocfilehash: a39aecf16d1c3303c0a590b389ba2aa69d4472f2
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87405125"
---
# <a name="azure-service-fabric-hosting-lifecycle"></a>Azure Service Fabric-Hostinglebenszyklus
Dieser Artikel enthält eine Übersicht über die Ereignisse, die auftreten, wenn eine Anwendung auf einem Knoten aktiviert wird und verschiedene Clusterkonfigurationen zum Steuern des Verhaltens verwendet werden.

Bevor Sie fortfahren, müssen Sie die verschiedenen Konzepte und Beziehungen kennen, die unter [Modellieren von Anwendungen in Service Fabric][a1] erläutert werden. 

> [!NOTE]
> In diesem Artikel werden folgende Begriffe mit folgenden Bedeutungen verwendet, sofern nicht explizit erwähnt wird, dass sie in einer anderen Bedeutung verwendet werden:
>
> - *Replikat* bezieht sich sowohl auf ein Replikat eines zustandsbehafteten Diensts als auch auf eine Instanz eines zustandslosen Diensts.
> - *CodePackage* wird gleichbedeutend mit dem *ServiceHost*-Prozess verwendet, der einen *ServiceType* registriert und Replikate von Diensten dieses *ServiceType* hostet.
>

## <a name="activation-of-applicationservicepackage"></a>Aktivierung der Anwendung bzw. ServicePackage

Die Vorgehensweise für die Aktivierung lautet wie folgt:

1. Laden Sie das Anwendungspaket herunter. Beispiel: „ApplicationManifest.xml“.
2. Richten Sie die Umgebung für die Anwendung ein, zum Beispiel zum Erstellen von Benutzern.
3. Starten Sie die Nachverfolgung der Anwendung, um die Deaktivierung zu ermitteln.
4. Laden Sie ServicePackage herunter. Beispiel: „ServiceManifest.xml“, Code, Konfiguration, Datenpakete usw.
5. Richten Sie die Umgebung für das Dienstpaket ein, richten Sie zum Beispiel die Firewall ein, weisen Sie Ports für Endpunkte zu usw.
6. Starten Sie die Nachverfolgung des Dienstpakets, um die Deaktivierung zu ermitteln.
7. Starten Sie SetupEntryPoint von CodePackages, und warten Sie, bis der Vorgang abgeschlossen ist.
8. Starten Sie MainEntryPoint von CodePackages.

### <a name="servicetype-blocklisting"></a>Aufnehmen von ServiceType in die Blockliste
**ServiceTypeDisableFailureThreshold** bestimmt die Anzahl der Fehler (Aktivierungs-, Download-, CodePackage-Fehler). Anschließend wird die Aufnahme von ServiceType in die Blockliste geplant. Der Aktivierungs-/Downloadfehler oder der Absturz von CodePackage sollte zunächst die Planung der Aufnahme von ServiceType in die Blockliste auslösen. Die Konfiguration **ServiceTypeDisableGraceInterval** bestimmt das Toleranzintervall, nach dem ServiceType auf dem Knoten schließlich in die Blockliste aufgenommen wird. Beachten Sie, dass hierzu sich der Neustart der Aktivierung, des Downloads oder von CodePackage weiterhin im internen Wiederholungsmodus befinden und vom Hostingsubsystem überwacht werden muss. Wiederholen bedeutet beispielsweise: Der erneute Start von CodePackage wird nach dem Absturz geplant, oder Service Fabric versucht, die Pakete wieder herunterzuladen.
Nach der Aufnahme in die Blockliste sollte der Fehler „System.Hosting hat Error für die Eigenschaft ServiceTypeRegistration:ServiceType gemeldet. Der ServiceType wurde auf dem Knoten deaktiviert.“ angezeigt werden.

ServiceType wird in folgenden Fällen wieder auf dem Knoten aktiviert: 
- Wenn der Aktivierungsvorgang erfolgreich durchgeführt wird oder die Anzahl von **ActivationMaxFailureCount** an Wiederholungen nach einem Fehler erreicht wird.
- Wenn der Downloadvorgang erfolgreich durchgeführt wird oder die Anzahl von **DeploymentMaxFailureCount** an Wiederholungen nach einem Fehler erreicht wird.
- Wenn ein abgestürztes CodePackage wieder gestartet wird und erfolgreich den ServiceType registriert.

Der Grund dafür, dass ServiceType nach **ActivationMaxFailureCount**/**DeploymentMaxFailureCount** Wiederholungen wieder aktiviert wird, besteht darin, dass diese Werte die maximale Anzahl der Versuche beschreiben, die Service Fabric durchführt, um eine Anwendung auf einem Knoten zu aktivieren oder herunterzuladen. Wenn dieser Vorgang nicht erfolgreich ausgeführt wird, wird der aktuelle Vorgang nicht wiederholt, und da Service Fabric dem Dienst eine weitere Aktivierungsmöglichkeit gibt, die erfolgreich durchgeführt werden könnte, löst sich das Problem möglicherweise selbst, wenn es an den Lebenszyklus der Aktivierung oder des Downloads gebunden ist. Ein neuer Aktivierungs-/Downloadvorgang, der durch die Platzierung eines Replikats ausgelöst wird, kann die Aufnahme von ServiceType wieder auslösen oder der Vorgang wird erfolgreich ausgeführt.

> [!NOTE]
> Wenn Ihr CodePackage den Absturz von ServiceType nicht registriert, wirkt sich das nicht auf ServiceType aus. Nur das CodePackage, das ein abstürzendes Replikat hostet, wirkt sich auf den ServiceType aus.
>

### <a name="codepackage-crash"></a>Absturz von CodePackage
Bei einem Absturz von CodePackage verwendet Service Fabric ein Backoff für den Neustart. Dieses Backoff ist nicht abhängig davon, ob das Codepaket über einen registrierten Typ verfügt oder nicht.

Der Backoffwert lautet immer „Min(RetryTime, **ActivationMaxRetryInterval**)“. Dabei kann es sich um einen konstanten, linearen oder exponentiellen Wert handeln, der auf der Konfiguration **ActivationRetryBackoffExponentiationBase** basiert.

- Konstante: Wenn „**ActivationRetryBackoffExponentiationBase** == 0“ vorliegt, dann gilt „RetryTime = **ActivationRetryBackoffInterval**;“
- Linear:  Wenn „**ActivationRetryBackoffExponentiationBase** == 0“ vorliegt, dann gilt RetryTime = ContinuousFailureCount × **ActivationRetryBackoffInterval**. Hierbei entspricht ContinousFailureCount der Anzahl der Abstürze oder fehlerhaften Aktivierungen von CodePackage.
- Exponentiell: RetryTime = (**ActivationRetryBackoffInterval** in Sekunden) × (**ActivationRetryBackoffExponentiationBase** ^ ContinuousFailureCount);
    
Sie können das Verhalten nach Bedarf steuern, um beispielsweise schnelle Neustarts einzurichten. In folgenden wird der lineare Wert näher betrachtet. In diesem Fall liegt der Startintervall bei einem CodePackage-Absturz nach 10, 20, 30 und 40 Sekunden, nachdem CodePackage deaktiviert wurde. 
    
Die maximale Zeit, die Service Fabric nach einem Fehler für das Backoff wartet, wird von dem Wert **ActivationMaxRetryInterval** vorgegeben.
    
Wenn Ihr CodePackage abstürzt und neu gestartet wird, muss es für den von **CodePackageContinuousExitFailureResetInterval** vorgegebenen Zeitraum aktiv bleiben, damit Service Fabric es als fehlerfrei ansieht. Dann wird der Integritätsbericht als „OK“ überschrieben, und ContinousFailureCount wird zurückgesetzt.

### <a name="codepackage-not-registering-servicetype"></a>CodePackage registriert ServiceType nicht
Angenommen ein CodePackage bleibt aktiv und soll einen ServiceType registrieren, aber tut dies nicht. In diesem Fall generiert Service Fabric eine Warnung (HealthReport) nach **ServiceTypeRegistrationTimeout**, die angibt, dass ServiceType nicht innerhalb des Zeitlimits konfiguriert wurde.

### <a name="activation-failure"></a>Aktivierungsfehler
Service Fabric verwendet immer ein lineares Backoff (wie bei CodePackage-Abstürzen), wenn während der Aktivierung ein Fehler erkannt wird. Das bedeutet, dass der Aktivierungsvorgang nach 100 Sekunden (0 + 10 + 20 + 30 + 40) aufgegeben wird (der erste Wiederholungsversuch erfolgt sofort). Anschließend werden keine neuen Aktivierungsversuche durchgeführt.
    
Für das maximale Backoff kann **ActivationMaxRetryInterval** und für Wiederholungen kann **ActivationMaxFailureCount** verwendet werden.

### <a name="download-failure"></a>Downloadfehler
Service Fabric verwendet immer ein lineares Backoff, wenn während des Downloads ein Fehler auftritt. Das bedeutet, dass der Aktivierungsvorgang nach 100 Sekunden (0 + 10 + 20 + 30 + 40) aufgegeben wird (der erste Wiederholungsversuch erfolgt sofort). Anschließend wird der Downloadvorgang nicht wiederholt. Das lineare Backoff für Downloads entspricht ContinuousFailureCount × **DeploymentRetryBackoffInterval**. Das maximale Backoff lautet **DeploymentMaxRetryInterval**. Wie bei Aktivierungen können Downloadvorgänge so oft wiederholt werden, wie es von **ActivationMaxFailureCount** vorgegeben wird.

> [!NOTE]
> Bevor Sie die Konfigurationen ändern, sollten Sie einige Beispiele berücksichtigen.

* Wenn das CodePackage weiterhin abstürzt und Backoffs vornimmt, wird ServiceType deaktiviert. Wenn die Aktivierungskonfiguration jedoch für schnelle Neustarts eingerichtet ist, kann das CodePackage mehrmals aktiviert werden, bevor die Deaktivierung von ServiceType erkannt wird. Beispiel: Angenommen Ihr CodePackage registriert den ServiceType mit Service Fabric und stürzt dann ab. In diesem Fall wird der **ServiceTypeDisableGraceInterval**-Zeitraum abgebrochen, sobald der Host eine Typregistrierung empfängt. Dies kann wiederholt werden, bis Ihr CodePackage ein Backoff durchführt, das länger als **ServiceTypeDisableGraceInterval** ist. Anschließend wird der ServiceType auf dem Knoten deaktiviert. Daher kann es eine Weile dauern, bis der ServiceType auf dem Knoten deaktiviert wird.

* Wenn das Service Fabric-System bei Aktivierungen ein Replikat auf einem Knoten platzieren muss, fordert ReconfigurationAgent (RA) das Hostingsubsystem dazu auf, die Anwendung zu aktivieren. Die Aktivierungsanforderung wird alle 15 Sekunden wiederholt (**RAPMessageRetryInterval**). Damit das Service Fabric-System weiß, dass ServiceType deaktiviert wurde, muss der Aktivierungsvorgang des Hosts länger als das Wiederholungsintervall und **ServiceTypeDisableGraceInterval** sein. Beispiel: Der Cluster verfügt über die Konfigurationen **ActivationMaxFailureCount** mit dem Wert 5 und **ActivationRetryBackoffInterval** mit einer Sekunde. Das bedeutet, dass der Aktivierungsvorgang nach 10 Sekunden (0 + 1 + 2 + 3 + 4) aufgegeben wird (die erste Wiederholung erfolgt sofort) und dass der Host keine weiteren Wiederholungsversuche durchführt. In diesem Fall wird der Aktivierungsvorgang abgeschlossen und nach 15 Sekunden nicht mehr wiederholt. Das liegt daran, dass Service Fabric alle Wiederholungsversuche innerhalb von 15 Sekunden ausführt. Jede Wiederholung von ReconfigurationAgent erstellt einen neuen Aktivierungsvorgang im Hostingsubsystem, und das Muster wird wiederholt, sodass ServiceType nie auf dem Knoten deaktiviert wird. Da ServiceType nicht auf dem Knoten deaktiviert wird, verschiebt die FailoverManager-Komponente des Service Fabric-Systems das Replikat nicht auf einen anderen Knoten.
> 

## <a name="deactivation"></a>Deaktivierung

Wenn ein ServicePackage auf einem Knoten aktiviert ist, wird er für die Deaktivierung überwacht. Die Deactivator-Entität überwacht die Deaktivierung.
Die Deactivator-Entität funktioniert auf zwei Arten:

1.  Regelmäßig:  Bei jedem **DeactivationScanInterval** prüft die Entität nach ServicePackages, die nie ein Replikat gehostet haben, und markiert diese als Kandidaten für die Deaktivierung.
2.  ReplicaClose: Wenn ein Replikat geschlossen wird, ruft die Deactivator-Entität einen DecrementUsageCount-Wert ab. Wenn der Wert auf 0 (null) fällt, hostet das ServicePackage kein Replikat und ist daher ein Kandidat für die Deaktivierung.

 Die Kandidaten für die Deaktivierung werden basierend auf dem Aktivierungsmodus [Exclusive/Shared][a2] geplant: nach **DeactivationGraceInterval** für SharedMode und nach **ExclusiveModeDeactivationGraceInterval** für ExclusiveMode. Wenn in diesem Zeitraum eine neue Platzierung eines Replikats erfolgt, wird die Deaktivierung abgebrochen.

### <a name="periodically"></a>Regelmäßig:
Beispiel 1: Angenommen die Deactivator-Entität führt zum Zeitpunkt von „Time T(**DeactivationScanInterval**)“ eine Überprüfung durch. Die nächste Überprüfung erfolgt um „2T“. Angenommen, eine ServicePackage-Aktivierung trat um „T+1“ auf. Dieses ServicePackage hostet kein Replikat und muss daher deaktiviert werden. Damit das ServicePackage als Kandidat für die Deaktivierung sein kann, muss es über den Zeitraum von „T“ kein Replikat aufweisen. Das bedeutet, es kann um „2T+1“ deaktiviert werden. Daher wird die Überprüfung um „2T“ das ServicePackage nicht als Kandidat für die Deaktivierung einstufen. Der nächste Deaktivierungszyklus um „3T“ plant dieses ServicePackage für die Deaktivierung ein, weil es dann über den Zeitraum von „T“ kein Replikat aufgewiesen hat.  

Beispiel 2: Angenommen ein ServicePackage wird zum Zeitpunkt „T–1“ aktiviert und die Deactivator-Entität führt um „T“ eine Überprüfung durch. Das ServicePackage hostet kein Replikat. Dann wird dieses ServicePackage bei der nächsten Überprüfung um „2T“ als Kandidat für die Deaktivierung eingestuft.  

Beispiel 3: Angenommen ein ServicePackage wird zum Zeitpunkt „T–1“ aktiviert und die Deactivator-Entität führt um „T“ eine Überprüfung durch. Das ServicePackage hostet noch kein Replikat. Jedoch wird um „T+1“ ein Replikat platziert. Der Host erhält einen IncrementUsageCount-Wert. d. h., ein Replikat wird erstellt. Dieses ServicePackage wird also um „2T“ nicht für die Deaktivierung eingeplant. Die Deaktivierung fährt dann mit der ReplicaClose-Logik fort, die weiter unten erläutert wird.

Beispiel 4: Angenommen, Ihr ServicePackage ist groß (z. B. 10 GB), dann kann der Download auf den Knoten einige Zeit dauern. Sobald eine Anwendung aktiviert wird, wird ihr Lebenszyklus von der Deactivator-Entität überwacht. Wenn Sie über einen kleinen **DeactivationScanInterval**-Wert verfügen, könnten Probleme auftreten, bei denen Ihr ServicePackage nicht über ausreichend Zeit für die Aktivierung auf dem Knoten verfügt, weil der Download zu viel Zeit erfordert. Dieses Problem können Sie umgehen, indem Sie [das ServicePackage vorab auf den Knoten herunterladen][p1]. 

> [!NOTE]
> Dadurch kann ein ServicePackage zu einem beliebigen Zeitpunkt zwischen (**DeactivationScanInterval** bis 2 × **DeactivationScanInterval**) + **DeactivationGraceInterval**/**ExclusiveModeDeactivationGraceInterval** deaktiviert werden. 
>

### <a name="replica-close"></a>Schließen von Replikaten
Die Deactivator-Entität zählt die Replikate, die ein ServicePackage enthält. Wenn ein ServicePackage ein Replikat enthält, das geschlossen wird oder ausfällt, erhält der Host einen DecrementUsageCount-Wert. Wenn ein Replikat geöffnet wird, erhält der Host einen IncrementUsageCount-Wert. Der DecrementUsageCount-Wert bedeutet, dass das ServicePackage nun ein Replikat weniger enthält. Wenn die Anzahl auf 0 (null) fällt, wird das ServicePackage für die Deaktivierung eingeplant. Der Zeitpunkt für die Deaktivierung ergibt sich aus **DeactivationGraceInterval**/**ExclusiveModeDeactivationGraceInterval**. 

Beispiel: Angenommen, diese Verringerung erfolgt zum Zeitpunkt „T“, und das ServicePackage soll zum Zeitpunkt 2T+X(**DeactivationGraceInterval**/**ExclusiveModeDeactivationGraceInterval**) deaktiviert werden. Wenn der Host in diesem Zeitraum einen IncrementUsage-Wert erhält, was bedeutet, dass ein Replikat erstellt wurde, wird die Deaktivierung abgebrochen.

> [!NOTE]
>Was diese Konfigurationen im Grunde bedeuten: **DeactivationGraceInterval**/**ExclusiveModeDeactivationGraceInterval:** Der Zeitraum für ein ServicePackage, ein weiteres Replikat zu hosten, nachdem es bereits ein Replikat gehostet hat. 
**DeactivationScanInterval:** Der Mindestzeitraum für ein ServicePackage, ein Replikat zu hosten, wenn es noch nie ein Replikat gehostet hat, z. B., wenn es nicht verwendet wurde.
>

### <a name="ctrlc"></a>STRG+C
Sobald ein ServicePackage **DeactivationGraceInterval**/**ExclusiveModeDeactivationGraceInterval** übergibt und noch kein Replikat hostet, kann die Deaktivierung nicht mehr abgebrochen werden. Das CodePackage empfängt einen STRG+C-Handler, d. h., das die Deaktivierungspipeline muss durchlaufen werden, um den Prozess zu beenden. Wenn in diesem Zeitraum ein neues Replikat für dasselbe ServicePackage platziert wird, wird diese Platzierung nicht erfolgreich ausgeführt, da ein Übergang von Deaktivierung zur Aktivierung nicht möglich ist.

## <a name="cluster-configs"></a>Clusterkonfigurationen

Dabei handelt es sich um Konfigurationen mit Standardwerten, die sich auf die Aktivierung/Deaktivierung auswirken.

### <a name="servicetype"></a>ServiceType
**ServiceTypeDisableFailureThreshold:** Der Standardwert lautet „1“. Dies ist der Schwellenwert für die Anzahl der Fehler. Bei Überschreitung wird FailoverManager (FM) benachrichtigt. FM deaktiviert den Diensttyp auf diesem Knoten und versucht, die Workload auf einem anderen Knoten zu platzieren.
**ServiceTypeDisableGraceInterval:** Der Standardwert beträgt „30 Sekunden“ und beschreibt das Zeitintervall, nach dem der Diensttyp deaktiviert werden kann.
**ServiceTypeRegistrationTimeout:** Der Standardwert beträgt „300 Sekunden“ und beschreibt das Zeitlimit, in dem der ServiceType bei Service Fabric registriert werden muss.

### <a name="activation"></a>Aktivierung
**ActivationRetryBackoffInterval:** Der Standardwert beträgt „10 Sekunden“ und beschreibt das Backoffintervall für jeden Aktivierungsfehler.
**ActivationMaxFailureCount:** Der Standardwert lautet „20“. Dies ist die maximale Anzahl von Versuchen, die Aktivierung nach einem Fehler zu wiederholen, bevor aufgegeben wird. 
**ActivationRetryBackoffExponentiationBase:** Der Standardwert lautet „1,5“.
**ActivationMaxRetryInterval:** Der Standardwert beträgt „3600 Sekunden“ und beschreibt den maximalen Backoffzeitraum für die Aktivierung bei Fehlern.
**CodePackageContinuousExitFailureResetInterval:** Der Standardwert beträgt „300 Sekunden“ und beschreibt das Zeitlimit zum Zurücksetzen der Anzahl der kontinuierlichen Beendigungsfehler für CodePackage.

### <a name="download"></a>Download
**DeploymentRetryBackoffInterval:** Standardwert: 10 Backoffintervall für den Fehler bei der Bereitstellung.
**DeploymentMaxRetryInterval:** Der Standardwert beträgt „3600 Sekunden“ und beschreibt den maximalen Backoffzeitraum für die Bereitstellung bei Fehlern.
**DeploymentMaxFailureCount:** Der Standardwert lautet „20“. Die Anwendungsbereitstellung wird DeploymentMaxFailureCount Mal wiederholt, bevor ein Fehler für die Bereitstellung der betreffenden Anwendung auf dem Knoten auftritt.

### <a name="deactivation"></a>Deaktivierung
**DeactivationScanInterval:** Der Standardwert beträgt „600 Sekunden“ und beschreibt den Mindestzeitraum für ein ServicePackage, ein Replikat zu hosten, wenn es noch nie ein Replikat gehostet hat, z. B., wenn es nicht verwendet wurde.
**DeactivationGraceInterval:** Der Standardwert beträgt „60 Sekunden“ und beschreibt den Zeitraum für ein ServicePackage, ein weiteres Replikat zu hosten, nachdem es bereits ein Replikat gehostet hat, wenn das Prozessmodell **Shared** vorliegt.
**ExclusiveModeDeactivationGraceInterval:** Der Standardwert beträgt „1 Sekunden“ und beschreibt den Zeitraum für ein ServicePackage, ein weiteres Replikat zu hosten, nachdem es bereits ein Replikat gehostet hat, wenn das Prozessmodell **Exclusive** vorliegt.

## <a name="next-steps"></a>Nächste Schritte
[Packen][a3] und Vorbereiten einer Anwendung für die Bereitstellung

[Bereitstellen und Entfernen von Anwendungen][a4] Dieser Artikel beschreibt, wie Sie Anwendungsinstanzen mithilfe von PowerShell verwalten.

<!--Link references--In actual articles, you only need a single period before the slash-->
[a1]: service-fabric-application-model.md
[a2]: service-fabric-hosting-model.md
[a3]: service-fabric-package-apps.md
[a4]: service-fabric-deploy-remove-applications.md

[p1]: /powershell/module/servicefabric/copy-servicefabricservicepackagetonode
