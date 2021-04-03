---
title: Azure Data Studio-Dashboards
description: Azure Data Studio-Dashboards
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: a96be6d4da3d292b2e9881652aad28f318ccee8a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "92107571"
---
# <a name="azure-data-studio-dashboards"></a>Azure Data Studio-Dashboards

[Azure Data Studio](/sql/azure-data-studio/what-is) bietet eine ähnliche Darstellung wie das Azure-Portal für Informationen zu Ihren Azure Arc-Ressourcen.  Bei diesen Ansichten handelt es sich um sogenannte **Dashboards**, deren Layout und Optionen denjenigen ähneln, die im Azure-Portal für bestimmte Ressourcen angezeigt werden. In Dashboards sind diese Informationen jedoch auch dann lokal in Ihrer Umgebung verfügbar, wenn keine Verbindung mit Azure besteht.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="connecting-to-a-data-controller"></a>Herstellen einer Verbindung mit einem Datencontroller

### <a name="prerequisites"></a>Voraussetzungen

- Herunterladen von [Azure Data Studio](/sql/azure-data-studio/download-azure-data-studio)
- Installation der Azure Arc-Erweiterung

### <a name="determine-the-data-controller-server-api-endpoint-url"></a>Ermitteln der Endpunkt-URL der Datencontroller-Server-API

Zunächst müssen Sie Azure Data Studio mit der Endpunkt-URL Ihrer Datencontroller-Dienst-API verbinden.

Führen Sie folgenden Befehl aus, um den Endpunkt abzurufen:

```console
kubectl get svc/controller-svc-external -n <namespace name>

#Example:
kubectl get svc/controller-svc-external -n arc
```

Die Ausgabe sieht in etwa wie folgt aus:

```console
NAME                      TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)                                       AGE
controller-svc-external   LoadBalancer   10.0.175.137   52.154.152.24    30080:32192/TCP                               22h
```

Wenn Sie einen LoadBalancer-Typ verwenden, sollten Sie die externe IP-Adresse und Portnummer kopieren. Wenn Sie NodePort verwenden, sollten Sie die IP-Adresse Ihres Kubernetes-API-Servers und die in der Spalte „PORT(S)“ aufgeführte Portnummer verwenden.

Anschließend sollten Sie eine URL für Ihren Endpunkt erstellen, indem Sie diese Informationen wie folgt kombinieren:

```console
https://<ip address>:<port>

Example:
https://52.154.152.24:30080
```

Notieren Sie sich Ihre IP-Adresse, da Sie sie im nächsten Schritt verwenden werden.

### <a name="connect"></a>Verbinden

1. Öffnen Sie Azure Data Studio.

1. Klicken Sie links auf die Registerkarte **Verbindungen**.

Erweitern Sie unten den Bereich **Azure Arc Controllers** (Azure Arc-Controller).

Klicken Sie auf das Plussymbol, um eine neue Datencontrollerverbindung hinzuzufügen.

Geben Sie oben im Bildschirm in der Befehlspalette die URL ein, die Sie in Schritt 1 erstellt haben, und drücken Sie die EINGABETASTE.
Geben Sie den Benutzernamen für den Datencontroller ein.  Dies ist der Benutzername, den Sie während der Bereitstellung des Datencontrollers übergeben haben.  Drücken Sie auf die EINGABETASTE.
Geben Sie das Kennwort für den Datencontroller ein.  Dies ist das Kennwort, den Sie während der Bereitstellung des Datencontrollers übergeben haben. Drücken Sie auf die EINGABETASTE.

Nachdem Sie nun eine Verbindung mit einem Datencontroller hergestellt haben, können Sie die Dashboards für den Datencontroller und alle verwalteten SQL Server-Instanzen oder PostgreSQL Hyperscale-Servergruppenressourcen anzeigen.

## <a name="view-the-data-controller-dashboard"></a>Anzeigen des Datencontrollerdashboards

Klicken Sie mit der rechten Maustaste auf den Datencontroller im Bereich „Verbindungen“ im erweiterbaren Bereich **Arc Controllers** (Arc-Controller), und klicken Sie auf **Verwalten**.

Hier finden Sie Details zur Datencontrollerressource wie den Namen, die Region, den Verbindungsmodus, die Ressourcengruppe, das Abonnement, den Controllerendpunkt und den Namespace.  Außerdem finden Sie dort eine Liste aller verwalteten Datenbankressourcen, die vom Datencontroller verwaltet werden.

Sie werden bemerken, dass das Layout an das des Azure-Portals erinnert.

Sie können die Erstellung einer verwalteten SQL-Instanz oder einer PostgreSQL Hyperscale-Servergruppe ganz unkompliziert starten, indem Sie auf die Schaltfläche „+ Neue Instanz“ klicken.

