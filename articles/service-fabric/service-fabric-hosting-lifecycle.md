---
title: Aktivierungs- und Deaktivierungslebenszyklus des Azure Service Fabric-Hostings
description: Erfahren Sie mehr über den Lebenszyklus einer Anwendung und eines ServicePackage auf einem Knoten.
author: tugup
ms.topic: conceptual
ms.date: 05/01/2020
ms.author: tugup
ms.openlocfilehash: d8585d0b39e4a4ef9cf77f40ea878ddb47bcb0de
ms.sourcegitcommit: beacda0b2b4b3a415b16ac2f58ddfb03dd1a04cf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/31/2020
ms.locfileid: "97831821"
---
# <a name="azure-service-fabric-hosting-life-cycle"></a>Azure Service Fabric-Hostinglebenszyklus

Dieser Artikel enthält eine Übersicht über die Ereignisse, die in Azure Service Fabric auftreten, wenn eine Anwendung auf einem Knoten aktiviert wird. Es werden verschiedene Clusterkonfigurationen erläutert, die das Verhalten steuern.

Bevor Sie fortfahren, müssen Sie die Konzepte und Beziehungen kennen, die unter [Modellieren von Anwendungen in Service Fabric][a1] erläutert werden. 

> [!NOTE]
> In diesem Artikel werden einige Begriffe auf spezielle Weise verwendet. Sofern nicht anders angegeben:
>
> - *Replikat* bezieht sich sowohl auf ein Replikat eines zustandsbehafteten Diensts als auch auf eine Instanz eines zustandslosen Diensts.
> - *CodePackage* wird gleichbedeutend mit dem ServiceHost-Prozess verwendet, der einen ServiceType registriert. Es hostet Replikate der Dienste dieses ServiceType.
>

## <a name="activate-an-applicationpackage-or-servicepackage"></a>Aktivieren eines ApplicationPackage oder ServicePackage

So aktivieren Sie ein ApplicationPackage oder ServicePackage:

1. Laden Sie das ApplicationPackage herunter (z. B. *ApplicationManifest.xml*).
2. Richten Sie eine Umgebung für die Anwendung ein. Die Schritte umfassen beispielsweise das Erstellen von Benutzern.
3. Starten Sie die Nachverfolgung der Anwendung für die Deaktivierung.
4. Laden Sie das ServicePackage herunter (z. B. *ServiceManifest.xml*, den Code, die Konfigurationsdateien und die Datenpakete).
5. Richten Sie eine Umgebung für das ServicePackage ein. Die Schritte umfassen beispielsweise das Einrichten einer Firewall und das Zuordnen von Ports für Endpunkte.
6. Starten Sie die Nachverfolgung des ServicePackage für die Deaktivierung.
7. Starten Sie SetupEntryPoint für die CodePackages, und warten Sie, bis der Vorgang abgeschlossen ist.
8. Starten Sie MainEntryPoint für die CodePackages.

### <a name="servicetype-blocklisting"></a>ServiceType-Sperrliste
`ServiceTypeDisableFailureThreshold` bestimmt die Anzahl zulässiger Aktivierungs-, Download- und CodePackage-Fehler. Nachdem der Schwellenwert erreicht wurde, wird der ServiceType für die Sperrliste geplant. Der erste Aktivierungs- oder Downloadfehler bzw. Absturz von CodePackage löst die Planung der Aufnahme von ServiceType in die Sperrliste aus. 

Die `ServiceTypeDisableGraceInterval`-Konfiguration bestimmt das Karenzintervall, bevor ServiceType auf dem Knoten blockiert wird. Während dieser Prozess abläuft, werden Aktivierung, Download und Neustart des CodePackage parallel erneut versucht. „Versucht“ bedeutet etwa, dass der erneute Start von CodePackage nach dem Absturz geplant wird oder dass Service Fabric erneut versucht, die Pakete herunterzuladen.

Wenn ServiceType blockiert ist, wird ein Integritätsfehler angezeigt: `'System.Hosting' reported Error for property 'ServiceTypeRegistration:ServiceType'. The ServiceType was disabled on the node.`

