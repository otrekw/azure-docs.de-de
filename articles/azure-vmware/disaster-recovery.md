---
title: Ausführen einer Notfallwiederherstellung virtueller Computer
description: In diesem Artikel wird gezeigt, wie Sie eine Notfallwiederherstellung virtueller Computer mithilfe von AVS durchführen.
ms.topic: how-to
ms.date: 05/04/2020
ms.openlocfilehash: 5fdfc252486d94fcc22ebba9705fa5e259539921
ms.sourcegitcommit: 2721b8d1ffe203226829958bee5c52699e1d2116
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84148156"
---
# <a name="complete-a-disaster-recovery-of-virtual-machines-using-azure-vmware-solution"></a>Ausführen einer Notfallwiederherstellung virtueller Computer mithilfe von Azure VMware Solution

In diesem Artikel wird das Verfahren zum Durchführen einer Notfallwiederherstellung Ihrer virtuellen Computer mit der VMware HCX-Lösung (Hybrid Cloud Extension) und unter Verwendung einer privaten Azure VMware Solution-Cloud als Wiederherstellungs- oder Zielstandort beschrieben.

VMware HCX bietet verschiedene Vorgänge, die eine genaue Kontrolle und präzise Steuerung in Replikationsrichtlinien ermöglichen. Die verfügbaren Vorgänge umfassen:

- Reverse (Umkehren) – nach dem Eintreten eines Notfalls. Durch Reverse wird Standort B der Quellstandort und Standort A, an dem sich die geschützte VM nun befindet.

- Pause (Anhalten) – Anhalten der aktuellen Replikationsrichtlinie, die der ausgewählten VM zugeordnet ist.

- Resume (Fortsetzen) – Fortsetzen der aktuellen Replikationsrichtlinie, die der ausgewählten VM zugeordnet ist.

- Remove (Entfernen) – Entfernen der aktuellen Replikationsrichtlinie, die der ausgewählten VM zugeordnet ist.

- Sync Now (Jetzt synchronisieren) – außerplanmäßige Synchronisierung des virtuellen Quellcomputers mit der geschützten VM.

In diesem Leitfaden werden die folgenden Replikationsszenarien behandelt:

- Schützen einer VM oder einer Gruppe von VMs.

- Ausführen einer Testwiederherstellung einer VM oder einer Gruppe von VMs.

- Wiederherstellen einer VM oder einer Gruppe von VMs.

- Umgekehrter Schutz einer VM oder einer Gruppe von VMs.

## <a name="protect-virtual-machines"></a>Schützen virtueller Computer

Melden Sie sich beim **vSphere-Client** am Quellstandort an, und greifen Sie auf das **HCX-Plug-In** zu.

:::image type="content" source="./media/disaster-recovery/hcx-vsphere.png" alt-text="HCX-Option in vSphere" border="true":::

Navigieren Sie zum Bereich **Disaster Recovery** (Notfallwiederherstellung), und klicken Sie auf **PROTECT VMS** (VMs schützen).

:::image type="content" source="./media/disaster-recovery/protect-virtual-machine.png" alt-text="Auswählen von „Protect VMs“" border="true":::

Wählen Sie im Fenster, das geöffnet wird, die Quell- und Remotestandorte aus. In diesem Fall sollte es sich bei dem Remotestandort um die private AVS-Cloud handeln.

:::image type="content" source="./media/disaster-recovery/protect-virtual-machines.png" alt-text="Fenster „VMs schützen“" border="true":::

Wählen Sie bei Bedarf die Standardreplikationsoptionen aus:

- **Enable Compression (Komprimierung aktivieren):** Empfohlen für Szenarien mit geringem Durchsatz.

- **Enable Quiescence (Ruhezustand aktivieren):** Hält den virtuellen Computer an, um sicherzustellen, dass eine konsistente Kopie mit dem Remotestandort synchronisiert wird.

- **Destination Storage (Zielspeicher):** Wählen Sie den Remotedatenspeicher für die geschützte(n) VM(s) aus. In einer privaten AVS-Cloud sollte diese Auswahl der VSAN-Datenspeicher sein.

- **Compute Container:** Der vSphere-Remotecluster oder -Ressourcenpool.

- **Destination Folder (Zielordner):** Der Remotezielordner. Diese Einstellung ist optional. Wenn kein Ordner ausgewählt ist, werden die VMs direkt unter dem ausgewählten Cluster platziert.

- **RPO:** Dieser Wert ist das Synchronisierungsintervall zwischen dem virtuellen Quellcomputer und dem geschützten virtuellen Computer. Er kann zwischen 5 Minuten und 24 Stunden liegen.

