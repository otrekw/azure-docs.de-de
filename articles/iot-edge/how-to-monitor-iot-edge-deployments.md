---
title: Überwachen von IoT Edge-Bereitstellungen – Azure IoT Edge
description: Überwachung von Bereitstellungen, einschließlich der gemeldeten EdgeHub- und EdgeAgent-Eigenschaften sowie der Metriken der automatischen Bereitstellung.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 04/21/2020
ms.topic: conceptual
ms.reviewer: veyalla
ms.service: iot-edge
ms.custom: devx-track-azurecli
services: iot-edge
ms.openlocfilehash: 39e7bb5c151d490e79ef111589f52f260c3e6c7a
ms.sourcegitcommit: afb79a35e687a91270973990ff111ef90634f142
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2021
ms.locfileid: "107483162"
---
# <a name="monitor-iot-edge-deployments"></a>Überwachen von IoT Edge-Bereitstellungen

[!INCLUDE [iot-edge-version-all-supported](../../includes/iot-edge-version-all-supported.md)]

Mit den Berichterstellungsfunktionen von Azure IoT Edge können Sie die Echtzeitinformationen der Module überwachen, die auf Ihren IoT Edge-Geräten bereitgestellt wurden. Der IoT Hub-Dienst ruft den Status von den Geräten ab und macht diese für den Operator verfügbar. Die Überwachung ist auch für [umfangreiche Bereitstellungen](module-deployment-monitoring.md) wichtig, wie z. B. automatische und mehrstufige Bereitstellungen.

Geräte und Module verfügen über ähnliche Daten (beispielsweise für die Konnektivität). Aus diesem Grund werden die Werte basierend auf der Geräte-ID oder der Modul-ID erfasst.

Der IoT Hub-Dienst erfasst die von Geräte- und Modulzwillingen gemeldeten Daten und gibt die Anzahl der unterschiedlichen Status an, die Geräte aufweisen können. Diese Daten werden vom IoT Hub-Dienst in vier Gruppen von Metriken eingeteilt:

| type | BESCHREIBUNG |
| --- | ---|
| Ziel | Zeigt die IoT Edge-Geräte an, die den Zielbedingungen für die Bereitstellung entsprechen. |
| Übernommen | Zeigt die IoT Edge-Zielgeräte an, für die keine andere Bereitstellung mit höherer Priorität vorhanden ist. |
| Erfolg gemeldet | Zeigt die IoT Edge-Geräte an, die gemeldet haben, dass die Module erfolgreich bereitgestellt wurden. |
| Fehler gemeldet | Zeigt die IoT Edge-Geräte an, die gemeldet haben, dass ein oder mehrere Module nicht erfolgreich bereitgestellt wurden. Stellen Sie zum weiteren Untersuchen des Fehlers eine Remoteverbindung mit diesen Geräten her, und zeigen Sie die Protokolldateien an. |

Der IoT Hub-Dienst stellt diese Daten zur Überwachung im Azure-Portal und über die Azure CLI zur Verfügung.

## <a name="monitor-a-deployment-in-the-azure-portal"></a>Überwachen einer Bereitstellung im Azure-Portal

So zeigen Sie ausführliche Informationen zu einer Bereitstellung an und überwachen die Geräte, die diese ausführen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu Ihrer IoT Hub-Instanz.
1. Wählen Sie im Menü auf der linken Seite die Option **IoT Edge** aus.
1. Wählen Sie die Registerkarte **IoT Edge-Bereitstellungen** aus.
1. Prüfen Sie die Bereitstellungsliste.  Für jede Bereitstellung können Sie die folgenden Details anzeigen:

    | Column | Beschreibung |
    | --- | --- |
    | id | Der Name der Bereitstellung |
    | type | Der Typ der Bereitstellung – entweder **Bereitstellung** oder **Mehrstufige Bereitstellung**. |
    | Zielbedingung | Das Tag zur Definition von Zielgeräten. |
    | Priority | Die Prioritätsnummer, die der Bereitstellung zugewiesen wurde. |
    | Systemmetriken | Die Anzahl von Gerätezwillingen in IoT Hub, die die Zielbedingung erfüllen. **Applied** gibt die Anzahl von Geräten an, auf deren Modulzwillinge in IoT Hub die Bereitstellungsinhalte angewendet wurden. |
    | Gerätemetriken | Die Anzahl von IoT Edge-Geräten, für die von der IoT Edge-Clientruntime eine Erfolgs- oder Fehlermeldung ausgegeben wurde. |
    | Benutzerdefinierte Metriken | Die Anzahl von IoT Edge-Geräten, die Daten für in der Bereitstellung definierte Metriken melden. |
    | Erstellungszeit | Der Zeitstempel der Bereitstellungserstellung. Dieser Zeitstempel wird zur Konfliktlösung verwendet, wenn zwei Bereitstellungen dieselbe Priorität haben. |

