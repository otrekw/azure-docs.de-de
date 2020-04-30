---
title: Erstellen einer Azure Kubernetes Service-Clusterressource
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie eine Azure Kubernetes Service-Ressource (AKS) erstellen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: d01245a76c5210a1843307b543f98d4f1d59d2cc
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "80877821"
---
## <a name="create-an-azure-kubernetes-service-cluster-resource"></a>Erstellen einer Azure Kubernetes Service-Clusterressource

1. Wechseln Sie zu [Azure Kubernetes Service](https://ms.portal.azure.com/#create/microsoft.aks), und klicken Sie auf **Erstellen**.

1. Geben Sie folgende Informationen auf der Registerkarte **Grundeinstellungen** an:

    |Einstellung|value|
    |--|--|
    |Subscription|Wählen Sie ein entsprechendes Abonnement aus.|
    |Resource group|Wählen Sie eine verfügbare Ressourcengruppe aus.|
    |Kubernetes-Clustername|Geben Sie einen Namen ein (Kleinbuchstaben).|
    |Region|Wählen Sie einen nahe gelegenen Speicherort aus.|
    |Kubernetes-Version|Jeder Wert, der als **(Standard)** markiert ist.|
    |DNS-Namenspräfix|Wird automatisch erstellt, kann jedoch überschrieben werden.|
    |Knotengröße|Standard DS2 v2:<br>`2 vCPUs`, `7 GB`|
    |Knotenanzahl|Lassen Sie den Schieberegler auf dem Standardwert.|

1. Behalten Sie auf der Registerkarte **Skalieren** die Standardwerte für **virtuelle Knoten** und **VM-Skalierungsgruppen** bei.
1. Behalten Sie Registerkarte **Authentifizierung** die Standardwerte für **Dienstprinzipal** und **RBAC aktivieren** bei.
1. Geben Sie auf der Registerkarte **Netzwerk** die folgende Auswahl ein:

    |Einstellung|value|
    |--|--|
    |HTTP-Anwendungsrouting|Nein|
    |Netzwerkkonfiguration|Basic|

1. Stellen Sie auf der Registerkarte **Überwachung** sicher, dass **Containerüberwachung aktivieren** auf **Ja** festgelegt ist, und behalten Sie den Standardwert für **Log Analytics-Arbeitsbereich** bei.
1. Lassen Sie auf der Registerkarte **Tags** die Name/Wert-Paare vorerst leer.
1. Klicken Sie auf **Überprüfen und erstellen**.
1. Wenn die Überprüfung erfolgreich war, wählen Sie **Erstellen** aus.

> [!NOTE]
> Wenn bei der Überprüfung ein Fehler auftritt, ist der Grund dafür möglicherweise ein Fehler beim „Dienstprinzipal“. Navigieren Sie zurück zur Registerkarte **Authentifizierung** und dann zurück zu **Überprüfen und erstellen**, wo die Überprüfung ausgeführt und bestanden werden sollte.
