---
title: Erstellen eines Verbindungsmonitors (Vorschau) – Azure-Portal
titleSuffix: Azure Network Watcher
description: Erfahren Sie, wie Sie mit dem Azure-Portal einen Verbindungsmonitor (Vorschau) erstellen.
services: network-watcher
documentationcenter: na
author: vinigam
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/30/2020
ms.author: vinigam
ms.openlocfilehash: 4e7067e537ce8fb6faf82198f7863957f79ffb22
ms.sourcegitcommit: 97a0d868b9d36072ec5e872b3c77fa33b9ce7194
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/04/2020
ms.locfileid: "87567814"
---
# <a name="create-a-connection-monitor-preview-using-the-azure-portal"></a>Erstellen eines Verbindungsmonitors (Vorschau) mithilfe des Azure-Portals

Erfahren Sie, wie Sie einen Verbindungsmonitor (Vorschau) erstellen, um die Kommunikation zwischen Ihren Ressourcen mithilfe des Azure-Portals zu überwachen. Er unterstützt Hybrid- und Azure-Cloudbereitstellungen.

## <a name="before-you-begin"></a>Voraussetzungen 

In Verbindungsmonitoren, die Sie im Verbindungsmonitor (Vorschau) erstellen, können Sie sowohl lokale Computer als auch Azure-VMs als Quellen hinzufügen. Mit diesen Verbindungsmonitoren kann auch die Konnektivität mit Endpunkten überwacht werden. Die Endpunkte können sich in Azure oder einer beliebigen anderen URL oder IP-Adresse befinden.

Der Verbindungsmonitor (Vorschau) umfasst die folgenden Entitäten:


* **Verbindungsmonitorressource**: Dies ist eine regionsspezifische Azure-Ressource. Alle folgenden Entitäten sind Eigenschaften einer Verbindungsmonitorressource.
* **Endpunkt**: Eine Quelle oder ein Ziel, die bzw. das an Konnektivitätsprüfungen beteiligt ist. Beispiele für Endpunkte umfassen Azure-VMs, lokale Agents, URLs und IP-Adressen.
* **Testkonfiguration**: Eine protokollspezifische Konfiguration für einen Test. Je nach ausgewähltem Protokoll können Sie den Port, Schwellenwerte, die Testhäufigkeit und weitere Parameter definieren.
* **Testgruppe**: Die Gruppe, die Quellendpunkte, Zielendpunkte und Testkonfigurationen enthält. Ein Verbindungsmonitor kann mehrere Testgruppen enthalten.
* **Test**: Die Kombination aus einem Quellendpunkt, einem Zielendpunkt und einer Testkonfiguration. Ein Test ist die differenzierteste Ebene, auf der Überwachungsdaten verfügbar sind. Die Überwachungsdaten umfassen den Prozentsatz von Überprüfungen mit Fehlern und die Roundtripzeit.

    ![Diagramm eines Verbindungsmonitors mit Definition der Beziehung zwischen Testgruppen und Tests](./media/connection-monitor-2-preview/cm-tg-2.png)

## <a name="steps-to-create"></a>Schritte zur Erstellung

Um einen Verbindungsmonitor (Vorschau) mithilfe des Azure-Portals zu erstellen, führen Sie die folgenden Schritte aus:

1. Navigieren Sie auf der Startseite des Azure-Portals zu **Network Watcher**.
1. Wählen Sie links im Abschnitt **Überwachung** die Option **Verbindungsmonitor (Vorschau)** aus.
1. Es werden alle Verbindungsmonitore angezeigt, die im Verbindungsmonitor (Vorschau) erstellt wurden. Zum Anzeigen der Verbindungsmonitore, die auf der klassischen Benutzeroberfläche des Verbindungsmonitors erstellt wurden, wechseln Sie zur Registerkarte **Verbindungsmonitor**.

    ![Screenshot: Verbindungsmonitore, die im Verbindungsmonitor (Vorschau) erstellt wurden](./media/connection-monitor-2-preview/cm-resource-view.png)   
    
