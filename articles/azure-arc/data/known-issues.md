---
title: 'Azure Arc-fähige Datendienste: Bekannte Probleme'
description: Neueste bekannte Probleme
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
ms.date: 03/02/2021
ms.topic: conceptual
ms.openlocfilehash: ee652047a33d73ece2d7648905fa590d90b1fb2f
ms.sourcegitcommit: d40ffda6ef9463bb75835754cabe84e3da24aab5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2021
ms.locfileid: "107029504"
---
# <a name="known-issues---azure-arc-enabled-data-services-preview"></a>Bekannte Probleme: Azure Arc-fähige Datendienste (Vorschauversion)

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="march-2021"></a>März 2021

### <a name="data-controller"></a>Datencontroller

- Sie können einen Datencontroller im direkten Konnektivitätsmodus über das Azure-Portal erstellen. Die Bereitstellung mit anderen Azure Arc-fähigen Datendiensttools wird nicht unterstützt. Insbesondere können Sie während dieses Releases keinen Datencontroller im direkten Konnektivitätsmodus mit einem der folgenden Tools bereitstellen.
   - Azure Data Studio
   - Azure Data CLI (`azdata`)
   - Native Kubernetes-Tools

   In [Bereitstellen eines Azure Arc-Datencontrollers | Direkter Konnektivitätsmodus](deploy-data-controller-direct-mode.md) wird erläutert, wie Sie den Datencontroller im Portal erstellen. 

### <a name="azure-arc-enabled-postgresql-hyperscale"></a>PostgreSQL Hyperscale mit Azure Arc-Aktivierung

- Die Bereitstellung einer Azure Arc-fähigen Postgres Hyperscale-Servergruppe in einem Arc-Datencontroller, der für den direkten Konnektivitätsmodus aktiviert ist, wird nicht unterstützt.
- Durch das Übergeben eines ungültigen Werts an den Parameter `--extensions` beim Bearbeiten der Konfiguration einer Servergruppe, um zusätzliche Erweiterungen zu aktivieren, wird die Liste der aktivierten Erweiterungen fälschlicherweise auf die Liste zurückgesetzt, die zum Zeitpunkt der Erstellung der Servergruppe bestanden hat, und verhindert, dass Benutzer zusätzliche Erweiterungen erstellen können. Die einzige verfügbare Problemumgehung besteht in diesem Fall im Löschen und erneuten Bereitstellen der Servergruppe.

## <a name="february-2021"></a>Februar 2021

### <a name="data-controller"></a>Datencontroller

- Der direkte Konnektivitätsmodus für Cluster ist deaktiviert.

### <a name="azure-arc-enabled-postgresql-hyperscale"></a>PostgreSQL Hyperscale mit Azure Arc-Aktivierung

- Die Point-in-Time-Wiederherstellung wird für NFS-Speicher vorerst nicht unterstützt.
- Es ist nicht möglich, die „pg_cron“-Erweiterung gleichzeitig zu aktivieren und zu konfigurieren. Hierfür müssen Sie zwei Befehle verwenden. Einen Befehl zum Aktivieren und einen Befehl zum Konfigurieren. 

   Beispiel:
   ```console
   § azdata arc postgres server edit -n myservergroup --extensions pg_cron 
   § azdata arc postgres server edit -n myservergroup --engine-settings cron.database_name='postgres'
   ```

   Der erste Befehl erfordert einen Neustart der Servergruppe. Stellen Sie also vor dem Ausführen des zweiten Befehls sicher, dass der Zustand der Servergruppe von „Aktualisierung“ in „Bereit“ gewechselt ist. Wenn Sie den zweiten Befehl ausführen, bevor der Neustart abgeschlossen wurde, tritt ein Fehler auf. In diesem Fall warten Sie einfach einige weitere Augenblicke, und führen Sie den zweiten Befehl erneut aus.

## <a name="introduced-prior-to-february-2021"></a>Vor Februar 2021

### <a name="data-controller"></a>Datencontroller

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


## <a name="next-steps"></a>Nächste Schritte

> **Möchten Sie es selbst ausprobieren?**  
> Mit dem [Azure Arc-Schnelleinstieg](https://azurearcjumpstart.io/azure_arc_jumpstart/azure_arc_data/) für AKS, AWS Elastic Kubernetes Service (EKS), Google Cloud Kubernetes Engine (GKE) oder auf einer Azure-VM können Sie schnell die ersten Schritte ausführen.

- [Installieren der Clienttools](install-client-tools.md)
- [Erstellen des Azure Arc-Datencontrollers](create-data-controller.md) (erfordert zuvor die Installation der Clienttools)
- [Erstellen einer Azure SQL Managed Instance-Instanz in Azure Arc](create-sql-managed-instance.md) (erfordert zuvor die Erstellung eines Azure Arc-Datencontrollers)
- [Erstellen einer Azure Database for PostgreSQL Hyperscale-Servergruppe in Azure Arc](create-postgresql-hyperscale-server-group.md) (erfordert zuvor die Erstellung eines Azure Arc-Datencontrollers)
- [Ressourcenanbieter für Azure-Dienste](../../azure-resource-manager/management/azure-services-resource-providers.md)
- [Versionshinweise – Azure Arc-fähige Datendienste (Vorschauversion)](release-notes.md)
