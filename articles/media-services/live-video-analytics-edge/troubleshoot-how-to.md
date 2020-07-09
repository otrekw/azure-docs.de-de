---
title: 'Troubleshooting von Live Video Analytics in IoT Edge: Azure'
description: Dieser Artikel behandelt Schritte zur Problembehandlung für Live Video Analytics in IoT Edge.
ms.topic: how-to
ms.date: 05/24/2020
ms.openlocfilehash: c235dd27da1d370531c1668c40586d4ae479aec7
ms.sourcegitcommit: 223cea58a527270fe60f5e2235f4146aea27af32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2020
ms.locfileid: "84260443"
---
# <a name="troubleshoot-live-video-analytics-on-iot-edge"></a>Troubleshooting Live Video Analytics in IoT Edge

Dieser Artikel behandelt Schritte zur Problembehandlung für Live Video Analytics in IoT Edge.

## <a name="troubleshoot-deployment-issues"></a>Problembehandlung bei Bereitstellungsproblemen

### <a name="diagnostics"></a>Diagnose

Im Rahmen der Bereitstellung von Live Video Analytics richten Sie Azure-Ressourcen wie den IoT Hub und das IoT Edge-Gerät ein. Vergewissern Sie sich im ersten Schritt zur Problemdiagnose immer, dass das Edge-Gerät ordnungsgemäß eingerichtet ist, indem die diese Anweisungen befolgen:

1. [Führen Sie den Befehl ‚check‘ aus](https://docs.microsoft.com/azure/iot-edge/troubleshoot#run-the-check-command)
1. [Überprüfen Sie Ihre IoT Edge-Version](https://docs.microsoft.com/azure/iot-edge/troubleshoot#check-your-iot-edge-version)
1. [Überprüfen Sie den Status des IoT Edge-Sicherheits-Managers und seiner Protokolle](https://docs.microsoft.com/azure/iot-edge/troubleshoot#check-the-status-of-the-iot-edge-security-manager-and-its-logs)
1. [Zeigen Sie die Nachrichten an, die den IoT Edge-Hub durchlaufen](https://docs.microsoft.com/azure/iot-edge/troubleshoot#view-the-messages-going-through-the-iot-edge-hub)
1. [Führen Sie einen Neustart der Container aus](https://docs.microsoft.com/azure/iot-edge/troubleshoot#restart-containers)
1. [Überprüfen Sie Ihre Konfigurationsregeln für Firewall und Ports](https://docs.microsoft.com/azure/iot-edge/troubleshoot#check-your-firewall-and-port-configuration-rules)

### <a name="pre-deployment-issues"></a>Probleme vor der Bereitstellung

Wenn die Edge-Infrastruktur in Ordnung ist, können Sie nach Problemen bei der Bereitstellungsmanifestdatei suchen. Um Live Video Analytics im IoT Edge-Modul auf dem Edge-Gerät neben allen anderen IoT-Modulen bereitzustellen, verwenden Sie ein Bereitstellungsmanifest, das den Edge-Hub, den Edge-Agent und weitere Module mit ihren Eigenschaften enthält. Wenn der JSON-Code nicht das korrekte Format aufweist, erhalten Sie einen Fehler wie den unten dargestellten: 

```
az iot edge set-modules --hub-name <iot-hub-name> --device-id lva-sample-device --content <path-to-deployment_manifest.json>
```

Fehler beim Analysieren des JSON-Codes aus Datei ‚<deployment manifest.json>‘ für das Argument ‚content‘ mit dieser Ausnahme: „Zusätzliche Daten: Zeile 101, Spalte 1 (Zeichen 5325)“

Wenn dieser Fehler auftritt, empfiehlt es sich, die JSON-Datei auf fehlende Klammern oder andere Probleme in der Dateistruktur zu überprüfen. Sie können einen Client wie [Notepad++ mit dem JSON Viewer-Plug-In](https://riptutorial.com/notepadplusplus/example/18201/json-viewer) oder ein Onlinetool wie https://jsonformatter.curiousconcept.com/ verwenden, um die Dateistruktur zu überprüfen.

### <a name="deployment--diagnose-with-media-graph-direct-methods"></a>Bereitstellung: Diagnose mit direkten Mediengraphmethoden 

Sobald die Live Video Analytics im IoT Edge-Modul ordnungsgemäß bereitgestellt sind, können Sie den Mediengraph durch Aufrufen [direkter Methoden](direct-methods.md) erstellen und ausführen. Sie können das Portal verwenden, um die Diagnose des Mediengraphs mithilfe direkter Methoden auszuführen:

1. Navigieren Sie im Portal zu dem IoT Hub, der mit Ihrem Edge-Gerät verbunden ist.
    1. Suchen Sie auf dem Blatt des IoT-Hubs nach der automatischen Geräteverwaltung > IoT Edge.
    1. Wenn Sie auf „IoT Edge“ klicken, sollte eine Liste mit Edge-Geräten angezeigt werden. Wählen Sie das Gerät aus, für das Sie die Diagnose ausführen möchten.
         
        ![Edge-Geräte](./media/troubleshoot-how-to/lva-sample-device.png)
    1. Überprüfen Sie, ob der Antwortcode „200-OK“ ist. Es gibt verschiedene andere Antwortcodes für die [IoT Edge Runtime](https://docs.microsoft.com/azure/iot-edge/iot-edge-runtime), wie etwa:
        1. 400 – Die Bereitstellungskonfiguration ist falsch formatiert oder ungültig.
        1. 417 – Für das Gerät ist keine Bereitstellungskonfiguration festgelegt.
        1. 412 – Die Schemaversion der Bereitstellungskonfiguration ist ungültig.
        1. 406 – das IoT Edge-Gerät ist offline oder sendet keine Statusberichte.
        1. 500 – in der IoT Edge-Runtime ist ein Fehler aufgetreten.
    1. Wenn Sie auf das Gerät klicken, sollte außerdem die Liste der erwarteten bereitgestellten IoT Edge-Module mit deren Status angezeigt werden.
    1. Wenn in den Spalten „In Bereitstellung angegeben“ und „Vom Gerät gemeldet“ der Wert „Ja“ angezeigt wird, können Sie direkte Methoden für die Live Video Analytics im IoT Edge-Modul aufrufen. Klicken Sie auf das Modul, dann gelangen Sie zu einem Bildschirm, auf dem Sie die gewünschten und gemeldeten Eigenschaften überprüfen und direkte Methoden aufrufen können. 
        1. Wenn Sie die gemeldeten und gewünschten Eigenschaften überprüfen, können Sie besser nachvollziehen, ob die Moduleigenschaften mit der Bereitstellung synchronisiert wurden. Wenn dies nicht der Fall ist, können Sie einen Neustart Ihres Edge-Geräts ausführen. 
        1. Verwenden Sie den Leitfaden für [direkte Methoden](direct-methods.md), um einige Methoden aufzurufen, insbesondere einfache wie GraphTopologyList. Der Leitfaden gibt darüber hinaus die erwartete Nutzlast für Anforderungen und Antworten sowie Fehlercodes an. Nachdem die einfachen direkten Methoden erfolgreich ausgeführt wurden, können Sie darauf vertrauen, dass das Live Video Analytics-Edge-Modul ordnungsgemäß funktioniert.
        
        ![Direkte Methode](./media/troubleshoot-how-to/direct-method.png) 
1. Wenn Sie einen 501-Statuscode empfangen, überprüfen Sie, ob der Name der direkten Methode korrekt ist. Wenn der Methodenname und die Anforderungsnutzlast korrekt sind, sollten die Ergebnisse vom Erfolgscode „= 200“ begleitet werden. Wenn die Angabe der Anforderungsnutzlast ungenau ist, erhalten Sie den Status „= 400“ und eine Antwortnutzlast, die den Fehlercode und eine Meldung angibt, die Sie bei der Diagnose des Problems bei Ihrem direkten Methodenaufrufe unterstützen soll. 

### <a name="post-deployment--diagnose-logs-for-issues-during-run"></a>Nach der Bereitstellung: Diagnose der Protokolle zu Problemen während der Ausführung 

Die Containerprotokolle für das Edge-Modul sollten Diagnoseinformationen<!--<todo:add link to diagnostics doc>--> aufweisen, die Sie beim Debuggen der Probleme zur Laufzeit der Module unterstützen sollen. Sie können [die Containerprotokolle auf Probleme überprüfen](https://docs.microsoft.com/azure/iot-edge/troubleshoot#check-container-logs-for-issues) und die Diagnose eigenständig durchführen. Wenn jedoch alle oben aufgeführten Überprüfungen abgearbeitet wurden und immer noch Probleme auftreten, sammeln Sie [mit dem Befehl ‚support bundle‘](https://docs.microsoft.com/azure/iot-edge/troubleshoot#gather-debug-information-with-support-bundle-command) Protokolle vom IoT Edge-Gerät zur weiteren Analyse durch das Azure-Team. Sie können sich zwecks Support [an uns wenden](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) und die gesammelten Protokolle übermitteln.

## <a name="common-error-resolutions"></a>Lösungen für allgemeine Fehler

Live Video Analytics wird als IoT Edge-Modul auf dem Edge-Gerät bereitgestellt und funktioniert zusammen mit dem IoT Edge-Agent und den Hub-Modulen. Einige der häufigeren Fehler, die bei der Bereitstellung von Live Video Analytics auftreten, sind auf Probleme mit der zugrunde liegenden IoT-Infrastruktur zurückzuführen. Dies sind einige häufige Fehler, die beim IoT Edge-Agent und dem Hub auftreten können:

1. [Der IoT Edge-Agent wird nach ungefähr einer Minute beendet](https://docs.microsoft.com/azure/iot-edge/troubleshoot-common-errors#iot-edge-agent-stops-after-about-a-minute).
1. [Der IoT Edge-Agent kann nicht auf das Image eines Moduls zugreifen (403)](https://docs.microsoft.com/azure/iot-edge/troubleshoot-common-errors#iot-edge-agent-cant-access-a-modules-image-403).
1. [Das Edge-Agent-Modul meldet „empty config file“, und auf dem Gerät werden keine Module gestartet](https://docs.microsoft.com/azure/iot-edge/troubleshoot-common-errors#edge-agent-module-reports-empty-config-file-and-no-modules-start-on-the-device).
1. [IoT Edge-Hub kann nicht gestartet werden](https://docs.microsoft.com/azure/iot-edge/troubleshoot-common-errors#iot-edge-hub-fails-to-start).
1. [Fehler beim Daemon für die IoT Edge-Sicherheit aufgrund eines ungültigen Hostnamens](https://docs.microsoft.com/azure/iot-edge/troubleshoot-common-errors#iot-edge-security-daemon-fails-with-an-invalid-hostname).
1. [Live Video Analytics oder andere benutzerdefinierte IoT Edge-Module können keine Nachricht an den Edge-Hub senden und geben einen 404-Fehler aus](https://docs.microsoft.com/azure/iot-edge/troubleshoot-common-errors#iot-edge-module-fails-to-send-a-message-to-edgehub-with-404-error).
1. [IoT Edge-Modul wird erfolgreich bereitgestellt und verschwindet dann vom Gerät](https://docs.microsoft.com/azure/iot-edge/troubleshoot-common-errors#iot-edge-module-deploys-successfully-then-disappears-from-device).

### <a name="edge-set-up-script-issues"></a>Probleme beim Edge-Setupskript

Im Rahmen der Dokumentation haben wir ein [Setupskript](https://github.com/Azure/live-video-analytics/tree/master/edge/setup) zur Verfügung gestellt, um Edge- und Cloud-Ressourcen bereitzustellen und den Einstieg in Live Video Analytics Edge zu erleichtern. In diesem Abschnitt haben wir Fehler zusammengestellt, auf die Sie möglicherweise bei der Skriptausführung stoßen können, und geben Hinweise zum Debuggen.

Skripts werden teilweise ausgeführt und einige Ressourcen erstellt, doch dann schlägt die Ausführung mit folgender Meldung fehl:

```
registering device...

Unable to load extension 'eventgrid: unrecognized kwargs: ['min_profile']'. Use --debug for more information.
The command failed with an unexpected error. Here is the traceback:

No module named 'azure.mgmt.iothub.iot_hub_client'
Traceback (most recent call last):
File "/opt/az/lib/python3.6/site-packages/knack/cli.py", line 215, in invoke
  cmd_result = self.invocation.execute(args)
File "/opt/az/lib/python3.6/site-packages/azure/cli/core/commands/__init__.py", line 631, in execute
  raise ex
File "/opt/az/lib/python3.6/site-packages/azure/cli/core/commands/__init__.py", line 695, in _run_jobs_serially
  results.append(self._run_job(expanded_arg, cmd_copy))
File "/opt/az/lib/python3.6/site-packages/azure/cli/core/commands/__init__.py", line 688, in _run_job
  six.reraise(*sys.exc_info())
File "/opt/az/lib/python3.6/site-packages/six.py", line 693, in reraise
  raise value
File "/opt/az/lib/python3.6/site-packages/azure/cli/core/commands/__init__.py", line 665, in _run_job
  result = cmd_copy(params)
File "/opt/az/lib/python3.6/site-packages/azure/cli/core/commands/__init__.py", line 324, in __call__
  return self.handler(*args, **kwargs)
File "/opt/az/lib/python3.6/site-packages/azure/cli/core/__init__.py", line 574, in default_command_handler
  return op(**command_args)
File "/home/.azure/cliextensions/azure-cli-iot-ext/azext_iot/operations/hub.py", line 75, in iot_device_list
  result = iot_query(cmd, query, hub_name, top, resource_group_name, login=login)
File "/home/.azure/cliextensions/azure-cli-iot-ext/azext_iot/operations/hub.py", line 45, in iot_query
  target = get_iot_hub_connection_string(cmd, hub_name, resource_group_name, login=login)
File "/home/.azure/cliextensions/azure-cli-iot-ext/azext_iot/common/_azure.py", line 112, in get_iot_hub_connection_string
  client = iot_hub_service_factory(cmd.cli_ctx)
File "/home/.azure/cliextensions/azure-cli-iot-ext/azext_iot/_factory.py", line 28, in iot_hub_service_factory
  from azure.mgmt.iothub.iot_hub_client import IotHubClient
ModuleNotFoundError: No module named 'azure.mgmt.iothub.iot_hub_client'
```
    
So beheben Sie dieses Problem:

1. Führen Sie den folgenden Befehl aus:

    ```
    az --version
    ```
1. Vergewissern Sie sich, dass die folgenden Erweiterungen installiert sind. Zum Zeitpunkt der Erstellung dieser Anleitung waren die folgenden Versionen für die Erweiterungen gültig:

    |||
    |---|---|
    |azure-cli   |      2.5.1*|
    |command-modules-nspkg         |   2.0.3|
    |core  |    2.5.1*|
    |nspkg    | 3.0.4|
    |telemetry| 1.0.4|
    |Erweiterungen:    ||
    |storage-preview          |     0.2.10|
    |azure-cli-iot-ext          |    0.8.9|
    |eventgrid| 0.4.9|
    |azure-iot                       | 0.9.2|
1. Wenn eine der Erweiterungen älter als die oben aufgeführten Releasenummern ist, aktualisieren Sie die Erweiterung mit dem folgenden Befehl auf die neueste Version:

    ```
    az extension update --name <Extension name>
    ```

    Zum Beispiel, `az extension update --name azure-iot`

### <a name="sample-app-issues"></a>Probleme mit der Beispiel-App

Im Rahmen der Veröffentlichung haben wir .NET-Beispielcode bereitgestellt, um Ihrer Entwicklercommunity den Einstieg zu erleichtern. In diesem Abschnitt haben wir Fehler gesammelt, die beim Ausführen des Beispielcodes auftreten können, zusammen mit Informationen zum Debuggen dieser Fehler.

1. „Program.cs“ schlägt beim Aufruf der direkten Methode mit folgendem Fehler fehl:

    ```
    Unhandled exception. Microsoft.Azure.Devices.Common.Exceptions.UnauthorizedException: {"Message":"{\"errorCode\":401002,\"trackingId\":\"b1da85801b2e4faf951a2291a2c467c3-G:32-TimeStamp:04/06/2020 17:15:11\",\"message\":\"Unauthorized\",\"timestampUtc\":\"2020-04-06T17:15:11.6990676Z\"}","ExceptionMessage":""}
    
        at Microsoft.Azure.Devices.HttpClientHelper.ExecuteAsync(HttpClient httpClient, HttpMethod httpMethod, Uri requestUri, Func`3 modifyRequestMessageAsync, Func`2 isMappedToException, Func`3 processResponseMessageAsync, IDictionary`2 errorMappingOverrides, CancellationToken cancellationToken)
    
        at Microsoft.Azure.Devices.HttpClientHelper.ExecuteAsync(HttpMethod httpMethod, Uri requestUri, Func`3 modifyRequestMessageAsync, Func`3 processResponseMessageAsync, IDictionary`2 errorMappingOverrides, CancellationToken cancellationToken)
        
        at Microsoft.Azure.Devices.HttpClientHelper.PostAsync[T,T2](Uri requestUri, T entity, TimeSpan operationTimeout, IDictionary`2 errorMappingOverrides, IDictionary`2 customHeaders, CancellationToken cancellationToken)…
    ```

    1. Stellen Sie sicher, dass die [Azure IoT-Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) in Ihrer VS Code-Umgebung installiert sind und die Verbindung zu Ihrem IoT Hub eingerichtet ist. (STRG+UMSCHALT+P, anschließend Methode „Select IoT Hub“ auswählen, um die Verbindung mit Ihrem Abonnement und IoT Hub herzustellen)
1. Überprüfen Sie, ob Sie über VS Code eine direkte Methode für das Edge-Modul ausführen können (rufen Sie beispielsweise GraphToplogyList mit der folgenden Nutzlast auf { "@apiVersion": "1.0"}), Sie sollten die folgende Antwort empfangen. 

    ```
    {
      "status": 200,
      "payload": {
        "values": [
          {…
    …}
          ]
        }
    }
    ```

    ![Visual Studio Code](./media/troubleshoot-how-to/visual-studio-code1.png)
1. Wenn bei dem Schritt oben eine Fehler auftritt, versuchen Sie Folgendes:
    1. Wechseln Sie an Ihrem Edge-Gerät zur Eingabeaufforderung, und geben Sie dies ein:
    
    ```
    sudo systemctl restart iotedge
    ```

    Dadurch wird das Edge-Gerät mit allen Modulen neu gestartet. Warten Sie einige Minuten, und führen Sie den folgenden Befehl aus, um zu bestätigen, dass die Module ausgeführt werden, bevor Sie erneut versuchen, die direkte Methode zu verwenden.

    ```
    sudo iotedge list
    ```
    1. Wenn dies ebenfalls fehlschlägt, versuchen Sie, Ihre VM oder den Computer neu zu starten.
    1. Wenn alles fehlschlägt, führen Sie den folgenden Befehl aus, um eine ZIP-Datei mit allen [relevanten Protokollen](https://docs.microsoft.com/azure/iot-edge/troubleshoot#gather-debug-information-with-support-bundle-command) zu erhalten, die Sie an dieses [Supportticket](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) anfügen.

    ```
    sudo iotedge support-bundle --since 2h
    ```
1. Wenn Sie einen Fehlerantwortcode 400 erhalten, vergewissern Sie sich, dass die Nutzlast Ihres Methodenaufrufs das korrekte Format aufweist, wie in der Anleitung zu [direkten Methoden](direct-methods.md) beschrieben.
1. Wenn Sie den Statuscode 200 empfangen, zeigt dies an, dass Ihr Hub ordnungsgemäß funktioniert und ihre Modulbereitstellung korrekt ist und reagiert. Im nächsten Schritt wird dann überprüft, ob die Konfiguration der App korrekt ist. Die Konfiguration Ihrer App besteht aus den folgenden Feldern in der Datei „appsettings.json“. Überprüfen Sie sorgfältig, ob „deviceId“ und „moduleId“ korrekt sind. Eine einfache Möglichkeit zur Überprüfung bietet derAzure IoT Hub-Erweiterungsabschnitt in VSCode. Die Werte in der Datei „appsettings.json“ und im Abschnitt „IoT Hub“ müssen übereinstimmen.
    
    ```
    {
        "IoThubConnectionString" : 
        "deviceId" : 
        "moduleId" : 
    }
    ```

    ![IOT HUB](./media/troubleshoot-how-to/iot-hub.png)

1. Vergewissern Sie sich schließlich, dass Sie in „appsettings.json“ die IoT Hub-Verbindungszeichenfolge und nicht die IoT Hub-Geräteverbindungszeichenfolge angegeben haben, da diese ein unterschiedliches [Format](https://devblogs.microsoft.com/iotdev/understand-different-connection-strings-in-azure-iot-hub/) aufweisen.

### <a name="live-video-analytics-working-with-external-modules"></a>Live Video Analytics bei der Arbeit mit externen Modulen

Live Video Analytics kann den Mediengraph über den HTTP-Erweiterungsprozessor erweitern, um mithilfe von REST Daten per HTTP an andere IoT Edge-Module zu senden und von diesen zu empfangen.  Als [spezifisches Beispiel](https://github.com/Azure/live-video-analytics/tree/master/MediaGraph/topologies/httpExtension) kann der Mediengraph Video-Einzelbilder als Bilder an ein externes Rückschlussmodul wie Yolo V3 senden und im Gegenzug JSON-basierte Analyseergebnisse zurück erhalten. In einer derartigen Topologie bildet der IoT Hub meistens das Endziel für die Ereignisse. In Fällen, in denen Sie keine Rückschlussereignisse auf dem Hub sehen können, prüfen Sie die folgenden Punkte:

1. Überprüfen Sie, ob Sie tatsächlich den Hub untersuchen, auf dem der Mediengraph veröffentlicht. Wenn Sie mit mehreren Bereitstellungen arbeiten haben Sie es auch mit mehreren Hubs zu tun und prüfen möglicherweise den falschen auf Ereignisse.
1. Überprüfen Sie mithilfe von VSCode, ob das externe Modul bereitgestellt ist und ausgeführt wird. In der Beispielabbildung hier sind „rtspsim“ und „cv“ IoT Edge-Module, die extern zum lvaEdge-Modul ausgeführt werden.

    ![IOT HUB](./media/troubleshoot-how-to/iot-hub.png)
1. Überprüfen Sie, ob Sie Ereignisse an den richtigen URL-Endpunkt senden. Der externe KI-Container macht eine URL und einen Port verfügbar, über die er die Daten von POST-Anforderungen empfängt und zurückgibt. Diese URL wird als endpoint: url-Eigenschaft des HTTP-Erweiterungsprozessors angegeben. Wie am Beispiel der [Topologie-URL](https://github.com/Azure/live-video-analytics/blob/master/MediaGraph/topologies/httpExtension/topology.json) zu sehen, wird sie auf den Rückschluss-URL-Parameter festgelegt. Vergewissern Sie sich, dass der Standardwert für den Parameter (http://yolov3/score) oder der übergebene Wert korrekt ist. Sie können die Funktion mithilfe von curl testen.  
    1. Beispielsweise wird ein yolo v3-Container auf dem lokalen Computer ausgeführt, und die IP-Adresse des Containers ist 172.17.0.3 (verwenden Sie docker inspect, um die IP-Adresse zu bestimmen).

    ```
    curl -X POST http://172.17.0.3/score -H "Content-Type: image/jpeg" --data-binary @<fullpath to jpg>
    ```

    Zurückgegebenes Ergebnis:

    ```
    {"inferences": [{"type": "entity", "entity": {"tag": {"value": "car", "confidence": 0.8668569922447205}, "box": {"l": 0.3853073438008626, "t": 0.6063712999658677, "w": 0.04174524943033854, "h": 0.02989496027381675}}}]}
    ```

1. Wenn Sie eine oder mehrere Instanzen eines Graphs ausführen und den HTTP-Erweiterungprozessor nutzen, sollten Sie einen Bildfrequenzfilter vor jedem HTTP-Erweiterungsprozessor verwenden, um die Bilder pro Sekunde (Frames per Second, fps) des Videofeeds zu verwalten. In bestimmten Situationen mit hoher CPU-Auslastung/Arbeitsspeicherbelegung des Edge-Computers können bestimmte Rückschlussereignisse verloren gehen. Um dies zu vermeiden, legen Sie einen niedrigen Wert für die maximumFps-Eigenschaft im Bildfrequenzfilter fest. Sie können ihn auf 0,5 ("maximumFps": 0.5) für jede Instanz des Graphen festlegen und die Ausführung erneut starten, um nach Rückschlussereignissen auf dem Hub zu suchen.
    1. Alternativ können Sie einen leistungsstärkeren Edge-Computer mit mehr CPU-Leistung und Arbeitsspeicher verwenden.
    
### <a name="multiple-direct-methods-in-parallel--timeout-failure"></a>Mehrere parallel ausgeführte direkte Methoden: Zeitlimitüberschreitung 

Live Video Analytics in IoT Edge bietet ein auf direkten Methoden aufbauendes Programmiermodell, das die Einrichtung mehrerer Topologien und mehrerer Graphinstanzen ermöglicht. Im Rahmen der Topologie- und Grapheinrichtung rufen Sie mehrere direkte Methodenaufrufe für das Edge-Modul auf. Wenn Sie mehrere Methodenaufrufe parallel ausführen, kann es insbesondere bei Aufrufen zum Starten und Beenden der Graphen zu Zeitlimitüberschreitungen kommen, wie unten dargestellt. 

Die Assembly-Initialisierungsmethode „Microsoft.Media.LiveVideoAnalytics.Test.Feature.Edge.AssemblyInitializer.InitializeAssemblyAsync“ hat eine Ausnahme ausgelöst. Microsoft.Azure.Devices.Common.Exceptions.IotHubException: Microsoft.Azure.Devices.Common.Exceptions.IotHubException:<br/> `{"Message":"{\"errorCode\":504101,\"trackingId\":\"55b1d7845498428593c2738d94442607-G:32-TimeStamp:05/15/2020 20:43:10-G:10-TimeStamp:05/15/2020 20:43:10\",\"message\":\"Timed out waiting for the response from device.\",\"info\":{},\"timestampUtc\":\"2020-05-15T20:43:10.3899553Z\"}","ExceptionMessage":""}. Aborting test execution. `

Es empfiehlt sich, direkte Methoden nicht parallel sondern nacheinander aufzurufen, d. h. eine direkte Methode erst aufzurufen, nachdem der vorhergehende Methodenaufruf abgeschlossen wurde. 

## <a name="next-steps"></a>Nächste Schritte

[Tutorial: Ereignisbasierte Videoaufzeichnung in der Cloud und Wiedergabe aus der Cloud](event-based-video-recording-tutorial.md)
