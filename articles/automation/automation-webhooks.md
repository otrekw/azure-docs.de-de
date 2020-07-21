---
title: Starten eines Azure Automation-Runbooks über einen Webhook
description: In diesem Artikel erfahren Sie, wie Sie mit einem Webhook ein Runbook in Azure Automation über einen HTTP-Aufruf starten.
services: automation
ms.subservice: process-automation
ms.date: 06/24/2020
ms.topic: conceptual
ms.openlocfilehash: 2d73b87248fff2e99f05d2d6d6263f2bb3abba57
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86185635"
---
# <a name="start-a-runbook-from-a-webhook"></a>Starten eines Runbooks über einen Webhook

Mit einem Webhook kann ein externer Dienst ein bestimmtes Runbook in Azure Automation über eine einfache HTTP-Anforderung starten. Externe Dienste umfassen Azure DevOps Services, GitHub, Azure Monitor-Protokolle und benutzerdefinierte Anwendungen. Ein solcher Dienst kann einen Webhook verwenden, um ein Runbook zu starten, ohne die vollständige Azure Automation-API zu implementieren. Unter [Starten eines Runbooks in Azure Automation](./start-runbooks.md) können Sie Webhooks mit anderen Methoden zum Starten eines Runbooks vergleichen.

> [!NOTE]
> Die Verwendung eines Webhooks zum Starten eines Python-Runbooks wird nicht unterstützt.

![WebhooksOverview](media/automation-webhooks/webhook-overview-image.png)

