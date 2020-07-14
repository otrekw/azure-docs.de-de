---
title: Advanced Data Security für SQL-Computer im Azure Security Center (Vorschau)
description: Erfahren Sie, wie Sie Advanced Data Security für SQL-Computer in Azure Security Center aktivieren.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: ba46c460-6ba7-48b2-a6a7-ec802dd4eec2
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2020
ms.author: memildin
ms.openlocfilehash: f159d2cdc48b144d0c75c62cd8a7ba6667424243
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2020
ms.locfileid: "86043868"
---
# <a name="advanced-data-security-for-sql-machines-preview"></a>Advanced Data Security für SQL-Computer (Vorschau)

Advanced Data Security für SQL-Computer im Azure Security Center schützt in Azure, in anderen Cloudumgebungen und sogar auf lokalen Computern gehostete SQL-Server-Instanzen. Dies erweitert den Schutz für Ihre nativen Azure-SQL Server-Instanzen, um vollständige Unterstützung für Hybridumgebungen bereitzustellen.

Zu dieser Previewfunktion zählen aktuell die Funktionen zur Ermittlung und Verringerung potenzieller Datenbankschwachstellen und Erkennung ungewöhnlicher Aktivitäten, die Bedrohungen für Ihre Datenbank darstellen können: 

* **Sicherheitsrisikobewertung**: der Scandienst, mit dem potenzielle Schwachstellen in der Datenbank ermittelt, nachverfolgt und behoben werden. Bewertungsscans bieten einen Überblick über den Sicherheitszustand Ihrer SQL-Computer und Einzelheiten zu allen Sicherheitsergebnissen.

* [Advanced Threat Protection](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview): Der Erkennungsdienst, der Ihre SQL Server-Instanzen kontinuierlich auf Bedrohungen wie die Einschleusung von SQL-Befehlen, Brute-Force-Angriffe und Berechtigungsmissbrauch überwacht. Dieser Dienst bietet aktionsorientierte Sicherheitswarnungen in Azure Security Center mit Details zur verdächtigen Aktivität, Anleitungen zum Minimieren der Bedrohungen und Optionen für die Fortsetzung ihrer Untersuchungen mit Azure Sentinel.

>[!TIP]
> Advanced Data Security für SQL-Computer ist eine Erweiterung des [Advanced Data Security-Pakets](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security) von Azure Security Center, das für Azure SQL Database, Azure Synapse und SQL Managed Instance verfügbar ist.


## <a name="set-up-advanced-data-security-for-sql-machines"></a>Einrichten von Advanced Data Security für SQL-Computer 

Das Einrichten von Advanced Data Security für SQL-Computer im Azure Security Center umfasst zwei Schritte:

* Bereitstellen des Log Analytics-Agents auf dem Host Ihrer SQL Server-Instanz. Dies ermöglicht die Verbindung mit Azure.

* Aktivieren des optionalen Pakets auf der Security Center-Seite mit den Preisen und Einstellungen.

Diese beiden Schritte werden nachfolgend beschrieben.

### <a name="step-1-provision-the-log-analytics-agent-on-your-sql-servers-host"></a>Schritt 1: Bereitstellen des Log Analytics-Agents auf dem Host Ihrer SQL Server-Instanz:

