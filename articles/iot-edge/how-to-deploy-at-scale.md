---
title: Bereitstellen von Modulen jeder Größenordnung im Azure-Portal – Azure IoT Edge
description: Verwenden des Azure-Portals zum Erstellen automatischer Bereitstellungen für Gruppen von IoT Edge-Geräten
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 4/21/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: e55d3f704c76d2783c3e442a90c829448129a4d0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82134054"
---
# <a name="deploy-iot-edge-modules-at-scale-using-the-azure-portal"></a>Bedarfsgerechtes Bereitstellen von IoT Edge-Modulen mithilfe des Azure-Portals

Erstellen Sie eine **automatische IoT Edge-Bereitstellung** im Azure-Portal zum gleichzeitigen Verwalten laufender Bereitstellungen vieler Geräte. Automatische Bereitstellungen für IoT Edge sind Teil des Features [Automatische Geräteverwaltung](/azure/iot-hub/iot-hub-automatic-device-management) von IoT Hub. Bereitstellungen sind dynamische Prozesse, mit denen Sie mehrere Module auf mehreren Geräten bereitstellen, Status und Integrität der Module nachverfolgen und bei Bedarf Änderungen vornehmen können.

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

IoT Edge bietet zwei verschiedene Arten von automatischen Bereitstellungen, die Sie zum Anpassen Ihres Szenarios verwenden können. Sie können eine Standard*bereitstellung* erstellen, die diese Systemlaufzeitmodule und alle zusätzlichen Module und Routen enthält. Jedes Gerät kann nur eine Bereitstellung anwenden. Oder Sie können eine *mehrstufige Bereitstellung* erstellen, die nur benutzerdefinierte Module und Routen (und nicht die Systemlaufzeit) enthält. Viele mehrstufige Bereitstellungen können auf einem Gerät – basierend auf einer Standardbereitstellung – kombiniert werden. Weitere Informationen dazu, wie die beiden Typen automatischer Bereitstellungen zusammenarbeiten, finden Sie unter [Grundlegendes zu automatischen IoT Edge-Bereitstellungen für einzelne Geräte oder nach Bedarf](module-deployment-monitoring.md).

Die Schritte zum Erstellen einer Bereitstellung und einer mehrstufigen Bereitstellung sind sehr ähnlich. In den folgenden Schritten wird auf eventuelle Unterschiede hingewiesen.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem IoT Hub.
1. Wählen Sie im Menü im linken Bereich unter **Automatische Geräteverwaltung** die Option **IoT Edge** aus.
1. Wählen Sie auf der oberen Leiste **Bereitstellung erstellen** oder **Mehrstufige Bereitstellung erstellen** aus.

Zum Erstellen einer Bereitstellung müssen fünf Schritte ausgeführt werden. Diese werden in den folgenden Abschnitten exemplarisch beschrieben.

### <a name="step-1-name-and-label"></a>Schritt 1: Benennen und Bezeichnen

1. Geben Sie Ihrer Bereitstellung einen eindeutigen Namen, der bis zu 128 Kleinbuchstaben umfasst. Verwenden Sie dabei weder Leerzeichen noch die folgenden ungültigen Zeichen: `& ^ [ ] { } \ | " < > /`.
1. Sie können Bezeichnungen als Schlüssel-Wert-Paare hinzufügen, um Ihre Bereitstellungen im Blick zu behalten. Beispiel: **HostPlatform** und **Linux** oder **Version** und **3.0.1**.
1. Klicken Sie auf **Weiter: Module**, um zu Schritt 2 zu wechseln.

### <a name="step-2-modules"></a>Schritt 2: Module

Sie können einer Bereitstellung bis zu 20 Module hinzufügen. Wenn Sie eine Bereitstellung ohne Module erstellen, werden alle aktuellen Module von den Zielgeräten entfernt.

In Bereitstellungen können Sie die Einstellungen für die IoT Edge-Agent- und IoT Edge-Hub-Module verwalten. Wählen Sie **Laufzeiteinstellungen** aus, um die beiden Laufzeitmodule zu konfigurieren. Weil die Laufzeitmodule in einer mehrstufigen Bereitstellung nicht enthalten sind, können sie nicht konfiguriert werden.

Sie können drei Modultypen hinzufügen:

* IoT Edge-Modul
* Marketplace-Modul
* Azure Stream Analytics-Modul

