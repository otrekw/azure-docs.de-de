---
title: Hochladen von Nutzungsdaten in Azure
description: Hochladen von Nutzungsdaten für Datendienste mit Azure Arc-Unterstützung in Azure
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 07/30/2021
ms.topic: how-to
zone_pivot_groups: client-operating-system-macos-and-linux-windows-powershell
ms.openlocfilehash: 74df592db61e4c9c50f9b199d7803fb8e1481878
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122346251"
---
# <a name="upload-usage-data-to-azure"></a>Hochladen von Nutzungsdaten in Azure

Sie können regelmäßig Informationen zur Nutzung exportieren. Beim Exportieren und Hochladen dieser Informationen werden die Ressourcen des Datencontrollers, der verwalteten SQL-Instanz und der PostgreSQL Hyperscale-Servergruppe in Azure erstellt und aktualisiert.

> [!NOTE] 
> Während der Vorschauphase fallen keine Kosten für die Verwendung Azure Arc-fähiger Datendienste an.



> [!NOTE]
> Warten Sie nach dem Erstellen des Azure Arc-Datencontrollers mindestens 24 Stunden, bevor Sie Nutzungsdaten hochladen.

## <a name="create-service-principal-and-assign-roles"></a>Erstellen des Dienstprinzipals und Zuweisen von Rollen

Vergewissern Sie sich zunächst, dass Sie den erforderlichen Dienstprinzipal erstellt und einer geeigneten Rolle zugewiesen haben. Details hierzu finden Sie in den folgenden Abschnitten:
* [Erstellen eines Dienstprinzipals](upload-metrics-and-logs-to-azure-monitor.md#create-service-principal)
* [Zuweisen von Rollen zum Dienstprinzipal](upload-metrics-and-logs-to-azure-monitor.md#assign-roles-to-the-service-principal)

## <a name="upload-usage-data"></a>Hochladen von Nutzungsdaten

Nutzungsdaten wie Bestand und Ressourcenverbrauch können wie folgt in zwei Schritten in Azure hochgeladen werden:

1. Exportieren Sie die Nutzungsdaten wie folgt mit dem Befehl `az arcdata dc export`:

> [!NOTE]
> Das Exportieren von Nutzungs-/Abrechnungsinformationen, Metriken und Protokollen mithilfe des Befehls `az arcdata dc export` erfordert vorerst die Umgehung der SSL-Überprüfung.  Sie werden aufgefordert, die SSL-Überprüfung zu umgehen, oder Sie können die Umgebungsvariable `AZDATA_VERIFY_SSL=no` festlegen, um Aufforderungen zu vermeiden.  Derzeit gibt es keine Möglichkeit, ein SSL-Zertifikat für die Datencontroller-Export-API zu konfigurieren.

   ```azurecli
   az arcdata dc export --type usage --path usage.json --k8s-namespace <namespace> --use-k8s
   ```
 
   Mit diesem Befehl wird die Datei `usage.json` mit allen Azure Arc-fähigen Datenressourcen erstellt, dies sind z. B. verwaltete SQL-Instanzen und PostgreSQL Hyperscale-Instanzen, die auf dem Datencontroller erstellt werden.

2. Laden Sie die Nutzungsdaten mit dem Befehl `upload` hoch.

   ```azurecli
   az arcdata dc upload --path usage.json
   ```

## <a name="automating-uploads-optional"></a>Automatisieren von Uploads (optional)

Wenn Sie Metriken und Protokolle nach einem Zeitplan hochladen möchten, können Sie ein Skript erstellen und es mit einem Timer alle paar Minuten ausführen. Im Folgenden finden Sie ein Beispiel für die Automatisierung der Uploads mithilfe eines Linux-Shellskripts.

Fügen Sie in dem von Ihnen bevorzugten Text- oder Code-Editor das folgende Skript in eine Datei ein, und speichern Sie die Datei als ausführbare Skriptdatei, z. B. `.sh` (Linux/Mac) oder `.cmd`, `.bat` oder `.ps1`.

```azurecli
az arcdata dc export --type usage --path usage.json --force --k8s-namespace <namespace> --use-k8s
az arcdata dc upload --path usage.json
```

Ausführen der Skriptdatei

```console
chmod +x myuploadscript.sh
```

Führen Sie das Skript täglich für die Nutzung aus:

```console
watch -n 1200 ./myuploadscript.sh
```

Sie können auch einen Auftragsplaner wie cron oder den Windows-Taskplaner bzw. einen Orchestrator wie Ansible, Puppet oder Chef verwenden.

## <a name="next-steps"></a>Nächste Schritte

[Hochladen von Metriken in Azure Monitor](upload-metrics.md)

[Hochladen von Protokollen in Azure Monitor](upload-logs.md)

[Hochladen von Abrechnungsdaten in Azure und Anzeigen im Azure-Portal](view-billing-data-in-azure.md)

[Anzeigen einer Azure Arc-Datencontrollerressource im Azure-Portal](view-data-controller-in-azure-portal.md)