- **Snapshot interval (Momentaufnahmeintervall):** Das Intervall zwischen Momentaufnahmen.

- **Number of Snapshots (Anzahl Momentaufnahmen):** Die Gesamtzahl der Momentaufnahmen innerhalb des konfigurierten Momentaufnahmeintervalls.

:::image type="content" source="./media/disaster-recovery/protect-virtual-machine-options.png" alt-text="Optionen zum Schützen virtueller Computer" border="true":::

Wählen Sie mindestens einen virtuellen Computer aus der Liste aus, und konfigurieren Sie für den virtuellen Computer die erforderlichen Replikationsoptionen.

Standardmäßig erben die virtuellen Computer die in den Standardreplikationsoptionen konfigurierte Richtlinie für globale Einstellungen. Konfigurieren Sie für jede Netzwerkschnittstelle auf dem ausgewählten virtuellen Computer die **Remote-Netzwerkportgruppe**, und wählen Sie **Finish** (Fertig stellen) aus, um den Schutz Prozess zu starten.

:::image type="content" source="./media/disaster-recovery/network-interface-options.png" alt-text="Netzwerkschnittstellenoptionen" border="true":::

Wie in der folgenden Abbildung gezeigt, können Sie den Prozess für jeden der ausgewählten virtuellen Computer im gleichen Notfallwiederherstellungsbereich überwachen.

:::image type="content" source="./media/disaster-recovery/protect-monitor-progress.png" alt-text="Überwachen des Status des Schutzes" border="true":::

Nachdem die VM geschützt wurde, können die verschiedenen Momentaufnahmen auf der Registerkarte **Snapshots** (Momentaufnahmen) angezeigt werden.

:::image type="content" source="./media/disaster-recovery/list-of-snapshots.png" alt-text="Liste der Momentaufnahmen" border="true":::

Das gelbe Dreieck bedeutet, dass die Momentaufnahmen und die VMs nicht in einem Testwiederherstellungsvorgang getestet wurden.

Es gibt wesentliche Unterschiede zwischen einer VM, die ausgeschaltet ist, und einer VM, die eingeschaltet ist.
Der Screenshot oben zeigt den Synchronisierungsvorgang für einen eingeschalteten virtuellen Computer. Der Synchronisierungsvorgang wurde gestartet, bis die erste Momentaufnahme abgeschlossen ist. Dabei handelt es sich um eine vollständige Kopie der VM. Die nächsten Momentaufnahmen werden dann im konfigurierten Intervall abgeschlossen.

Bei einer ausgeschalteten VM wird eine Kopie synchronisiert, und die VM wird als inaktiv und der Schutzvorgang als abgeschlossen angezeigt.

Wenn der virtuelle Computer eingeschaltet ist, wird die Synchronisierung mit dem Remotestandort gestartet.

## <a name="complete-a-test-recover-of-virtual-machines"></a>Ausführen einer Testwiederherstellung virtueller Computer

Melden Sie sich beim **vSphere-Client** am Remotestandort (der privaten AVS-Cloud) an. Wählen Sie im **HCX-Plug-In** im Bereich „Notfallwiederherstellung“ die vertikalen Auslassungspunkte für eine beliebige VM aus, um das Vorgangsmenü anzuzeigen. Wählen Sie **Test Recover VM** (Testwiederherstellung VM) aus.

:::image type="content" source="./media/disaster-recovery/test-recover-virtual-machine.png" alt-text="Auswählen von „Test Recover VM“ (Testwiederherstellung VM)" border="true":::

Wählen Sie im neuen Fenster die Optionen für den Test aus. Wählen Sie die Momentaufnahme aus, die Sie verwenden möchten, um verschiedene Zustände des virtuellen Computers zu testen.

:::image type="content" source="./media/disaster-recovery/choose-snapshot.png" alt-text="Auswählen einer Momentaufnahme und Klicken auf „Testen“" border="true":::

Nachdem Sie auf **Test** (Testen) geklickt haben, beginnt der Wiederherstellungsvorgang.

Wenn der Testwiederherstellungsvorgang abgeschlossen ist, kann die neue VM in vCenter der privaten AVS-Cloud überprüft werden.

:::image type="content" source="./media/disaster-recovery/verify-test-recovery.png" alt-text="Überprüfen des Wiederherstellungsvorgangs" border="true":::

Führen Sie nach dem Testen der VM oder einer Anwendung, die auf dieser ausgeführt wird, eine Bereinigung durch, um die Testinstanz zu löschen.

:::image type="content" source="./media/disaster-recovery/cleanup-test-instance.png" alt-text="Bereinigen einer Testinstanz" border="true":::

