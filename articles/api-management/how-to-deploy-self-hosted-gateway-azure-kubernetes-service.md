---
title: Bereitstellen eines selbstgehosteten Gateways für Azure Kubernetes Service | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine selbstgehostete Gatewaykomponente von Azure API Management für Azure Kubernetes Service bereitstellen.
services: api-management
documentationcenter: ''
author: miaojiang
manager: gwallace
editor: ''
ms.service: api-management
ms.topic: article
ms.date: 04/26/2020
ms.author: apimpm
ms.openlocfilehash: abf7e6d7032e7e5dc35b2f4397a5630d45d762de
ms.sourcegitcommit: 67bddb15f90fb7e845ca739d16ad568cbc368c06
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82204835"
---
# <a name="deploy-to-azure-kubernetes-service"></a>Bereitstellen für Azure Kubernetes Service

In diesem Artikel werden die Schritte für die Bereitstellung einer selbstgehosteten Gatewaykomponente von Azure API Management für den [Azure Kubernetes Service](https://azure.microsoft.com/services/kubernetes-service/) beschrieben. Informationen zur Bereitstellung eines selbstgehosteten Gateways in einem Kubernetes-Cluster finden Sie in diesem [Dokument](how-to-deploy-self-hosted-gateway-kubernetes.md).

## <a name="prerequisites"></a>Voraussetzungen

- [Erstellen einer neuen Azure API Management-Dienstinstanz](get-started-create-service-instance.md)
- [Erstellen eines Azure Kubernetes-Clusters](../aks/kubernetes-walkthrough-portal.md)
- [Stellen Sie eine Gatewayressource in Ihrer API Management-Instanz bereit](api-management-howto-provision-self-hosted-gateway.md).

## <a name="deploy-the-self-hosted-gateway-to-aks"></a>Bereitstellen des selbstgehosteten Gateways für AKS

1. Wählen Sie **Gateways** unter **Einstellungen** aus.
2. Wählen Sie die selbstgehostete Gatewayressource aus, die Sie bereitstellen möchten.
3. Wählen Sie **Bereitstellung** aus.
4. Beachten Sie, dass ein neues Token im Textfeld **Token** automatisch für Sie mit den Standardwerten **Ablauf** und **Geheimer Schlüssel** generiert wurde. Passen Sie einen Wert oder beide Werte an, und wählen Sie **Generieren** aus, um ein neues Token zu erstellen.
5. Stellen Sie sicher, dass **Kubernetes** unter **Bereitstellungsskripts** ausgewählt ist.
6. Wählen Sie den **<Gatewayname>.yml**-Dateilink neben **Bereitstellung** aus, um die Datei herunterzuladen.
7. Passen Sie die Portzuordnungen und den Containernamen in der YML-Datei nach Bedarf an.
8. Abhängig von Ihrem Szenario müssen Sie möglicherweise den [Diensttyp](https://docs.microsoft.com/azure/aks/concepts-network#services) ändern. Standardwert: `NodePort`.
9. Wählen Sie das Symbol **Kopieren** rechts neben dem Textfeld **Bereitstellen** aus, um den `kubectl`-Befehl in der Zwischenablage zu speichern.
10. Fügen Sie den Befehl in das Terminalfenster (oder Befehlsfenster) ein. Beachten Sie, dass der Befehl erwartet, dass die heruntergeladene Umgebungsdatei im aktuellen Verzeichnis vorhanden ist.
```console
    kubectl apply -f <gateway-name>.yaml
```
11. Führen Sie den Befehl aus. Der Befehl weist Ihren AKS-Cluster an, den Container mithilfe des aus der Microsoft Container Registry heruntergeladenen Images des selbstgehosteten Gateways auszuführen, und den Container zu konfigurieren, um die Ports HTTP (8080) und HTTPS (443) verfügbar zu machen.
12. Führen Sie den folgenden Befehl aus, um zu überprüfen, ob der Gatewaypod ausgeführt wird. Beachten Sie, dass Ihr Podname anders ist.
```console
kubectl get pods
NAME                                   READY     STATUS    RESTARTS   AGE
contoso-apim-gateway-59f5fb94c-s9stz   1/1       Running   0          1m
```
13. Führen Sie den folgenden Befehl aus, um zu überprüfen, ob der Gatewaydienst ausgeführt wird. Beachten Sie, dass Ihr Dienstname und die IP-Adressen anders sind.
```console
kubectl get services
NAME             TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
contosogateway   NodePort    10.110.230.87   <none>        80:32504/TCP,443:30043/TCP   1m
```
14. Navigieren Sie zurück zum Azure-Portal, und vergewissern Sie sich, dass der soeben bereitgestellte Gatewayknoten einen fehlerfreien Status meldet.

> [!TIP]
> Verwenden Sie den Befehl <code>kubectl logs <gateway-pod-name></code>, um eine Momentaufnahme des Protokolls des selbstgehosteten Gateways anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum selbstgehosteten Gateway finden Sie unter [Azure API Management: Übersicht über das selbstgehostete Gateway](self-hosted-gateway-overview.md).
* Erfahren Sie mehr über [Azure Kubernetes Service](https://docs.microsoft.com/azure/aks/intro-kubernetes).
* Erfahren Sie mehr zum [Konfigurieren und Beibehalten von Protokollen in der Cloud](how-to-configure-cloud-metrics-logs.md).
* * Erfahren Sie mehr zum [lokalen Konfigurieren und Beibehalten von Protokollen](how-to-configure-local-metrics-logs.md).




