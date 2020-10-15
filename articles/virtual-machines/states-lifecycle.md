---
title: Lebenszyklus und Status eines virtuellen Computers in Azure
description: Übersicht über den Lebenszyklus einer VM in Azure, einschließlich Beschreibungen der verschiedenen möglichen Zustände von VMs zu einem beliebigen Zeitpunkt.
services: virtual-machines
author: shandilvarun
ms.service: virtual-machines
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 08/09/2018
ms.author: vashan
ms.openlocfilehash: 8c9641e722cf3892450bbfbea54f169ac6dc764b
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91963294"
---
# <a name="virtual-machines-lifecycle-and-states"></a>Lebenszyklus und Status virtueller Computer

Azure-VMs durchlaufen verschiedene Zustände, die sich in die Kategorien *Bereitstellungszustände* und *Betriebszustände* unterteilen lassen. Im vorliegenden Artikel werden diese Zustände beschrieben, und zudem enthält er genaue Angaben dazu, wann die Instanznutzung Kunden in Rechnung gestellt wird. 

## <a name="power-states"></a>Betriebszustände

Der Betriebszustand stellt den letzten bekannten Zustand der VM dar.

![Diagramm zum VM-Betriebszustand](./media/virtual-machines-common-states-lifecycle/vm-power-states.png)

<br>
Die folgende Tabelle enthält eine Beschreibung der einzelnen Instanzzustände und Angaben dazu, ob die Instanznutzung in Rechnung gestellt wird.

:::row:::
   :::column span="":::

   **State**
   
   :::column-end:::
   :::column span="":::

   **Beschreibung**

   :::column-end:::
   :::column span="":::

   **Abgerechnete Instanznutzung**

   :::column-end:::
:::row-end:::

:::row:::
   :::column span="":::

   **Wird gestartet**

   :::column-end:::
   :::column span="":::

   Die VM wird gestartet.

   ```json
   "statuses": [
    {
    "code": "PowerState/starting",
    "level": "Info",
    "displayStatus": "VM starting"
    }
   ]
   ```
   :::column-end:::
   :::column span="":::

   **Nicht in Rechnung gestellt**

   :::column-end:::
:::row-end:::

:::row:::
   :::column span="":::

   **Wird ausgeführt**

   :::column-end:::
   :::column span="":::

   Dies ist der normale Ausführungszustand einer VM.

   ```json
   "statuses": [
    {
    "code": "PowerState/running",
    "level": "Info",
    "displayStatus": "VM running"
    }
  ]
  ```
   :::column-end:::
   :::column span="":::

   **In Rechnung gestellt**

   :::column-end:::
:::row-end:::

:::row:::
   :::column span="":::

   **Wird beendet**

   :::column-end:::
   :::column span="":::

   Dies ist ein Übergangszustand. Nach Abschluss des Vorgangs wird für die VM **Beendet** angezeigt.

   ```json
   "statuses": [
    {
    "code": "PowerState/stopping",
    "level": "Info",
    "displayStatus": "VM stopping"
    }
   ]
  ```
   :::column-end:::
   :::column span="":::

   **In Rechnung gestellt**

   :::column-end:::
:::row-end:::

:::row:::
   :::column span="":::

   **Beendet**

   :::column-end:::
   :::column span="":::

   Die VM wurde über das Gastbetriebssystem oder mithilfe der PowerOff-APIs heruntergefahren.

   Der VM ist weiterhin Hardware zugeordnet, und sie verbleibt auf dem Host.

   ```json
   "statuses": [
    {
    "code": "PowerState/stopped",
    "level": "Info",
    "displayStatus": "VM stopped"
    }
   ]
  ```
   :::column-end:::
   :::column span="":::

   **In Rechnung gestellt***

   :::column-end:::
:::row-end:::

:::row:::
   :::column span="":::

   **Zuordnung wird aufgehoben**

   :::column-end:::
   :::column span="":::

   Dies ist ein Übergangszustand. Nach Abschluss des Vorgangs wird für die VM **Zuordnung aufgehoben** angezeigt.

   ```json
   "statuses": [
    {
    "code": "PowerState/deallocating",
    "level": "Info",
    "displayStatus": "VM deallocating"
    }
   ]
  ```
   :::column-end:::
   :::column span="":::

   **Nicht in Rechnung gestellt***

   :::column-end:::
:::row-end:::

:::row:::
   :::column span="":::

   **Zuordnung aufgehoben**

   :::column-end:::
   :::column span="":::

   Die VM wurde erfolgreich beendet und vom Host entfernt.

   ```json
   "statuses": [
    {
    "code": "PowerState/deallocated",
    "level": "Info",
    "displayStatus": "VM deallocated"
    }
   ]
  ```
   :::column-end:::
   :::column span="":::

   **Nicht in Rechnung gestellt**

   :::column-end:::
:::row-end:::


&#42; Für einige Azure-Ressourcen (z. B. Datenträger und Netzwerk) fallen Kosten an. Für Softwarelizenzen für die Instanz fallen keine Gebühren an.

## <a name="provisioning-states"></a>Bereitstellungszustände

Ein Bereitstellungzustand ist der Status eines vom Benutzer initiierten Vorgangs der Steuerungsebene auf der VM. Diese Zustände sind unabhängig vom Betriebszustand einer VM.

- **Erstellen** – Erstellung einer VM.

- **Aktualisieren** – Aktualisierung des Modells für eine vorhandene VM. Einige Änderungen an einer VM, die nicht das Modell betreffen (z.B. Start/Neustart), fallen ebenfalls unter „Aktualisieren“.

