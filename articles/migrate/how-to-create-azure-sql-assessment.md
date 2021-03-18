---
title: Erstellen einer Azure SQL-Bewertung
description: Es wird beschrieben, wie Sie SQL-Instanzen für die Migration zu Azure SQL Managed Instance und Azure SQL-Datenbank bewerten.
author: rashi-ms
ms.author: rajosh
ms.topic: tutorial
ms.date: 02/07/2021
ms.openlocfilehash: 14197516c0669055f756614b9559f1423703c6a8
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102053577"
---
# <a name="create-an-azure-sql-assessment"></a>Erstellen einer Azure SQL-Bewertung

Im Rahmen Ihrer Migrationsjourney zu Azure bewerten Sie Ihre lokalen Workloads, um die Cloudbereitschaft zu messen, Risiken zu identifizieren und Kosten und Komplexität zu schätzen.
In diesem Artikel wird veranschaulicht, wie Sie ermittelte SQL-Instanzen als Vorbereitung auf die Migration zu Azure SQL bewerten, indem Sie das Azure Migrate-Tool für die Ermittlung und Bewertung verwenden.

> [!Note]
> Das Feature für die Ermittlung und Bewertung von SQL Server-Instanzen und -Datenbanken, die in Ihrer VMware-Umgebung ausgeführt werden, befindet sich nun in der Vorschauphase. Verwenden Sie [**diesen Link**](https://aka.ms/AzureMigrate/SQL), und erstellen Sie ein Projekt in der Region **Australien, Osten**, um dieses Feature zu testen. Falls Sie bereits über ein Projekt in „Australien, Osten“ verfügen und dieses Feature ausprobieren möchten, sollten Sie sicherstellen, dass Sie im Portal die [**Voraussetzungen**](how-to-discover-sql-existing-project.md) erfüllt haben.

## <a name="before-you-start"></a>Vorbereitung

- Stellen Sie sicher, dass Sie ein Azure Migrate-Projekt [erstellt](./create-manage-projects.md) und das Azure Migrate-Tool für die Ermittlung und Bewertung hinzugefügt haben.
- Zum Erstellen einer Bewertung müssen Sie eine Azure Migrate-Appliance für [VMware](how-to-set-up-appliance-vmware.md) einrichten. Die Appliance ermittelt lokale Server und sendet Metadaten und Leistungsdaten an Azure Migrate. [Weitere Informationen](migrate-appliance.md)

## <a name="azure-sql-assessment-overview"></a>Übersicht über Azure SQL-Bewertungen
Sie können eine Azure SQL-Bewertung mit Größenkriterien vom Typ **Leistungsbasiert** erstellen. 

**Größenkriterien** | **Details** | **Daten**
--- | --- | ---
**Leistungsbasiert** | Bewertungen basierend auf gesammelten Leistungsdaten | Die empfohlene **Azure SQL-Konfiguration** basiert auf Leistungsdaten von SQL-Instanzen und -Datenbanken. Hierzu gehören CPU-Auslastung, Kernanzahl, Strukturierung und Größe von Datenbankdateien, E/A-Vorgänge für Dateien, Batchabfrage pro Sekunde und Arbeitsspeichergröße und Auslastung für die einzelnen Datenbanken.

Weitere Informationen zu Azure SQL-Bewertungen finden Sie [hier](concepts-azure-sql-assessment-calculation.md).

## <a name="run-an-assessment"></a>Durchführen einer Bewertung
Führen Sie eine Bewertung wie folgt aus:
1. Klicken Sie auf der Seite **Übersicht** unter **Windows, Linux und SQL Server** auf **Server bewerten und migrieren**.
    :::image type="content" source="./media/tutorial-assess-sql/assess-migrate.png" alt-text="Seite „Übersicht“ für Azure Migrate":::
2. Klicken Sie unter **Azure Migrate: Ermittlung und Bewertung** auf **Bewerten**, und wählen Sie **Azure SQL** als Bewertungstyp aus.
    :::image type="content" source="./media/tutorial-assess-sql/assess.png" alt-text="Dropdownliste zum Auswählen des Bewertungstyps „Azure SQL“":::
3. Unter **Server bewerten** sehen Sie, dass **Azure SQL** bereits als Bewertungstyp ausgewählt ist und für die Ermittlungsquelle die Standardeinstellung **Von Azure Migrate-Appliance erkannte Computer** angezeigt wird.

4. Klicken Sie auf **Bearbeiten**, um die Eigenschaften für die Bewertung zu überprüfen.
     :::image type="content" source="./media/tutorial-assess-sql/assess-servers-sql.png" alt-text="Schaltfläche „Bearbeiten“ zum Anpassen der Bewertungseigenschaften":::
5. Gehen Sie unter „Bewertungseigenschaften“ > **Zieleigenschaften** wie folgt vor:
    - Geben Sie unter **Zielspeicherort** die Azure-Region an, zu der Sie die Migration durchführen möchten. 
        - Empfehlungen zur Azure SQL-Konfiguration und zu den Kosten basieren auf dem von Ihnen angegebenen Standort. 
    - Unter **Zielbereitstellungstyp**:
        - Wählen Sie die Option **Empfohlen** aus, wenn mit Azure Migrate die Bereitschaft Ihrer SQL-Instanzen für die Migration zu Azure SQL Managed Instance und Azure SQL-Datenbank bewertet werden soll und zusätzlich die am besten geeignete Zielbereitstellungsoption, der Zieltarif, die Azure SQL-Konfiguration und monatliche Schätzungen empfohlen werden sollen. [Weitere Informationen](concepts-azure-sql-assessment-calculation.md)
        - Wählen Sie **Azure SQL-Datenbank** aus, falls Sie nur die Bereitschaft Ihrer SQL-Instanzen für die Migration zu Azure SQL-Datenbank-Instanzen bewerten lassen und den Zieltarif, die Azure SQL-Konfiguration und die monatlichen Schätzungen überprüfen möchten.
        - Wählen Sie **Azure SQL Managed Instance** aus, falls Sie nur die Bereitschaft Ihrer SQL-Instanzen für die Migration zu Azure SQL Managed Instance bewerten lassen und den Zieltarif, die Azure SQL-Konfiguration und die monatlichen Schätzungen überprüfen möchten.
    - Geben Sie unter **Reservierte Kapazität** an, ob Sie reservierte Kapazität nach der Migration für den SQL-Server verwenden möchten.
        - Wenn Sie eine Option für reservierte Kapazität auswählen, können Sie nicht „Abzug (%)“ auswählen.

6. Unter „Bewertungseigenschaften“ > **Bewertungskriterien**:
    - Für die Größenkriterien ist standardmäßig **Leistungsbasiert** ausgewählt. Dies bedeutet, dass Azure Migrate Leistungsmetriken von SQL-Instanzen und den damit verwalteten Datenbanken sammelt, um eine optimale Größe für die Konfiguration von Azure SQL-Datenbank oder Azure SQL Managed Instance empfehlen zu können. Sie können Folgendes angeben:
        - **Leistungsverlauf**, um den Datenzeitraum festzulegen, auf dem die Bewertung basieren soll. (Standardeinstellung: 1 Tag.)
        - **Perzentilwert der Nutzung**, um den Perzentilwert festzulegen, den Sie für das Leistungsbeispiel verwenden möchten. (Standardwert: 95. Perzentil.)
    - Geben Sie unter **Komfortfaktor** den Puffer an, den Sie während der Bewertung verwenden möchten. Hierbei werden Aspekte wie saisonale Nutzung, ein kurzer Leistungsverlauf und eine voraussichtliche Zunahme der zukünftigen Nutzung berücksichtigt. Beispiel für einen Komfortfaktor von „2“: 
        
        **Komponente** | **Tatsächliche Auslastung** | **Komfortfaktor hinzufügen (2.0)**
        --- | --- | ---
        Kerne | 2  | 4
        Arbeitsspeicher | 8 GB | 16 GB
   
7. Unter **Preise**:
    - Geben Sie unter **Angebot/Lizenzierungsprogramm** das Azure-Angebot an, falls Sie registriert sind. Derzeit können Sie nur zwischen „Nutzungsbasierte Bezahlung“ und „Dev/Test Pay-As-You-Go“ wählen. 
        - Sie können einen zusätzlichen Rabatt erzielen, indem Sie zusätzlich zum Angebot „Nutzungsbasierte Bezahlung“ die reservierte Kapazität und den Azure-Hybridvorteil anwenden. 
        - Sie können den Azure-Hybridvorteil zusätzlich zu „Dev/Test Pay-As-You-Go“ anwenden. Für die Bewertung wird das Anwenden von reservierter Kapazität zusätzlich zum Angebot „Dev/Test Pay-As-You-Go“ derzeit nicht unterstützt.
    - Wählen Sie unter **Dienstebene** die am besten geeignete Dienstebenenoption aus, um Ihre Geschäftsanforderungen für die Migration zu Azure SQL-Datenbank bzw. Azure SQL Managed Instance zu erfüllen: 
        - Wählen Sie die Option **Empfohlen** aus, wenn von Azure Migrate die am besten geeignete Dienstebene für Ihre Server empfohlen werden soll. Dies kann beispielsweise „Universell“ oder „Unternehmenskritisch“ sein. Weitere Informationen
        - Wählen Sie **Universell** aus, wenn Sie eine Azure SQL-Konfiguration erzielen möchten, die für budgetorientierte Workloads ausgelegt ist.
        - Wählen Sie **Unternehmenskritisch** aus, wenn Sie eine Azure SQL-Konfiguration erhalten möchten, die auf Workloads mit geringer Latenz und hoher Resilienz gegenüber Ausfällen mit schnellen Failovern ausgelegt ist.
    - Fügen Sie unter **Abzug (%)** alle abonnementspezifischen Rabatte hinzu, die Sie zusätzlich zum Azure-Angebot erhalten. Die Standardeinstellung ist 0 %.
    - Wählen Sie unter **Währung** die Abrechnungswährung für Ihr Konto aus.
    - Geben Sie unter **Azure-Hybridvorteil** an, ob Sie bereits über eine SQL Server-Lizenz verfügen. Wenn dies der Fall ist und eine aktive Software Assurance-Abdeckung für SQL Server-Abonnements besteht, können Sie sich für den Azure-Hybridvorteil bewerben, sofern Sie eigene Lizenzen für Azure mitbringen.
    - Klicken Sie auf Speichern, falls Sie Änderungen vorgenommen haben.
     :::image type="content" source="./media/tutorial-assess-sql/view-all.png" alt-text="Schaltfläche „Speichern“ für Bewertungseigenschaften":::
8. Klicken Sie unter **Server bewerten** auf „Weiter“.
9.  Geben Sie in **Server für die Bewertung auswählen** > **Bewertungsname** einen Namen für die Bewertung an.
10. Wählen Sie unter **Gruppe auswählen oder erstellen** die Option **Neu erstellen** aus, und geben Sie einen Namen für die Gruppe ein.
     :::image type="content" source="./media/tutorial-assess-sql/assessment-add-servers.png" alt-text="Schaltfläche für neue Gruppe":::
11. Wählen Sie die Appliance und dann die Server aus, die Sie der Gruppe hinzufügen möchten. Klicken Sie anschließend auf Weiter.
12. Sehen Sie sich unter **Überprüfen + Bewertung erstellen** die Bewertungsdetails an, und klicken Sie auf Bewertung erstellen, um die Gruppe zu erstellen und die Bewertung durchzuführen.
     :::image type="content" source="./media/tutorial-assess-sql/assessment-create.png" alt-text="Schaltfläche „Überprüfen + Bewertung erstellen“":::
13. Navigieren Sie nach dem Erstellen der Bewertung zur Kachel **Windows, Linux und SQL Server** > **Azure Migrate: Ermittlung und Bewertung**, und klicken Sie auf die Zahl neben der Azure SQL-Bewertung.
     :::image type="content" source="./media/tutorial-assess-sql/assessment-sql-navigation.png" alt-text="Navigation zur erstellten Bewertung":::
15. Klicken Sie auf den Namen der Bewertung, die Sie anzeigen möchten.

> [!NOTE]
> Da es sich bei Azure SQL-Bewertungen um leistungsbezogene Bewertungen handelt, empfehlen wir Ihnen, nach dem Starten einer Ermittlung mindestens einen Tag zu warten, bevor Sie eine Bewertung erstellen. Dies ermöglicht einen längeren Zeitraum zum Sammeln von Leistungsdaten mit höherer Zuverlässigkeit. Falls die Ermittlung noch nicht abgeschlossen ist, wird für die Bereitschaft Ihrer SQL-Instanzen der Status **Unbekannt** angezeigt. Idealerweise sollten Sie nach dem Starten der Ermittlung den **Zeitraum der Leistungsdauer abwarten, den Sie angegeben haben (Tag/Woche/Monat)** , damit eine Bewertung mit einer hohen Zuverlässigkeit erstellt bzw. neu berechnet werden kann. 

## <a name="review-an-assessment"></a>Überprüfen einer Bewertung

**Zeigen Sie eine Bewertung wie folgt an**:

1. **Windows, Linux und SQL Server** > **Azure Migrate: Ermittlung und Bewertung**, und klicken Sie auf die Zahl neben der Azure SQL-Bewertung.
2. Klicken Sie auf den Namen der Bewertung, die Sie anzeigen möchten. Beispiel (Schätzungen und Kosten gelten nur für dieses Beispiel): :::image type="content" source="./media/tutorial-assess-sql/assessment-sql-summary.png" alt-text="Übersicht über SQL-Bewertung":::
3. Sehen Sie sich die Zusammenfassung der Bewertung an. Sie können auch die Bewertungseigenschaften bearbeiten oder die Bewertung neu berechnen.

#### <a name="discovered-items"></a>Ermittelte Elemente

Hiermit wird die Anzahl von SQL-Servern, -Instanzen und -Datenbanken angegeben, die bei diesem Bewertungsvorgang bewertet wurden.
    
#### <a name="azure-readiness"></a>Azure-Bereitschaft

Hiermit wird die Verteilung der bewerteten SQL-Instanzen angegeben: 
    
**Zielbereitstellungstyp (in Bewertungseigenschaften)** | **Bereitschaft**   
--- | --- |
**Empfohlen** |  Für Azure SQL-Datenbank bereit, Für Azure SQL Managed Instance bereit, Potenziell für virtuellen Azure-Computer bereit, Bereitschaft unbekannt (falls die Ermittlung noch nicht abgeschlossen ist oder einige Ermittlungsprobleme behoben werden müssen)
**Azure SQL-Datenbank** oder **Azure SQL Managed Instance** | Für Azure SQL-Datenbank oder Azure SQL Managed Instance bereit, Nicht für Azure SQL-Datenbank oder Azure SQL Managed Instance bereit, Bereitschaft unbekannt (falls die Ermittlung noch nicht abgeschlossen ist oder einige Ermittlungsprobleme behoben werden müssen)
     
Sie können einen Drilldown ausführen, um sich mit den Details zu Migrationsproblemen und -warnungen vertraut zu machen, die Sie vor der Migration zu Azure SQL beheben können. Weitere Informationen finden Sie [hier](concepts-azure-sql-assessment-calculation.md). Sie können sich auch die empfohlenen Azure SQL-Konfigurationen für die Migration zu Azure SQL-Datenbank-Instanzen bzw. Managed Instances ansehen.
    
#### <a name="azure-sql-database-and-managed-instance-cost-details"></a>Details zu den Kosten für Azure SQL-Datenbank und Managed Instance

Die Schätzung der monatlichen Kosten enthält die Compute- und Speicherkosten für Azure SQL-Konfigurationen, die dem empfohlenen Azure SQL-Datenbank- bzw. Azure SQL Managed Instance-Bereitstellungstyp entsprechen. [Weitere Informationen](concepts-azure-sql-assessment-calculation.md#calculate-monthly-costs)


### <a name="review-readiness"></a>Überprüfen der Bereitschaft

1. Klicken Sie auf **Azure SQL-Bereitschaft**.
    :::image type="content" source="./media/tutorial-assess-sql/assessment-sql-readiness.png" alt-text="Details zur Azure SQL-Bereitschaft":::
1. Sehen Sie sich auf der Seite mit der Azure SQL-Bereitschaft die Angaben unter **Azure SQL DB-Bereitschaft** und **Azure SQL MI-Bereitschaft** für die bewerteten SQL-Instanzen an:
    - **Bereit**: Die Instanz ist zum Migrieren zu Azure SQL DB/MI bereit, und es sind keine Migrationsprobleme oder -warnungen vorhanden. 
        - Bereit (Hyperlink und blaues Infosymbol): Die Instanz ist zum Migrieren zu Azure SQL DB/MI bereit, und es bestehen keine Migrationsprobleme, aber einige Migrationswarnungen müssen überprüft werden. Sie können auf den Hyperlink klicken, um die Migrationswarnungen zu überprüfen und die empfohlene Anleitung zur Behebung anzuzeigen: :::image type="content" source="./media/tutorial-assess-sql/assess-ready.png" alt-text="Bewertung mit Status „Bereit“":::       
    - **Nicht bereit**: Für die Instanz besteht mindestens ein Problem für die Migration zu Azure SQL DB/MI. Sie können auf den Hyperlink klicken, um die Migrationsprobleme zu überprüfen und die empfohlene Anleitung zur Behebung anzuzeigen.
    - **Unbekannt**: Die Bereitschaft kann von Azure Migrate nicht bewertet werden, weil die Ermittlung noch nicht abgeschlossen ist oder weil dabei Probleme aufgetreten sind, die auf dem Blatt „Benachrichtigungen“ behoben werden müssen. Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft-Support. 
1. Sehen Sie sich den empfohlenen Bereitstellungstyp für die SQL-Instanz an, den Sie über die folgende Matrix ermitteln können:

    - **Zielbereitstellungstyp** (gemäß Auswahl in Bewertungseigenschaften): **Empfohlen**

        **Azure SQL DB-Bereitschaft** | **Azure SQL MI-Bereitschaft** | **Empfohlener Bereitstellungstyp** | **Azure SQL-Konfiguration und Kostenschätzungen berechnet?**
         --- | --- | --- | --- |
        Bereit | Bereit | Azure SQL DB oder Azure SQL MI [Weitere Informationen](concepts-azure-sql-assessment-calculation.md#recommended-deployment-type) | Ja
        Bereit | Nicht bereit oder Unbekannt | Azure SQL-Datenbank | Ja
        Nicht bereit oder Unbekannt | Bereit | Azure SQL MI  | Ja
        Nicht bereit. | Nicht bereit. | Potenziell für virtuellen Azure-Computer bereit [Weitere Informationen](concepts-azure-sql-assessment-calculation.md#potentially-ready-for-azure-vm) | Nein
        Nicht bereit oder Unbekannt | Nicht bereit oder Unbekannt | Unbekannt | Nein
    
    - **Zielbereitstellungstyp** (gemäß Auswahl in Bewertungseigenschaften): **Azure SQL-Datenbank**
    
        **Azure SQL DB-Bereitschaft** | **Azure SQL-Konfiguration und Kostenschätzungen berechnet?**
        --- | --- |
        Bereit | Ja
        Nicht bereit. | Nein
        Unknown | Nein
    
    - **Zielbereitstellungstyp** (gemäß Auswahl in Bewertungseigenschaften): **Azure SQL MI**
    
        **Azure SQL MI-Bereitschaft** | **Azure SQL-Konfiguration und Kostenschätzungen berechnet?**
         --- | --- |
        Bereit | Ja
        Nicht bereit. | Nein
        Unknown | Nein

4. Klicken Sie auf die Drilldownoption für den Instanznamen, um die Anzahl von Benutzerdatenbanken, Instanzdetails (einschließlich Instanzeigenschaften), Computedetails (pro Instanz) und Details zum Quelldatenbankspeicher anzuzeigen.
5. Klicken Sie auf die Anzahl von Benutzerdatenbanken, um die Liste mit den Datenbanken und den zugehörigen Details zu überprüfen. Beispiel (Schätzungen und Kosten gelten nur für dieses Beispiel): :::image type="content" source="./media/tutorial-assess-sql/assessment-db.png" alt-text="Details zur SQL-Instanz":::
5. Klicken Sie in der Spalte „Migrationsprobleme“ auf „Details anzeigen“, um die Migrationsprobleme und -warnungen für einen bestimmten Zielbereitstellungstyp zu überprüfen. 
    :::image type="content" source="./media/tutorial-assess-sql/assessment-db-issues.png" alt-text="DB-Migrationsprobleme und -warnungen":::

### <a name="review-cost-estimates"></a>Überprüfen der Kostenschätzungen
Auf der Seite mit der Zusammenfassung der Bewertung werden die geschätzten monatlichen Compute- und Speicherkosten für Azure SQL-Konfigurationen angezeigt, die für den empfohlenen Bereitstellungstyp für Azure SQL-Datenbanken bzw. Managed Instances gelten.

1. Überprüfen Sie die monatlichen Gesamtkosten. Die Kosten werden für alle SQL-Instanzen aggregiert, die in der bewerteten Gruppe enthalten sind.
    - Die Kostenschätzungen basieren auf der empfohlenen Azure SQL-Konfiguration für eine Instanz. 
    - Die geschätzten monatlichen Kosten für Compute und Speicher werden angezeigt. Beispiel (Schätzungen und Kosten gelten nur für das Beispiel):
    
    :::image type="content" source="./media/tutorial-assess-sql/assessment-sql-cost.png" alt-text="Kostendetails":::

1. Sie können einen Drilldown auf einer Instanzebene ausführen, um die Azure SQL-Konfiguration und die Kostenschätzungen für die jeweilige Instanzebene anzuzeigen.  
1. Darüber hinaus können Sie auch einen Drilldown zur Datenbankliste ausführen, um die Azure SQL-Konfiguration und die Kostenschätzungen pro Datenbank zu überprüfen, wenn eine Konfiguration für Azure SQL-Datenbank empfohlen wird.

### <a name="review-confidence-rating"></a>Prüfen der Zuverlässigkeitsstufe
Von Azure Migrate wird allen Azure SQL-Bewertungen eine Zuverlässigkeitsstufe zugewiesen. Dieser Vorgang basiert auf der Verfügbarkeit der Datenpunkte für die Leistung bzw. Auslastung, die benötigt werden, um die Bewertung für alle relevanten SQL-Instanzen und -Datenbanken zu berechnen. Die Bewertung kann einen Wert zwischen einem Stern (am niedrigsten) und fünf Sternen (am höchsten) aufweisen.
 
Anhand der Zuverlässigkeitsstufe können Sie die Zuverlässigkeit der Größenempfehlungen in der Bewertung besser einschätzen.  Die Zuverlässigkeitsstufen lauten wie unten angegeben.

**Verfügbarkeit von Datenpunkten** | **Zuverlässigkeitsstufe**
--- | ---
0 % bis 20 % | 1 Stern
21 % bis 40 % | 2 Sterne
41 % bis 60 % | 3 Sterne
61 % bis 80 % | 4 Sterne
81 % bis 100 % | 5 Sterne

Informieren Sie sich über [Zuverlässigkeitsstufen](concepts-azure-sql-assessment-calculation.md#confidence-ratings).

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über die [Berechnung von Azure SQL-Bewertungen](concepts-azure-sql-assessment-calculation.md).
- Steigen Sie in die Migration von SQL-Instanzen und -Datenbanken mit [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) ein.