1. Wählen Sie im Dashboard **Verbindungsmonitor (Vorschau)** oben links die Option **Erstellen** aus.
1. Geben Sie auf der Registerkarte **Grundlagen** die Informationen zu Ihrem Verbindungsmonitor ein:
   * **Name des Verbindungsmonitors**: Fügen Sie den Namen Ihres Verbindungsmonitors hinzu. Verwenden Sie die Standardbenennungsregeln für Azure-Ressourcen.
   * **Abonnement**: Wählen Sie ein Abonnement für Ihren Verbindungsmonitor aus.
   * **Region**: Wählen Sie eine Region für Ihren Verbindungsmonitor aus. Dabei kommen nur die Quell-VMs infrage, die in dieser Region erstellt wurden.
   * **Arbeitsbereichskonfiguration**: Der Arbeitsbereich enthält Ihre Überwachungsdaten. Sie können einen benutzerdefinierten Arbeitsbereich oder den Standardarbeitsbereich verwenden. 
       * Aktivieren Sie das Kontrollkästchen, um den Standardarbeitsbereich zu verwenden. 
       * Wenn Sie einen benutzerdefinierten Arbeitsbereich auswählen möchten, deaktivieren Sie das Kontrollkästchen. Wählen Sie dann das Abonnement und die Region für Ihren benutzerdefinierten Arbeitsbereich aus. 
1. Wählen Sie unten auf der Registerkarte die Option **Weiter: Testgruppen** aus.

   ![Screenshot: Registerkarte „Grundlagen“ im Verbindungsmonitor](./media/connection-monitor-2-preview/create-cm-basics.png)

