---
title: .NET-Anwendungen mit mehreren Ebenen unter Verwendung von Azure Service Bus | Microsoft-Dokumentation
description: Ein .NET-Lernprogramm, das Ihnen hilft, eine Anwendung mit mehreren Ebenen in Azure zu erstellen, die Service Bus-Warteschlangen für die Kommunikation zwischen Ebenen verwendet.
ms.devlang: dotnet
ms.topic: article
ms.date: 04/30/2021
ms.custom: devx-track-csharp
ms.openlocfilehash: 38a1c975df578b32ec2d6cac9ff5c6ad4acb3687
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108759189"
---
# <a name="net-multi-tier-application-using-azure-service-bus-queues"></a>.NET-Anwendungen mit mehreren Ebenen unter Verwendung von Azure Service Bus-Warteschlangen

Mit Visual Studio und dem kostenlosen Azure SDK für .NET können Sie schnell und einfach für Microsoft Azure entwickeln. In diesem Tutorial werden Sie durch die Schritte zum Erstellen einer Anwendung geführt, von der mehrere in der lokalen Umgebung ausgeführte Azure-Ressourcen verwendet werden.

Sie lernen Folgendes:

* Vorbereitung Ihres Computers für die Azure-Entwicklung mit einem einzigen Download und einer einzigen Installation
* Einsatz von Visual Studio für die Azure-Entwicklung
* Erstellung von Anwendungen mit mehreren Ebenen in Azure mit Web- und Arbeiterrollen
* Kommunikation zwischen Ebenen mithilfe von Service Bus-Warteschlangen

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

In diesem Tutorial werden Sie eine mehrschichtige Anwendung in einem Azure-Clouddienst erstellen und ausführen. Als Front-End dient eine ASP.NET MVC-Webrolle, und als Back-End eine Workerrolle, die eine Service Bus-Warteschlange nutzt. Dieselbe Anwendung mit mehreren Ebenen kann auch mit einem Webprojekt als Front-End erstellt und auf einer Azure-Website anstelle eines Clouddiensts bereitgestellt werden. Sie können auch das Tutorial [Hybride lokale/Cloud-.NET-Anwendung](../azure-relay/service-bus-dotnet-hybrid-app-using-service-bus-relay.md) ausprobieren.

Der folgende Screenshot zeigt die fertige Anwendung.

:::image type="content" source="./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-app.png" alt-text="Seite „Übermitteln“ der Anwendung":::

## <a name="scenario-overview-inter-role-communication"></a>Szenario-Übersicht: Kommunikation zwischen Rollen
Um eine Bestellung zur Verarbeitung zu übermitteln, muss die Front-End-GUI in ihrer Funktion als Webrolle mit der Logikkomponente in der mittleren Ebene interagieren, die eine Workerrolle erfüllt. In diesem Beispiel wird Service Bus-Messaging für die Kommunikation zwischen Ebenen verwendet.

Durch die Nutzung von Service Bus-Messaging zwischen Web- und mittlerer Ebene werden die beiden Komponenten voneinander entkoppelt. Im Gegensatz zur direkten Kommunikation (z. B. per TCP oder HTTP) verbindet sich die Webebene nicht direkt mit der mittleren Ebene, sondern schiebt Arbeitseinheiten in Form von Nachrichten in Service Bus, wo diese zuverlässig aufbewahrt werden, bis die mittlere Ebene diese konsumieren und verarbeiten kann.

Service Bus bietet zwei Entitäten für das Brokermessaging: Warteschlangen und Themen. Mit Warteschlangen wird jede Nachricht von einem einzelnen Empfänger konsumiert. Themen unterstützen das Veröffentlichungs- und Abonnementmuster, mit dem jede veröffentlichte Nachricht den für das entsprechende Thema registrierten Abonnements zugänglich gemacht wird. Jedes Abonnement pflegt eine eigene Nachrichten-Warteschlange. Abonnements können mit Filterregeln konfiguriert werden. Diese sorgen dafür, dass nur Nachrichten in der Abonnement-Warteschlange landen, welche die Filterregeln erfüllen. Das folgende Beispiel verwendet Service Bus-Warteschlangen.

:::image type="content" source="./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-100.png" alt-text="Diagramm: Kommunikation zwischen Webrolle, Service Bus und Workerrolle":::

Dieser Kommunikationsmechanismus bietet verschiedene Vorteile gegenüber direkten Nachrichten:

