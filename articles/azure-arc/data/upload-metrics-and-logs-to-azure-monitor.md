---
title: Hochladen von Ressourceninventardaten, Nutzungsdaten, Metriken und Protokollen in Azure Monitor
description: Hochladen von Ressourceninventardaten, Nutzungsdaten, Metriken und Protokollen in Azure Monitor
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: ac6ffd2b5bf48079db6a0cd261dbe2535e1821ac
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90931293"
---
# <a name="upload-resource-inventory-usage-data-metrics-and-logs-to-azure-monitor"></a>Hochladen von Ressourceninventardaten, Nutzungsdaten, Metriken und Protokollen in Azure Monitor

Mit den Azure Arc-Datendiensten können Sie *optional* Metriken und Protokolle in Azure Monitor hochladen, damit Sie Metriken und Protokolle aggregieren und analysieren, Warnungen auslösen, Benachrichtigungen senden oder automatisierte Aktionen auslösen können. Wenn Sie Ihre Daten an Azure Monitor senden, können Sie auch Überwachungs- und Protokollierungsdaten außerhalb des Standorts und in großem Umfang für die langfristige Speicherung der Daten für erweiterte Analysen speichern.  Wenn Sie über mehrere Standorte mit Azure Arc-Datendiensten verfügen, können Sie Azure Monitor als zentralen Speicherort verwenden, um alle Ihre Protokolle und Metriken für Ihre Standorte zu erfassen.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="before-you-begin"></a>Voraussetzungen

Zum Aktivieren der Szenarien für das Hochladen von Protokollen und Metriken sind einige einmalige Einrichtungsschritte erforderlich:

1) Erstellen eines Dienstprinzipals/einer Azure Active Directory-Anwendung, einschließlich Erstellen eines geheimen Clientzugriffsschlüssels und Zuweisen des Dienstprinzipals zur Rolle „Herausgeber für Überwachungsmetriken“ für die Abonnements, in denen sich die Datenbankinstanzressourcen befinden.
2) Erstellen eines Log Analytics-Arbeitsbereichs und Abrufen der Schlüssel sowie Festlegen der Informationen in Umgebungsvariablen.

Der erste Schritt ist zum Hochladen von Metriken erforderlich, der zweite zum Hochladen von Protokollen.

Führen Sie diese Befehle aus, um den Dienstprinzipal für das Hochladen von Metriken zu erstellen, und weisen Sie ihn den Rollen „Herausgeber für Überwachungsmetriken“ und „Mitwirkender“ zu, damit der Dienstprinzipal Metriken hochladen und Erstellungs- und Hochladevorgänge ausführen kann.

## <a name="create-service-principal-and-assign-roles"></a>Erstellen des Dienstprinzipals und Zuweisen von Rollen

Führen Sie diese Befehle aus, um den Dienstprinzipal für den Metrikupload zu erstellen, und weisen Sie ihn der Rolle „Herausgeber für Überwachungsmetriken“ zu.

Führen Sie diesen Befehl aus, um einen Dienstprinzipal zu erstellen:

> [!NOTE]
> Zum Erstellen eines Dienstprinzipals sind [bestimmte Berechtigungen in Azure](/active-directory/develop/howto-create-service-principal-portal#required-permissions) erforderlich.

```console
az ad sp create-for-rbac --name <a name you choose>

#Example:
#az ad sp create-for-rbac --name azure-arc-metrics
```

Beispielausgabe:

```console
"appId": "2e72adbf-de57-4c25-b90d-2f73f126e123",
"displayName": "azure-arc-metrics",
"name": "http://azure-arc-metrics",
"password": "5039d676-23f9-416c-9534-3bd6afc78123",
"tenant": "72f988bf-85f1-41af-91ab-2d7cd01ad1234"
```

Speichern Sie die appId- und tenant-Werte in einer Umgebungsvariablen, um sie später zu verwenden:

```console
#PowerShell

$Env:SPN_CLIENT_ID='<the 'appId' value from the output of the 'az ad sp create-for-rbac' command above>'
$Env:SPN_CLIENT_SECRET='<the 'password' value from the output of the 'az ad sp create-for-rbac' command above>'
$Env:SPN_TENANT_ID='<the 'tenant' value from the output of the 'az ad sp create-for-rbac' command above>'

#Linux/macOS

export SPN_CLIENT_ID='<the 'appId' value from the output of the 'az ad sp create-for-rbac' command above>'
export SPN_CLIENT_SECRET='<the 'password' value from the output of the 'az ad sp create-for-rbac' command above>'
export SPN_TENANT_ID='<the 'tenant' value from the output of the 'az ad sp create-for-rbac' command above>'

#Example (using Linux):
export SPN_CLIENT_ID='2e72adbf-de57-4c25-b90d-2f73f126e123'
export SPN_CLIENT_SECRET='5039d676-23f9-416c-9534-3bd6afc78123'
export SPN_TENANT_ID='72f988bf-85f1-41af-91ab-2d7cd01ad1234'
```

Führen Sie diesen Befehl aus, um dem Dienstprinzipal die Rolle „Herausgeber für Überwachungsmetriken“ für das Abonnement zuzuweisen, in dem sich Ihre Datenbankinstanzressourcen befinden:

```console
az role assignment create --assignee <appId value from output above> --role 'Monitoring Metrics Publisher' --scope subscriptions/<sub ID>
az role assignment create --assignee <appId value from output above> --role 'Contributor' --scope subscriptions/<sub ID>

#Example:
#az role assignment create --assignee 2e72adbf-de57-4c25-b90d-2f73f126ede5 --role 'Monitoring Metrics Publisher' --scope subscriptions/182c901a-129a-4f5d-56e4-cc6b29459123
#az role assignment create --assignee 2e72adbf-de57-4c25-b90d-2f73f126ede5 --role 'Contributor' --scope subscriptions/182c901a-129a-4f5d-56e4-cc6b29459123
```

Beispielausgabe:

```console
{
  "canDelegate": null,
  "id": "/subscriptions/182c901a-129a-4f5d-86e4-cc6b29459123/providers/Microsoft.Authorization/roleAssignments/f82b7dc6-17bd-4e78-93a1-3fb733b912d",
  "name": "f82b7dc6-17bd-4e78-93a1-3fb733b9d123",
  "principalId": "5901025f-0353-4e33-aeb1-d814dbc5d123",
  "principalType": "ServicePrincipal",
  "roleDefinitionId": "/subscriptions/182c901a-129a-4f5d-86e4-cc6b29459123/providers/Microsoft.Authorization/roleDefinitions/3913510d-42f4-4e42-8a64-420c39005123",
  "scope": "/subscriptions/182c901a-129a-4f5d-86e4-cc6b29459123",
  "type": "Microsoft.Authorization/roleAssignments"
}
```

## <a name="create-a-log-analytics-workspace"></a>Erstellen eines Log Analytics-Arbeitsbereichs

Führen Sie anschließend diese Befehle aus, um einen Log Analytics-Arbeitsbereich zu erstellen und die Zugriffsinformationen in Umgebungsvariablen festzulegen.

> [!NOTE]
> Überspringen Sie diesen Schritt,wenn Sie bereits über einen Arbeitsbereich verfügen.

```console
az monitor log-analytics workspace create --resource-group <resource group name> --name <some name you choose>

#Example:
#az monitor log-analytics workspace create --resource-group MyResourceGroup --name MyLogsWorkpace
```

Beispielausgabe:

```console
{
  "customerId": "d6abb435-2626-4df1-b887-445fe44a4123",
  "eTag": null,
  "id": "/subscriptions/182c901a-129a-4f5d-86e4-cc6b29459123/resourcegroups/user-arc-demo/providers/microsoft.operationalinsights/workspaces/user-logworkspace",
  "location": "eastus",
  "name": "user-logworkspace",
  "portalUrl": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "user-arc-demo",
  "retentionInDays": 30,
  "sku": {
    "lastSkuUpdate": "Thu, 30 Jul 2020 22:37:53 GMT",
    "maxCapacityReservationLevel": 3000,
    "name": "pergb2018"
  },
  "source": "Azure",
  "tags": null,
  "type": "Microsoft.OperationalInsights/workspaces"
}
```

## <a name="assign-id-and-shared-key-to-environment-variables"></a>Zuweisen von ID und gemeinsam verwendetem Schlüssel zu Umgebungsvariablen

Speichern Sie die customerId (Arbeitsbereichs-ID) als Umgebungsvariable zur späteren Verwendung:

```console
#PowerShell
$Env:WORKSPACE_ID='<the customerId from the 'log-analytics workspace create' command output above>'

#Linux/macOS
export WORKSPACE_ID='<the customerId from the 'log-analytics workspace create' command output above>'

#Example (using Linux)
#export WORKSPACE_ID='d6abb435-2626-4df1-b887-445fe44a4123'
```

Mit diesem Befehl werden die Zugriffsschlüssel ausgegeben, die für die Verbindung mit Ihrem Log Analytics-Arbeitsbereich erforderlich sind:

```console
az monitor log-analytics workspace get-shared-keys --resource-group MyResourceGroup --name MyLogsWorkpace
```

Beispielausgabe:

```console
{
  "primarySharedKey": "JXzQp1RcGgjXFCDS3v0sXoxPvbgCoGaIv35lf11Km2WbdGFvLXqaydpaj1ByWGvKoCghL8hL4BRoypXxkLr123==",
  "secondarySharedKey": "p2XHSxLJ4o9IAqm2zINcEmx0UWU5Z5EZz8PQC0OHpFjdpuVaI0zsPbTv5VyPFgaCUlCZb2yEbkiR4eTuTSF123=="
}
```

Speichern Sie den Primärschlüssel zur späteren Verwendung in einer Umgebungsvariablen:

```console
#PowerShell:
$Env:WORKSPACE_SHARED_KEY='<the primarySharedKey value from the 'get-shared-keys' command above'

#Linux/macOS:
export WORKSPACE_SHARED_KEY='<the primarySharedKey value from the 'get-shared-keys' command above'

#Example (using Linux):
export WORKSPACE_SHARED_KEY='JXzQp1RcGgjXFCDS3v0sXoxPvbgCoGaIv35lf11Km2WbdGFvLXqaydpaj1ByWGvKoCghL8hL4BRoypXxkLr123=='

```

## <a name="set-final-environment-variables-and-confirm"></a>Festlegen und Bestätigen der endgültigen Umgebungsvariablen

Legen Sie die URL der SPN-Autorität in einer Umgebungsvariablen fest:

```console
#PowerShell
$Env:SPN_AUTHORITY='https://login.microsoftonline.com'

#Linux/macOS:
export SPN_AUTHORITY='https://login.microsoftonline.com'
```

Stellen Sie sicher, dass alle erforderlichen Umgebungsvariablen festgelegt sind:

```console
#PowerShell
$Env:WORKSPACE_ID
$Env:WORKSPACE_SHARED_KEY
$Env:SPN_TENANT_ID
$Env:SPN_CLIENT_ID
$Env:SPN_CLIENT_SECRET
$Env:SPN_AUTHORITY

#Linux/macOS
echo $WORKSPACE_ID
echo $WORKSPACE_SHARED_KEY
echo $SPN_TENANT_ID
echo $SPN_CLIENT_ID
echo $SPN_CLIENT_SECRET
echo $SPN_AUTHORITY
```

## <a name="upload-metrics-to-azure-monitor"></a>Hochladen von Metriken in Azure Monitor

Zum Hochladen von Metriken für Ihre verwalteten Azure SQL-Instanzen und Azure Database for PostgreSQL Hyperscale-Servergruppen führen Sie die folgenden CLI-Befehle aus:

Hiermit werden alle Metriken in die angegebene Datei exportiert:

```console
azdata arc dc export -t metrics --path metrics.json
```

Hiermit werden die Metriken in Azure Monitor hochgeladen:

```console
azdata arc dc upload --path metrics.json
```

## <a name="view-the-metrics-in-the-portal"></a>Anzeigen der Metriken im Portal

Nach dem Hochladen Ihrer Metriken sollten Sie sie im Azure-Portal visualisieren können.

Zum Anzeigen Ihrer Metriken im Portal verwenden Sie diesen speziellen Link, um das Portal zu öffnen: <https://portal.azure.com>. Suchen Sie anschließend in der Suchleiste anhand ihres Namens nach Ihrer Datenbankinstanz:

Sie können die CPU-Auslastung auf der Übersichtsseite anzeigen, oder Sie können ausführlichere Metriken anzeigen, indem Sie im linken Navigationsbereich auf „Metriken“ klicken.

Wählen Sie SQL Server als Metriknamespace aus:

Wählen Sie die Metrik aus, die Sie visualisieren möchten (Sie können auch mehrere Metriken auswählen):

Ändern Sie die Häufigkeit in die letzten 30 Minuten:

> [!NOTE]
> Metriken können nur für die letzten 30 Minuten hochgeladen werden. Azure Monitor lehnt Metriken ab, die älter als 30 Minuten sind.

## <a name="upload-logs-to-azure-monitor"></a>Hochladen von Protokollen in Azure Monitor

 Zum Hochladen von Protokollen für Ihre verwalteten Azure SQL-Instanzen und Azure Database for PostgreSQL Hyperscale-Servergruppen führen Sie die folgenden CLI-Befehle aus:

Hiermit werden alle Protokolle in die angegebene Datei exportiert:

```console
azdata arc dc export -t logs --path logs.json
```

Hiermit werden Protokolle in einen Log Analytics-Arbeitsbereich in Azure Monitor hochgeladen:

```console
azdata arc dc upload --path logs.json
```

## <a name="view-your-logs-in-azure-portal"></a>Anzeigen Ihrer Protokolle im Azure-Portal

Nachdem Ihre Protokolle hochgeladen wurden, sollten Sie sie mit dem Abfrage-Explorer für Protokolle wie folgt abfragen können:

1. Öffnen Sie das Azure-Portal, und suchen Sie dann oben in der Suchleiste anhand des Namens nach Ihrem Arbeitsbereich. Wählen Sie ihn aus.
2. Klicken Sie im linken Bereich auf „Protokolle“.
3. Klicken Sie auf „Erste Schritte“ (bzw. auf die Links auf der Seite „Erste Schritte“, um mehr über Log Analytics zu erfahren, falls Sie noch nicht mit Log Analytics vertraut sind).
4. Arbeiten Sie das Tutorial durch, um weitere Informationen zu Log Analytics zu erhalten, wenn Sie dieses Produkt erstmals verwenden.
5. Erweitern Sie unten in der Liste mit den Tabellen die Option „Benutzerdefinierte Protokolle“. Eine Tabelle mit dem Namen „sql_instance_logs_CL“ wird angezeigt.
6. Klicken Sie neben dem Tabellennamen auf das Augensymbol.
7. Klicken Sie auf die Schaltfläche „View in query editor“ (Im Abfrage-Editor anzeigen).
8. Sie verfügen nun über eine Abfrage im Abfrage-Editor, mit der die letzten zehn Ereignisse im Protokoll angezeigt werden.
9. Sie können jetzt mit Abfragen für die Protokolle experimentieren, indem Sie den Abfrage-Editor verwenden, Warnungen festlegen usw.

## <a name="automating-metrics-and-logs-uploads-optional"></a>Automatisieren von Metrik- und Protokolluploads (optional)

Wenn Sie regelmäßig Metriken und Protokolle hochladen möchten, können Sie ein Skript erstellen und es alle paar Minuten mit einem Timer ausführen.  Im Folgenden finden Sie ein Beispiel für die Automatisierung der Uploads mithilfe eines Linux-Shellskripts.

Fügen Sie in Ihrem bevorzugten Text-/Code-Editor Folgendes zum Skriptinhalt der Datei hinzu, und speichern Sie die Datei als ausführbare Skriptdatei, z. B. als SH-Datei (Linux/Mac) oder als CMD-, BAT- oder PS1-Datei.

```console
azdata arc dc export --type metrics --path metrics.json --force
azdata arc dc upload --path metrics.json
```

Ausführen der Skriptdatei

```console
chmod +x myuploadscript.sh
```

Führen Sie das Skript alle 2 Minuten aus:

```console
watch -n 120 ./myuploadscript.sh
```

Sie können auch einen Auftragsplaner wie cron oder den Windows-Taskplaner bzw. einen Orchestrator wie Ansible, Puppet oder Chef verwenden.
