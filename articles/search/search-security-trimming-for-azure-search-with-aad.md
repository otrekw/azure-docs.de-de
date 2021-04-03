---
title: Sicherheitsfilter zum Einschränken von Ergebnissen mit Active Directory
titleSuffix: Azure Cognitive Search
description: Hier finden Sie Informationen zu Sicherheitsberechtigungen auf Dokumentebene für Azure Cognitive Search-Suchergebnisse sowie zur Verwendung von Sicherheitsfiltern und Azure Active Directory-Identitäten (AD).
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/16/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: 5788585b2365b12a90a508e5a972b61f73e48c15
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "97629509"
---
# <a name="security-filters-for-trimming-azure-cognitive-search-results-using-active-directory-identities"></a>Sicherheitsfilter zum Einschränken von Ergebnissen der kognitiven Azure-Suche mit Active Directory-Identitäten

Dieser Artikel zeigt, wie Sie mithilfe von AD-Sicherheitsidentitäten (Azure Active Directory) und Filtern in Azure Cognitive Search Suchergebnisse basierend auf der Benutzergruppenmitgliedschaft einschränken.

In diesem Artikel werden die folgenden Aufgaben behandelt:
> [!div class="checklist"]
> - Erstellen von Azure AD-Gruppen und -Benutzern
> - Zuordnen des Benutzers zur erstellten Gruppe
> - Zwischenspeichern der neuen Gruppen
> - Indizieren von Dokumenten mit zugeordneten Gruppen
> - Ausgeben einer Suchanfrage mit Gruppenbezeichnerfilter
> 
> [!NOTE]
> Beispielcodeausschnitte in diesem Artikel sind in C# geschrieben. Den vollständigen Quellcode finden Sie [auf GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started). 

## <a name="prerequisites"></a>Voraussetzungen

Ihr Index in der kognitiven Azure-Suche muss ein [Sicherheitsfeld](search-security-trimming-for-azure-search.md) zum Speichern der Liste mit Gruppenidentitäten enthalten, die über Lesezugriff auf das Dokument verfügen. Dieser Anwendungsfall geht von einer 1:1-Zuordnung zwischen einem sicherungsfähigen Objekt (z.B. der Hochschulbewerbung einer Einzelperson) und einem Sicherheitsfeld aus, in dem festgelegt ist, wer Zugriff auf dieses Objekt hat (Zulassungspersonal).

Sie müssen über Azure AD-Administratorberechtigungen verfügen, die in dieser exemplarischen Vorgehensweise zum Erstellen von Benutzern, Gruppen und Zuordnungen benötigt werden. 

Ihre Anwendung muss außerdem als mehrinstanzenfähige App bei Azure AD registriert werden, wie im folgenden Verfahren beschrieben.

### <a name="register-your-application-with-azure-active-directory"></a>Registrieren Ihrer Anwendung bei Azure Active Directory

In diesem Schritt wird Ihre Anwendung in Azure AD integriert, um Anmeldungen von Benutzer- und Gruppenkonten zu akzeptieren. Wenn Sie nicht als Mandantenadministrator in Ihrer Organisation fungieren, müssen Sie möglicherweise einen [neuen Mandanten erstellen](../active-directory/develop/quickstart-create-new-tenant.md), um die folgenden Schritte auszuführen.

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach der Azure Active Directory-Ressource für Ihr Abonnement.

1. Suchen Sie auf der linken Seite unter **Verwalten** nach **App-Registrierungen**, und klicken Sie dann auf **New registration** (Neue Registrierung).

1. Benennen Sie die Registrierung. Verwenden Sie dabei beispielsweise einen Namen, der dem Namen der Suchanwendung ähnelt. Wählen Sie **Registrieren**.

1. Sobald die App-Registrierung erstellt wurde, kopieren Sie die Anwendungs-ID. Sie müssen diese Zeichenfolge für Ihre Anwendung bereitstellen.

   Wenn Sie [DotNetHowToSecurityTrimming](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToEncryptionUsingCMK) durchlaufen, fügen Sie diesen Wert in die Datei **app.config** ein.

   Wiederholen Sie diesen Vorgang für die Mandanten-ID.

   :::image type="content" source="media/search-manage-encryption-keys/cmk-application-id.png" alt-text="Anwendungs-ID im Abschnitt „Zusammenfassung“":::

1. Wählen Sie auf der linken Seite **API-Berechtigungen** und dann **Berechtigung hinzufügen** aus. 

1. Wählen Sie **Microsoft Graph** und dann **Delegierte Berechtigungen** aus.

1. Suchen Sie nach den folgenden delegierten Berechtigungen, und fügen Sie sie hinzu:

   - **Directory.ReadWrite.All**
   - **Group.ReadWrite.All**
   - **User.ReadWrite.All**

