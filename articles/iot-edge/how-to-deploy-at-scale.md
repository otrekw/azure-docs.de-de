---
title: Bereitstellen von Modulen jeder Größenordnung im Azure-Portal – Azure IoT Edge
description: Verwenden des Azure-Portals zum Erstellen automatischer Bereitstellungen für Gruppen von IoT Edge-Geräten
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 10/13/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: db27a466ca5f1370e8b43ceb472f5deeaba509f1
ms.sourcegitcommit: 5f32f03eeb892bf0d023b23bd709e642d1812696
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2021
ms.locfileid: "103200331"
---
# <a name="deploy-iot-edge-modules-at-scale-using-the-azure-portal"></a>Bedarfsgerechtes Bereitstellen von IoT Edge-Modulen mithilfe des Azure-Portals

[!INCLUDE [iot-edge-version-all-supported](../../includes/iot-edge-version-all-supported.md)]

Erstellen Sie eine **automatische IoT Edge-Bereitstellung** im Azure-Portal zum gleichzeitigen Verwalten laufender Bereitstellungen vieler Geräte. Automatische Bereitstellungen für IoT Edge sind Teil des Features [Automatische Geräteverwaltung](../iot-hub/iot-hub-automatic-device-management.md) von IoT Hub. Bereitstellungen sind dynamische Prozesse, mit denen Sie mehrere Module auf mehreren Geräten bereitstellen, Status und Integrität der Module nachverfolgen und bei Bedarf Änderungen vornehmen können.

Weitere Informationen finden Sie unter [Grundlegendes zu automatischen IoT Edge-Bereitstellungen für einzelne Geräte oder nach Bedarf](module-deployment-monitoring.md).

## <a name="identify-devices-using-tags"></a>Identifizieren von Geräten mithilfe von Tags

Bevor Sie eine Bereitstellung erstellen können, müssen Sie angeben können, welche Geräte Sie ansprechen möchten. Azure IoT Edge erkennt Geräte anhand von **Tags** im Gerätezwilling. Jedes Gerät kann mehrere Tags aufweisen, die Sie auf eine für Ihre Lösung sinnvolle Weise definieren können.

Wenn Sie z.B. einen Campus von intelligenten Gebäuden verwalten, könnten Sie einem Gerät Standort, Raumtyp und Umgebungstags hinzufügen:

```json
"tags":{
  "location":{
    "building": "20",
    "floor": "2"
  },
  "roomtype": "conference",
  "environment": "prod"
}
```

Weitere Informationen zu Gerätezwillingen und Tags finden Sie unter [Verstehen und Verwenden von Gerätezwillingen in IoT Hub](../iot-hub/iot-hub-devguide-device-twins.md).

## <a name="create-a-deployment"></a>Erstellen einer Bereitstellung

IoT Edge bietet zwei verschiedene Arten von automatischen Bereitstellungen, die Sie zum Anpassen Ihres Szenarios verwenden können. Sie können eine Standard *bereitstellung* erstellen, die diese Systemlaufzeitmodule und alle zusätzlichen Module und Routen enthält. Jedes Gerät kann nur eine Bereitstellung anwenden. Oder Sie können eine *mehrstufige Bereitstellung* erstellen, die nur benutzerdefinierte Module und Routen (und nicht die Systemlaufzeit) enthält. Viele mehrstufige Bereitstellungen können auf einem Gerät – basierend auf einer Standardbereitstellung – kombiniert werden. Weitere Informationen dazu, wie die beiden Typen automatischer Bereitstellungen zusammenarbeiten, finden Sie unter [Grundlegendes zu automatischen IoT Edge-Bereitstellungen für einzelne Geräte oder nach Bedarf](module-deployment-monitoring.md).

Die Schritte zum Erstellen einer Bereitstellung und einer mehrstufigen Bereitstellung sind sehr ähnlich. In den folgenden Schritten wird auf eventuelle Unterschiede hingewiesen.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem IoT Hub.
1. Wählen Sie im Menü im linken Bereich unter **Automatische Geräteverwaltung** die Option **IoT Edge** aus.
1. Wählen Sie auf der oberen Leiste **Bereitstellung erstellen** oder **Mehrstufige Bereitstellung erstellen** aus.

Zum Erstellen einer Bereitstellung müssen fünf Schritte ausgeführt werden. Diese werden in den folgenden Abschnitten exemplarisch beschrieben.

>[!NOTE]
>Die Schritte in diesem Artikel entsprechen der aktuellen Schemaversion des IoT Edge-Agents und -Hubs. Die Schemaversion 1.1 wurde zusammen mit der IoT Edge-Version 1.0.10 veröffentlicht und ermöglicht die Verwendung von Startreihenfolgen für Module und Features für die Priorisierung von Routen.
>
>Bei einer Bereitstellung auf einem Gerät, auf dem Version 1.0.9 oder früher ausgeführt wird, bearbeiten Sie die **Runtimeeinstellungen** im Schritt **Module** des Assistenten, um Schemaversion 1.0 zu verwenden.

