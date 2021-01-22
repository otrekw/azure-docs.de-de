---
title: Erstellen einer Azure VM-Bewertung mit Azure Migrate-Serverbewertung | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie eine Azure VM-Bewertung mit dem Tool Azure Migrate-Serverbewertung ausführen.
author: rashi-ms
ms.author: rajosh
ms.manager: abhemraj
ms.topic: how-to
ms.date: 07/15/2019
ms.openlocfilehash: 178bdca78c6f011c607de8e1f5d5eabcdbaab7d4
ms.sourcegitcommit: ca215fa220b924f19f56513fc810c8c728dff420
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2021
ms.locfileid: "98567694"
---
# <a name="create-an-azure-vm-assessment"></a>Erstellen einer Azure-VM-Bewertung

In diesem Artikel wird beschrieben, wie Sie eine Azure VM-Bewertung für lokale VMware-VMs oder Hyper-V-VMs mit Azure Migrate erstellen: Server Assessment“ (Azure Migrate-Serverbewertung) erstellen.

[Azure Migrate](migrate-services-overview.md) hilft Ihnen bei der Migration zu Azure. Azure Migrate bietet einen zentralisierten Hub zum Nachverfolgen der Ermittlung, Bewertung und Migration lokaler Infrastruktur, Anwendungen und Daten zu Azure. Der Hub stellt Azure-Tools für die Bewertung und Migration sowie Drittanbietertools von unabhängigen Softwareanbietern (ISVs) bereit. 

## <a name="before-you-start"></a>Vorbereitung

- Stellen Sie sicher, dass Sie ein Azure Migrate-Projekt [erstellt](./create-manage-projects.md) haben.
- Wenn Sie bereits ein Projekt erstellt haben, vergewissern Sie sich, dass Sie das Tool Azure Migrate-Serverbewertung[hinzugefügt](how-to-assess.md): Migrate-Serverbewertung bewerten.
- Zum Erstellen einer Bewertung müssen Sie eine Azure Migrate-Appliance für [VMware](how-to-set-up-appliance-vmware.md) oder [Hyper-V](how-to-set-up-appliance-hyper-v.md) einrichten. Die Appliance ermittelt lokale Computer und sendet Metadaten und Leistungsdaten an Azure Migrate: Server Assessment“ (Azure Migrate-Serverbewertung) erstellen. [Weitere Informationen](migrate-appliance.md)


## <a name="azure-vm-assessment-overview"></a>Übersicht über Azure VM-Bewertungen
Es gibt zwei Arten von Größenanpassungskriterien, die Sie zur Erstellung einer Azure VM-Bewertung mit Azure Migrate verwenden können: Server Assessment“ (Azure Migrate-Serverbewertung) erstellen.

**Bewertung** | **Details** | **Daten**
--- | --- | ---
**Leistungsbasiert** | Bewertungen basierend auf gesammelten Leistungsdaten | **Empfohlene VM-Größe**: Basierend auf CPU- und Arbeitsspeicher-Nutzungsdaten<br/><br/> **Empfohlener Datenträgertyp (Verwalteter Datenträger vom Typ Standard oder Premium)** : Basierend auf IOPS und Durchsatz der lokalen Datenträger
**Wie lokal** | Bewertungen basierend auf lokaler Größenanpassung | **Empfohlene VM-Größe**: Basierend auf der Größe der lokalen VM<br/><br> **Empfohlener Datenträgertyp**: Basierend auf der für die Bewertung ausgewählten Speichertypeinstellung

[Hier erfahren Sie mehr](concepts-assessment-calculation.md) über Bewertungen.

## <a name="run-an-assessment"></a>Durchführen einer Bewertung

Führen Sie eine Bewertung wie folgt aus:

1. Klicken Sie auf der Seite **Server** unter **Windows- und Linux-Server** auf **Server bewerten und migrieren**.

   ![Ort der Schaltfläche „Server bewerten und migrieren“](./media/tutorial-assess-vmware-azure-vm/assess.png)

2. Klicken Sie in **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) auf **Bewerten**.

    ![Position der Schaltfläche „Bewerten“](./media/tutorial-assess-vmware-azure-vm/assess-servers.png)