#### <a name="add-an-iot-edge-module"></a>Hinzufügen eines IoT Edge-Moduls

So fügen Sie benutzerdefinierten Code als Modul oder aber ein Azure-Dienstmodul manuell hinzu:

1. Geben Sie im Abschnitt **Anmeldeinformationen für Containerregistrierung** der Seite die Namen und Anmeldeinformationen für private Containerregistrierungen an, die die Modulimages für diese Bereitstellung enthalten. Der IoT Edge-Agent gibt den Fehler 500 zurück, wenn die Anmeldeinformationen einer Containerregistrierung für ein Docker-Image nicht gefunden wurden.
1. Klicken Sie im Abschnitt **IoT Edge-Module** der Seite auf **Hinzufügen**.
1. Wählen Sie im Dropdownmenü die Option **IoT Edge-Modul** aus.
1. Geben Sie Ihrem Modul in **Name des IoT Edge-Moduls** einen Namen.
1. Geben Sie im Feld **Image-URI** das Containerimage für Ihr Modul ein.
1. Wählen Sie über das Dropdownmenü eine **Neustartrichtlinie** aus. Sie können zwischen folgenden Optionen wählen:
   * **Immer**: Das Modul wird immer neu gestartet, wenn es aus irgendeinem Grund beendet wurde.
   * **Nie**: Das Modul wird niemals neu gestartet, wenn es aus irgendeinem Grund beendet wird.
   * **on-failure** (Bei Absturz): Das Modul wird neu gestartet, wenn es abgestürzt ist, aber nicht ordnungsgemäß heruntergefahren wurde.
   * **on-unhealthy** (Bei Fehler): Das Modul wird neu gestartet, wenn es abstürzt oder einen Integritätsfehlerstatus zurückgibt. Die Implementierung der Integritätsstatusfunktion ist modulspezifisch unterschiedlich.
1. Wählen Sie über das Dropdownmenü den **Gewünschten Status** für das Modul aus. Sie können zwischen folgenden Optionen wählen:
   * **Wird ausgeführt**: Dies ist die Standardoption. Das Modul wird unmittelbar nach Bereitstellung gestartet.
   * **Beendet**: Nach der Bereitstellung verbleibt das Modul im Leerlauf, bis der Start durch Sie oder ein anderes Modul ausgelöst wird.