Sie können das Azure-Portal auch im Kontext dieses Datencontrollers öffnen, indem Sie auf die Schaltfläche „Im Azure-Portal öffnen“ klicken.

## <a name="view-the-sql-managed-instance-dashboards"></a>Anzeigen der Dashboards für verwaltete SQL-Instanzen

Wenn Sie einige SQL Managed Instance-Instanzen erstellt haben, werden diese im Bereich „Verbindungen“ im erweiterbaren Bereich „Azure Data Controllers“ (Azure-Datencontroller) unterhalb des Datencontrollers angezeigt, von dem sie verwaltet werden.

Klicken Sie mit der rechten Maustaste auf die Instanz, und klicken Sie dann auf „Verwalten“, um das Dashboard für eine bestimmte verwaltete SQL-Instanz anzuzeigen.

Der Bereich „Verbindungen“ wird auf der rechten Seite angezeigt, und Sie werden aufgefordert, Ihren Benutzernamen und das Kennwort einzugeben, um eine Verbindung zu dieser SQL-Instanz herzustellen. Wenn Sie die Verbindungsinformationen kennen, können Sie sie eingeben und auf „Verbinden“ klicken.  Andernfalls können Sie auf „Abbrechen“ klicken.  In beiden Fällen gelangen Sie zum Dashboard, wenn der Bereich „Verbindungen“ geschlossen wird.

Auf der Registerkarte „Übersicht“ können Sie Details über die verwaltete SQL-Instanz anzeigen, z. B. ihre Ressourcengruppe, den Datencontroller, die Abonnement-ID, den Status, die Region und mehr.  Dort ist auch ein Link aufgeführt, über den Sie im Kontext dieser verwalteten SQL-Instanz zu den Grafana- oder Kibana-Dashboards wechseln können.

Wenn Sie eine Verbindung zu der SQL Server-Instanz herstellen können, finden Sie hier weitere Informationen.

Sie können hier die verwaltete SQL-Instanz löschen oder das Azure-Portal öffnen, um die verwaltete SQL-Instanz im Azure-Portal anzuzeigen.

Wenn Sie links auf die Registerkarte „Verbindungszeichenfolge“ klicken, wird eine Liste der vorab erstellten Verbindungszeichenfolgen für diese verwaltete SQL-Instanz angezeigt. Sie erleichtert Ihnen das Kopieren/Einfügen in viele weitere Anwendungen oder Codeabschnitte.

## <a name="view-the-postgresql-hyperscale-server-group-dashboards"></a>Anzeigen der Dashboards für die PostgreSQL Hyperscale-Servergruppe

Wenn Sie einige PostgreSQL Hyperscale-Servergruppen erstellt haben, werden diese im Bereich „Verbindungen“ im erweiterbaren Bereich „Azure Data Controllers“ (Azure-Datencontroller) unterhalb des Datencontrollers angezeigt, von dem sie verwaltet werden.

Klicken Sie mit der rechten Maustaste auf die PostgreSQL Hyperscale-Servergruppe, und klicken Sie auf „Verwalten“, um das Dashboard für eine bestimmte PostgreSQL Hyperscale-Servergruppe anzuzeigen.

Auf der Registerkarte „Übersicht“ können Sie Details über die Servergruppe anzeigen, z. B. ihre Ressourcengruppe, den Datencontroller, die Abonnement-ID, den Status, die Region und mehr.  Dort ist auch ein Link aufgeführt, über den Sie im Kontext dieser Servergruppe zu den Grafana- oder Kibana-Dashboards wechseln können.

Sie können dort die Servergruppe löschen oder das Azure-Portal öffnen, um die Servergruppe im Azure-Portal anzuzeigen.

Wenn Sie links auf die Registerkarte „Verbindungszeichenfolge“ klicken, wird eine Liste der vorab erstellten Verbindungszeichenfolgen für diese Servergruppe angezeigt. Sie erleichtert Ihnen das Kopieren/Einfügen in viele weitere Anwendungen oder Codeabschnitte.

Wenn Sie links auf die Registerkarte „Eigenschaften“ klicken, werden weitere Details angezeigt.

Wenn Sie links auf die Registerkarte „Ressourcenintegrität“ klicken, wird die aktuelle allgemeine Integrität dieser Servergruppe angezeigt.

Wenn Sie links auf die Registerkarte „Probleme diagnostizieren und beheben“ klicken, können Sie das PostgreSQL-Notebook für die Problembehandlung starten.

Wenn Sie links auf die Registerkarte „Neue Supportanfrage“ klicken, können Sie das Azure-Portal im Kontext der Servergruppe starten und dort eine Azure-Supportanfrage erstellen.