### <a name="step-1-name-and-label"></a>Schritt 1: Benennen und Bezeichnen

1. Geben Sie Ihrer Bereitstellung einen eindeutigen Namen, der bis zu 128 Kleinbuchstaben umfasst. Verwenden Sie dabei weder Leerzeichen noch die folgenden ungültigen Zeichen: `& ^ [ ] { } \ | " < > /`.
1. Sie können Bezeichnungen als Schlüssel-Wert-Paare hinzufügen, um Ihre Bereitstellungen im Blick zu behalten. Beispiel: **HostPlatform** und **Linux** oder **Version** und **3.0.1**.
1. Klicken Sie auf **Weiter: Module**, um zu Schritt 2 zu wechseln.

### <a name="step-2-modules"></a>Schritt 2: Module

Sie können einer Bereitstellung bis zu 50 Module hinzufügen. Wenn Sie eine Bereitstellung ohne Module erstellen, werden alle aktuellen Module von den Zielgeräten entfernt.

In Bereitstellungen können Sie die Einstellungen für die IoT Edge-Agent- und IoT Edge-Hub-Module verwalten. Wählen Sie **Laufzeiteinstellungen** aus, um die beiden Laufzeitmodule zu konfigurieren. Weil die Laufzeitmodule in einer mehrstufigen Bereitstellung nicht enthalten sind, können sie nicht konfiguriert werden.

So fügen Sie benutzerdefinierten Code als Modul oder aber ein Azure-Dienstmodul manuell hinzu:

1. Geben Sie im Abschnitt **Container Registry-Einstellungen** der Seite die Anmeldeinformationen für den Zugriff auf private Containerregistrierungen mit Ihren Modulimages an.
1. Wählen Sie im Abschnitt **IoT Edge-Module** der Seite die Option **Hinzufügen** aus.
1. Wählen Sie einen der drei Modultypen aus dem Dropdownmenü aus:

   * **IoT Edge-Modul** – Geben Sie den Modulnamen und den URI des Containerimages an. Beispielsweise lautet der Image-URI für das Beispielmodul „SimulatedTemperatureSensor“ `mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0`. Wenn das Modulimage in einer privaten Containerregistrierung gespeichert ist, fügen Sie die Anmeldeinformationen auf dieser Seite hinzu, um auf das Image zugreifen zu können.
   * **Marketplace-Modul** – Module, die im Azure Marketplace gehostet werden. Weil einige Marketplace-Module eine zusätzliche Konfiguration erfordern, überprüfen Sie die Moduldetails in der Liste [Azure Marketplace IoT-Edge-Module](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules).
   * **Azure Stream Analytics-Modul** – Module, die aus einer Azure Stream Analytics-Workload generiert wurden.

1. Wiederholen Sie bei Bedarf die Schritte 2 und 3, um Ihrer Bereitstellung weitere Module hinzuzufügen.

