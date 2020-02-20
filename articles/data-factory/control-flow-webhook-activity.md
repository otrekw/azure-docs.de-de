---
title: Webhook-Aktivität in Azure Data Factory
description: Die Webhook-Aktivität setzt die Ausführung der Pipeline erst fort, nachdem sie das angefügte Dataset mit bestimmten vom Benutzer angegebenen Kriterien überprüft hat.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 03/25/2019
ms.openlocfilehash: 70c67a99274eaedc5592c7b90b1ef80a3a17acf8
ms.sourcegitcommit: 9add86fb5cc19edf0b8cd2f42aeea5772511810c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2020
ms.locfileid: "77110004"
---
# <a name="webhook-activity-in-azure-data-factory"></a>Webhook-Aktivität in Azure Data Factory
Sie können mithilfe einer Webhook-Aktivität die Ausführung von Pipelines über Ihren benutzerdefinierten Code steuern. Mithilfe der Webhook-Aktivität können Kunden einen Endpunkt aufrufen und eine Rückruf-URL übergeben. Die Pipelineausführung wartet, bis der Rückruf aufgerufen wurde, bevor sie mit der nächsten Aktivität fortfährt.

## <a name="syntax"></a>Syntax

```json

{
    "name": "MyWebHookActivity",
    "type": "WebHook",
    "typeProperties": {
        "method": "POST",
        "url": "<URLEndpoint>",
        "headers": {
            "Content-Type": "application/json"
        },
        "body": {
            "key": "value"
        },
        "timeout": "00:03:00",
        "authentication": {
            "type": "ClientCertificate",
            "pfx": "****",
            "password": "****"
        }
    }
}

```


## <a name="type-properties"></a>Typeigenschaften



Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich
-------- | ----------- | -------------- | --------
name | Name der Webhook-Aktivität | String | Ja |
type | Muss auf **WebHook** festgelegt werden. | String | Ja |
method | REST-API-Methode für den Zielendpunkt. | Eine Zeichenfolge. Unterstützte Typen: ‚POST‘ | Ja |
url | Zielendpunkt und Pfad | Zeichenfolge (oder Ausdruck mit resultType der Zeichenfolge). | Ja |
headers | Header, die in der Anforderung gesendet werden. So legen Sie beispielsweise die Sprache und den Typ für eine Anforderung fest: „headers“: { „Accept-Language“: „en-us“, „Content-Type“: „application/json“ }. | Zeichenfolge (oder ein Ausdruck mit resultType der Zeichenfolge) | Ja, der Content-Type-Header ist erforderlich. "headers":{ "Content-Type":"application/json"} |
body | Stellt die an den Endpunkt gesendete Nutzlast dar. | Gültiges JSON (oder Ausdruck mit „resultType“ des JSON). Weitere Informationen finden Sie in den Details zum Schema der Anforderungsnutzlast im Abschnitt [Schema der Anforderungsnutzlast](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fdata-factory%2Fcontrol-flow-web-activity%23request-payload-schema&amp;data=02%7C01%7Cshlo%40microsoft.com%7Cde517eae4e7f4f2c408d08d6b167f6b1%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636891457414397501&amp;sdata=ljUZv5csQQux2TT3JtTU9ZU8e1uViRzuX5DSNYkL0uE%3D&amp;reserved=0). | Ja |
authentication | Die zum Aufrufen des Endpunkts verwendete Authentifizierungsmethode. Unterstützte Typen sind „Basic“ oder „ClientCertificate“. Weitere Informationen finden Sie im Abschnitt [Authentifizierung](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fdata-factory%2Fcontrol-flow-web-activity%23authentication&amp;data=02%7C01%7Cshlo%40microsoft.com%7Cde517eae4e7f4f2c408d08d6b167f6b1%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636891457414397501&amp;sdata=GdA1%2Fh2pAD%2BSyWJHSW%2BSKucqoAXux%2F4L5Jgndd3YziM%3D&amp;reserved=0). Wenn keine Authentifizierung erforderlich ist, schließen Sie diese Eigenschaft aus. | Zeichenfolge (oder ein Ausdruck mit resultType der Zeichenfolge) | Nein |
timeout | Legt fest, wie lange die Aktivität darauf warten soll, dass &#39;callBackUri&#39; aufgerufen wird. Legt fest, wie lange die Aktivität darauf warten soll, dass „callBackUri“ aufgerufen wird. Der Standardwert ist 10 Minuten („00:10:00“). Das Format ist „Timespan“, d.h. „d.hh:mm:ss“. | String | Nein |
Report status on callback | Ermöglicht es dem Benutzer, den Status „Fehler“ der Webhook-Aktivität zu melden, wodurch die Aktivität als „fehlgeschlagen“ markiert wird. | Boolean | Nein |