Microsoft Graph stellt eine API bereit, die über eine REST-API programmgesteuerten Zugriff auf Azure AD bietet. Das Codebeispiel für diese exemplarische Vorgehensweise verwendet die Berechtigungen zum Aufrufen der Microsoft Graph-API, um Gruppen, Benutzer und Zuordnungen zu erstellen. Die APIs werden außerdem zum Zwischenspeichern von Gruppenbezeichnern für eine schnellere Filterung verwendet.

## <a name="create-users-and-groups"></a>Erstellen von Benutzern und Gruppen

Wenn Sie einer vorhandenen Anwendung eine Suchfunktion hinzufügen, liegen in Azure AD möglicherweise bereits Benutzer- und Gruppenbezeichner vor. In diesem Fall können Sie die nächsten drei Schritte überspringen. 

Wenn Sie jedoch über keine vorhandenen Benutzer verfügen, können Sie die Sicherheitsprinzipale mithilfe der Microsoft Graph-APIs erstellen. Die folgenden Codeausschnitte veranschaulichen, wie Sie Bezeichner generieren, die zu Datenwerten für das Sicherheitsfeld in Ihrem Index für die kognitive Azure-Suche werden. In unserer hypothetischen Anwendung für die Hochschulzulassung entspräche dies den Sicherheitsbezeichnern für das Zulassungspersonal.

Die Benutzer- und Gruppenmitgliedschaft kann sich – besonders in großen Organisationen – häufig ändern. Code zum Erstellen von Benutzer- und Gruppenidentitäten muss häufig genug ausgeführt werden, um Änderungen in der Organisationsmitgliedschaft zu erfassen. Ebenso erfordert Ihr Index für die kognitive Azure-Suche einen ähnlichen Aktualisierungsplan, um den aktuellen Status der zugelassenen Benutzer und Ressourcen widerzuspiegeln.

### <a name="step-1-create-group"></a>Schritt 1: [Gruppe erstellen](/graph/api/group-post-groups) 

```csharp
private static Dictionary<Group, List<User>> CreateGroupsWithUsers(string tenant)
{
    Group group = new Group()
    {
        DisplayName = "My First Prog Group",
        SecurityEnabled = true,
        MailEnabled = false,
        MailNickname = "group1"
    };
```

### <a name="step-2-create-user"></a>Schritt 2: [Benutzer erstellen](/graph/api/user-post-users)

```csharp
User user1 = new User()
{
    GivenName = "First User",
    Surname = "User1",
    MailNickname = "User1",
    DisplayName = "First User",
    UserPrincipalName = String.Format("user1@{0}", tenant),
    PasswordProfile = new PasswordProfile() { Password = "********" },
    AccountEnabled = true
};
```

### <a name="step-3-associate-user-and-group"></a>Schritt 3: Zuordnen von Benutzer und Gruppe

```csharp
List<User> users = new List<User>() { user1, user2 };
Dictionary<Group, List<User>> groups = new Dictionary<Group, List<User>>() { { group, users } };
```

### <a name="step-4-cache-the-groups-identifiers"></a>Schritt 4: Zwischenspeichern der Gruppenbezeichner

Um die Netzwerklatenz zu verringern, können Sie optional die Benutzer-Gruppen-Zuordnungen zwischenspeichern, sodass bei einer Suchanfrage Gruppen aus dem Cache zurückgegeben werden und ein Roundtrip zu Azure AD entfällt. Sie können mit der [Azure AD-Batch-API](/graph/json-batching) eine einzelne HTTP-Anforderung mit mehreren Benutzern senden und den Cache erstellen.

Microsoft Graph ist für die Verarbeitung einer großen Anzahl von Anforderungen konzipiert. Wenn die Anzahl von Anforderungen zu hoch ist, gibt Microsoft Graph einen Fehler mit dem HTTP-Statuscode 429 aus. Weitere Informationen finden Sie unter [Microsoft Graph-Drosselung](/graph/throttling).

## <a name="index-document-with-their-permitted-groups"></a>Indizieren von Dokumenten mit den zugelassenen Gruppen

Abfragevorgänge in der kognitiven Azure-Suche werden über einen Index für die kognitive Azure-Suche ausgeführt. In diesem Schritt importiert ein Indizierungsvorgang durchsuchbare Daten in einen Index, die als Sicherheitsfilter verwendeten Bezeichner eingeschlossen. 

Die kognitive Azure-Suche führt keine Authentifizierung von Benutzeridentitäten durch und stellt keine Logik zur Verfügung, um festzustellen, welche Inhalte ein Benutzer anzeigen darf. Im Anwendungsfall für die Sicherheitskürzung wird davon ausgegangen, dass Sie für die Zuordnung zwischen einem sensiblen Dokument und dem Gruppenbezeichner sorgen, der Zugriff auf dieses Dokument hat, und dass diese Informationen vollständig in einen Suchindex importiert wurden. 

