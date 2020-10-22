---
title: Hochladen von Nutzungsdaten, Metriken und Protokollen in Azure Monitor
description: Hochladen von Ressourcenbestandsdaten, Nutzungsdaten, Metriken und Protokollen in Azure Monitor
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 69a2bc002b2ccccba90bde5b6c92d162b6e78815
ms.sourcegitcommit: a2d8acc1b0bf4fba90bfed9241b299dc35753ee6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2020
ms.locfileid: "91952249"
---
# <a name="upload-usage-data-metrics-and-logs-to-azure-monitor"></a>Hochladen von Nutzungsdaten, Metriken und Protokollen in Azure Monitor

Sie können in regelmäßigen Abständen Nutzungsinformationen zu Abrechnungszwecken, Überwachungsmetriken und Protokolle exportieren und dann in Azure hochladen.  Beim Exportieren und Hochladen dieser drei Datentypen werden auch die Ressourcen des Datencontrollers, der verwalteten SQL-Instanz und der PostgreSQL Hyperscale-Servergruppe in Azure erstellt und aktualisiert.

> [!NOTE] 
> Während der Vorschauphase fallen keine Kosten für die Verwendung Azure Arc-fähiger Datendienste an.

## <a name="prerequisites"></a>Voraussetzungen

Die Azure-Befehlszeilenschnittstelle (az) und Azure Data CLI (azdata) müssen installiert sein.  [Installieren Sie die Tools.](./install-client-tools.md)

Vor dem Hochladen von Daten in Azure müssen Sie sicherstellen, dass der Microsoft.AzureData-Ressourcenanbieter für Ihr Azure-Abonnement registriert wurde.

Dies können Sie durch Ausführen des folgenden Befehls überprüfen:

```console
az provider show -n Microsoft.AzureData -o table
```

Wenn der Ressourcenanbieter in Ihrem Abonnement aktuell nicht registriert ist, können Sie ihn durch Ausführen des folgenden Befehls registrieren.  Die Ausführung dieses Befehls dauert unter Umständen ein bis zwei Minuten.

```console
az provider register -n Microsoft.AzureData --wait
```

## <a name="upload-usage-data"></a>Hochladen von Nutzungsdaten

Nutzungsdaten wie Bestand und Ressourcenverbrauch können wie folgt in zwei Schritten in Azure hochgeladen werden:

1. Exportieren Sie die Nutzungsdaten wie folgt mit dem Befehl ```azdata export```:

   ```console
   #login to the data controller and enter the values at the prompt
   azdata login

   #run the export command
   azdata arc dc export --type usage --path usage.json
   ```
   Mit diesem Befehl wird die Datei `usage.json` mit allen Azure Arc-fähigen Datenressourcen erstellt, dies sind z. B. verwaltete SQL-Instanzen und PostgreSQL Hyperscale-Instanzen, die auf dem Datencontroller erstellt werden.

2. Hochladen der Nutzungsdaten mit dem Befehl ```azdata upload```

   > [!NOTE]
   > Warten Sie nach dem Erstellen des Azure Arc-Datencontrollers mindestens 24 Stunden, bevor Sie Daten hochladen.

   ```console
   #login to the data controller and enter the values at the prompt
   azdata login

   #run the upload command
   azdata arc dc upload --path usage.json
   ```

## <a name="upload-metrics-and-logs"></a>Hochladen von Metriken und Protokollen

Mit den Azure Arc-Datendiensten können Sie optional Metriken und Protokolle in Azure Monitor hochladen, um Metriken und Protokolle aggregieren und analysieren, Warnungen auslösen, Benachrichtigungen senden oder automatisierte Aktionen auslösen zu können. 

Wenn Sie Ihre Daten an Azure Monitor senden, können Sie auch Überwachungs- und Protokollierungsdaten außerhalb des Standorts und in riesigem Umfang für die langfristige Aufbewahrung der Daten für erweiterte Analysen speichern.

Wenn Sie Azure Arc-Datendienste an mehreren Standorten verwenden, können Sie Azure Monitor als zentralen Speicherort verwenden, um alle Protokolle und Metriken für Ihre Standorte zu erfassen.