* **Zeitliche Entkopplung:** Dank des asynchronen Nachrichtenmusters müssen Producer und Consumer nicht gleichzeitig online sein. Der Servicebus speichert die Nachrichten zuverlässig, bis der Consumer diese entgegennehmen kann. Auf diese Weise können die Komponenten verteilter Anwendungen voneinander entkoppelt werden, z. B. zu Wartungszwecken oder bei einem Komponentenausfall, ohne das Gesamtsystem zu beeinträchtigen. Außerdem genügt es unter Umständen, wenn die konsumierende Anwendung nur zu bestimmten Tageszeiten online ist.
* **Belastungsausgleich:** In vielen Anwendungen schwankt die Systemlast mit der Zeit, während die Bearbeitungszeit pro Arbeitseinheit normalerweise konstant ist. Durch die Einführung einer Warteschlange zwischen Nachrichtenproducer und Consumer muss der Consumer (Arbeiter) anstatt der Spitzenlast nur die durchschnittliche Last verarbeiten können. Die Tiefe der Warteschlange erhöht und verringert sich mit der eingehenden Last. Dies ermöglicht direkte Einsparungen bei der Infrastruktur, die zur Bearbeitung der Anwendungslast benötigt wird.
* **Lastenausgleich:** Mit zunehmender Last können zusätzliche Arbeitsprozesse zur Verarbeitung der Warteschlange eingesetzt werden. Jede Nachricht wird nur von einem der Arbeitsprozesse verarbeitet. Außerdem ermöglicht dieser entnahmebasierte Lastenausgleich eine optimale Auslastung der Workercomputer, selbst wenn sich deren Rechenleistung stark unterscheidet, da jeder Workercomputer die Nachrichten mit seinem eigenen Maximaldurchsatz aus der Warteschlange entnimmt. Dieses Muster nennt man auch *konkurrierende Consumer*.
  
    :::image type="content" source="./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-101.png" alt-text="Diagramm: Kommunikation zwischen Webrolle, Service Bus und zwei Workerrollen":::
  
In den folgenden Abschnitten wird der Code für die Implementierung dieser Architektur behandelt.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Tutorial erstellen Sie mithilfe der Azure Active Directory-Authentifizierung (Azure AD) `ServiceBusClient`- und `ServiceBusAdministrationClient`-Objekte. Sie verwenden außerdem `DefaultAzureCredential`. Dazu müssen Sie die folgenden Schritte ausführen, um die Anwendung lokal in einer Entwicklungsumgebung zu testen.