## <a name="authentication"></a>Authentifizierung

Im Folgenden finden Sie die unterstützten Authentifizierungstypen in der Webhookaktivität.

### <a name="none"></a>Keine

Wenn keine Authentifizierung erforderlich ist, schließen Sie die Eigenschaft „authentication“ nicht ein.

### <a name="basic"></a>Basic

Geben Sie Benutzername und Kennwort für die Standardauthentifizierung an.

```json
"authentication":{
   "type":"Basic",
   "username":"****",
   "password":"****"
}
```

### <a name="client-certificate"></a>Clientzertifikat

Geben Sie die Base64-codierten Inhalte einer PFX-Datei und das Kennwort an.

```json
"authentication":{
   "type":"ClientCertificate",
   "pfx":"****",
   "password":"****"
}
```

### <a name="managed-identity"></a>Verwaltete Identität

Geben Sie den Ressourcen-URI an, für den das Zugriffstoken mithilfe der verwalteten Identität für die Data Factory angefordert wird. Verwenden Sie zum Aufrufen der Azure-Ressourcenverwaltungs-API `https://management.azure.com/`. Weitere Informationen zur Funktion verwalteter Identitäten finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](/azure/active-directory/managed-identities-azure-resources/overview).

```json
"authentication": {
    "type": "MSI",
    "resource": "https://management.azure.com/"
}
```

> [!NOTE]
> Wenn Ihre Data Factory mit einem Git-Repository konfiguriert ist, müssen Sie Ihre Anmeldeinformationen in Azure Key Vault speichern, um die Standardauthentifizierung oder die Clientzertifikatauthentifizierung zu verwenden. Azure Data Factory speichert keine Kennwörter in Git.

## <a name="additional-notes"></a>Zusätzliche Hinweise

Azure Data Factory übergibt die zusätzliche Eigenschaft „callBackUri“ im Textkörper an den URL-Endpunkt und erwartet, dass dieser URI vor dem angegebenen Timeoutwert aufgerufen wird. Wenn der URI nicht aufgerufen wird, schlägt die Aktivität mit dem Status „TimedOut“ fehl.

Die Webhook-Aktivität selbst schlägt fehl, wenn beim Aufruf des benutzerdefinierten Endpunkts ein Fehler auftritt. Im Textkörper des Rückrufs kann jede beliebige Fehlermeldung hinzugefügt und in einer nachfolgenden Aktivität verwendet werden.

Der an den Rückruf-URI zurückgegebene Textkörper sollte gültiges JSON sein. Sie müssen den Content-Type-Header auf `application/json` festlegen.

Wenn Sie die Option „Report status on callback“ (Status bei Rückruf melden) verwenden, müssen Sie dem Textkörper bei einem Rückruf den folgenden Ausschnitt hinzufügen:

```
{
    "Output": {
        // output object will be used in activity output
        "testProp": "testPropValue"
    },
    "Error": {
        // Optional, set it when you want to fail the activity
        "ErrorCode": "testErrorCode",
        "Message": "error message to show in activity error"
    },
    "StatusCode": "403" // when status code is >=400, activity will be marked as failed
}
```



## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter anderen Ablaufsteuerungsaktivitäten, die von Data Factory unterstützt werden:

- [Aktivität „If Condition“](control-flow-if-condition-activity.md)
- [Aktivität „Pipeline ausführen“](control-flow-execute-pipeline-activity.md)
- [ForEach-Aktivität](control-flow-for-each-activity.md)
- [Aktivität „Metadaten abrufen“](control-flow-get-metadata-activity.md)
- [Lookup-Aktivität](control-flow-lookup-activity.md)
- [Webaktivität](control-flow-web-activity.md)
- [Until-Aktivität](control-flow-until-activity.md)
