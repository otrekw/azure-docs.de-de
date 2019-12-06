---
title: Einrichten des Bereitschaftstests in einer Containerinstanz
description: Erfahren Sie, wie Sie einen Test konfigurieren, um sicherzustellen, dass Container in Azure Container Instances Anforderungen nur erhalten, wenn sie bereit sind.
ms.topic: article
ms.date: 10/17/2019
ms.openlocfilehash: 5ebbcdeee231e3e67abd6758485a12984137997e
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2019
ms.locfileid: "74533564"
---
# <a name="configure-readiness-probes"></a>Konfigurieren von Bereitschaftstests

Bei Containeranwendungen, die den Datenverkehr verarbeiten, sollten Sie überprüfen, ob Ihr Container für die Verarbeitung eingehender Anforderungen bereit ist. Azure Container Instances unterstützt Bereitschaftstests, um Konfigurationen einzubinden, sodass Sie auf Ihren Container unter bestimmten Bedingungen keinen Zugriff haben. Der Bereitschaftstest verhält sich wie ein [Kubernetes-Bereitschaftstest](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/). Eine Container-App muss z. B. beim Start einen großen Datensatz laden, und Sie möchten nicht, dass diese während dieser Zeit Anforderungen erhält.

In diesem Artikel wird erläutert, wie Sie eine Containergruppe mit einem Bereitschaftstest einsetzen, sodass ein Container nur dann Datenverkehr erhält, wenn der Test erfolgreich ist.

Azure Container Instances unterstützt auch [Livetests](container-instances-liveness-probe.md), die Sie so konfigurieren können, dass ein fehlerhafter Container automatisch neu startet.

## <a name="yaml-configuration"></a>YAML-Konfiguration

Erstellen Sie beispielsweise eine `readiness-probe.yaml`-Datei mit dem folgenden Codeausschnitt, der einen Bereitschaftstest beinhaltet. Diese Datei definiert eine Containergruppe, die aus einem Container mit einer kleinen Web-App besteht. Die App wird aus dem öffentlichen `mcr.microsoft.com/azuredocs/aci-helloworld`-Image bereitgestellt. Diese Container-App wird auch in Schnellstarts wie [Bereitstellen einer Containerinstanz in Azure mithilfe der Azure-Befehlszeilenschnittstelle](container-instances-quickstart.md) veranschaulicht.