ServiceType wird auf dem Knoten erneut aktiviert, wenn eines der folgenden Ereignisse eintritt:
- Die Aktivierung ist erfolgreich. Wenn sie fehlschlägt, erreicht sie andernfalls die maximale Anzahl von Wiederholungen (`ActivationMaxFailureCount`).
- Der Download ist erfolgreich. Wenn er fehlschlägt, erreicht er andernfalls die maximale Anzahl von Wiederholungen (`DeploymentMaxFailureCount`).
- Ein abgestürztes CodePackage wird wieder gestartet und registriert erfolgreich den ServiceType.

`ActivationMaxFailureCount` und `DeploymentMaxFailureCount` geben die maximale Anzahl von Versuchen an, die Service Fabric vornimmt, um eine Anwendung auf einem Knoten zu aktivieren oder herunterzuladen. Nachdem der Höchstwert erreicht wurde, aktiviert Service Fabric den ServiceType erneut für die Aktivierung. 

Diese Schwellenwerte bieten dem Dienst eine weitere Möglichkeit für die Aktivierung. Wenn die Aktivierung erfolgreich ist, wird das Problem automatisch behoben. 

Ein neu platziertes oder aktiviertes Replikat kann einen neuen Aktivierungs- oder Downloadvorgang auslösen. Diese Aktion wird entweder erfolgreich ausgeführt, oder es wird erneut die Service Type-Sperrliste auslöst.

> [!NOTE]
> Ein abstürzenden CodePackage, das keinen ServiceType registriert, wirkt sich nicht auf den ServiceType aus. Nur ein abstürzendes CodePackage, das ein Replikat hostet, wirkt sich auf den ServiceType aus.
>

### <a name="codepackage-crash"></a>Absturz von CodePackage
Beim Absturz eines CodePackage wartet Service Fabric, bevor es erneut gestartet wird. Dieses als „Backoff“ bezeichnete Warten erfolgt unabhängig davon, ob das CodePackage einen Typ bei der Service Fabric-Runtime registriert hat.

Der Backoffwert lautet `Min(RetryTime, ActivationMaxRetryInterval)`. Der Wert wird auf der Grundlage der `ActivationRetryBackoffExponentiationBase`-Konfigurationseinstellung konstant, linear oder exponentiell erhöht:

- **Konstant**: Wenn `ActivationRetryBackoffExponentiationBase == 0`, dann `RetryTime = ActivationRetryBackoffInterval`.
- **Linear**:  Wenn `ActivationRetryBackoffExponentiationBase == 0`, dann `RetryTime = ContinuousFailureCount ActivationRetryBackoffInterval`, wobei `ContinuousFailureCount` die Häufigkeit ist, mit der ein CodePackage abstürzt oder nicht aktiviert werden kann.
- **Exponentiell**: `RetryTime = (ActivationRetryBackoffInterval in seconds) * (ActivationRetryBackoffExponentiationBase ^ ContinuousFailureCount)`.
    
Sie können das Verhalten steuern, indem Sie die Werte ändern. Wenn Sie z. B. mehrere schnelle Neustartversuche durchführen möchten, können Sie lineare Werte verwenden, indem Sie `ActivationRetryBackoffExponentiationBase` auf `0` und `ActivationRetryBackoffInterval` auf `10` festlegen. Wenn ein CodePackage abstürzt, beginnt das Intervall also nach 10 Sekunden. Wenn das Paket weiterhin abstürzt, ändert sich das Backoff in 20, 30, 40 Sekunden usw., bis die CodePackage-Aktivierung erfolgreich war oder das CodePackage deaktiviert wurde. 
    
Die maximale Zeit, die Service Fabric nach einem Fehler wartet, wird durch `ActivationMaxRetryInterval` vorgegeben.
    
Wenn Ihr CodePackage abstürzt und erneut aktiviert wird, muss es für die durch `CodePackageContinuousExitFailureResetInterval` angegebene Zeitspanne aktiv bleiben. Nach diesem Intervall wird das CodePackage von Service Fabric als fehlerfrei angesehen. Service Fabric überschreibt dann den vorherigen Fehlerintegritätsbericht als fehlerfrei und setzt `ContinuousFailureCount` zurück.