3. Wählen Sie unter **Server bewerten** > **Bewertungstyp** die Option **Azure-VM** aus.
4. Unter **Ermittlungsquelle**:

    - Wählen Sie die Option **Von Azure Migrate-Appliance erkannte Computer** aus, falls Sie Computer mit der Appliance ermittelt haben.
    - Wählen Sie die Option **Importierte Computer** aus, falls Sie Computer mit einer importierten CSV-Datei ermittelt haben. 
    
1. Klicken Sie auf **Bearbeiten**, um die Eigenschaften für die Bewertung zu überprüfen.

    :::image type="content" source="./media/tutorial-assess-vmware-azure-vm/assessment-name.png" alt-text="Ort der Schaltfläche „Bearbeiten“ zum Überprüfen der Bewertungseigenschaften":::

1. Unter **Bewertungseigenschaften** > **Zieleigenschaften**:
    - Geben Sie unter **Zielspeicherort** die Azure-Region an, zu der Sie die Migration durchführen möchten.
        - Empfehlungen zur Größe und zu den Kosten basieren auf dem von Ihnen angegebenen Standort. Nachdem Sie den Standardwert des Zielstandorts geändert haben, werden Sie aufgefordert, **Reservierte Instanzen** und **VM-Serien** anzugeben.
        - Bei Azure Government können Sie Bewertungen in [diesen Regionen](migrate-support-matrix.md#supported-geographies-azure-government) durchführen.
    - Unter **Speichertyp**:
        - Wählen Sie bei Verwendung von leistungsbezogenen Daten in der Bewertung die Option **Automatisch** für Azure Migrate aus, um basierend auf dem IOPS-Wert und dem Durchsatz des Datenträgers eine Empfehlung zum Speichertyp anzuzeigen.
        - Wählen Sie alternativ den Speichertyp aus, den Sie beim Migrieren für die VM verwenden möchten.
    - Geben Sie unter **Reservierte Instanzen** an, ob Sie für die VM beim Migrieren reservierte Instanzen verwenden möchten.
        - Wenn Sie die Verwendung einer reservierten Instanz auswählen, können Sie nicht **Abzug (%)** oder **VM-Betriebszeit** angeben. 
        - [Weitere Informationen](https://aka.ms/azurereservedinstances)
 1. Unter **VM-Größe**:
     - Wählen Sie unter **Größenkriterium** aus, ob die Bewertung auf Daten bzw. Metadaten für die Computerkonfiguration oder leistungsbezogenen Daten basieren soll. Bei Verwendung von Leistungsdaten:
        - Geben Sie unter **Leistungsverlauf** die Datendauer an, auf der die Bewertung basieren soll.
        - Geben Sie unter **Perzentilwert der Nutzung** den Perzentilwert an, den Sie für das Leistungsbeispiel verwenden möchten. 
    - Geben Sie unter **VM-Serie** die gewünschte Azure-VM-Serie an.
        - Bei Verwendung der leistungsbezogenen Bewertung wird Ihnen von Azure Migrate ein Wert vorgeschlagen.
        - Optimieren Sie die Einstellungen nach Bedarf. Wenn Sie beispielsweise in der Produktionsumgebung keine virtuellen Computer der A-Serie in Azure benötigen, können Sie die A-Serie aus der Liste der Serien ausschließen.
    - Geben Sie unter **Komfortfaktor** den Puffer an, den Sie während der Bewertung verwenden möchten. Hierbei werden Aspekte wie saisonale Nutzung, ein kurzer Leistungsverlauf und eine voraussichtliche Zunahme der zukünftigen Nutzung berücksichtigt. Beispiel für einen Komfortfaktor von „2“:
    
        **Komponente** | **Tatsächliche Auslastung** | **Komfortfaktor hinzufügen (2.0)**
        --- | --- | ---
        Kerne | 2  | 4
        Arbeitsspeicher | 8 GB | 16 GB
   
1. Unter **Preise**:
    - Geben Sie unter **Angebot** das [Azure-Angebot](https://azure.microsoft.com/support/legal/offer-details/) ein, wenn Sie registriert sind. Bei der Serverbewertung werden die Kosten für dieses Angebot geschätzt.
    - Wählen Sie unter **Währung** die Abrechnungswährung für Ihr Konto aus.
    - Fügen Sie unter **Abzug (%)** alle abonnementspezifischen Rabatte hinzu, die Sie zusätzlich zum Azure-Angebot erhalten. Die Standardeinstellung ist 0 %.
    - Geben Sie unter **VM-Betriebszeit** die Dauer (Tage pro Monat/Stunden pro Tag) für die Ausführung der VMs an.
        - Dies ist für Azure-VMs hilfreich, die nicht dauerhaft ausgeführt werden.
        - Die Kostenschätzungen basieren auf der angegebenen Dauer.
        - Der Standardwert ist „31 Tage pro Monat/24 Stunden pro Tag“.
    - Geben Sie unter **EA-Abonnement** an, ob bei der Kostenschätzung ein Rabatt für ein EA-Abonnement (Enterprise Agreement) berücksichtigt werden soll. 
    - Geben Sie unter **Azure-Hybridvorteil** an, ob Sie bereits über eine Windows Server-Lizenz verfügen. Wenn dies der Fall ist und eine aktive Software Assurance-Abdeckung für Windows Server-Abonnements besteht, können Sie sich für den [Azure-Hybridvorteil](https://azure.microsoft.com/pricing/hybrid-use-benefit/) bewerben, sofern Sie eigene Lizenzen für Azure mitbringen.

1. Klicken Sie auf **Speichern**, falls Sie Änderungen vorgenommen haben.

    ![Bewertungseigenschaften](./media/tutorial-assess-vmware-azure-vm/assessment-properties.png)

1. Klicken Sie unter **Server bewerten** auf **Weiter**.

1. Geben Sie in **Zu bewertende Computer auswählen** > **Bewertungsname** einen Namen für die Bewertung an. 

1. Wählen Sie unter **Gruppe auswählen oder erstellen** die Option **Neu erstellen** aus, und geben Sie einen Namen für die Gruppe ein. 

    :::image type="content" source="./media/tutorial-assess-vmware-azure-vm/assess-group.png" alt-text="Hinzufügen von VMs zu einer Gruppe":::

1. Wählen Sie die Appliance und dann die VMs aus, die Sie der Gruppe hinzufügen möchten. Klicken Sie dann auf **Weiter**.


1. Sehen Sie sich unter **Überprüfen + Bewertung erstellen** die Bewertungsdetails an, und klicken Sie auf **Bewertung erstellen**, um die Gruppe zu erstellen und die Bewertung durchzuführen.

1. Zeigen Sie die erstellte Bewertung unter **Server** > **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) > **Bewertungen** an.

1. Klicken Sie auf **Bewertung exportieren**, um sie als Excel-Datei herunterzuladen.
    > [!NOTE]
    > Bei leistungsbezogenen Bewertungen empfehlen wir Ihnen, nach dem Starten einer Ermittlung mindestens einen Tag zu warten, bevor Sie eine Bewertung erstellen. Dies ermöglicht einen längeren Zeitraum zum Sammeln von Leistungsdaten mit höherer Zuverlässigkeit. Idealerweise sollten Sie nach dem Starten der Ermittlung den Zeitraum der Leistungsdauer abwarten, den Sie angegeben haben (Tag/Woche/Monat), um eine Bewertung mit einer hohen Zuverlässigkeit zu erzielen.


## <a name="review-an-azure-vm-assessment"></a>Überprüfen einer Azure-VM-Bewertung

Bei einer Azure-VM-Bewertung wird Folgendes beschrieben:

- **Azure-Bereitschaft**: Gibt an, ob VMs für die Migration zu Azure geeignet sind.
- **Geschätzte monatliche Kosten**: Die geschätzten monatlichen Compute- und Speicherkosten für die Ausführung der VMs in Azure.
- **Geschätzte monatliche Speicherkosten**: Die geschätzten Kosten für den Datenträgerspeicher nach der Migration.

### <a name="view-an-azure-vm-assessment"></a>Anzeigen einer Azure VM-Bewertung

1. Klicken Sie unter **Migrationsziele** >  **Server** auf **Bewertungen** (unter **Azure Migrate: Serverbewertung** aus.
2. Klicken Sie unter **Bewertungen** auf eine Bewertung, um sie zu öffnen.

    ![Zusammenfassung der Bewertung](./media/how-to-create-assessment/assessment-summary.png)

### <a name="review-azure-readiness"></a>Überprüfen der Azure-Bereitschaft

1. Überprüfen Sie unter **Azure-Bereitschaft**, ob VMs für die Migration zu Azure bereit sind.
2. Überprüfen Sie den VM-Status:
    - **Bereit für Azure**: Azure Migrate empfiehlt in der Bewertung eine VM-Größe und gibt Kostenschätzungen für VMs ab.
    - **Bereit mit Bedingungen**: Zeigt Probleme und eine vorgeschlagene Abhilfe an.
    - **Nicht bereit für Azure**: Zeigt Probleme und eine vorgeschlagene Abhilfe an.
    - **Bereitschaft unbekannt**: Wird verwendet, wenn Azure Migrate die Bereitschaft aufgrund von Problemen mit der Datenverfügbarkeit nicht bewerten kann.

3. Klicken Sie auf einen **Azure-Bereitschaftsstatus**. Sie können Details zur VM-Bereitschaft sowie VM-Details wie Compute-, Speicher- und Netzwerkeinstellungen anzeigen.



### <a name="review-cost-details"></a>Überprüfen der Kostendetails

In dieser Ansicht werden die geschätzten Compute- und Speicherkosten für die Ausführung der VMs in Azure angezeigt.

1. Überprüfen Sie die monatlichen Compute- und Speicherkosten. Die Kosten für alle VMs in der bewerteten Gruppe werden aggregiert.

    - Kostenschätzungen basieren auf den Größenempfehlungen für einen Computer sowie auf seinen Datenträgern und Eigenschaften.
    - Die geschätzten monatlichen Kosten für Compute und Speicher werden angezeigt.
    - Die Kostenschätzung gilt für die Ausführung der lokalen VMs als IaaS-VMs. PaaS- oder SaaS-Kosten werden von der Azure Migrate-Serverbewertung nicht berücksichtigt.

2. Sie können die geschätzten monatlichen Speicherkosten überprüfen. Diese Ansicht zeigt aggregierte Speicherkosten für die bewertete Gruppe, aufgeschlüsselt nach verschiedenen Arten von Speicherdatenträgern.
3. Sie können einen Drilldown ausführen, um die Details für bestimmte VMs anzuzeigen.


### <a name="review-confidence-rating"></a>Prüfen der Zuverlässigkeitsstufe

Wenn Sie leistungsbasierte Bewertungen ausführen, wird der Bewertung eine Zuverlässigkeitsstufe zugewiesen.

![Zuverlässigkeitsstufe](./media/how-to-create-assessment/confidence-rating.png)

- Es wird eine Bewertung zwischen einem Stern (niedrigste Stufe) und fünf Sternen (höchste Stufe) vergeben.
- Anhand der Zuverlässigkeitsstufe können Sie die Zuverlässigkeit der von der Bewertung bereitgestellten Größenempfehlungen besser einschätzen.
- Die Zuverlässigkeitsstufe basiert auf der Verfügbarkeit von Datenpunkten, die zum Berechnen der Bewertung erforderlich sind.

Die Zuverlässigkeitsstufen für eine Bewertung sehen wie folgt aus:

**Verfügbarkeit von Datenpunkten** | **Zuverlässigkeitsstufe**
--- | ---
0 % bis 20 % | Ein Stern
21 % bis 40 % | Zwei Sterne
41 % bis 60 % | Drei Sterne
61 % bis 80 % | Vier Sterne
81 % bis 100 % | Fünf Sterne




## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das Erstellen von Gruppen mit hoher Vertrauenswürdigkeit mithilfe der [Zuordnung von Abhängigkeiten](how-to-create-group-machine-dependencies.md).
- [Weitere Informationen](concepts-assessment-calculation.md) zur Berechnung von Bewertungen