1. Wählen Sie die Bereitstellung aus, die Sie überwachen möchten.  
1. Führen Sie auf der Seite **Bereitstellungsdetails** einen Bildlauf zum unteren Abschnitt durch, und wählen Sie die Registerkarte **Zielbedingung** aus. Wählen Sie **Anzeigen** aus, um eine Liste mit Geräten anzuzeigen, die der Zielbedingung entsprechen. Sie können die Bedingung ändern und auch die **Priorität** festlegen. Wählen Sie **Speichern** aus, wenn Sie Änderungen vorgenommen haben.

   ![Anzeigen von Zielgeräten für eine Bereitstellung](./media/how-to-monitor-iot-edge-deployments/target-devices.png)

1. Wählen Sie die Registerkarte **Metriken** aus. Wenn Sie eine Metrik im Dropdownmenü **Metrik auswählen** ausgewählt haben, wird eine Schaltfläche **Anzeigen** angezeigt, über die Sie auf die Ergebnisse zugreifen können. Sie können auch **Metriken bearbeiten** auswählen, um die Kriterien für benutzerdefinierte Metriken anzupassen, die Sie definiert haben. Wählen Sie **Speichern** aus, wenn Sie Änderungen vorgenommen haben.

   ![Anzeigen von Metriken für eine Bereitstellung](./media/how-to-monitor-iot-edge-deployments/deployment-metrics-tab.png)

Informationen zum Durchführen von Änderungen an Ihrer Bereitstellung finden Sie unter [Ändern einer Bereitstellung](how-to-deploy-at-scale.md#modify-a-deployment).

## <a name="monitor-a-deployment-with-azure-cli"></a>Überwachen einer Bereitstellung über die Azure-Befehlszeilenschnittstelle

Mit dem Befehl [az iot edge deployment show](/cli/azure/iot/edge/deployment) können Sie die Details einer einzelnen Bereitstellung anzeigen:

```azurecli
az iot edge deployment show --deployment-id [deployment id] --hub-name [hub name]
```

Für diesen Befehl werden die folgenden Parameter verwendet:

* **--deployment-id**: Der Name der Bereitstellung, die im IoT-Hub vorhanden ist Erforderlicher Parameter.
* **--hub-name** – Name des IoT Hub, in dem die Bereitstellung vorhanden ist. Der Hub muss aus dem aktuellen Abonnement stammen. Wechseln Sie mit dem Befehl `az account set -s [subscription name]` zum gewünschten Abonnement.

Überprüfen Sie die Bereitstellung im Befehlsfenster.  Die Eigenschaft **metrics** enthält eine Anzahl für jede Metrik, die von den einzelnen Hubs ausgewertet wird:

* **targetedCount** – eine Systemmetrik, die die Anzahl der Gerätezwillinge in IoT Hub angibt, die die Zielbedingung erfüllen.
* **appliedCount**: Eine Systemmetrik, die die Anzahl von Geräten angibt, auf deren Modulzwillinge in IoT Hub die Bereitstellungsinhalte angewendet wurden.
* **reportedSuccessfulCount**: Eine Gerätemetrik, die die Anzahl von IoT Edge-Geräten in der Bereitstellung angibt, für die von der IoT Edge-Clientruntime eine Erfolgsmeldung ausgegeben wurde.
* **reportedFailedCount**: Eine Gerätemetrik, die die Anzahl von IoT -Geräten in der Bereitstellung angibt, für die von der IoT Edge-Clientruntime eine Fehlermeldung ausgegeben wurde.

Sie können mit dem Befehl [az iot edge deployment show-metric](/cli/azure/iot/edge/deployment) eine Liste der Geräte-IDs oder Objekte für jede der Metriken anzeigen:

```azurecli
az iot edge deployment show-metric --deployment-id [deployment id] --metric-id [metric id] --hub-name [hub name]
```

Für diesen Befehl werden die folgenden Parameter verwendet:

* **--deployment-id**: Der Name der Bereitstellung, die im IoT-Hub vorhanden ist
* **--metric-id:** Der Name der Metrik, für die Sie eine Liste der Geräte-IDs anzeigen möchten, z. B. `reportedFailedCount`.
* **--hub-name** – Name des IoT Hub, in dem die Bereitstellung vorhanden ist. Der Hub muss aus dem aktuellen Abonnement stammen. Wechseln Sie mit dem Befehl `az account set -s [subscription name]` zum gewünschten Abonnement.

Informationen zum Durchführen von Änderungen an Ihrer Bereitstellung finden Sie unter [Ändern einer Bereitstellung](how-to-deploy-cli-at-scale.md#modify-a-deployment).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [Modulzwillinge überwachen](how-to-monitor-module-twins.md) (primär die IoT Edge-Agent- und IoT Edge-Hub-Runtimemodule), um die Konnektivität und Integrität Ihrer IoT Edge-Bereitstellungen zu gewährleisten.