### <a name="codepackage-not-registering-servicetype"></a>CodePackage registriert ServiceType nicht
Wenn ein CodePackage aktiv bleibt und von ihm erwartet wird, dass es einen ServiceType registriert, was aber nicht der Fall ist, generiert Service Fabric einen Warnungsintegritätsbericht nach `ServiceTypeRegistrationTimeout`. Der Bericht gibt an, dass ServiceType nicht innerhalb des erwarteten Zeitraums registriert wurde.

### <a name="activation-failure"></a>Aktivierungsfehler
Wenn Service Fabric während der Aktivierung einen Fehler findet, wird wie bei einem CodePackage-Absturz immer ein lineares Backoff verwendet. Der Aktivierungsvorgang wird nach Wiederholungsversuchen in diesen Intervallen abgebrochen: (0 + 10 + 20 + 30 + 40) = 100 Sekunden. (Der erste Wiederholungsversuch erfolgt sofort.) Nach dieser Sequenz wird die Aktivierung nicht mehr wiederholt.
    
Der maximal zulässige Aktivierungsbackoff kann `ActivationMaxRetryInterval` betragen. Die Wiederholungsversuche können `ActivationMaxFailureCount` sein.

### <a name="download-failure"></a>Fehler beim Herunterladen
Service Fabric verwendet immer ein lineares Backoff, wenn während des Downloads ein Fehler auftritt. Der Aktivierungsvorgang wird nach Wiederholungsversuchen in diesen Intervallen abgebrochen: (0 + 10 + 20 + 30 + 40) = 100 Sekunden. (Der erste Wiederholungsversuch erfolgt sofort.) Nach dieser Sequenz wird der Download nicht mehr wiederholt. 

Der lineare Backoff für einen Download entspricht `ContinuousFailureCount` * `DeploymentRetryBackoffInterval`. Der maximale Backoff kann `DeploymentMaxRetryInterval` sein. Ebenso wie Aktivierungen können Downloadvorgänge bis zum Grenzwert `ActivationMaxFailureCount` Wiederholungsversuche ausführen.

> [!NOTE]
> Bevor Sie diese Einstellungen ändern, beachten Sie die folgenden Beispiele:
>
>* Wenn das CodePackage weiterhin abstürzt und Backoffs verursacht, wird der ServiceType deaktiviert. Wenn die Aktivierungskonfiguration jedoch für schnelle Neustarts eingerichtet ist, kann das CodePackage mehrmals aktiviert werden, bevor der ServiceType in die Sperrliste aufgenommen wird. 
>
>    Beispiel: Angenommen Ihr CodePackage registriert den ServiceType mit Service Fabric und stürzt dann ab. In diesem Fall wird der Zeitraum `ServiceTypeDisableGraceInterval` abgebrochen, nachdem das Hosting eine Typregistrierung empfangen hat. Dieser Prozess kann wiederholt werden, bis das CodePackage einen Wert verwendet, der größer ist als der `ServiceTypeDisableGraceInterval`-Zeitraum ist. Anschließend wird der ServiceType auf dem Knoten in die Sperrliste aufgenommen. Das Eintragen des ServiceType in die Sperrliste kann länger als erwartet dauern.
>
>* Wenn das Service Fabric-System bei Aktivierungen ein Replikat auf einem Knoten platzieren muss, fordert ReconfigurationAgent (RA) das Hostingsubsystem dazu auf, die Anwendung zu aktivieren. Die Aktivierungsanforderung wird alle 15 Sekunden wiederholt. (Die Dauer wird durch die `RAPMessageRetryInterval`-Konfigurationseinstellung vorgegeben.) Damit das Service Fabric-System weiß, dass der ServiceType in die Sperrliste eingetragen wurde, muss der Aktivierungsvorgang des Hosts länger als das Wiederholungsintervall und `ServiceTypeDisableGraceInterval` sein. 
>
>    Nehmen Sie beispielsweise an, `ActivationMaxFailureCount` des Clusters ist auf 5 festgelegt, und `ActivationRetryBackoffInterval` wird auf 1 Sekunde festgelegt. In diesem Fall wird der Aktivierungsvorgang nach Intervallen von (0 + 1 + 2 + 3 + 4) = 10 Sekunden abgebrochen. (Der erste Wiederholungsversuch erfolgt sofort.) Nach dieser Sequenz bricht das Hosting die Wiederholungsversuche ab. Der Aktivierungsvorgang wird beendet und nicht nach 15 Sekunden wiederholt. 
>
>    Service Fabric hat alle zulässigen Wiederholungsversuche innerhalb von 15 Sekunden ausgeschöpft. Jede Wiederholung des Neukonfigurations-Agents erstellt einen neuen Aktivierungsvorgang im Hostingsubsystem, und das Muster wird wiederholt. Daher wird der ServiceType auf dem Knoten nie in die Sperrliste eingetragen. Da der ServiceType auf dem Knoten nicht in die Sperrliste gelangt, wird das Replikat nicht verschoben und auf einem anderen Knoten versucht.
> 

