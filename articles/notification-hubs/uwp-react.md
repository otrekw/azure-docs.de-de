---
title: Senden von Azure Notification Hub-Benachrichtigungen an Android- und iOS-Anwendungen
description: Erfahren Sie mehr über die plattformübergreifenden Funktionen von Azure Notification Hubs.
author: sethmanheim
ms.author: sethm
ms.service: notification-hubs
ms.topic: conceptual
ms.date: 06/14/2021
ms.custom: template-concept
ms.openlocfilehash: 7158fa486483f1ff26599e47e43ee3219a23045a
ms.sourcegitcommit: 3bb9f8cee51e3b9c711679b460ab7b7363a62e6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112082727"
---
# <a name="send-notifications-to-android-and-ios-applications"></a>Senden von Benachrichtigungen an Android- und iOS-Anwendungen

Dieser Artikel bietet eine Übersicht über die Azure Notification Hub-Beispielanwendung, die entwickelt wurde, um die Funktionen von Azure Notification Hub auf verschiedenen Plattformen zu demonstrieren. Die Anwendung verwendet ein Land-Survey-Szenario, in dem die **Contoso Land Survey**-Desktopanwendung Benachrichtigungen sendet, die sowohl Android- als auch iOS-Contoso-Anwendungen empfangen können.

