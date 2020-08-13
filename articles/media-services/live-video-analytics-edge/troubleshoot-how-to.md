---
title: 'Troubleshooting von Live Video Analytics in IoT Edge: Azure'
description: Dieser Artikel behandelt Schritte zur Problembehandlung für Live Video Analytics in IoT Edge.
author: IngridAtMicrosoft
ms.topic: how-to
ms.author: inhenkel
ms.date: 05/24/2020
ms.openlocfilehash: bbd3cb88b017209adff58a646e274caf31ab425f
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87486441"
---
# <a name="troubleshoot-live-video-analytics-on-iot-edge"></a>Troubleshooting Live Video Analytics in IoT Edge

In diesem Artikel werden die Schritte zur Problembehandlung bei Live Video Analytics (LVA) in Azure IoT Edge beschrieben.

## <a name="troubleshoot-deployment-issues"></a>Problembehandlung bei Bereitstellungsproblemen

### <a name="diagnostics"></a>Diagnose

Im Rahmen Ihrer Bereitstellung von Live Video Analytics richten Sie Azure-Ressourcen wie IoT Hub und IoT Edge-Geräte ein. Vergewissern Sie sich im ersten Schritt der Problemdiagnose immer, dass das Edge-Gerät ordnungsgemäß eingerichtet ist, indem Sie diese Anweisungen befolgen:

1. [Ausführen des Befehls `check`](../../iot-edge/troubleshoot.md#run-the-check-command)
1. [Überprüfen Ihrer IoT Edge-Version](../../iot-edge/troubleshoot.md#check-your-iot-edge-version)
1. [Überprüfen des Status von IoT Edge Security Manager und der zugehörigen Protokolle](../../iot-edge/troubleshoot.md#check-the-status-of-the-iot-edge-security-manager-and-its-logs)
1. [Anzeigen der Nachrichten, die den IoT Edge-Hub durchlaufen](../../iot-edge/troubleshoot.md#view-the-messages-going-through-the-iot-edge-hub)
1. [Neustarten von Containern](../../iot-edge/troubleshoot.md#restart-containers)
1. [Überprüfen der Konfigurationsregeln für Firewall und Port](../../iot-edge/troubleshoot.md#check-your-firewall-and-port-configuration-rules)

### <a name="pre-deployment-issues"></a>Probleme vor der Bereitstellung

Wenn die Edge-Infrastruktur in Ordnung ist, können Sie nach Problemen in der Bereitstellungsmanifestdatei suchen. Um das IoT Edge-Modul „Live Video Analytics“ auf dem IoT Edge-Gerät neben allen anderen IoT-Modulen bereitzustellen, verwenden Sie ein Bereitstellungsmanifest, das den IoT Edge-Hub, den IoT Edge-Agent und weitere Module mit ihren Eigenschaften enthält. Wenn der JSON-Code nicht wohlgeformt ist, erhalten Sie möglicherweise die folgende Fehlermeldung: 

```
az iot edge set-modules --hub-name <iot-hub-name> --device-id lva-sample-device --content <path-to-deployment_manifest.json>
```

Fehler beim Analysieren des JSON-Codes aus Datei „<deployment manifest.json>“ für das Argument „content“ mit dieser Ausnahme: „Zusätzliche Daten: Zeile 101, Spalte 1 (Zeichen 5325)“

Wenn dieser Fehler auftritt, empfiehlt es sich, den JSON-Code auf fehlende Klammern oder andere Probleme in der Dateistruktur zu überprüfen. Zum Überprüfen der Dateistruktur können Sie einen Client wie [Notepad++ mit dem JSON Viewer-Plug-In](https://riptutorial.com/notepadplusplus/example/18201/json-viewer) oder ein Onlinetool wie [JSON Formatter & Validator](https://jsonformatter.curiousconcept.com/) verwenden.

### <a name="during-deployment-diagnose-with-media-graph-direct-methods"></a>Während der Bereitstellung: Diagnose mit direkten Mediengraphmethoden 

Nachdem Live Video Analytics ordnungsgemäß auf dem IoT Edge-Gerät bereitgestellt wurde, können Sie den Mediengraph durch Aufrufen [direkter Methoden](direct-methods.md) erstellen und ausführen. Sie können das Azure-Portal verwenden, um eine Diagnose des Mediengraphs mithilfe direkter Methoden auszuführen:

1. Navigieren Sie im Azure-Portal zu dem IoT Hub, der mit Ihrem IoT Edge-Gerät verbunden ist.

1. Suchen Sie nach **Automatische Geräteverwaltung**, und wählen Sie **IoT Edge** aus.  

1. Wählen Sie aus der Liste der Edge-Geräte das Gerät aus, das Sie diagnostizieren möchten.  
         
    ![Screenshot des Azure-Portals, in dem eine Liste der Edge-Geräte angezeigt wird](./media/troubleshoot-how-to/lva-sample-device.png)

1. Überprüfen Sie, ob der Antwortcode *200 – OK* lautet. Zu den weiteren Antwortcodes für die [IoT Edge-Runtime](../../iot-edge/iot-edge-runtime.md) gehören die folgenden:
    * 400 – Die Bereitstellungskonfiguration ist falsch formatiert oder ungültig.
    * 417 – Für das Gerät ist keine Bereitstellungskonfiguration festgelegt.
    * 412 – Die Schemaversion der Bereitstellungskonfiguration ist ungültig.
    * 406 – das IoT Edge-Gerät ist offline oder sendet keine Statusberichte.
    * 500 – in der IoT Edge-Runtime ist ein Fehler aufgetreten.

1. Wenn Sie einen 501-Statuscode empfangen, überprüfen Sie, ob der Name der direkten Methode korrekt ist. Wenn der Methodenname und die Anforderungsnutzlast korrekt sind, sollten Sie Ergebnisse mit dem Erfolgscode „200“ erhalten. Wenn die Angabe der Anforderungsnutzlast ungenau ist, erhalten Sie den Status „= 400“ und eine Antwortnutzlast, die den Fehlercode und eine Meldung angibt, die Sie bei der Diagnose des Problems bei Ihrem direkten Methodenaufrufe unterstützen soll.
    * Indem Sie die gemeldeten und die gewünschten Eigenschaften überprüfen, können Sie besser nachvollziehen, ob die Moduleigenschaften mit der Bereitstellung synchronisiert wurden. Wenn dies nicht der Fall ist, können Sie Ihr IoT Edge-Gerät neu starten. 
    * Verwenden Sie den Leitfaden für [direkte Methoden](direct-methods.md), um einige Methoden aufzurufen, insbesondere einfache wie GraphTopologyList. Der Leitfaden gibt darüber hinaus die erwartete Nutzlast für Anforderungen und Antworten sowie Fehlercodes an. Nachdem die einfachen direkten Methoden erfolgreich ausgeführt wurden, können Sie sicher sein, dass das IoT Edge-Modul „Live Video Analytics“ ordnungsgemäß funktioniert.
        
       ![Screenshot des Bereichs „Direkte Methode“ für das IoT Edge-Modul](./media/troubleshoot-how-to/direct-method.png) 

1. Wenn in den Spalten **In Bereitstellung angegeben** und **Vom Gerät gemeldet** der Wert *Ja* angezeigt wird, können Sie direkte Methoden für das IoT Edge-Modul „Live Video Analytics“ aufrufen. Wählen Sie das Modul aus, um zu einer Seite zu gelangen, auf der Sie die gewünschten und die gemeldeten Eigenschaften überprüfen und direkte Methoden aufrufen können. Berücksichtigen Sie dabei Folgendes: 

### <a name="post-deployment-diagnose-logs-for-issues-during-the-run"></a>Nach der Bereitstellung: Diagnose der Protokolle zu Problemen während der Ausführung 

Die Containerprotokolle für Ihr IoT Edge-Modul sollten Diagnoseinformationen enthalten, die beim Debuggen von Problemen während der Modullaufzeit helfen. Sie können [Containerprotokolle auf Probleme überprüfen](../../iot-edge/troubleshoot.md#check-container-logs-for-issues) und das Problem selbst diagnostizieren. 

Wenn Sie sämtliche bereits genannten Überprüfungen ausgeführt haben und weiterhin Probleme auftreten, sammeln Sie [mit dem Befehl `support bundle`](../../iot-edge/troubleshoot.md#gather-debug-information-with-support-bundle-command) Protokolle aus dem IoT Edge-Gerät, damit das Azure-Team weitere Analysen durchführen kann. Sie können sich zwecks Support [an uns wenden](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) und die gesammelten Protokolle übermitteln.

## <a name="common-error-resolutions"></a>Lösungen für allgemeine Fehler

Live Video Analytics wird als IoT Edge-Modul auf dem IoT Edge-Gerät bereitgestellt und arbeitet mit den IoT Edge-Agent- und IoT Edge-Hub-Modulen zusammen. Einige der häufigeren Fehler, die bei der Bereitstellung von Live Video Analytics auftreten können, werden durch Probleme mit der zugrunde liegenden IoT-Infrastruktur verursacht. Zu diesen Fehlern zählen die folgenden:

* [Der IoT Edge-Agent wird nach ungefähr einer Minute beendet](../../iot-edge/troubleshoot-common-errors.md#iot-edge-agent-stops-after-about-a-minute).
* [Der IoT Edge-Agent kann nicht auf das Image eines Moduls zugreifen (403)](../../iot-edge/troubleshoot-common-errors.md#iot-edge-agent-cant-access-a-modules-image-403).
* [Das IoT Edge-Agent-Modul meldet „empty config file“, und auf dem Gerät werden keine Module gestartet](../../iot-edge/troubleshoot-common-errors.md#edge-agent-module-reports-empty-config-file-and-no-modules-start-on-the-device).
* [Der IoT Edge-Hub kann nicht gestartet werden](../../iot-edge/troubleshoot-common-errors.md#iot-edge-hub-fails-to-start).
* [Fehler beim Daemon für die IoT Edge-Sicherheit aufgrund eines ungültigen Hostnamens](../../iot-edge/troubleshoot-common-errors.md#iot-edge-security-daemon-fails-with-an-invalid-hostname).
* [Live Video Analytics oder andere benutzerdefinierte IoT Edge-Module können keine Nachricht an den Edge-Hub senden und geben einen 404-Fehler aus](../../iot-edge/troubleshoot-common-errors.md#iot-edge-module-fails-to-send-a-message-to-edgehub-with-404-error).
* [Das IoT Edge-Modul wird erfolgreich bereitgestellt und verschwindet dann vom Gerät](../../iot-edge/troubleshoot-common-errors.md#iot-edge-module-deploys-successfully-then-disappears-from-device).

### <a name="edge-setup-script-issues"></a>Probleme beim Edge-Setupskript

Im Rahmen unserer Dokumentation haben wir ein [Setupskript](https://github.com/Azure/live-video-analytics/tree/master/edge/setup) für die Bereitstellung von Edge- und Cloud-Ressourcen zur Verfügung gestellt, um den Einstieg in Live Video Analytics in IoT Edge zu erleichtern. Dieser Abschnitt zeigt einige Skriptfehler, die auftreten können, sowie Lösungen zum Debuggen dieser Fehler.

Problem: Das Skript wird ausgeführt, erstellt teilweise einige Ressourcen und wird dann mit der folgenden Meldung abgebrochen:

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
1. Vergewissern Sie sich, dass die folgenden Erweiterungen installiert sind. Zum Zeitpunkt der Veröffentlichung dieses Artikels sind folgende Erweiterungen und Versionen verfügbar:

    | Durchwahl | Version |
    |---|---|
    |azure-cli   |      2.5.1*|
    |command-modules-nspkg         |   2.0.3|
    |core  |    2.5.1*|
    |nspkg    | 3.0.4|
    |telemetry| 1.0.4|
    |storage-preview          |     0.2.10|
    |azure-cli-iot-ext          |    0.8.9|
    |eventgrid| 0.4.9|
    |azure-iot                       | 0.9.2|
1. Wenn Sie eine Erweiterung installiert haben, deren Version niedriger ist als die hier aufgeführte Releasenummer, aktualisieren Sie die Erweiterung mithilfe des folgenden Befehls:

    ```
    az extension update --name <Extension name>
    ```

    Sie können beispielsweise `az extension update --name azure-iot` ausführen.

### <a name="sample-app-issues"></a>Probleme mit der Beispiel-App

Im Rahmen der Veröffentlichung haben wir .NET-Beispielcode bereitgestellt, um unserer Entwicklercommunity den Einstieg zu erleichtern. Dieser Abschnitt zeigt einige Fehler, die beim Ausführen des Beispielcodes auftreten können, sowie Lösungen zum Debuggen dieser Fehler.

Problem: „Program.cs“ schlägt beim Aufruf der direkten Methode mit folgendem Fehler fehl:

```
Unhandled exception. Microsoft.Azure.Devices.Common.Exceptions.UnauthorizedException: {"Message":"{\"errorCode\":401002,\"trackingId\":\"b1da85801b2e4faf951a2291a2c467c3-G:32-TimeStamp:04/06/2020 17:15:11\",\"message\":\"Unauthorized\",\"timestampUtc\":\"2020-04-06T17:15:11.6990676Z\"}","ExceptionMessage":""}
    
        at Microsoft.Azure.Devices.HttpClientHelper.ExecuteAsync(HttpClient httpClient, HttpMethod httpMethod, Uri requestUri, Func`3 modifyRequestMessageAsync, Func`2 isMappedToException, Func`3 processResponseMessageAsync, IDictionary`2 errorMappingOverrides, CancellationToken cancellationToken)
    
        at Microsoft.Azure.Devices.HttpClientHelper.ExecuteAsync(HttpMethod httpMethod, Uri requestUri, Func`3 modifyRequestMessageAsync, Func`3 processResponseMessageAsync, IDictionary`2 errorMappingOverrides, CancellationToken cancellationToken)
        
        at Microsoft.Azure.Devices.HttpClientHelper.PostAsync[T,T2](Uri requestUri, T entity, TimeSpan operationTimeout, IDictionary`2 errorMappingOverrides, IDictionary`2 customHeaders, CancellationToken cancellationToken)…
```

1. Stellen Sie sicher, dass die [Azure IoT Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) in Ihrer Visual Studio Code-Umgebung installiert sind und die Verbindung mit Ihrer IoT Hub-Instanz eingerichtet ist. Drücken Sie zur Überprüfung die Tastenkombination STRG+UMSCHALT+P, und wählen Sie dann **IoT Hub-Methode auswählen** aus.

1. Überprüfen Sie, ob Sie über Visual Studio Code eine direkte Methode im IoT Edge-Modul aufrufen können. Rufen Sie beispielsweise GraphTopologyList mit der folgenden Nutzlast auf: {&nbsp;"@apiVersion": "1.0"}. Sie sollten folgende Antwort erhalten: 

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

    ![Screenshot der Antwort in Visual Studio Code](./media/troubleshoot-how-to/visual-studio-code1.png)
1. Wenn die oben genannte Lösung nicht funktioniert, versuchen Sie Folgendes:

    a. Führen Sie an der Eingabeaufforderung auf Ihrem IoT Edge-Gerät den folgenden Befehl aus:
    
      ```
      sudo systemctl restart iotedge
      ```

      Dieser Befehl startet das IoT Edge-Gerät und alle Module neu. Warten Sie einige Minuten. Bevor Sie versuchen, die direkte Methode erneut zu versuchen, vergewissern Sie sich mithilfe des folgenden Befehls, dass die Module ausgeführt werden:

      ```
      sudo iotedge list
      ```

    b. Wenn dieser Ansatz auch nicht funktioniert, versuchen Sie, die VM oder den Computer neu zu starten.

    c. Wenn keine der hier beschriebenen Vorgehensweisen das Problem löst, führen Sie den folgenden Befehl aus, um eine ZIP-Datei mit allen [relevanten Protokollen](../../iot-edge/troubleshoot.md#gather-debug-information-with-support-bundle-command) abzurufen, und fügen Sie diese an ein [Supportticket](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) an.

    ```
    sudo iotedge support-bundle --since 2h
    ```

1. Wenn Sie den Fehlerantwortcode *400* erhalten, vergewissern Sie sich, dass die Nutzlast Ihres Methodenaufrufs wohlgeformt ist, wie in der Anleitung zu [direkten Methoden](direct-methods.md) beschrieben.
1. Wenn Sie den Statuscode *200* empfangen, zeigt dies an, dass Ihr Hub ordnungsgemäß funktioniert und Ihre Modulbereitstellung korrekt ist und reagiert. 

1. Überprüfen Sie, ob die App-Konfiguration korrekt ist. Die Konfiguration Ihrer App besteht aus den folgenden Feldern in der Datei *appsettings.json*. Überprüfen Sie sorgfältig, ob „deviceId“ und „moduleId“ korrekt sind. Der Abschnitt für die Azure IoT Hub-Erweiterung in Visual Studio Code bietet eine einfache Möglichkeit für diese Überprüfung. Die Werte in der Datei *appsettings.json* und im Abschnitt „IoT Hub“ müssen übereinstimmen.
    
    ```
    {
        "IoThubConnectionString" : 
        "deviceId" : 
        "moduleId" : 
    }
    ```

1. Vergewissern Sie sich, dass Sie in der Datei *appsettings.json* die Verbindungszeichenfolge für IoT Hub und *nicht* die Verbindungszeichenfolge für das IoT Hub-Gerät angegeben haben, da [diese unterschiedliche Formate aufweisen](https://devblogs.microsoft.com/iotdev/understand-different-connection-strings-in-azure-iot-hub/).

### <a name="live-video-analytics-working-with-external-modules"></a>Live Video Analytics bei der Arbeit mit externen Modulen

Live Video Analytics kann den Mediengraph über den HTTP-Erweiterungsprozessor erweitern, um mithilfe von REST Daten über HTTP an andere IoT Edge-Module zu senden und von diesen zu empfangen. Ein [spezifisches Beispiel](https://github.com/Azure/live-video-analytics/tree/master/MediaGraph/topologies/httpExtension): Der Mediengraph kann Videoeinzelbilder als Bilder an ein externes Rückschlussmodul wie Yolo v3 senden und als Rückgabe JSON-basierte Analyseergebnisse erhalten. In einer solchen Topologie bildet in den meisten Fällen der IoT Hub das Ziel für die Ereignisse. In Situationen, in denen Sie keine Rückschlussereignisse auf dem Hub sehen können, prüfen Sie die folgenden Punkte:

* Überprüfen Sie, ob Sie tatsächlich den Hub untersuchen, auf dem der Mediengraph veröffentlicht. Wenn Sie mehrere Bereitstellungen erstellen, haben Sie es auch mit mehreren Hubs zu tun und suchen möglicherweise auf dem falschen nach Ereignissen.
* Überprüfen Sie in Visual Studio Code, ob das externe Modul bereitgestellt ist und ausgeführt wird. In der Beispielabbildung hier sind „rtspsim“ und „cv“ IoT Edge-Module, die extern zum lvaEdge-Modul ausgeführt werden.

    ![Screenshot: Ausführungsstatus von Modulen in Azure IoT Hub](./media/troubleshoot-how-to/iot-hub.png)

* Überprüfen Sie, ob Sie Ereignisse an den richtigen URL-Endpunkt senden. Der externe KI-Container macht eine URL und einen Port verfügbar, über die bzw. den er die Daten von POST-Anforderungen empfängt und zurückgibt. Diese URL wird als `endpoint: url`-Eigenschaft des HTTP-Erweiterungsprozessors angegeben. Wie Sie am Beispiel der [Topologie-URL](https://github.com/Azure/live-video-analytics/blob/master/MediaGraph/topologies/httpExtension/topology.json) sehen, ist der Endpunkt auf den Parameter für die Rückschluss-URL festgelegt. Vergewissern Sie sich, dass der Standardwert für den Parameter bzw. der übergebene Wert korrekt ist. Sie können die Funktionalität mithilfe der Client-URL (cURL) testen.  

    Im Folgenden finden Sie als Beispiel einen Yolo v3-Container, der auf einem lokalen Computer mit der IP-Adresse 172.17.0.3 ausgeführt wird. Verwenden Sie den inspect-Befehl von Docker, um die IP-Adresse zu ermitteln.

    ```
    curl -X POST http://172.17.0.3/score -H "Content-Type: image/jpeg" --data-binary @<fullpath to jpg>
    ```

    Zurückgegebenes Ergebnis:

    ```
    {"inferences": [{"type": "entity", "entity": {"tag": {"value": "car", "confidence": 0.8668569922447205}, "box": {"l": 0.3853073438008626, "t": 0.6063712999658677, "w": 0.04174524943033854, "h": 0.02989496027381675}}}]}
    ```

* Wenn Sie eine oder mehrere Instanzen eines Graphs ausführen, der den HTTP-Erweiterungsprozessor nutzt, sollten Sie einen Bildfrequenzfilter vor jedem HTTP-Erweiterungsprozessor verwenden, um die fps-Rate (frames per second, Bilder pro Sekunde) des Videofeeds zu verwalten. 

   In bestimmten Situationen mit hoher CPU-Auslastung/Arbeitsspeicherbelegung des Edge-Computers können bestimmte Rückschlussereignisse verloren gehen. Um dies zu vermeiden, legen Sie einen niedrigen Wert für die maximumFps-Eigenschaft im Bildfrequenzfilter fest. Sie können ihn auf 0,5 ("maximumFps": 0.5) für jede Instanz des Graphs festlegen und die Ausführung erneut starten, um nach Rückschlussereignissen auf dem Hub zu suchen.

   Alternativ dazu können Sie einen leistungsstärkeren Edge-Computer mit mehr CPU-Leistung und Arbeitsspeicher verwenden.
    
### <a name="multiple-direct-methods-in-parallel--timeout-failure"></a>Mehrere parallel ausgeführte direkte Methoden: Zeitlimitüberschreitung 

Live Video Analytics in IoT Edge bietet ein auf direkten Methoden aufbauendes Programmiermodell, das die Einrichtung mehrerer Topologien und mehrerer Graphinstanzen ermöglicht. Im Rahmen der Topologie- und Grapheinrichtung führen Sie mehrere direkte Methodenaufrufe für das IoT Edge-Modul aus. Wenn Sie diese Methodenaufrufe parallel ausführen, kann es insbesondere bei denjenigen zum Starten und Beenden der Graphs zu Timeoutfehlern kommen, wie beispielsweise den folgenden: 

Die Assembly-Initialisierungsmethode „Microsoft.Media.LiveVideoAnalytics.Test.Feature.Edge.AssemblyInitializer.InitializeAssemblyAsync“ hat eine Ausnahme ausgelöst. Microsoft.Azure.Devices.Common.Exceptions.IotHubException: Microsoft.Azure.Devices.Common.Exceptions.IotHubException:<br/> `{"Message":"{\"errorCode\":504101,\"trackingId\":\"55b1d7845498428593c2738d94442607-G:32-TimeStamp:05/15/2020 20:43:10-G:10-TimeStamp:05/15/2020 20:43:10\",\"message\":\"Timed out waiting for the response from device.\",\"info\":{},\"timestampUtc\":\"2020-05-15T20:43:10.3899553Z\"}","ExceptionMessage":""}. Aborting test execution. `

Es wird empfohlen, direkte Methoden *nicht* parallel aufzurufen. Rufen Sie Methoden stattdessen sequenziell auf, indem Sie einen Methodenaufruf erst dann starten, wenn der vorherige beendet ist.

### <a name="collect-logs-for-submitting-a-support-ticket"></a>Sammeln von Protokollen zum Übermitteln eines Supporttickets

Wenn die Probleme mit der Self-Service-Problembehandlung nicht gelöst werden können, navigieren Sie zum Azure-Portal, und [öffnen Sie ein Supportticket](../../azure-portal/supportability/how-to-create-azure-support-request.md).

> [!WARNING]
> Es kann sein, dass diese Protokolldateien personenbezogene Informationen (Personally Identifiable Information, PII) enthalten, beispielsweise Ihre IP-Adresse. Alle lokalen Kopien der Protokolle werden gelöscht, sobald wir die Untersuchung abgeschlossen haben und das Supportticket schließen.  

Um die relevanten Protokolle zu sammeln, die dem Ticket hinzugefügt werden sollen, befolgen Sie die Anweisungen in den nächsten Abschnitten. Sie können die Protokolldateien im Bereich **Details** der Supportanfrage hochladen.

### <a name="use-the-support-bundle-command"></a>Verwenden des Befehls „support-bundle“

Wenn Sie Protokolle von einem IoT Edge-Gerät sammeln müssen, verwenden Sie dazu am einfachsten den Befehl `support-bundle`. Dieser Befehl sammelt Folgendes:

- Modulprotokolle
- IoT Edge Security Manager- und Container-Engine-Protokolle
- Überprüfen der IoT Edge-JSON-Ausgabe
- Nützliche Debuginformationen

1. Führen Sie den Befehl `support-bundle` mit dem Flag *--since* aus, um anzugeben, welchen Zeitraum die Protokolle abdecken sollen. Beispielsweise werden mit „2h“ die Protokolle der letzten zwei Stunden abgerufen. Sie können den Wert dieses Flags ändern, um Protokolle für andere Zeiträume einzuschließen.

    ```
    sudo iotedge support-bundle --since 2h
    ```

   Dieser Befehl erstellt eine Datei namens *support_bundle.zip* in dem Verzeichnis, in dem Sie den Befehl ausführen. 
   
1. Fügen Sie die Datei *support_bundle.zip* an das Supportticket an.

### <a name="live-video-analytics-debug-logs"></a>Debugprotokolle für Live Video Analytics

Um das IoT Edge-Modul „Live Video Analytics“ so zu konfigurieren, dass Debugprotokolle generiert werden, gehen Sie folgendermaßen vor:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu Ihrer IoT Hub-Instanz.
1. Wählen Sie im linken Bereich die Option **IoT Edge** aus.
1. Wählen Sie in der Liste der Geräte die ID des Zielgeräts aus.
1. Wählen Sie oben im Bereich die Option **Module festlegen** aus.

   ![Screenshot der Schaltfläche „Module festlegen“ im Azure-Portal](media/troubleshoot-how-to/set-modules.png)

1. Suchen Sie im Abschnitt **IoT Edge-Module** nach **IvaEdge**, und wählen Sie dieses Modul aus.
1. Klicken Sie auf **Optionen für Containererstellung**.
1. Fügen Sie im Abschnitt **Bindungen** den folgenden Befehl hinzu:

    `/var/local/mediaservices/logs:/var/lib/azuremediaservices/logs`

    > [!NOTE] 
    > Dieser Befehl bindet die Protokollordner zwischen dem Edge-Gerät und dem Container. Wenn Sie die Protokolle an einem anderen Speicherort sammeln möchten, verwenden Sie den folgenden Befehl, und ersetzen Sie **$LOG_LOCATION_ON_EDGE_DEVICE** durch den gewünschten Speicherort: `/var/$LOG_LOCATION_ON_EDGE_DEVICE:/var/lib/azuremediaservices/logs`

1. Wählen Sie **Update** aus.
1. Klicken Sie auf **Überprüfen + erstellen**. Unter einem grünen Banner wird eine Erfolgsmeldung angezeigt.
1. Klicken Sie auf **Erstellen**.
1. Aktualisieren Sie die Option **Zwilling der Modulkennung** so, dass diese auf den Parameter „DebugLogsDirectory“ verweist, der wiederum auf das Verzeichnis verweist, in dem die Protokolle gesammelt werden:

    a. Wählen Sie in der Tabelle **Module** das Modul **IvaEdge** aus.  
    b. Wählen Sie oben im Bereich die Option **Zwilling der Modulkennung** aus. Ein bearbeitbarer Bereich wird geöffnet.  
    c. Fügen Sie unter **desired key** das folgende Schlüssel-Wert-Paar hinzu:  
    `"DebugLogsDirectory": "/var/lib/azuremediaservices/logs"`

    > [!NOTE] 
    > Dieser Befehl bindet die Protokollordner zwischen dem Edge-Gerät und dem Container. Wenn Sie die Protokolle an einem anderen Speicherort sammeln möchten, verwenden Sie den folgenden Befehl, und ersetzen Sie **$DEBUG_LOG_LOCATION_ON_EDGE_DEVICE** durch den gewünschten Speicherort:  
    > `"DebugLogsDirectory": "/var/$DEBUG_LOG_LOCATION_ON_EDGE_DEVICE"`  

    d. Wählen Sie **Speichern** aus.

1. Reproduzieren Sie das Problem.
1. Stellen Sie im Portal auf der Seite **IoT Hub** eine Verbindung mit dem virtuellen Computer her.
1. Zippen Sie alle Dateien im Ordner *debugLogs*.

   > [!NOTE]
   > Diese Protokolldateien sind nicht für die Selbstdiagnose gedacht. Sie dienen dem Azure-Technikteam zum Analysieren Ihrer Probleme.

   a. Stellen Sie sicher, dass Sie im folgenden Befehl **$DEBUG_LOG_LOCATION_ON_EDGE_DEVICE** durch den Speicherort der Debugprotokolle auf dem Edge-Gerät ersetzen, das Sie zuvor eingerichtet haben.  

   ```
   sudo apt install zip unzip  
   zip -r debugLogs.zip $DEBUG_LOG_LOCATION_ON_EDGE_DEVICE 
   ```

   b. Fügen Sie die Datei *debugLogs.zip* an das Supportticket an.

1. Sie können die Protokollsammlung beenden, indem Sie den Wert unter **Zwilling der Modulkennung** auf *NULL* festlegen. Wechseln Sie zurück zur Seite **Zwilling der Modulkennung**, und aktualisieren Sie den folgenden Parameter wie folgt:

    `"DebugLogsDirectory": ""`

## <a name="next-steps"></a>Nächste Schritte

[Tutorial: Ereignisbasierte Videoaufzeichnung in der Cloud und Wiedergabe aus der Cloud](event-based-video-recording-tutorial.md)