Im hypothetischen Beispiel würde der Textkörper der PUT-Anforderung in einem Index für die kognitive Azure-Suche den Aufsatz oder die Aufzeichnung eines Bewerbers für eine Hochschule sowie den Gruppenbezeichner enthalten, der die Berechtigung zur Anzeige dieses Inhalts erteilt. 

Im generischen Beispiel, das im Codebeispiel für diese exemplarische Vorgehensweise verwendet wird, könnte die Indexaktion wie folgt aussehen:

```csharp
private static void IndexDocuments(string indexName, List<string> groups)
{
    IndexDocumentsBatch<SecuredFiles> batch = IndexDocumentsBatch.Create(
        IndexDocumentsAction.Upload(
            new SecuredFiles()
            {
                FileId = "1",
                Name = "secured_file_a",
                GroupIds = new[] { groups[0] }
            }),
              ...
            };

IndexDocumentsResult result = searchClient.IndexDocuments(batch);
```

## <a name="issue-a-search-request"></a>Ausgeben einer Suchanfrage

Zum Zweck der Sicherheitskürzung sind die Werte im Sicherheitsfeld des Index statische Werte, die zum Ein- oder Ausschließen von Dokumenten in Suchergebnissen verwendet werden. Wenn der Gruppenbezeichner für Zulassungen beispielsweise „A11B22C33D44-E55F66G77-H88I99JKK“ lautet, werden alle Dokumente in einem Index für die kognitive Azure-Suche, die diesen Bezeichner im Sicherheitsfeld enthalten, in die an den Anforderer zurückgesendeten Suchergebnisse eingeschlossen (oder aus diesen ausgeschlossen).

Um die in Suchergebnissen zurückgegebenen Dokumente basierend nach Gruppen von Benutzern zu filtern, die die Anforderung ausgeben, führen Sie die folgenden Schritte aus.

### <a name="step-1-retrieve-users-group-identifiers"></a>Schritt 1: Abrufen der Gruppenbezeichner des Benutzers

Wenn die Gruppen des Benutzers nicht bereits zwischengespeichert wurden oder der Cache abgelaufen ist, geben Sie die [groups](/graph/api/directoryobject-getmembergroups)-Anforderung aus.

```csharp
private static async void RefreshCache(IEnumerable<User> users)
{
    HttpClient client = new HttpClient();
    var userGroups = await _microsoftGraphHelper.GetGroupsForUsers(client, users);
    _groupsCache = new ConcurrentDictionary<string, List<string>>(userGroups);
}
```

### <a name="step-2-compose-the-search-request"></a>Schritt 2: Erstellen der Suchanfrage

Vorausgesetzt, dass Sie über die Gruppenmitgliedschaft des Benutzers verfügen, können Sie die Suchanfrage mit den geeigneten Filterwerten ausgeben.

```csharp
private static void SearchQueryWithFilter(string user)
{
    // Using the filter below, the search result will contain all documents that their GroupIds field   
    // contain any one of the Ids in the groups list
    string filter = String.Format("groupIds/any(p:search.in(p, '{0}'))", string.Join(",", String.Join(",", _groupsCache[user])));
    SearchOptions searchOptions =
        new SearchOptions()
        {
            Filter = filter
        };
    searchOptions.Select.Add("name");

    SearchResults<SecuredFiles> results = searchClient.Search<SecuredFiles>("*", searchOptions);

    Console.WriteLine("Results for groups '{0}' : {1}", _groupsCache[user], results.GetResults().Select(r => r.Document.Name));
}
```

### <a name="step-3-handle-the-results"></a>Schritt 3: Verarbeiten der Ergebnisse

Die Antwort umfasst eine gefilterte Liste mit Dokumenten, für die der Benutzer über Anzeigeberechtigungen verfügt. Je nachdem, wie Sie die Seite mit den Suchergebnissen aufbauen, können Sie visuelle Hinweise zum gefilterten Resultset einschließen.

## <a name="next-steps"></a>Nächste Schritte

In dieser exemplarischen Vorgehensweise haben Sie ein Muster zur Verwendung von Azure AD-Anmeldungen zum Filtern von Dokumenten in Azure Cognitive Search-Ergebnissen kennengelernt, um Dokumente aus den Ergebnissen auszuschließen, die nicht mit den Filtern der Anforderung übereinstimmen. Um ein alternatives, womöglich einfacheres Muster anzuzeigen oder erneut auf andere Sicherheitsfeatures zuzugreifen, verwenden Sie die folgenden Links:

- [Sicherheitsfilter zum Einschränken von Ergebnissen](search-security-trimming-for-azure-search.md)
- [Sicherheit in der kognitiven Azure-Suche](search-security-overview.md)