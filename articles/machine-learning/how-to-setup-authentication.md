---
title: Einrichten der Authentifizierung
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie die Authentifizierung für verschiedene Ressourcen und Workflows in Azure Machine Learning einrichten und konfigurieren.
services: machine-learning
author: cjgronlund
ms.author: cgronlun
ms.reviewer: larryfr
ms.service: machine-learning
ms.subservice: core
ms.date: 06/17/2020
ms.topic: conceptual
ms.custom: how-to, has-adal-ref, devx-track-js, devx-track-azurecli
ms.openlocfilehash: fd6f933e1b3c1e7c003f62e03215273e3d28ea5c
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2020
ms.locfileid: "93318526"
---
# <a name="set-up-authentication-for-azure-machine-learning-resources-and-workflows"></a>Einrichten der Authentifizierung für Azure Machine Learning-Ressourcen und -Workflows


Erfahren Sie, wie Sie sich bei Ihrem Azure Machine Learning-Arbeitsbereich und bei Modellen authentifizieren, die als Webdienste bereitgestellt werden.

Im Allgemeinen gibt es zwei Arten von Authentifizierung, die Sie für Azure Machine Learning verwenden können:

* __Interaktiv:__ Sie verwenden Ihr Konto in Azure Active Directory, um sich entweder direkt zu authentifizieren oder ein Token zu erhalten, das für die Authentifizierung verwendet wird. Die interaktive Authentifizierung wird beim Experimentieren und bei der iterativen Entwicklung verwendet. Außerdem wird sie verwendet, wenn der Zugriff auf Ressourcen (z. B. einen Webdienst) auf Benutzerbasis gesteuert werden soll.
* __Dienstprinzipal__ : Sie erstellen ein Dienstprinzipalkonto in Azure Active Directory und verwenden es, um sich zu authentifizieren oder ein Token zu erhalten. Ein Dienstprinzipal wird verwendet, wenn ein automatisierter Prozess sich beim Dienst authentifizieren soll, ohne dass eine Benutzerinteraktion erforderlich ist. Ein Beispiel hierfür wäre ein Continuous-Integration- und Bereitstellungsskript, das ein Modell bei jeder Änderung des Trainingscodes trainiert und testet. Sie können einen Dienstprinzipal auch verwenden, um ein Token für die Authentifizierung bei einem Webdienst abzurufen, wenn Sie nicht möchten, dass sich der Endbenutzer des Diensts authentifizieren muss, oder wenn die Endbenutzerauthentifizierung nicht direkt in Azure Active Directory ausgeführt wird.

Unabhängig vom verwendeten Authentifizierungstyp wird die rollenbasierte Zugriffssteuerung von Azure (Role-Based Access Control, RBAC) verwendet, um die zulässige Zugriffsebene für die Ressourcen zu beschränken. Ein Konto, das zum Abrufen des Zugriffstokens für ein bereitgestelltes Modell verwendet wird, benötigt z. B. nur Lesezugriff auf den Arbeitsbereich. Weitere Informationen zur Azure RBAC finden Sie unter [Verwalten des Zugriffs auf einen Azure Machine Learning-Arbeitsbereich](how-to-assign-roles.md).

## <a name="prerequisites"></a>Voraussetzungen