Informationen zu den Clientanforderungen für TLS 1.2 mit Webhooks finden Sie unter [Erzwingen von TLS 1.2 für Azure Automation](automation-managing-data.md#tls-12-enforcement-for-azure-automation).

## <a name="webhook-properties"></a>Eigenschaften von Webhooks

Die folgende Tabelle beschreibt die Eigenschaften, die Sie für einen Webhook konfigurieren müssen.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Name |Name des Webhooks. Sie können einen beliebigen Namen vergeben, da er nicht für den Client verfügbar gemacht wird. Sie benötigen den Namen nur zur Identifizierung des Runbooks in Azure Automation. Es empfiehlt sich, den Webhook entsprechend dem Client zu benennen, der ihn verwenden wird. |
| URL |URL des Webhooks. Dies ist die eindeutige Adresse, die ein Client mit einer HTTP POST-Anforderung aufruft, um das mit dem Webhook verknüpfte Runbook zu starten. Sie wird beim Erstellen des Webhooks automatisch generiert. Sie können keine benutzerdefinierte URL angeben. <br> <br> Die URL enthält ein Sicherheitstoken, das es einem Drittanbietersystem ermöglicht, das Runbook ohne weitere Authentifizierung aufzurufen. Behandeln Sie die URL daher wie ein Kennwort. Aus Sicherheitsgründen können Sie die URL im Azure-Portal nur zu dem Zeitpunkt anzeigen, zu dem der Webhook erstellt wird. Sie sollten die URL zur späteren Verwendung an einem sicheren Ort speichern. |
| Ablaufdatum | Das Ablaufdatum des Webhooks, nach dem er nicht mehr verwendet werden kann. Sie können das Ablaufdatum nach dem Erstellen des Webhooks ändern, solange der Webhook noch nicht abgelaufen ist. |
| Aktiviert | Diese Einstellung gibt an, dass der Webhook bei der Erstellung standardmäßig aktiviert ist. Wenn Sie diese Eigenschaft deaktivieren, kann der Webhook von keinem Client verwendet werden. Sie können diese Eigenschaft beim Erstellen des Webhooks oder zu einem beliebigen späteren Zeitpunkt festlegen. |

## <a name="parameters-used-when-the-webhook-starts-a-runbook"></a>Parameter, die verwendet werden, wenn der Webhook ein Runbook startet

Ein Webhook kann Werte für Runbookparameter definieren, die verwendet werden, wenn das Runbook gestartet wird. Der Webhook muss Werte für alle obligatorischen Parameter des Runbooks einschließen und kann auch Werte für optionale Parameter umfassen. Ein Parameterwert, der für einen Webhook konfiguriert wurde, kann auch nach der Erstellung des Webhooks geändert werden. Mehrere mit einem einzelnen Runbook verknüpften Webhooks können verschiedene Parameterwerte für das Runbook verwenden. Wenn ein Client ein Runbook mithilfe eines Webhooks startet, können die im Webhook definierten Parameterwerte nicht überschrieben werden.

Um Daten vom Client zu empfangen, unterstützt das Runbook einen einzelnen Parameter mit dem Namen `WebhookData`. Dieser Parameter definiert ein Objekt, das Daten enthält, die der Client in eine POST-Anforderung einschließt.

![WebhookData-Eigenschaften](media/automation-webhooks/webhook-data-properties.png)

Der `WebhookData`-Parameter hat folgende Eigenschaften:

| Eigenschaft | Beschreibung |
|:--- |:--- |
| `WebhookName` | Name des Webhooks |
| `RequestHeader` | Die Hashtabelle mit den Headern der eingehenden POST-Anforderung |
| `RequestBody` | Der Text der eingehenden POST-Anforderung. In diesem Text werden sämtliche Datenformatierungen gespeichert, z. B. Zeichenfolgen, JSON, XML oder formularcodierte Daten. Das Runbook muss so verfasst werden, dass es mit dem erwarteten Datenformat funktioniert. |

Zur Unterstützung des Parameters `WebhookData` ist keine Konfiguration des Webhooks erforderlich, und das Runbook muss ihn nicht akzeptieren. Wenn das Runbook den Parameter nicht definiert, werden alle vom Client gesendeten Details der Anforderung ignoriert.

> [!NOTE]
> Beim Aufrufen eines Webhooks muss der Client für den Fall, dass der Aufruf fehlschlägt, immer alle Parameterwerte speichern. Bei einem Netzwerkausfall oder einem Verbindungsproblem kann die Anwendung fehlgeschlagene Webhook-Aufrufe nicht abrufen.

Wenn Sie beim Erstellen des Webhooks einen Wert für `WebhookData` festlegen, wird dieser überschrieben, wenn der Webhook das Runbook mit den Daten aus der POST-Anforderung des Clients startet. Dies geschieht selbst dann, wenn die Anwendung keine Daten in den Anforderungstext einbezieht. 

Wenn Sie ein Runbook starten, das `WebhookData` mit einem anderen Mechanismus als einem Webhook definiert, können Sie einen Wert für `WebhookData` angeben, der vom Runbook erkannt wird. Bei diesem Wert sollte es sich um ein Objekt mit den gleichen [Eigenschaften](#webhook-properties) wie dem Parameter `WebhookData` handeln, damit das Runbook damit genauso funktioniert, wie mit `WebhookData`-Objekten, die über einen Webhook übergeben werden.

Wenn Sie beispielsweise das folgende Runbook aus dem Azure-Portal starten und Webhook-Beispieldaten zum Testen übergeben möchten, müssen Sie die Daten in der Benutzeroberfläche als JSON übergeben.

![WebhookData-Parameter von der Benutzeroberfläche](media/automation-webhooks/WebhookData-parameter-from-UI.png)

Für das nächste Beispiel für ein Runbook definieren wir die folgenden Eigenschaften für `WebhookData`:

* **WebhookName**: MyWebhook
* **RequestBody**: `*[{'ResourceGroup': 'myResourceGroup','Name': 'vm01'},{'ResourceGroup': 'myResourceGroup','Name': 'vm02'}]*`

Nun übergeben wir das folgende JSON-Objekt in der Benutzeroberfläche für den Parameter `WebhookData`. Dieses Beispiel mit Wagenrücklauf und Zeilenumbruchzeichen entspricht dem vom Webhook übergebenen Format.

```json
{"WebhookName":"mywebhook","RequestBody":"[\r\n {\r\n \"ResourceGroup\": \"vm01\",\r\n \"Name\": \"vm01\"\r\n },\r\n {\r\n \"ResourceGroup\": \"vm02\",\r\n \"Name\": \"vm02\"\r\n }\r\n]"}
```

![WebhookData-Startparameter von der Benutzeroberfläche](media/automation-webhooks/Start-WebhookData-parameter-from-UI.png)

> [!NOTE]
> Azure Automation protokolliert die Werte aller Eingabeparameter mit dem Runbookauftrag. Somit werden alle vom Client in der Webhookanforderung bereitgestellten Eingaben protokolliert und stehen jedem Benutzer mit Zugriff auf den Automatisierungsauftrag zur Verfügung. Aus diesem Grund sollten Sie sorgfältig überlegen, welche vertraulichen Daten Sie in Webhookaufrufe einschließen.

## <a name="webhook-security"></a>Webhook-Sicherheit

Die Sicherheit eines Webhooks beruht auf dem Schutz seiner URL, die ein Sicherheitstoken enthält, mit dem der Webhook aufgerufen werden kann. Azure Automation führt keinerlei Authentifizierung der Anforderung durch, sofern diese über die richtige URL erfolgt. Aus diesem Grund sollten Ihre Client keine Webhooks für Runbooks verwenden, die sehr vertrauliche Vorgänge ausführen, ohne eine alternative Möglichkeit zur Überprüfung der Anforderung zu verwenden.

Sehen Sie sich die folgenden Strategien an:

* Sie können Logik in ein Runbook einschließen, um zu bestimmen, ob es von einem Webhook aufgerufen wird. Lassen Sie das Runbook die Eigenschaft `WebhookName` des Parameters `WebhookData` überprüfen. Das Runbook kann eine weitere Überprüfung durchführen, indem es in den Eigenschaften `RequestHeader`oder `RequestBody` nach bestimmten Informationen sucht.

* Lassen Sie das Runbook eine externe Bedingung überprüfen, wenn es eine Webhookanforderung empfängt. Betrachten Sie z. B. ein Runbook, das von GitHub aufgerufen wird, sobald ein neuer Commit für ein GitHub-Repository vorhanden ist. Das Runbook kann eine Verbindung mit GitHub herstellen, um zu überprüfen, ob ein neuer Commit durchgeführt wurde, bevor es mit der Ausführung fortfährt.

* Azure Automation unterstützt Diensttags für virtuelle Azure-Netzwerke (genauer gesagt: [GuestAndHybridManagement](../virtual-network/service-tags-overview.md)). Sie können Diensttags verwenden, um Netzwerkzugriffssteuerungen für [Netzwerksicherheitsgruppen](../virtual-network/security-overview.md#security-rules) oder [Azure Firewall](../firewall/service-tags.md) zu definieren und Webhooks innerhalb Ihres virtuellen Netzwerks auszulösen. Diensttags können anstelle von spezifischen IP-Adressen verwendet werden, wenn Sie Sicherheitsregeln erstellen. Indem Sie den Diensttagnamen **GuestAndHybridManagement** im entsprechenden Quell- oder Zielfeld einer Regel angeben, können Sie den Datenverkehr für den Automation-Dienst zulassen oder verweigern. Dieses Diensttag weist keine Unterstützung für das Zulassen einer präziseren Steuerung durch das Einschränken von IP-Adressbereichen auf eine bestimmte Region auf.

## <a name="create-a-webhook"></a>Erstellen eines Webhooks

Gehen Sie wie folgt vor, um einen neuen Webhook zu erstellen, der mit einem Runbook im Azure-Portal verknüpft ist

1. Klicken Sie im Azure-Portal auf der Seite „Runbooks“ auf das Runbook, das vom Webhook gestartet werden soll, um die Details des Runbooks anzuzeigen. Stellen Sie sicher, dass das Feld **Status** des Runbooks auf **Veröffentlicht** festgelegt ist.
2. Klicken Sie oben auf der Seite auf **Webhook**, um die Seite „Webhook hinzufügen“ zu öffnen.
3. Klicken Sie auf **Neuen Webhook erstellen**, um die Seite „Webhook erstellen“ zu öffnen.
4. Füllen Sie die Felder **Name** und **Ablaufdatum** für den Webhook aus, und legen Sie fest, dass er aktiviert werden soll. Weitere Informationen zu diesen Eigenschaften finden Sie unter [Eigenschaften von Webhooks](#webhook-properties).
5. Klicken Sie auf das Symbol zum Kopieren, und drücken Sie STRG+C, um die URL des Webhooks zu kopieren. Speichern Sie die URL an einem sicheren Ort. 

    > [!NOTE]
    > Nachdem Sie den Webhook erstellt haben, können Sie die URL nicht erneut abrufen.

   ![Webhook-URL](media/automation-webhooks/copy-webhook-url.png)

1. Klicken Sie auf **Parameter** , um Werte für die Runbookparameter bereitzustellen. Wenn das Runbook über obligatorische Parameter verfügt, können Sie einen Webhook nur dann erstellen, wenn Sie Werte bereitstellen.

2. Klicken Sie auf **Erstellen** , um das Projekt zu erstellen.

## <a name="use-a-webhook"></a>Verwenden eines Webhooks

Damit ein Webhook nach dem Erstellen verwendet werden kann, muss Ihr Client eine HTTP-`POST`-Anforderung mit der URL für den Webhook ausgeben. Die Syntax ist:

```http
http://<Webhook Server>/token?=<Token Value>
```

Der Client empfängt einen der folgenden Rückgabecodes als Antwort auf die `POST`-Anforderung.

| Code | Text | Beschreibung |
|:--- |:--- |:--- |
| 202 |Zulässig |Die Anforderung wurde akzeptiert, und das Runbook wurde erfolgreich in die Warteschlange gestellt. |
| 400 |Ungültige Anforderung |Die Anforderung wurde aus einem der folgenden Gründe nicht akzeptiert: <ul> <li>Der Webhook ist abgelaufen.</li> <li>Der Webhook ist deaktiviert.</li> <li>Das Token in der URL ist ungültig.</li>  </ul> |
| 404 |Nicht gefunden |Die Anforderung wurde aus einem der folgenden Gründe nicht akzeptiert: <ul> <li>Der Webhook wurde nicht gefunden.</li> <li>Das Runbook wurde nicht gefunden.</li> <li>Das Konto wurde nicht gefunden.</li>  </ul> |
| 500 |Interner Serverfehler |Die URL ist gültig, es ist jedoch ein Fehler aufgetreten. Senden Sie die Anforderung erneut. |

Wenn die Anforderung erfolgreich ist, enthält die Antwort des Webhooks wie im Folgenden dargestellt die Auftrags-ID im JSON-Format. Sie enthält eine einzelne Auftrags-ID, wobei das JSON-Format Potenzial für künftige Verbesserungen bietet.

```json
{"JobIds":["<JobId>"]}
```

Der Client kann weder die Bestätigung, dass ein Runbookauftrag abgeschlossen wurde, noch den Abschlussstatus aus einem Webhook abrufen. Diese Informationen können mithilfe der Auftrags-ID unter Verwendung einer anderen Methode ermittelt werden, z. B. mit [Windows PowerShell](/powershell/module/servicemanagement/azure/get-azureautomationjob) oder der [Azure Automation-API](/rest/api/automation/job).

## <a name="renew-a-webhook"></a>Verlängern eines Webhooks

Wenn ein Webhook erstellt wird, verfügt er über einen Gültigkeitszeitraum von zehn Jahren, nach dem er automatisch abläuft. Wenn ein Webhook abgelaufen ist, kann er nicht reaktiviert werden. Sie können ihn nur entfernen und anschließend neu erstellen. 

Sie können einen Webhook verlängern, der seine Ablaufzeit noch nicht erreicht hat. So verlängern Sie einen Webhook:

1. Navigieren Sie zu dem Runbook, das den Webhook enthält. 
2. Wählen Sie unter **Ressourcen** die Option **Webhooks** aus. 
3. Klicken Sie auf den Webhook, den Sie verlängern möchten. 
4. Wählen Sie auf der Seite „Webhook“ ein neues Ablaufdatum aus, und klicken Sie auf **Speichern**.

## <a name="sample-runbook"></a>Beispiel-Runbook

Das folgende Beispiel-Runbook akzeptiert Webhook-Daten und startet die im Anforderungstext angegebenen virtuellen Computer. Wenn Sie dieses Runbook testen möchten, klicken Sie in Ihrem Automation-Konto unter **Runbooks** auf **Runbook erstellen**. Sollten Sie nicht wissen, wie man ein Runbook erstellt, finden Sie unter [Erstellen eines Azure Automation-Runbooks](automation-quickstart-create-runbook.md) weitere Informationen.

> [!NOTE]
> Für nicht grafische PowerShell-Runbooks sind `Add-AzAccount` und `Add-AzureRMAccount` Aliase für [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount?view=azps-3.5.0). Sie können diese Cmdlets verwenden, oder Sie können Ihre Module in Ihrem Automation-Konto auf die aktuellen Versionen [aktualisieren](automation-update-azure-modules.md). Möglicherweise müssen Sie Ihre Module auch dann aktualisieren, wenn Sie gerade ein neues Automation-Konto erstellt haben.

```powershell
param
(
    [Parameter (Mandatory = $false)]
    [object] $WebhookData
)

# If runbook was called from Webhook, WebhookData will not be null.
if ($WebhookData) {

    # Check header for message to validate request
    if ($WebhookData.RequestHeader.message -eq 'StartedbyContoso')
    {
        Write-Output "Header has required information"}
    else
    {
        Write-Output "Header missing required information";
        exit;
    }

    # Retrieve VMs from Webhook request body
    $vms = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

    # Authenticate to Azure by using the service principal and certificate. Then, set the subscription.

    Write-Output "Authenticating to Azure with service principal and certificate"
    $ConnectionAssetName = "AzureRunAsConnection"
    Write-Output "Get connection asset: $ConnectionAssetName"

    $Conn = Get-AutomationConnection -Name $ConnectionAssetName
            if ($Conn -eq $null)
            {
                throw "Could not retrieve connection asset: $ConnectionAssetName. Check that this asset exists in the Automation account."
            }
            Write-Output "Authenticating to Azure with service principal."
            Add-AzAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Write-Output

        # Start each virtual machine
        foreach ($vm in $vms)
        {
            $vmName = $vm.Name
            Write-Output "Starting $vmName"
            Start-AzVM -Name $vm.Name -ResourceGroup $vm.ResourceGroup
        }
}
else {
    # Error
    write-Error "This runbook is meant to be started from an Azure alert webhook only."
}
```

## <a name="test-the-sample"></a>Testen des Beispiels

Im folgenden Beispiel wird Windows PowerShell verwendet, um ein Runbook mit einem Webhook zu starten. Jede Sprache, die eine HTTP-Anforderung durchführen kann, kann einen Webhook verwenden. Windows PowerShell wird hier als Beispiel verwendet.

Das Runbook erwartet im Anforderungstext eine Liste von virtuellen Computern, die in JSON formatiert ist. Das Runbook überprüft auch, ob die Header eine definierte Nachricht enthalten, um sicherzustellen, dass der Webhookaufrufer gültig ist.

```azurepowershell-interactive
$uri = "<webHook Uri>"

$vms  = @(
            @{ Name="vm01";ResourceGroup="vm01"},
            @{ Name="vm02";ResourceGroup="vm02"}
        )
$body = ConvertTo-Json -InputObject $vms
$header = @{ message="StartedbyContoso"}
$response = Invoke-WebRequest -Method Post -Uri $uri -Body $body -Headers $header
$jobid = (ConvertFrom-Json ($response.Content)).jobids[0]
```

Das folgende Beispiel zeigt den Anforderungstext, der dem Runbook in der Eigenschaft `RequestBody` von `WebhookData` zur Verfügung steht. Der Wert ist als JSON formatiert, damit er mit dem Format kompatibel ist, das im Anforderungstext enthalten war.

```json
[
    {
        "Name":  "vm01",
        "ResourceGroup":  "myResourceGroup"
    },
    {
        "Name":  "vm02",
        "ResourceGroup":  "myResourceGroup"
    }
]
```

Die folgende Abbildung zeigt die Anforderung, die von Windows PowerShell gesendet wurde, sowie die daraus resultierende Antwort. Die Auftrags-ID wurde aus der Antwort extrahiert und in eine Zeichenfolge konvertiert.

![Schaltfläche "Webhooks"](media/automation-webhooks/webhook-request-response.png)

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Auslösen eines Runbooks mithilfe einer Warnung finden Sie unter [Verwenden einer Warnung zum Auslösen eines Azure Automation-Runbooks](automation-create-alert-triggered-runbook.md).