## <a name="deactivation"></a>Deaktivierung

Wenn ein ServicePackage auf einem Knoten aktiviert ist, wird es für die Deaktivierung überwacht. Die Deaktivierung funktioniert auf zwei Arten:

- **Regelmäßige Deaktivierung**:  Nach jedem `DeactivationScanInterval` erfolgt eine Prüfung auf ServicePackages durch das System, die *nie* ein Replikat gehostet haben. Diese werden dann als Kandidaten für die Deaktivierung markiert.
- **ReplicaClose-Deaktivierung**: Wenn ein Replikat geschlossen wird, ruft der Deaktivator einen `DecrementUsageCount`-Wert ab. Die Anzahl liegt bei 0, wenn das ServicePackage kein Replikat hostet, sodass das ServicePackage ein Kandidat für die Deaktivierung ist.

Der Aktivierungsmodus bestimmt, wann Kandidaten für die Deaktivierung geplant sind. Im freigegebenen Modus werden Kandidaten für die Deaktivierung nach dem `DeactivationGraceInterval` geplant. Im exklusiven Modus werden sie nach dem `ExclusiveModeDeactivationGraceInterval` geplant. Wenn in diesem Zeitraum ein neues Replikat eingeht, erfolgt ein Abbruch der Deaktivierung. 

Weitere Informationen finden Sie unter [Exklusiver Modus und freigegebener Modus][a2].

### <a name="periodic-deactivation"></a>Regelmäßige Deaktivierung
Im Folgenden werden einige Beispiele für regelmäßige Deaktivierungen erläutert:

* **Beispiel 1:** Nehmen wir an, dass der Deaktivator zum Zeitpunkt T (`DeactivationScanInterval`) eine Überprüfung startet. Die nächste Überprüfung erfolgt um „2T“. Angenommen, eine ServicePackage-Aktivierung trat um „T+1“ auf. Dieses ServicePackage hostet kein Replikat, daher muss es deaktiviert werden. 

    Damit das ServicePackage ein Kandidat für die Deaktivierung sein kann, darf es mindestens über den Zeitraum von „T“ kein Replikat hosten. Es kann um „2T + 1“ deaktiviert werden. Daher wird die Überprüfung um „2T“ das ServicePackage nicht als Kandidat für die Deaktivierung identifizieren. 

    Der nächste Deaktivierungszyklus um „3T“ plant dieses ServicePackage für die Deaktivierung ein, weil das Paket dann über den Zeitraum von „T“ kein Replikat aufgewiesen hat.  

* **Beispiel 2:** Angenommen ein ServicePackage wird zum Zeitpunkt „T - 1“ aktiviert und der Deaktivator startet um „T“ eine Überprüfung. Das ServicePackage hostet kein Replikat. Dann wird dieses ServicePackage bei der nächsten Überprüfung um „2T“ als Kandidat für die Deaktivierung eingestuft und daher für die Deaktivierung geplant.  