1. Wählen Sie auf der Registerkarte **Testgruppen** die Option **+ Testgruppe** aus. Informationen zum Einrichten der Testgruppen finden Sie unter [Erstellen von Testgruppen im Verbindungsmonitor](#create-test-groups-in-a-connection-monitor). 
1. Wählen Sie unten auf der Registerkarte die Option **Weiter: Überprüfen + erstellen** aus, um Ihren Verbindungsmonitor zu überprüfen.

   ![Screenshot: Registerkarte „Testgruppen“ und Bereich zum Hinzufügen von Testgruppendetails](./media/connection-monitor-2-preview/create-tg.png)

1. Überprüfen Sie auf der Registerkarte **Überprüfen + erstellen** die grundlegenden Informationen und Testgruppen, bevor Sie den Verbindungsmonitor erstellen. Wenn Sie den Verbindungsmonitor bearbeiten müssen, gehen Sie folgendermaßen vor:
   * Zum Bearbeiten grundlegender Informationen wählen Sie das Stiftsymbol aus.
   * Zum Bearbeiten einer Testgruppe wählen Sie diese aus.

   > [!NOTE] 
   > Auf der Registerkarte **Überprüfen + erstellen** werden die Kosten pro Monat während der Vorschauphase des Verbindungsmonitors angezeigt. Derzeit werden in der Spalte **AKTUELLE KOSTEN** keine Kosten angezeigt. Sobald der Verbindungsmonitor allgemein verfügbar ist, wird in dieser Spalte eine monatliche Gebühr angegeben. 
   > 
   > Auch in der Vorschauphase des Verbindungsmonitors fallen Gebühren für die Log Analytics-Erfassung an.

1. Wenn Sie den Verbindungsmonitor erstellen möchten, wählen Sie unten auf der Registerkarte **Überprüfen + erstellen** die Option **Erstellen** aus.

   ![Screenshot: Verbindungsmonitor mit der Registerkarte „Überprüfen + erstellen“](./media/connection-monitor-2-preview/review-create-cm.png)

Der Verbindungsmonitor (Vorschau) erstellt die Verbindungsmonitorressource im Hintergrund.

## <a name="create-test-groups-in-a-connection-monitor"></a>Erstellen von Testgruppen in einem Verbindungsmonitor

Jede Testgruppe in einem Verbindungsmonitor enthält Quellen und Ziele, die auf Netzwerkparameter getestet werden. Sie werden mithilfe von Testkonfigurationen auf den Prozentsatz von Überprüfungen mit Fehlern und die Roundtripzeit getestet.

Wenn Sie eine Testgruppe in einem Verbindungsmonitor über das Azure-Portal erstellen möchten, geben Sie Werte für die folgenden Felder an:

* **Testgruppe deaktivieren**: Sie können dieses Feld auswählen, um die Überwachung für alle in der Testgruppe angegebenen Quellen und Ziele zu deaktivieren. Diese Auswahl ist standardmäßig deaktiviert.
* **Name**: Geben Sie der Testgruppe einen Namen.
* **Quellen**: Sie können sowohl Azure-VMs als auch lokale Computer als Quellen angeben, wenn Agents darauf installiert sind. Informationen zum Installieren eines Agents für Ihre Quelle finden Sie unter [Installieren von Überwachungs-Agents](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview#install-monitoring-agents).
   * Wechseln Sie zum Auswählen von Azure-Agents zur Registerkarte **Azure-Agents**. Hier werden nur VMs angezeigt, die an die Region gebunden sind, die Sie beim Erstellen des Verbindungsmonitors angegeben haben. Standardmäßig sind VMs in den Abonnements gruppiert, denen Sie angehören. Diese Gruppen sind reduziert. 
   
       Sie können von der Abonnementebene einen Drilldown auf andere Hierarchieebenen ausführen:

      **Abonnement** > **Ressourcengruppen** > **VNETs** > **Subnetze** > **VMs mit Agents**

      Außerdem lässt sich der Wert des Felds **Gruppieren nach** ändern, sodass der Baum mit einer beliebigen anderen Ebene beginnt. Wenn Sie z. B. nach virtuellem Netzwerk gruppieren, werden die VMs mit Agents in der Hierarchie **VNETs** > **Subnetze** > **VMs mit Agents** angezeigt.

      ![Screenshot: Verbindungsmonitor mit dem Bereich „Quellen hinzufügen“ und der Registerkarte „Azure-Agents“](./media/connection-monitor-2-preview/add-azure-sources.png)

   * Zum Auswählen lokaler Agents wählen Sie die Registerkarte **Nicht-Azure-Agents** aus. Standardmäßig sind Agents in Arbeitsbereichen nach Region gruppiert. Für alle diese Arbeitsbereiche ist die Netzwerkleistungsmonitor-Lösung konfiguriert. 
   
       Wenn Sie dem Arbeitsbereich den Netzwerkleistungsmonitor hinzufügen müssen, rufen Sie ihn aus dem [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.NetworkMonitoringOMS?tab=Overview) ab. Informationen zum Hinzufügen des Netzwerkleistungsmonitors finden Sie unter [Überwachungslösungen in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/insights/solutions). 
   
       In der Ansicht **Verbindungsmonitor erstellen** ist auf der Registerkarte **Grundlagen** die Standardregion ausgewählt. Wenn Sie die Region ändern, können Sie Agents aus Arbeitsbereichen in der neuen Region auswählen. Sie haben auch die Möglichkeit, den Wert im Feld **Gruppieren nach** zu ändern, sodass nach Subnetzen sortiert wird.

      ![Screenshot: Verbindungsmonitor mit dem Bereich „Quellen hinzufügen“ und der Registerkarte „Nicht-Azure-Agents“](./media/connection-monitor-2-preview/add-non-azure-sources.png)


   * Zum Überprüfen der ausgewählten Azure- und Nicht-Azure-Agents wechseln Sie zur Registerkarte **Überprüfen**.

      ![Screenshot: Verbindungsmonitor mit dem Bereich „Quellen hinzufügen“ und der Registerkarte „Überprüfen“](./media/connection-monitor-2-preview/review-sources.png)

   * Wenn Sie die Einrichtung von Quellen abgeschlossen haben, wählen Sie unten im Bereich **Quellen hinzufügen** die Option **Fertig** aus.

* **Ziele**: Sie können die Konnektivität mit Azure-VMs oder anderen Endpunkten (öffentliche IP-Adresse, URL oder FQDN) überwachen, indem Sie diese als Ziele festlegen. In einer einzelnen Testgruppe können Sie Azure-VMs, Office 365-URLs, Dynamics 365-URLs und benutzerdefinierte Endpunkte hinzufügen.

    * Wenn Sie Azure-VMs als Ziele auswählen möchten, wechseln Sie zur Registerkarte **Azure-VMs**. Die Azure-VMs sind standardmäßig in einer Abonnementhierarchie gruppiert, die sich in der gleichen Region befindet, die Sie in der Ansicht **Verbindungsmonitor erstellen** auf der Registerkarte **Grundlagen** ausgewählt haben. Sie können die Region ändern und die Azure-VMs aus der neuen Region auswählen. Anschließend können Sie von der Abonnementebene einen Drilldown auf andere Hierarchieebenen wie z. B. die Ebene der Azure-Agents ausführen.

       ![Screenshot: Bereich „Ziele hinzufügen“ mit der Registerkarte „Azure-VMs“](./media/connection-monitor-2-preview/add-azure-dests1.png)

       ![Screenshot: Bereich „Ziele hinzufügen“ mit der Abonnementebene](./media/connection-monitor-2-preview/add-azure-dests2.png)

    * Wenn Sie Endpunkte als Ziele auswählen möchten, wechseln Sie zur Registerkarte **Endpunkte**. Die Liste der Endpunkte umfasst die Office 365- und Dynamics 365-Test-URLs, die nach Namen gruppiert sind. Zusätzlich zu diesen Endpunkten können Sie auch einen Endpunkt auswählen, der in einer anderen Testgruppe im selben Verbindungsmonitor erstellt wurde. 
    
        Zum Hinzufügen eines neuen Endpunkts wählen Sie oben rechts die Option **+ Endpunkte** aus. Geben Sie dann einen Endpunktnamen und eine URL, eine IP-Adresse oder einen FQDN an.

       ![Screenshot: Hinzufügen von Endpunkten als Ziele im Verbindungsmonitor](./media/connection-monitor-2-preview/add-endpoints.png)

    * Zum Überprüfen der von Ihnen ausgewählten Azure-VMs und-Endpunkte wählen Sie die Registerkarte **Überprüfen** aus.
    * Wenn Sie die Auswahl von Zielen abgeschlossen haben, wählen Sie **Fertig** aus.

* **Testkonfigurationen**: Sie können Testkonfigurationen in einer Testgruppe zuordnen. Im Azure-Portal ist nur eine Testkonfiguration pro Testgruppe zulässig. Sie können aber mit ARMClient weitere Testkonfigurationen hinzufügen.

    * **Name**: Geben Sie der Testkonfiguration einen Namen.
    * **Protokoll**: Wählen Sie TCP, ICMP oder HTTP aus. Wenn Sie HTTP in HTTPS ändern möchten, wählen Sie **HTTP** als Protokoll und **443** als Port aus.
        * **Netzwerktestkonfiguration erstellen**: Dieses Kontrollkästchen wird nur angezeigt, wenn Sie im Feld **Protokoll** die Option **HTTP** ausgewählt haben. Aktivieren Sie dieses Kontrollkästchen, um eine weitere Testkonfiguration zu erstellen, die dieselben Quellen und Ziele verwendet, die Sie an anderer Stelle in der Konfiguration angegeben haben. Die neu erstellte Testkonfiguration erhält den Namen `<the name of your test configuration>_networkTestConfig`.
        * **Traceroute deaktivieren** Dieses Feld gilt für Testgruppen mit TCP- oder ICMP-Protokoll. Aktivieren Sie dieses Kontrollkästchen, damit Quellen weder die Topologie noch Hop-by-Hop-Roundtripzeit ermitteln.
    * **Zielport**: Sie können dieses Feld mit einem Zielport Ihrer Wahl anpassen.
    * **Testhäufigkeit**: Mit diesem Feld können Sie auswählen, wie oft die Quellen die Ziele über das angegebene Protokoll und den angegebenen Port pingen. Sie können Intervalle von 30 Sekunden, 1 Minute, 5 Minuten, 15 Minuten oder 30 Minuten auswählen. Die Quellen testen die Konnektivität mit den Zielen auf Grundlage des von Ihnen ausgewählten Werts.  Wenn Sie z. B. 30 Sekunden auswählen, überprüfen die Quellen die Konnektivität mit dem Ziel mindestens einmal alle 30 Sekunden.
    * **Schwellenwert für erfolgreiche Tests**: Sie können Schwellenwerte für die folgenden Netzwerkparameter festlegen:
       * **Überprüfungen mit Fehlern**: Legen Sie den Prozentsatz der Überprüfungen fest, die Fehler aufweisen können, wenn Quellen die Konnektivität mit Zielen anhand der von Ihnen angegebenen Kriterien überprüfen. Beim TCP- oder ICMP-Protokoll kann der Prozentsatz der Überprüfungen mit Fehlern dem Prozentsatz der Paketverluste gleichgesetzt werden. Beim HTTP-Protokoll stellt dieses Feld den Prozentsatz von HTTP-Anforderungen dar, die keine Antwort erhielen.
       * **Roundtripzeit**: Legen Sie die Roundtripzeit in Millisekunden fest, die von Quellen zum Herstellen einer Verbindung mit dem Ziel über die Testkonfiguration benötigt werden darf.
    
       ![Screenshot: Einrichten einer Testkonfiguration im Verbindungsmonitor](./media/connection-monitor-2-preview/add-test-config.png)


## <a name="scale-limits"></a>Skalierungslimits

Für Verbindungsmonitore gelten die folgenden Skalierungslimits:

* Maximale Anzahl von Verbindungsmonitoren pro Abonnement und Region: 100
* Maximale Anzahl von Testgruppen pro Verbindungsmonitor: 20
* Maximale Anzahl von Quellen und Zielen pro Verbindungsmonitor: 100
* Maximale Anzahl von Testkonfigurationen pro Verbindungsmonitor: 2 über das Azure-Portal

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie [Überwachungsdaten analysieren und Warnungen festlegen](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview#analyze-monitoring-data-and-set-alerts).
* Erfahren Sie, wie Sie [Probleme in Ihrem Netzwerk diagnostizieren](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview#diagnose-issues-in-your-network).
