---
author: v-dalc
ms.service: databox
ms.author: alkohli
ms.topic: include
ms.date: 03/23/2021
ms.openlocfilehash: 0d912d0ac3f0fcf4c52116e67909038a1973304b
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105105225"
---
Verwenden Sie die Runtimeantworten des IoT Edge-Agents, um auf Compute-Instanzen bezogene Fehler zu beheben. Im Folgenden sehen Sie eine Liste der möglichen Antworten:

* 200 – OK
* 400 – Die Bereitstellungskonfiguration ist falsch formatiert oder ungültig.
* 417 – Für das Gerät ist keine Bereitstellungskonfiguration festgelegt.
* 412 – Die Schemaversion der Bereitstellungskonfiguration ist ungültig.
* 406 – das IoT Edge-Gerät ist offline oder sendet keine Statusberichte.
* 500 – in der IoT Edge-Runtime ist ein Fehler aufgetreten.

Weitere Informationen finden Sie unter [IoT Edge-Agent](../articles/iot-edge/iot-edge-runtime.md?preserve-view=true&view=iotedge-2018-06#iot-edge-agent).

Der folgende Fehler steht im Zusammenhang mit dem IoT Edge-Dienst auf Ihrem Azure Stack Edge Pro-Gerät.

### <a name="compute-modules-have-unknown-status-and-cant-be-used"></a>Computemodule weisen den Status „Unbekannt“ auf und können nicht verwendet werden

#### <a name="error-description"></a>Fehlerbeschreibung

Alle Module auf dem Gerät weisen den Status „Unbekannt“ auf und können nicht verwendet werden. Der Status „Unbekannt“ liegt auch noch nach einem Neustart vor.<!--Original Support ticket relates to trying to deploy a container app on a Hub. Based on the work item, I assume the error description should not be that specific, and that the error applies to Azure Stack Edge Devices, which is the focus of this troubleshooting.-->

#### <a name="suggested-solution"></a>Vorgeschlagene Lösung

Löschen Sie den IoT Edge-Dienst, und stellen Sie dann das Modul bzw. die Module erneut bereit. Weitere Informationen finden Sie unter [Entfernen des IoT Edge-Diensts](../articles/databox-online/azure-stack-edge-gpu-manage-compute.md#remove-iot-edge-service).


### <a name="modules-show-as-running-but-are-not-working"></a>Module werden als ausgeführt angezeigt, funktionieren jedoch nicht.

#### <a name="error-description"></a>Fehlerbeschreibung

Als Laufzeitstatus des Moduls wird „ausgeführt“ angezeigt, aber die erwarteten Ergebnisse sind nicht festzustellen. 

Die Ursache dieser Bedingung könnte sein, dass die Modulroutenkonfiguration nicht funktioniert oder `edgehub` Nachrichten nicht erwartungsgemäß weiterleitet. Sie können die `edgehub`-Protokolle überprüfen. Wenn Sie feststellen, dass Fehler auftreten, beispielsweise die Verbindung mit dem IoT Hub-Dienst nicht hergestellt werden kann, sind Konnektivitätsprobleme der häufigste Grund. Die Ursache der Konnektivitätsprobleme könnte darin liegen, dass der AMPQ-Port, der vom IoT Hub-Dienst als Standardport für die Kommunikation verwendet wird, blockiert ist oder der Webproxyserver diese Nachrichten blockiert.

#### <a name="suggested-solution"></a>Vorgeschlagene Lösung

Führen Sie die folgenden Schritte aus:
1. Um den Fehler zu beheben, wechseln Sie zur IoT Hub-Ressource für Ihr Gerät, und wählen Sie dann Ihr Edge-Gerät aus. 
1. Gehen Sie zu **Module festlegen > Laufzeiteinstellungen**. 
1. Fügen Sie die Umgebungsvariable `Upstream protocol` hinzu, und weisen Sie ihr den Wert `AMQPWS` zu. Die in diesem Fall konfigurierten Meldungen werden über den Port 443 über WebSockets gesendet.

### <a name="modules-show-as-running-but-do-not-have-an-ip-assigned"></a>Module werden als ausgeführt angezeigt, aber ihnen ist keine IP-Adresse zugewiesen.

#### <a name="error-description"></a>Fehlerbeschreibung

Als Laufzeitstatus des Moduls wird „ausgeführt“ angezeigt, aber der containerisierten App ist keine IP-Adresse zugewiesen. 

Ursache dieser Bedingung ist, dass der Bereich der IP-Adressen, den Sie für „Externe Dienst-IP-Adressen für Kubernetes“ angegeben haben, nicht ausreicht. Sie müssen diesen Bereich erweitern, um sicherzustellen, dass alle bereitgestellten Container oder VMs abgedeckt werden.

#### <a name="suggested-solution"></a>Vorgeschlagene Lösung

Gehen Sie auf der lokalen Webbenutzeroberfläche Ihres Geräts wie folgt vor:
1. Wechseln Sie zur Seite **Compute**. Wählen Sie den Port aus, für den Sie das Computenetzwerk aktiviert haben. 
1. Geben Sie einen statischen, zusammenhängenden Bereich von IP-Adressen für **Externe Dienst-IP-Adressen für Kubernetes** ein. Sie benötigen 1 IP-Adresse für den `edgehub`-Dienst. Außerdem benötigen Sie für jedes IoT Edge-Modul und für jeden virtuellen Computer, den Sie bereitstellen, eine IP-Adresse. 
1. Wählen Sie **Übernehmen**. Der geänderte IP-Adressbereich sollte sofort wirksam werden.

Weitere Informationen finden Sie unter [Ändern externer Dienst-IP-Adressen für Container](../articles/databox-online/azure-stack-edge-gpu-manage-compute.md#change-external-service-ips-for-containers).

### <a name="configure-static-ips-for-iot-edge-modules"></a>Konfigurieren statischer IP-Adressen für IoT Edge-Module

#### <a name="problem-description"></a>Problembeschreibung

Kubernetes weist jedem IoT Edge-Modul auf Ihrem Azure Stack Edge Pro-GPU-Gerät dynamische IP-Adressen zu. Um statische IP-Adresse für die Module zu konfigurieren, ist eine Methode erforderlich.

#### <a name="suggested-solution"></a>Vorgeschlagene Lösung

Sie können feste IP-Adressen für Ihre IoT Edge-Module über den Abschnitt „K8s-experimental“ angeben, wie unten beschrieben: 

```yaml
{
  "k8s-experimental": {
    "serviceOptions" : {
      "loadBalancerIP" : "100.23.201.78",
      "type" : "LoadBalancer"
    }
  }
}
```
### <a name="expose-kubernetes-service-as-cluster-ip-service-for-internal-communication"></a>Verfügbarmachen des Kubernetes-Diensts als Cluster-IP-Dienst für die interne Kommunikation

#### <a name="problem-description"></a>Problembeschreibung

Standardmäßig weist der IoT-Diensttyp den Typ „Load Balancer“ auf und ihm werden externe IP-Adressen zugewiesen. Eventuell möchten Sie keine externe IP-Adresse für Ihre Anwendung. Möglicherweise müssen Sie die Pods im Kubernetes-Cluster für den Zugriff wie andere Pods und nicht als extern verfügbar gemachten Lastenausgleichsdienst verfügbar machen. 

#### <a name="suggested-solution"></a>Vorgeschlagene Lösung

Sie können die Erstellungsoptionen über den Abschnitt „K8s-experimental“ verwenden. Die folgende Dienstoption sollte mit Portbindungen funktionieren.

```yaml
{
"k8s-experimental": {
  "serviceOptions" : {
    "type" : "ClusterIP"
    }
  }
}
```