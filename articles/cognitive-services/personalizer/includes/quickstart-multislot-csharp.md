---
title: Datei einfügen
description: Datei einfügen
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: include
ms.custom: cog-serv-seo-aug-2020
ms.date: 03/23/2021
ms.openlocfilehash: e21dab310c41cf6aae0e201d7d1b8e78a8540a30
ms.sourcegitcommit: f3b930eeacdaebe5a5f25471bc10014a36e52e5e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/16/2021
ms.locfileid: "112255495"
---
[Referenzdokumentation](/dotnet/api/Microsoft.Azure.CognitiveServices.Personalizer) | [Multi-Slot-Konzept](..\concept-multi-slot-personalization.md) | [Beispiele](https://aka.ms/personalizer/ms-dotnet)

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/cognitive-services)
* Aktuelle Version von [.NET Core](https://dotnet.microsoft.com/download/dotnet-core).
* Sobald Sie über Ihr Azure-Abonnement verfügen, sollten Sie über <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesPersonalizer"  title="Erstellen einer Personalisierungsressource"  target="_blank"> im Azure-Portal eine Personalisierungsressource </a> erstellen, um Ihren Schlüssel und Endpunkt abzurufen. Klicken Sie nach Abschluss der Bereitstellung auf **Zu Ressource wechseln**.
    * Sie benötigen den Schlüssel und Endpunkt der von Ihnen erstellten Ressource, um Ihre Anwendung mit der Personalisierungs-API zu verbinden. Der Schlüssel und der Endpunkt werden weiter unten in der Schnellstartanleitung in den Code eingefügt.
    * Sie können den kostenlosen Tarif (`F0`) verwenden, um den Dienst zu testen, und später für die Produktion auf einen kostenpflichtigen Tarif upgraden.

## <a name="setting-up"></a>Einrichten

[!INCLUDE [Upgrade Personalizer instance to multi-slot](upgrade-personalizer-multi-slot.md)]

[!INCLUDE [Change model frequency](change-model-frequency.md)]

[!INCLUDE [Change reward wait time](change-reward-wait-time.md)]

### <a name="create-a-new-c-application"></a>Erstellen einer neuen C#-Anwendung

Erstellen Sie eine neue .NET Core-Anwendung in Ihrem bevorzugten Editor oder Ihrer bevorzugten IDE.

Verwenden Sie in einem Konsolenfenster (z. B. cmd, PowerShell oder Bash) den Befehl dotnet `new` zum Erstellen einer neuen Konsolen-App mit dem Namen `personalizer-quickstart`. Dieser Befehl erstellt ein einfaches „Hallo Welt“-C#-Projekt mit einer einzigen Quelldatei: `Program.cs`.

```console
dotnet new console -n personalizer-quickstart
```

Wechseln Sie zum Ordner der neu erstellten App. Sie können die Anwendung mit folgendem Befehl erstellen:

```console
dotnet build
```

Die Buildausgabe sollte keine Warnungen oder Fehler enthalten.

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

Öffnen Sie über das Projektverzeichnis die Datei `Program.cs` in Ihrem bevorzugten Editor oder in Ihrer bevorzugten IDE. Fügen Sie die folgenden using-Direktiven hinzu:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Text;
using System.Text.Json;
using System.Text.Json.Serialization;
using System.Threading.Tasks;
```

## <a name="object-model"></a>Objektmodell

Um das beste einzelne Element des Inhalts für jeden Slot zu erfragen, erstellen Sie eine **MultiSlotRankRequest**, und senden Sie dann eine Postanforderung an [multislot/rank](https://westus2.dev.cognitive.microsoft.com/docs/services/personalizer-api-v1-1-preview-1/operations/MultiSlot_Rank). Die Antwort wird dann in eine **MultiSlotRankResponse** analysiert.

Um eine Relevanzbewertung an die Personalisierung zu senden, erstellen Sie eine **MultiSlotReward** und senden dann eine Postanforderung an [multislot/events/{eventId}/reward](https://westus2.dev.cognitive.microsoft.com/docs/services/personalizer-api-v1-1-preview-1/operations/MultiSlot_Events_Reward).

Im Rahmen dieser Schnellstartanleitung ist die Bestimmung der Relevanzbewertung ganz einfach. In einem Produktionssystem kann die Bestimmung der Einflussfaktoren für die [Relevanzbewertung](../concept-rewards.md) sowie deren jeweilige Gewichtung allerdings eine komplexe Angelegenheit sein und muss unter Umständen im Laufe der Zeit überarbeitet werden. Diese Entwurfsentscheidung sollte eine der wichtigsten Entscheidungen im Zusammenhang mit Ihrer Personalisierungsarchitektur sein.

## <a name="code-examples"></a>Codebeispiele

In diesen Codeausschnitten wird gezeigt, wie Sie die folgenden Aufgaben ausführen, indem Sie HTTP-Anforderungen für .NET senden:

* [Erstellen von Basis-URLs](#create-base-urls)
* [Rangfolge-API mit mehreren Slots](#request-the-best-action)
* [Belohnungs-API mit mehreren Slots](#send-a-reward)


## <a name="create-base-urls"></a>Erstellen von Basis-URLs

In diesem Abschnitt führen Sie zwei Aktionen aus:
* Angeben Ihres Schlüssels und Endpunkts
* Erstellen der Rangfolge- und Belohnungs-URLs

Fügen Sie zunächst der Program-Klasse die unten angegebenen Zeilen hinzu. Fügen Sie unbedingt den Schlüssel und Endpunkt Ihrer Personalisierungsressource hinzu.

[!INCLUDE [Personalizer find resource info](find-azure-resource-info.md)]

```csharp
//Replace 'PersonalizationBaseUrl' and 'ResourceKey' with your valid endpoint values.
private const string PersonalizationBaseUrl = "<REPLACE-WITH-YOUR-PERSONALIZER-ENDPOINT>";
private const string ResourceKey = "<REPLACE-WITH-YOUR-PERSONALIZER-KEY>";
```

Erstellen Sie als Nächstes die Rangfolge- und Belohnungs-URLs.

```csharp
private static string MultiSlotRankUrl = string.Concat(PersonalizationBaseUrl, "personalizer/v1.1-preview.1/multislot/rank");
private static string MultiSlotRewardUrlBase = string.Concat(PersonalizationBaseUrl, "personalizer/v1.1-preview.1/multislot/events/");
```

## <a name="get-content-choices-represented-as-actions"></a>Abrufen von Inhaltsoptionen, dargestellt als Aktionen

Aktionen stellen die Inhaltsoptionen dar, aus denen die Personalisierung das beste Inhaltselement auswählen soll. Fügen Sie der Klasse „Program“ die folgenden Methoden hinzu, um die Gruppe der Aktionen und deren Features darzustellen. 

```csharp
private static IList<Action> GetActions()
{
    IList<Action> actions = new List<Action>
    {
        new Action
        {
            Id = "Red-Polo-Shirt-432",
            Features =
            new List<object>() { new { onSale = "true", price = "20", category = "Clothing" } }
        },
        new Action
        {
            Id = "Tennis-Racket-133",
            Features =
            new List<object>() { new { onSale = "false", price = "70", category = "Sports" } }
        },
        new Action
        {
            Id = "31-Inch-Monitor-771",
            Features =
            new List<object>() { new { onSale = "true", price = "200", category = "Electronics" } }
        },
        new Action
        {
            Id = "XBox-Series X-117",
            Features =
            new List<object>() { new { onSale = "false", price = "499", category = "Electronics" } }
        }
    };
    return actions;
}
```

## <a name="get-slots"></a>Slots abrufen

Slots machen die Seite aus, mit der der Benutzer interagiert. Personalizer entscheidet, welche Aktion in jedem der definierten Slots angezeigt werden soll. Aktionen können aus bestimmten Slots ausgeschlossen werden, die als `ExcludeActions` angezeigt werden. `BaselineAction` ist die Standardaktion für den Slot, die ohne die Verwendung von Personalizer angezeigt worden wäre.


Diese Schnellstartanleitung enthält einfache Slotfeatures. In Produktionssystemen kann die Bestimmung und [Auswertung](../concept-feature-evaluation.md) von [Merkmalen](../concepts-features.md) allerdings eine komplexe Angelegenheit sein.

```csharp
private static IList<Slot> GetSlots()
{
    IList<Slot> slots = new List<Slot>
    {
        new Slot
        {
            Id = "BigHeroPosition",
            Features = new List<object>() { new { size = "large", position = "left" } },
            ExcludedActions = new List<string>() { "31-Inch-Monitor-771" },
            BaselineAction = "Red-Polo-Shirt-432"

        },
        new Slot
        {
            Id = "SmallSidebar",
            Features = new List<object>() { new { size = "small", position = "right" } },
            ExcludedActions = new List<string>() { "Tennis-Racket-133" },
            BaselineAction = "XBox-Series X-117"
        },
    };

    return slots;
}
```

## <a name="classes-for-constructing-rankreward-requests-and-responses"></a>Klassen zum Erstellen von Rangfolge-/Belohnungsanforderungen und -antworten

Fügen Sie die folgenden geschachtelten Klassen hinzu, die zum Erstellen der Rangfolge-/Belohnungsanforderungen und Analysieren ihrer Antworten verwendet werden.

```csharp
private class Action
{
    [JsonPropertyName("id")]
    public string Id { get; set; }

    [JsonPropertyName("features")]
    public object Features { get; set; }
}
```

```csharp
private class Slot
{
    [JsonPropertyName("id")]
    public string Id { get; set; }

    [JsonPropertyName("features")]
    public List<object> Features { get; set; }

    [JsonPropertyName("excludedActions")]
    public List<string> ExcludedActions { get; set; }

    [JsonPropertyName("baselineAction")]
    public string BaselineAction { get; set; }
}
```

```csharp
private class Context
{
    [JsonPropertyName("features")]
    public object Features { get; set; }
}
```

```csharp
private class MultiSlotRankRequest
{
    [JsonPropertyName("contextFeatures")]
    public IList<Context> ContextFeatures { get; set; }

    [JsonPropertyName("actions")]
    public IList<Action> Actions { get; set; }

    [JsonPropertyName("slots")]
    public IList<Slot> Slots { get; set; }

    [JsonPropertyName("eventId")]
    public string EventId { get; set; }

    [JsonPropertyName("deferActivation")]
    public bool DeferActivation { get; set; }
}
```

```csharp
private class MultiSlotRankResponse
{
    [JsonPropertyName("slots")]
    public IList<SlotResponse> Slots { get; set; }

    [JsonPropertyName("eventId")]
    public string EventId { get; set; }
}
```

```csharp
private class SlotResponse
{
    [JsonPropertyName("id")]
    public string Id { get; set; }

    [JsonPropertyName("rewardActionId")]
    public string RewardActionId { get; set; }
}
```

```csharp
private class MultiSlotReward
{
    [JsonPropertyName("reward")]
    public List<SlotReward> Reward { get; set; }
}
```

```csharp
private class SlotReward
{
    [JsonPropertyName("slotId")]
    public string SlotId { get; set; }

    [JsonPropertyName("value")]
    public float Value { get; set; }
}
```

## <a name="get-user-preferences-for-context"></a>Abrufen von Benutzerwünschen für den Kontext

Fügen Sie der Program-Klasse die folgenden Methoden hinzu, um die Eingabe eines Benutzers über die Befehlszeile für die Tageszeit und den Gerätetyp des Benutzers zu erhalten. Diese werden als Kontextfeatures verwendet.

```csharp
static string GetTimeOfDayForContext()
{
    string[] timeOfDayFeatures = new string[] { "morning", "afternoon", "evening", "night" };

    Console.WriteLine("\nWhat time of day is it (enter number)? 1. morning 2. afternoon 3. evening 4. night");
    if (!int.TryParse(GetKey(), out int timeIndex) || timeIndex < 1 || timeIndex > timeOfDayFeatures.Length)
    {
        Console.WriteLine("\nEntered value is invalid. Setting feature value to " + timeOfDayFeatures[0] + ".");
        timeIndex = 1;
    }

    return timeOfDayFeatures[timeIndex - 1];
}
```

```csharp
static string GetDeviceForContext()
{
    string[] deviceFeatures = new string[] { "mobile", "tablet", "desktop" };

    Console.WriteLine("\nWhat is the device type (enter number)? 1. Mobile 2. Tablet 3. Desktop");
    if (!int.TryParse(GetKey(), out int deviceIndex) || deviceIndex < 1 || deviceIndex > deviceFeatures.Length)
    {
        Console.WriteLine("\nEntered value is invalid. Setting feature value to " + deviceFeatures[0] + ".");
        deviceIndex = 1;
    }

    return deviceFeatures[deviceIndex - 1];
}
```

Bei beiden Methoden wird die Methode `GetKey` verwendet, um die Auswahl des Benutzers aus der Befehlszeile zu lesen.

```csharp
private static string GetKey()
{
    return Console.ReadKey().Key.ToString().Last().ToString().ToUpper();
}
```

```csharp
private static IList<Context> GetContext(string time, string device)
{
    IList<Context> context = new List<Context>
    {
        new Context
        {
            Features = new {timeOfDay = time, device = device }
        }
    };

    return context;
}
```

## <a name="make-http-requests"></a>Übermitteln von HTTP-Anforderungen

Fügen Sie diese Funktionen hinzu, um Postanforderungen für Rangfolge- und Relevanzaufrufe mit mehreren Slots an den Personalizer-Endpunkt zu senden.

```csharp
private static async Task<MultiSlotRankResponse> SendMultiSlotRank(HttpClient client, string rankRequestBody, string rankUrl)
{
    try
    {
    var rankBuilder = new UriBuilder(new Uri(rankUrl));
    HttpRequestMessage rankRequest = new HttpRequestMessage(HttpMethod.Post, rankBuilder.Uri);
    rankRequest.Content = new StringContent(rankRequestBody, Encoding.UTF8, "application/json");
    HttpResponseMessage response = await client.SendAsync(rankRequest);
    response.EnsureSuccessStatusCode();
    MultiSlotRankResponse rankResponse = JsonSerializer.Deserialize<MultiSlotRankResponse>(await response.Content.ReadAsByteArrayAsync());
    return rankResponse;
    }
    catch (Exception e)
    {
        Console.WriteLine("\n" + e.Message);
        Console.WriteLine("Please make sure multi-slot feature is enabled. To do so, follow multi-slot Personalizer documentation to update your loop settings to enable multi-slot functionality.");
        throw;
    }
}
```

```csharp
private static async Task SendMultiSlotReward(HttpClient client, string rewardRequestBody, string rewardUrlBase, string eventId)
{
    string rewardUrl = String.Concat(rewardUrlBase, eventId, "/reward");
    var rewardBuilder = new UriBuilder(new Uri(rewardUrl));
    HttpRequestMessage rewardRequest = new HttpRequestMessage(HttpMethod.Post, rewardBuilder.Uri);
    rewardRequest.Content = new StringContent(rewardRequestBody, Encoding.UTF8, "application/json");

    await client.SendAsync(rewardRequest);
}
```

## <a name="create-the-learning-loop"></a>Erstellen der Lernschleife

Die Lernschleife der Personalisierung ist ein Zyklus aus [MultiSlotRank](#request-the-best-action)- und [MultiSlotReward](#send-a-reward)-Aufrufen. In dieser Schnellstartanleitung folgt auf jeden Rangfolgeaufruf zur Personalisierung des Inhalts ein Relevanzaufruf, um der Personalisierung mitzuteilen, wie gut der Dienst abgeschnitten hat.

Der folgende Code durchläuft einen Zyklus durch eine Schleife: Der Benutzer wird an der Befehlszeile nach seinen Präferenzen befragt, die Angaben werden zur Wahl der besten Aktion für jeden Slot an die Personalisierung gesendet, die Auswahl wird dem Kunden in einer Auswahlliste angezeigt, und anschließend wird eine Relevanzbewertung an die Personalisierung gesendet, die angibt, wie gut die Wahl des Diensts war.

```csharp
static async Task Main(string[] args)
{
    Console.WriteLine($"Welcome to this Personalizer Quickstart!\n" +
            $"This code will help you understand how to use the Personalizer APIs (multislot rank and multislot reward).\n" +
            $"Each iteration represents a user interaction and will demonstrate how context, actions, slots, and rewards work.\n" +
            $"Note: Personalizer AI models learn from a large number of user interactions:\n" +
            $"You won't be able to tell the difference in what Personalizer returns by simulating a few events by hand.\n" +
            $"If you want a sample that focuses on seeing how Personalizer learns, see the Python Notebook sample.");

    IList<Action> actions = GetActions();
    IList<Slot> slots = GetSlots();

    using (var client = new HttpClient())
    {
        client.DefaultRequestHeaders.Add("ocp-apim-subscription-key", ResourceKey);
        int iteration = 1;
        bool runLoop = true;
        do
        {
            Console.WriteLine($"\nIteration: {iteration++}");
            string timeOfDayFeature = GetTimeOfDayForContext();
            string deviceFeature = GetDeviceForContext();

            IList<Context> context = GetContext(timeOfDayFeature, deviceFeature);

            string eventId = Guid.NewGuid().ToString();

            string rankRequestBody = JsonSerializer.Serialize(new MultiSlotRankRequest()
            {
                ContextFeatures = context,
                Actions = actions,
                Slots = slots,
                EventId = eventId,
                DeferActivation = false
            });

            //Ask Personalizer what action to show for each slot
            MultiSlotRankResponse multiSlotRankResponse = await SendMultiSlotRank(client, rankRequestBody, MultiSlotRankUrl);

            MultiSlotReward multiSlotRewards = new MultiSlotReward()
            {
                Reward = new List<SlotReward>()
            };

            for (int i = 0; i < multiSlotRankResponse.Slots.Count(); ++i)
            {
                Console.WriteLine($"\nPersonalizer service decided you should display: { multiSlotRankResponse.Slots[i].RewardActionId} in slot {multiSlotRankResponse.Slots[i].Id}. Is this correct? (y/n)");
                SlotReward reward = new SlotReward()
                {
                    SlotId = multiSlotRankResponse.Slots[i].Id
                };

                string answer = GetKey();

                if (answer == "Y")
                {
                    reward.Value = 1;
                    Console.WriteLine("\nGreat! The application will send Personalizer a reward of 1 so it learns from this choice of action for this slot.");
                }
                else if (answer == "N")
                {
                    reward.Value = 0;
                    Console.WriteLine("\nYou didn't like the recommended item. The application will send Personalizer a reward of 0 for this choice of action for this slot.");
                }
                else
                {
                    reward.Value = 0;
                    Console.WriteLine("\nEntered choice is invalid. Service assumes that you didn't like the recommended item.");
                }
                multiSlotRewards.Reward.Add(reward);
            }

            string rewardRequestBody = JsonSerializer.Serialize(multiSlotRewards);

            // Send the reward for the action based on user response for each slot.
            await SendMultiSlotReward(client, rewardRequestBody, MultiSlotRewardUrlBase, multiSlotRankResponse.EventId);

            Console.WriteLine("\nPress q to break, any other key to continue:");
            runLoop = !(GetKey() == "Q");
        } while (runLoop);
    }
}
```
Sehen Sie sich die Rangfolge- und Relevanzaufrufe in den folgenden Abschnitten genauer an.
Fügen Sie die folgenden Methoden hinzu, die [die Inhaltsauswahl abrufen](#get-content-choices-represented-as-actions), [Slots abrufen](#get-slots) und [Rangfolge- und Belohnungsanforderungen mit mehreren Slots senden](#make-http-requests), bevor die Codedatei ausgeführt wird:

* `GetActions`
* `GetSlots`
* `GetTimeOfDayForContext`
* `GetTimeOfDayForContext`
* `GetKey`
* `GetContext`
* `SendMultiSlotRank`
* `SendMultiSlotReward`

Fügen Sie die folgenden Klassen hinzu, die [die Texte der Rangfolge-/Belohnungsanforderungen erstellen und deren Antworten analysieren](#classes-for-constructing-rankreward-requests-and-responses), bevor die Codedatei ausgeführt wird:

* `Action`
* `Slot`
* `Context`
* `MultiSlotRankRequest`
* `MultiSlotRankResponse`
* `SlotResponse`
* `MultiSlotReward`
* `SlotReward`

## <a name="request-the-best-action"></a>Anfordern der besten Aktion

Für die Rangfolgeanforderung erfragt das Programm die Präferenzen des Benutzers, um für die Inhaltsoptionen ein Element vom Typ `context` zu erstellen. Der Anforderungstext enthält den Ausführungskontext, Aktionen und Slots mit ihren jeweiligen Funktionen. Die `SendMultiSlotRank`-Methode verwendet einen HTTP-Client, den Anforderungstext und die URL, um die Anforderung zu senden.

In dieser Schnellstartanleitung werden die einfachen Kontextmerkmale „Tageszeit“ und „Gerätewunsch des Benutzers“ verwendet. In Produktionssystemen kann die Bestimmung und [Auswertung](../concept-feature-evaluation.md) von [Aktionen und Merkmalen](../concepts-features.md) allerdings eine komplexe Angelegenheit sein.

```csharp
string timeOfDayFeature = GetTimeOfDayForContext();
string deviceFeature = GetDeviceForContext();

IList<Context> context = GetContext(timeOfDayFeature, deviceFeature);

string eventId = Guid.NewGuid().ToString();

string rankRequestBody = JsonSerializer.Serialize(new MultiSlotRankRequest()
{
    ContextFeatures = context,
    Actions = actions,
    Slots = slots,
    EventId = eventId,
    DeferActivation = false
});

//Ask Personalizer what action to show for each slot
MultiSlotRankResponse multiSlotRankResponse = await SendMultiSlotRank(client, rankRequestBody, MultiSlotRankUrl);
```

## <a name="send-a-reward"></a>Senden einer Relevanz

Um die Relevanzbewertung für die Relevanzanforderung zu erhalten, ruft das Programm die Auswahl des Benutzers für jeden Slot über die Befehlszeile ab, weist der Auswahl einen numerischen Wert (Relevanzbewertung) zu und sendet dann die eindeutige Ereignis-ID, Slot-ID und die Relevanzbewertung für jeden Slot als numerischen Wert an die Relevanz-API. Eine Belohnung muss nicht für jeden Slot definiert werden.

In dieser Schnellstartanleitung wird als Relevanzbewertung eine einfache Zahl (0 oder 1) zugewiesen. In Produktionssystemen ist die Entscheidung, was wann an den [Relevanzaufruf](../concept-rewards.md) gesendet werden soll (abhängig von Ihren spezifischen Anforderungen), unter Umständen etwas komplizierter.

```csharp
MultiSlotReward multiSlotRewards = new MultiSlotReward()
{
    Reward = new List<SlotReward>()
};

for (int i = 0; i < multiSlotRankResponse.Slots.Count(); ++i)
{
    Console.WriteLine($"\nPersonalizer service decided you should display: { multiSlotRankResponse.Slots[i].RewardActionId} in slot {multiSlotRankResponse.Slots[i].Id}. Is this correct? (y/n)");
    SlotReward reward = new SlotReward()
    {
        SlotId = multiSlotRankResponse.Slots[i].Id
    };

    string answer = GetKey();

    if (answer == "Y")
    {
        reward.Value = 1;
        Console.WriteLine("\nGreat! The application will send Personalizer a reward of 1 so it learns from this choice of action for this slot.");
    }
    else if (answer == "N")
    {
        reward.Value = 0;
        Console.WriteLine("\nYou didn't like the recommended item. The application will send Personalizer a reward of 0 for this choice of action for this slot.");
    }
    else
    {
        reward.Value = 0;
        Console.WriteLine("\nEntered choice is invalid. Service assumes that you didn't like the recommended item.");
    }
    multiSlotRewards.Reward.Add(reward);
}

string rewardRequestBody = JsonSerializer.Serialize(multiSlotRewards);

// Send the reward for the action based on user response for each slot.
await SendMultiSlotReward(client, rewardRequestBody, MultiSlotRewardUrlBase, multiSlotRankResponse.EventId);
```

## <a name="run-the-program"></a>Ausführen des Programms

Führen Sie die Anwendung mit dem Befehl dotnet `run` über das Anwendungsverzeichnis aus.

```console
dotnet run
```

![Das Schnellstartprogramm stellt ein paar Fragen zum Sammeln von Benutzereinstellungen – auch bekannt als Features – und stellt dann die am besten bewertete Aktion bereit.](../media/csharp-quickstart-commandline-feedback-loop/multislot-quickstart-program-feedback-loop-example-1.png)


Der [Quellcode für diese Schnellstartanleitung](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/dotnet/Personalizer/multislot-quickstart) ist verfügbar.
