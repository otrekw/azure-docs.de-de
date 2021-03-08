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
ms.openlocfilehash: d4667e8fa3a5624dddc3cb0dd792fc73ea812332
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101692888"
---
# <a name="known-issues---azure-arc-enabled-data-services-preview"></a>Bekannte Probleme: Azure Arc-fähige Datendienste (Vorschauversion)

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="february-2021"></a>Februar 2021


- Modus für verbundene Cluster ist deaktiviert
- PostgreSQL Hyperscale mit Azure Arc-Unterstützung gibt eine ungenaue Fehlermeldung zurück, wenn es nicht auf den von Ihnen angegebenen relativen Zeitpunkt wiederherstellen kann. Wenn Sie beispielsweise einen Zeitpunkt für die Wiederherstellung angegeben haben, der weiter zurückliegt als das, was Ihre Sicherungen enthalten, ist die Wiederherstellung nicht erfolgreich, und es wird eine Fehlermeldung wie die folgende angezeigt: `ERROR: (404). Reason: Not found. HTTP response body: {"code":404, "internalStatus":"NOT_FOUND", "reason":"Failed to restore backup for server...}`. Wenn dies eintritt, starten Sie den Befehl neu, nachdem Sie einen Zeitpunkt angegeben haben, der innerhalb des Datumsbereichs liegt, für den Sie Sicherungen besitzen. Listen Sie zum Bestimmen dieses Bereichs Ihre Sicherungen auf, und sehen Sie sich jeweils das Erstellungsdatum an.
- Bei Durchführung einer vollständigen Wiederherstellung ist eine Backup-ID erforderlich. Wenn Sie keine Sicherungs-ID angeben, wird standardmäßig die letzte Sicherung verwendet. Dies funktioniert in dieser Version nicht.

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

### <a name="postgresql"></a>PostgreSQL

- PostgreSQL Hyperscale mit Azure Arc-Unterstützung gibt eine ungenaue Fehlermeldung zurück, wenn es nicht auf den von Ihnen angegebenen relativen Zeitpunkt wiederherstellen kann. Wenn Sie beispielsweise einen Zeitpunkt für die Wiederherstellung angegeben haben, der weiter zurückliegt als das, was Ihre Sicherungen enthalten, ist die Wiederherstellung nicht erfolgreich, und es wird eine Fehlermeldung wie die folgende angezeigt: `ERROR: (404). Reason: Not found. HTTP response body: {"code":404, "internalStatus":"NOT_FOUND", "reason":"Failed to restore backup for server...}`.
Wenn dies eintritt, starten Sie den Befehl neu, nachdem Sie einen Zeitpunkt angegeben haben, der innerhalb des Datumsbereichs liegt, für den Sie Sicherungen besitzen. Sie bestimmen diesen Bereich, indem Sie Ihre Sicherungen auflisten und die Datumsangaben prüfen, an denen sie erstellt wurden.
- Point-in-Time-Wiederherstellung wird nur zwischen Servergruppen unterstützt. Der Zielserver eines Point-in-Time-Wiederherstellungsvorgangs kann nicht der Server sein, von dem Sie die Sicherung erstellt haben. Er muss in einer anderen Servergruppe sein. Eine vollständige Wiederherstellung wird jedoch in derselben Servergruppe unterstützt.
- Bei Durchführung einer vollständigen Wiederherstellung ist eine Backup-ID erforderlich. Wenn Sie keine Sicherungs-ID angeben, wird standardmäßig die letzte Sicherung verwendet. Dies funktioniert in dieser Version nicht.

## <a name="next-steps"></a>Nächste Schritte

> **Möchten Sie es selbst ausprobieren?**  
> Mit dem [Azure Arc-Schnelleinstieg](https://azurearcjumpstart.io/azure_arc_jumpstart/azure_arc_data/) für AKS, AWS Elastic Kubernetes Service (EKS), Google Cloud Kubernetes Engine (GKE) oder auf einer Azure-VM können Sie schnell die ersten Schritte ausführen.

- [Installieren der Clienttools](install-client-tools.md)
- [Erstellen des Azure Arc-Datencontrollers](create-data-controller.md) (erfordert zuvor die Installation der Clienttools)
- [Erstellen einer Azure SQL Managed Instance-Instanz in Azure Arc](create-sql-managed-instance.md) (erfordert zuvor die Erstellung eines Azure Arc-Datencontrollers)
- [Erstellen einer Azure Database for PostgreSQL Hyperscale-Servergruppe in Azure Arc](create-postgresql-hyperscale-server-group.md) (erfordert zuvor die Erstellung eines Azure Arc-Datencontrollers)
- [Ressourcenanbieter für Azure-Dienste](../../azure-resource-manager/management/azure-services-resource-providers.md)
- [Versionshinweise – Azure Arc-fähige Datendienste (Vorschauversion)](release-notes.md)