- **Löschen** – Löschen einer VM.

- **Zuordnung aufheben** – Beenden einer VM und Entfernen der VM vom Host. Das Aufheben der Zuordnung einer VM gilt als Aktualisierung. Daher werden Bereitstellungszustände im Zusammenhang mit dem Aktualisieren angezeigt.



Die folgenden Übergangszustände treten auf, nachdem die Plattform eine vom Benutzer initiierte Aktion akzeptiert hat:

:::row:::
   :::column span="":::

   **State**
   
   :::column-end:::
   :::column span="2":::

   **Beschreibung**

   :::column-end:::

:::row-end:::

:::row:::
   :::column span="":::

   **Wird erstellt**

   :::column-end:::
   :::column span="2":::

  ```json
   "statuses": [
    {
    "code": "ProvisioningState/creating",
    "level": "Info",
    "displayStatus": "Creating"
    }
   [
   ```
   :::column-end:::

:::row-end:::

:::row:::
   :::column span="":::

   **Wird aktualisiert**

   :::column-end:::
   :::column span="2":::

   ```json
   "statuses": [
    {
    "code": "ProvisioningState/updating",
    "level": "Info",
    "displayStatus": "Updating"
    }
   [
   ```
   :::column-end:::

:::row-end:::

:::row:::
   :::column span="":::

   **Wird gelöscht**.

   :::column-end:::
   :::column span="2":::

   ```json
   "statuses": [
    {
    "code": "ProvisioningState/deleting",
    "level": "Info",
    "displayStatus": "Deleting"
    }
   [
   ```
   :::column-end:::

:::row-end:::

:::row:::
   :::column span="":::

   **Bereitstellungszustände des Betriebssystems**
   
   :::column-end:::
   :::column span="2":::

   **Beschreibung**

   :::column-end:::

:::row-end:::

:::row:::
   :::column span="":::



   :::column-end:::
   :::column span="2":::

   Wenn eine VM mit einem Betriebssystemimage und nicht mit einem speziellen Image erstellt wird, können folgende Unterzustände auftreten:

   :::column-end:::

:::row-end:::

:::row:::
   :::column span="":::

   **OSProvisioningInprogress**

   :::column-end:::
   :::column span="2":::

   Die VM wird ausgeführt, und das Gastbetriebssystem wird installiert.
 
   ```json
   "statuses": [
    {
    "code": "ProvisioningState/creating/OSProvisioningInprogress",
    "level": "Info",
    "displayStatus": "OS Provisioning In progress"
    }
   [
   ```
   :::column-end:::

:::row-end:::

:::row:::
   :::column span="":::

   **OSProvisioningComplete**

   :::column-end:::
   :::column span="2":::
   
   Dies ist ein Zustand von kurzer Dauer. Die VM wechselt schnell in den Zustand **Erfolgreich**, sofern keine Erweiterungen installiert werden müssen. Die Installation von Erweiterungen kann einige Zeit in Anspruch nehmen.
   
   ```json
   "statuses": [
    {
    "code": "ProvisioningState/creating/OSProvisioningComplete",
    "level": "Info",
    "displayStatus": "OS Provisioning Complete"
    }
   [
   ```
   
   **Hinweis**: Die Bereitstellung des Betriebssystems kann in den Zustand **Fehler** wechseln, wenn ein Fehler des Betriebssystems vorliegt oder das Betriebssystem nicht in der vorgesehenen Zeit installiert wird. Kunden wird die auf der Infrastruktur bereitgestellte VM in Rechnung gestellt.

   :::column-end:::

:::row-end:::

Sobald der Vorgang abgeschlossen ist, wechselt die VM in einen der folgenden Zustände:

- **Erfolgreich** – Die vom Benutzer initiierten Aktionen wurden abgeschlossen.

    ```
  "statuses": [ 
  {
     "code": "ProvisioningState/succeeded",
     "level": "Info",
     "displayStatus": "Provisioning succeeded",
     "time": "time"
  }
  ]
    ```

 

- **Fehler** – Stellt einen fehlgeschlagenen Vorgang dar. Weitere Informationen und mögliche Lösungen können Sie den Fehlercodes entnehmen.

    ```
  "statuses": [
    {
      "code": "ProvisioningState/failed/InternalOperationError",
      "level": "Error",
      "displayStatus": "Provisioning failed",
      "message": "Operation abandoned due to internal error. Please try again later.",
      "time": "time"
    }
    ]
    ```



## <a name="vm-instance-view"></a>VM-Instanzenansicht

Die Instanzenansicht-API bietet Informationen zum Ausführungszustand der VM. Weitere Informationen finden Sie in der API-Dokumentation unter [Virtual Machines – Instance View](/rest/api/compute/virtualmachines/instanceview) (VMs – Instanzenansicht).

Der Azure-Ressourcen-Explorer bietet eine einfache Benutzeroberfläche zum Anzeigen des Ausführungszustands des virtuellen Computers: [Ressourcen-Explorer](https://resources.azure.com/).

Bereitstellungszustände werden in den VM-Eigenschaften und in der Instanzenansicht angezeigt. Betriebszustände sind in der Instanzenansicht der VM verfügbar.

Verwenden Sie die [API zum Auflisten aller virtuellen Computer](/rest/api/compute/virtualmachines/listall), und setzen Sie dabei den Parameter **statusOnly** auf *true*, um den Energiezustand aller virtuellen Computer in Ihrem Abonnement abzurufen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Überwachen Ihres virtuellen Computers finden Sie unter [Überwachen von virtuellen Computern in Azure](../azure-monitor/insights/monitor-vm-azure.md).