* **Beispiel 3:** Angenommen ein ServicePackage wird um „T - 1“ aktiviert und der Deaktivator startet um „T“ eine Überprüfung. Das ServicePackage hostet noch kein Replikat. Jedoch wird um „T + 1“ ein Replikat platziert. Das Hosting erhält einen `IncrementUsageCount`-Wert, d. h. ein Replikat wird erstellt. 

    Dieses ServicePackage wird also um „2T“ nicht für die Deaktivierung geplant. Da das Paket ein Replikat enthält, folgt die Deaktivierung der ReplicaClose-Logik, wie im nächsten Abschnitt in diesem Artikel erläutert wird.

* **Beispiel 4:** Nehmen wir an, Ihr ServicePackage ist 10 GB groß. Da das Paket groß ist, dauert es eine Weile, bis es auf den Knoten heruntergeladen wird. Wenn eine Anwendung aktiviert wird, wird ihr Lebenszyklus vom Deaktivator nachverfolgt. Wenn das `DeactivationScanInterval` auf einen kleinen Wert festgelegt ist, kann das ServicePackage aufgrund der zum Herunterladen benötigte Zeit nicht auf dem Knoten aktiviert werden. Dieses Problem können Sie umgehen, indem Sie [das ServicePackage vorab auf den Knoten herunterladen][p1] oder das `DeactivationScanInterval` erhöhen. 

> [!NOTE]
> Ein ServicePackage kann zwischen (`DeactivationScanInterval` und 2 * `DeactivationScanInterval`) + `DeactivationGraceInterval`/`ExclusiveModeDeactivationGraceInterval` deaktiviert werden. 
>

### <a name="replicaclose-deactivation"></a>ReplicaClose-Deaktivierung

