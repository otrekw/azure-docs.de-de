---
author: v-dalc
ms.service: databox
ms.author: alkohli
ms.topic: include
ms.date: 06/09/2021
ms.openlocfilehash: 86ccf634a9c33d3e8cfb8efd97e94f322fd5127f
ms.sourcegitcommit: e39ad7e8db27c97c8fb0d6afa322d4d135fd2066
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111987852"
---
### <a name="compute-modules-have-unknown-status-and-cant-be-used"></a>Computemodule weisen den Status „Unbekannt“ auf und können nicht verwendet werden

#### <a name="error-description"></a>Fehlerbeschreibung

Alle Module auf dem Gerät weisen den Status „Unbekannt“ auf und können nicht verwendet werden. Der Status „Unbekannt“ liegt auch noch nach einem Neustart vor.<!--Original Support ticket relates to trying to deploy a container app on a Hub. Based on the work item, I assume the error description should not be that specific, and that the error applies to Azure Stack Edge Devices, which is the focus of this troubleshooting.-->

#### <a name="suggested-solution"></a>Vorgeschlagene Lösung

Löschen Sie den IoT Edge-Dienst, und stellen Sie dann das Modul bzw. die Module erneut bereit. Weitere Informationen finden Sie unter [Entfernen des IoT Edge-Diensts](../articles/databox-online/azure-stack-edge-gpu-manage-compute.md#remove-iot-edge-service).


### <a name="modules-show-as-running-but-are-not-working"></a>Module werden als ausgeführt angezeigt, funktionieren jedoch nicht.

#### <a name="error-description"></a>Fehlerbeschreibung

Als Laufzeitstatus des Moduls wird „ausgeführt“ angezeigt, aber Sie sehen nicht die erwarteten Ergebnisse. 

Die Ursache dieser Bedingung könnte sein, dass eine Modulroutenkonfiguration nicht funktioniert oder `edgehub` Nachrichten nicht wie erwartet weiterleitet. Sie können die `edgehub`-Protokolle überprüfen. Wenn Sie Fehler sehen, beispielsweise die Verbindung mit dem IoT Hub-Dienst nicht hergestellt werden kann, sind Konnektivitätsprobleme der häufigste Grund. Die Ursache der Konnektivitätsprobleme könnte sein, dass der AMPQ-Port, den der IoT Hub-Dienst als Standardport für die Kommunikation verwendet, blockiert ist oder der Webproxyserver diese Nachrichten blockiert.

#### <a name="suggested-solution"></a>Vorgeschlagene Lösung

Führen Sie die folgenden Schritte aus:
1. Um den Fehler zu beheben, wechseln Sie zur IoT Hub-Ressource für Ihr Gerät, und wählen Sie dann Ihr Edge-Gerät aus. 
1. Gehen Sie zu **Module festlegen > Laufzeiteinstellungen**. 
1. Fügen Sie die Umgebungsvariable `Upstream protocol` hinzu, und weisen Sie ihr den Wert `AMQPWS` zu. Die in diesem Fall konfigurierten Meldungen werden über den Port 443 über WebSockets gesendet.

### <a name="modules-show-as-running-but-do-not-have-an-ip-assigned"></a>Module werden als ausgeführt angezeigt, aber ihnen ist keine IP-Adresse zugewiesen.

#### <a name="error-description"></a>Fehlerbeschreibung

Als Laufzeitstatus des Moduls wird „ausgeführt“ angezeigt, aber der containerisierten App ist keine IP-Adresse zugewiesen. 

Ursache dieser Bedingung ist, dass der Bereich der IP-Adressen, den Sie für „Externe Dienst-IP-Adressen für Kubernetes“ angegeben haben, nicht ausreicht. Erweitern Sie diesen Bereich, um sicherzustellen, dass alle bereitgestellten Container oder VMs abgedeckt werden.

#### <a name="suggested-solution"></a>Vorgeschlagene Lösung

Gehen Sie auf der lokalen Webbenutzeroberfläche Ihres Geräts wie folgt vor:
1. Wechseln Sie zur Seite **Compute**. Wählen Sie den Port aus, für den Sie das Computenetzwerk aktiviert haben. 
1. Geben Sie einen statischen, zusammenhängenden Bereich von IP-Adressen für **Externe Dienst-IP-Adressen für Kubernetes** ein. Sie benötigen eine IP-Adresse für den `edgehub`-Dienst. Außerdem benötigen Sie für jedes IoT Edge-Modul und für jeden virtuellen Computer, den Sie bereitstellen, eine IP-Adresse. 
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

Standardmäßig ist der IoT-Diensttyp **Load Balancer**, und dem Dienst werden extern ausgerichtete IP-Adressen zugewiesen. Wenn eine Anwendung Kubernetes-Pods innerhalb des Kubernetes-Clusters für den Zugriff auf andere Pods im Cluster benötigt, müssen Sie den Dienst möglicherweise als Cluster-IP-Dienst statt als Load Balancer-Dienst konfigurieren. Weitere Informationen finden Sie unter [Kubernetes-Netzwerke auf Ihrem Azure Stack Edge Pro-GPU-Gerät](../articles/databox-online/azure-stack-edge-gpu-kubernetes-networking.md).

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