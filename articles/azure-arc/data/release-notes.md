---
title: Azure Arc-fähige Datendienste – Versionshinweise
description: Aktuelle Versionshinweise
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
ms.date: 04/09/2021
ms.topic: conceptual
ms.openlocfilehash: 1fe5974bafddcb4e474ef59a062836e071ab9461
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107304918"
---
# <a name="release-notes---azure-arc-enabled-data-services-preview"></a>Versionshinweise – Azure Arc-fähige Datendienste (Vorschauversion)

In diesem Artikel werden die Funktionen, Features und Verbesserungen hervorgehoben, die kürzlich für Azure Arc-fähige Datendienste veröffentlicht bzw. durchgeführt wurden. 

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="march-2021"></a>März 2021

Das Release vom März 2021 wurde ursprünglich am 5. April 2021 eingeführt, und die letzten Releasephasen wurden am 9. April 2021 abgeschlossen.

Informationen zu den Einschränkungen dieses Releases finden Sie unter [Bekannte Probleme – Azure Arc-fähige Datendienste (Vorschauversion)](known-issues.md).

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

### <a name="known-issues"></a>Bekannte Probleme

- Im direkten Konnektivitätsmodus ist das Hochladen von Verbrauch, Metriken und Protokollen mit `azdata arc dc upload` derzeit blockiert. Der Verbrauch wird automatisch hochgeladen. Der Upload für einen Datencontroller, der im indirekten Konnektivitätsmodus erstellt wurde, sollte weiterhin funktionieren.
- Die Bereitstellung eines Datencontrollers im direkten Modus kann nur über das Azure-Portal erfolgen und ist nicht über Clienttools wie azdata, Azure Data Studio oder kubectl verfügbar.
- Die Bereitstellung einer SQL Managed Instance-Instanz mit Azure Arc-Unterstützung im direkten Modus kann nur über das Azure-Portal erfolgen und ist nicht über Tools wie azdata, Azure Data Studio oder kubectl verfügbar.
- Die Bereitstellung von Azure Arc-fähigem PostgeSQL Hyperscale im direkten Modus ist derzeit nicht verfügbar.
- Der automatische Upload von Verbrauchsdaten im direkten Konnektivitätsmodus schlägt fehl, wenn ein Proxy über `–proxy-cert <path-t-cert-file>` verwendet wird.

## <a name="february-2021"></a>Februar 2021

### <a name="new-capabilities-and-features"></a>Neue Funktionen und Features

Versionsnummer der Azure Data CLI (`azdata`): 20.3.1. Sie können `azdata` über [Installieren der Azure Data CLI (`azdata`)](/sql/azdata/install/deploy-install-azdata) installieren.

Zusätzliche Updates umfassen:

- Azure Arc-fähige SQL Managed Instance-Instanz
   - Hochverfügbarkeit mit Always On-Verfügbarkeitsgruppen

- Azure Data Studio für PostgreSQL Hyperscale mit Azure Arc-Unterstützung: 
   - Auf der Übersichtsseite wird nun der Status der Servergruppe angezeigt (aufgeschlüsselt nach Knoten).
   - Eine neue Eigenschaftenseite mit weiteren Details zur Servergruppe ist verfügbar.
   - Parameter der Postgres-Engine können auf der Seite für **Knotenparameter** konfiguriert werden.

Informationen zu Problemen im Zusammenhang mit diesem Release finden Sie unter [Bekannte Probleme: Azure Arc-fähige Datendienste (Vorschau)](known-issues.md).

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

- PostgreSQL-Bereitstellungen berücksichtigen jetzt die in create-Befehlen angegebenen Volumegrößenparameter.
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

### <a name="known-issues"></a>Bekannte Probleme

- In Azure Kubernetes Service (AKS) wird die Kubernetes-Version 1.19.x nicht unterstützt.
- In Kubernetes 1.19 wird `containerd` nicht unterstützt.
- Die Datencontrollerressource in Azure ist zurzeit eine Azure-Ressource. Alle Updates wie z. B. Löschen werden nicht an den Kubernetes-Cluster zurückgegeben.
- Instanznamen dürfen maximal 13 Zeichen umfassen.
- Für Azure Arc-Datencontroller oder -Datenbankinstanzen sind keine direkten Upgrades verfügbar.
- Containerimages von Arc-fähigen Datendiensten sind nicht signiert.  Möglicherweise müssen Sie die Kubernetes-Knoten so konfigurieren, dass das Pullen nicht signierter Containerimages zulässig ist.  Wenn Sie z. B. Docker als Containerruntime verwenden, können Sie die Umgebungsvariable DOCKER_CONTENT_TRUST=0 festlegen und neu starten.  Andere Containerruntimes bieten ähnliche Optionen beispielsweise in [OpenShift](https://docs.openshift.com/container-platform/4.5/openshift_images/image-configuration.html#images-configuration-file_image-configuration).
- Es können keine Azure Arc-fähigen SQL Managed Instance-Instanzen oder PostgreSQL Hyperscale-Servergruppen über das Azure-Portal erstellt werden.
- Bei Verwendung von NFS muss `allowRunAsRoot` in der Bereitstellungsprofildatei vorerst auf `true` festgelegt werden, bevor der Azure Arc-Datencontroller erstellt wird.
- Es wird nur die SQL- und PostgreSQL-Anmeldeauthentifizierung unterstützt.  Azure Active Directory oder Active Directory werden nicht unterstützt.
- Die Erstellung eines Datencontrollers in OpenShift erfordert gelockerte Sicherheitseinschränkungen.  Einzelheiten dazu finden Sie der Dokumentation.
- Wenn Sie die Azure Kubernetes Service Engine (AKS) in Azure Stack Hub mit Azure Arc-Datencontrollern und -Datenbankinstanzen verwenden, wird kein Upgrade auf eine neuere Kubernetes-Version unterstützt. Deinstallieren Sie den Azure Arc-Datencontroller und alle Datenbankinstanzen, bevor Sie den Kubernetes-Cluster aktualisieren.
- AKS-Cluster, die [mehrere Verfügbarkeitszonen](../../aks/availability-zones.md) umfassen, werden für Datendienste mit Azure Arc-Unterstützung derzeit nicht unterstützt. Löschen Sie zur Vermeidung dieses Problems alle Zonen aus dem Auswahlsteuerelement, wenn Sie den AKS-Cluster im Azure-Portal erstellen und eine Region mit verfügbaren Zonen auswählen. Sehen Sie sich die folgende Abbildung an:

   :::image type="content" source="media/release-notes/aks-zone-selector.png" alt-text="Deaktivieren der Kontrollkästchen für die einzelnen Zonen, um keine Zone anzugeben":::

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
