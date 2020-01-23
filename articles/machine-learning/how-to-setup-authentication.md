---
title: Einrichten der Authentifizierung
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie die Authentifizierung für verschiedene Ressourcen und Workflows in Azure Machine Learning einrichten und konfigurieren. Es gibt mehrere Möglichkeiten, die Authentifizierung innerhalb des Dienstes zu konfigurieren und zu nutzen – angefangen bei der einfachen Authentisierung auf Basis der Benutzeroberfläche für Entwicklungs- oder Testzwecke bis hin zur vollständigen Dienstprinzipalauthentifizierung des Azure Active Directory-Dienstes.
services: machine-learning
author: trevorbye
ms.author: trbye
ms.reviewer: trbye
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 12/17/2019
ms.openlocfilehash: ce85c45d80a776af84a0987cfbc3f496c2bbb72b
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2020
ms.locfileid: "75893958"
---
# <a name="set-up-authentication-for-azure-machine-learning-resources-and-workflows"></a>Einrichten der Authentifizierung für Azure Machine Learning-Ressourcen und -Workflows
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie die Authentifizierung für verschiedene Ressourcen und Workflows in Azure Machine Learning einrichten und konfigurieren. Es gibt mehrere Möglichkeiten, die Authentifizierung für den Dienst zu konfigurieren und zu nutzen – angefangen bei der einfachen Authentisierung auf Basis der Benutzeroberfläche für Entwicklungs- oder Testzwecke bis hin zur vollständigen Dienstprinzipalauthentifizierung des Azure Active Directory-Dienstes. Dieser Artikel erklärt zudem die Unterschiede in der Funktionsweise der Webdienstauthentifizierung sowie die Authentifizierung bei der Azure Machine Learning-REST-API.

Diese Anleitung zeigt, wie Sie die folgenden Aufgaben ausführen:

* Verwenden der interaktiven Benutzeroberflächenauthentifizierung für Tests/Entwicklung
* Einrichten der Dienstprinzipalauthentifizierung
* Authentifizieren bei Ihrem Arbeitsbereich
* Abrufen des OAuth 2.0-Tokens vom Typ Bearer für die Azure Machine Learning-Rest-API
* Grundlegendes zur Webdienstauthentifizierung

Eine allgemeine Übersicht zu Sicherheit und Authentifizierung in Azure Machine Learning finden Sie in diesem [Konzeptartikel](concept-enterprise-security.md).

## <a name="prerequisites"></a>Voraussetzungen

