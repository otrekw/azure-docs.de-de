---
title: Protokollierung für verwaltete Azure-HSMs
description: Dieses Tutorial unterstützt Sie bei den ersten Schritten mit der Protokollierung für verwaltete HSMs.
services: key-vault
author: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: managed-hsm
ms.topic: tutorial
ms.date: 09/15/2020
ms.author: mbaldwin
ms.openlocfilehash: 7420ffbe5b365c635c1eac2620cfd54ceb649ebf
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102211803"
---
# <a name="managed-hsm-logging"></a>Protokollierung für verwaltete HSMs 

Nachdem Sie mindestens ein verwaltetes HSM erstellt haben, möchten Sie wahrscheinlich überwachen, wie, wann und von wem auf Ihre HSMs zugegriffen wird. Hierzu können Sie die Protokollierung aktivieren, um Informationen im von Ihnen angegebenen Azure-Speicherkonto zu speichern. Für Ihr angegebenes Speicherkonto wird automatisch ein neuer Container namens **insights-logs-logs-auditevent** erstellt. Das gleiche Speicherkonto kann verwendet werden, um Protokolle für mehrere verwaltete HSMs zu sammeln.

Sie können auf Ihre Protokollinformationen (spätestens) zehn Minuten nach dem Vorgang des verwalteten HSM zugreifen. In den meisten Fällen geht es aber schneller.  Die Verwaltung der Protokolle im Speicherkonto ist Ihre Aufgabe:

* Verwenden Sie zum Schutz Ihrer Protokolle standardmäßige Azure-Zugriffssteuerungsmethoden, indem Sie den Zugriff auf diese auf bestimmte Benutzer beschränken.
* Löschen Sie Protokolle, die im Speicherkonto nicht mehr aufbewahrt werden sollen.

Dieses Tutorial unterstützt Sie bei den ersten Schritten mit der Protokollierung für verwaltete HSMs. Sie erstellen ein Speicherkonto, aktivieren die Protokollierung und interpretieren die erfassten Protokollinformationen.  

> [!NOTE]
> Dieses Tutorial enthält keine Anleitung für die Erstellung von verwalteten HSMs oder Schlüsseln. Dieser Artikel enthält Azure CLI-Anweisungen zum Aktualisieren der Diagnoseprotokollierung.

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie die in diesem Artikel aufgeführten Schritte ausführen können, benötigen Sie Folgendes:

* Ein Abonnement für Microsoft Azure. Falls Sie über kein Azure-Abonnement verfügen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial) registrieren.
* Azure-Befehlszeilenschnittstelle ab Version 2.12.0. Führen Sie `az --version` aus, um die Version zu ermitteln. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI]( /cli/azure/install-azure-cli).
* Ein verwaltetes HSM in Ihrem Abonnement. Weitere Informationen finden Sie unter [Schnellstart: Bereitstellen und Aktivieren eines verwalteten HSM mithilfe der Azure-Befehlszeilenschnittstelle](quick-create-cli.md). Dort erfahren Sie, wie Sie ein verwaltetes HSM bereitstellen und aktivieren.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="connect-to-your-azure-subscription"></a>Verbinden mit Ihrem Azure-Abonnement

Der erste Schritt bei der Einrichtung der Schlüsselprotokollierung besteht darin, die Azure-Befehlszeilenschnittstelle auf das verwaltete HSM zu verweisen, das Sie protokollieren möchten.

```azurecli-interactive
az login
```

Weitere Informationen zu den Anmeldeoptionen für die Befehlszeilenschnittstelle finden Sie unter [Anmelden mit Azure CLI](/cli/azure/authenticate-azure-cli).

Möglicherweise müssen Sie das Abonnement angeben, mit dem Sie Ihr verwaltetes HSM erstellt haben. Geben Sie den folgenden Befehl ein, um die Abonnements für Ihr Konto anzuzeigen:

## <a name="identify-the-managed-hsm-and-storage-account"></a>Angeben des verwalteten HSM und des Speicherkontos

```azurecli-interactive
hsmresource=$(az keyvault show --hsm-name ContosoMHSM --query id -o tsv)
storageresource=$(az storage account show --name ContosoMHSMLogs --query id -o tsv)
```

