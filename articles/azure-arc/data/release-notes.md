---
title: Azure Arc-fähige Datendienste – Versionshinweise
description: Aktuelle Versionshinweise
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
ms.date: 09/22/2020
ms.topic: conceptual
ms.openlocfilehash: d22976254cc804ca53060fb284abde8e80a684e6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91319725"
---
# <a name="release-notes---azure-arc-enabled-data-services-preview"></a>Versionshinweise – Azure Arc-fähige Datendienste (Vorschauversion)

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="september-2020"></a>September 2020

Azure Arc-fähige Datendienste sind für die öffentliche Vorschauversion freigegeben. Mit Arc-fähigen Datendiensten können Sie Datendienste an jedem Ort verwalten.

- Verwaltete SQL-Instanz
- PostgreSQL Hyperscale

Anweisungen finden Sie unter [Was sind Azure Arc-fähige Datendienste?](overview.md)

### <a name="known-issues"></a>Bekannte Probleme

Für diese Version gelten die folgenden Probleme:

* **Löschen einer PostgreSQL Hyperscale-Servergruppe**: Wenn Sie die Konfiguration Ihrer Servergruppe oder -instanz geändert haben, warten Sie, bis der Bearbeitungsvorgang abgeschlossen ist, bevor Sie eine PostgreSQL Hyperscale-Servergruppe löschen.

* **`azdata notebook run` kann fehlschlagen**: Um dieses Problem zu umgehen, führen Sie `azdata notebook run` in einer virtuellen Python-Umgebung aus. Dieses Problem tritt auch bei einem fehlerhaften Versuch auf, eine verwaltete SQL-Instanz oder eine PostgreSQL Hyperscale-Servergruppe mithilfe des Azure Data Studio-Bereitstellungs-Assistenten zu erstellen. In diesem Fall können Sie das Notebook öffnen und oben im Notebook auf die Schaltfläche **Alle ausführen** klicken.

## <a name="next-steps"></a>Nächste Schritte

> **Möchten Sie es selbst ausprobieren?**  
> Mit dem [Azure Arc-Schnelleinstieg](https://github.com/microsoft/azure_arc#azure-arc-enabled-data-services) für Azure Kubernetes Service (AKS), AWS Elastic Kubernetes Service (EKS), Google Cloud Kubernetes Engine (GKE) oder auf einer Azure-VM können Sie schnell die ersten Schritte unternehmen.

[Installieren der Clienttools](install-client-tools.md)

[Erstellen des Azure Arc-Datencontrollers](create-data-controller.md) (erfordert zuvor die Installation der Clienttools)

[Erstellen einer Azure SQL Managed Instance-Instanz in Azure Arc](create-sql-managed-instance.md) (erfordert zuvor die Erstellung eines Azure Arc-Datencontrollers)

[Erstellen einer Azure Database for PostgreSQL Hyperscale-Servergruppe in Azure Arc](create-postgresql-hyperscale-server-group.md) (erfordert zuvor die Erstellung eines Azure Arc-Datencontrollers)

## <a name="known-limitations-and-issues"></a>Bekannte Einschränkungen und Probleme

- Namen von SQL Managed Instance-Instanzen dürfen nicht mehr als 13 Zeichen umfassen.
- Für Azure Arc-Datencontroller oder -Datenbankinstanzen sind keine direkten Upgrades verfügbar.
- Containerimages von Arc-fähigen Datendiensten sind nicht signiert.  Möglicherweise müssen Sie die Kubernetes-Knoten so konfigurieren, dass das Pullen nicht signierter Containerimages zulässig ist.  Wenn Sie z. B. Docker als Containerruntime verwenden, können Sie die Umgebungsvariable DOCKER_CONTENT_TRUST=0 festlegen und neu starten.  Andere Containerruntimes bieten ähnliche Optionen beispielsweise in [OpenShift](https://docs.openshift.com/container-platform/4.5/openshift_images/image-configuration.html#images-configuration-file_image-configuration).
- Es können keine Azure Arc-fähigen SQL Managed Instance-Instanzen oder PostgreSQL Hyperscale-Servergruppen über das Azure-Portal erstellt werden.
- Bei Verwendung von NFS müssen Sie allowRunAsRoot in der Bereitstellungsprofildatei vorerst auf „true“ festlegen, bevor Sie den Azure Arc-Datencontroller erstellen.
- Es wird nur die SQL- und PostgreSQL-Anmeldeauthentifizierung unterstützt.  Azure Active Directory oder Active Directory werden nicht unterstützt.
- Die Erstellung eines Datencontrollers in OpenShift erfordert gelockerte Sicherheitseinschränkungen.  Einzelheiten dazu finden Sie der Dokumentation.
- Das _Herunterskalieren_ der Anzahl von Postgres Hyperscale-Workerknoten wird nicht unterstützt.
- Wenn Sie die Azure Kubernetes Service Engine (AKS-Engine) in Azure Stack Hub mit Azure Arc-Datencontrollern und -Datenbankinstanzen verwenden, wird kein Upgrade auf eine neuere Kubernetes-Version unterstützt. Deinstallieren Sie den Azure Arc-Datencontroller und alle Datenbankinstanzen, bevor Sie den Kubernetes-Cluster aktualisieren.
- Die Vorschau unterstützt keine Sicherung/Wiederherstellung für Version 11 der Postgres-Engine. Es wird nur die Sicherung/Wiederherstellung für die Postgres-Version 12 unterstützt.