> [!NOTE]
> Dieser Abschnitt bezieht sich auf die folgenden Konfigurationseinstellungen:
> - **DeactivationGraceInterval**/**ExclusiveModeDeactivationGraceInterval:** Der Zeitraum für ein ServicePackage, um ein weiteres Replikat zu hosten, wenn es bereits ein Replikat gehostet hat. 
> - **DeactivationScanInterval:** Der Mindestzeitraum für ein ServicePackage, um ein Replikat zu hosten, wenn es noch *nie* ein Replikat gehostet hat, also noch nicht verwendet wurde.
>

Das System hält die Anzahl der Replikate nach, die ein ServicePackage enthält. Wenn ein ServicePackage ein Replikat enthält, das geschlossen wird oder ausfällt, erhält der Host einen `DecrementUsageCount`-Wert. Wenn ein Replikat geöffnet wird, erhält der Host einen `IncrementUsageCount`-Wert. 

Das Dekrement gibt an, dass die Anzahl der Replikate, die das ServicePackage hostet, um ein Replikat reduziert wurde. Wenn die Anzahl auf 0 fällt, wird das ServicePackage für die Deaktivierung geplant. Die Zeit, nach der es deaktiviert wird, ist `DeactivationGraceInterval`/`ExclusiveModeDeactivationGraceInterval`. 

Nehmen wir beispielsweise an, dass ein Dekrement um T erfolgt und dass das ServicePackage für die Deaktivierung um „2T + X“ (`DeactivationGraceInterval`/`ExclusiveModeDeactivationGraceInterval`) geplant ist. Wenn der Host in diesem Zeitraum einen `IncrementUsage`-Wert erhält, weil ein Replikat erstellt wurde, wird die Deaktivierung abgebrochen.

### <a name="ctrl--c"></a>STRG+C
Wenn ein ServicePackage das `DeactivationGraceInterval`/`ExclusiveModeDeactivationGraceInterval` übergibt und immer noch kein Replikat hostet, kann die Deaktivierung nicht abgebrochen werden. CodePackages erhalten einen STRG+C-Handler. Nun muss die Deaktivierungspipeline beendet werden, um den Prozess zu beenden. 

Wenn in diesem Zeitraum ein neues Replikat für dasselbe ServicePackage platziert wird, wird diese Platzierung nicht erfolgreich ausgeführt, da ein Übergang des Prozesses von der Deaktivierung zur Aktivierung nicht möglich ist.

## <a name="cluster-configurations"></a>Clusterkonfigurationen

In diesem Abschnitt werden Konfigurationen mit Standardeinstellungen aufgeführt, die sich auf die Aktivierung und Deaktivierung auswirken.

### <a name="servicetype"></a>ServiceType
- **ServiceTypeDisableFailureThreshold:** Standardwert: 1. Dies ist der Schwellenwert für die Anzahl der Fehler. Bei Überschreitung wird FailoverManager benachrichtigt, um den Diensttyp auf diesem Knoten zu deaktivieren und einen anderen Knoten für die Platzierung zu verwenden.
- **ServiceTypeDisableGraceInterval:** Standardwert: 30 Sekunden. Das Zeitintervall, nach dem der Diensttyp deaktiviert werden kann.
- **ServiceTypeRegistrationTimeout:** Standardwert: 300 Sekunden. Das Timeout, vor dem der ServiceType bei Service Fabric registriert werden muss.

### <a name="activation"></a>Aktivierung
- **ActivationRetryBackoffInterval:** Standardwert: 10 Sekunden. Das Backoffintervall für jeden Aktivierungsfehler.
- **ActivationMaxFailureCount:** Standardwert: 20. Die maximale Anzahl von Versuchen, die Aktivierung nach einem Fehler zu wiederholen, bevor keine weiteren Versuche mehr stattfinden. 
- **ActivationRetryBackoffExponentiationBase:** Standardwert: 1,5.
- **ActivationMaxRetryInterval:** Standardwert: 3.600 Sekunden. Maximales Backoffwiederholungsintervall für die Aktivierung nach Fehlern.
- **CodePackageContinuousExitFailureResetInterval:** Standardwert: 300 Sekunden. Das Timeoutintervall, nach dem die Anzahl der kontinuierlichen Beendigungsfehler für das CodePackage zurückgesetzt wird.

### <a name="download"></a>Download
- **DeploymentRetryBackoffInterval:** Standardwert: 10. Das Backoffintervall für den Bereitstellungsfehler.
- **DeploymentMaxRetryInterval:** Standardwert: 3.600 Sekunden. Das maximale Backoffintervall für die Bereitstellung nach Fehlern.
- **DeploymentMaxFailureCount:** Standardwert: 20. Die Anwendungsbereitstellung wird `DeploymentMaxFailureCount` Mal wiederholt, bevor ein Fehler für die Bereitstellung der betreffenden Anwendung auf dem Knoten auftritt.

### <a name="deactivation"></a>Deaktivierung
- **DeactivationScanInterval:** Standardwert: 600 Sekunden. Der Mindestzeitraum für ein ServicePackage, um ein Replikat zu hosten, wenn es noch nie ein Replikat gehostet hat (also nicht verwendet wird).
- **DeactivationGraceInterval:** Standardwert: 60 Sekunden. In einem *freigegebenen* Prozessmodell der Zeitraum für ein ServicePackage, um erneut ein weiteres Replikat zu hosten, nachdem es bereits ein Replikat gehostet hat.
- **ExclusiveModeDeactivationGraceInterval:** Standardwert: 1 Sekunde In einem *exklusiven* Prozessmodell der Zeitraum für ein ServicePackage, um erneut ein weiteres Replikat zu hosten, nachdem es bereits ein Replikat gehostet hat.

## <a name="next-steps"></a>Nächste Schritte

- [Packen][a3] und Vorbereiten einer Anwendung für die Bereitstellung
- [Bereitstellen und Entfernen von Anwendungen][a4] in PowerShell.

<!--Link references--In actual articles, you only need a single period before the slash-->
[a1]: service-fabric-application-model.md
[a2]: service-fabric-hosting-model.md
[a3]: service-fabric-package-apps.md
[a4]: service-fabric-deploy-remove-applications.md

[p1]: /powershell/module/servicefabric/copy-servicefabricservicepackagetonode