1. Legen Sie alle **Optionen zur Containererstellung** fest, die an den Container übergeben werden sollen. Weitere Informationen finden Sie unter [docker create](https://docs.docker.com/engine/reference/commandline/create/).
1. Wählen Sie **Einstellungen für Modulzwilling**, aus, wenn Sie dem Modulzwilling Tags oder andere Eigenschaften hinzufügen möchten.
1. Geben Sie **Umgebungsvariablen** für dieses Modul ein. Umgebungsvariablen stellen einem Modul Konfigurationsinformationen zur Verfügung.
1. Wählen Sie **Hinzufügen** aus, um Ihr Modul zur Bereitstellung hinzuzufügen.

#### <a name="add-a-module-from-the-marketplace"></a>Hinzufügen eines Moduls aus dem Marketplace

Führen Sie zum Hinzufügen eines Moduls aus dem Azure Marketplace die folgenden Schritte aus:

1. Klicken Sie im Abschnitt **IoT Edge-Module** der Seite auf **Hinzufügen**.
1. Wählen Sie im Dropdownmenü die Option **Marketplace-Modul** aus.
1. Wählen Sie auf der Seite **IoT Edge-Modul aus Marketplace** das gewünschte Modul aus. Das ausgewählte Modul wird für Ihr Abonnement, Ihre Ressourcengruppe und Ihr Gerät automatisch konfiguriert. Dann wird es in Ihrer Liste von IoT Edge-Modulen angezeigt. Bei einigen Modulen ist möglicherweise eine zusätzliche Konfiguration erforderlich. Weitere Informationen hierzu finden Sie unter [Bereitstellen von Modulen aus Azure Marketplace](how-to-deploy-modules-portal.md#deploy-modules-from-azure-marketplace).

#### <a name="add-a-stream-analytics-module"></a>Hinzufügen eines Azure Stream Analytics-Moduls

Führen Sie die folgenden Schritte aus, um ein Modul aus Azure Stream Analytics hinzuzufügen:

1. Klicken Sie im Abschnitt **IoT Edge-Module** der Seite auf **Hinzufügen**.
1. Wählen Sie im Dropdownmenü die Option **Azure Stream Analytics-Modul** aus.
1. Wählen Sie im rechten Bereich Ihr **Abonnement** aus.
1. Wählen Sie Ihren **IoT Edge-Auftrag** aus.
1. Klicken Sie auf **Speichern**, um Ihr Modul zur Bereitstellung hinzuzufügen.

#### <a name="configure-module-settings"></a>Konfigurieren von Moduleinstellungen

Nachdem Sie ein Modul zu einer Bereitstellung hinzugefügt haben, können Sie dessen Namen zum Öffnen der Seite **IoT Edge-Modul aktualisieren** auswählen. Auf dieser Seite können Sie die Moduleinstellungen und Umgebungsvariablen bearbeiten sowie Optionen und einen Modulzwilling erstellen. Wenn Sie ein Modul aus dem Marketplace hinzugefügt haben, sind darin einige dieser Parameter möglicherweise bereits ausgefüllt.

Wenn Sie eine mehrstufige Bereitstellung erstellen, konfigurieren Sie eventuell ein Modul, das in anderen Bereitstellungen für dieselben Geräte bereits vorhanden ist. Möchten Sie den Modulzwilling aktualisieren, ohne dadurch andere Versionen zu überschreiben, öffnen Sie die Registerkarte **Einstellungen für Modulzwilling**. Erstellen Sie eine neue **Eigenschaft für Modulzwilling** mit einem eindeutigen Namen für einen Unterabschnitt innerhalb der gewünschten Eigenschaften des Modulzwillings, beispielsweise `properties.desired.settings`. Wenn Sie Eigenschaften nur im Feld `properties.desired` definieren, werden dadurch die gewünschten Eigenschaften für das Modul überschrieben, das in beliebigen Bereitstellungen mit niedrigerer Priorität definiert wurde.

![Festlegen einer Eigenschaft für Modulzwilling für mehrstufige Bereitstellung](./media/how-to-deploy-monitor/module-twin-property.png)

Weitere Informationen zur Konfiguration von Modulzwillingen in mehrstufigen Bereitstellungen finden Sie unter [Mehrstufige Bereitstellung](module-deployment-monitoring.md#layered-deployment).

Nachdem Sie alle Module für eine Bereitstellung konfiguriert haben, wählen Sie **Weiter: Routen** aus, um zu Schritt 3 zu wechseln.

### <a name="step-3-routes"></a>Schritt 3: Routen

Routen definieren, wie Module innerhalb einer Bereitstellung miteinander kommunizieren. Standardmäßig erhalten Sie vom Assistenten eine Route mit dem Namen **upstream**, die als „**FROM /messages/\* INTO $upstream**“ definiert ist. Dies bedeutet, dass alle von beliebigen Modulen ausgegebenen Nachrichten an Ihren IoT Hub gesendet werden.  

Fügen Sie die Routen mit Informationen aus dem Abschnitt [Deklarieren von Routen](module-composition.md#declare-routes) hinzu, oder aktualisieren Sie diese, und wählen Sie dann **Weiter** aus, um mit dem Abschnitt zur Überprüfung fortzufahren.

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
1. Geben Sie unter **Zielbedingung** eine Bedingung ein, um festzulegen, auf welche Geräte diese Bereitstellung angewendet werden soll. Die Bedingung basiert auf den Gerätezwillingstags oder auf den gemeldeten Gerätezwillingseigenschaften und muss dem Ausdrucksformat entsprechen. Zum Beispiel: `tags.environment='test'` oder `properties.reported.devicemodel='4000x'`.

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
1. Eine Meldung informiert Sie darüber, dass diese Aktion diese Bereitstellung löschen wird und der vorherige Status auf allen Geräten wiederhergestellt wird. Eine Bereitstellung mit einer niedrigeren Priorität wird angewendet. Wenn keine andere Zielbereitstellung für das Gerät vorhanden ist, werden keine Module entfernt. Wenn Sie alle Module vom Gerät entfernen möchten, erstellen Sie eine Bereitstellung ohne Module, und stellen Sie sie auf den gleichen Geräten bereit. Wählen Sie **Yes** (Ja), um fortzufahren.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zum [Bereitstellen von Modulen auf IoT Edge-Geräten](module-deployment-monitoring.md).