## <a name="recover-virtual-machines"></a>Wiederherstellen virtueller Computer

Melden Sie sich beim **vSphere-Client** am Remotestandort (der privaten AVS-Cloud) an, und greifen Sie auf das **HCX-Plug-In** zu.

Für das Wiederherstellungsszenario wird eine Gruppe von virtuellen Computern für dieses Beispiel verwendet.

Wählen Sie den wiederherzustellenden virtuellen Computer aus der Liste aus, öffnen Sie das Menü **ACTIONS** (Aktionen), und wählen Sie **Recover VMs** (VMs wiederherstellen) aus.

:::image type="content" source="./media/disaster-recovery/recover-virtual-machines.png" alt-text="Wiederherstellen virtueller Computer" border="true":::

Konfigurieren Sie die Wiederherstellungsoptionen für jede Instanz, und klicken Sie auf **Recover** (Wiederherstellen), um den Wiederherstellungsvorgang zu starten.

:::image type="content" source="./media/disaster-recovery/recover-virtual-machines-confirm.png" alt-text="Bestätigung der Wiederherstellung virtueller Computer" border="true":::

Nachdem der Wiederherstellungsvorgang abgeschlossen ist, werden die neuen VMs im Inventar des vCenter-Remoteservers angezeigt.

## <a name="complete-a-reverse-replication-on-virtual-machines"></a>Abschließen einer umgekehrten Replikation auf virtuellen Computern

Melden Sie sich beim **vSphere-Client** in Ihrer privaten AVS-Cloud an, und greifen Sie auf das **HCX-Plug-In** zu.
Es ist erforderlich, dass die ursprünglichen virtuellen Computer am Quellstandort ausgeschaltet werden, bevor Sie mit der umgekehrten Replikation beginnen. Der Vorgang schlägt fehl, wenn die virtuellen Computer nicht ausgeschaltet sind.

Wählen Sie in der Liste die virtuellen Computer aus, die zurück an den Quellstandort repliziert werden sollen, öffnen Sie das Menü **ACTIONS** (Aktionen), und wählen Sie **Reverse** (Umgekehrt) aus. Klicken Sie im Popupfenster auf **Reverse** (Umgekehrt), um die Replikation zu starten.

:::image type="content" source="./media/disaster-recovery/reverse-operation-virtual-machines.png" alt-text="Auswählen der Aktion „Umgekehrt“ unter den Schutzvorgängen" border="true":::

Die Replikation kann im Abschnitt „Details“ der einzelnen virtuellen Computer überwacht werden.

:::image type="content" source="./media/disaster-recovery/review-reverse-operation.png" alt-text="Überprüfen der Ergebnisse der Aktion „Umgekehrt“" border="true":::

## <a name="disaster-recovery-plan-automation"></a>Automatisierung des Plans zur Notfallwiederherstellung

VMware HCX verfügt derzeit nicht über einen integrierten Mechanismus zum Erstellen und Automatisieren eines Plans für die Notfallwiederherstellung. Diese Funktion ist in HCX nicht verfügbar. Es wird jedoch eine Reihe von REST-APIs bereitgestellt, einschließlich APIs für den Notfallwiederherstellungsvorgang.

Auf die API-Spezifikation kann in HCX Manager in der URL zugegriffen werden.

Die folgenden Vorgänge bei der Notfallwiederherstellung werden durch diese APIs abgedeckt.

- Schützen

- Wiederherstellen

- Testwiederherstellung

- Geplante Wiederherstellung

- Reverse

- Abfrage

- Testbereinigung

- Pause

- Fortfahren

- Schutz entfernen

- Neu konfigurieren

Im Folgenden finden Sie ein Beispiel für eine Wiederherstellungsvorgangsnutzlast in JSON.

```json
[

    {

        "replicationId": "string",

        "needPowerOn": true,

        "instanceId": "string",

        "source": {

            "endpointType": "string",

            "endpointId": "string",

            "endpointName": "string",

            "resourceType": "string",

            "resourceId": "string",

            "resourceName": "string"

        },

        "destination": {

            "endpointType": "string",

            "endpointId": "string",

            "endpointName": "string",

            "resourceType": "string",

            "resourceId": "string",

            "resourceName": "string"

        },

        "placement": [

            {

                "containerType": "string",

                "containerId": "string"

            }

        ],

        "resourceId": "string",

        "forcePowerOff": true,

        "isTest": true,

        "forcePowerOffAfterTimeout": true,

        "isPlanned": true

    }

]
```

Mithilfe dieser APIs kann ein Kunde einen benutzerdefinierten Mechanismus erstellen, um die Erstellung und Ausführung eines Notfallwiederherstellungsplans zu automatisieren.
