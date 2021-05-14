---
title: 'Tutorial: Erstellen einer Blazor Server-Chat-App: Azure SignalR'
description: In diesem Tutorial erfahren Sie, wie Sie mit Azure SignalR Service eine Blazor Server-App erstellen und ändern.
author: JialinXin
ms.service: signalr
ms.topic: tutorial
ms.date: 09/09/2020
ms.author: jixin
ms.openlocfilehash: 5ed5107cadcfbf247b79c18a6a2e391ab3043565
ms.sourcegitcommit: dd425ae91675b7db264288f899cff6add31e9f69
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2021
ms.locfileid: "108331824"
---
# <a name="tutorial-build-a-blazor-server-chat-app"></a>Tutorial: Erstellen einer Blazor Server-Chat-App

In diesem Tutorial erfahren Sie, wie Sie eine Blazor Server-App erstellen und ändern. Sie lernen Folgendes:
> [!div class="checklist"] 
> * Erstellen eines einfachen Chatrooms mit der Blazor Server-App-Vorlage
> * Verwenden von Razor-Komponenten
> * Verwenden von Ereignisbehandlung und Datenbindung in Razor-Komponenten
> * Schnelles Bereitstellen für Azure App Service in Visual Studio
> * Migrieren von der lokalen SignalR-Instanz zu Azure SignalR Service

## <a name="prerequisites"></a>Voraussetzungen

* Installieren Sie das [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) (Mindestversion: 3.0.100).
* Installieren Sie [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) (Mindestversion: 16.3).


