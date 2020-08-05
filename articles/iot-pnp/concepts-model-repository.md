---
title: Konzepte des Azure IoT-Modellrepositorys | Microsoft-Dokumentation
description: In diesem Artikel lernen Lösungsentwickler und IT-Experten die grundlegenden Konzepte des Azure IoT-Modellrepositorys.
author: JimacoMS3
ms.author: v-jambra
ms.date: 07/24/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 53ae5bf15c303f26d48550734f46e69ef1fcdd75
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87352010"
---
# <a name="azure-iot-model-repository"></a>Azure IoT-Modellrepository

Das Azure IoT-Modellrepository ermöglicht Geräteentwicklern das Verwalten und Freigeben von IoT Plug & Play-Gerätemodellen. Bei den Gerätemodellen handelt es sich um JSON LD-Dokumente, die mit der [Digital Twins-Modellierungssprache (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md) definiert werden. Die im Modellrepositorydienst gespeicherten Modelle können entweder privat über die Zugriffssteuerung oder öffentlich ohne jegliche Authentifizierung mit Lösungsentwicklern geteilt werden, um die IoT Plug & Play-Cloudlösung zu integrieren und zu entwickeln.

Für den Zugriff auf das Modellrepository stehen Ihnen folgende Optionen zur Verfügung:

- [Das Portal für das Azure IoT-Modellrepository](https://aka.ms/iotmodelrepo)
- [Die REST-API für das Azure IoT-Modellrepository](https://docs.microsoft.com/rest/api/iothub/digitaltwinmodelrepositoryservice/getmodelasync/getmodelasync)
- [Die Azure CLI-Befehle für das Azure IoT-Modellrepository](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/pnp?view=azure-cli-latest)

## <a name="public-models"></a>Öffentliche Modelle

Die im Modellrepository gespeicherten öffentlichen Digital Twins-Modelle sind für alle Benutzer verfügbar und können ohne Authentifizierung in Anwendungen integriert werden. Darüber hinaus ermöglichen die öffentlichen Modelle ein offenes Ökosystem für Gerätehersteller und Lösungsentwickler, über das sie ihre IoT Plug & Play-Gerätemodelle teilen und wiederverwenden können.

Anweisungen zum Veröffentlichen eines Modells im Modellrepository finden Sie im Abschnitt [Veröffentlichen eines Modells](#publish-a-model) unter **Unternehmensmodelle**.

So zeigen Sie ein öffentliches Modell mithilfe des Modellrepositoryportals an:

1. Rufen Sie das [Portal für das Azure IoT-Modellrepository](https://aka.ms/iotmodelrepo) auf.

1. Klicken Sie auf **Öffentliche Modelle anzeigen**.

    ![Öffentliche Modelle anzeigen](./media/concepts-model-repository/public-models.png)

Informationen zum programmgesteuerten Anzeigen eines öffentlichen Modells mithilfe der REST-API finden Sie in der REST-API-Dokumentation unter [Abrufen von Modellen](https://docs.microsoft.com/rest/api/iothub/digitaltwinmodelrepositoryservice/getmodelasync/getmodelasync).

```csharp
var httpClient = new HttpClient();
httpClient.BaseAddress = new Uri("<url>");

var modelId = "dtmi:com:mxchip:model;1";
var response = await httpClient.GetAsync($"/models/{modelId}?api-version=2020-05-01-preview").ConfigureAwait(false);
```

Informationen zum Anzeigen eines öffentlichen Modells mithilfe der CLI finden Sie im Artikel zum Azure CLI-Befehl [Get Model](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/pnp/model?view=azure-cli-latest#ext-azure-iot-az-iot-pnp-model-show).

## <a name="company-models"></a>Unternehmensmodelle

Das Unternehmensmodellrepository ist ein Mandant im Azure IoT-Modellrepository, mit dem Ihre Organisation Digital Twins-Modelle erstellen und verwalten kann, die von Benutzern in Ihrem Unternehmen bzw. Ihrer Organisation erstellt wurden. Unternehmensmodelle stehen nur authentifizierten Benutzern Ihres Unternehmens bzw. Ihrer Organisation zur Verfügung. Ein Mandantenadministrator des Modellrepositorys kann Berechtigungen zuweisen und den Zugriff auf die Modelle im Unternehmensmodellrepository anderer Benutzer im Unternehmen oder der Organisation steuern.

### <a name="set-up-your-company-model-repository"></a>Einrichten Ihres Unternehmensmodellrepositorys

Verwenden Sie Ihr *Geschäfts-, Uni- oder Schulkonto für Azure Active Directory (Azure AD)* , um auf das Modellrepository zuzugreifen. Wenn Ihre Organisation bereits über einen Azure AD-Mandanten verfügt, können Sie Benutzerkonten und Dienstprinzipale dieses Azure AD-Mandanten verwenden.

Informationen zum Einrichten eines Azure AD-Mandanten und zum Erstellen eines Benutzers oder Dienstprinzipals in einem Azure AD-Mandanten finden Sie im Abschnitt [Weitere Informationen](#additional-information).

- Wenn Sie der erste Benutzer Ihrer Organisation sind, der auf das Modellrepository zugreift oder sich beim Portal anmeldet, wird Ihnen die Rolle **Mandantenadministrator** zugewiesen. Mit dieser Rolle können Sie anderen Benutzern im Repositorymandanten Ihrer Organisation Rollen zuweisen.

- Ihren können von einem **Mandantenadministrator** andere Rollen zugewiesen werden, z. B. zum **Lesen von Modellen** oder **Erstellen von Modellen**.

### <a name="understand-access-management"></a>Informationen zur Zugriffsverwaltung

In der folgenden Tabelle werden die unterstützten Funktionen im Unternehmensmodellrepository und ihre zugehörigen Berechtigungen aufgeführt:

| Funktion  | Berechtigung| BESCHREIBUNG|
|-------------|-----------|------------|
|Modelle lesen|Modelle lesen|Standardmäßig können alle Benutzer im Unternehmensmandanten ihre Unternehmensmodelle anzeigen. Darüber hinaus kann der Benutzer ebenfalls die privaten Modelle anzeigen, die von anderen Unternehmen für ihn freigegeben wurden.|
|Verwalten des Zugriffs|Verwalten des Zugriffs|Mit dieser Berechtigung können Sie die Benutzerrollenzuweisung für andere Benutzer in der Organisation verwalten (hinzufügen oder entfernen).|
|Erstellen von Modellen|Erstellen von Modellen|Mit dieser Berechtigung können Sie Modelle im Modellrepository des Unternehmens erstellen.|
|Modelle veröffentlichen|Modelle veröffentlichen|Mit dieser Berechtigung können Sie Modelle veröffentlichen, damit sie öffentlich von allen Benutzern angezeigt werden können.|

In der folgenden Tabelle werden die unterstützten Rollen und ihre Funktionen im Modellrepository zusammengefasst, die für die Zugriffsverwaltung verwendet werden können.

|Role|Funktion|
|----|----------|
|TenantAdministrator|Zugriff verwalten, Modelle lesen|
|Creator|Modelle erstellen, Modelle lesen|
|Herausgeber|Modelle veröffentlichen, Modelle lesen|

#### <a name="passing-a-security-token-when-accessing-company-models-with-a-rest-api"></a>Übergeben eines Sicherheitstokens beim Zugreifen auf Unternehmensmodelle mit einer REST-API

Wenn Sie die REST-APIs aufrufen, um Unternehmensmodelle zu verwalten, die privat oder freigegeben sind, müssen Sie ein Autorisierungstoken für den Benutzer oder Dienstprinzipal im JWT-Format bereitstellen. Im Abschnitt [Weitere Informationen](#additional-information) erfahren Sie, wie Sie ein JWT-Token für einen Benutzer oder ein Dienstprinzipal abrufen.

Das JWT-Token muss an den HTTP-Autorisierungsheader in der API übergeben werden, wenn Unternehmensmodelle oder freigegebene Modelle verwendet werden. Das JWT-Token ist für öffentliche Modelle nicht erforderlich.

```csharp
// sample token
var authorizationToken = "eyJhbGciOiJIUzI1NiIsInR5cCTI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c";
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", authorizationToken);
```

### <a name="view-company-or-shared-models"></a>Anzeigen von Unternehmensmodellen oder freigegebenen Modellen

Sie müssen Mitglied der Rolle *Leser* des Repositorymandanten sein, oder das Modell muss für Sie freigegeben sein, damit Sie dieses lesen können. Sie können eine Liste nicht veröffentlichter Modelle und eine Liste veröffentlichter Modelle anzeigen, die für Sie freigegeben wurden. Benutzer können standardmäßig die Modelle ihres Unternehmens, von anderen Unternehmen für den Benutzer freigegebene Modelle und alle öffentlichen Modelle lesen.

So zeigen Sie ein Unternehmensmodell oder ein freigegebenes Modell über das Portal an:

1. Melden Sie sich beim [Portal für das Azure IoT-Modellrepository](https://aka.ms/iotmodelrepo) an.

1. Erweitern Sie im linken Bereich **Unternehmensmodelle** – **Nicht veröffentlicht**.

    ![Anzeigen von Unternehmensmodellen](./media/concepts-model-repository/view-company-models.png)

1. Erweitern Sie im linken Bereich **Freigegebene Modelle – Nicht veröffentlicht**.

    ![Freigegebene Modelle anzeigen](./media/concepts-model-repository/view-shared-models.png)

Informationen zum Anzeigen eines Unternehmensmodells oder eines freigegebenen Modells mithilfe der REST-API finden Sie in der REST-API-Dokumentation unter [Abrufen von Modellen](https://docs.microsoft.com/rest/api/iothub/digitaltwinmodelrepositoryservice/getmodelasync/getmodelasync). Informationen zum Übergeben eines JWT-Autorisierungsheaders in der HTTP-Anforderung finden Sie unter [Übergeben eines Sicherheitstokens beim Zugriff auf Unternehmensmodelle mit einer REST-API](#passing-a-security-token-when-accessing-company-models-with-a-rest-api).

```csharp
var modelId = "dtmi:com:mxchip:model;1";
var response = await httpClient.GetAsync($"/models/{modelId}?api-version=2020-05-01-preview").ConfigureAwait(false);
```

Informationen zum Anzeigen eines Unternehmensmodells oder eines freigegebenen Modells mithilfe der CLI finden Sie im Artikel zum Azure CLI-Befehl [Get Model](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/pnp/model?view=azure-cli-latest#ext-azure-iot-az-iot-pnp-model-show).

### <a name="manage-roles"></a>Verwalten von Rollen

Der Mandantenadministrator kann Benutzern im Repositorymandanten Rollen zuweisen, damit diese private Modelle für die Organisation erstellen, Modelle veröffentlichen oder Modelle für andere Benutzer verwalten können.

So fügen Sie einen Benutzer mithilfe des Portals zu einer Rolle des Modellrepositorymandanten hinzu:

1. Melden Sie sich beim [Portal für das Azure IoT-Modellrepository](https://aka.ms/iotmodelrepo) an.

1. Klicken Sie im linken Bereich auf **Zugriffsverwaltung**, und klicken Sie dann auf **+ Hinzufügen**. Geben Sie im Bereich **Berechtigung hinzufügen** die geschäftliche E-Mail-Adresse des Benutzers ein, den Sie zur Rolle hinzufügen möchten:

    ![Hinzufügen der geschäftlichen E-Mail-Adresse](./media/concepts-model-repository/add-user.png)

1. Wählen Sie die Rolle aus der Dropdownliste **Rolle** aus, die Sie dem Benutzer zuweisen möchten. Wählen Sie dann **Speichern** aus:

    ![Rolle auswählen](./media/concepts-model-repository/choose-role.png)

### <a name="upload-a-model"></a>Hochladen eines Modells

Sie müssen ein Mitglied der Rolle **Ersteller** des Repositorymandanten sein, um ein Modell in das Unternehmensmodellrepository hochladen zu können.

Diese Modelle werden nicht veröffentlicht und sind standardmäßig nur für Benutzer innerhalb Ihrer Organisation zugänglich. Sie können auch nicht veröffentlichte Modelle für externe Benutzer freigeben.

Hochgeladene Modelle sind unveränderlich.

Die Modell-IDs für diese Modelle müssen über alle Repositorymandanten hinweg für alle hochgeladenen Modelle global eindeutig sein.

So laden Sie ein Modell mithilfe des Portal hoch:

1. Melden Sie sich beim [Portal für das Azure IoT-Modellrepository](https://aka.ms/iotmodelrepo) an.

1. Erweitern Sie **Unternehmensmodelle** im linken Bereich, und wählen Sie dann **Modell erstellen** aus. Klicken Sie dann auf **JSON importieren**.

    ![Erstellen eines Modells](./media/concepts-model-repository/create-model.png)

1. Wählen Sie die Datei, die Sie hochladen möchten. Wenn das Portal Ihr Modell erfolgreich validiert, klicken Sie auf **Speichern**.

Informationen zum Hochladen eines Modells mithilfe der REST-API finden Sie im API-Artikel [Erstellen eines Modells](https://docs.microsoft.com/rest/api/iothub/digitaltwinmodelrepositoryservice/createorupdateasync/createorupdateasync). Informationen zum Übergeben eines JWT-Autorisierungsheaders in der HTTP-Anforderung finden Sie unter [Übergeben eines Sicherheitstokens beim Zugriff auf Unternehmensmodelle mit einer REST-API](#passing-a-security-token-when-accessing-company-models-with-a-rest-api).

```csharp
var httpContent = new StringContent(jsonLdModel, Encoding.UTF8, "application/json");
var modelId = "dtmi:com:mxchip:model;1";
var response = await httpClient.PutAsync($"/models/{modelId}?api-version=2020-05-01-preview", httpContent).ConfigureAwait(false);
```

Informationen zum Hochladen eines Modells mithilfe der CLI finden Sie im Artikel zum Azure CLI-Befehl zum [Erstellen eines Modells](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/pnp/model?view=azure-cli-latest#ext-azure-iot-az-iot-pnp-model-create).

### <a name="publish-a-model"></a>Veröffentlichen eines Modells

Zum Veröffentlichen eines Modells müssen die folgenden Anforderungen erfüllt sein:

1. Ihre Organisation muss Mitglied des [Microsoft Partner Network](https://docs.microsoft.com/partner-center/) sein, um ein Modell zu veröffentlichen. Informationen zum Erstellen eines Partner Center-Kontos finden Sie unter [Erstellen eines Partner Center-Kontos](https://docs.microsoft.com/partner-center/mpn-create-a-partner-center-account). Sobald Ihr Konto genehmigt wurde, können Sie Ihre Modelle veröffentlichen. Weitere Informationen finden Sie in den [häufig gestellten Fragen zu Partner Center](https://support.microsoft.com/help/4340639/partner-center-account-faqs).

2. Der Benutzer muss Mitglied der Rolle *Herausgeber* des Repositorymandanten sein.

Modelle, die von Benutzern in Ihrer Organisation erstellt und veröffentlicht werden, werden als *veröffentlichte Modelle* angezeigt. Diese Modelle sind öffentlich und können unter **Öffentliche Modelle** von beliebigen Personen gefunden werden.

So veröffentlichen Sie ein Modell mithilfe des Portals:

1. Melden Sie sich beim [Portal für das Azure IoT-Modellrepository](https://aka.ms/iotmodelrepo) an.

2. Erweitern Sie im linken Bereich **Unternehmensmodelle**, und wählen Sie das Modell aus, das Sie veröffentlichen möchten. Wählen Sie anschließend **Veröffentlichen** aus.

    ![Veröffentlichen des Modells](./media/concepts-model-repository/publish-model.png)

> [!NOTE]
> Wenn Ihnen eine Benachrichtigung angezeigt wird, die angibt, dass Sie über keine Microsoft Partner Network-ID verfügen, befolgen Sie die Registrierungsschritte in der Benachrichtigung. Weitere Informationen finden Sie in den Anforderungen am Anfang dieses Abschnitts.

Informationen zum Veröffentlichen eines Modells mithilfe der REST-API finden Sie in der REST-API-Dokumentation unter [Veröffentlichen von Modellen](https://docs.microsoft.com/rest/api/iothub/digitaltwinmodelrepositoryservice/createorupdateasync/createorupdateasync). Geben Sie den Abfragezeichenfolgenparameter `update-metadata=true` an, um ein Modell mithilfe der REST-API zu veröffentlichen. Informationen zum Übergeben eines JWT-Autorisierungsheaders in der HTTP-Anforderung finden Sie unter [Übergeben eines Sicherheitstokens beim Zugriff auf Unternehmensmodelle mit einer REST-API](#passing-a-security-token-when-accessing-company-models-with-a-rest-api).

Informationen zum Veröffentlichen eines Modells mithilfe der CLI finden Sie im Artikel zum Azure CLI-Befehl zum [Veröffentlichen eines Modells](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/pnp/model?view=azure-cli-latest#ext-azure-iot-az-iot-pnp-model-publish).

### <a name="share-a-model"></a>Freigeben eines Modells

Sie können Unternehmensmodelle freigeben, die Sie mit Benutzern externer Organisationen erstellt haben. Auf diese Weise können Sie Projektmitarbeitern ermöglichen, Lösungen mit Ihren privaten Unternehmensmodellen anzuzeigen und zu entwickeln.

Angenommen, ein Gerätehersteller möchte Modelle privat für das Unternehmen verwalten. Für seine Kunden ist erforderlich, dass die Gerätefunktionen vertraulich bleiben.

Durch die Freigabe von Modellen zwischen Unternehmen oder Organisationen wird der sichere Zugriff auf Modelle ermöglicht, die nicht öffentlich sind.

So geben Sie ein Unternehmensmodell mithilfe des Portals frei:

- Wenn Sie der Ersteller eines Modells sind, sind die Schaltflächen **Freigeben** und **Freigeben für** verfügbar, wenn Sie das Modell im Abschnitt **Unternehmensmodelle** anzeigen.

    ![Modell freigeben](./media/concepts-model-repository/share-model.png)

- Klicken Sie auf **Freigeben**, um das Modell für einen externen Benutzer freizugeben. Geben Sie im Bereich **Modell freigeben** die E-Mail-Adresse des externen Benutzers ein, und klicken Sie dann auf **Speichern**.

- Klicken Sie auf **Freigegeben für**, um die Benutzer anzuzeigen, für die Sie das Modell freigegeben haben.

- Sie können die Freigabe des Modells für einen spezifischen Benutzer aufheben, indem Sie den Benutzer aus der Benutzerliste im Bereich **Freigegeben für** auswählen. Klicken Sie dann auf **Entfernen**, und bestätigen Sie Ihre Auswahl, wenn Sie dazu aufgefordert werden.

    ![Beenden der Freigabe](./media/concepts-model-repository/stop-sharing.png)

## <a name="additional-information"></a>Zusätzliche Informationen

Die folgenden Artikel können sich für die Arbeit mit Azure AD als nützlich erweisen:

- Informationen zum Erstellen eines neuen Azure AD-Mandanten finden Sie unter [Erstellen eines neuen Mandanten in Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant). Die meisten Organisationen verfügen bereits über Azure AD-Mandanten.

- Informationen zum Hinzufügen von Benutzern oder Gastbenutzern zu einem Azure AD-Mandanten finden Sie unter [Hinzufügen oder Löschen von Benutzern mit Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/add-users-azure-active-directory).

- Informationen zum Hinzufügen eines Dienstprinzipals zu einem Azure AD-Mandanten finden Sie unter [Erstellen einer Azure AD-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff über das Portal](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

- Informationen zum Abrufen eines JWT-Tokens von Azure AD zum Aufrufen von REST-APIs finden Sie unter [Abrufen eines Tokens von Azure AD zum Autorisieren von Anforderungen einer Clientanwendung](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-app).

## <a name="next-steps"></a>Nächste Schritte

Es wird empfohlen, dass Sie sich als Nächstes die [IoT Plug & Play-Architektur](concepts-architecture.md) ansehen.