Nachdem Sie ein Modul zu einer Bereitstellung hinzugefügt haben, können Sie dessen Namen zum Öffnen der Seite **IoT Edge-Modul aktualisieren** auswählen. Auf dieser Seite können Sie die Moduleinstellungen und Umgebungsvariablen bearbeiten und Optionen, Startreihenfolgen und einen Modulzwilling erstellen. Wenn Sie ein Modul aus dem Marketplace hinzugefügt haben, sind darin einige dieser Parameter möglicherweise bereits ausgefüllt. Weitere Informationen zu den verfügbaren Moduleinstellungen finden Sie unter [Bereitstellen von Modulen und Einrichten von Routen in IoT Edge](module-composition.md#module-configuration-and-management).

Wenn Sie eine mehrstufige Bereitstellung erstellen, konfigurieren Sie eventuell ein Modul, das in anderen Bereitstellungen für dieselben Geräte bereits vorhanden ist. Möchten Sie den Modulzwilling aktualisieren, ohne dadurch andere Versionen zu überschreiben, öffnen Sie die Registerkarte **Einstellungen für Modulzwilling**. Erstellen Sie eine neue **Eigenschaft für Modulzwilling** mit einem eindeutigen Namen für einen Unterabschnitt innerhalb der gewünschten Eigenschaften des Modulzwillings, beispielsweise `properties.desired.settings`. Wenn Sie Eigenschaften nur im Feld `properties.desired` definieren, werden dadurch die gewünschten Eigenschaften für das Modul überschrieben, das in beliebigen Bereitstellungen mit niedrigerer Priorität definiert wurde.

![Festlegen einer Eigenschaft für Modulzwilling für mehrstufige Bereitstellung](./media/how-to-deploy-monitor/module-twin-property.png)

Weitere Informationen zur Konfiguration von Modulzwillingen in mehrstufigen Bereitstellungen finden Sie unter [Mehrstufige Bereitstellung](module-deployment-monitoring.md#layered-deployment).

Nachdem Sie alle Module für eine Bereitstellung konfiguriert haben, wählen Sie **Weiter: Routen** aus, um zu Schritt 3 zu wechseln.

### <a name="step-3-routes"></a>Schritt 3: Routen

Auf der Registerkarte **Routen** definieren Sie, wie Nachrichten zwischen Modulen und dem IoT Hub übergeben werden. Nachrichten werden mit Name-Wert-Paaren erstellt.

Eine Route mit dem Namen **route** und dem Wert **FROM /messages/\* INTO $upstream** würde alle Nachrichten übernehmen, die von allen Modulen ausgegeben und an Ihren IoT-Hub gesendet werden.  

Die Parameter **Priority** und **Time to live** sind optionale Parameter, die Sie in eine Routendefinition einschließen können. Mit dem Parameter „Priority“ können Sie auswählen, für welche Routen die Nachrichten zuerst bzw. zuletzt verarbeitet werden sollen. Die Priorität wird durch das Festlegen einer Zahl zwischen 0 und 9 festgelegt, wobei 0 für die höchste Priorität steht. Mit dem Parameter „Time to live“ können Sie deklarieren, wie lange Nachrichten in dieser Route aufbewahrt werden sollen, bis sie entweder verarbeitet oder aus der Warteschlange entfernt werden.

Weitere Informationen zum Erstellen von Routen finden Sie unter [Deklarieren von Routen](module-composition.md#declare-routes).

Klicken Sie auf **Weiter: Metriken**.

### <a name="step-4-metrics"></a>Schritt 4: Metriken

Metriken bieten zusammenfassende Angaben zu den verschiedenen Zuständen, die ein Gerät nach dem Anwenden von Konfigurationsinhalten möglicherweise zurückmeldet.

1. Geben Sie unter **Metrikname** einen Namen ein.

1. Geben Sie eine Abfrage unter **Metrikkriterien** ein. Diese Abfrage basiert auf [gemeldeten Eigenschaften](module-edgeagent-edgehub.md#edgehub-reported-properties) eines IoT Edge-Hub-Modulzwillings. Die Metrik stellt die Anzahl der von der Abfrage zurückgegebenen Zeilen dar.

   Beispiel:

   ```sql
   SELECT deviceId FROM devices
     WHERE properties.reported.lastDesiredStatus.code = 200
   ```

Klicken Sie auf **Weiter: Zielgeräte**.

### <a name="step-5-target-devices"></a>Schritt 5: Zielgeräte

Mit der Tageigenschaft Ihrer Geräte wählen Sie bestimmte Geräte als Ziele aus, die diese Bereitstellung erhalten sollen.

Da mehrere Bereitstellungen dasselbe Gerät als Ziel verwenden können, sollten Sie für jede Bereitstellung eine Priorität festlegen. Wenn irgendwann ein Konflikt auftritt, „gewinnt“ die Bereitstellung mit der höchsten Priorität (höhere Werte deuten auf eine höhere Priorität hin). Haben zwei Bereitstellungen dieselbe Priorität, dann wird jeweils diejenige verwendet, die später erstellt wurde.

Wenn mehrere Bereitstellungen auf dasselbe Gerät abzielen, wird nur die Bereitstellung mit der höheren Priorität angewendet. Wenn mehrere mehrstufige Bereitstellungen auf dasselbe Gerät abzielen, werden alle angewendet. Werden aber irgendwelche Eigenschaften dupliziert, z. B. zwei Routen mit demselben Namen, überschreibt die Eigenschaft aus der mehrstufigen Bereitstellung mit der höheren Priorität die restlichen Eigenschaften.

Eine mehrstufige Bereitstellung für ein Gerät muss eine höhere Priorität als die Basisbereitstellung haben, damit sie angewendet wird.

1. Geben Sie eine positive ganze Zahl als **Priorität** für die Bereitstellung ein.
1. Geben Sie unter **Zielbedingung** eine Bedingung ein, um festzulegen, auf welche Geräte diese Bereitstellung angewendet werden soll.  Die Bedingung basiert auf den Gerätezwillingstags oder auf den gemeldeten Gerätezwillingseigenschaften und muss dem Ausdrucksformat entsprechen.  Zum Beispiel: `tags.environment='test'` oder `properties.reported.devicemodel='4000x'`.

Klicken Sie auf **Weiter: Überprüfen + erstellen**, um zum letzten Schritt zu wechseln.

### <a name="step-6-review-and-create"></a>Schritt 6: Überprüfen und Erstellen

Überprüfen Sie Ihre Bereitstellungsinformationen, und wählen Sie **Erstellen** aus.

Informationen zum Überwachen der Bereitstellung finden Sie unter [Überwachen von IoT Edge-Bereitstellungen](how-to-monitor-iot-edge-deployments.md).

## <a name="modify-a-deployment"></a>Ändern einer Bereitstellung

Wenn Sie Änderungen an einer Bereitstellung vornehmen, werden diese sofort auf alle Zielgeräte repliziert. Sie können die folgenden Einstellungen und Features für eine vorhandene Bereitstellung ändern:

* Zielbedingungen
* Benutzerdefinierte Metriken
* Bezeichnungen
* `Tags`
* Gewünschte Eigenschaften

### <a name="modify-target-conditions-custom-metrics-and-labels"></a>Ändern von Zielbedingungen, benutzerdefinierten Metriken und Bezeichnungen

1. Wählen Sie in Ihrem IoT-Hub **IoT Edge** im linken Bereich aus.
1. Wählen Sie die Registerkarte **IoT Edge-Bereitstellungen** und dann die Bereitstellung aus, die Sie konfigurieren möchten.
1. Wählen Sie die Registerkarte **Zielbedingung** aus. Ändern Sie die **Zielbedingung**, um die gewünschten Geräte anzuvisieren. Sie können auch die **Priorität** einstellen.  Wählen Sie **Speichern** aus.

    Wenn Sie die Zielbedingung ändern, erfolgen die nachfolgend aufgeführten Anpassungen:

    * Wenn ein Gerät die alte Zielbedingung nicht erfüllt, wohl aber die neue, und diese Bereitstellung die höchste Priorität für das Gerät hat, wird diese Bereitstellung auf das Gerät angewendet.
    * Wenn ein Gerät, auf dem diese Bereitstellung gegenwärtig ausgeführt wird, die Zielbedingung nicht mehr erfüllt, wird diese Bereitstellung deinstalliert, und die Bereitstellung mit der nächsthöheren Priorität wird angewendet.
    * Wenn ein Gerät, auf dem diese Bereitstellung gegenwärtig ausgeführt wird, diese Zielbedingung wie auch die Zielbedingungen aller anderen Bereitstellungen nicht erfüllt, erfolgt keine Änderung auf dem Gerät. Das Gerät führt die aktuellen Module im aktuellen Zustand weiter aus, wird aber nicht mehr als Teil dieser Bereitstellung verwaltet. Sobald es die Zielbedingung einer anderen Bereitstellung erfüllt, wird diese Bereitstellung deinstalliert, und die neue Bereitstellung wird angewendet.

1. Wählen Sie die Registerkarte **Metriken** aus, und klicken Sie auf die Schaltfläche **Metriken bearbeiten**. Fügen Sie benutzerdefinierte Metriken hinzu oder ändern Sie sie, wobei die Beispielsyntax als Leitfaden dient. Wählen Sie **Speichern** aus.

    ![Bearbeiten benutzerdefinierter Metriken in einer Bereitstellung](./media/how-to-deploy-monitor/metric-list.png)

1. Wählen Sie die Registerkarte **Bezeichnungen** aus, nehmen Sie die gewünschten Änderungen vor, und wählen Sie dann **Speichern** aus.

## <a name="delete-a-deployment"></a>Löschen einer Bereitstellung

Wenn Sie eine Bereitstellung löschen, übernehmen alle bereitgestellten Geräte ihre nächste Bereitstellung mit der höchsten Priorität. Wenn Ihre Geräte die Zielbedingung keiner anderen Bereitstellung erfüllen, werden die Module beim Löschen der Bereitstellung nicht entfernt.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu Ihrer IoT Hub-Instanz.
1. Wählen Sie **IoT Edge** aus.
1. Wählen Sie die Registerkarte **IoT Edge-Bereitstellungen** aus.

   ![Anzeigen von IoT Edge-Bereitstellungen](./media/how-to-deploy-monitor/iot-edge-deployments.png)

1. Wählen Sie die zu löschende Bereitstellung durch Aktivieren des zugehörigen Kontrollkästchens aus.
1. Klicken Sie auf **Löschen**.
1. Eine Meldung informiert Sie darüber, dass diese Aktion diese Bereitstellung löschen wird und der vorherige Status auf allen Geräten wiederhergestellt wird.  Eine Bereitstellung mit einer niedrigeren Priorität wird angewendet.  Wenn keine andere Zielbereitstellung für das Gerät vorhanden ist, werden keine Module entfernt. Wenn Sie alle Module vom Gerät entfernen möchten, erstellen Sie eine Bereitstellung ohne Module, und stellen Sie sie auf den gleichen Geräten bereit.  Wählen Sie **Yes** (Ja), um fortzufahren.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zum [Bereitstellen von Modulen auf IoT Edge-Geräten](module-deployment-monitoring.md).