[Treten Probleme auf? Informieren Sie uns darüber.](https://aka.ms/asrs/qsblazor)

## <a name="build-a-local-chat-room-in-blazor-server-app"></a>Erstellen eines lokalen Chatrooms in einer Blazor Server-App

Ab der Version 16.2.0 von Visual Studio 2019 ist Azure SignalR Service in den Veröffentlichungsprozess für Webanwendungen integriert, was die Verwaltung der Abhängigkeiten zwischen Web-App und SignalR-Dienst deutlich komfortabler macht. Sie können in einer lokalen SignalR-Instanz in einer lokalen Entwicklungsumgebung arbeiten und gleichzeitig Azure SignalR Service für Azure App Service verwenden, ohne Codeänderungen vornehmen zu müssen.

1. Erstellen einer Blazor-Chat-App:
   1. Wählen Sie in Visual Studio die Option **Neues Projekt erstellen** aus. 
   1. Wählen Sie **Blazor-App** aus. 
   1. Geben Sie der Anwendung einen Namen, und wählen Sie einen Ordner aus. 
   1. Wählen Sie die Vorlage **Blazor Server App** aus.
    
       > [!NOTE]
       > Vergewissern Sie sich, dass Sie mindestens die Version 3.0 des .NET Core SDK installiert haben, damit das Zielframework ordnungsgemäß von Visual Studio erkannt wird.
   
       [ ![Auswählen der Blazor-App-Vorlage unter „Neues Projekt erstellen“](media/signalr-tutorial-build-blazor-server-chat-app/blazor-chat-create.png) ](media/signalr-tutorial-build-blazor-server-chat-app/blazor-chat-create.png#lightbox)
   
   5. Sie können auch den Befehl [`dotnet new`](/dotnet/core/tools/dotnet-new) in der .NET-CLI ausführen, um ein Projekt zu erstellen:
   
       ```dotnetcli
       dotnet new blazorserver -o BlazorChat
       ```
   
1. Fügen Sie eine neue C#-Datei namens `BlazorChatSampleHub.cs` hinzu, und erstellen Sie eine neue Klasse (`BlazorSampleHub`), die sich von der Klasse `Hub` für die Chat-App ableitet. Weitere Informationen zum Erstellen von Hubs finden Sie unter [Erstellen und Verwenden von Hubs](/aspnet/core/signalr/hubs#create-and-use-hubs). 
   
   ```cs
   using System;
   using System.Threading.Tasks;
   using Microsoft.AspNetCore.SignalR;
   
   namespace BlazorChat
   {
       public class BlazorChatSampleHub : Hub
       {
           public const string HubUrl = "/chat";
   
           public async Task Broadcast(string username, string message)
           {
               await Clients.All.SendAsync("Broadcast", username, message);
           }
   
           public override Task OnConnectedAsync()
           {
               Console.WriteLine($"{Context.ConnectionId} connected");
               return base.OnConnectedAsync();
           }
   
           public override async Task OnDisconnectedAsync(Exception e)
           {
               Console.WriteLine($"Disconnected {e?.Message} {Context.ConnectionId}");
               await base.OnDisconnectedAsync(e);
           }
       }
   }
   ```
   
1. Fügen Sie in der Methode `Startup.Configure()` einen Endpunkt für den Hub hinzu.
   
   ```cs
   app.UseEndpoints(endpoints =>
   {
       endpoints.MapBlazorHub();
       endpoints.MapFallbackToPage("/_Host");
       endpoints.MapHub<BlazorChatSampleHub>(BlazorChatSampleHub.HubUrl);
   });
   ```
   
1. Installieren Sie das Paket `Microsoft.AspNetCore.SignalR.Client`, um den SignalR-Client zu verwenden.

   ```dotnetcli
   dotnet add package Microsoft.AspNetCore.SignalR.Client --version 3.1.7
   ```

1. Erstellen Sie unter dem Ordner `Pages` eine neue [Razor-Komponente](/aspnet/core/blazor/components/) namens `ChatRoom.razor`, um den SignalR-Client zu implementieren. Führen Sie die folgenden Schritte aus, oder verwenden Sie die Datei [ChatRoom.razor](https://github.com/aspnet/AzureSignalR-samples/tree/master/samples/BlazorChat/Pages/ChatRoom.razor):

   1. Fügen Sie die Anweisung [`@page`](/aspnet/core/mvc/views/razor#page) und die using-Anweisungen hinzu. Verwenden Sie die Anweisung [`@inject`](/aspnet/core/mvc/views/razor#inject), um den Dienst [`NavigationManager`](/aspnet/core/blazor/fundamentals/routing#uri-and-navigation-state-helpers) einzufügen.
      
      ```razor
      @page "/chatroom"
      @inject NavigationManager navigationManager
      @using Microsoft.AspNetCore.SignalR.Client;
      ```

   1. Fügen Sie dem neuen SignalR-Client im Abschnitt `@code` die folgenden Elemente hinzu, um Nachrichten zu senden und zu empfangen:
      
      ```razor
      @code {
          // flag to indicate chat status
          private bool _isChatting = false;
          
          // name of the user who will be chatting
          private string _username;
      
          // on-screen message
          private string _message;
          
          // new message input
          private string _newMessage;
          
          // list of messages in chat
          private List<Message> _messages = new List<Message>();
          
          private string _hubUrl;
          private HubConnection _hubConnection;
      
          public async Task Chat()
          {
              // check username is valid
              if (string.IsNullOrWhiteSpace(_username))
              {
                  _message = "Please enter a name";
                  return;
              };
      
              try
              {
                  // Start chatting and force refresh UI.
                  _isChatting = true;
                  await Task.Delay(1);

                  // remove old messages if any
                  _messages.Clear();
         
                  // Create the chat client
                  string baseUrl = navigationManager.BaseUri;
      
                  _hubUrl = baseUrl.TrimEnd('/') + BlazorChatSampleHub.HubUrl;
      
                  _hubConnection = new HubConnectionBuilder()
                      .WithUrl(_hubUrl)
                      .Build();
      
                  _hubConnection.On<string, string>("Broadcast", BroadcastMessage);
      
                  await _hubConnection.StartAsync();
      
                  await SendAsync($"[Notice] {_username} joined chat room.");
              }
              catch (Exception e)
              {
                  _message = $"ERROR: Failed to start chat client: {e.Message}";
                  _isChatting = false;
              }
          }
      
          private void BroadcastMessage(string name, string message)
          {
              bool isMine = name.Equals(_username, StringComparison.OrdinalIgnoreCase);
      
              _messages.Add(new Message(name, message, isMine));
      
              // Inform blazor the UI needs updating
              StateHasChanged();
          }
      
          private async Task DisconnectAsync()
          {
              if (_isChatting)
              {
                  await SendAsync($"[Notice] {_username} left chat room.");
      
                  await _hubConnection.StopAsync();
                  await _hubConnection.DisposeAsync();
      
                  _hubConnection = null;
                  _isChatting = false;
              }
          }
      
          private async Task SendAsync(string message)
          {
              if (_isChatting && !string.IsNullOrWhiteSpace(message))
              {
                  await _hubConnection.SendAsync("Broadcast", _username, message);
      
                  _newMessage = string.Empty;
              }
          }
      
          private class Message
          {
              public Message(string username, string body, bool mine)
              {
                  Username = username;
                  Body = body;
                  Mine = mine;
              }
      
              public string Username { get; set; }
              public string Body { get; set; }
              public bool Mine { get; set; }
      
              public bool IsNotice => Body.StartsWith("[Notice]");
      
              public string CSS => Mine ? "sent" : "received";
          }
      }
      ```

   1. Fügen Sie vor dem Abschnitt `@code` das Benutzeroberflächenmarkup für die Interaktion mit dem SignalR-Client hinzu.
      
      ```razor
      <h1>Blazor SignalR Chat Sample</h1>
      <hr />
      
      @if (!_isChatting)
      {
          <p>
              Enter your name to start chatting:
          </p>
      
          <input type="text" maxlength="32" @bind="@_username" />
          <button type="button" @onclick="@Chat"><span class="oi oi-chat" aria-hidden="true"></span> Chat!</button>
      
          // Error messages
          @if (_message != null)
          {
              <div class="invalid-feedback">@_message</div>
              <small id="emailHelp" class="form-text text-muted">@_message</small>
          }
      }
      else
      {
          // banner to show current user
          <div class="alert alert-secondary mt-4" role="alert">
              <span class="oi oi-person mr-2" aria-hidden="true"></span>
              <span>You are connected as <b>@_username</b></span>
              <button class="btn btn-sm btn-warning ml-md-auto" @onclick="@DisconnectAsync">Disconnect</button>
          </div>
          // display messages
          <div id="scrollbox">
              @foreach (var item in _messages)
              {
                  @if (item.IsNotice)
                  {
                      <div class="alert alert-info">@item.Body</div>
                  }
                  else
                  {
                      <div class="@item.CSS">
                          <div class="user">@item.Username</div>
                          <div class="msg">@item.Body</div>
                      </div>
                  }
              }
              <hr />
              <textarea class="input-lg" placeholder="enter your comment" @bind="@_newMessage"></textarea>
              <button class="btn btn-default" @onclick="@(() => SendAsync(_newMessage))">Send</button>
          </div>
      }
      ```

1. Aktualisieren Sie die Komponente `NavMenu.razor`, um eine neue Komponente vom Typ `NavLink` für die Verknüpfung mit dem Chatroom unter `NavMenuCssClass` einzufügen.

   ```razor
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="chatroom">
           <span class="oi oi-chat" aria-hidden="true"></span> Chat room
       </NavLink>
   </li>
   ```
   
1. Fügen Sie der Datei `site.css` einige CSS-Klassen hinzu, um die Benutzeroberflächenelemente auf der Chatseite zu formatieren.

   ```css
   /* improved for chat text box */
   textarea {
       border: 1px dashed #888;
       border-radius: 5px;
       width: 80%;
       overflow: auto;
       background: #f7f7f7
   }
   
   /* improved for speech bubbles */
   .received, .sent {
       position: relative;
       font-family: arial;
       font-size: 1.1em;
       border-radius: 10px;
       padding: 20px;
       margin-bottom: 20px;
   }
   
   .received:after, .sent:after {
       content: '';
       border: 20px solid transparent;
       position: absolute;
       margin-top: -30px;
   }
   
   .sent {
       background: #03a9f4;
       color: #fff;
       margin-left: 10%;
       top: 50%;
       text-align: right;
   }
   
   .received {
       background: #4CAF50;
       color: #fff;
       margin-left: 10px;
       margin-right: 10%;
   }
   
   .sent:after {
       border-left-color: #03a9f4;
       border-right: 0;
       right: -20px;
   }
   
   .received:after {
       border-right-color: #4CAF50;
       border-left: 0;
       left: -20px;
   }
   
   /* div within bubble for name */
   .user {
       font-size: 0.8em;
       font-weight: bold;
       color: #000;
   }
   
   .msg {
       /*display: inline;*/
   }
   ```

1. Drücken Sie <kbd>F5</kbd> , um die App auszuführen. Nun können Sie den Chat initiieren:

   [ ![Ein animierter Chat zwischen Bob und Alice. Alice sagt „Hello“, und Bob sagt „Hi“.](media/signalr-tutorial-build-blazor-server-chat-app/blazor-chat.gif) ](media/signalr-tutorial-build-blazor-server-chat-app/blazor-chat.gif#lightbox)
   
[Treten Probleme auf? Informieren Sie uns darüber.](https://aka.ms/asrs/qsblazor)

## <a name="publish-to-azure"></a>Veröffentlichen in Azure

Wir empfehlen die Verwendung von [Azure SignalR Service](/aspnet/core/signalr/scale#azure-signalr-service), um die Blazor-App für Azure App Service bereitzustellen. Azure SignalR Service ermöglicht das Hochskalieren einer Blazor Server-App auf eine hohe Anzahl gleichzeitiger SignalR-Verbindungen. Außerdem tragen die globale Reichweite und die Hochleistungsrechenzentren von SignalR Service erheblich zur Verringerung geografiebedingter Wartezeiten bei.

> [!IMPORTANT]
> In einer Blazor Server-App werden Benutzeroberflächenzustände serverseitig verwaltet. Zur Bewahrung des Zustands ist daher eine beständige Serversitzung erforderlich. Ist nur ein einzelner App-Server vorhanden, ist die Beständigkeit von Sitzungen standardmäßig gewährleistet. Bei mehreren App-Servern kann es jedoch vorkommen, dass für die Clientaushandlung und für die Verbindung jeweils unterschiedliche Server verwendet werden, was eine inkonsistente Verwaltung des Benutzeroberflächenzustands in einer Blazor-App zur Folge haben kann. Daher empfiehlt es sich, beständige Serversitzungen wie folgt in *appsettings.json* zu aktivieren:
>
> ```json
> "Azure:SignalR:ServerStickyMode": "Required"
> ```

1. Klicken Sie mit der rechten Maustaste auf das Projekt, und navigieren Sie zu **Veröffentlichen**. Verwenden Sie folgende Einstellungen:
   * **Ziel**: Azure
   * **Bestimmtes Ziel**: Alle Arten von **Azure App Service** werden unterstützt.
   * **App Service**: Erstellen Sie die App Service-Instanz, oder wählen Sie sie aus.

   [ ![Animation: Auswählen von „Azure“ als Ziel und „Azure App Service“ als bestimmtes Ziel](media/signalr-tutorial-build-blazor-server-chat-app/blazor-chat-profile.gif) ](media/signalr-tutorial-build-blazor-server-chat-app/blazor-chat-profile.gif#lightbox)

1. Fügen Sie die Azure SignalR Service-Abhängigkeit hinzu.

   Nach der Erstellung des Veröffentlichungsprofils wird unter **Dienstabhängigkeiten** eine Empfehlungsmeldung zum Hinzufügen des Azure SignalR-Diensts angezeigt. Wählen Sie **Konfigurieren** aus, um eine neue Instanz zu erstellen, oder wählen Sie eine bereits vorhandene Azure SignalR Service-Instanz aus.

   [ ![Hervorgehobener Konfigurationslink beim Veröffentlichen](media/signalr-tutorial-build-blazor-server-chat-app/blazor-chat-dependency.png) ](media/signalr-tutorial-build-blazor-server-chat-app/blazor-chat-dependency.png#lightbox)

   Von der Dienstabhängigkeit werden die folgenden Aktivitäten ausgeführt, damit Ihre App in Azure automatisch zu Azure SignalR Service wechseln kann:

   * Aktualisieren von [`HostingStartupAssembly`](/aspnet/core/fundamentals/host/platform-specific-configuration) für die Verwendung von Azure SignalR Service
   * Hinzufügen des Verweises auf das Azure SignalR Service-NuGet-Paket
   * Aktualisieren der Profileigenschaften zum Speichern der Abhängigkeitseinstellungen
   * Konfigurieren des Geheimnisspeichers Ihrer Wahl
   * Hinzufügen der Konfiguration in *appsettings.json*, um die Azure SignalR Service-Instanz als Ziel für Ihre App festzulegen

   [ ![In der Zusammenfassung der Änderungen werden mithilfe der Kontrollkästchen alle Abhängigkeiten ausgewählt.](media/signalr-tutorial-build-blazor-server-chat-app/blazor-chat-dependency-summary.png) ](media/signalr-tutorial-build-blazor-server-chat-app/blazor-chat-dependency-summary.png#lightbox)

1. Veröffentlichen Sie die App.

   Ihre App ist nun bereit für die Veröffentlichung. Nach Abschluss des Veröffentlichungsprozesses wird die App automatisch in einem Browser gestartet.
 
   > [!NOTE]
   > Aufgrund der Wartezeit beim Start der Azure App Service-Bereitstellung kann der Start der App etwas dauern. Sie können die Debuggertools des Browsers verwenden (in der Regel durch Drücken von <kbd>F12</kbd>), um sich zu vergewissern, dass der Datenverkehr an Azure SignalR Service umgeleitet wurde.

   [ ![Blazor-SignalR-Chatbeispiel mit einem Textfeld für Ihren Namen und einer mit „Chat!“ beschrifteten Schaltfläche zum Starten eines Chats](media/signalr-tutorial-build-blazor-server-chat-app/blazor-chat-azure.png) ](media/signalr-tutorial-build-blazor-server-chat-app/blazor-chat-azure.png#lightbox)
   
[Treten Probleme auf? Informieren Sie uns darüber.](https://aka.ms/asrs/qsblazor)

## <a name="enable-azure-signalr-service-for-local-development"></a>Aktivieren von Azure SignalR Service für die lokale Entwicklung

1. Fügen Sie mithilfe des folgenden Befehls einen Verweis auf das Azure SignalR SDK hinzu:

   ```dotnetcli
   dotnet add package Microsoft.Azure.SignalR --version 1.5.1
   ```

1. Fügen Sie in `Startup.ConfigureServices()` einen Aufruf von `AddAzureSingalR()` hinzu:

   ```cs
   public void ConfigureServices(IServiceCollection services)
   {
       ...
       services.AddSignalR().AddAzureSignalR();
       ...
   }
   ```

1. Konfigurieren Sie die Azure SignalR Service-Verbindungszeichenfolge – entweder in *appsettings.json* oder mit dem [Geheimnis-Manager](/aspnet/core/security/app-secrets?tabs=visual-studio#secret-manager).

> [!NOTE]
> Schritt 2 kann durch Konfigurieren von [Hostingstartassemblys](/aspnet/core/fundamentals/host/platform-specific-configuration) für die Verwendung des SignalR SDK ersetzt werden.
>
> 1. Fügen Sie in *appsettings.json* die Konfiguration zum Aktivieren von Azure SignalR Service hinzu:
>
>     ```json
>     "Azure": {
>       "SignalR": {
>         "Enabled": true,
>         "ConnectionString": <your-connection-string>       
>       }
>     }
>    
>    ```
>
> 1. Konfigurieren Sie die Hostingstartassembly für die Verwendung des Azure SignalR SDK. Bearbeiten Sie *launchSettings.json*, und fügen Sie innerhalb von `environmentVariables` eine Konfiguration wie die folgende hinzu:
>
>     ```json
>    "environmentVariables": {
>         ...,
>        "ASPNETCORE_HOSTINGSTARTUPASSEMBLIES": "Microsoft.Azure.SignalR"
>      }
>  
>     ```
>

[Treten Probleme auf? Informieren Sie uns darüber.](https://aka.ms/asrs/qsblazor)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Zum Bereinigen der im Rahmen dieses Tutorials erstellten Ressourcen löschen Sie die Ressourcengruppe über das Azure-Portal.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Einführung in ASP.NET Core Blazor](/aspnet/core/blazor)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines einfachen Chatrooms mit der Blazor Server-App-Vorlage
> * Verwenden von Razor-Komponenten
> * Verwenden von Ereignisbehandlung und Datenbindung in Razor-Komponenten
> * Schnelles Bereitstellen für Azure App Service in Visual Studio
> * Migrieren von der lokalen SignalR-Instanz zu Azure SignalR Service

Weitere Informationen zu Hochverfügbarkeit finden Sie hier:
> [!div class="nextstepaction"]
> [Resilienz und Notfallwiederherstellung](signalr-concept-disaster-recovery.md)