Sie können das [vollständige Beispiel von GitHub herunterladen](https://github.com/Azure/azure-notificationhubs-samples/tree/main/NotificationHubSample).

## <a name="prerequisites"></a>Voraussetzungen

Für die Erstellung des Beispiels müssen die folgenden Voraussetzungen erfüllt sein:

- Ein Azure-Abonnement. Falls Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), bevor Sie beginnen.
- Microsoft Visual Studio 2019 oder höher. Dieses Beispiel verwendet [Visual Studio 2019](https://www.visualstudio.com/products).
- Visual Studio 2019 wird mit den folgenden Workloads installiert:
  - .NET 5.0 SDK
  - ASP.NET und Webentwicklung
  - Azure-Entwicklung
  - Node.js-Entwicklung
  - [UWP-App-Entwicklungstools](/windows/uwp/get-started/get-set-up)
- Firebase-Konto, um Pushbenachrichtigungen für Android zu aktivieren.
- Apple-Entwicklerkonto, um Pushbenachrichtigungen für iOS zu aktivieren.
- Eine SQL Server-Datenbankinstanz, die in Azure gehostet wird.
- Ein Azure Notification Hub-Namespace und ein Notification Hub.

## <a name="sample-architecture"></a>Beispielarchitektur

Die Lösung besteht aus den folgenden Komponenten.

- Azure Notification Hub-Instanz: ein ANH-Namespace und ein Hub, die im [Azure-Portal](https://portal.azure.com) konfiguriert wurden.
- SQL Server-Datenbank: Eine SQL Server-Datenbankinstanz, die im [Azure-Portal](https://portal.azure.com) konfiguriert wurde.
- ASP.NET App-Back-End: Ein Web-API-Back-End, das auf .NET 5.0 basiert, eine Verbindung mit dem Notification Hub herstellt und als Azure App Service gehostet wird.
- Windows UWP-Anwendung: Eine UWP-Anwendung, die mithilfe von React Native erstellt wurde und als „Manager“-Anwendung agiert, d. h. dass sie Nachrichten und Umfrageinformationen an verschiedene Benutzer und Umfragegruppen sendet. Die Anwendung hilft ebenfalls dabei, neue Benutzer zu erstellen und Gruppen zu bearbeiten, denen ein Benutzer zugewiesen ist.
- Android- und iOS-Client-Apps: „Land Survey“-Anwendungen, die mithilfe von React Native erstellt wurden. Diese Apps zeigen Benutzern die Informationen an, die von der UWP-Manager-Anwendung versendet wurden.

## <a name="sample-folder-structure"></a>Ordnerstruktur des Beispiels

Die Beispielanwendung auf GitHub enthält die folgenden Ordner:

- **NotificationHub.Sample.API:** Eine Visual Studio 2019 ASP.NET Web-API-Lösung, die als Back-End fungiert.
- **app:** Eine plattformübergreifende React Native-Anwendung, die das Senden von Benachrichtigungen über ein Manager-Login und das anschließende Empfangen von Benachrichtigungen mit einem Umfragebenutzer-Login ermöglicht.
- **azure-template:** Azure Resource Manager-Vorlagen (`parameters.json` und `template.json`), die Sie für die Bereitstellung aller erforderlichen Ressourcen zum Konfigurieren dieser Bereitstellung in Ihrem Azure-Abonnement verwenden können. Weitere Informationen über die Resource Manager-Vorlage finden Sie unter [Erstellen und Bereitstellen von ARM-Vorlagen mithilfe von Azure-Portal](/azure/azure-resource-manager/templates/quickstart-create-templates-use-the-portal).

## <a name="sample-overview"></a>Beispielübersicht

Die folgenden Abschnitte bieten eine Übersicht über die Komponenten, aus denen das Beispiel besteht.

### <a name="controllers"></a>Controller

#### <a name="authentication"></a>Authentifizierung

In AuthenticateController.cs werden die folgenden Methoden verwendet, um einen angemeldeten Benutzer zu authentifizieren:

```cs
[HttpPost]
[Route("login")]
public async Task<IActionResult> Login([FromBody] LoginModel model)
{
   var user = await userManager.FindByNameAsync(model.Username);
    if (user != null && await userManager.CheckPasswordAsync(user, model.Password))
    {
        var userRoles = await userManager.GetRolesAsync(user);

        var authClaims = new List<Claim>
        {
            new Claim(ClaimTypes.Name, user.UserName),
            new Claim(JwtRegisteredClaimNames.Jti, Guid.NewGuid().ToString()),
        };

        foreach (var userRole in userRoles)
        {
            authClaims.Add(new Claim(ClaimTypes.Role, userRole));
        }

        var authSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(_configuration["JWT:Secret"]));

        var token = new JwtSecurityToken(
            issuer: _configuration["JWT:ValidIssuer"],
            audience: _configuration["JWT:ValidAudience"],
            expires: DateTime.Now.AddHours(3),
            claims: authClaims,
            signingCredentials: new SigningCredentials(authSigningKey, SecurityAlgorithms.HmacSha256)
            );

        UserDetails userDetails = new UserDetails();
        userDetails.FirstName = model.Username;
        userDetails.LastName = model.Username;
        userDetails.UserName = model.Username;

        return Ok(new
        {
            token = new JwtSecurityTokenHandler().WriteToken(token),
            expiration = token.ValidTo,
            username = model.Username,
            email = user.Email,
            role = userRoles != null ? userRoles[0] : "Site-Manager",
            user = userDetails
        });
    }
    return Unauthorized();
}

[HttpPost]
[Route("register")]
public async Task<IActionResult> Register([FromBody] RegisterModel model)
{
    var userExists = await userManager.FindByNameAsync(model.Username);
    if (userExists != null)
        return StatusCode(StatusCodes.Status500InternalServerError, new Response { Status = "Error", Message = "User already exists!" });

    ApplicationUser user = new ApplicationUser()
    {
        Email = model.Email,
        SecurityStamp = Guid.NewGuid().ToString(),
        UserName = model.Username
    };
    var result = await userManager.CreateAsync(user, model.Password);
    if (!result.Succeeded)
        return StatusCode(StatusCodes.Status500InternalServerError, new Response { Status = "Error", Message = "User creation failed! Please check user details and try again." });

    if (!await roleManager.RoleExistsAsync(UserRoles.SiteManager))
        await roleManager.CreateAsync(new IdentityRole(UserRoles.SiteManager));

    if (await roleManager.RoleExistsAsync(UserRoles.SiteManager))
    {
        await userManager.AddToRoleAsync(user, UserRoles.SiteManager);
    }

    return Ok(new Response { Status = "Success", Message = "User created successfully!" });
}
```

### <a name="dashboard"></a>Dashboard

Der Dashboardcontroller in DashboardController.cs gibt alle Benachrichtigungsinformationen zurück:

```cs
public class DashboardController : ControllerBase
{
    private readonly ApplicationDbContext _db;
    private readonly INotificationService _notificationService;

    public DashboardController(ApplicationDbContext dbContext, INotificationService notificationService)
    {
        _db = dbContext;
        _notificationService = notificationService;
    }

    [HttpGet("insights")]
    public async Task<IActionResult> GetDashboardInsight(string duration)
    {
        DashboardInsight dashboardInsight = new DashboardInsight();

        dashboardInsight.DeviceTrends = await _notificationService.GetAllRegistrationInfoAsync();

        var notificationMessages = _db.NotificationMessages.ToList();

        switch (duration)
        {
            case "Daily":
                {
                    dashboardInsight.NotificationTrends = _db.NotificationMessages
                                                            .GroupBy(m => m.SentTime.Date)
                                                            .Select(m => new NotificationTrend()
                                                            {
                                                                Timestamp = m.Key.ToShortDateString(),
                                                                NotificationsSent = m.Count()
                                                            }).ToList();
                }
                break;
            case "Weekly":
                {
                    dashboardInsight.NotificationTrends = notificationMessages
                                                            .GroupBy(m => WeekNumber(m.SentTime.Date))
                                                            .Select(m => new NotificationTrend()
                                                            {
                                                                Timestamp = FirstDateOfWeekISO8601(DateTime.Now.Year, m.Key).ToShortDateString(),
                                                                NotificationsSent = m.Count()
                                                            }).ToList();
                }
                break;
            case "Monthly":
                {
                    dashboardInsight.NotificationTrends = _db.NotificationMessages
                                                            .GroupBy(m => m.SentTime.Date.Month)
                                                            .Select(m => new NotificationTrend()
                                                            {
                                                                Timestamp = m.Key + "-" + DateTime.Now.Year,
                                                                NotificationsSent = m.Count()
                                                            }).ToList();
                }
                break;
            default:
                break;
        }

        dashboardInsight.TotalGroups = _db.SurveyGroups.Count();
        dashboardInsight.TotalUsers = _db.Users.Count();
        dashboardInsight.TotalNotificationsSent = _db.NotificationMessages.Count();

        return Ok(dashboardInsight);
    }
```

### <a name="front-end"></a>Front-End

Zum Aufrufen einer beliebigen Back-End-API erstellt das Beispiel den Dienst notification.service.js, der dann den tatsächlichen API-Aufruf tätigt. Dieser Code befindet sich in app\data\services\notification.service.js:

```nodejs
export const sendNotificationAPI = async (userInfo) => {
  let url = `${api}notification/send`;
  let authHeader = await getAuthHeaders();
  return await post(url, userInfo, { ...authHeader });
};

export const getNotificationsAPI = async () => {
  let url = `${api}notification/get`;
  let authHeader = await getAuthHeaders();
  return await get(url, { ...authHeader });
};
```

### <a name="manager-application"></a>Manager-Anwendung

In diesem Beispiel wird eine Windows UWP-Anwendung genutzt, die mithilfe von React Native erstellt wurde und als „Manager“-Anwendung agiert, d. h. dass sie Nachrichten und Umfrageinformationen an verschiedene Benutzer und Umfragegruppen sendet. Die Anwendung hilft ebenfalls dabei, neue Benutzer zu erstellen und Gruppen zu bearbeiten, denen ein Benutzer zugewiesen ist.

Verwenden Sie für das Manager-Login den folgenden Endpunkt und POST-Text, um Anmeldeinformationen Ihrer Wahl für die Benutzer zu generieren. Sie können einen beliebigen HTTP-REST-Client Ihrer Wahl verwenden:

#### <a name="endpoint"></a>Endpunkt

```http
POST {{endpoint}}api/authenticate/register-admin
```

#### <a name="body"></a>Text

```http
{
  "username": "<USER_NAME>",
  "email": "<EMAIL>",
  "password": "<PASSWORD>"
}
```

Nach der Registrierung sollten Sie sich mit denselben Anmeldeinformationen bei der UWP-Anwendung anmelden können.

### <a name="notification-controller"></a>Benachrichtigungscontroller

Der folgende Code, in NotificationController.cs, ruft Benachrichtigungen ab und sendet diese:

```cs
[Produces("application/json")]
[Consumes("application/json")]
[HttpPost("send")]
public async Task<ActionResult> SendNotification([FromBody] NotificationMessage notificationMessage)
{
    try
    {
        List<string> tags = new List<string>();

        // attach survey group and user information with notificationMessage
        notificationMessage.SurveyGroupTags.ForEach(surveyGroupId =>
        {
            var group = _db.SurveyGroups.Where(g => g.Id == surveyGroupId).FirstOrDefault();
            if (group != null)
            {
                notificationMessage.SurveyGroups.Add(group);
                tags.Add($"group:{group.GroupName.Replace(' ', '-')}");
            }
        });

        notificationMessage.UserTags.ForEach(userId =>
        {
            var user = _db.Users.Where(u => u.Id == userId).FirstOrDefault();
            if (user != null)
            {
                notificationMessage.Users.Add(user);
                tags.Add($"username:{user.UserName}");
            }
        });
        _db.NotificationMessages.Add(notificationMessage);

        // send template notification
        var notification = new Dictionary<string, string>();
        notification.Add("title", notificationMessage.NotificationTitle);
        notification.Add("message", notificationMessage.NotificationDescription);

        var res = await _notificationService.RequestNotificationAsync(notificationMessage, tags, HttpContext.RequestAborted);

        await _db.SaveChangesAsync();
        return Ok(notificationMessage);
    }
    catch (Exception ex)
    {
        return BadRequest(ex.Message);
    }
}

[Produces("application/json")]
[HttpGet("get")]
public async Task<ActionResult> Get()
{
    try
    {
        var surveyGroups = _db.NotificationMessages.Include(message => message.SurveyGroups).Include(message => message.Users).ToList();
        return Ok(surveyGroups);
    }
    catch (Exception ex)
    {
        return BadRequest();
    }
}
```

### <a name="notification-service"></a>Benachrichtigungsdienst

Mit dem Benachrichtigungsdienst in **NotificationHub.Sample.API/NotificationHub.Sample.API/Services/Notifications/INotificationService.cs** kann die Installation erstellt oder gelöscht werden. Es besteht auch die Möglichkeit, Benachrichtigungen an alle registrierten Benutzer zu senden und alle Registrierungsinformationen abzurufen:

```cs
public interface INotificationService
{
   Task<bool> CreateOrUpdateInstallationAsync(DeviceInstallation deviceInstallation, CancellationToken cancellationToken);
   Task<bool> DeleteInstallationByIdAsync(string installationId, CancellationToken cancellationToken);
   Task<bool> RequestNotificationAsync(NotificationMessage notificationMessage, IList<string> tags, CancellationToken cancellationToken);
   Task<List<DeviceTrend>> GetAllRegistrationInfoAsync();
}
```

## <a name="deploy-the-solution"></a>Bereitstellen der Lösung

Um das Beispiel auszuführen, ist Folgendes erforderlich:

- Ein Azure-Abonnement. Erstellen Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), falls Sie nicht über ein Azure-Abonnement verfügen.
- Firebase-Konto, um Pushbenachrichtigungen für Android einzurichten.
- Apple-Entwicklerkonto, um Pushbenachrichtigungen für iOS einzurichten.
- Ein Windows-Computer, auf dem Visual Studio 2019 installiert ist, zusammen mit einer Anwendungsentwicklungsunterstützung durch ASP.NET Core und UWP.
- Android SDK und Android Studio auf Ihrem Entwicklungscomputer, wenn die Android-Anwendung ausgeführt werden soll.
- Ein Mac OSX-Computer mit XCode und dem neuesten iOS SDK, wenn die iOS-Anwendung ausgeführt werden soll.

### <a name="deploy-resources"></a>Bereitstellen von Ressourcen

- **[Azure Notification Hub-Instanz](#create-resource-notification-hub):** ein in Azure konfigurierter Notification Hub.
- **[SQL Server-Datenbankinstanz](#create-resource-sql-database):** Eine in Azure gehostete SQL Server-Datenbank.
- **ASP.NET App-Back-End:** Web-API-Back-End, das mit .NET 5.0 erstellt wurde, eine Verbindung mit Azure Notification Hub herstellt und als Azure App Service gehostet wird. Weitere Informationen finden Sie unter [Schnellstart: Bereitstellen einer ASP.NET-Web-App](/azure/app-service/quickstart-dotnetcore?tabs=net50&pivots=development-environment-vs).

Wenn Sie nicht alle Ressourcen manuell bereitstellen möchten, können Sie die im GitHub-Repository bereitgestellte Azure Resource Manager template.json-Datei verwenden, um alle erforderlichen Instanzen in einem Schritt bereitzustellen. Die Vorlagendatei ist im Repository unter /azure-template verfügbar. Weitere Informationen zur Verwendung von Resource Manager-Vorlagen finden Sie unter [Tutorial: Verwenden von Azure-Schnellstartvorlagen](/azure/azure-resource-manager/templates/template-tutorial-quickstart-template?tabs=azure-powershell).

### <a name="set-up-notifications-for-android-and-ios"></a>Einrichten von Pushbenachrichtigungen für Android und iOS

- **Firebase:** Konfigurieren Sie zum Empfangen von Benachrichtigungen unter Android den Firebase-Dienst und verbinden Sie ihn mit Ihrer Azure Notification Hub-Instanz. Weitere Informationen finden Sie unter [Konfigurieren von Google Firebase-Einstellungen](configure-google-firebase-cloud-messaging.md).
- **Apple Push Notification Service (APNS):** Um Benachrichtigungen unter iOS zu erhalten, konfigurieren Sie den APNS-Dienst mit Ihrem Apple-Entwicklerkonto und verbinden Sie ihn mit Ihrer Azure Notification Hub-Instanz. Weitere Informationen finden Sie unter [Konfigurieren von Apple Push Notification Service-Einstellungen](configure-apple-push-notification-service.md).

## <a name="build-the-solution"></a>Erstellen Sie die Lösung.

Führen Sie zum Erstellen des Beispiels die folgenden Schritte aus.

### <a name="create-resource-sql-database"></a>Ressource erstellen: SQL-Datenbank

[Erstellen Sie eine SQL Server-Datenbankinstanz](/azure/azure-sql/database/single-database-create-quickstart?tabs=azure-portal) im Azure-Portal. Beispiel:

:::image type="content" source="media/uwp-react/resources-sql.png" alt-text="SQL-Instanzressourcen":::

:::image type="content" source="media/uwp-react/template-screenshot.png" alt-text="Bereitstellungsvorlage":::

### <a name="create-resource-notification-hub"></a>Ressource erstellen: Notification Hub

Erstellen Sie wie nachfolgend beschrieben einen Benachrichtigungshub im Azure-Portal:

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

### <a name="configure-backend"></a>Konfigurieren des Back-Ends

Suchen Sie zum Konfigurieren des App-Back-Ends die Datei **/NotificationHub.Sample.API/appsettings.json** und konfigurieren Sie die SQL Server-Verbindungszeichenfolge:

```json
"ConnectionStrings": {
    "SQLServerConnectionString": "Server=tcp:<SERVER_NAME>,1433;Initial Catalog=<DB_NAME>;Persist Security Info=False;User ID=<DB_USER_NAME>;Password=<PASSWORD>;MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"
  },
```

Ersetzen Sie `<SERVER_NAME>` durch den Namen Ihrer SQL Server-Instanz, `<DB_NAME>` durch ihre bereitgestellte Datenbank-URL, `<DB_USER_NAME>` durch den konfigurierten Benutzernamen und `<PASSWORD>` durch das konfigurierte Kennwort.

Sie können die API-Projektmappe lokal oder auf einem beliebigen IIS-Server ausführen oder sie als Azure Web-App-Service bereitstellen. Halten Sie die URL der API bereit.

### <a name="build-the-notificationhubsampleapi-application"></a>Erstellen der NotificationHub.Sample.API-Anwendung

1. Laden Sie die Projektmappe /NotificationHubSample/NotificationHub.Sample.API/NotificationHub.Sample.API.sln in Visual Studio.
2. Klicken Sie im Menü **Build** (Erstellen) auf **Build Solution** (Projektmappe erstellen).
3. Veröffentlichen Sie die Projektmappe in Azure: Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **NotificationHub.Sample.API** und wählen Sie dann **Publish** (veröffentlichen) aus.
4. Wählen Sie im Dialogfeld **Publish** (Veröffentlichen) die Option **Azure** (erste Option) und dann **Next** (Weiter) aus.
5. Wählen Sie **Azure App Service (Windows)** und dann **Next** (Weiter) aus.
6. Sie werden zum folgenden Dialogfeld umgeleitet, in dem Sie den entsprechenden Abonnementnamen und die AppService-Instanzen auswählen können. Ihr Back-End wird in Azure bereitgestellt und Sie werden zu einer neuen URL umgeleitet.

   :::image type="content" source="media/uwp-react/publish.png" alt-text="Projekt veröffentlichen":::

7. Fügen Sie nach der Veröffentlichung des Back-Ends die generierte URL zu **config.js** hinzu, welches sich im Ordner **/app** befindet. Achten Sie darauf, `/api/` nach der URL anzufügen.

## <a name="run-react-native-frontend-application-for-windows"></a>Ausführen der nativen React-Front-End-Anwendung für Windows

Für die Anwendung ist es erforderlich, dass sowohl die mobile Anwendung (entweder Android oder iOS) als auch die UWP-Manager-Anwendung gleichzeitig ausgeführt werden. Befolgen Sie die folgenden Schritte, um beide auszuführen:

Öffnen Sie den **App**-Ordner in Ihrem bevorzugten Terminal- oder Shellfenster. Gehen Sie nun wie folgt vor:

### <a name="windows"></a>Windows

1. Führen Sie `npm install` aus, um alle Paketabhängigkeiten zu installieren.
2. Führen Sie `npm run start` aus, um den Metroserver in einem Konsolenfenster zu starten.
3. Öffnen Sie ein weiteres Terminalfenster und führen Sie `npx react-native run-windows` aus, um die UWP-Anwendung auszuführen.
4. Wenn die Bereitstellung oder der Build einen Fehler meldet, lesen Sie den Leitfaden zur Problembehandlung.

### <a name="android"></a>Android

1. Konfigurieren Sie Firebase für Ihr React-Native-Projekt, um sicherzustellen, dass Sie die Benachrichtigungsfunktionen verwenden können.
2. Nachdem Sie das Firebase-Projekt erfolgreich konfiguriert haben, laden Sie die Datei **google-services.json** aus dem Firebase-Portal herunter.
3. Ersetzen Sie **./app/android/app/google-services.json** durch diese neue Datei. Stellen Sie sicher, dass der Name des Anwendungspakets mit dem in Firebase konfigurierten Namen übereinstimmt. Der Paketname ist in der **AndroidManifest.xml**-Datei konfiguriert.
4. Weitere Informationen über das Konfigurieren von Benachrichtigungen in einer React Native-Anwendung finden Sie unter [Tutorial: Senden von Pushbenachrichtigungen an React Native](/azure/developer/mobile-apps/notification-hubs-backend-service-react-native).
5. Führen Sie nach dem Konfigurieren der Benachrichtigungen `npm run start` aus, um den Metroserver in einem Konsolenfenster zu starten. Wenn Ihre Windows-Anwendung bereits ausgeführt wird, können Sie diesen Schritt überspringen.
6. Führen Sie in einem neuen Konsolenfenster `npx react-native run-android` aus, um die Android-Anwendung auszuführen.

### <a name="ios"></a>iOS

1. Konfigurieren Sie APNS für Ihr React-Native-Projekt, um sicherzustellen, dass Sie die Benachrichtigungsfunktionen verwenden können.
2. In iOS können Benachrichtigungen nur von Anwendungen empfangen werden, die über den App Store oder über TestFlight signiert und installiert wurden. Sie müssen eine Anwendung in Ihrem Apple-Entwicklerkonto erstellen. Die in Ihrem Apple-Entwicklerkonto konfigurierte Paket-ID der Anwendung sollte in den Dateien **Info.plist** und **Entitlements.plist** Ihrer Anwendung konfiguriert werden.
3. Weitere Informationen über das Konfigurieren von Benachrichtigungen in einer React Native-Anwendung finden Sie unter [Tutorial: Senden von Pushbenachrichtigungen an React Native](/azure/developer/mobile-apps/notification-hubs-backend-service-react-native).
4. Führen Sie nach dem Konfigurieren der Benachrichtigungen `npm run start` aus, um den Metroserver in einem Konsolenfenster zu starten. Wenn Ihre Windows-Anwendung bereits ausgeführt wird, können Sie diesen Schritt überspringen.
5. Führen Sie in einem neuen Konsolenfenster `npx react-native run-ios` aus, um die iOS-Anwendung auszuführen. Wie bereits erwähnt, funktionieren die Benachrichtigungen unter iOS nicht, wenn sie lokal bereitgestellt werden. Im iOS-Simulator können Sie keine Benachrichtigungen empfangen.

## <a name="troubleshooting"></a>Problembehandlung

Beim Ausführen der React Native für Windows-Anwendung wird Ihnen möglicherweise der folgende Fehler angezeigt:

`error MSB4057: The target "Deploy" does not exist in the project`

Dies ist ein bekanntes Problem bei React Native für Windows. Um dies zu beheben, entladen Sie zunächst die **.csproj**-Datei für die Projekte **CheckboxWindows** und **ReactNativeAsyncStorage** in Visual Studio, nachdem Sie **app.sln** im Ordner **app/windows** geöffnet haben. Fügen Sie dann in der Datei **app.csproj** die folgende Zeile direkt vor `...</Project>` hinzu und laden Sie das Projekt anschließend erneut:

```xml
<Target Name="Deploy"/>
```

## <a name="next-steps"></a>Nächste Schritte

- [Tutorial: Senden von Pushbenachrichtigungen an React Native-Apps mit Azure Notification Hubs](/azure/developer/mobile-apps/notification-hubs-backend-service-react-native)
- [Tutorial: Senden von Pushbenachrichtigungen an Android-Geräte mit Firebase SDK](android-sdk.md)
- [Azure Notification Hubs](notification-hubs-push-notification-overview.md)
- [React Native für Windows](https://microsoft.github.io/react-native-windows/)
