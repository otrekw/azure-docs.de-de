---
title: Integrieren von Azure Security Center in Azure VMware Solution
description: Erfahren Sie, wie Sie Ihre Azure VMware Solution-VMs über das Azure Security Center-Dashboard mit den nativen Sicherheitstools von Azure schützen.
ms.topic: how-to
ms.date: 06/14/2021
ms.openlocfilehash: 6060be11ada028234b11e74f56de8c9741fc4cd4
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111754239"
---
# <a name="integrate-azure-security-center-with-azure-vmware-solution"></a>Integrieren von Azure Security Center in Azure VMware Solution 

Azure Security Center bietet erweiterten Bedrohungsschutz für Azure VMware Solution-VMs und lokale virtuelle Computer. Es bewertet das Sicherheitsrisiko von Azure VMware Solution-VMs und gibt bei Bedarf Warnungen aus. Diese Sicherheitswarnungen können zur Behebung an Azure Monitor weitergeleitet werden. Sie können in Azure Security Center Sicherheitsrichtlinien definieren. Weitere Informationen finden Sie unter [Arbeiten mit Sicherheitswarnungen](../security-center/tutorial-security-policy.md). 

Azure Security Center bietet viele Features. Dazu gehören:
- Überwachung der Dateiintegrität
- Erkennung dateiloser Angriffe
- Bewertung von Patches für Betriebssysteme 
- Bewertung von Sicherheitsfehlkonfigurationen
- Bewertung von Endpoint Protection

Das Diagramm zeigt die integrierte Überwachungsarchitektur der integrierten Sicherheit für Azure VMware Solution-VMs.
 
:::image type="content" source="media/azure-security-integration/azure-integrated-security-architecture.png" alt-text="Diagramm: Architektur der integrierten Azure-Sicherheit" border="false":::


## <a name="prerequisites"></a>Voraussetzungen

- [Planen Sie die optimierte Verwendung von Security Center](../security-center/security-center-planning-and-operations-guide.md).

- [Überprüfen Sie die unterstützten Plattformen in Security Center](../security-center/security-center-os-coverage.md).

- [Erstellen Sie einen Log Analytics-Arbeitsbereich](../azure-monitor/logs/quick-create-workspace.md), um Daten aus verschiedenen Quellen zu sammeln.

- [Aktivieren Sie Azure Security Center für Ihr Abonnement](../security-center/security-center-get-started.md). 

   >[!NOTE]
   >Azure Security Center ist ein vorkonfiguriertes Tool, für das keine Bereitstellung erforderlich ist, das aber im Azure-Portal aktiviert werden muss.

- [Aktivieren Sie Azure Defender](../security-center/enable-azure-defender.md). 


## <a name="add-azure-vmware-solution-vms-to-security-center"></a>Hinzufügen von Azure VMware Solution-VMs zu Security Center

1. Suchen Sie im Azure-Portal nach **Azure Arc**, und wählen Sie den Eintrag aus.

2. Klicken Sie unter „Ressourcen“ auf **Server** und dann auf **+Hinzufügen**.

   :::image type="content" source="media/azure-security-integration/add-server-to-azure-arc.png" alt-text="Screenshot der Azure Arc-Seite „Server“ zum Hinzufügen einer Azure VMware Solution-VM zu Azure":::

3. Wählen Sie **Skript generieren** aus.
 
   :::image type="content" source="media/azure-security-integration/add-server-using-script.png" alt-text="Screenshot einer Azure Arc-Seite mit der Option zum Hinzufügen eines Servers mithilfe eines interaktiven Skripts"::: 
 
4. Klicken Sie auf der Registerkarte **Voraussetzungen** auf **Weiter**.

5. Geben Sie auf der Registerkarte **Ressourcendetails** die folgenden Details ein, und wählen Sie dann **Weiter: Tags** aus. 

    - Subscription

    - Resource group

    - Region 

    - Betriebssystem

    - Details zum Proxyserver
    
6. Klicken Sie auf der Registerkarte **Tags** auf **Weiter**.

7. Klicken Sie auf der Registerkarte **Skript herunterladen und ausführen** auf **Herunterladen**.

8. Geben Sie Ihr Betriebssystem an, und führen Sie das Skript auf Ihrer Azure VMware Solution-VM aus.

## <a name="view-recommendations-and-passed-assessments"></a>Anzeigen von Empfehlungen und bestandenen Bewertungen

Damit erhalten Sie Details zur Sicherheitsintegrität Ihrer Ressource. 

