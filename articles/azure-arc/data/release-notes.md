---
title: Azure Arc-fähige Datendienste – Versionshinweise
description: Aktuelle Versionshinweise
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
ms.date: 06/02/2021
ms.topic: conceptual
ms.openlocfilehash: fc4dd4bcdb18b33fa9c6098c32e0aefecb7c46ee
ms.sourcegitcommit: 070122ad3aba7c602bf004fbcf1c70419b48f29e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2021
ms.locfileid: "111439636"
---
# <a name="release-notes---azure-arc-enabled-data-services-preview"></a>Versionshinweise – Azure Arc-fähige Datendienste (Vorschauversion)

In diesem Artikel werden die Funktionen, Features und Verbesserungen hervorgehoben, die kürzlich für Azure Arc-fähige Datendienste veröffentlicht bzw. durchgeführt wurden. 

## <a name="may-2021"></a>Mai 2021

Diese Vorschauversion wird am 2. Juni 2021 veröffentlicht.

Als Previewfunktion unterliegt die in diesem Artikel vorgestellte Technologie den [zusätzlichen Nutzungsbedingungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

### <a name="breaking-change"></a>Unterbrechende Änderung

- Native Bereitstellungsvorlagen von Kubernetes wurden geändert. Aktualisieren Sie Ihre YML-Vorlagen.
    - Aktualisierte Vorlagen für Datencontroller, Bootstrapper und verwaltete SQL-Instanzen: [GitHub microsoft/azure-arc pr 574](https://github.com/microsoft/azure_arc/pull/574)
    - Aktualisierte Vorlagen für PostgreSQL Hyperscale: [GitHub microsoft/azure-arc pr 574](https://github.com/microsoft/azure_arc/pull/574)

### <a name="whats-new"></a>Neues

#### <a name="platform"></a>Plattform

- Erstellen und Löschen von Servergruppen für Datencontroller, verwaltete SQL-Instanzen und PostgreSQL Hyperscale auf dem Azure-Portal. 
- Validieren Sie Portalaktionen beim Löschen von Azure Arc-Datendiensten. Das Portal warnt beispielsweise, wenn Sie versuchen, den Datencontroller zu löschen, wenn mit ihm SQL Managed Instances bereitgestellt werden.
- Erstellen Sie benutzerdefinierte Konfigurationsprofile, um benutzerdefinierte Einstellungen zu unterstützen, wenn Sie Arc-fähige Datencontroller mithilfe des Azure-Portals bereitstellen.
- Optional können Sie Ihre Protokolle im direkt verbundenen Modus automatisch in den Azure Log Analytics-Arbeitsbereich hochladen.

####    <a name="azure-arc-enabled-postgresql-hyperscale"></a>PostgreSQL Hyperscale mit Azure Arc-Aktivierung

Dieses Release führt folgende Funktionen ein:

- Löschen eines PostgreSQL Hyperscale mit Azure Arc-Unterstützung, dessen Datencontroller für den direkt verbundenen Modus konfiguriert wurde, aus dem Azure-Portal.
- Bereitstellung von PostgreSQL Hyperscale mit Azure Arc-Unterstützung über die Azure Database for Postgres-Bereitstellungsseite im Azure-Portal Weitere Informationen finden Sie unter [Auswählen der Azure Database for PostgreSQL-Bereitstellungsoption](https://ms.portal.azure.com/#create/Microsoft.PostgreSQLServer) – Microsoft Azure.
- Geben Sie Speicherklassen und Postgres-Erweiterungen an, wenn Sie PostgreSQL Hyperscale mit Azure Arc-Unterstützung über das Azure-Portal bereitstellen.
- Reduzieren Sie die Anzahl der Workerknoten in Ihrem PostgreSQL Hyperscale mit Azure Arc-Unterstützung. Sie können diesen Vorgang über die `azdata`-Befehlszeile ausführen. Er wird als Abskalieren bezeichnet, im Gegensatz zum Aufskalieren, bei dem die Anzahl der Workerknoten erhöht wird.

#### <a name="azure-arc-enabled-sql-managed-instance"></a>Azure Arc-fähige SQL Managed Instance-Instanz

- Die neue [Azure CLI-Erweiterung](/cli/azure/azure-cli-extensions-overview) für Arc-fähiges SQL Managed Instance verfügt über die gleichen Befehle wie `azdata arc sql mi <command>`. Alle Befehle für Arc-fähiges SQL Managed Instance befinden sich unter `az sql mi-arc`. Alle Arc-bezogenen `azdata`-Befehle werden als veraltet gekennzeichnet und in einem zukünftigen Release in die Azure CLI integriert.

   Hinzufügen der Erweiterung:
  
   ```azurecli
   az extension add --source https://azurearcdatacli.blob.core.windows.net/cli-extensions/arcdata-0.0.1-py2.py3-none-any.whl -y
   az sql mi-arc --help
   ```

- Lösen Sie manuell ein Failover mit Transact-SQL aus. Geben Sie der Reihe nach diese Befehle ein:

   1. Auf der Endpunktverbindung des primären Replikats:
   
      ```sql
       ALTER AVAILABILITY GROUP current SET (ROLE = SECONDARY);
      ```

   1. Auf der Endpunktverbindung des sekundären Replikats:
   
      ```sql
      ALTER AVAILABILITY GROUP current SET (ROLE = PRIMARY);
      ```
    
- Der Transact-SQL-Befehl `BACKUP` wird blockiert, sofern nicht die `COPY_ONLY`-Einstellung verwendet wird. Dies unterstützt die Point-in-Time-Wiederherstellung.

### <a name="known-issues"></a>Bekannte Probleme

#### <a name="platform"></a>Plattform

- Sie können einen Datencontroller, eine verwaltete SQL-Instanz oder eine PostgreSQL Hyperscale-Servergruppe in einem verbundenen Cluster mit dem Azure-Portal erstellen. Die Bereitstellung mit anderen Azure Arc-fähigen Datendiensttools wird nicht unterstützt. Insbesondere können Sie während dieses Releases keinen Datencontroller im direkten Konnektivitätsmodus mit einem der folgenden Tools bereitstellen.
   - Azure Data Studio
   - Azure Data CLI (`azdata`)
   - Native Kubernetes-Tools (`kubectl`)

   In [Bereitstellen eines Azure Arc-Datencontrollers | Direkter Konnektivitätsmodus](deploy-data-controller-direct-mode.md) wird erläutert, wie Sie den Datencontroller im Portal erstellen. 

- Sie können weiterhin `kubectl` verwenden, um Ressourcen direkt in einem Kubernetes-Cluster zu erstellen. Diese werden jedoch nicht im Azure-Portal widergespiegelt.

- Im direkten Konnektivitätsmodus ist das Hochladen von Verbrauch, Metriken und Protokollen mit `azdata arc dc upload` derzeit blockiert. Der Verbrauch wird automatisch hochgeladen. Der Upload für einen Datencontroller, der im indirekten Konnektivitätsmodus erstellt wurde, sollte weiterhin funktionieren.
- Der automatische Upload von Verbrauchsdaten im direkten Konnektivitätsmodus schlägt fehl, wenn ein Proxy über `–proxy-cert <path-t-cert-file>` verwendet wird.
- Azure Arc-aktivierte SQL Managed-Instanz und Azure Arc-aktivierte PostgreSQL Hyperscale sind nicht GB18030-zertifiziert.
- Derzeit wird nur ein Azure Arc-Datencontroller im direkten Konnektivitätsmodus pro Kubernetes-Cluster unterstützt.

#### <a name="azure-arc-enabled-postgresql-hyperscale"></a>PostgreSQL Hyperscale mit Azure Arc-Aktivierung

- Die Point-in-Time-Wiederherstellung wird für NFS-Speicher vorerst nicht unterstützt.
- Es ist nicht möglich, die `pg_cron`-Erweiterung gleichzeitig zu aktivieren und zu konfigurieren. Hierfür müssen Sie zwei Befehle verwenden. Einen Befehl zum Aktivieren und einen Befehl zum Konfigurieren. Beispiel:

   1. Aktivieren der Erweiterung:
   
      ```console
      azdata arc postgres server edit -n myservergroup --extensions pg_cron 
      ```

   1. Neustarten des Servers.

   1. Konfigurieren der Erweiterung:
   
      ```console
      azdata arc postgres server edit -n myservergroup --engine-settings cron.database_name='postgres'
      ```

   Wenn Sie den zweiten Befehl ausführen, bevor der Neustart abgeschlossen wurde, tritt ein Fehler auf. In diesem Fall warten Sie einfach einige weitere Augenblicke, und führen Sie den zweiten Befehl erneut aus.

- Durch das Übergeben eines ungültigen Werts an den Parameter `--extensions` beim Bearbeiten der Konfiguration einer Servergruppe, um zusätzliche Erweiterungen zu aktivieren, wird die Liste der aktivierten Erweiterungen fälschlicherweise auf die Liste zurückgesetzt, die zum Zeitpunkt der Erstellung der Servergruppe bestanden hat, und verhindert, dass Benutzer zusätzliche Erweiterungen erstellen können. Die einzige verfügbare Problemumgehung besteht in diesem Fall im Löschen und erneuten Bereitstellen der Servergruppe.

## <a name="april-2021"></a>April 2021

Diese Vorschauversion wird am 29. April 2021 veröffentlicht.

### <a name="whats-new"></a>Neuigkeiten

In diesem Abschnitt werden die neuen Features beschrieben, die für dieses Release eingeführt oder aktiviert wurden. 

#### <a name="platform"></a>Plattform

- Direkt verbundene Cluster laden Telemetrieinformationen automatisch in Azure hoch. 

####    <a name="azure-arc-enabled-postgresql-hyperscale"></a>PostgreSQL Hyperscale mit Azure Arc-Aktivierung

- PostgreSQL Hyperscale mit Azure Arc-Unterstützung wird jetzt im Direct Connect-Modus unterstützt. Sie können jetzt PostgreSQL Hyperscale mit Azure Arc-Unterstützung über den Azure Market Place im Azure-Portal bereitstellen. 
- PostgreSQL Hyperscale mit Azure Arc-Unterstützung wird mit der Citus 10.0-Erweiterung ausgeliefert, die spaltenbasierte Tabellenspeicher bietet.
- PostgreSQL Hyperscale mit Azure Arc-Unterstützung unterstützt jetzt die vollständige Benutzer-/Rollenverwaltung.
- PostgreSQL Hyperscale mit Azure Arc-Unterstützung unterstützt jetzt zusätzliche Erweiterungen mit `Tdigest` und `pg_partman`.
- PostgreSQL Hyperscale mit Azure Arc-Unterstützung unterstützt jetzt die Konfiguration von Einstellungen für virtuelle Kerne und Arbeitsspeicher pro Rolle der PostgreSQL-Instanz in der Servergruppe.
- PostgreSQL Hyperscale mit Azure Arc-Unterstützung unterstützt jetzt die Konfiguration von Datenbank-Engine-/Servereinstellungen pro Rolle der PostgreSQL-Instanz in der Servergruppe.

#### <a name="azure-arc-enabled-sql-managed-instance"></a>Azure Arc-fähige SQL Managed Instance-Instanz

- Stellen Sie eine Datenbank auf SQL Managed Instance mit drei Replikaten wieder her, und sie wird automatisch der Verfügbarkeitsgruppe hinzugefügt. 
- Stellen Sie eine Verbindung mit einem sekundären schreibgeschützten Endpunkt auf SQL Managed Instance-Instanzen her, die mit drei Replikaten bereitgestellt wurden. Verwenden Sie `azdata arc sql endpoint list`, um den sekundären schreibgeschützten Verbindungsendpunkt anzuzeigen.

## <a name="march-2021"></a>März 2021

Das Release vom März 2021 wurde ursprünglich am 5. April 2021 eingeführt, und die letzten Releasephasen wurden am 9. April 2021 abgeschlossen.

Versionsnummer der Azure Data CLI (`azdata`): 20.3.2. Sie können `azdata` über [Installieren der Azure Data CLI (`azdata`)](/sql/azdata/install/deploy-install-azdata) installieren.

### <a name="data-controller"></a>Datencontroller

- Stellen Sie den Datencontroller der Azure Arc-fähige Datendienste im direkten Konnektivitätsmodus über das Portal bereit. Beginnen Sie mit [Bereitstellen des Datencontrollers: Direkter Konnektivitätsmodus – Voraussetzungen](deploy-data-controller-direct-mode-prerequisites.md).

### <a name="azure-arc-enabled-postgresql-hyperscale"></a>PostgreSQL Hyperscale mit Azure Arc-Aktivierung

Beide benutzerdefinierten Ressourcendefinitionen (Custom Resource Definitions, CRD) für PostgreSQL wurden in einer einzigen CRD konsolidiert. Siehe hierzu die folgende Tabelle.

|Release |CRD |
|-----|-----|
|Februar 2021 und früher| postgresql-11s.arcdata.microsoft.com<br/>postgresql-12s.arcdata.microsoft.com |
|Ab März 2021 | postgresqls.arcdata.microsoft.com

Beim Bereinigen früherer Installationen löschen Sie die vorherigen CRDs. Siehe [Bereinigen früherer Installationen](create-data-controller-using-kubernetes-native-tools.md#cleanup-from-past-installations).

### <a name="azure-arc-enabled-sql-managed-instance"></a>Azure Arc-fähige SQL Managed Instance-Instanz

- Sie können jetzt eine SQL Managed Instance-Instanz über das Azure-Portal im direkten Konnektivitätsmodus erstellen.

- Sie können jetzt eine Datenbank auf SQL Managed Instance mit drei Replikaten wiederherstellen, und sie wird automatisch der Verfügbarkeitsgruppe hinzugefügt. 

- Sie können jetzt eine Verbindung mit einem sekundären schreibgeschützten Endpunkt auf SQL Managed Instance-Instanzen herstellen, die mit drei Replikaten bereitgestellt wurden. Verwenden Sie `azdata arc sql endpoint list`, um den sekundären schreibgeschützten Verbindungsendpunkt anzuzeigen.

## <a name="february-2021"></a>Februar 2021

### <a name="new-capabilities-and-features"></a>Neue Funktionen und Features

Versionsnummer der Azure Data CLI (`azdata`): 20.3.1. Sie können `azdata` über [Installieren der Azure Data CLI (`azdata`)](/sql/azdata/install/deploy-install-azdata) installieren.

Zusätzliche Updates umfassen:

- Azure Arc-fähige SQL Managed Instance-Instanz
   - Hochverfügbarkeit mit Always On-Verfügbarkeitsgruppen

- Azure Data Studio für PostgreSQL Hyperscale mit Azure Arc-Unterstützung: 
   - Auf der Übersichtsseite wird der Status der Servergruppe angezeigt (aufgeschlüsselt nach Knoten).
   - Eine neue Eigenschaftenseite zeigt weitere Details zur Servergruppe.
   - Parameter der Postgres-Engine können auf der Seite für **Knotenparameter** konfiguriert werden.

## <a name="january-2021"></a>Januar 2021

### <a name="new-capabilities-and-features"></a>Neue Funktionen und Features

Versionsnummer der Azure Data CLI (`azdata`): 20.3.0. Sie können `azdata` über [Installieren der Azure Data CLI (`azdata`)](/sql/azdata/install/deploy-install-azdata) installieren.

Zusätzliche Updates umfassen:
- Lokalisiertes Portal für 17 neue Sprachen verfügbar.
- Kleinere Änderungen an Kube-nativen YAML-Dateien.
- Neue Versionen von Grafana und Kibana.
- Probleme mit Python-Umgebungen bei der Verwendung von azdata in Notebooks in Azure Data Studio gelöst.
- Die pg_audit-Erweiterung ist jetzt für die PostgreSQL Hyperscale verfügbar.
- Eine Sicherungs-ID ist bei einer vollständigen Wiederherstellung einer PostgreSQL Hyperscale-Datenbank nicht mehr erforderlich.
- Der Status (Integritätszustand) wird für jede der PostgreSQL-Instanzen gemeldet, die eine Servergruppe bilden.

   In früheren Versionen wurde der Status auf Servergruppenebene aggregiert und nicht auf PostgreSQL-Knotenebene aufgeschlüsselt.

- PostgreSQL-Bereitstellungen berücksichtigen die in create-Befehlen angegebenen Volumengrößenparameter.
- Die Engineversionsparameter werden beim Bearbeiten einer Servergruppe nun berücksichtigt.
- Die Namenskonvention der Pods für PostgreSQL Hyperscale mit Azure Arc-Unterstützung hat sich geändert:

    Sie hat nun folgende Form: `ServergroupName{c, w}-n`. Beispielsweise wird eine Servergruppe mit drei Knoten, einem Koordinatorknoten und zwei Workerknoten wie folgt dargestellt:
   - `Postgres01c-0` (Koordinatorknoten)
   - `Postgres01w-0` (Workerknoten)
   - `Postgres01w-1` (Workerknoten)

## <a name="december-2020"></a>Dezember 2020

### <a name="new-capabilities--features"></a>Neue Funktionen und Features

Versionsnummer der Azure Data CLI (`azdata`): 20.2.5. Sie können `azdata` über [Installieren der Azure Data CLI (`azdata`)](/sql/azdata/install/deploy-install-azdata) installieren.

Anzeigen von Endpunkten für SQL Managed Instance und PostgreSQL Hyperscale mithilfe der Azure Data-CLI (`azdata`) mit den Befehlen `azdata arc sql endpoint list` -und `azdata arc postgres endpoint list`.

Bearbeiten von Anforderungen und Grenzwerten für SQL Managed Instance-Ressourcen (CPU-Kern und Arbeitsspeicher) mithilfe von Azure Data Studio.

PostgreSQL Hyperscale mit Azure Arc-Unterstützung unterstützt jetzt zusätzlich zur vollständigen Wiederherstellung von Sicherungen die Point-in-Time-Wiederherstellung für die Versionen 11 und 12 von PostgreSQL. Mithilfe der Point-in-Time-Wiederherstellungsfunktion können Sie ein bestimmtes Datum und eine bestimmte Uhrzeit angeben, auf die wiederhergestellt werden soll.

Die Namenskonvention der Pods für PostgreSQL Hyperscale mit Azure Arc-Unterstützung hat sich geändert. Sie hat nun folgende Form: Servergroupname{r, s}-_n_. Beispielsweise wird eine Servergruppe mit drei Knoten, einem Koordinatorknoten und zwei Workerknoten wie folgt dargestellt:
- `postgres02r-0` (Koordinatorknoten)
- `postgres02s-0` (Workerknoten)
- `postgres02s-1` (Workerknoten)

### <a name="breaking-change"></a>Unterbrechende Änderung

#### <a name="new-resource-provider"></a>Neuer Ressourcenanbieter

Mit diesem Release wird ein aktualisierter [Ressourcenanbieter](../../azure-resource-manager/management/azure-services-resource-providers.md) mit dem Namen `Microsoft.AzureArcData` eingeführt. Bevor Sie diese Funktion verwenden können, müssen Sie diesen Ressourcenanbieter registrieren. 

So registrieren Sie diesen Ressourcenanbieter 

1. Wählen Sie im Azure-Portal **Abonnements** aus. 
2. Auswählen Ihres Abonnements
3. Klicken Sie unter **Einstellungen** auf **Ressourcenanbieter**. 
4. Suchen Sie nach `Microsoft.AzureArcData`, und klicken Sie auf **Registrieren**. 

Detaillierte Schritte finden Sie unter [Azure-Ressourcenanbieter und -typen](../../azure-resource-manager/management/resource-providers-and-types.md). Diese Änderung entfernt auch alle vorhandenen Azure-Ressourcen, die Sie in das Azure-Portal hochgeladen haben. Um den Ressourcenanbieter verwenden zu können, müssen Sie den Datencontroller aktualisieren und die neueste `azdata`-CLI verwenden.  

### <a name="platform-release-notes"></a>Versionshinweise zur Plattform

#### <a name="direct-connectivity-mode"></a>Direkter Konnektivitätsmodus

In dieser Version wird der direkte Konnektivitätsmodus eingeführt. Der Modus für die direkte Konnektivität ermöglicht dem Datencontroller das automatische Hochladen der Nutzungsinformationen in Azure. Im Rahmen des Nutzungsuploads wird die Arc-Datencontrollerressource automatisch im Portal erstellt, wenn Sie nicht bereits per `azdata`-Upload erstellt wurde.  

Sie können die direkte Konnektivität angeben, wenn Sie den Datencontroller erstellen. Im folgenden Beispiel wird ein Datencontroller namens `arc` mithilfe von `azdata arc dc create` im direkten Konnektivitätsmodus (`connectivity-mode direct`) erstellt. Bevor Sie das Beispiel ausführen, ersetzen Sie `<subscription id>` durch Ihre Abonnement-ID.

```console
azdata arc dc create --profile-name azure-arc-aks-hci --namespace arc --name arc --subscription <subscription id> --resource-group my-resource-group --location eastus --connectivity-mode direct
```

## <a name="october-2020"></a>Oktober 2020 

Versionsnummer der Azure Data CLI (`azdata`): 20.2.3. Sie können `azdata` über [Installieren der Azure Data CLI (`azdata`)](/sql/azdata/install/deploy-install-azdata) installieren.

### <a name="breaking-changes"></a>Aktuelle Änderungen

Dieses Release enthält folgende Breaking Changes: 

* In der benutzerdefinierten PostgreSQL-Ressourcendefinition (Custom Resource Definition, CRD) wird der Begriff `shards` in `workers` umbenannt. Dieser Begriff (`workers`) entspricht dem Namen des Befehlszeilenparameters.

* Von `azdata arc postgres server delete` wird vor dem Löschen einer Postgres-Instanz eine Bestätigung angefordert.  Verwenden Sie `--force`, um die Aufforderung zu überspringen.

### <a name="additional-changes"></a>Weitere Änderungen

* `azdata arc postgres server create` wurde ein neuer optionaler Parameter namens `--volume-claim mounts` hinzugefügt. Der Wert ist eine durch Trennzeichen getrennte Liste mit Volumeanspruchseinbindungen. Bei einer Volumeanspruchseinbindung handelt es sich um ein Paar aus Volumetyp und PVC-Name. Derzeit wird ausschließlich der Volumetyp `backup` unterstützt.  Wenn der Volumetyp in PostgreSQL `backup` lautet, wird das PVC in `/mnt/db-backups` eingebunden.  Dies ermöglicht die gemeinsame Nutzung von Sicherungen zwischen PostgresSQL-Instanzen, sodass die Sicherung einer PostgresSQL-Instanz in einer anderen Instanz wiederhergestellt werden kann.

* Neue Kurznamen für benutzerdefinierte PostgresSQL-Ressourcendefinitionen: 
  * `pg11` 
  * `pg12`
* Telemetrieupload bietet dem Benutzer Folgendes:
   * Anzahl der in Azure hochgeladenenen Punkte oder 
   * Aufforderung zum erneuten Versuch, falls keine Daten in Azure geladen wurden.
* `azdata arc dc debug copy-logs` liest nun auch aus dem Ordner `/var/opt/controller/log` und sammelt PostgreSQL-Engineprotokolle unter Linux.
*   Anzeige eines Funktionsindikators beim Erstellen und Wiederherstellen einer Sicherung mit PostgreSQL Hyperscale.
* `azdata arc postrgres backup list` enthält jetzt Informationen zur Sicherungsgröße.
* Die Administratornamenseigenschaft von SQL Managed Instance wurde der rechten Spalte des Übersichtsblatts im Azure-Portal hinzugefügt.
* Azure Data Studio unterstützt das Konfigurieren der Anzahl von Workerknoten sowie von virtuellen Kernen und Arbeitsspeichereinstellungen für PostgreSQL Hyperscale. 
* In der Vorschauversion werden Sicherungen/Wiederherstellungen für die Postgres-Versionen 11 und 12 unterstützt.

## <a name="september-2020"></a>September 2020

Azure Arc-fähige Datendienste sind für die öffentliche Vorschauversion freigegeben. Mit Arc-fähigen Datendiensten können Sie Datendienste an jedem Ort verwalten.

- Verwaltete SQL-Instanz
- PostgreSQL Hyperscale

Anweisungen finden Sie unter [Was sind Azure Arc-fähige Datendienste?](overview.md)

## <a name="next-steps"></a>Nächste Schritte

> **Möchten Sie es selbst ausprobieren?**  
> Mit dem [Azure Arc-Schnelleinstieg](https://azurearcjumpstart.io/azure_arc_jumpstart/azure_arc_data/) für AKS, AWS Elastic Kubernetes Service (EKS), Google Cloud Kubernetes Engine (GKE) oder auf einer Azure-VM können Sie schnell die ersten Schritte ausführen.

- [Installieren der Clienttools](install-client-tools.md)
- [Erstellen des Azure Arc-Datencontrollers](create-data-controller.md) (erfordert zuvor die Installation der Clienttools)
- [Erstellen einer Azure SQL Managed Instance-Instanz in Azure Arc](create-sql-managed-instance.md) (erfordert zuvor die Erstellung eines Azure Arc-Datencontrollers)
- [Erstellen einer Azure Database for PostgreSQL Hyperscale-Servergruppe in Azure Arc](create-postgresql-hyperscale-server-group.md) (erfordert zuvor die Erstellung eines Azure Arc-Datencontrollers)
- [Ressourcenanbieter für Azure-Dienste](../../azure-resource-manager/management/azure-services-resource-providers.md)
