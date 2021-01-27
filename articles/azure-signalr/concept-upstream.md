---
title: Upstreameinstellungen in Azure SignalR Service
description: Erhalten Sie eine Einführung in die Upstreameinstellungen und Protokolle von Upstreamnachrichten.
author: chenyl
ms.service: signalr
ms.topic: conceptual
ms.date: 06/11/2020
ms.author: chenyl
ms.openlocfilehash: 6752a9564dc0d9351d1c21f5be14eb626186ac0d
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2021
ms.locfileid: "98724055"
---
# <a name="upstream-settings"></a>Upstreameinstellungen

Upstream ist eine Previewfunktion, die es Azure SignalR Service ermöglicht, Nachrichten und Verbindungsereignisse im serverlosen Modus an eine Reihe von Endpunkten zu senden. Sie können Upstream verwenden, um eine Hubmethode von Clients im serverlosen Modus aufzurufen und Endpunkte benachrichtigen zu lassen, wenn Clientverbindungen hergestellt oder getrennt werden.

> [!NOTE]
> Nur im serverlosen Modus können Upstreameinstellungen konfiguriert werden.

## <a name="details-of-upstream-settings"></a>Einzelheiten der Upstreameinstellungen

Die Upstreameinstellungen bestehen aus einer Liste von Elementen mit einer bestimmten Reihenfolge. Jedes Element besteht aus Folgendem:

* Eine URL-Vorlage, die angibt, wohin Nachrichten gesendet werden
* Eine Reihe von Regeln
* Authentifizierungskonfigurationen 

Wenn das angegebene Ereignis eintritt, werden die Regeln eines Elements nacheinander der Reihe nach geprüft. Die Nachrichten werden an die Upstream-URL des ersten übereinstimmenden Elements gesendet.

### <a name="url-template-settings"></a>URL-Vorlageneinstellungen

Sie können die URL parametrisieren, um verschiedene Muster zu unterstützen. Es gibt drei vordefinierte Parameter:

|Vordefinierter Parameter|BESCHREIBUNG|
|---------|---------|
|{hub}| Ein Hub ist ein Konzept von Azure SignalR Service. Ein Hub ist eine Isolationseinheit. Der Bereich der Benutzer und der Nachrichtenübermittlung ist auf einen Hub beschränkt.|
|{category}| Eine Kategorie kann einen der folgenden Werte aufweisen: <ul><li>**connections**: Ereignisse der Verbindungslebensdauer. Sie wird ausgelöst, wenn eine Clientverbindung hergestellt oder getrennt wird. Sie umfasst verbundene und nicht verbundene Ereignisse.</li><li>**messages**: Wird ausgelöst, wenn Clients eine Hubmethode aufrufen. Sie umfasst alle anderen Ereignisse, mit Ausnahme der Ereignisse in der Kategorie **connections**.</li></ul>|
|{event}| Für die Kategorie **messages** ist ein Ereignis das Ziel in der [Aufrufnachricht](https://github.com/dotnet/aspnetcore/blob/master/src/SignalR/docs/specs/HubProtocol.md#invocation-message-encoding), die Clients senden. Für die Kategorie **connections** werden nur *connected* und *disconnected* verwendet.|

Diese vordefinierten Parameter können im URL-Muster verwendet werden. Parameter werden durch einen angegebenen Wert ersetzt, wenn Sie die Upstream-URL auswerten. Beispiel: 
```
http://host.com/{hub}/api/{category}/{event}
```
Wenn eine Clientverbindung im „chat“-Hub verbunden ist, wird eine Nachricht an diese URL gesendet:
```
http://host.com/chat/api/connections/connected
```
Wenn ein Client im „chat“-Hub die Hubmethode `broadcast` aufruft, wird eine Nachricht an diese URL gesendet:
```
http://host.com/chat/api/messages/broadcast
```

### <a name="key-vault-secret-reference-in-url-template-settings"></a>Verweis auf Key Vault-Geheimnis in den URL-Vorlageneinstellungen

Die Upstream-URL ist keine Verschlüsselung im Ruhezustand. Sie sollten für die Speicherung von vertraulichen Informationen Key Vault verwenden, dessen Zugriffskontrolle eine höhere Sicherheit bietet. Grundsätzlich können Sie die verwaltete Identität von Azure SignalR Service aktivieren, dann eine Leseberechtigung für eine Key Vault-Instanz erteilen und einen Key Vault-Verweis anstelle von Klartext im Upstream-URL-Muster verwenden.

1. Fügen Sie eine systemseitig oder benutzerseitig zugewiesene Identität hinzu. Mehr dazu finden Sie unter [How to add managed identity in Azure Portal](./howto-use-managed-identity.md#add-a-system-assigned-identity) (Hinzufügen von verwalteter Identität im Azure-Portal)

2. Erteilen Sie der verwalteten Identität in den Zugriffsrichtlinien im Key Vault Leseberechtigung für das Geheimnis. Weitere Informationen finden Sie unter [Zuweisen einer Key Vault-Zugriffsrichtlinie über das Azure-Portal](../key-vault/general/assign-access-policy-portal.md).

3. Ersetzen Sie den vertraulichen Text im Upstream-URL-Muster durch die Syntax `{@Microsoft.KeyVault(SecretUri=<secret-identity>)}`.

> [!NOTE]
> Der geheime Inhalt wird nur dann erneut gelesen, wenn Sie die Upstreameinstellungen oder die verwaltete Identität ändern. Vergewissern Sie sich, dass Sie der verwalteten Identität Leseberechtigung für das Geheimnis erteilt haben, bevor Sie den Key Vault-Geheimnisverweis verwenden.

### <a name="rule-settings"></a>Regeleinstellungen

Sie können Regeln für *Hubregeln*, *Kategorieregeln* und *Ereignisregeln* separat festlegen. Die Abgleichsregel unterstützt drei Formate. Nehmen wir als Beispiel die Ereignisregeln:
- Verwenden Sie ein Sternchen (*), um alle Ereignisse zu vergleichen.
- Verwenden Sie ein Komma (,), um mehrere Ereignisse zu verknüpfen. Beispielsweise entspricht `connected, disconnected` den verbundenen und nicht verbundenen Ereignissen.
- Verwenden Sie den vollständigen Namen des Ereignisses, um das Ereignis abzugleichen. Beispielsweise entspricht `connected` dem verbundenen Ereignis.

> [!NOTE]
> Wenn Sie Azure Functions und den [SignalR-Trigger](../azure-functions/functions-bindings-signalr-service-trigger.md) verwenden, macht SignalR-Trigger einen einzelnen Endpunkt im folgenden Format verfügbar: `<Function_App_URL>/runtime/webhooks/signalr?code=<API_KEY>`.
> Sie können einfach **URL-Vorlageneinstellungen** mit dieser URL konfigurieren und für die **Regeleinstellungen** die Standardwerte beibehalten. Ausführliche Informationen zum Finden von `<Function_App_URL>` und `<API_KEY>` finden Sie unter [SignalR Service-Integration](../azure-functions/functions-bindings-signalr-service-trigger.md#signalr-service-integration).

### <a name="authentication-settings"></a>Authentifizierungseinstellungen

Sie können die Authentifizierung für jedes Upstreameinstellungselement separat konfigurieren. Wenn Sie die Authentifizierung konfigurieren, wird ein Token im `Authentication`-Header der Upstreamnachricht festgelegt. Derzeit unterstützt Azure SignalR Service die folgenden Authentifizierungstypen:
- `None`
- `ManagedIdentity`

Wenn Sie `ManagedIdentity` auswählen, müssen Sie im Voraus eine verwaltete Identität in Azure SignalR Service aktivieren und optional eine Ressource angeben. Weitere Informationen finden Sie unter [Verwaltete Identitäten für Azure SignalR Service](howto-use-managed-identity.md).

## <a name="create-upstream-settings-via-the-azure-portal"></a>Erstellen von Upstreameinstellungen über das Azure-Portal

1. Wechseln Sie zu Azure SignalR Service.
2. Wählen Sie **Einstellungen** aus, und legen Sie **Dienstmodus** auf **Serverlos** fest. Die folgenden Upstreameinstellungen werden angezeigt:

    :::image type="content" source="media/concept-upstream/upstream-portal.png" alt-text="Upstreameinstellungen":::

3. Fügen Sie unter **Upstream-URL-Muster** URLs hinzu. Dann zeigen Einstellungen, z. B. **Hubregeln**, den Standardwert an.
4. Um Einstellungen für **Hubregeln**, **Ereignisregeln**, **Kategorieregeln** und **Upstreamauthentifizierung** festzulegen, wählen Sie den Wert von **Hubregeln** aus. Es wird eine Seite angezeigt, auf der Sie die Einstellungen bearbeiten können:

    :::image type="content" source="media/concept-upstream/upstream-detail-portal.png" alt-text="Upstream-Einstellungsdetails":::

5. Stellen Sie sicher, dass Sie zunächst eine verwaltete Identität aktiviert haben, um die **Upstreamauthentifizierung** festzulegen. Wählen Sie dann **Verwaltete Identität verwenden** aus. Entsprechend Ihren Anforderungen können Sie unter der **Authentifizierungsressourcen-ID** beliebige Optionen auswählen. Weitere Informationen finden Sie unter [Verwaltete Identitäten für Azure SignalR Service](howto-use-managed-identity.md).

## <a name="create-upstream-settings-via-resource-manager-template"></a>Erstellen von Upstreameinstellungen über Resource Manager-Vorlage

Um Upstreameinstellungen unter Verwendung einer [Azure Resource Manager-Vorlage](../azure-resource-manager/templates/overview.md) zu erstellen, legen Sie die `upstream`-Eigenschaft in der `properties`-Eigenschaft fest. Der folgende Codeausschnitt zeigt, wie die `upstream`-Eigenschaft zum Erstellen und Aktualisieren von Upstreameinstellungen festgelegt werden kann.

```JSON
{
  "properties": {
    "upstream": {
      "templates": [
        {
          "UrlTemplate": "http://host.com/{hub}/api/{category}/{event}",
          "EventPattern": "*",
          "HubPattern": "*",
          "CategoryPattern": "*",
          "Auth": {
            "Type": "ManagedIdentity",
            "ManagedIdentity": {
              "Resource": "<resource>"
            }
          }
        }
      ]
    }
  }
}
```

## <a name="serverless-protocols"></a>Serverlose Protokolle

Azure SignalR Service sendet Nachrichten an Endpunkte, die den folgenden Protokollen folgen. Sie können [SignalR Service-Triggerbindung](../azure-functions/functions-bindings-signalr-service-trigger.md) mit der Funktions-App verwenden, die diese Protokolle für Sie verarbeitet.

### <a name="method"></a>Methode

POST

### <a name="request-header"></a>Anforderungsheader

|Name |BESCHREIBUNG|
|---------|---------|
|X-ASRS-Connection-Id |Die Verbindungs-ID für die Clientverbindung.|
|X-ASRS-Hub |Der Hub, zu dem die Clientverbindung gehört.|
|X-ASRS-Category |Die Kategorie, zu der die Nachricht gehört.|
|X-ASRS-Event |Das Ereignis, zu dem die Nachricht gehört.|
|X-ASRS-Signature |Ein Hash-based Message Authentication Code (HMAC), der zur Validierung verwendet wird. Weitere Informationen finden Sie unter [Signatur](#signature).|
|X-ASRS-User-Claims |Eine Gruppe von Ansprüchen der Clientverbindung.|
|X-ASRS-User-Id |Die Benutzeridentität des Clients, der die Nachricht sendet.|
|X-ASRS-Client-Query |Die Abfrage der Anforderung, wenn sich Clients mit dem Dienst verbinden.|
|Authentifizierung |Ein optionales Token, wenn Sie `ManagedIdentity` verwenden. |

### <a name="request-body"></a>Anforderungstext

#### <a name="connected"></a>Verbunden

Content-Type: application/json

#### <a name="disconnected"></a>Getrennt

Content-Type: `application/json`

|Name  |type  |BESCHREIBUNG  |
|---------|---------|---------|
|Fehler |Zeichenfolge |Die Fehlermeldung einer geschlossenen Verbindung. Leer, wenn Verbindungen ohne Fehler geschlossen werden.|

#### <a name="invocation-message"></a>Aufrufnachricht

Content-Type: `application/json` oder `application/x-msgpack`

|Name  |type  |BESCHREIBUNG  |
|---------|---------|---------|
|InvocationId |Zeichenfolge | Eine optionale Zeichenfolge, die eine Aufrufnachricht darstellt. Details finden Sie in den [Aufrufen](https://github.com/dotnet/aspnetcore/blob/master/src/SignalR/docs/specs/HubProtocol.md#invocations).|
|Ziel |Zeichenfolge | Dasselbe wie das Ereignis und das Ziel in einer [Aufrufnachricht](https://github.com/dotnet/aspnetcore/blob/master/src/SignalR/docs/specs/HubProtocol.md#invocation-message-encoding). |
|Argumente |Array von Objekten |Ein Array, das Argumente enthält, die auf die in `Target` genannte Methode anzuwenden sind. |

### <a name="signature"></a>Signatur

Der Dienst berechnet den SHA256-Code für den `X-ASRS-Connection-Id`-Wert, indem er sowohl den primären als auch den sekundären Zugriffsschlüssel als `HMAC`-Schlüssel verwendet. Der Dienst legt ihn im `X-ASRS-Signature`-Header fest, wenn HTTP-Upstreamanforderungen ausgeführt werden:
```
Hex_encoded(HMAC_SHA256(accessKey, connection-id))
```

## <a name="next-steps"></a>Nächste Schritte

- [Verwaltete Identitäten für Azure SignalR Service](howto-use-managed-identity.md)
- [Azure Functions-Entwicklung und -Konfiguration mit Azure SignalR Service](signalr-concept-serverless-development-config.md)
- [Verarbeiten von Nachrichten von SignalR Service (Triggerbindung)](../azure-functions/functions-bindings-signalr-service-trigger.md)
- [Beispiel einer SignalR Service-Auslöserbindung](https://github.com/aspnet/AzureSignalR-samples/tree/master/samples/BidirectionChat)