1. Wählen Sie in Azure Security Center **Inventar** aus.

2. Wählen Sie als Ressourcentyp **Server – Azure Arc** aus.
 
   :::image type="content" source="media/azure-security-integration/select-resource-in-security-center.png" alt-text="Screenshot der Azure Security Center-Seite „Inventar“ mit der unter „Ressourcentyp“ ausgewählten Option „Server – Azure Arc“":::

3. Wählen Sie den Namen Ihrer Ressource aus. Daraufhin wird eine Seite geöffnet, auf die Details zur Sicherheitsintegrität Ihrer Ressource angezeigt werden.

4. Wählen Sie unter **Empfehlungsliste** die Registerkarten **Empfehlungen**, **Bestandene Bewertungen** und **Nicht verfügbare Bewertungen** aus, um die jeweiligen Details anzuzeigen.

   :::image type="content" source="media/azure-security-integration/view-recommendations-assessments.png" alt-text="Screenshot von Azure Security Center mit Sicherheitsempfehlungen und -bewertungen":::

## <a name="deploy-an-azure-sentinel-workspace"></a>Bereitstellen eines Azure Sentinel-Arbeitsbereichs

Da Azure Sentinel auf einem Log Analytics-Arbeitsbereich basiert, müssen Sie lediglich den Log Analytics-Arbeitsbereich auswählen, den Sie verwenden möchten.

1. Suchen Sie im Azure-Portal nach **Azure Sentinel**, und wählen Sie den Eintrag aus.

2. Klicken Sie auf der Seite mit den Azure Sentinel-Arbeitsbereichen auf **+Hinzufügen**.

3. Wählen Sie den Log Analytics-Arbeitsbereich aus, und klicken Sie auf **Hinzufügen**.

## <a name="enable-data-collector-for-security-events"></a>Aktivieren eines Datensammlers für sicherheitsrelevante Ereignisse

1. Wählen Sie auf der Seite der Azure Sentinel-Arbeitsbereiche den konfigurierten Arbeitsbereich aus.

2. Wählen Sie unter „Konfiguration“ die Option **Datenconnectors** aus.

3. Wählen Sie in der Spalte „Connectorname“ den Eintrag **Sicherheitsereignisse** aus, und klicken Sie dann auf **Connectorseite öffnen**.

4. Wählen Sie auf der Connectorseite die Ereignisse aus, die Sie streamen möchten, und klicken Sie dann auf **Änderungen übernehmen**.

   :::image type="content" source="media/azure-security-integration/select-events-you-want-to-stream.png" alt-text="Screenshot der Azure Sentinel-Seite „Sicherheitsereignisse“, auf der Sie die zu streamenden Ereignisse auswählen können":::

## <a name="connect-azure-sentinel-with-azure-security-center"></a>Verbinden von Azure Sentinel mit Azure Security Center  

1. Wählen Sie auf der Seite der Azure Sentinel-Arbeitsbereiche den konfigurierten Arbeitsbereich aus.

2. Wählen Sie unter „Konfiguration“ die Option **Datenconnectors** aus.

3. Wählen Sie **Azure Security Center** aus der Liste aus, und klicken Sie dann auf **Connectorseite öffnen**.

    :::image type="content" source="media/azure-security-integration/connect-security-center-with-azure-sentinel.png" alt-text="Screenshot der Seite „Datenconnectors“ in Azure Sentinel mit der Auswahl zum Herstellen einer Verbindung zwischen Azure Security Center und Azure Sentinel":::

4. Klicken Sie auf **Verbinden**, um Azure Security Center mit Azure Sentinel zu verbinden.

5. Aktivieren Sie **Incident erstellen**, um einen Incident für Azure Security Center zu generieren.

## <a name="create-rules-to-identify-security-threats"></a>Erstellen von Regeln zum Identifizieren von Sicherheitsbedrohungen

Nachdem Sie Datenquellen mit Azure Sentinel verbunden haben, können Sie Regeln erstellen, um Warnungen für erkannte Bedrohungen zu generieren. Im folgenden Beispiel wird eine Regel für Versuche erstellt, sich mit einem falschen Kennwort bei einem Windows-Server anzumelden.

1. Wählen Sie auf der Übersichtsseite von Azure Sentinel unter „Konfigurationen“ die Option **Analyse** aus.

2. Wählen Sie unter „Konfigurationen“ die Option **Analyse** aus.