1. [Registrieren Sie eine Anwendung in Azure AD.](../active-directory/develop/quickstart-register-app.md)
1. [Fügen Sie die Anwendung der Rolle `Service Bus Data Owner` hinzu.](service-bus-managed-service-identity.md#to-assign-azure-roles-using-the-azure-portal)
1. Legen Sie die Umgebungsvariablen `AZURE-CLIENT-ID`, `AZURE-TENANT-ID` und `AZURE-CLIENT-SECRET` fest. Anweisungen finden Sie in [diesem Artikel](/dotnet/api/overview/azure/identity-readme#environment-variables).


## <a name="create-a-namespace"></a>Erstellen eines Namespace

Der erste Schritt besteht im Erstellen eines *Namespace* und Abrufen eines [SAS-Schlüssels](service-bus-sas.md) (Shared Access Signature) für diesen Namespace. Ein Namespace stellt eine Anwendungsgrenze für jede Anwendung bereit, die über Service Bus zur Verfügung steht. Das System generiert einen SAS-Schlüssel, wenn ein Namespace erstellt wird. Namespace-Name und SAS-Schlüssel bilden gemeinsam die Anmeldeinformationen, mit denen sich der Service Bus bei der Anwendung authentifiziert.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-web-role"></a>Erstellen einer Webrolle

In diesem Abschnitt lernen Sie, wie Sie das Front-End Ihrer Anwendung erstellen. Zunächst erstellen Sie die Seiten, aus denen Ihre Anwendung besteht.
Anschließend fügen Sie Code hinzu, mit dem Elemente an eine Service Bus-Warteschlange übermittelt und Statusinformationen zur Warteschlange angezeigt werden.

### <a name="create-the-project"></a>Erstellen des Projekts

1. Starten Sie Visual Studio mit Administratorrechten: Klicken Sie mit der rechten Maustaste auf das Programmsymbol von **Visual Studio**, und klicken Sie anschließend auf **Als Administrator ausführen**. Für den ebenfalls in diesem Artikel behandelten Azure-Serveremulator muss Visual Studio mit Administratorrechten gestartet werden.
   
   Klicken Sie in Visual Studio im Menü **Datei** auf **Neu** und dann auf **Projekt**.
2. Führen Sie auf der Seite **Vorlagen** die folgenden Schritte aus:
    1. Wählen Sie als Programmiersprache **C#** aus.
    1. Wählen Sie als Projekttyp **Cloud** aus.
    1. Wählen Sie **Azure-Clouddienst** aus.
    1. Wählen Sie **Weiter** aus. 
   
        :::image type="content" source="./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-10.png" alt-text="Screenshot: Dialogfeld „Neues Projekt“, in dem die Option „Cloud“ ausgewählt und „Azure-Clouddienst“/„Visual C#“ hervorgehoben und rot umrandet wurde":::
3.  Nennen Sie das Projekt **MultiTierApp**, und wählen Sie den Speicherort für das Projekt und dann **Erstellen** aus.

    :::image type="content" source="./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/project-name.png" alt-text="Angeben des Projektnamens":::    
1. Doppelklicken Sie auf der Seite **Rollen** auf **ASP.NET-Webrolle**, und wählen Sie **OK** aus. 
   
    :::image type="content" source="./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-11.png" alt-text="Auswählen von „Webrolle“":::
4. Zeigen Sie auf **WebRole1** unter **Azure-Clouddienst-Lösung**, klicken Sie auf das Stiftsymbol, und geben Sie der Webrolle den Namen **FrontendWebRole**. Klicken Sie dann auf **OK**. (Achten Sie darauf „Frontend“ mit kleinem „e“ einzugeben, nicht als „FrontEnd“.)
   
    :::image type="content" source="./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-02.png" alt-text="Screenshot: Dialogfeld „Neuer Cloud-Dienst in Microsoft Azure“, in dem die Lösung in „FrontendWebRole“ umbenannt wurde":::
5. Wählen Sie im Dialogfeld **Neue ASP.NET-Webanwendung erstellen** die Option **MVC** und dann **Erstellen** aus.
   
    :::image type="content" source="./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-12.png" alt-text="Screenshot des Dialogfelds „Neues ASP.NET-Projekt“, in dem „MVC“ hervorgehoben und rot umrandet und die Option „Authentifizierung ändern“ rot umrandet wurde":::
8. Klicken Sie im **Projektmappen-Explorer** im Projekt **FrontendWebRole** mit der rechten Maustaste auf **Verweise**, und klicken Sie dann auf **NuGet-Pakete verwalten**.
9. Klicken Sie auf die Registerkarte **Durchsuchen**, und suchen Sie nach **Azure.Messaging.ServiceBus**. Wählen Sie das Paket **Azure.Messaging.ServiceBus** und dann **Installieren** aus, und akzeptieren Sie die Nutzungsbedingungen.
   
    :::image type="content" source="./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-13.png" alt-text="Screenshot des Dialogfelds „NuGet-Pakete verwalten“, in dem „Azure.Messaging.ServiceBus“ hervorgehoben und die Option „Installieren“ rot umrandet wurde":::

   Beachten Sie, dass die benötigten Clientassemblys nun referenziert sind und einige neue Codedateien hinzugefügt wurden.
10. Führen Sie die gleichen Schritte aus, um dem Projekt das NuGet-Paket `Azure.Identity` hinzuzufügen.  
10. Erweitern Sie im **Projektmappen-Explorer** den Eintrag **FrontendWebRole**, klicken Sie mit der rechten Maustaste auf **Modelle**, und klicken Sie anschließend auf **Hinzufügen** und auf **Klasse**. Geben Sie in das Feld **Name** den Namen **OnlineOrder.cs** ein. Klicken Sie anschließend auf **Hinzufügen**.

### <a name="write-the-code-for-your-web-role"></a>Schreiben des Codes für Ihre Webrolle
In diesem Abschnitt erstellen Sie die verschiedenen Seiten, aus denen Ihre Anwendung besteht.

1. Ersetzen Sie in der Datei "OnlineOrder.cs" in Visual Studio die bestehende Namespacedefinition durch den folgenden Code:
   
   ```csharp
   namespace FrontendWebRole.Models
   {
       public class OnlineOrder
       {
           public string Customer { get; set; }
           public string Product { get; set; }
       }
   }
   ```
2. Doppelklicken Sie im **Projektmappen-Explorer** auf **Controllers\HomeController.cs**. Fügen Sie die folgenden **using**-Anweisungen am Anfang Ihrer Datei hinzu, um die Namespaces für Ihr neu erstelltes Modell sowie Service Bus einzuschließen.
   
   ```csharp
    using FrontendWebRole.Models;
    using Azure.Messaging.ServiceBus;    
   ```
3. Ersetzen Sie in der Datei "HomeController.cs" in Visual Studio die bestehende Namespacedefinition ebenfalls durch den folgenden Code. Dieser Code enthält die Methoden für die Übermittlung von Elementen in die Warteschlange.
   
   ```csharp
   namespace FrontendWebRole.Controllers
   {
       public class HomeController : Controller
       {
           public ActionResult Index()
           {
               // Simply redirect to Submit, since Submit will serve as the
               // front page of this application.
               return RedirectToAction("Submit");
           }
   
           public ActionResult About()
           {
               return View();
           }
   
           // GET: /Home/Submit.
           // Controller method for a view you will create for the submission
           // form.
           public ActionResult Submit()
           {
               // Will put code for displaying queue message count here.
   
               return View();
           }
   
           // POST: /Home/Submit.
           // Controller method for handling submissions from the submission
           // form.
           [HttpPost]
           // Attribute to help prevent cross-site scripting attacks and
           // cross-site request forgery.  
           [ValidateAntiForgeryToken]
           public ActionResult Submit(OnlineOrder order)
           {
               if (ModelState.IsValid)
               {
                   // Will put code for submitting to queue here.
   
                   return RedirectToAction("Submit");
               }
               else
               {
                   return View(order);
               }
           }
       }
   }
   ```
4. Klicken Sie im Menü **Build** auf **Projektmappe erstellen**, um die Richtigkeit Ihrer bisherigen Arbeit zu überprüfen.
5. Erstellen Sie anschließend die Ansicht für die zuvor erstellte `Submit()`-Methode. Klicken Sie mit der rechten Maustaste in die `Submit()`-Methode (Überladung von `Submit()` ohne Parameter), und wählen Sie in der Datei **HomeController.cs** die Option **Ansicht hinzufügen** aus.
6. Wählen Sie im Dialogfeld **Neues Gerüstelement hinzufügen** die Option **Hinzufügen** aus. 
1. Führen Sie im Dialogfeld **Ansicht hinzufügen** die folgenden Schritte aus:
    1. Wählen Sie in der Liste **Vorlage** die Option **Erstellen** aus. 
    1. Wählen Sie in der Liste **Modellklasse** die **OnlineOrder**-Klasse aus.
    1. Wählen Sie **Hinzufügen**. 
   
        :::image type="content" source="./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-34.png" alt-text="Screenshot: Dialogfeld „Ansicht hinzufügen“, in dem die Dropdownlisten „Vorlage“ und „Modell“ rot umrandet wurden":::
8. Ändern Sie nun den angezeigten Namen Ihrer Anwendung. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei **Views\Shared\\_Layout.cshtml**, um sie im Visual Studio-Editor zu öffnen.
9. Ersetzen Sie alle Vorkommnisse von **My ASP.NET Application** durch **Northwind Traders Products**.
10. Entfernen Sie die Links **Home**, **About** und **Contact**. Löschen Sie den hervorgehobenen Code:
    
    :::image type="content" source="./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-40.png" alt-text="Screenshot: Code mit drei hervorgehobenen Zeilen vom Typ „Html.ActionLink“":::
11. Erweitern Sie anschließend die Übermittlungsseite um einige Informationen zur Warteschlange. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei **Views\Home\Submit.cshtml**, um sie im Visual Studio-Editor zu öffnen. Fügen Sie nach `<h2>Submit</h2>` die folgende Zeile hinzu. Derzeit ist `ViewBag.MessageCount` leer. Sie werden diesen Bereich später ausfüllen.
    
    ```html
    <p>Current number of orders in queue waiting to be processed: @ViewBag.MessageCount</p>
    ```
12. Sie haben nun Ihre GUI implementiert. Drücken Sie **F5** , um Ihre Anwendung auszuführen und zu prüfen, ob diese korrekt angezeigt wird.
    
    :::image type="content" source="./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-app.png" alt-text="Screenshot: Übermittlungsseite der Anwendung":::

### <a name="write-the-code-for-submitting-items-to-a-service-bus-queue"></a>Schreiben des Codes für die Übermittlung von Elementen an die Service Bus-Warteschlange
Sie fügen nun den Code für die Übermittlung von Elementen in die Warteschlange hinzu. Zuerst erstellen Sie die Klasse mit den Verbindungsinformationen für die Service Bus-Warteschlange. Anschließend initialisieren Sie Ihre Verbindung in „Global.aspx.cs“. Zuletzt aktualisieren Sie den zuvor in „HomeController.cs“ erstellten Übermittlungscode, um die Elemente an die Service Bus-Warteschlange zu übermitteln.

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **FrontendWebRole** (klicken Sie auf das Projekt, nicht auf die Rolle). Klicken Sie auf **Hinzufügen** und anschließend auf **Klasse**.
2. Geben Sie der Klasse den Namen **QueueConnector.cs**. Klicken Sie auf **Hinzufügen**, um die Klasse zu erstellen.
3. Sie fügen nun den Code hinzu, der Ihre Verbindungsinformationen kapselt und die Verbindung zur Service Bus-Warteschlange initialisiert. Ersetzen Sie den gesamten Inhalt von „QueueConnector.cs“ durch den folgenden Code, und geben Sie Werte für `your Service Bus namespace` (Ihr Namespacename) und `yourKey` ein. Letzterer ist der **Primärschlüssel**, den Sie über das Azure-Portal abgerufen haben.
   
   ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Threading.Tasks;
    using Azure.Messaging.ServiceBus;
    using Azure.Messaging.ServiceBus.Administration;
       
   namespace FrontendWebRole
   {
        public static class QueueConnector
        {
            // object to send messages to a Service Bus queue
            internal static ServiceBusSender SBSender;
    
            // object to create a queue and get runtime properties (like message count) of queue
            internal static ServiceBusAdministrationClient SBAdminClient;
        
            // Fully qualified Service Bus namespace
            private const string FullyQualifiedNamespace = "<SERVICE BUS NAMESPACE NAME>.servicebus.windows.net";
            
            // The name of your queue.
            internal const string QueueName = "OrdersQueue";
        
            public static async Task Initialize()
            {
                // Create a Service Bus client that you can use to send or receive messages
                ServiceBusClient SBClient = new ServiceBusClient(FullyQualifiedNamespace, new DefaultAzureCredential());
        
                // Create a Service Bus admin client to create queue if it doesn't exist or to get message count
                SBAdminClient = new ServiceBusAdministrationClient(FullyQualifiedNamespace, new DefaultAzureCredential());
        
                // create the OrdersQueue if it doesn't exist already
                if (!(await SBAdminClient.QueueExistsAsync(QueueName)))
                {
                    await SBAdminClient.CreateQueueAsync(QueueName);
                }
        
                // create a sender for the queue 
                SBSender = SBClient.CreateSender(QueueName);    
            }
        }    
   }
   ```
4. Stellen Sie nun sicher, dass Ihre **Initialize**-Methode aufgerufen wird. Doppelklicken Sie im **Projektmappen-Explorer** auf **Global.asax\Global.asax.cs**.
5. Fügen Sie am Ende der **Application_Start**-Methode die folgende Codezeile hinzu.
   
   ```csharp
    FrontendWebRole.QueueConnector.Initialize().Wait();
   ```
6. Zuletzt aktualisieren Sie den zuvor erstellten Webcode, um die Elemente an die Warteschlange zu übermitteln. Doppelklicken Sie im **Projektmappen-Explorer** auf **Controllers\HomeController.cs**.
7. Aktualisieren Sie die `Submit()`-Methode (Überladung ohne Parameter) wie folgt, um die Nachrichtenanzahl für die Warteschlange abzurufen.
   
   ```csharp
        public ActionResult Submit()
        {
            QueueRuntimeProperties properties = QueueConnector.adminClient.GetQueueRuntimePropertiesAsync(QueueConnector.queueName).Result;
            ViewBag.MessageCount = properties.ActiveMessageCount;

            return View();
        }
   ```
8. Aktualisieren Sie die `Submit(OnlineOrder order)`-Methode (Überladung mit einem Parameter) wie folgt, um Bestellinformationen an die Warteschlange zu übermitteln.
   
   ```csharp
        public ActionResult Submit(OnlineOrder order)
        {
            if (ModelState.IsValid)
            {
                // create a message 
                var message = new ServiceBusMessage(new BinaryData(order));

                // send the message to the queue
                QueueConnector.sbSender.SendMessageAsync(message);

                return RedirectToAction("Submit");
            }
            else
            {
                return View(order);
            }
        }
   ```
9. Führen Sie die Anwendung nun erneut aus. Bei jeder Übermittlung einer Bestellung steigt der Nachrichtenzähler an.
   
    :::image type="content" source="./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-app2.png" alt-text="Screenshot: Übermittlungsseite der Anwendung, auf der sich die Nachrichtenanzahl auf „1“ erhöht hat":::

## <a name="create-the-worker-role"></a>Erstellen einer Workerrolle
Sie werden nun die Workerrolle zur Verarbeitung der übermittelten Nachrichten erstellen. In diesem Beispiel wird die Visual Studio-Projektvorlage **Workerrolle mit Service Bus-Warteschlange** verwendet. Sie haben die erforderlichen Anmeldeinformationen bereits aus dem Portal abgerufen.

1. Stellen Sie sicher, dass Sie Visual Studio mit Ihrem Azure-Konto verbunden haben.
2. Klicken Sie in Visual Studio im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner **Rollen** im Projekt **MultiTierApp**.
3. Klicken Sie auf **Hinzufügen**, und klicken Sie dann auf **Neues Workerrollenprojekt**. Das Dialogfeld **Neues Rollenprojekt hinzufügen** wird geöffnet.

   :::image type="content" source="./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBNewWorkerRole.png" alt-text="Screenshot des Projektmappen-Explorers mit hervorgehobenen Optionen „Neues Workerrollenprojekt“ und „Hinzufügen“":::
1. Wählen Sie im Dialogfeld **Neues Rollenprojekt hinzufügen** die Option **Workerrolle** aus. Wählen Sie nicht **Workerrolle mit Service Bus-Warteschlange** aus, da damit Code generiert wird, der das Legacy-SDK von Service Bus verwendet. 
   
    :::image type="content" source="./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRole1.png" alt-text="Screenshot: Dialogfeld „Neues Rollenprojekt hinzufügen“ mit hervorgehobener und rot umrandeter Option „Workerrolle mit Service Bus-Warteschlange“":::
5. Geben Sie im dem Projekt im Feld **Name** den Namen **OrderProcessingRole**. Klicken Sie anschließend auf **Hinzufügen**.
1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **OrderProcessingRole**, und wählen Sie **NuGet-Pakete verwalten** aus.
9. Wählen Sie die Registerkarte **Durchsuchen** aus, und suchen Sie nach **Azure.Messaging.ServiceBus**. Wählen Sie das Paket **Azure.Messaging.ServiceBus** und dann **Installieren** aus, und akzeptieren Sie die Nutzungsbedingungen.
   
    :::image type="content" source="./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-13.png" alt-text="Screenshot des Dialogfelds „NuGet-Pakete verwalten“, in dem „Azure.Messaging.ServiceBus“ hervorgehoben und die Option „Installieren“ rot umrandet wurde":::
1. Führen Sie die gleichen Schritte aus, um dem Projekt das NuGet-Paket `Azure.Identity` hinzuzufügen.  
1. Erstellen Sie die **OnlineOrder**-Klasse, um die Nachrichten abzubilden, während diese aus der Warteschlange verarbeitet werden. Sie können dabei eine zuvor erstellte Klasse wiederverwenden. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die **OrderProcessingRole**-Klasse (Rechtsklick auf die Klasse, nicht die Rolle). Klicken Sie auf **Hinzufügen** und dann auf **Vorhandenes Element**.
1. Durchsuchen Sie den Unterordner nach **FrontendWebRole\Models**, und doppelklicken Sie dann auf **OnlineOrder.cs**, um die Klasse dem Projekt hinzuzufügen.
1. Fügen Sie in der Datei **WorkerRole.cs** im **Projekt OrderProcessingRole** die folgende `using`-Anweisung hinzu. 

    ```csharp
    using FrontendWebRole.Models;
    using Azure.Messaging.ServiceBus;
    using Azure.Messaging.ServiceBus.Administration; 
    ```    
1. Fügen Sie in **WorkerRole.cs** die folgenden Eigenschaften hinzu. 

    > [!IMPORTANT]
    > Verwenden Sie die Verbindungszeichenfolge für den Namespace, den Sie sich als Teil der Voraussetzungen notiert haben. 

    ```csharp
        // Fully qualified Service Bus namespace
        private const string FullyQualifiedNamespace = "<SERVICE BUS NAMESPACE NAME>.servicebus.windows.net";

        // The name of your queue.
        private const string QueueName = "OrdersQueue";

        // Service Bus Receiver object to receive messages message the specific queue
        private ServiceBusReceiver SBReceiver;

    ```
1. Aktualisieren Sie die `OnStart`-Methode, um ein `ServiceBusClient`-Objekt und dann ein `ServiceBusReceiver`-Objekt zum Empfangen von Nachrichten von `OrdersQueue` zu erstellen. 
    
    ```csharp
        public override bool OnStart()
        {
            // Create a Service Bus client that you can use to send or receive messages
            ServiceBusClient SBClient = new ServiceBusClient(FullyQualifiedNamespace, new DefaultAzureCredential());

            CreateQueue(QueueName).Wait();

            // create a receiver that we can use to receive the message
            SBReceiver = SBClient.CreateReceiver(QueueName);

            return base.OnStart();
        }
        private async Task CreateQueue(string queueName)
        {
            // Create a Service Bus admin client to create queue if it doesn't exist or to get message count
            ServiceBusAdministrationClient SBAdminClient = new ServiceBusAdministrationClient(FullyQualifiedNamespace, new DefaultAzureCredential());

            // create the OrdersQueue if it doesn't exist already
            if (!(await SBAdminClient.QueueExistsAsync(queueName)))
            {
                await SBAdminClient.CreateQueueAsync(queueName);
            }
        }
    ```
12. Aktualisieren Sie die `RunAsync`-Methode so, dass sie den Code zum Empfangen von Nachrichten enthält. 

    ```csharp
        private async Task RunAsync(CancellationToken cancellationToken)
        {
            // TODO: Replace the following with your own logic.
            while (!cancellationToken.IsCancellationRequested)
            {
                // receive message from the queue
                ServiceBusReceivedMessage receivedMessage = await SBReceiver.ReceiveMessageAsync();

                if (receivedMessage != null)
                {
                    Trace.WriteLine("Processing", receivedMessage.SequenceNumber.ToString());

                    // view the message as an OnlineOrder
                    OnlineOrder order = receivedMessage.Body.ToObjectFromJson<OnlineOrder>();
                    Trace.WriteLine(order.Customer + ": " + order.Product, "ProcessingMessage");

                    // complete message so that it's removed from the queue
                    await SBReceiver.CompleteMessageAsync(receivedMessage);
                }
            }
        }
    ```
14. Ihre Anwendung ist nun fertig. Sie können die vollständige Anwendung testen, indem Sie im Projektmappen-Explorer mit der rechten Maustaste auf das MultiTierApp-Projekt klicken und dann **Set as Startup Project** auswählen und F5 drücken. Beachten Sie, dass die Nachrichtenzahl nicht ansteigt, da die Workerrolle die Elemente in der Warteschlange verarbeitet und als abgeschlossen markiert. Sie können die Ausgabe Ihrer Workerrolle in der Azure-Serveremulator-GUI anzeigen. Klicken Sie dazu mit der rechten Maustaste in den Infobereich Ihrer Taskleiste, und wählen Sie **Serveremulator-GUI anzeigen**.
    
    :::image type="content" source="./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-38.png" alt-text="Screenshot der Anzeige nach dem Klicken auf das Emulatorsymbol. „Serveremulator-Benutzeroberfläche anzeigen“ ist in der Liste der Optionen enthalten.":::
    
    :::image type="content" source="./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-39.png" alt-text="Screenshot: Dialogfeld „Microsoft Azure-Serveremulator (Express)“":::

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Servicebus finden Sie in den folgenden Ressourcen:  

* [Erste Schritte mit der Verwendung von Service Bus-Warteschlangen][sbacomqhowto]
* [Service Bus-Dienstseite][sbacom]  

Weitere Informationen zu Szenarien mit mehreren Ebenen finden Sie unter:  

* [.NET-Anwendungen mit mehreren Ebenen mithilfe von Speichertabellen, Warteschlangen und Blobs][mutitierstorage]  



[sbacom]: https://azure.microsoft.com/services/service-bus/  
[sbacomqhowto]: service-bus-dotnet-get-started-with-queues.md  
[mutitierstorage]: https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36
