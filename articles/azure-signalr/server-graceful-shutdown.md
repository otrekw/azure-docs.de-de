---
title: Ordnungsgemäßes Beenden Ihres App-Servers
description: Dieser Artikel enthält Informationen zum ordnungsgemäßen Herunterfahren des SignalR-App-Servers.
author: terencefan
ms.author: tefa
ms.date: 11/12/2020
ms.service: signalr
ms.topic: conceptual
ms.openlocfilehash: d9dd7ce9cf321628598a7bb866c5d1b1a6fb0e1e
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98201670"
---
# <a name="server-graceful-shutdown"></a>Ordnungsgemäßes Herunterfahren von Servern
Microsoft Azure SignalR Service bietet zwei Modi zum ordnungsgemäßen Herunterfahren eines Servers. 

Der Hauptvorteil der Verwendung dieses Features besteht darin, dass bei Ihrem Kunden keine unerwarteten Verbindungsausfälle auftreten. 

Stattdessen können Sie in Ihrer Geschäftslogik darauf warten, dass Ihre Clientverbindungen selbstständig getrennt werden, oder Sie können die Clientverbindung sogar zu einem anderen Server migrieren, ohne Daten zu verlieren. 

## <a name="how-it-works"></a>Funktionsweise

Im Allgemeinen erfolgt das ordnungsgemäße Herunterfahren in vier Phasen:

1. **Versetzen des Servers in den Offlinemodus**

    Dadurch werden keine weiteren Clientverbindungen an diesen Server weitergeleitet.

2. **Auslösen von `OnShutdown`-Hooks**

    Sie können für jeden Hub in Ihrem Besitz auf dem Server Shutdown-Hooks registrieren.
    Diese werden direkt nach einer **FINACK**-Antwort von Azure SignalR Service in der registrierten Reihenfolge aufgerufen, da durch diese angezeigt wird, dass dieser Server in Azure SignalR Service offline geschaltet wurde.

    In dieser Phase können Sie Nachrichten übertragen oder Bereinigungsaufträge ausführen. Nachdem alle Shutdown-Hooks ausgeführt wurden, tritt die nächste Phase ein.

3. **Warten auf das Trennen aller Clientverbindungen.** Dies ist vom ausgewählten Modus abhängig, der einer der folgenden sein kann:

    **Modus WaitForClientsToClose**

    Azure SignalR Service wartet auf vorhandene Clients.

    Möglicherweise müssen Sie ein entsprechendes Verfahren entwickeln, z. B. das Senden einer Benachrichtigung über das Trennen an alle Clients. Anschließend können Ihre Clients entscheiden, wann die Verbindung getrennt oder wiederhergestellt werden soll.

    Sehen Sie sich das [ChatSample](https://github.com/Azure/azure-signalr/tree/dev/samples/ChatSample) an. Dort finden Sie ein Beispiel, bei dem über einen Shutdown-Hook eine „exit“-Meldung übertragen wird, um die Clients zu trennen.

    **Modus MigrateClients**

    Azure SignalR Service versucht, die Clientverbindungen auf diesem Server auf einen anderen gültigen Server umzuleiten. 
    
    In diesem Szenario werden `OnConnectedAsync` und `OnDisconnectedAsync` auf dem neuen bzw. dem alten Server ausgelöst, wobei ein `IConnectionMigrationFeature` im `HttpContext` festgelegt wird. Damit können Sie die Richtung der Migration der Clientverbindung ermitteln. Dies kann besonders in zustandsbehafteten Szenarien hilfreich sein.

    Die Clientverbindung wird sofort migriert, nachdem die aktuelle Nachricht übermittelt wurde. Dies bedeutet, dass die nächste Nachricht an den neuen Server weitergeleitet wird.

4. **Trennen der Serververbindungen**

    Nachdem alle Clientverbindungen getrennt/migriert wurden oder das Timeout (standardmäßig 30 s) überschritten wurde, geschieht Folgendes:

    Das SignalR-Server SDK fährt mit dieser Phase des Herunterfahrens fort und schließt alle Serververbindungen.

    Clientverbindungen, die nicht geschlossen oder migriert wurden, werden trotzdem getrennt. Beispiele sind das Fehlen eines geeigneten Zielservers oder eine nicht abgeschlossene Meldung vom aktuellen Client zum Server.

## <a name="sample-codes"></a>Codebeispiele

Fügen Sie bei Verwendung von `AddAzureSignalR` folgende Optionen hinzu:

```csharp
services.AddSignalR().AddAzureSignalR(option =>
{
    option.GracefulShutdown.Mode = GracefulShutdownMode.WaitForClientsClose;
    // option.GracefulShutdown.Mode = GracefulShutdownMode.MigrateClients;
    option.GracefulShutdown.Timeout = TimeSpan.FromSeconds(30);

    option.GracefulShutdown.Add<Chat>(async (c) =>
    {
        await c.Clients.All.SendAsync("exit");
    });
});
```

### <a name="configure-onconnected-and-ondisconnected-while-setting-graceful-shutdown-mode-to-migrateclients"></a>Konfigurieren von `OnConnected` und `OnDisconnected` beim Herunterfahrmodus `MigrateClients`

Wir haben ein IConnectionMigrationFeature eingeführt, mit dem angegeben wird, in welche Richtung eine Verbindung migriert wurde.

```csharp
public class Chat : Hub {

    public override async Task OnConnectedAsync()
    {
        Console.WriteLine($"{Context.ConnectionId} connected.");

        var feature = Context.GetHttpContext().Features.Get<IConnectionMigrationFeature>();
        if (feature != null)
        {
            Console.WriteLine($"[{feature.MigrateTo}] {Context.ConnectionId} is migrated from {feature.MigrateFrom}.");
            // Your business logic.
        }

        await base.OnConnectedAsync();
    }

    public override async Task OnDisconnectedAsync(Exception e)
    {
        Console.WriteLine($"{Context.ConnectionId} disconnected.");

        var feature = Context.GetHttpContext().Features.Get<IConnectionMigrationFeature>();
        if (feature != null)
        {
            Console.WriteLine($"[{feature.MigrateFrom}] {Context.ConnectionId} will be migrated to {feature.MigrateTo}.");
            // Your business logic.
        }

        await base.OnDisconnectedAsync(e);
    }
}
```