### <a name="before-you-begin"></a>Voraussetzungen

Zum Aktivieren der Szenarien für das Hochladen von Protokollen und Metriken sind einige einmalige Einrichtungsschritte erforderlich:

1. Erstellen eines Dienstprinzipals oder einer Azure Active Directory-Anwendung, einschließlich Erstellung eines geheimen Clientzugriffsschlüssels und Zuweisung des Dienstprinzipals zur Rolle „Herausgeber für Überwachungsmetriken“ für die Abonnements, in denen sich die Ressourcen der Datenbankinstanz befinden
2. Erstellen eines Log Analytics-Arbeitsbereichs und Abrufen der Schlüssel sowie Festlegen der Informationen in Umgebungsvariablen.

Der erste Schritt ist zum Hochladen von Metriken erforderlich, der zweite zum Hochladen von Protokollen.

Führen Sie diese Befehle aus, um den Dienstprinzipal für das Hochladen von Metriken zu erstellen, und weisen Sie ihn den Rollen „Herausgeber für Überwachungsmetriken“ und „Mitwirkender“ zu, damit der Dienstprinzipal Metriken hochladen und Erstellungs- und Hochladevorgänge ausführen kann.

## <a name="create-service-principal-and-assign-roles"></a>Erstellen des Dienstprinzipals und Zuweisen von Rollen

Führen Sie diese Befehle aus, um den Dienstprinzipal für den Metrikupload zu erstellen, und weisen Sie ihn der Rolle „Herausgeber für Überwachungsmetriken“ zu.

Führen Sie diesen Befehl aus, um einen Dienstprinzipal zu erstellen:

> [!NOTE]
> Zum Erstellen eines Dienstprinzipals sind [bestimmte Berechtigungen in Azure](/azure/active-directory/develop/howto-create-service-principal-portal#permissions-required-for-registering-an-app) erforderlich.

```console
az ad sp create-for-rbac --name <a name you choose>

#Example:
#az ad sp create-for-rbac --name azure-arc-metrics
```

Beispielausgabe:

```output
"appId": "2e72adbf-de57-4c25-b90d-2f73f126e123",
"displayName": "azure-arc-metrics",
"name": "http://azure-arc-metrics",
"password": "5039d676-23f9-416c-9534-3bd6afc78123",
"tenant": "72f988bf-85f1-41af-91ab-2d7cd01ad1234"
```

Speichern Sie die Werte von appId und tenant in einer Umgebungsvariable, um sie später zu verwenden. 

Um die Werte von appId und tenant mithilfe von PowerShell zu speichern, folgen Sie diesem Beispiel:

```powershell
$Env:SPN_CLIENT_ID='<the 'appId' value from the output of the 'az ad sp create-for-rbac' command above>'
$Env:SPN_CLIENT_SECRET='<the 'password' value from the output of the 'az ad sp create-for-rbac' command above>'
$Env:SPN_TENANT_ID='<the 'tenant' value from the output of the 'az ad sp create-for-rbac' command above>'
```

Unter Linux oder macOS können Sie die Werte von appId und tenant alternativ wie im folgenden Beispiel speichern:

   ```console
   export SPN_CLIENT_ID='<the 'appId' value from the output of the 'az ad sp create-for-rbac' command above>'
   export SPN_CLIENT_SECRET='<the 'password' value from the output of the 'az ad sp create-for-rbac' command above>'
   export SPN_TENANT_ID='<the 'tenant' value from the output of the 'az ad sp create-for-rbac' command above>'

   #Example (using Linux):
   export SPN_CLIENT_ID='2e72adbf-de57-4c25-b90d-2f73f126e123'
   export SPN_CLIENT_SECRET='5039d676-23f9-416c-9534-3bd6afc78123'
   export SPN_TENANT_ID='72f988bf-85f1-41af-91ab-2d7cd01ad1234'
   ```

Führen Sie diesen Befehl aus, um dem Dienstprinzipal die Rolle „Herausgeber für Überwachungsmetriken“ für das Abonnement zuzuweisen, in dem sich Ihre Datenbankinstanzressourcen befinden:


> [!NOTE]
> Bei der Ausführung in einer Windows-Umgebung müssen Sie für Rollennamen doppelte Anführungszeichen verwenden.


```console
az role assignment create --assignee <appId value from output above> --role "Monitoring Metrics Publisher" --scope subscriptions/<sub ID>
az role assignment create --assignee <appId value from output above> --role 'Contributor' --scope subscriptions/<sub ID>

#Example:
#az role assignment create --assignee 2e72adbf-de57-4c25-b90d-2f73f126ede5 --role "Monitoring Metrics Publisher" --scope subscriptions/182c901a-129a-4f5d-56e4-cc6b29459123
#az role assignment create --assignee 2e72adbf-de57-4c25-b90d-2f73f126ede5 --role 'Contributor' --scope subscriptions/182c901a-129a-4f5d-56e4-cc6b29459123

#On Windows environment
#az role assignment create --assignee 2e72adbf-de57-4c25-b90d-2f73f126ede5 --role "Monitoring Metrics Publisher" --scope subscriptions/182c901a-129a-4f5d-56e4-cc6b29459123
#az role assignment create --assignee 2e72adbf-de57-4c25-b90d-2f73f126ede5 --role "Contributor" --scope subscriptions/182c901a-129a-4f5d-56e4-cc6b29459123
```

Beispielausgabe:

```console
{
  "canDelegate": null,
  "id": "/subscriptions/<Subscription ID>/providers/Microsoft.Authorization/roleAssignments/f82b7dc6-17bd-4e78-93a1-3fb733b912d",
  "name": "f82b7dc6-17bd-4e78-93a1-3fb733b9d123",
  "principalId": "5901025f-0353-4e33-aeb1-d814dbc5d123",
  "principalType": "ServicePrincipal",
  "roleDefinitionId": "/subscriptions/<Subscription ID>/providers/Microsoft.Authorization/roleDefinitions/3913510d-42f4-4e42-8a64-420c39005123",
  "scope": "/subscriptions/<Subscription ID>",
  "type": "Microsoft.Authorization/roleAssignments"
}
```

## <a name="create-a-log-analytics-workspace"></a>Erstellen eines Log Analytics-Arbeitsbereichs

Führen Sie anschließend diese Befehle aus, um einen Log Analytics-Arbeitsbereich zu erstellen und die Zugriffsinformationen in Umgebungsvariablen festzulegen.

> [!NOTE]
> Überspringen Sie diesen Schritt,wenn Sie bereits über einen Arbeitsbereich verfügen.

```console
az monitor log-analytics workspace create --resource-group <resource group name> --workspace-name <some name you choose>

#Example:
#az monitor log-analytics workspace create --resource-group MyResourceGroup --workspace-name MyLogsWorkpace
```

Beispielausgabe:

```output
{
  "customerId": "d6abb435-2626-4df1-b887-445fe44a4123",
  "eTag": null,
  "id": "/subscriptions/<Subscription ID>/resourcegroups/user-arc-demo/providers/microsoft.operationalinsights/workspaces/user-logworkspace",
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
az monitor log-analytics workspace get-shared-keys --resource-group MyResourceGroup --workspace-name MyLogsWorkpace
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

Zum Hochladen von Metriken für Ihre Azure Arc-fähigen verwalteten SQL-Instanzen und Azure Arc-fähigen PostgreSQL Hyperscale-Servergruppen führen Sie die folgenden CLI-Befehle aus:

1. Exportieren Sie alle Metriken in die angegebene Datei:

   ```console
   #login to the data controller and enter the values at the prompt
   azdata login

   #export the metrics
   azdata arc dc export --type metrics --path metrics.json
   ```

2. Laden Sie die Metriken in Azure Monitor hoch:

   ```console
   #login to the data controller and enter the values at the prompt
   azdata login

   #upload the metrics
   azdata arc dc upload --path metrics.json
   ```

   >[!NOTE]
   >Warten Sie mindestens 30 Minuten, nachdem die Azure Arc-fähigen Dateninstanzen für den ersten Upload erstellt wurden.
   >
   >Führen Sie direkt nach dem `export` unbedingt den `upload` durch, da Azure Monitor nur Metriken für die letzten 30 Minuten akzeptiert. [Weitere Informationen](../../azure-monitor/platform/metrics-store-custom-rest-api.md#troubleshooting)


Wenn beim Exportieren Fehler vom Typ „Failure to get metrics“ (Fehler beim Abrufen der Metriken) angezeigt werden, überprüfen Sie, ob die Datensammlung auf ```true``` festgelegt ist, indem Sie den folgenden Befehl ausführen:

```console
azdata arc dc config show
```

Sehen Sie sich den Abschnitt „security“ an.

```output
 "security": {
      "allowDumps": true,
      "allowNodeMetricsCollection": true,
      "allowPodMetricsCollection": true,
      "allowRunAsRoot": false
    },
```

Überprüfen Sie, ob die Eigenschaften `allowNodeMetricsCollection` und `allowPodMetricsCollection` auf `true` festgelegt sind.

## <a name="view-the-metrics-in-the-portal"></a>Anzeigen der Metriken im Portal

Nachdem die Metriken hochgeladen wurden, können Sie sie im Azure-Portal anzeigen.
> [!NOTE]
> Beachten Sie, dass es einige Minuten dauern kann, bis die hochgeladenen Daten verarbeitet wurden und die Metriken im Portal angezeigt werden.


Zum Anzeigen der Metriken im Portal verwenden Sie folgenden Link, um das Portal zu öffnen: <https://portal.azure.com>. Suchen Sie anschließend in der Suchleiste anhand ihres Namens nach Ihrer Datenbankinstanz:

Sie können die CPU-Auslastung auf der Übersichtsseite anzeigen, oder Sie können ausführlichere Metriken anzeigen, indem Sie im linken Navigationsbereich auf „Metriken“ klicken.

Wählen Sie SQL Server als Metriknamespace aus:

Wählen Sie die Metrik aus, die Sie visualisieren möchten (Sie können auch mehrere Metriken auswählen):

Ändern Sie die Häufigkeit in die letzten 30 Minuten:

> [!NOTE]
> Metriken können nur für die letzten 30 Minuten hochgeladen werden. Azure Monitor lehnt Metriken ab, die älter als 30 Minuten sind.

## <a name="upload-logs-to-azure-monitor"></a>Hochladen von Protokollen in Azure Monitor

 Zum Hochladen von Protokollen für Ihre Azure Arc-fähigen verwalteten SQL-Instanzen und Azure Arc-fähigen PostgreSQL Hyperscale-Servergruppen führen Sie die folgenden CLI-Befehle aus:

1. Exportieren Sie alle Protokolle in die angegebene Datei:

   ```console
   #login to the data controller and enter the values at the prompt
   azdata login

   #export the logs
   azdata arc dc export --type logs --path logs.json
   ```

2. Laden Sie die Protokolle in einen Log Analytics-Arbeitsbereich in Azure Monitor hoch:

   ```console
   #login to the data controller and enter the values at the prompt
   azdata login

   #Upload the logs
   azdata arc dc upload --path logs.json
   ```

## <a name="view-your-logs-in-azure-portal"></a>Anzeigen Ihrer Protokolle im Azure-Portal

Nachdem Ihre Protokolle hochgeladen wurden, sollten Sie sie mit dem Abfrage-Explorer für Protokolle wie folgt abfragen können:

1. Öffnen Sie das Azure-Portal, und suchen Sie dann oben in der Suchleiste anhand des Namens nach Ihrem Arbeitsbereich. Wählen Sie ihn aus.
2. Klicken Sie im linken Bereich auf „Protokolle“.
3. Klicken Sie auf „Erste Schritte“ (bzw. auf die Links auf der Seite „Erste Schritte“, um mehr über Log Analytics zu erfahren, falls Sie noch nicht mit Log Analytics vertraut sind).
4. Wenn Sie Log Analytics erstmals verwenden, arbeiten Sie das Tutorial durch, um weitere Informationen zu Log Analytics zu erhalten.
5. Erweitern Sie unten in der Liste mit den Tabellen die Option „Benutzerdefinierte Protokolle“. Eine Tabelle mit dem Namen „sql_instance_logs_CL“ wird angezeigt.
6. Klicken Sie neben dem Tabellennamen auf das Augensymbol.
7. Klicken Sie auf die Schaltfläche „View in query editor“ (Im Abfrage-Editor anzeigen).
8. Sie verfügen nun über eine Abfrage im Abfrage-Editor, mit der die letzten zehn Ereignisse im Protokoll angezeigt werden.
9. Sie können jetzt mit Abfragen für die Protokolle experimentieren, indem Sie den Abfrage-Editor verwenden, Warnungen festlegen usw.

## <a name="automating-uploads-optional"></a>Automatisieren von Uploads (optional)

Wenn Sie Metriken und Protokolle nach einem Zeitplan hochladen möchten, können Sie ein Skript erstellen und es mit einem Timer alle paar Minuten ausführen. Im Folgenden finden Sie ein Beispiel für die Automatisierung der Uploads mithilfe eines Linux-Shellskripts.

Fügen Sie in Ihrem bevorzugten Text-Editor oder Code-Editor das folgende Skript in eine Datei ein, und speichern Sie die Datei als ausführbare Skriptdatei, z. B. als SH-Datei (Linux/Mac) oder als CMD-, BAT- oder PS1-Datei.

```console
azdata arc dc export --type metrics --path metrics.json --force
azdata arc dc upload --path metrics.json
```

Ausführen der Skriptdatei

```console
chmod +x myuploadscript.sh
```

Führen Sie das Skript alle 20 Minuten aus:

```console
watch -n 1200 ./myuploadscript.sh
```

Sie können auch einen Auftragsplaner wie cron oder den Windows-Taskplaner bzw. einen Orchestrator wie Ansible, Puppet oder Chef verwenden.

## <a name="general-guidance-on-exporting-and-uploading-usage-metrics"></a>Allgemeine Anleitung zum Exportieren und Hochladen von Nutzungsdaten und Metriken

Erstellungs-, Lese-, Aktualisierungs- und Löschvorgänge – sogenannte CRUD-Vorgänge (Create, Read, Update, Delete) – in Azure Arc-fähigen Datendiensten werden zu Abrechnungs- und Überwachungszwecken protokolliert. Hintergrunddienste überwachen diese CRUD-Vorgänge und berechnen den Verbrauch entsprechend. Die tatsächliche Berechnung der Nutzung oder des Verbrauchs erfolgt nach einem Zeitplan und wird im Hintergrund durchgeführt. 

Während der Vorschauphase erfolgt dieser Vorgang in der Nacht. Allgemein wird empfohlen, die Nutzungsdaten nur einmal täglich hochzuladen. Wenn Nutzungsinformationen innerhalb desselben 24-Stunden-Zeitraums mehrmals exportiert und hochgeladen werden, wird nur der Ressourcenbestand im Azure-Portal aktualisiert, jedoch nicht der Ressourcenverbrauch.

Beim Hochladen von Metriken können in Azure Monitor nur die Daten der letzten 30 Minuten hochgeladen werden ([weitere Informationen](../../azure-monitor/platform/metrics-store-custom-rest-api.md#troubleshooting)). Beim Hochladen von Metriken wird empfohlen, die Metriken unmittelbar nach dem Erstellen der Exportdatei hochzuladen, damit Sie das gesamte Dataset im Azure-Portal anzeigen können. Beispiel: Sie haben die Metriken um 14:00 Uhr exportiert und den Befehl für den Upload um 14:50 Uhr ausgeführt. Da in Azure Monitor nur die Daten der letzten 30 Minuten akzeptiert werden, werden im Portal möglicherweise keine Daten angezeigt. 

## <a name="next-steps"></a>Nächste Schritte

[Hochladen von Abrechnungsdaten in Azure und Anzeigen im Azure-Portal](view-billing-data-in-azure.md)

[Anzeigen einer Azure Arc-Datencontrollerressource im Azure-Portal](view-data-controller-in-azure-portal.md)