* Erstellen Sie einen [Azure Machine Learning-Arbeitsbereich](how-to-manage-workspace.md).
* [Konfigurieren Sie Ihre Entwicklungsumgebung](how-to-configure-environment.md) für die Installation des Azure Machine Learning SDK, oder verwenden Sie eine [Azure Machine Learning-Notebook-VM](concept-azure-machine-learning-architecture.md#compute-instance) mit bereits installiertem SDK.

## <a name="interactive-authentication"></a>Interaktive Authentifizierung

Die meisten Beispiele in der Dokumentation zu diesem Dienst verwenden die interaktive Authentifizierung in Jupyter-Notebooks als einfache Methode zum Testen und zur Demonstration. Dies ist eine vereinfachte Möglichkeit, das zu testen, was Sie entwickeln. Es gibt zwei Funktionsaufrufe, die Sie automatisch zur Eingabe eines auf der Benutzeroberfläche basierenden Authentifizierungsflusses auffordern.

Wenn Sie die Funktion `from_config()` aufrufen, wird die Aufforderung ausgegeben.

```python
from azureml.core import Workspace
ws = Workspace.from_config()
```

Die Funktion `from_config()` sucht nach einer JSON-Datei, die ihre Informationen zur Arbeitsbereichsverbindung enthält. Sie können die Verbindungsdetails auch explizit mit dem `Workspace`-Konstruktor angeben. Dieser fordert auch zur interaktiven Authentifizierung auf. Beide Aufrufe sind gleichwertig.

```python
ws = Workspace(subscription_id="your-sub-id",
               resource_group="your-resource-group-id",
               workspace_name="your-workspace-name"
              )
```

Wenn Sie Zugriff auf mehrere Mandanten haben, müssen Sie möglicherweise die-Klasse importieren und explizit definieren, auf welchen Mandanten Sie abzielen. Wenn Sie den Konstruktor für `InteractiveLoginAuthentication` aufrufen, werden Sie außerdem aufgefordert, sich ähnlich wie bei den oben aufgeführten Aufrufen anzumelden.

```python
from azureml.core.authentication import InteractiveLoginAuthentication
interactive_auth = InteractiveLoginAuthentication(tenant_id="your-tenant-id")
```

Die interaktive Authentifizierung ist zwar nützlich zum Testen und Lernen, hilft Ihnen aber nicht beim Aufbau automatisierter oder monitorloser Workflows. Für automatisierte Prozesse, die das SDK verwenden, ist die Einrichtung der Dienstprinzipalauthentifizierung der beste Ansatz.

## <a name="set-up-service-principal-authentication"></a>Einrichten der Dienstprinzipalauthentifizierung

Dieser Prozess ist erforderlich, um eine von einer spezifischen Benutzeranmeldung entkoppelte Authentifizierung zu ermöglichen. Damit können Sie sich in automatisierten Workflows beim Azure Machine Learning-Python SDK authentifizieren. Die Dienstprinzipalauthentifizierung ermöglicht Ihnen außerdem die [Authentifizierung bei der Rest-API](#azure-machine-learning-rest-api-auth).

Um die Dienstprinzipalauthentifizierung einzurichten, erstellen Sie zunächst eine App-Registrierung im Azure Active Directory und gewähren dann Ihrer App rollenbasierten Zugriff auf Ihren ML-Arbeitsbereich. Die einfachste Möglichkeit dafür ist die [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/) im Azure-Portal. Nachdem Sie sich beim Portal angemeldet haben, klicken Sie auf das Symbol `>_` oben rechts auf der Seite neben Ihrem Namen, um die Shell zu öffnen.

Wenn Sie die Cloud Shell noch nie in Ihrem Azure-Account verwendet haben, müssen Sie eine Speicherkontoressource zum Speichern aller geschriebenen Dateien erstellen. In der Regel fallen für dieses Speicherkonto nur geringe monatliche Kosten an. Installieren Sie außerdem über den folgenden Befehl die Machine Learning-Erweiterung, wenn Sie sie zuvor noch nicht verwendet haben.

```azurecli-interactive
az extension add -n azure-cli-ml
```

> [!NOTE]
> Sie müssen ein Administrator für das Abonnement sein, um die folgenden Schritte ausführen zu können.

Führen Sie anschließend den folgenden Befehl aus, um den Dienstprinzipal zu erstellen. Benennen Sie es in diesem Fall mit **ml-auth**.

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

Führen Sie als Nächstes den folgenden Befehl aus, um die Details zum gerade erstellten Dienstprinzipal abzurufen. Verwenden Sie dazu den Wert `clientId` von oben als Eingabe für den Parameter `--id`.

```azurecli-interactive
az ad sp show --id your-client-id
```

Nachfolgend finden Sie ein vereinfachtes der JSON-Ausgabe des Befehls. Notieren Sie sich den Wert in Feld `objectId`, da Sie diesen für den nächsten Schritt benötigen.

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

Verwenden Sie jetzt den folgenden Befehl, um den Zugriff auf den Dienstprinzipal Ihrem Machine Learning-Arbeitsbereich zuzuweisen. Sie benötigen den Namen Ihres Arbeitsbereichs und den Namen der Ressourcengruppe für die Parameter `-w` und `-g`. Verwenden Sie für den Parameter `--user` den Wert `objectId` aus dem vorherigen Schritt. Mit dem Parameter `--role` können Sie die Zugriffsrolle für den Dienstprinzipal festlegen. Im Allgemeinen verwenden Sie entweder **Besitzer** oder **Mitwirkender**. Beide haben Schreibzugriff auf vorhandene Ressourcen wie Computecluster und Datenspeicher, aber nur der **Besitzer** kann diese Ressourcen bereitstellen. 

```azurecli-interactive
az ml workspace share -w your-workspace-name -g your-resource-group-name --user your-sp-object-id --role owner
```

Dieser Befehl erzeugt zwar keine Ausgabe, aber Sie haben jetzt die Dienstprinzipalauthentifizierung für Ihren Arbeitsbereich eingerichtet.

## <a name="authenticate-to-your-workspace"></a>Authentifizieren bei Ihrem Arbeitsbereich

Nachdem Sie die Dienstprinzipalauthentifizierung aktiviert haben, können Sie sich bei Ihrem Arbeitsbereich im SDK authentifizieren, ohne sich physisch als Benutzer anzumelden. Verwenden Sie den Konstruktor der `ServicePrincipalAuthentication`-Klasse, und verwenden Sie als Parameter die Werte, die Sie in den vorherigen Schritten erhalten haben. Der Parameter `tenant_id` wird `tenantId` oben zugeordnet, `service_principal_id` wird `clientId` zugeordnet und `service_principal_password` `clientSecret`.

```python
from azureml.core.authentication import ServicePrincipalAuthentication

sp = ServicePrincipalAuthentication(tenant_id="your-tenant-id", # tenantID
                                    service_principal_id="your-client-id", # clientId
                                    service_principal_password="your-client-secret") # clientSecret
```

Die Variable `sp` enthält jetzt ein Authentifizierungsobjekt, das Sie direkt im SDK verwenden können. Im Allgemeinen empfiehlt es sich, die oben verwendeten IDs/Geheimnisse in Umgebungsvariablen zu speichern, wie im folgenden Code gezeigt.

```python
import os 

sp = ServicePrincipalAuthentication(tenant_id=os.environ['AML_TENANT_ID'],
                                    service_principal_id=os.environ['AML_PRINCIPAL_ID'],
                                    service_principal_password=os.environ['AML_PRINCIPAL_PASS'])
```

Bei automatisierten Workflows, die in Python ausgeführt werden und das SDK primär verwenden, können Sie dieses Objekt in den meisten Fällen unverändert für die Authentifizierung verwenden. Der folgende Code dient zur Authentifizierung bei Ihrem Arbeitsbereich mit dem soeben erstellten Authentifizierungsobjekt.

```python
from azureml.core import Workspace

ws = Workspace.get(name="ml-example", 
                   auth=sp,
                   subscription_id="your-sub-id")
ws.get_details()
```

## <a name="azure-machine-learning-rest-api-auth"></a>Authentifizierung bei der Azure Machine Learning-REST-API

Der in den vorherigen Schritten erstellte Dienstprinzipal kann auch zum Authentifizieren bei der Azure Machine Learning-[Rest-API](https://docs.microsoft.com/rest/api/azureml/) verwendet werden. Sie verwenden den [Fluss zum Gewähren von Clientanmeldeinformationen](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-client-creds-grant-flow) von Azure Active Directory, der dienstübergreifende Aufrufe für die monitorlose Authentifizierung in automatisierten Workflows ermöglicht. Die Beispiele sind mit der [ADAL-Bibliothek](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries) sowohl in Python als auch in Node.js implementiert. Sie können aber auch jede Open-Source-Bibliothek verwenden, die OpenID Connect 1.0 unterstützt. 

> [!NOTE]
> MSAL.js ist eine neuere Bibliothek als ADAL. Allerdings können Sie mit MSAL.js keine dienstübergreifende Authentifizierung mit Clientanmeldeinformationen durchführen, da es sich in erster Linie um eine clientseitige Bibliothek handelt, die für die an einen bestimmten Benutzer gebundene interaktive Authentifizierung/Authentifizierung über die Benutzeroberfläche gedacht ist. Wir empfehlen für die Erstellung automatisierter Workflows mit der REST-API die Verwendung von ADAL wie unten gezeigt.

### <a name="nodejs"></a>Node.js

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

Die Variable `tokenResponse` ist ein Objekt, das das Token und zugehörige Metadaten enthält, wie die Ablaufzeit. Token sind 1 Stunde gültig und können durch erneutes Ausführen desselben Anrufs aktualisiert werden, um ein neues Token zu erhalten. Nachfolgend finden Sie eine Beispielantwort.

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

### <a name="python"></a>Python 

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

Die Variable `token_response` ist ein Wörterbuch, das das Token und zugehörige Metadaten enthält, wie die Ablaufzeit. Token sind 1 Stunde gültig und können durch erneutes Ausführen desselben Anrufs aktualisiert werden, um ein neues Token zu erhalten. Nachfolgend finden Sie eine Beispielantwort.

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

## <a name="web-service-authentication"></a>Webdienstauthentifizierung

Webdienste in Azure Machine Learning verwenden ein anderes Authentifizierungsmuster als das oben beschriebene. Der einfachste Weg, die Authentifizierung bei den bereitgestellten Webdiensten durchzuführen, ist die Verwendung von **schlüsselbasierter Authentifizierung**, die statische Authentifizierungsschlüssel vom Typ Bearer generiert, die nicht aktualisiert werden müssen. Wenn Sie sich nur bei einem bereitgestellten Webdienst authentifizieren müssen, müssen Sie nicht die oben beschriebene Dienstprinzipalauthentifizierung einrichten.

Für in Azure Kubernetes Service bereitgestellte Webdienste ist die schlüsselbasierte Autorisierung standardmäßig *aktiviert*. Für in Azure Container Instances bereitgestellte Dienste ist die schlüsselbasierte Authentifizierung standardmäßig *deaktiviert*, aber Sie können sie aktivieren, indem Sie bei der Erstellung des ACI-Webdiensts `auth_enabled=True`festlegen. Nachfolgend finden Sie ein Beispiel für die Erstellung einer ACI-Bereitstellungskonfiguration mit aktivierter schlüsselbasierter Authentifizierung.

```python
from azureml.core.webservice import AciWebservice

aci_config = AciWebservice.deploy_configuration(cpu_cores = 1,
                                                memory_gb = 1,
                                                auth_enable=True)
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

Webdienste unterstützen auch die tokenbasierte Authentifizierung, aber nur für Azure Kubernetes Service-Bereitstellungen. Weitere Informationen zum Authentifizieren finden Sie in der [Anleitung](how-to-consume-web-service.md) zum Verwenden von Webdiensten.

### <a name="token-based-web-service-authentication"></a>Tokenbasierte Webdienstauthentifizierung

Wenn Sie die Tokenauthentifizierung für einen Webdienst aktivieren, müssen die Benutzer ein Azure Machine Learning-JSON-Webtoken für den Webdienst bereitstellen, um auf den Dienst zugreifen zu können. Das Token läuft nach einem bestimmten Zeitrahmen ab und muss zum Durchführen weiterer Aufrufe aktualisiert werden.

* Die Tokenauthentifizierung ist **standardmäßig deaktiviert**, wenn die Bereitstellung in Azure Kubernetes Service erfolgt.
* Die Tokenauthentifizierung wird **nicht unterstützt**, wenn die Bereitstellung in Azure Container Instances erfolgt.

Wenn Sie die Tokenauthentifizierung steuern möchten, verwenden Sie beim Erstellen oder Aktualisieren einer Bereitstellung den Parameter `token_auth_enabled`.

Bei aktivierter Tokenauthentifizierung können Sie mithilfe der Methode `get_token` ein JSON Web Token (JWT) und dessen Ablaufzeit abrufen:

```python
token, refresh_by = service.get_token()
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

* [Trainieren und Bereitstellen eines Bildklassifizierungsmodells](tutorial-train-models-with-aml.md)
* [Nutzen eines als Webdienst bereitgestellten Azure Machine Learning-Modells](how-to-consume-web-service.md)
