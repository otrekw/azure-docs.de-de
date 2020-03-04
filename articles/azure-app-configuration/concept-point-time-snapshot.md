---
title: Abrufen von Schlüssel-Wert-Paaren für einen bestimmten Zeitpunkt
titleSuffix: Azure App Configuration
description: Rufen Sie alte Schlüssel-Wert-Paare mithilfe von Point-in-Time-Momentaufnahmen in Azure App Configuration ab.
services: azure-app-configuration
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/20/2020
ms.openlocfilehash: 1e2a4f7a7bc5db1b6a49f085821f7fa2bde54229
ms.sourcegitcommit: 3c8fbce6989174b6c3cdbb6fea38974b46197ebe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2020
ms.locfileid: "77523656"
---
# <a name="point-in-time-snapshot"></a>Point-in-Time-Momentaufnahme

In Azure App Configuration wird ein Datensatz von Änderungen an Schlüssel-Wert-Paaren verwaltet. Dieser Datensatz stellt eine Zeitachse der Schlüssel-Wert-Änderungen bereit. Sie können den Verlauf eines beliebigen Schlüssel-Wert-Paars wiederherstellen und dessen bisherigen Wert in einem beliebigen Moment innerhalb der letzten sieben Tage angeben. Mithilfe dieses Features können Sie ein altes Schlüssel-Wert-Paar aus der Vergangenheit abrufen. Beispielsweise können Sie Konfigurationseinstellungen wiederherstellen, die vor der neuesten Bereitstellung verwendet wurden, um ein Rollback der Anwendung zu einer vorherigen Konfiguration durchzuführen.

## <a name="key-value-retrieval"></a>Abrufen von Schlüssel-Wert-Paaren

Sie können mit Azure PowerShell frühere Schlüsselwerte abrufen.  Verwenden Sie `az appconfig revision list`, und fügen Sie die entsprechenden Parameter hinzu, um die erforderlichen Werte abzurufen.  Geben Sie die Azure App Configuration-Instanz über den Speichernamen (`--name {app-config-store-name}`) oder eine Verbindungszeichenfolge (`--connection-string {your-connection-string}`) an. Schränken Sie die Ausgabe ein, indem Sie einen bestimmten Zeitpunkt (`--datetime`) und die maximale Anzahl zurückzugebender Elemente angeben (`--top`).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Rufen Sie alle aufgezeichneten Änderungen an Ihren Schlüssel-Wert-Paaren ab.

```azurepowershell
az appconfig revision list --name {your-app-config-store-name}.
```

Rufen Sie alle aufgezeichneten Änderungen für den Schlüssel `environment` und die Bezeichnungen `test` und `prod` ab.

```azurepowershell
az appconfig revision list --name {your-app-config-store-name} --key environment --label test,prod
```

Rufen Sie alle aufgezeichneten Änderungen im hierarchischen Schlüsselraum `environment:prod` ab.

```azurepowershell
az appconfig revision list --name {your-app-config-store-name} --key environment:prod:* 
```

Rufen Sie alle aufgezeichneten Änderungen für den Schlüssel `color` zu einem bestimmten Zeitpunkt ab.

```azurepowershell
az appconfig revision list --connection-string {your-app-config-connection-string} --key color --datetime "2019-05-01T11:24:12Z" 
```

Rufen Sie die letzten 10 aufgezeichneten Änderungen an Ihren Schlüsselwerten ab, und geben Sie nur die Werte für `key`, `label` und den Zeitstempel `last-modified` zurück.

```azurepowershell
az appconfig revision list --name {your-app-config-store-name} --top 10 --fields key,label,last-modified
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer ASP.NET Core-Web-App](./quickstart-aspnet-core-app.md)  