```yaml
apiVersion: 2018-10-01
location: eastus
name: readinesstest
properties:
  containers:
  - name: mycontainer
    properties:
      image: mcr.microsoft.com/azuredocs/aci-helloworld
      command:
        - "/bin/sh"
        - "-c"
        - "node /usr/src/app/index.js & (sleep 240; touch /tmp/ready); wait"
      ports:
      - port: 80
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
      readinessProbe:
        exec:
          command:
          - "cat"
          - "/tmp/ready"
        periodSeconds: 5
  osType: Linux
  restartPolicy: Always
  ipAddress:
    type: Public
    ports:
    - protocol: tcp
      port: '80'
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

### <a name="start-command"></a>Startbefehl

Die YAML-Datei enthält einen Startbefehl, der beim Start des Containers ausgeführt werden soll. Dieser ist durch die Eigenschaft `command` definiert, die ein Array von Zeichenfolgen akzeptiert. Dieser Befehl simuliert eine Zeit, zu der die Web-App ausgeführt wird, der Container aber noch nicht bereit ist. Zuerst wird eine Shell-Sitzung gestartet und ein `node`-Befehl ausgeführt, um die Web-App zu starten. Es wird auch ein Befehl zum Standbymodus für 240 Sekunden gestartet. Danach wird eine Datei mit dem Namen `ready` im Verzeichnis `/tmp` erstellt:

```console
node /usr/src/app/index.js & (sleep 240; touch /tmp/ready); wait
```

### <a name="readiness-command"></a>Bereitschaftsbefehl

Diese YAML-Datei definiert ein `readinessProbe`-Element, das einen `exec`-Bereitschaftsbefehl unterstützt, der als Bereitschaftsüberprüfung agiert. Dieser Beispielbereitschaftsbefehl prüft die Existenz der Datei `ready` im Verzeichnis `/tmp`.

Wenn die Datei `ready` nicht existiert, wird der Bereitschaftsbefehl mit einem Wert ungleich Null beendet; der Container wird weiterhin ausgeführt, kann aber nicht aufgerufen werden. Wenn der Befehl mit dem Exitcode 0 erfolgreich beendet wird, ist der Container bereit für den Zugriff. 

Die `periodSeconds`-Eigenschaft gibt an, dass der Bereitschaftsbefehl alle fünf Sekunden ausgeführt werden soll. Der Bereitschaftstest wird über die gesamte Lebensdauer der Containergruppe ausgeführt.

## <a name="example-deployment"></a>Beispielbereitstellung

Führen Sie den folgenden Befehl aus, um eine Containergruppe mit der aufgeführten YAML-Konfiguration bereitzustellen:

```azurecli-interactive
az container create --resource-group myResourceGroup --file readiness-probe.yaml
```

## <a name="view-readiness-checks"></a>Anzeigen der Bereitschaftsprüfung

In diesem Beispiel schlägt der Bereitschaftsbefehl in den ersten 240 Sekunden fehl, wenn dieser nach der Existenz der Datei `ready` sucht. Der zurückgegebene Statuscode signalisiert, dass der Container nicht bereit ist.

Diese Ereignisse können über das Azure-Portal oder Azure CLI angezeigt werden. Beispielsweise zeigt das Portal an, dass Ereignisse vom Typ `Unhealthy` ausgelöst werden, wenn der Bereitschaftsbefehl fehlschlägt. 

![Fehlerhaftes Ereignis im Portal][portal-unhealthy]

## <a name="verify-container-readiness"></a>Bestätigen der Containerbereitschaft

Nach dem Start des Containers können Sie überprüfen, ob dieser zunächst nicht zugänglich ist. Ermitteln Sie nach der Bereitstellung die IP-Adresse der Containergruppe:

```azurecli
az container show --resource-group myResourceGroup --name readinesstest --query "ipAddress.ip" --out tsv
```

Versuchen Sie, auf die Website zuzugreifen, während der Bereitschaftstest fehlschlägt:

```bash
wget <ipAddress>
```

Die Ausgabe zeigt, dass die Website zunächst nicht zugänglich ist:
```
$ wget 192.0.2.1
--2019-10-15 16:46:02--  http://192.0.2.1/
Connecting to 192.0.2.1... connected.
HTTP request sent, awaiting response... 
```

Nach 240 Sekunden ist der Bereitschaftsbefehl erfolgreich und signalisiert, dass der Container bereit ist. Wenn Sie jetzt den Befehl `wget` ausführen, ist dieser erfolgreich:

```
$ wget 192.0.2.1
--2019-10-15 16:46:02--  http://192.0.2.1/
Connecting to 192.0.2.1... connected.
HTTP request sent, awaiting response...200 OK
Length: 1663 (1.6K) [text/html]
Saving to: ‘index.html.1’

index.html.1                       100%[===============================================================>]   1.62K  --.-KB/s    in 0s      

2019-10-15 16:49:38 (113 MB/s) - ‘index.html.1’ saved [1663/1663] 
```

Wenn der Container bereit ist, können Sie auch auf die Web-App zugreifen, indem Sie mit einem Webbrowser auf die IP-Adresse zugreifen.

> [!NOTE]
> Der Bereitschaftstest wird weiterhin über die gesamte Lebensdauer der Containergruppe ausgeführt. Fällt der Bereitschaftsbefehl zu einem späteren Zeitpunkt aus, wird der Container wieder unzugänglich. 
> 

## <a name="next-steps"></a>Nächste Schritte

Ein Bereitschaftstest kann in Szenarios mit Gruppen aus mehreren Containern nützlich sein, die aus abhängigen Containern bestehen. Weitere Informationen zu Szenarios mit mehreren Containern finden Sie unter [Containergruppen in Azure Container Instances](container-instances-container-groups.md).

<!-- IMAGES -->
[portal-unhealthy]: ./media/container-instances-readiness-probe/readiness-probe-failed.png