3. Klicken Sie auf **+Erstellen**, und wählen Sie in der Dropdownliste den Eintrag **Geplante Abfrageregel** aus.

4. Geben Sie auf der Registerkarte **Allgemein** die erforderlichen Informationen ein, und wählen Sie dann **Weiter: Regellogik festlegen** aus.

    - Name

    - BESCHREIBUNG

    - Taktik

    - Schweregrad

    - Status

5. Geben Sie auf der Registerkarte **Regellogik festlegen** die erforderlichen Informationen ein, und wählen Sie dann **Weiter** aus.

    - Regelabfrage (gezeigt wird die Beispielabfrage)
    
        ```
        SecurityEvent
        |where Activity startswith '4625'
        |summarize count () by IpAddress,Computer
        |where count_ > 3
        ```
        
    - Entitäten zuordnen

    - Abfrageplanung

    - Benachrichtigungsschwellenwert

    - Ereignisgruppierung

    - Unterdrücken


6. Aktivieren Sie auf der Registerkarte **Incidenteinstellungen** die Option **Incidents aus Warnungen erstellen, die von dieser Analyseregel ausgelöst werden**, und wählen Sie **Weiter: Automatisierte Antwort** aus.
 
    :::image type="content" source="media/azure-security-integration/create-new-analytic-rule-wizard.png" alt-text="Screenshot des Analyseregel-Assistenten zum Erstellen einer neuen Regel in Azure Sentinel mit aktivierter Option „Incidents aus Warnungen erstellen, die von dieser Analyseregel ausgelöst werden“":::

7. Klicken Sie auf **Weiter: Review** (Weiter: Überprüfen).

8. Überprüfen Sie die Informationen auf der Registerkarte **Überprüfen + erstellen**, und wählen Sie **Erstellen** aus.

>[!TIP]
>Nach dem dritten fehlerhaften Anmeldeversuch beim Windows-Server löst die erstellte Regel einen Incident für jeden erfolglosen Versuch aus.

## <a name="view-alerts"></a>Anzeigen von Warnungen

Sie können generierte Incidents mit Azure Sentinel anzeigen. Sie können auch Incidents zuweisen und nach der Auflösung schließen – alles innerhalb von Azure Sentinel.

1. Wechseln Sie zur Übersichtsseite von Azure Sentinel.

2. Wählen Sie unter „Bedrohungsmanagement“ die Option **Incidents** aus.

3. Wählen Sie einen Incident aus, und weisen Sie ihn dann einem Team zur Auflösung zu.

    :::image type="content" source="media/azure-security-integration/assign-incident.png" alt-text="Screenshot der Azure Sentinel-Seite mit Incidents mit ausgewähltem Incident und der Option zum Zuweisen zur Auflösung":::

>[!TIP]
>Nachdem das Problem behoben wurde, können Sie den Incident schließen.

## <a name="hunt-security-threats-with-queries"></a>Suchen nach Sicherheitsbedrohungen mithilfe von Abfragen

Sie können Abfragen erstellen oder vordefinierte Abfragen in Azure Sentinel verwenden, um Bedrohungen in Ihrer Umgebung zu identifizieren. In den folgenden Schritten wird eine vordefinierte Abfrage ausgeführt.

1. Wählen Sie auf der Azure Sentinel-Übersichtsseite unter „Bedrohungsverwaltung“ die Option **Suche** aus. Eine Liste vordefinierter Abfragen wird angezeigt.

   >[!TIP]
   >Sie können auch eine neue Abfrage erstellen, indem Sie **+ Neue Abfrage** auswählen. 
   >
   >:::image type="content" source="media/azure-security-integration/create-new-query.png" alt-text="Screenshot der Azure Sentinel-Seite für das Hunting mit hervorgehobener Option „+Neue Abfrage“":::

3. Wählen Sie eine Abfrage aus, und klicken Sie dann auf **Abfrage ausführen**.

4. Klicken Sie auf **Ergebnisse anzeigen**, um die Ergebnisse zu überprüfen.



## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich mit dem Schützen Ihrer Azure VMware Solution-VMs vertraut gemacht haben, informieren Sie sich über die folgenden Themen:

- [Verwenden des Azure Defender-Dashboards](../security-center/azure-defender-dashboard.md)
- [Erweiterte Erkennung von mehrstufigen Angriffen in Azure Sentinel](../azure-monitor/logs/quick-create-workspace.md)
- [Integrieren von nativen Azure-Diensten in Azure VMware Solution](integrate-azure-native-services.md)