## <a name="enable-logging"></a>Aktivieren der Protokollierung

Verwenden Sie zum Aktivieren der Protokollierung für das verwaltete HSM den Befehl **az monitor diagnostic-settings create** mit den Variablen, die für das neue Speicherkonto und das verwaltete HSM erstellt wurden. Legen Sie außerdem das Flag **-Enabled** auf **$true** und die Kategorie auf **AuditEvent** (einzige Kategorie für die Protokollierung für verwaltete HSMs) fest:

Diese Ausgabe bestätigt, dass die Protokollierung für Ihr verwaltetes HSM nun aktiviert ist und Informationen in Ihrem Speicherkonto gespeichert werden.

Optional können Sie eine Aufbewahrungsrichtlinie für Ihre Protokolle festlegen, mit der ältere Protokolle automatisch gelöscht werden. Richten Sie die Aufbewahrungsrichtlinie z. B. wie folgt ein: Legen Sie das Flag **-RetentionEnabled** auf **$true** und den Parameter **-RetentionInDays** auf **90** fest, sodass Protokolle, die älter sind als 90 Tage, automatisch gelöscht werden.

```azurecli-interactive
az monitor diagnostic-settings create --name ContosoMHSM-Diagnostics --resource $hsmresource --logs '[{"category": "AuditEvent","enabled": true}]' --storage-account $storageresource
```

Protokollierte Daten:

* Alle authentifizierten REST-API-Anforderungen, z. B. auch Anforderungen, die aufgrund von Zugriffsberechtigungen, Systemfehlern oder fehlerhaften Anforderungen nicht erfolgreich sind.
* Vorgänge für das eigentliche verwaltete HSM – einschließlich Erstellung, Löschung und Aktualisierung von Attributen (beispielsweise Tags).
* Vorgänge im Zusammenhang mit der Sicherheitsdomäne wie Initialisieren und Herunterladen, Initialisieren der Wiederherstellung oder Hochladen
* Vollständige Sicherung, vollständige Wiederherstellung und selektive Wiederherstellung für das HSM
* Vorgänge für Schlüssel. Beispielsweise:
  * Erstellen, Ändern oder Löschen der Schlüssel
  * Signieren, Überprüfen, Verschlüsseln, Entschlüsseln, Packen und Entpacken von Schlüsseln sowie Auflisten von Schlüsseln
  * Sichern, Wiederherstellen und Bereinigen von Schlüsseln
* Bei nicht authentifizierten Anforderungen wird eine 401-Antwort zurückgegeben. Beispiele sind Anforderungen ohne Bearertoken, falsch formatierte oder abgelaufene Anforderungen oder Anforderungen, deren Token ungültig ist.  

## <a name="access-your-logs"></a>Zugreifen auf Ihre Protokolle

Protokolle für verwaltete HSMs werden im Container **insights-logs-auditevent** im von Ihnen angegebenen Speicherkonto gespeichert. Zur Anzeige der Protokolle müssen Sie Blobs herunterladen. Weitere Informationen zu Azure Storage finden Sie unter [Schnellstart: Erstellen, Herunterladen und Auflisten von Blobs mit der Azure-Befehlszeilenschnittstelle](../../storage/blobs/storage-quickstart-blobs-cli.md).

Einzelne Blobs werden als Text im JSON-Format gespeichert. Schauen wir uns einen Beispielprotokolleintrag an. Das folgende Beispiel zeigt den Protokolleintrag, der erstellt wird, wenn eine Anforderung zum Erstellen einer vollständigen Sicherung an das verwaltete HSM gesendet wird:

```json
[
  {
    "TenantId": "766eaf62-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "time": "2020-08-31T19:52:39.763Z",
    "resourceId": "/SUBSCRIPTIONS/A1BA9AAA-xxxx-xxxx-xxxx-xxxxxxxxxxxx/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/MANAGEDHSMS/CONTOSOMHSM",
    "operationName": "BackupCreate",
    "operationVersion": "7.0",
    "category": "AuditEvent",
    "resultType": "Success",
    "properties": {
        "PoolType": "M-HSM",
        "sku_Family": "B",
        "sku_Name": "Standard_B1"
    },
    "durationMs": 488,
    "callerIpAddress": "X.X.X.X",
    "identity": "{\"claim\":{\"appid\":\"04b07795-xxxx-xxxx-xxxx-xxxxxxxxxxxx\",\"http_schemas_microsoft_com_identity\":{\"claims\":{\"objectidentifier\":\"b1c52bf0-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"}},\"http_schemas_xmlsoap_org_ws_2005_05_identity\":{\"claims\":{\"upn\":\"admin@contoso.com\"}}}}",
    "clientInfo": "azsdk-python-core/1.7.0 Python/3.8.2 (Linux-4.19.84-microsoft-standard-x86_64-with-glibc2.29) azsdk-python-azure-keyvault/7.2",
    "correlationId": "8806614c-ebc3-11ea-9e9b-00155db778ad",
    "subnetId": "(unknown)",
    "httpStatusCode": 202,
    "PoolName": "mhsmdemo",
    "requestUri": "https://ContosoMHSM.managedhsm.azure.net/backup",
    "resourceGroup": "ContosoResourceGroup",
    "resourceProvider": "MICROSOFT.KEYVAULT",
    "resource": "ContosoMHSM",
    "resourceType": "managedHSMs"
  }
]
```

In der folgenden Tabelle sind die Feldnamen und Beschreibungen aufgeführt:

| Feldname | BESCHREIBUNG |
| --- | --- |
| **TenantId** | Azure Active Directory-Mandanten-ID des Abonnements, in dem das verwaltete HSM erstellt wird |
| **time** |Datum und Uhrzeit (UTC). |
| **Ressourcen-ID** |Azure Resource Manager-Ressourcen-ID Bei Protokollen für verwaltete HSMs handelt es sich hierbei immer um die Ressourcen-ID des verwalteten HSM. |
| **operationName** |Name des Vorgangs, wie in der folgenden Tabelle beschrieben |
| **operationVersion** |Die vom Client angeforderte REST-API-Version. |
| **category** |Der Typ des Ergebnisses. Bei Protokollen für verwaltete HSMs ist **AuditEvent** der einzige verfügbare Wert. |
| **resultType** |Das Ergebnis der REST-API-Anforderung. |
| **properties** |Informationen, die je nach Vorgang (**operationName**) variieren|
| **resultSignature** |HTTP-Status |
| **resultDescription** |Zusätzliche Beschreibung zum Ergebnis, falls verfügbar |
| **durationMs** |Verarbeitungsdauer der REST-API-Anforderung in Millisekunden. Die Netzwerklatenz ist hierin nicht enthalten, sodass die auf der Clientseite gemessene Zeit unter Umständen nicht mit diesem Zeitraum übereinstimmt. |
| **callerIpAddress** |Die IP-Adresse des Clients, der die Anforderung gestellt hat. |
| **correlationId** |Optionale GUID, die vom Client zum Korrelieren von clientseitigen Protokollen mit dienstseitigen Protokollen übergeben werden kann |
| **Identität** |Identität des Tokens, das in der REST-API-Anforderung angegeben wurde. Hierbei handelt es sich in der Regel um einen Benutzer oder Dienstprinzipal. |
| **requestUri** | Anforderungs-URI der REST-API |
| **clientInfo** | 

## <a name="use-azure-monitor-logs"></a>Verwenden von Azure Monitor-Protokollen

Sie können die Key Vault-Lösung in Azure Monitor verwenden, um Protokolle vom Typ **AuditEvent** für verwaltete HSMs zu überprüfen. In Azure Monitor-Protokollen verwenden Sie Protokollabfragen, um Daten zu analysieren und die benötigten Informationen zu erhalten. 

## <a name="next-steps"></a>Nächste Schritte

- Machen Sie sich mit [bewährten Methoden](best-practices.md) für die Bereitstellung und Verwendung eines verwalteten HSM vertraut.
- Informieren Sie sich über die [Sicherung und Wiederherstellung](backup-restore.md) verwalteter HSMs.