* Erstellen Sie einen [Azure Machine Learning-Arbeitsbereich](how-to-manage-workspace.md).
* [Konfigurieren Sie Ihre Entwicklungsumgebung](how-to-configure-environment.md) für die Installation des Azure Machine Learning SDK, oder verwenden Sie eine [Azure Machine Learning-Notebook-VM](concept-azure-machine-learning-architecture.md#compute-instance) mit bereits installiertem SDK.

## <a name="interactive-authentication"></a>Interaktive Authentifizierung

> [!IMPORTANT]
> Die interaktive Authentifizierung verwendet Ihren Browser und erfordert Cookies (einschließlich Cookies von Drittanbietern). Wenn Sie Cookies deaktiviert haben, erhalten Sie möglicherweise eine Fehlermeldung wie „Wir konnten Sie nicht anmelden“. Dieser Fehler kann auch auftreten, wenn Sie [Azure Multi-Factor Authentication](../active-directory/authentication/concept-mfa-howitworks.md) aktiviert haben.

In den meisten Beispielen in der Dokumentation und Verwendungsbeispielen wird interaktive Authentifizierung verwendet. Bei Verwendung des SDK gibt es z. B. zwei Funktionsaufrufe, bei denen Sie automatisch über einen auf einer Benutzeroberfläche basierenden Authentifizierungsflow eine Aufforderung erhalten:

* Wenn Sie die Funktion `from_config()` aufrufen, wird die Aufforderung ausgegeben.

    ```python
    from azureml.core import Workspace
    ws = Workspace.from_config()
    ```

    Die Funktion `from_config()` sucht nach einer JSON-Datei, die ihre Informationen zur Arbeitsbereichsverbindung enthält.

* Bei Verwendung des Konstruktors `Workspace` für das Angeben von Abonnement-, Ressourcengruppen- und Arbeitsbereichsinformationen wird ebenfalls zur interaktiven Authentifizierung aufgefordert.

    ```python
    ws = Workspace(subscription_id="your-sub-id",
                  resource_group="your-resource-group-id",
                  workspace_name="your-workspace-name"
                  )
    ```

> [!TIP]
> Wenn Sie Zugriff auf mehrere Mandanten haben, müssen Sie möglicherweise die-Klasse importieren und explizit definieren, auf welchen Mandanten Sie abzielen. Wenn Sie den Konstruktor für `InteractiveLoginAuthentication` aufrufen, werden Sie außerdem aufgefordert, sich ähnlich wie bei den oben aufgeführten Aufrufen anzumelden.
>
> ```python
> from azureml.core.authentication import InteractiveLoginAuthentication
> interactive_auth = InteractiveLoginAuthentication(tenant_id="your-tenant-id")
> ```

## <a name="service-principal-authentication"></a>Dienstprinzipalauthentifizierung

Wenn Sie die Dienstprinzipalauthentifizierung verwenden möchten, müssen Sie zunächst den Dienstprinzipal erstellen und ihm Zugriff auf Ihren Arbeitsbereich gewähren. Wie bereits erwähnt, wird die rollenbasierte Zugriffssteuerung von Azure (Azure RBAC) verwendet, um den Zugriff zu steuern. Sie müssen daher auch entscheiden, welcher Zugriff dem Dienstprinzipal erteilt werden soll.

> [!IMPORTANT]
> Wenn Sie einen Dienstprinzipal verwenden, erteilen Sie ihm den __Mindestzugriff, der für den Task erforderlich ist,__ für den er verwendet wird. Beispielsweise wird einem Dienstprinzipal kein Besitzer- oder Mitwirkendenzugriff erteilt, wenn er lediglich für das Lesen des Zugriffstokens für eine Webbereitstellung verwendet wird.
>
> Der Grund für das Erteilen der geringstmöglichen Zugriffsberechtigungen ist, dass ein Dienstprinzipal ein Kennwort für die Authentifizierung verwendet und dieses möglicherweise als Teil eines Automatisierungsskripts gespeichert wird. Wenn das Kennwort offengelegt wird, wird durch die Beschränkung auf den für einen bestimmten Task erforderlichen Mindestzugriff die mögliche böswillige Verwendung des Dienstprinzipals auf ein Minimum beschränkt.

Die einfachste Art der Erstellung eines Dienstprinzipals und Gewährung von Zugriff auf Ihren Arbeitsbereich ist die Verwendung der [Azure CLI](/cli/azure/install-azure-cli?preserve-view=true&view=azure-cli-latest). Führen Sie die folgenden Schritte aus, um einen Dienstprinzipal zu erstellen und ihm Zugriff auf Ihren Arbeitsbereich zu gewähren:

> [!NOTE]
> Sie müssen ein Administrator für das Abonnement sein, um alle Schritte ausführen zu können.

1. Authentifizieren Sie sich bei Ihrem Azure-Abonnement:

    ```azurecli-interactive
    az login
    ```

    Die CLI öffnet Ihren Standardbrowser, sofern sie dazu in der Lage ist, und lädt eine Anmeldeseite. Andernfalls müssen Sie einen Browser öffnen und die Anweisungen in der Befehlszeile befolgen. Die Anweisungen umfassen das Navigieren zu [https://aka.ms/devicelogin](https://aka.ms/devicelogin) und Eingeben eines Autorisierungscodes.

    [!INCLUDE [select-subscription](../../includes/machine-learning-cli-subscription.md)] 

    Andere Methoden zur Authentifizierung finden Sie unter [Anmelden mit der Azure CLI](/cli/azure/authenticate-azure-cli?preserve-view=true&view=azure-cli-latest).

1. Installieren Sie die Azure Machine Learning-Erweiterung:

    ```azurecli-interactive
    az extension add -n azure-cli-ml
    ```

1. Erstellen Sie den Dienstprinzipal. Im folgenden Beispiel wird ein Dienstprinzipal mit dem Namen **ml-auth** erstellt:

    ```azurecli-interactive
    az ad sp create-for-rbac --sdk-auth --name ml-auth
    ```

    Die Ausgabe ist eine JSON-Datei wie etwa die folgende. Notieren Sie sich die Werte in den Feldern `clientId`, `clientSecret`und `tenantId`, da Sie diese für andere Schritte in diesem Artikel benötigen.

    ```json
    {
        "clientId": "your-client-id",
        "clientSecret": "your-client-secret",
        "subscriptionId": "your-sub-id",
        "tenantId": "your-tenant-id",
        "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
        "resourceManagerEndpointUrl": "https://management.azure.com",
        "activeDirectoryGraphResourceId": "https://graph.windows.net",
        "sqlManagementEndpointUrl": "https://management.core.windows.net:5555",
        "galleryEndpointUrl": "https://gallery.azure.com/",
        "managementEndpointUrl": "https://management.core.windows.net"
    }
    ```

1. Rufen Sie mithilfe des Werts für `clientId`, der im vorherigen Schritt zurückgegeben wurde, die Details für den Dienstprinzipal ab:

    ```azurecli-interactive
    az ad sp show --id your-client-id
    ```

    Der folgende JSON-Code ist ein vereinfachtes Beispiel für die Ausgabe des Befehls. Notieren Sie sich den Wert in Feld `objectId`, da Sie diesen für den nächsten Schritt benötigen.

    ```json
    {
        "accountEnabled": "True",
        "addIns": [],
        "appDisplayName": "ml-auth",
        ...
        ...
        ...
        "objectId": "your-sp-object-id",
        "objectType": "ServicePrincipal"
    }
    ```

1. Erlauben Sie dem Dienstprinzipal, auf Ihren Azure Machine Learning-Arbeitsbereich zuzugreifen. Sie benötigen den Namen Ihres Arbeitsbereichs und den Namen der Ressourcengruppe für die Parameter `-w` und `-g`. Verwenden Sie für den Parameter `--user` den Wert `objectId` aus dem vorherigen Schritt. Über den Parameter `--role` können Sie die Zugriffsrolle für den Dienstprinzipal festlegen. Im folgenden Beispiel wird dem Dienstprinzipal die Rolle **Besitzer** zugewiesen. 

    > [!IMPORTANT]
    > Der Besitzerzugriff ermöglicht es dem Dienstprinzipal, so gut wie jeden Vorgang in Ihrem Arbeitsbereich durchzuführen. Er wird in diesem Dokument verwendet, um zu veranschaulichen, wie Zugriff gewährt wird. In einer Produktionsumgebung empfiehlt Microsoft, dem Dienstprinzipal nur den für die von Ihnen vorgesehene Rolle erforderlichen Mindestzugriff zu erteilen. Weitere Informationen finden Sie unter [Verwalten des Zugriffs auf einen Azure Machine Learning-Arbeitsbereich](how-to-assign-roles.md).

    ```azurecli-interactive
    az ml workspace share -w your-workspace-name -g your-resource-group-name --user your-sp-object-id --role owner
    ```

    Dieser Aufruf erzeugt bei Erfolg keine Ausgabe.

### <a name="use-a-service-principal-from-the-sdk"></a>Verwenden eines Dienstprinzipals im SDK

Verwenden Sie zum Authentifizieren bei Ihrem Arbeitsbereich mit dem Dienstprinzipal über das SDK den Klassenkonstruktor `ServicePrincipalAuthentication`. Geben Sie dabei die Werte an, die Sie beim Erstellen des Dienstanbieters als Parameter erhalten haben. Der Parameter `tenant_id` wird `tenantId` oben zugeordnet, `service_principal_id` wird `clientId` zugeordnet und `service_principal_password` `clientSecret`.

```python
from azureml.core.authentication import ServicePrincipalAuthentication

sp = ServicePrincipalAuthentication(tenant_id="your-tenant-id", # tenantID
                                    service_principal_id="your-client-id", # clientId
                                    service_principal_password="your-client-secret") # clientSecret
```

Die Variable `sp` enthält jetzt ein Authentifizierungsobjekt, das Sie direkt im SDK verwenden können. Im Allgemeinen empfiehlt es sich, die oben verwendeten IDs/Geheimnisse in Umgebungsvariablen zu speichern, wie im folgenden Code gezeigt. Das Speichern in Umgebungsvariablen verhindert, dass die Informationen versehentlich in ein GitHub-Repository eingecheckt werden.

```python
import os

sp = ServicePrincipalAuthentication(tenant_id=os.environ['AML_TENANT_ID'],
                                    service_principal_id=os.environ['AML_PRINCIPAL_ID'],
                                    service_principal_password=os.environ['AML_PRINCIPAL_PASS'])
```

Bei automatisierten Workflows, die in Python ausgeführt werden und das SDK primär verwenden, können Sie dieses Objekt in den meisten Fällen unverändert für die Authentifizierung verwenden. Der folgende Code dient zur Authentifizierung bei Ihrem Arbeitsbereich mit dem von Ihnen erstellten Authentifizierungsobjekt.

```python
from azureml.core import Workspace

ws = Workspace.get(name="ml-example",
                   auth=sp,
                   subscription_id="your-sub-id")
ws.get_details()
```

### <a name="use-a-service-principal-from-the-azure-cli"></a>Verwenden eines Dienstprinzipals in der Azure CLI

Sie können einen Dienstprinzipal für Azure CLI-Befehle verwenden. Weitere Informationen finden Sie unter [Anmelden mithilfe eines Dienstprinzipals](/cli/azure/create-an-azure-service-principal-azure-cli?preserve-view=true&view=azure-cli-latest#sign-in-using-a-service-principal).

### <a name="use-a-service-principal-with-the-rest-api-preview"></a>Verwenden eines Dienstprinzipals mit der REST-API (Vorschau)

Der Dienstprinzipal kann auch zum Authentifizieren bei der Azure Machine Learning-[REST-API](/rest/api/azureml/) (Vorschau) verwendet werden. Sie verwenden den [Fluss zum Gewähren von Clientanmeldeinformationen](../active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow.md) von Azure Active Directory, der dienstübergreifende Aufrufe für die monitorlose Authentifizierung in automatisierten Workflows ermöglicht. Die Beispiele sind mit der [ADAL-Bibliothek](../active-directory/azuread-dev/active-directory-authentication-libraries.md) sowohl in Python als auch in Node.js implementiert. Sie können aber auch jede Open-Source-Bibliothek verwenden, die OpenID Connect 1.0 unterstützt.

> [!NOTE]
> MSAL.js ist eine neuere Bibliothek als ADAL. Allerdings können Sie mit MSAL.js keine dienstübergreifende Authentifizierung mit Clientanmeldeinformationen durchführen, da es sich in erster Linie um eine clientseitige Bibliothek handelt, die für die an einen bestimmten Benutzer gebundene interaktive Authentifizierung/Authentifizierung über die Benutzeroberfläche gedacht ist. Wir empfehlen für die Erstellung automatisierter Workflows mit der REST-API die Verwendung von ADAL wie unten gezeigt.

#### <a name="nodejs"></a>Node.js

Verwenden Sie die folgenden Schritte, um ein Authentifizierungstoken mithilfe von Node.js zu generieren. Führen Sie in Ihrer Umgebung `npm install adal-node` aus. Verwenden Sie dann `tenantId`, `clientId` und `clientSecret` aus dem Dienstprinzipal, den Sie in den obigen Schritten erstellt haben, als Werte für die übereinstimmenden Variablen im folgenden Skript.

```javascript
const adal = require('adal-node').AuthenticationContext;

const authorityHostUrl = 'https://login.microsoftonline.com/';
const tenantId = 'your-tenant-id';
const authorityUrl = authorityHostUrl + tenantId;
const clientId = 'your-client-id';
const clientSecret = 'your-client-secret';
const resource = 'https://management.azure.com/';

const context = new adal(authorityUrl);

context.acquireTokenWithClientCredentials(
  resource,
  clientId,
  clientSecret,
  (err, tokenResponse) => {
    if (err) {
      console.log(`Token generation failed due to ${err}`);
    } else {
      console.dir(tokenResponse, { depth: null, colors: true });
    }
  }
);
```

Die Variable `tokenResponse` ist ein Objekt, das das Token und zugehörige Metadaten enthält, wie die Ablaufzeit. Token sind 1 Stunde gültig und können durch erneutes Ausführen desselben Anrufs aktualisiert werden, um ein neues Token zu erhalten. Der folgende Ausschnitt zeigt eine Beispielantwort.

```javascript
{
    tokenType: 'Bearer',
    expiresIn: 3599,
    expiresOn: 2019-12-17T19:15:56.326Z,
    resource: 'https://management.azure.com/',
    accessToken: "random-oauth-token",
    isMRRT: true,
    _clientId: 'your-client-id',
    _authority: 'https://login.microsoftonline.com/your-tenant-id'
}
```

Verwenden Sie die Eigenschaft `accessToken`, um das Authentifizierungstoken abzurufen. In der [Rest-API-Dokumentation](https://github.com/microsoft/MLOps/tree/master/examples/AzureML-REST-API) finden Sie Beispiele dazu, wie Sie das Token zum Ausführen von API-Aufrufen verwenden.

#### <a name="python"></a>Python

Verwenden Sie die folgenden Schritte, um ein Authentifizierungstoken mithilfe von Python zu generieren. Führen Sie in Ihrer Umgebung `pip install adal` aus. Verwenden Sie dann `tenantId`, `clientId` und `clientSecret` aus dem Dienstprinzipal, den Sie in den obigen Schritten erstellt haben, als Werte für die entsprechenden Variablen im folgenden Skript.

```python
from adal import AuthenticationContext

client_id = "your-client-id"
client_secret = "your-client-secret"
resource_url = "https://login.microsoftonline.com"
tenant_id = "your-tenant-id"
authority = "{}/{}".format(resource_url, tenant_id)

auth_context = AuthenticationContext(authority)
token_response = auth_context.acquire_token_with_client_credentials("https://management.azure.com/", client_id, client_secret)
print(token_response)
```

Die Variable `token_response` ist ein Wörterbuch, das das Token und zugehörige Metadaten enthält, wie die Ablaufzeit. Token sind 1 Stunde gültig und können durch erneutes Ausführen desselben Anrufs aktualisiert werden, um ein neues Token zu erhalten. Der folgende Ausschnitt zeigt eine Beispielantwort.

```python
{
    'tokenType': 'Bearer',
    'expiresIn': 3599,
    'expiresOn': '2019-12-17 19:47:15.150205',
    'resource': 'https://management.azure.com/',
    'accessToken': 'random-oauth-token',
    'isMRRT': True,
    '_clientId': 'your-client-id',
    '_authority': 'https://login.microsoftonline.com/your-tenant-id'
}
```

Verwenden Sie `token_response["accessToken"]`, um das Authentifizierungstoken abzurufen. In der [Rest-API-Dokumentation](https://github.com/microsoft/MLOps/tree/master/examples/AzureML-REST-API) finden Sie Beispiele dazu, wie Sie das Token zum Ausführen von API-Aufrufen verwenden.

#### <a name="java"></a>Java

Rufen Sie in Java über einen standardmäßigen REST-Aufruf das Bearertoken ab:

```java
String tenantId = "your-tenant-id";
String clientId = "your-client-id";
String clientSecret = "your-client-secret";
String resourceManagerUrl = "https://management.azure.com";

HttpRequest tokenAuthenticationRequest = tokenAuthenticationRequest(tenantId, clientId, clientSecret, resourceManagerUrl);

HttpClient client = HttpClient.newBuilder().build();
Gson gson = new Gson();
HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
if (response.statusCode == 200)
{
     body = gson.fromJson(body, AuthenticationBody.class);

    // ... etc ... 
}
// ... etc ...

static HttpRequest tokenAuthenticationRequest(String tenantId, String clientId, String clientSecret, String resourceManagerUrl){
    String authUrl = String.format("https://login.microsoftonline.com/%s/oauth2/token", tenantId);
    String clientIdParam = String.format("client_id=%s", clientId);
    String resourceParam = String.format("resource=%s", resourceManagerUrl);
    String clientSecretParam = String.format("client_secret=%s", clientSecret);

    String bodyString = String.format("grant_type=client_credentials&%s&%s&%s", clientIdParam, resourceParam, clientSecretParam);

    HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create(authUrl))
            .POST(HttpRequest.BodyPublishers.ofString(bodyString))
            .build();
    return request;
}

class AuthenticationBody {
    String access_token;
    String token_type;
    int expires_in;
    String scope;
    String refresh_token;
    String id_token;
    
    AuthenticationBody() {}
}
```

Der oben stehende Code müsste Ausnahmen und andere Statuscodes als `200 OK` verarbeiten, aber er veranschaulicht das Muster: 

- Sie verwenden Client-ID und Clientgeheimnis, um zu überprüfen, ob Ihr Programm Zugriff haben sollte.
- Sie verwenden die Mandanten-ID, um anzugeben, wo `login.microsoftonline.com` suchen soll.
- Sie verwenden Azure Resource Manager als Quelle des Autorisierungstokens.

## <a name="web-service-authentication"></a>Webdienstauthentifizierung

Die von Azure Machine Learning erstellten Modellbereitstellungen bieten zwei Authentifizierungsmethoden:

* **Schlüsselbasiert:** Für die Authentifizierung beim Webdienst wird ein statischer Schlüssel verwendet.
* **Tokenbasiert:** Ein temporäres Token muss aus dem Arbeitsbereich abgerufen und für die Authentifizierung beim Webdienst verwendet werden. Dieses Token läuft nach einer bestimmten Zeit ab und muss aktualisiert werden, um weiter mit dem Webdienst zu arbeiten.

    > [!NOTE]
    > Die tokenbasierte Authentifizierung ist nur bei der Bereitstellung in Azure Kubernetes Service verfügbar.

### <a name="key-based-web-service-authentication"></a>Schlüsselbasierte Webdienstauthentifizierung

Für in Azure Kubernetes Service (AKS) bereitgestellte Webdienste ist die schlüsselbasierte Authentifizierung standardmäßig *aktiviert*. Für in Azure Container Instances (ACI) bereitgestellte Dienste ist die schlüsselbasierte Authentifizierung standardmäßig *deaktiviert* , aber Sie können sie aktivieren, indem Sie bei der Erstellung des ACI-Webdiensts `auth_enabled=True` festlegen. Der folgende Code zeigt ein Beispiel für die Erstellung einer ACI-Bereitstellungskonfiguration mit aktivierter schlüsselbasierter Authentifizierung.

```python
from azureml.core.webservice import AciWebservice

aci_config = AciWebservice.deploy_configuration(cpu_cores = 1,
                                                memory_gb = 1,
                                                auth_enabled=True)
```

Anschließend können Sie die benutzerdefinierte ACI-Konfiguration bei der Bereitstellung mit der `Model`-Klasse verwenden.

```python
from azureml.core.model import Model, InferenceConfig


inference_config = InferenceConfig(entry_script="score.py",
                                   environment=myenv)
aci_service = Model.deploy(workspace=ws,
                       name="aci_service_sample",
                       models=[model],
                       inference_config=inference_config,
                       deployment_config=aci_config)
aci_service.wait_for_deployment(True)
```

Verwenden Sie `aci_service.get_keys()`, um die Authentifizierungsschlüssel abzurufen. Um einen Schlüssel neu zu generieren, verwenden Sie die Sie die Funktion `regen_key()` und übergeben entweder **Primary** oder **Secondary**.

```python
aci_service.regen_key("Primary")
# or
aci_service.regen_key("Secondary")
```

Weitere Informationen zur Authentifizierung bei einem bereitgestellten Modell finden Sie unter [Nutzen eines als Webdienst bereitgestellten Azure Machine Learning-Modells](how-to-consume-web-service.md).

### <a name="token-based-web-service-authentication"></a>Tokenbasierte Webdienstauthentifizierung

Wenn Sie die Tokenauthentifizierung für einen Webdienst aktivieren, müssen die Benutzer ein Azure Machine Learning-JSON-Webtoken für den Webdienst bereitstellen, um auf den Dienst zugreifen zu können. Das Token läuft nach einem bestimmten Zeitrahmen ab und muss zum Durchführen weiterer Aufrufe aktualisiert werden.

* Die Tokenauthentifizierung ist **standardmäßig deaktiviert** , wenn die Bereitstellung in Azure Kubernetes Service erfolgt.
* Die Tokenauthentifizierung wird **nicht unterstützt** , wenn die Bereitstellung in Azure Container Instances erfolgt.
* Die Tokenauthentifizierung **und die schlüsselbasierte Authentifizierung können nicht gleichzeitig verwendet werden**.

Wenn Sie die Tokenauthentifizierung steuern möchten, verwenden Sie beim Erstellen oder Aktualisieren einer Bereitstellung den Parameter `token_auth_enabled`:

```python
from azureml.core.webservice import AksWebservice
from azureml.core.model import Model, InferenceConfig

# Create the config
aks_config = AksWebservice.deploy_configuration()

#  Enable token auth and disable (key) auth on the webservice
aks_config = AksWebservice.deploy_configuration(token_auth_enabled=True, auth_enabled=False)

aks_service_name ='aks-service-1'

# deploy the model
aks_service = Model.deploy(workspace=ws,
                           name=aks_service_name,
                           models=[model],
                           inference_config=inference_config,
                           deployment_config=aks_config,
                           deployment_target=aks_target)

aks_service.wait_for_deployment(show_output = True)
```

Bei aktivierter Tokenauthentifizierung können Sie mithilfe der Methode `get_token` ein JSON Web Token (JWT) und dessen Ablaufzeit abrufen:

> [!TIP]
> Wenn Sie zum Abrufen des Tokens einen Dienstprinzipal verwenden, der nur über den für das Abrufen eines Tokens erforderlichen Mindestzugriff verfügen soll, weisen Sie ihm für den Arbeitsbereich die Rolle **Leser** zu.

```python
token, refresh_by = aks_service.get_token()
print(token)
```

> [!IMPORTANT]
> Nach Ablauf der für `refresh_by` festgelegten Zeit müssen Sie ein neues Token anfordern. Wenn Sie Token außerhalb des Python SDK aktualisieren müssen, besteht eine Möglichkeit darin, die REST-API mit Dienstprinzipalauthentifizierung zu verwenden, um den `service.get_token()`-Aufruf in regelmäßigen Abständen durchzuführen, wie zuvor beschrieben.
>
> Wir empfehlen Ihnen dringend, den Azure Machine Learning-Arbeitsbereich in der gleichen Region zu erstellen wie den Azure Kubernetes Service-Cluster.
>
> Im Zuge der Tokenauthentifizierung richtet der Webdienst einen Aufruf an die Region, in der Ihr Azure Machine Learning-Arbeitsbereich erstellt wird. Wenn die Region Ihres Arbeitsbereichs nicht verfügbar ist, können Sie kein Token für Ihren Webdienst abrufen, auch dann nicht, wenn sich Ihr Cluster in einer anderen Region befindet als Ihr Arbeitsbereich. Die Azure AD-Authentifizierung ist dann erst wieder verfügbar, wenn die Region Ihres Arbeitsbereichs wieder verfügbar wird.
>
> Außerdem wirkt sich die Entfernung zwischen der Region Ihres Clusters und der Region Ihres Arbeitsbereichs direkt auf die Tokenabrufdauer aus.

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden von Geheimnissen in Trainingsausführungen](how-to-use-secrets-in-runs.md)
* [Trainieren und Bereitstellen eines Bildklassifizierungsmodells](tutorial-train-models-with-aml.md)
* [Nutzen eines als Webdienst bereitgestellten Azure Machine Learning-Modells](how-to-consume-web-service.md)