- **SQL Server auf einem virtuellen Azure-Computer**: Wenn Ihr SQL-Computer auf einem virtuellen Azure-Computer gehostet wird, können Sie [den Log Analytics-Agent automatisch bereitstellen](security-center-enable-data-collection.md#workspace-configuration). Alternativ können Sie das manuelle Verfahren zum [Hinzufügen eines virtuellen Azure-Computers](quick-onboard-azure-stack.md#add-the-virtual-machine-extension-to-your-existing-azure-stack-virtual-machines) befolgen.

- **SQL-Server unter Azure Arc**: Wenn Ihre SQL Server-Instanz auf einem [Azure Arc](https://docs.microsoft.com/azure/azure-arc/)-Computer gehostet wird, können Sie den Log Analytics-Agent unter Verwendung der Security Center-Empfehlung „Log Analytics-Agent muss auf Ihren Windows-basierten Azure Arc-Computern installiert sein“ bereitstellen. Alternativ können Sie das manuelle Verfahren in der [Azure Arc-Dokumentation](https://docs.microsoft.com/azure/azure-arc/servers/manage-vm-extensions#enable-extensions-from-the-portal) befolgen.

- **Lokale SQL Server-Instanz**: Wenn Ihre SQL Server-Instanz auf einem lokalen Windows-Computer ohne Azure Arc gehostet wird, haben Sie zwei Möglichkeiten, ihn mit Azure zu verbinden:
    
    - **Bereitstellen von Azure Arc**: Sie können jeden Windows-Computer mit Security Center verbinden. Azure Arc bietet jedoch eine tiefere Integration über Ihre *gesamte* Azure-Umgebung. Wenn Sie Azure Arc einrichten, wird die Seite **SQL Server – Azure Arc** im Portal angezeigt, und Ihre Sicherheitswarnungen werden auf dieser Seite auf einer dedizierten Registerkarte **Sicherheit** angezeigt. Die erste und empfohlene Option ist also, [Azure Arc auf dem Host einzurichten](https://docs.microsoft.com/azure/azure-arc/servers/onboard-portal#install-and-validate-the-agent-on-windows) und den Anweisungen für **SQL-Server unter Azure Arc** (siehe oben) zu folgen.
        
    - **Verbinden des Windows-Computers ohne Azure Arc**: Wenn Sie sich dafür entscheiden, eine auf einem Windows-Computer ausgeführte SQL Server-Instanz ohne Azure Arc zu verbinden, folgen Sie den Anweisungen unter [Verbinden von Windows-Computern mit Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/agent-windows).


### <a name="step-2-enable-the-optional-bundle-in-security-centers-pricing-and-settings-page"></a>Schritt 2: Aktivieren des optionalen Pakets auf der Security Center-Seite „Preise und Einstellungen“:

1. Öffnen Sie auf der Security Center-Randleiste die Seite **Preise und Einstellungen**.

    - Wenn Sie den **Standardarbeitsbereich von Azure Security Center** (namens „defaultworkspace-[Ihre Abonnement-ID]-[Region]“) verwenden, wählen Sie das relevante **Abonnement** aus.

    - Wenn Sie **einen nicht standardmäßigen Arbeitsbereich** verwenden, wählen Sie den relevanten **Arbeitsbereich** aus (geben Sie ggf. den Namen des Arbeitsbereichs in den Filter ein):

        ![title](./media/security-center-advanced-iaas-data/pricing-and-settings-workspaces.png)


1. Aktivieren Sie die Option **SQL Server-Instanzen auf Computern (Vorschau)** . 

    [![Security Center-Seite „Preise“ mit optionalen Paketen](media/security-center-advanced-iaas-data/sql-servers-on-vms-in-pricing-small.png)](media/security-center-advanced-iaas-data/sql-servers-on-vms-in-pricing-large.png#lightbox)

    Advanced Data Security für SQL Server-Instanzen auf Computern wird auf allen SQL Server-Instanzen aktiviert, die mit dem ausgewählten Arbeitsbereich verbunden sind. Der Schutz ist nach dem ersten Neustart der SQL Server-Instanz vollständig aktiv. 

    >[!TIP] 
    > Um einen neuen Arbeitsbereich zu erstellen, befolgen Sie die Anweisungen unter [Erstellen eines Log Analytics-Arbeitsbereichs](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).


1. Konfigurieren Sie optional eine E-Mail-Benachrichtigung für Sicherheitswarnungen. 
    Sie können eine Liste von Empfängern festlegen, die eine E-Mail-Benachrichtigung erhalten sollen, wenn Security Center-Warnungen generiert werden. Die E-Mail enthält einen direkten Link zur Warnung in Azure Security Center mit allen relevanten Details. Weitere Informationen finden Sie unter [Einrichten von E-Mail-Benachrichtigungen für Sicherheitswarnungen](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details).



## <a name="explore-vulnerability-assessment-reports"></a>Untersuchen der Berichte zur Sicherheitsrisikobewertung

Der Sicherheitsrisikobewertungs-Dienst scannt Ihre Datenbanken einmal pro Woche. Die Scans werden an dem Tag der Woche ausgeführt, an dem Sie den Dienst aktiviert haben.

Das Sicherheitsrisikobewertungs-Dashboard bietet einen Überblick über Ihre Bewertungsergebnisse für alle Ihre Datenbanken, zusammen mit einer Zusammenfassung der fehlerfreien und fehlerhaften Datenbanken sowie einer Gesamtzusammenfassung der fehlgeschlagenen Prüfungen nach Risikoverteilung.

Sie können die Ergebnisse der Sicherheitsrisikobewertung direkt aus Security Center anzeigen.

1. Öffnen Sie in der Security Center-Seitenleiste die Seite **Empfehlungen**, und wählen Sie die Empfehlung **Sicherheitsrisiken für SQL Server-Instanzen auf Ihren Computern müssen entschärft werden (Vorschau)** aus. Weitere Informationen finden Sie unter [Security Center-Empfehlungen](security-center-recommendations.md). 


    [![Empfehlung **Sicherheitsrisiken für SQL Server-Instanzen auf Ihren Computern müssen entschärft werden (Vorschau)**](media/security-center-advanced-iaas-data/data-and-storage-sqldb-vulns-on-vm.png)](media/security-center-advanced-iaas-data/data-and-storage-sqldb-vulns-on-vm.png#lightbox)

    Die detaillierte Ansicht für diese Empfehlung wird angezeigt.

    [![Detaillierte Ansicht für die Empfehlung **Sicherheitsrisiken für SQL Server-Instanzen auf Ihren Computern müssen entschärft werden (Vorschau)**](media/security-center-advanced-iaas-data/all-servers-view.png)](media/security-center-advanced-iaas-data/all-servers-view.png#lightbox)

1. Für weitere Details führen Sie einen Drilldown aus:

    * Wählen Sie den gewünschten Server aus, um eine Übersicht über gescannte Ressourcen (Datenbanken) und die Liste der getesteten Sicherheitsüberprüfungen zu erhalten.

    * Wählen Sie die gewünschte Datenbank aus, um eine Übersicht über die Sicherheitsrisiken zu erhalten, die nach einer bestimmten SQL-Datenbank gruppiert sind.

    Die Sicherheitsüberprüfungen werden in jeder Ansicht nach **Schweregrad** sortiert. Klicken Sie auf eine bestimmte Sicherheitsüberprüfung, um einen Detailbereich mit einer **Beschreibung** anzuzeigen sowie **Empfehlungen zur Behebung von Problemen** und andere verwandte Informationen wie zur **Auswirkung** oder **Benchmark** zu erhalten.

## <a name="advanced-threat-protection-for-sql-servers-on-machines-alerts"></a>Warnungen der Advanced Threat Protection für SQL Server-Instanzen auf Computern
Warnungen werden bei ungewöhnlichen und potenziell schädlichen Zugriffsversuchen oder Exploitvorgängen für SQL-Computer generiert. Diese Ereignisse können Warnungen auslösen, die im [Abschnitt „Warnungen für SQL-Datenbank und SQL Data Warehouse“ der Warnungsreferenzseite](alerts-reference.md#alerts-sql-db-and-warehouse) angezeigt werden.



## <a name="explore-and-investigate-security-alerts"></a>Erkunden und Untersuchen von Sicherheitswarnungen

Die Sicherheitswarnungen sind auf der Security Center-Seite „Warnungen“, auf der Registerkarte „Sicherheit“ der Ressource oder über den direkten Link in den Benachrichtigungs-E-Mails verfügbar.

1. Um die Warnungen anzuzeigen, wählen Sie **Sicherheitswarnungen** auf der Security Center-Randleiste und dann eine Warnung aus.

1. Warnungen sind eigenständig und enthalten jeweils ausführliche Schritte zur Bereinigung und Untersuchungsinformationen. Sie können weitere Untersuchungen durchführen, indem Sie andere Azure Security Center- und Azure Sentinel-Funktionen für eine umfassendere Ansicht verwenden:

    * Aktivieren Sie die Überwachungsfunktion von SQL Server, um weitere Untersuchungen durchzuführen. Wenn Sie Azure Sentinel-Benutzer sind, können Sie die SQL-Überwachungsprotokolle aus den Windows-Sicherheitsprotokollereignissen in Sentinel hochladen, wo Ihnen umfassende Untersuchungsfunktionen zur Verfügung stehen. [Weitere Informationen zur SQL Server-Überwachung](https://docs.microsoft.com/sql/relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification?view=sql-server-ver15).
    * Um Ihren Sicherheitsstatus zu verbessern, verwenden Sie die in jeder Warnung aufgeführten Security Center-Empfehlungen für den Hostcomputer. Dadurch werden die Risiken zukünftiger Angriffe reduziert. 

    [Weitere Informationen zum Verwalten von und Reagieren auf Warnungen](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts).


## <a name="next-steps"></a>Nächste Schritte

Verwandte Informationen finden Sie im folgenden Artikel:

- [Sicherheitswarnungen für SQL-Datenbank und SQL Data Warehouse](alerts-reference.md#alerts-sql-db-and-warehouse)
- [Einrichten einer E-Mail-Benachrichtigung für Sicherheitswarnungen](security-center-provide-security-contact-details.md)
- [Weitere Informationen zu Azure Sentinel](https://docs.microsoft.com/azure/sentinel/)
- [Advanced Data Security-Paket im Azure Security Center](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security)