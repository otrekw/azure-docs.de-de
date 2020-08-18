---
title: Sichern des Zugriffs und der Daten
description: Schützen des Zugriffs auf Eingaben, Ausgaben, anforderungsbasierte Trigger, Verlaufsprotokolle, Verwaltungsaufgaben und des Zugriffs auf andere Ressourcen in Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: rarayudu, logicappspm
ms.topic: conceptual
ms.date: 08/11/2020
ms.openlocfilehash: e7199b6d54a0150845bfc09c38e002e6cc298ee7
ms.sourcegitcommit: d8b8768d62672e9c287a04f2578383d0eb857950
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2020
ms.locfileid: "88066728"
---
# <a name="secure-access-and-data-in-azure-logic-apps"></a>Schützen des Zugriffs und der Daten in Azure Logic Apps

Azure Logic Apps nutzt [Azure Storage](../storage/index.yml) zum Speichern und automatischen [Verschlüsseln von ruhenden Daten](../security/fundamentals/encryption-atrest.md). Diese Verschlüsselung schützt Ihre Daten und unterstützt Sie beim Einhalten der Sicherheits- und Complianceanforderungen Ihrer Organisation. Azure Storage verwendet standardmäßig von Microsoft verwaltete Schlüssel, um Ihre Daten zu verschlüsseln. Weitere Informationen finden Sie unter [Azure Storage-Verschlüsselung für ruhende Daten](../storage/common/storage-service-encryption.md).

Um den Zugriff stärker zu steuern und vertrauliche Daten in Azure Logic Apps zu schützen, können Sie zusätzliche Sicherheit in diesen Bereichen einrichten:

* [Zugriff auf anforderungsbasierte Trigger](#secure-triggers)
* [Zugriff auf Logik-App-Vorgänge](#secure-operations)
* [Zugriff auf Eingaben und Ausgaben von Ausführungsverläufen](#secure-run-history)
* [Zugriff auf Parametereingaben](#secure-action-parameters)
* [Zugriff auf Dienste und Systeme, die von Logik-Apps aus aufgerufen werden](#secure-outbound-requests)
* [Blockieren des Erstellens von Verbindungen für bestimmte Connectors](#block-connections)
* [Isolationsanleitung für Logik-Apps](#isolation-logic-apps)
* [Azure-Sicherheitsbaseline für Azure Logic Apps](../logic-apps/security-baseline.md)

Weitere Informationen zur Sicherheit in Azure finden Sie in diesen Themen:

* [Übersicht über die Azure-Verschlüsselung](../security/fundamentals/encryption-overview.md)
* [Azure-Datenverschlüsselung ruhender Daten](../security/fundamentals/encryption-atrest.md)
* [Einführung zum Azure Security-Vergleichstest](../security/benchmarks/overview.md)

<a name="secure-triggers"></a>

## <a name="access-to-request-based-triggers"></a>Zugriff auf anforderungsbasierte Trigger

Wenn Ihre Logik-App einen anforderungsbasierten Trigger verwendet, der eingehende Aufrufe oder Anforderungen empfängt, z. B. den Trigger [Anforderung](../connectors/connectors-native-reqres.md) oder [Webhook](../connectors/connectors-native-webhook.md), können Sie den Zugriff so einschränken, dass nur autorisierte Clients die Logik-App aufrufen können. Alle von einer Logik-App empfangenen Anforderungen werden mit dem Transport Layer Security-Protokoll (TLS), zuvor als Secure Sockets Layer (SSL) bezeichnet, verschlüsselt und gesichert.

Mit diesen Optionen können Sie den Zugriff auf diesen Triggertyp absichern:

* [Generieren von Shared Access Signatures](#sas)
* [Aktivieren der Azure Active Directory Open Authentication (Azure AD OAuth)](#enable-oauth)
* [Einschränken eingehender IP-Adressen](#restrict-inbound-ip-addresses)
* [Hinzufügen von Azure Active Directory OAuth oder anderen Sicherheitsfeatures](#add-authentication)

<a name="sas"></a>

### <a name="generate-shared-access-signatures-sas"></a>Generieren von Shared Access Signatures (SAS)

Jeder Anforderungsendpunkt für eine Logik-App weist in der URL des Endpunkts eine [Shared Access Signature (SAS)](/rest/api/storageservices/constructing-a-service-sas) im folgenden Format auf:

`https://<request-endpoint-URI>sp=<permissions>sv=<SAS-version>sig=<signature>`

Jede URL enthält die Abfrageparameter `sp`, `sv` und `sig`, wie in dieser Tabelle beschrieben:

| Query parameter (Abfrageparameter) | BESCHREIBUNG |
|-----------------|-------------|
| `sp` | Gibt die Berechtigungen für die zulässigen HTTP-Methoden an. |
| `sv` | Gibt die SAS-Version an, die zum Generieren der Signatur verwendet werden soll. |
| `sig` | Gibt die Signatur an, die für die Authentifizierung des Zugriffs auf den Trigger verwendet werden soll. Diese Signatur wird mit dem SHA256-Algorithmus mit einem geheimen Zugriffsschlüssel für alle URL-Pfade und -Eigenschaften generiert. Dieser Schlüssel wird nie verfügbar gemacht oder veröffentlicht. Er bleibt verschlüsselt und wird mit der Logik-App gespeichert. Die Logik-App autorisiert nur Trigger, die eine gültige, mit dem geheimen Schlüssel erstellte Signatur enthalten. |
|||

Weitere Informationen zum Schützen des Zugriffs mit einer SAS (Shared Access Signature) finden Sie in den folgenden Abschnitten in diesem Thema:

* [Erneutes Generieren von Zugriffsschlüsseln](#access-keys)
* [Erstellen von ablaufenden Rückruf-URLs](#expiring-urls)
* [Erstellen von URLs mit Primär- oder Sekundärschlüssel](#primary-secondary-key)

<a name="access-keys"></a>

#### <a name="regenerate-access-keys"></a>Erneutes Generieren von Zugriffsschlüsseln

Über die Azure-REST-API oder das Azure-Portal können Sie jederzeit einen neuen Sicherheitszugriffsschlüssel generieren. Alle zuvor generierten URLs, die den alten Schlüssel verwenden, werden ungültig und sind nicht mehr berechtigt, die Logik-App auszulösen. Die URLs, die Sie nach der erneuten Generierung abrufen, werden mit dem neuen Zugriffsschlüssel signiert.

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com) die Logik-App mit dem Schlüssel, den Sie erneut generieren möchten.

1. Wählen Sie im Menü der Logik-App unter **Einstellungen** die Option **Zugriffsschlüssel** aus.

1. Wählen Sie den Schlüssel aus, den Sie erneut generieren möchten, und schließen Sie den Vorgang ab.

<a name="expiring-urls"></a>

#### <a name="create-expiring-callback-urls"></a>Erstellen ablaufender Rückruf-URLs

Wenn Sie die Endpunkt-URL eines anforderungsbasierten Triggers für andere Parteien freigeben, können Sie Rückruf-URLs mit bestimmten Schlüsseln und Ablaufdaten generieren. So können Sie nahtlos rollierende Schlüssel bereitstellen oder den Zugriff für das Auslösen Ihrer Logik-App auf einen bestimmten Zeitraum einschränken. Über die [Logic Apps-REST-API](/rest/api/logic/workflowtriggers) können Sie ein Ablaufdatum für eine URL angeben. Beispiel:

```http
POST /subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/workflows/<workflow-name>/triggers/<trigger-name>/listCallbackUrl?api-version=2016-06-01
```

Verwenden Sie im Text die `NotAfter`-Eigenschaft mit einer JSON-Datumszeichenfolge. Diese Eigenschaft gibt eine Rückruf-URL zurück, die nur bis zum Datum und der Uhrzeit in `NotAfter` gültig ist.

<a name="primary-secondary-key"></a>

#### <a name="create-urls-with-primary-or-secondary-secret-key"></a>Erstellen von URLs mit einem primären oder sekundären geheimen Schlüssel

Beim Generieren oder Auflisten von Rückruf-URLs für anforderungsbasierte Trigger können Sie den Schlüssel zum Signieren der URL angeben. Um eine URL zu generieren, die von einem bestimmten Schlüssel signiert wird, verwenden Sie die [Logic Apps-REST-API](/rest/api/logic/workflowtriggers). Beispiel:

```http
POST /subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/workflows/<workflow-name>/triggers/<trigger-name>/listCallbackUrl?api-version=2016-06-01
```

Schließen Sie in den Textkörper die `KeyType`-Eigenschaft als `Primary` oder als `Secondary` ein. Diese Eigenschaft gibt eine URL zurück, die mit dem angegebenen Sicherheitsschlüssel signiert ist.

<a name="enable-oauth"></a>

### <a name="enable-azure-active-directory-oauth"></a>Aktivieren von Azure Active Directory OAuth

Wenn Ihre Logik-App mit einem [Anforderungstrigger](../connectors/connectors-native-reqres.md) beginnt, können Sie [Azure Active Directory Open Authentication](../active-directory/develop/index.yml) (Azure AD OAuth) aktivieren, indem Sie eine Autorisierungsrichtlinie für eingehende Aufrufe an den Anforderungstrigger definieren oder hinzufügen. Wenn Ihre Logik-App eine eingehende Anforderung empfängt, die ein Authentifizierungstoken enthält, vergleicht Azure Logic Apps die Ansprüche des Tokens mit den Ansprüchen in den einzelnen Autorisierungsrichtlinien. Wenn eine Übereinstimmung zwischen den Ansprüchen des Tokens und allen Ansprüchen in mindestens einer Richtlinie vorliegt, ist die Autorisierung für die eingehende Anforderung erfolgreich. Das Token kann mehr Ansprüche als von der Autorisierungsrichtlinie angegeben aufweisen.

Angenommen, Ihre Logik-App verfügt über eine Autorisierungsrichtlinie, die zwei Anspruchstypen erfordert, **Aussteller** und **Zielgruppe**. Dieses Beispiel eines [Zugriffstokens](../active-directory/develop/access-tokens.md) enthält beide Anspruchstypen:

```json
{
   "aud": "https://management.core.windows.net/",
   "iss": "https://sts.windows.net/<Azure-AD-issuer-ID>/",
   "iat": 1582056988,
   "nbf": 1582056988,
   "exp": 1582060888,
   "_claim_names": {
      "groups": "src1"
   },
   "_claim_sources": {
      "src1": {
         "endpoint": "https://graph.windows.net/7200000-86f1-41af-91ab-2d7cd011db47/users/00000-f433-403e-b3aa-7d8406464625d7/getMemberObjects"
    }
   },
   "acr": "1",
   "aio": "AVQAq/8OAAAA7k1O1C2fRfeG604U9e6EzYcy52wb65Cx2OkaHIqDOkuyyr0IBa/YuaImaydaf/twVaeW/etbzzlKFNI4Q=",
   "amr": [
      "rsa",
      "mfa"
   ],
   "appid": "c44b4083-3bb0-00001-b47d-97400853cbdf3c",
   "appidacr": "2",
   "deviceid": "bfk817a1-3d981-4dddf82-8ade-2bddd2f5f8172ab",
   "family_name": "Sophia Owen",
   "given_name": "Sophia Owen (Fabrikam)",
   "ipaddr": "167.220.2.46",
   "name": "sophiaowen",
   "oid": "3d5053d9-f433-00000e-b3aa-7d84041625d7",
   "onprem_sid": "S-1-5-21-2497521184-1604012920-1887927527-21913475",
   "puid": "1003000000098FE48CE",
   "scp": "user_impersonation",
   "sub": "KGlhIodTx3XCVIWjJarRfJbsLX9JcdYYWDPkufGVij7_7k",
   "tid": "72f988bf-86f1-41af-91ab-2d7cd011db47",
   "unique_name": "SophiaOwen@fabrikam.com",
   "upn": "SophiaOwen@fabrikam.com",
   "uti": "TPJ7nNNMMZkOSx6_uVczUAA",
   "ver": "1.0"
}
```

#### <a name="considerations-for-enabling-azure-oauth"></a>Überlegungen zum Aktivieren von Azure OAuth

Bevor Sie diese Authentifizierung aktivieren, berücksichtigen Sie die folgenden Überlegungen:

* Bei einem eingehenden Aufruf Ihrer Logik-App kann nur ein Autorisierungsschema verwendet werden, entweder Azure AD OAuth oder [Shared Access Signature (SAS)](#sas). Nur Autorisierungsschemata vom [Typ „Bearer“](../active-directory/develop/active-directory-v2-protocols.md#tokens) werden für OAuth-Token unterstützt, die nur für den Anforderungstrigger unterstützt werden.

* Ihre Logik-App ist auf eine maximale Anzahl von Autorisierungsrichtlinien beschränkt. Jede Autorisierungsrichtlinie verfügt auch über eine maximale Anzahl von [Ansprüchen](../active-directory/develop/developer-glossary.md#claim). Weitere Informationen finden Sie unter [Grenzwerte und Konfiguration für Azure Logic Apps](../logic-apps/logic-apps-limits-and-config.md#authentication-limits).

* Eine Autorisierungsrichtlinie muss mindestens den Anspruch **Aussteller** enthalten, dem ein Wert ab `https://sts.windows.net/` oder `https://login.microsoftonline.com/` (OAuth V2) als Azure AD-Aussteller-ID zugeordnet ist. Weitere Informationen zu Zugriffstoken finden Sie unter [Microsoft Identity Platform-Zugriffstoken](../active-directory/develop/access-tokens.md).

<a name="define-authorization-policy-portal"></a>

#### <a name="define-authorization-policy-in-azure-portal"></a>Definieren einer Autorisierungsrichtlinie im Azure-Portal

Um Azure AD OAuth für Ihre Logik-App im Azure-Portal zu aktivieren, führen Sie die folgenden Schritte aus, um Ihrer Logik-App mindestens eine Autorisierungsrichtlinie hinzuzufügen:

1. Suchen oder öffnen Sie Ihre Logik-App im [Azure-Portal](https://portal.microsoft.com) im Logik-App-Designer.

1. Wählen Sie im Menü der Logik-App unter **Einstellungen** die Option **Autorisierung** aus. Sobald der Bereich „Autorisierung“ geöffnet ist, wählen Sie **Richtlinie hinzufügen** aus.

   ![Auswählen von „Autorisierung > Richtlinie hinzufügen“](./media/logic-apps-securing-a-logic-app/add-azure-active-directory-authorization-policies.png)

1. Geben Sie Informationen zur Autorisierungsrichtlinie an, indem Sie die [Anspruchstypen](../active-directory/develop/developer-glossary.md#claim) und Werte angeben, die ihre Logik-App im Authentifizierungstoken erwartet, das von jedem eingehenden Aufruf des Anforderungstriggers präsentiert wird:

   ![Bereitstellen von Informationen für die Autorisierungsrichtlinie](./media/logic-apps-securing-a-logic-app/set-up-authorization-policy.png)

   | Eigenschaft | Erforderlich | BESCHREIBUNG |
   |----------|----------|-------------|
   | **Richtlinienname** | Ja | Der Name, den Sie für die Autorisierungsinstanz verwenden möchten |
   | **Ansprüche** | Ja | Die Anspruchstypen und -werte, die ihre Logik-App von eingehenden Aufrufen akzeptiert. Im Folgenden finden Sie die verfügbaren Anspruchstypen: <p><p>- **Aussteller** <br>- **Zielgruppe** <br>- **Betreff** <br>- **JWT-ID** (JSON Web Token-ID) <p><p>Die Liste **Ansprüche** muss mindestens den Anspruch **Aussteller** enthalten, dem ein Wert ab `https://sts.windows.net/` oder `https://login.microsoftonline.com/` als Azure AD-Aussteller-ID zugeordnet ist. Weitere Informationen zu diesen Anspruchstypen finden Sie unter [Ansprüche in Sicherheitstoken von Azure AD](../active-directory/azuread-dev/v1-authentication-scenarios.md#claims-in-azure-ad-security-tokens). Sie können auch Ihren eigenen Anspruchstyp und -wert angeben. |
   |||

1. Zum Hinzufügen eines weiteren Anspruchs wählen Sie eine der folgenden Optionen aus:

   * Um einen weiteren Anspruchstyp hinzuzufügen, wählen Sie **Standardanspruch hinzufügen** aus und geben den Anspruchswert an.

   * Um Ihren eigenen Anspruch hinzuzufügen, wählen Sie **Benutzerdefinierten Anspruch hinzufügen** aus und geben den benutzerdefinierten Anspruchswert an.

1. Zum Hinzufügen einer weiteren Autorisierungsrichtlinie wählen Sie **Richtlinie hinzufügen** aus. Wiederholen Sie die vorherigen Schritte, um die Richtlinie einzurichten.

1. Klicken Sie auf **Speichern**, wenn Sie fertig sind.

<a name="define-authorization-policy-template"></a>

#### <a name="define-authorization-policy-in-azure-resource-manager-template"></a>Definieren einer Autorisierungsrichtlinie in einer Azure Resource Manager-Vorlage

Um Azure AD OAuth in der ARM-Vorlage für die Bereitstellung Ihrer Logik-App zu aktivieren, fügen Sie im Abschnitt `properties` für die [Ressourcendefinition Ihrer Logik-App](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md#logic-app-resource-definition) ein `accessControl`-Objekt hinzu, falls keines vorhanden ist, das ein `triggers`-Objekt enthält. Fügen Sie im `triggers`-Objekt ein `openAuthenticationPolicies`-Objekt hinzu, in dem Sie eine oder mehrere Autorisierungsrichtlinien definieren, indem Sie die folgende Syntax befolgen:

```json
"resources": [
   {
      // Start logic app resource definition
      "properties": {
         "state": "<Enabled-or-Disabled>",
         "definition": {<workflow-definition>},
         "parameters": {<workflow-definition-parameter-values>},
         "accessControl": {
            "triggers": {
               "openAuthenticationPolicies": {
                  "policies": {
                     "<policy-name>": {
                        "type": "AAD",
                        "claims": [
                           {
                              "name": "<claim-name>",
                              "values": "<claim-value>"
                           }
                        ]
                     }
                  }
               }
            },
         },
      },
      "name": "[parameters('LogicAppName')]",
      "type": "Microsoft.Logic/workflows",
      "location": "[parameters('LogicAppLocation')]",
      "apiVersion": "2016-06-01",
      "dependsOn": [
      ]
   }
   // End logic app resource definition
],
```

Weitere Informationen zum `accessControl`-Abschnitt finden Sie unter [Einschränken eingehender IP-Adressbereiche in Azure Resource Manager-Vorlagen](#restrict-inbound-ip-template) und [Microsoft.Logic-Workflows-Vorlagenreferenz](/templates/microsoft.logic/2019-05-01/workflows).

<a name="restrict-inbound-ip"></a>

### <a name="restrict-inbound-ip-addresses"></a>Einschränken eingehender IP-Adressen

Zusätzlich zur Shared Access Signature (SAS) empfiehlt es sich, spezifisch die Clients einzuschränken, die Ihre Logik-App aufrufen können. Beispiel: Wenn Sie den Anforderungsendpunkt mit [Azure API Management](../api-management/api-management-key-concepts.md) verwalten, können Sie für die Logik-App festlegen, dass sie Anforderungen nur von der IP-Adresse der [von Ihnen erstellten API Management-Dienstinstanz](../api-management/get-started-create-service-instance.md) annimmt.

#### <a name="restrict-inbound-ip-ranges-in-azure-portal"></a>Einschränken eingehender IP-Adressbereiche im Azure-Portal

1. Öffnen Sie Ihre Logik-App über das [Azure-Portal](https://portal.azure.com) im Logik-App-Designer.

1. Wählen Sie im Menü Ihrer Logik-App unter **Einstellungen** die Option **Workfloweinstellungen** aus.

1. Wählen Sie unter **Konfiguration der Zugriffssteuerung** > **Zulässige eingehende IP-Adressen** die Option **Spezifische IP-Bereiche** aus.

1. Geben Sie unter **IP-Bereiche für Trigger** die IP-Adressbereiche an, die der Trigger akzeptiert.

   Gültige IP-Adressbereiche verwenden die Formate *x.x.x.x/x* oder *x.x.x.x-x.x.x.x*.

Wenn Sie Ihre Logik-App nur als geschachtelte Logik-App auslösen möchten, wählen Sie in der Liste **Zulässige eingehende IP-Adressen** die Option **Nur andere Logik-Apps** aus. Diese Option schreibt ein leeres Array in Ihre Logik-App-Ressource. Auf diese Weise können nur Aufrufe über den Logic Apps-Dienst (übergeordnete Logik-Apps) die geschachtelte Logik-App auslösen.

> [!NOTE]
> Unabhängig von der IP-Adresse können Sie eine Logik-App weiterhin ausführen, die einen anforderungsbasierten Trigger hat, indem Sie die Anforderung [Logic Apps-REST-API:  Workflowtrigger – Ausführen](/rest/api/logic/workflowtriggers/run) oder die API Management verwenden. In diesem Szenario ist jedoch weiterhin eine [Authentifizierung](../active-directory/develop/authentication-vs-authorization.md) über die Azure-REST-API erforderlich. Alle Ereignisse werden im Azure-Überwachungsprotokoll angezeigt. Achten Sie darauf, die Richtlinien für die Zugriffssteuerung entsprechend festzulegen.

<a name="restrict-inbound-ip-template"></a>

#### <a name="restrict-inbound-ip-ranges-in-azure-resource-manager-template"></a>Einschränken eingehender IP-Adressbereich in der Azure Resource Manager-Vorlage

Wenn Sie die [Bereitstellung von Logik-Apps mithilfe einer Resource Manager-Vorlage automatisieren](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), können Sie die IP-Adressbereiche im Format *x.x.x.x/x* oder *x.x.x.x-x.x.x.x* angeben, indem Sie den `accessControl`-Abschnitt verwenden und die Abschnitte `triggers` und `actions` in die Ressourcendefinition Ihrer Logik-App einbeziehen. Beispiel:

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {},
   "variables": {},
   "resources": [
      {
         "name": "[parameters('LogicAppName')]",
         "type": "Microsoft.Logic/workflows",
         "location": "[parameters('LogicAppLocation')]",
         "tags": {
            "displayName": "LogicApp"
         },
         "apiVersion": "2016-06-01",
         "properties": {
            "definition": {
               <workflow-definition>
            },
            "parameters": {
            },
            "accessControl": {
               "triggers": {
                  "allowedCallerIpAddresses": [
                     {
                        "addressRange": "192.168.12.0/23"
                     }
                  ]
               },
               "actions": {
                  "allowedCallerIpAddresses": []
               }
            },
            "endpointsConfiguration": {}
         }
      }
   ],
   "outputs": {}
}
```

<a name="add-authentication"></a>

### <a name="add-azure-active-directory-open-authentication-or-other-security"></a>Hinzufügen von Azure Active Directory Open Authentication oder anderen Sicherheitsfeatures

Um Ihrer Logik-App weitere [Authentifizierungsprotokolle](../active-directory/develop/authentication-vs-authorization.md) hinzuzufügen, können Sie den Dienst [Azure API Management](../api-management/api-management-key-concepts.md) verwenden. Dieser Dienst hilft Ihnen, Ihre Logik-App als API verfügbar zu machen. Er bietet außerdem umfassende Überwachung, Sicherheit, Richtlinien und Dokumentation für alle Endpunkte. Mit API Management können Sie einen öffentlichen oder privaten Endpunkt für die Logik-App verfügbar machen. Um den Zugriff auf diesen Endpunkt zu autorisieren, können Sie [Azure Active Directory Open Authentication](#azure-active-directory-oauth-authentication) (Azure AD OAuth), [Clientzertifikate](#client-certificate-authentication) oder andere Sicherheitsstandards verwenden. Wenn API Management eine Anforderung empfängt, sendet der Dienst die Anforderung an Ihre Logik-App und setzt dazu alle erforderlichen Transformationen oder Einschränkungen um. Damit nur API Management Ihre Logik-App auslösen kann, können Sie die Einstellungen für den eingehenden IP-Bereich Ihrer Logik-App anpassen.

<a name="secure-operations"></a>

## <a name="access-to-logic-app-operations"></a>Zugriff auf Logik-App-Vorgänge

Sie können nur bestimmten Benutzern oder Gruppen erlauben, bestimmte Aufgaben auszuführen, wie z. B. das Verwalten, Bearbeiten und Anzeigen von Logik-Apps. Um die jeweiligen Berechtigungen zu steuern, verwenden Sie die [rollenbasierte Zugriffssteuerung in Azure (Azure Role-Based Access Control, RBAC)](../role-based-access-control/role-assignments-portal.md), damit Sie den Mitgliedern in Ihrem Azure-Abonnement benutzerdefinierte oder integrierte Rollen zuweisen können:

* [Logik-App-Mitwirkender:](../role-based-access-control/built-in-roles.md#logic-app-contributor) Ermöglicht Ihnen die Verwaltung von Logik-Apps, aber nicht die Änderung des App-Zugriffs.

* [Logik-App-Operator:](../role-based-access-control/built-in-roles.md#logic-app-operator) Ermöglicht Ihnen das Lesen, Aktivieren und Deaktivieren von Logik-Apps, die Sie aber nicht bearbeiten oder aktualisieren können.

Um zu verhindern, dass andere Personen Ihre Logik-App ändern oder löschen, können Sie [Azure-Ressourcensperren](../azure-resource-manager/management/lock-resources.md) verwenden. Diese Funktion verhindert, dass andere Personen Produktionsressourcen ändern oder löschen.

<a name="secure-run-history"></a>

## <a name="access-to-run-history-data"></a>Zugriff auf Ausführungsverlaufsdaten

Während der Ausführung einer Logik-App werden alle Daten [bei der Übertragung](../security/fundamentals/encryption-overview.md#encryption-of-data-in-transit) mithilfe von Transport Layer Security (TLS) sowie [im Ruhezustand](../security/fundamentals/encryption-atrest.md) verschlüsselt. Wenn Ihre Logik-App die Ausführung beendet hat, können Sie den Verlauf für diese Ausführung anzeigen, einschließlich der ausgeführten Schritte sowie Status, Dauer, Eingaben und Ausgaben für die einzelnen Aktionen. Diese umfangreichen Informationen geben einen Einblick in die Funktionsweise Ihrer Logik-App und zeigen, wo Sie bei der Problembehandlung ansetzen können.

Wenn Sie den Ausführungsverlauf Ihrer Logik-App anzeigen, authentifiziert Azure Logic Apps Ihren Zugriff und stellt dann Links zu den Ein- und Ausgaben für die Anforderungen und Antworten für jede Ausführung bereit. Bei Aktionen, die Kennwörter, Geheimnisse, Schlüssel oder andere sensible Informationen verarbeiten, sollten Sie jedoch verhindern, dass andere Personen diese Daten einsehen und darauf zugreifen. Wenn Ihre Logik-App beispielsweise ein Geheimnis aus [Azure Key Vault](../key-vault/general/overview.md) erhält, das bei der Authentifizierung einer HTTP-Aktion verwendet werden soll, sollten Sie dieses Geheimnis ausblenden.

Um den Zugriff auf die Ein- und Ausgaben im Ausführungsverlauf Ihrer Logik-App zu steuern, stehen Ihnen folgende Optionen zur Verfügung:

* [Beschränken des Zugriffs nach IP-Adressbereich](#restrict-ip).

  Mit dieser Option können Sie den Zugriff auf den Ausführungsverlauf basierend auf den Anforderungen aus einem bestimmten IP-Adressbereich schützen.

* [Schützen von Daten im Ausführungsverlauf mittels Obfuskation](#obfuscate).

  In vielen Triggern und Aktionen können Sie Eingaben, Ausgaben oder beides im Ausführungsverlauf einer Logik-App schützen.

<a name="restrict-ip"></a>

### <a name="restrict-access-by-ip-address-range"></a>Beschränken des Zugriffs nach IP-Adressbereich

Sie können den Zugriff auf die Ein- und Ausgaben im Ausführungsverlauf Ihrer Logik-App so einschränken, dass diese Daten nur für Anforderungen aus bestimmten IP-Adressbereichen einsehbar sind. Um beispielsweise den Zugriff auf Ein- und Ausgaben zu verhindern, geben Sie einen IP-Adressbereich wie z. B. `0.0.0.0-0.0.0.0` an. Nur Personen mit Administratorberechtigungen können diese Einschränkung entfernen und erhalten damit die Möglichkeit für einen Just-In-Time-Zugriff auf die Daten Ihrer Logik-App. Sie können die einzuschränkenden IP-Bereiche entweder über das Azure-Portal oder in einer Azure Resource Manager-Vorlage angeben, die Sie für die Bereitstellung von Logik-Apps verwenden.

#### <a name="restrict-ip-ranges-in-azure-portal"></a>Einschränken von IP-Adressbereichen im Azure-Portal

1. Öffnen Sie die Logik-App im Azure-Portal im Logik-App-Designer.

1. Wählen Sie im Menü Ihrer Logik-App unter **Einstellungen** die Option **Workfloweinstellungen** aus.

1. Wählen Sie unter **Konfiguration der Zugriffssteuerung** > **Zulässige eingehende IP-Adressen** die Option **Spezifische IP-Bereiche** aus.

1. Geben Sie unter **IP-Bereiche für Inhalte** die IP-Adressbereiche an, die auf Inhalte von Eingaben und Ausgaben zugreifen können.

   Gültige IP-Adressbereiche verwenden die Formate *x.x.x.x/x* oder *x.x.x.x-x.x.x.x*.

#### <a name="restrict-ip-ranges-in-azure-resource-manager-template"></a>Einschränken von IP-Adressbereichen in einer Azure Resource Manager-Vorlage

Wenn Sie die [Bereitstellung von Logik-Apps mithilfe einer Resource Manager-Vorlage automatisieren](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), können Sie die IP-Adressbereiche angeben, indem Sie den Abschnitt `accessControl` mit dem Abschnitt `contents` in der Ressourcendefinition Ihrer Logik-App verwenden. Beispiel:

``` json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {},
   "variables": {},
   "resources": [
      {
         "name": "[parameters('LogicAppName')]",
         "type": "Microsoft.Logic/workflows",
         "location": "[parameters('LogicAppLocation')]",
         "tags": {
            "displayName": "LogicApp"
         },
         "apiVersion": "2016-06-01",
         "properties": {
            "definition": {<workflow-definition>},
            "parameters": {},
            "accessControl": {
               "contents": {
                  "allowedCallerIpAddresses": [
                     {
                        "addressRange": "192.168.12.0/23"
                     },
                     {
                        "addressRange": "2001:0db8::/64"
                     }
                  ]
               }
            }
         }
      }
   ],
   "outputs": {}
}
```

<a name="obfuscate"></a>

### <a name="secure-data-in-run-history-by-using-obfuscation"></a>Schützen von Daten im Ausführungsverlauf mittels Obfuskation

Bei vielen Triggern und Aktionen stehen Einstellungen zur Verfügung, um Eingaben, Ausgaben oder beides im Ausführungsverlauf einer Logik-App zu schützen. Bevor Sie diese Einstellungen verwenden, um entsprechende Daten zu schützen, [berücksichtigen Sie diese Aspekte](#obfuscation-considerations).

#### <a name="secure-inputs-and-outputs-in-the-designer"></a>Schützen von Ein- und Ausgaben im Designer

1. Öffnen Sie Ihre Logik-App über das [Azure-Portal](https://portal.azure.com) im Logik-App-Designer.

   ![Öffnen einer Logik-App im Logik-App-Designer](./media/logic-apps-securing-a-logic-app/open-sample-logic-app-in-designer.png)

1. Wählen Sie für den Trigger oder die Aktion, bei dem/der Sie vertrauliche Daten schützen möchten, die Schaltfläche mit den Auslassungspunkten ( **...** ) und dann **Einstellungen** aus.

   ![Öffnen von Trigger- oder Aktionseinstellungen](./media/logic-apps-securing-a-logic-app/open-action-trigger-settings.png)

1. Aktivieren Sie entweder **Sichere Eingaben**, **Sichere Ausgaben** oder beides. Klicken Sie auf **Fertig**, wenn Sie fertig sind.

   ![Aktivieren von „Sichere Eingaben“ oder „Sichere Ausgaben“](./media/logic-apps-securing-a-logic-app/turn-on-secure-inputs-outputs.png)

   Die Aktion oder der Trigger zeigt jetzt ein Schlosssymbol in der Titelleiste an.

   ![Titelleiste einer Aktion oder eines Triggers mit Sperrsymbol](./media/logic-apps-securing-a-logic-app/lock-icon-action-trigger-title-bar.png)

   Token, die sichere Ausgaben aus früheren Aktionen darstellen, zeigen ebenfalls Schlosssymbole an. Wenn Sie beispielsweise eine solche Ausgabe aus der Liste mit dynamischen Inhalten für eine Aktion auswählen, zeigt das entsprechende Token ein Schlosssymbol an.

   ![Auswählen des Tokens für abgesicherte Ausgabe](./media/logic-apps-securing-a-logic-app/select-secured-token.png)

1. Nachdem die Logik-App ausgeführt wurde, können Sie den Verlauf für diese Ausführung anzeigen.

   1. Wählen Sie im Bereich **Übersicht** der Logik-App die Ausführung aus, die Sie anzeigen möchten.

   1. Erweitern Sie im Bereich **Logik-App-Ausführung** die Aktionen, die Sie überprüfen möchten.

      Wenn Sie sowohl die Ein- als auch die Ausgaben zum Verbergen ausgewählt haben, werden diese Werte jetzt ausgeblendet.

      ![Ausgeblendete Ein- und Ausgaben im Ausführungsverlauf](./media/logic-apps-securing-a-logic-app/hidden-data-run-history.png)

<a name="secure-data-code-view"></a>

#### <a name="secure-inputs-and-outputs-in-code-view"></a>Schützen von Ein- und Ausgaben in der Codeansicht

Fügen Sie in der zugrunde liegenden Trigger- oder Aktionsdefinition das Array `runtimeConfiguration.secureData.properties` mit einem der folgenden Werte (oder mit beiden Werten) hinzu, oder aktualisieren Sie es entsprechend:

* `"inputs"`: Schützt Eingaben im Ausführungsverlauf.
* `"outputs"`: Schützt Ausgaben im Ausführungsverlauf.

Wenn Sie diese Einstellungen verwenden, um entsprechende Daten zu schützen, müssen [einige Aspekte](#obfuscation-considerations) berücksichtigt werden.

```json
"<trigger-or-action-name>": {
   "type": "<trigger-or-action-type>",
   "inputs": {
      <trigger-or-action-inputs>
   },
   "runtimeConfiguration": {
      "secureData": {
         "properties": [
            "inputs",
            "outputs"
         ]
      }
   },
   <other-attributes>
}
```

<a name="obfuscation-considerations"></a>

#### <a name="considerations-when-securing-inputs-and-outputs"></a>Überlegungen im Zusammenhang mit dem Schützen von Ein- und Ausgaben

* Wenn Sie die Ein- oder Ausgaben für einen Trigger oder eine Aktion verbergen, sendet Logic Apps die geschützten Daten nicht an Azure Log Analytics. Außerdem können Sie diesem Auslöser oder dieser Aktion keine [nachverfolgten Eigenschaften](../logic-apps/monitor-logic-apps-log-analytics.md#extend-data) zur Überwachung hinzufügen.

* Die [Logic Apps-API zur Verarbeitung des Workflowverlaufs](/rest/api/logic/) gibt keine sicheren Ausgaben zurück.

* Um Ausgaben einer Aktion zu schützen, die Eingaben verbirgt oder explizit Ausgaben verbirgt, aktivieren Sie in dieser Aktion **Sichere Ausgaben** manuell.

* Stellen Sie sicher, dass Sie **Sichere Eingaben** oder **Sichere Ausgaben** in nachfolgenden Aktionen aktivieren, bei denen Sie erwarten, dass die Daten im Ausführungsverlauf verborgen werden.

  **Einstellung „Sichere Ausgaben“**

  Wenn Sie **Sichere Ausgaben** in einem Trigger oder einer Aktion manuell aktivieren, blendet Logic Apps diese Ausgaben im Ausführungsverlauf aus. Wenn eine nachfolgende Aktion diese sicheren Ausgaben explizit als Eingaben verwendet, blendet Logic Apps die Eingaben dieser Aktion im Ausführungsverlauf aus, *aktiviert aber nicht* die Einstellung **Sichere Eingaben** der Aktion.

  ![Sichere Ausgaben als Eingaben und nachgeschaltete Auswirkungen auf die meisten Aktionen](./media/logic-apps-securing-a-logic-app/secure-outputs-as-inputs-flow.png)

  Die Aktionen „Erstellen“, „JSON analysieren“ und „Antwort“ weisen nur die Einstellung **Sichere Eingaben** auf. Wenn diese aktiviert wird, blendet diese Einstellung auch die Ausgaben dieser Aktionen aus. Wenn diese Aktionen explizit die sicheren Upstreamausgaben als Eingaben verwenden, blendet Logic Apps die Ein- und Ausgaben dieser Aktionen aus, *aktiviert aber nicht* die Einstellung **Sichere Eingaben** dieser Aktionen. Wenn eine nachfolgende Aktion explizit die ausgeblendeten Ausgaben der Aktionen „Erstellen“, „JSON analysieren“ oder „Antwort“ als Eingaben verwendet, *blendet Logic Apps die Ein- oder Ausgaben dieser nachfolgenden Aktion nicht aus*.

  ![Sichere Ausgaben als Eingaben mit nachgeschalteten Auswirkungen auf bestimmte Aktionen](./media/logic-apps-securing-a-logic-app/secure-outputs-as-inputs-flow-special.png)

  **Einstellung „Sichere Eingaben“**

  Wenn Sie **Sichere Eingaben** in einem Trigger oder einer Aktion manuell aktivieren, blendet Logic Apps diese Eingaben im Ausführungsverlauf aus. Wenn eine nachfolgende Aktion explizit die sichtbaren Ausgaben dieses Triggers oder dieser Aktion als Eingaben verwendet, blendet Logic Apps die Eingaben dieser nachfolgenden Aktion im Ausführungsverlauf aus, *aktiviert aber nicht* die Option **Sichere Eingaben** in dieser Aktion und blendet die Ausgaben dieser Aktion nicht aus.

  ![Sichere Eingaben und nachgeschaltete Auswirkungen auf die meisten Aktionen](./media/logic-apps-securing-a-logic-app/secure-inputs-impact-on-downstream.png)

  Wenn die Aktionen „Erstellen“, „JSON analysieren“ und „Antwort“ explizit die sichtbaren Ausgaben des Triggers oder der Aktion mit den sicheren Eingaben verwenden, blendet Logic Apps die Ein- und Ausgaben dieser Aktionen, *aktiviert aber nicht* die Einstellung **Sichere Eingaben** der jeweiligen Aktion. Wenn eine nachfolgende Aktion explizit die ausgeblendeten Ausgaben der Aktionen „Erstellen“, „JSON analysieren“ oder „Antwort“ als Eingaben verwendet, *blendet Logic Apps die Ein- oder Ausgaben dieser nachfolgenden Aktion nicht aus*.

  ![Sichere Eingaben und nachgeschaltete Auswirkungen auf bestimmte Aktionen](./media/logic-apps-securing-a-logic-app/secure-inputs-flow-special.png)

<a name="secure-action-parameters"></a>

## <a name="access-to-parameter-inputs"></a>Zugriff auf Parametereingaben

Wenn Sie Bereitstellungen in verschiedenen Umgebungen durchführen, sollten Sie die Werte in Ihrer Workflowdefinition parametrisieren, die je nach Umgebung variieren. Auf diese Weise können Sie hartcodierte Daten vermeiden, indem Sie eine [Azure Resource Manager-Vorlage](../azure-resource-manager/templates/overview.md) verwenden, um Ihre Logikanwendung bereitzustellen, sensible Daten durch die Definition abgesicherter Parameter zu schützen und diese Daten als separate Eingaben durch die [Parameter der Vorlage](../azure-resource-manager/templates/template-parameters.md) mithilfe einer [Parameterdatei](../azure-resource-manager/templates/parameter-files.md) zu übergeben.

Wenn Sie z. B. HTTP-Aktionen mit [Azure Active Directory Open Authentication](#azure-active-directory-oauth-authentication) (Azure AD OAuth) authentifizieren, können Sie die Parameter definieren und verbergen, die die Client-ID und das Clientgeheimnis akzeptieren, die für die Authentifizierung verwendet werden. Um diese Parameter für Ihre Logik-App zu definieren, verwenden Sie den Abschnitt `parameters` innerhalb der Workflowdefinition Ihrer Logik-App und eine Resource Manager-Vorlage zur Bereitstellung. Um die Parameterwerte zu schützen, die beim Bearbeiten der Logik-App oder beim Anzeigen des Ausführungsverlaufs nicht angezeigt werden sollen, können Sie Parameter des Typs `securestring` oder `secureobject` definieren und bei Bedarf codieren. Parameter dieses Typs werden nicht mit der Ressourcendefinition zurückgegeben und sind beim Anzeigen der Ressource nach der Bereitstellung nicht zugänglich. Um zur Laufzeit auf diese Parameterwerte zuzugreifen, verwenden Sie den Ausdruck `@parameters('<parameter-name>')` in Ihrer Workflowdefinition. Dieser Ausdruck wird nur zur Laufzeit ausgewertet und durch die [Workflowdefinitionssprache](../logic-apps/logic-apps-workflow-definition-language.md) beschrieben.

> [!NOTE]
> Wenn Sie einen Parameter im Anforderungsheader oder -text verwenden, kann dieser Parameter sichtbar sein, wenn Sie den Ausführungsverlauf Ihrer Logik-App und die ausgehende HTTP-Anforderung anzeigen. Achten Sie darauf, Ihre Richtlinien für den Zugriff auf Inhalte entsprechend festzulegen.
> Sie können auch [Obfuskation](#obfuscate) verwenden, um Ein- und Ausgaben im Ausführungsverlauf auszublenden. Autorisierungsheader sind nie über Eingaben oder Ausgaben sichtbar. Wenn hier ein Geheimnis verwendet wird, ist dieses daher nicht abrufbar.

Weitere Informationen finden in diesem Artikel in diesen Abschnitten:

* [Schützen von Parameter in Workflowdefinitionen](#secure-parameters-workflow)
* [Sichern von Daten im Ausführungsverlauf mittels Obfuskation](#obfuscate)

Wenn Sie die [Bereitstellung für Logik-Apps mithilfe von Resource Manager-Vorlagen automatisieren](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), können Sie geschützte [Vorlagenparameter](../azure-resource-manager/templates/template-parameters.md) definieren, die beim Bereitstellen ausgewertet werden, indem Sie die Typen `securestring` und `secureobject` verwenden. Um Vorlagenparameter zu definieren, verwenden Sie den Abschnitt `parameters` auf der obersten Ebene Ihrer Vorlage, der vom Abschnitt `parameters` Ihrer Workflowdefinition getrennt ist und anders lautet. Um die Werte für Vorlagenparameter bereitzustellen, verwenden Sie eine separate [Parameterdatei](../azure-resource-manager/templates/parameter-files.md).

Wenn Sie beispielsweise Geheimnisse verwenden, können Sie sichere Vorlagenparameter definieren und verwenden, die diese Geheimnisse bei der Bereitstellung aus [Azure Key Vault](../key-vault/general/overview.md) abrufen. Anschließend können Sie auf den Schlüsseltresor und das Geheimnis in Ihrer Parameterdatei verweisen. Weitere Informationen finden Sie in den folgenden Themen:

* [Übergeben vertraulicher Werte bei der Bereitstellung mithilfe von Azure Key Vault](../azure-resource-manager/templates/key-vault-parameter.md)
* [Sichere Parameter in Azure Resource Manager-Vorlagen](#secure-parameters-deployment-template) weiter unten in diesem Thema

<a name="secure-parameters-workflow"></a>

### <a name="secure-parameters-in-workflow-definitions"></a>Sichere Parameter in Workflowdefinitionen

Zum Schützen sensibler Informationen in der Workflowdefinition der Logik-App verwenden Sie sichere Parameter, damit diese Informationen nach dem Speichern der Logik-App nicht sichtbar sind. Angenommen, Sie verwenden eine HTTP-Aktion, die eine Standardauthentifizierung mit Benutzernamen und Kennwort erfordert. In der Workflowdefinition definiert der Abschnitt `parameters` die Parameter `basicAuthPasswordParam` und `basicAuthUsernameParam` über den Typ `securestring`. Die Aktionsdefinition verweist dann auf diese Parameter im Abschnitt `authentication`.

```json
"definition": {
   "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
   "actions": {
      "HTTP": {
         "type": "Http",
         "inputs": {
            "method": "GET",
            "uri": "https://www.microsoft.com",
            "authentication": {
               "type": "Basic",
               "username": "@parameters('basicAuthUsernameParam')",
               "password": "@parameters('basicAuthPasswordParam')"
            }
         },
         "runAfter": {}
      }
   },
   "parameters": {
      "basicAuthPasswordParam": {
         "type": "securestring"
      },
      "basicAuthUsernameParam": {
         "type": "securestring"
      }
   },
   "triggers": {
      "manual": {
         "type": "Request",
         "kind": "Http",
         "inputs": {
            "schema": {}
         }
      }
   },
   "contentVersion": "1.0.0.0",
   "outputs": {}
}
```

<a name="secure-parameters-deployment-template"></a>

### <a name="secure-parameters-in-azure-resource-manager-templates"></a>Sichere Parameter in Azure Resource Manager-Vorlagen

Eine [Resource Manager-Vorlage](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md) für eine Logik-App enthält mehrere `parameters`-Abschnitte. Um Kennwörter, Schlüssel, Geheimnisse und andere sensible Informationen zu schützen, definieren Sie sichere Parameter auf Vorlagen- und Workflowdefinitionsebene mit dem Typ `securestring` oder `secureobject`. Sie können diese Werte dann in [Azure Key Vault](../key-vault/general/overview.md) speichern und die [Parameterdatei](../azure-resource-manager/templates/parameter-files.md) verwenden, um auf den Schlüsselspeicher und das Geheimnis zu verweisen. Ihre Vorlage ruft diese Informationen dann bei der Bereitstellung ab. Weiter Informationen finden Sie unter [Übergeben vertraulicher Werte bei der Bereitstellung mithilfe von Azure Key Vault](../azure-resource-manager/templates/key-vault-parameter.md).

Hier finden Sie weitere Informationen zu diesen `parameters`-Abschnitten:

* Auf der obersten Ebene der Vorlage definiert ein `parameters`-Abschnitt die Parameter für die Werte, die von der Vorlage bei der *Bereitstellung* verwendet werden. Diese Werte können beispielsweise Verbindungszeichenfolgen für eine bestimmte Bereitstellungsumgebung beinhalten. Sie können diese Werte dann in einer separaten [Parameterdatei](../azure-resource-manager/templates/parameter-files.md) speichern, was das Ändern dieser Werte erleichtert.

* Innerhalb der Ressourcendefinition Ihrer Logik-App, aber außerhalb Ihrer Workflowdefinition, gibt der Abschnitt `parameters` die Werte für die Parameter Ihrer Workflowdefinition an. In diesem Abschnitt können Sie diese Werte anhand von Vorlagenausdrücken zuweisen, die auf die Parameter Ihrer Vorlage verweisen. Diese Ausdrücke werden bei der Bereitstellung ausgewertet.

* Innerhalb Ihrer Workflowdefinition definiert der Abschnitt `parameters` die Parameter, die von Ihrer Logik-App zur Runtime verwendet werden. Sie können auf diese Parameter dann innerhalb des Workflows Ihrer Logik-App verweisen, indem Sie Workflowdefinitionsausdrücke verwenden, die zur Laufzeit ausgewertet werden.

Diese Beispielvorlage weist mehrere sichere Parameterdefinitionen auf, die den Typ `securestring` verwenden:

| Parametername | BESCHREIBUNG |
|----------------|-------------|
| `TemplatePasswordParam` | Ein Vorlagenparameter, der ein Kennwort akzeptiert, das anschließend an den `basicAuthPasswordParam`-Parameter der Workflowdefinition übergeben wird |
| `TemplateUsernameParam` | Ein Vorlagenparameter, der einen Benutzernamen akzeptiert, der dann an den `basicAuthUserNameParam`-Parameter der Workflowdefinition übergeben wird |
| `basicAuthPasswordParam` | Ein Workflowdefinitionsparameter, der das Kennwort für die Standardauthentifizierung in einer HTTP-Aktion akzeptiert |
| `basicAuthUserNameParam` | Ein Workflowdefinitionsparameter, der den Benutzernamen für die Standardauthentifizierung in einer HTTP-Aktion akzeptiert |
|||

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {
      "LogicAppName": {
         "type": "string",
         "minLength": 1,
         "maxLength": 80,
         "metadata": {
            "description": "Name of the Logic App."
         }
      },
      "TemplatePasswordParam": {
         "type": "securestring"
      },
      "TemplateUsernameParam": {
         "type": "securestring"
      },
      "LogicAppLocation": {
         "type": "string",
         "defaultValue": "[resourceGroup().location]",
         "allowedValues": [
            "[resourceGroup().location]",
            "eastasia",
            "southeastasia",
            "centralus",
            "eastus",
            "eastus2",
            "westus",
            "northcentralus",
            "southcentralus",
            "northeurope",
            "westeurope",
            "japanwest",
            "japaneast",
            "brazilsouth",
            "australiaeast",
            "australiasoutheast",
            "southindia",
            "centralindia",
            "westindia",
            "canadacentral",
            "canadaeast",
            "uksouth",
            "ukwest",
            "westcentralus",
            "westus2"
         ],
         "metadata": {
            "description": "Location of the Logic App."
         }
      }
   },
   "variables": {},
   "resources": [
      {
         "name": "[parameters('LogicAppName')]",
         "type": "Microsoft.Logic/workflows",
         "location": "[parameters('LogicAppLocation')]",
         "tags": {
            "displayName": "LogicApp"
         },
         "apiVersion": "2016-06-01",
         "properties": {
            "definition": {
               "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
               "actions": {
                  "HTTP": {
                     "type": "Http",
                     "inputs": {
                        "method": "GET",
                        "uri": "https://www.microsoft.com",
                        "authentication": {
                           "type": "Basic",
                           "username": "@parameters('basicAuthUsernameParam')",
                           "password": "@parameters('basicAuthPasswordParam')"
                        }
                     },
                  "runAfter": {}
                  }
               },
               "parameters": {
                  "basicAuthPasswordParam": {
                     "type": "securestring"
                  },
                  "basicAuthUsernameParam": {
                     "type": "securestring"
                  }
               },
               "triggers": {
                  "manual": {
                     "type": "Request",
                     "kind": "Http",
                     "inputs": {
                        "schema": {}
                     }
                  }
               },
               "contentVersion": "1.0.0.0",
               "outputs": {}
            },
            "parameters": {
               "basicAuthPasswordParam": {
                  "value": "[parameters('TemplatePasswordParam')]"
               },
               "basicAuthUsernameParam": {
                  "value": "[parameters('TemplateUsernameParam')]"
               }
            }
         }
      }
   ],
   "outputs": {}
}
```

<a name="secure-outbound-requests"></a>

## <a name="access-to-services-and-systems-called-from-logic-apps"></a>Zugriff auf Dienste und Systeme, die von Logik-Apps aus aufgerufen werden

Hier sind einige Möglichkeiten, wie Sie Endpunkte schützen können, die Anrufe oder Anforderungen von Ihrer Logik-App empfangen:

* Fügen Sie für ausgehende Anforderungen eine Authentifizierung hinzu.

  Wenn Sie mit einem HTTP-basierten Trigger oder einer Aktion arbeiten, die ausgehende Aufrufe auslöst, wie z. B. HTTP, HTTP + Swagger oder Webhook, können Sie der Anforderung, die von Ihrer Logik-App gesendet wird, eine Authentifizierung hinzufügen. Beispielsweise können Sie diese Authentifizierungstypen auswählen:

  * [Standardauthentifizierung](#basic-authentication)

  * [Clientzertifikatsauthentifizierung](#client-certificate-authentication)

  * [Active Directory OAuth-Authentifizierung](#azure-active-directory-oauth-authentication)

  * [Authentifizierung der verwalteten Identität](#managed-identity-authentication)

  Weitere Informationen finden Sie weiter unten in diesem Thema unter [Hinzufügen der Authentifizierung zu ausgehenden Aufrufen](#add-authentication-outbound).

* Schränken Sie den Zugriff von IP-Adressen von Logik-Apps ein.

  Alle Aufrufe von Logik-Apps an Endpunkte stammen von bestimmten IP-Adressen, die auf den Regionen Ihrer Logik-App basieren. Sie können Filter hinzufügen, die Anforderungen nur von diesen IP-Adressen akzeptieren. Weitere Informationen zu diesen IP-Adressen finden Sie unter [Grenzwert- und Konfigurationsinformationen für Azure Logic Apps](logic-apps-limits-and-config.md#configuration).

* Erhöhen der Sicherheit für Verbindungen mit lokalen Systemen.

  Azure Logic Apps bietet eine Integration mit diesen Diensten, um eine sicherere und zuverlässigere lokale Kommunikation bereitzustellen.

  * Lokales Datengateway

    Viele verwaltete Connectors in Azure Logic Apps nutzen sichere Verbindungen mit lokalen Systemen wie dem Dateisystem, SQL, SharePoint und DB2. Das Gateway sendet Daten von lokalen Quellen durch verschlüsselte Kanäle über Azure Service Bus. Der gesamte Datenverkehr stammt als sicherer ausgehender Datenverkehr vom Gateway-Agent. Erfahren Sie, [wie das lokale Datengateway funktioniert](logic-apps-gateway-install.md#gateway-cloud-service).

  * Verbindung über Azure API Management

    [Azure API Management](../api-management/api-management-key-concepts.md) bietet lokale Konnektivitätsoptionen wie die Integration von Site-to-Site-VPN und [ExpressRoute](../expressroute/expressroute-introduction.md) für geschützte Proxys und die Kommunikation mit lokalen Systemen. Wenn Sie über eine API verfügen, die Zugriff auf Ihr lokales System bietet, und Sie diese API durch das Erstellen einer [API Management-Dienstinstanz](../api-management/get-started-create-service-instance.md) verfügbar gemacht haben, können Sie diese API im Workflow ihrer Logik-App aufrufen, indem Sie den integrierten API Management-Trigger bzw. die integrierte API Management-Aktion im Logik-App-Designer auswählen.

    > [!NOTE]
    > Der Connector zeigt nur die API Management-Dienste an, für die Sie über Berechtigungen zum Anzeigen und Verbinden verfügen, aber keine verbrauchsbasierten API Management Dienste.

    1. Geben Sie im Logik-App-Designer im Suchfeld `api management` ein. Wählen Sie den Schritt aus, je nachdem, ob Sie einen Trigger oder eine Aktion hinzufügen:<p>

       * Wenn Sie einen Trigger hinzufügen, bei dem es sich immer um den ersten Schritt in Ihrem Workflow handelt, wählen Sie **Azure API Management-Trigger auswählen** aus.

       * Wenn Sie eine Aktion hinzufügen, wählen Sie **Azure API Management-Aktion auswählen** aus.

       Im folgenden Beispiel wird ein Trigger hinzugefügt:

       ![Hinzufügen eines Azure API Management-Triggers](./media/logic-apps-securing-a-logic-app/select-api-management.png)

    1. Wählen Sie Ihre zuvor erstellte API Management-Dienstinstanz aus.

       ![Auswählen der API Management-Dienstinstanz](./media/logic-apps-securing-a-logic-app/select-api-management-service-instance.png)

    1. Wählen Sie den zu verwendenden API-Aufruf aus.

       ![Auswählen der vorhandenen API](./media/logic-apps-securing-a-logic-app/select-api.png)

<a name="add-authentication-outbound"></a>

## <a name="add-authentication-to-outbound-calls"></a>Hinzufügen der Authentifizierung zu ausgehenden Aufrufen

HTTP- und HTTP-Endpunkte unterstützen verschiedene Arten der Authentifizierung. Bei einigen Triggern und Aktionen, die Sie zum Senden ausgehender Aufrufe oder Anforderungen an diese Endpunkte verwenden, können Sie einen Authentifizierungstyp angeben. Im Logik-App-Designer besitzen Trigger und Aktionen, die die Auswahl eines Authentifizierungstyps unterstützen, eine Eigenschaft **Authentifizierung**. Diese Eigenschaft wird jedoch möglicherweise nicht immer standardmäßig angezeigt. In diesen Fällen öffnen Sie für den Trigger oder die Aktion die Liste **Neuen Parameter hinzufügen**, und wählen Sie **Authentifizierung** aus.

> [!IMPORTANT]
> Um vertrauliche Informationen, die Ihre Logik-App verarbeitet, zu schützen, verwenden Sie abgesicherte Parameter, und verschlüsseln Sie Daten nach Bedarf.
> Weitere Informationen zum Verwenden und Absichern von Parametern finden Sie unter [Zugriff auf Parametereingaben](#secure-action-parameters).

In dieser Tabelle werden die Authentifizierungstypen aufgeführt, die für die Trigger und Aktionen verfügbar sind, bei denen Sie einen Authentifizierungstyp auswählen können:

| Authentifizierungsart | Verfügbarkeit |
|---------------------|--------------|
| [Grundlegend](#basic-authentication) | Azure API Management, Azure App Services, HTTP, HTTP + Swagger, HTTP Webhook |
| [Clientzertifikat](#client-certificate-authentication) | Azure API Management, Azure App Services, HTTP, HTTP + Swagger, HTTP Webhook |
| [Active Directory OAuth](#azure-active-directory-oauth-authentication) | Azure API Management, Azure App Services, Azure Functions, HTTP, HTTP + Swagger, HTTP Webhook |
| [Raw](#raw-authentication) | Azure API Management, Azure App Services, Azure Functions, HTTP, HTTP + Swagger, HTTP Webhook |
| [Verwaltete Identität](#managed-identity-authentication) | Azure API Management, Azure App Services, Azure Functions, HTTP, HTTP + Swagger, HTTP Webhook |
|||

<a name="basic-authentication"></a>

### <a name="basic-authentication"></a>Standardauthentifizierung

Wenn die Option [Standard](../active-directory-b2c/secure-rest-api.md) verfügbar ist, geben Sie die folgenden Eigenschaftswerte an:

| Eigenschaft (Designer) | Eigenschaft (JSON) | Erforderlich | Wert | BESCHREIBUNG |
|---------------------|-----------------|----------|-------|-------------|
| **Authentifizierung** | `type` | Ja | Basic | Der zu verwendende Authentifizierungstyp |
| **Benutzername** | `username` | Ja | <*user-name*>| Der Benutzername, der verwendet wird, um den Zugriff auf den Ziel-Dienstendpunkt zu authentifizieren |
| **Kennwort** | `password` | Ja | <*password*> | Das Kennwort, das verwendet wird, um den Zugriff auf den Ziel-Dienstendpunkt zu authentifizieren |
||||||

Wenn Sie [abgesicherte Parameter](#secure-action-parameters) verwenden, um vertrauliche Informationen zu verarbeiten und zu schützen, z. B. in einer [Azure Resource Manager-Vorlage zur Automatisierung der Bereitstellung](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), können Sie zur Runtime mit Ausdrücken auf diese Parameterwerte zugreifen. In dieser Beispieldefinition einer HTTP-Aktion werden der `type` der Authentifizierung als `Basic` angegeben und die Funktion [parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) verwendet, um die Parameterwerte abzurufen:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "@parameters('endpointUrlParam')",
      "authentication": {
         "type": "Basic",
         "username": "@parameters('userNameParam')",
         "password": "@parameters('passwordParam')"
      }
  },
  "runAfter": {}
}
```

<a name="client-certificate-authentication"></a>

### <a name="client-certificate-authentication"></a>Authentifizierung mit Clientzertifikat

Wenn die Option [Clientzertifikat](../active-directory/authentication/active-directory-certificate-based-authentication-get-started.md) verfügbar ist, geben Sie die folgenden Eigenschaftswerte an:

| Eigenschaft (Designer) | Eigenschaft (JSON) | Erforderlich | Wert | BESCHREIBUNG |
|---------------------|-----------------|----------|-------|-------------|
| **Authentifizierung** | `type` | Ja | **Clientzertifikat** <br>oder <br>`ClientCertificate` | Der zu verwendende Authentifizierungstyp. Sie können Zertifikate mit [Azure API Management](../api-management/api-management-howto-mutual-certificates.md) verwalten. <p></p>**Hinweis**: Benutzerdefinierte Connectors unterstützen sowohl für eingehende als auch für ausgehende Aufrufe keine zertifikatbasierte Authentifizierung. |
| **Pfx** | `pfx` | Ja | <*encoded-pfx-file-content*> | Der base64-codierte Inhalt aus einer Personal Information Exchange-Datei (PFX) <p><p>Zum Konvertieren der PFX-Datei in ein base64-codiertes Format können Sie PowerShell verwenden, indem Sie die folgenden Schritte ausführen: <p>1. Speichern Sie den Zertifikatsinhalt in einer Variablen: <p>   `$pfx_cert = get-content 'c:\certificate.pfx' -Encoding Byte` <p>2. Konvertieren Sie den Zertifikatsinhalt mithilfe der `ToBase64String()`-Funktion, und speichern Sie den Inhalt in einer Textdatei: <p>   `[System.Convert]::ToBase64String($pfx_cert) | Out-File 'pfx-encoded-bytes.txt'` |
| **Kennwort** | `password`| Nein | <*password-for-pfx-file*> | Der Parameter für den Zugriff auf die PFX-Datei |
|||||

Wenn Sie [abgesicherte Parameter](#secure-action-parameters) verwenden, um vertrauliche Informationen zu verarbeiten und zu schützen, z. B. in einer [Azure Resource Manager-Vorlage zur Automatisierung der Bereitstellung](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), können Sie zur Runtime mit Ausdrücken auf diese Parameterwerte zugreifen. In dieser Beispieldefinition einer HTTP-Aktion werden der `type` der Authentifizierung als `ClientCertificate` angegeben und die Funktion [parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) verwendet, um die Parameterwerte abzurufen:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "@parameters('endpointUrlParam')",
      "authentication": {
         "type": "ClientCertificate",
         "pfx": "@parameters('pfxParam')",
         "password": "@parameters('passwordParam')"
      }
   },
   "runAfter": {}
}
```

Weitere Informationen zum Absichern von Diensten mithilfe der Clientzertifikatauthentifizierung finden Sie in den folgenden Themen:

* [Erhöhen der Sicherheit von APIs mithilfe von Clientzertifikatauthentifizierung in API Management](../api-management/api-management-howto-mutual-certificates-for-clients.md)
* [Erhöhen der Sicherheit von Back-End-Diensten mithilfe von Clientzertifikatauthentifizierung in API Management](../api-management/api-management-howto-mutual-certificates.md)
* [Erhöhen der Sicherheit Ihres RESTful-Diensts mit Clientzertifikaten](../active-directory-b2c/secure-rest-api.md)
* [Zertifikatanmeldeinformationen für die Anwendungsauthentifizierung](../active-directory/develop/active-directory-certificate-credentials.md)
* [Verwenden eines TLS-/SSL-Zertifikats in Ihrem Code in Azure App Service](../app-service/configure-ssl-certificate-in-code.md)

<a name="azure-active-directory-oauth-authentication"></a>

### <a name="azure-active-directory-open-authentication"></a>Azure Active Directory Open Authentication

Bei Anforderungstriggern können Sie nach der [Einrichtung von Azure AD-Autorisierungsrichtlinien](#enable-oauth) mit [Azure Active Directory Open Authentication](../active-directory/develop/index.yml) (Azure AD OAuth) eingehende Anrufe für Ihre Logik-App authentifizieren. Geben Sie für alle anderen Trigger und Aktionen, die Ihnen den **Active Directory OAuth**-Authentifizierungstyp zur Auswahl bereitstellen, die folgenden Eigenschaftswerte an:

| Eigenschaft (Designer) | Eigenschaft (JSON) | Erforderlich | Wert | BESCHREIBUNG |
|---------------------|-----------------|----------|-------|-------------|
| **Authentifizierung** | `type` | Ja | **Active Directory OAuth** <br>oder <br>`ActiveDirectoryOAuth` | Der zu verwendende Authentifizierungstyp. Azure Logic Apps befolgt derzeit das [Protokoll OAuth 2.0](../active-directory/develop/v2-overview.md). |
| **Autoritative Stelle** | `authority` | Nein | <*URL-for-authority-token-issuer*> | Die URL für die autoritative Stelle, die das Authentifizierungstoken bereitstellt. Standardmäßig ist dieser Wert auf `https://login.windows.net` festgelegt. |
| **Mandant** | `tenant` | Ja | <*tenant-ID*> | Die Mandanten-ID für den Azure AD-Mandanten |
| **Zielgruppe** | `audience` | Ja | <*resource-to-authorize*> | Die Ressource, die Sie für die Autorisierung verwenden möchten, z. B. `https://management.core.windows.net/` |
| **Client-ID** | `clientId` | Ja | <*client-ID*> | Die Client-ID für die App, die eine Autorisierung anfordert |
| **Typ der Anmeldeinformationen** | `credentialType` | Ja | Zertifikat <br>oder <br>`Secret` | Der Anmeldeinformationstyp, den der Client zum Anfordern der Autorisierung verwendet. Diese Eigenschaft und dieser Wert tauchen nicht in der zugrunde liegenden Definition Ihrer Logik-App auf, sondern bestimmen die Eigenschaften, die für den ausgewählten Anmeldeinformationstyp angezeigt werden. |
| **Geheimnis** | `secret` | Ja, aber nur für den Anmeldeinformationstyp „Geheimnis“ | <*client-secret*> | Der geheime Clientschlüssel zum Anfordern der Autorisierung |
| **Pfx** | `pfx` | Ja, aber nur für den Anmeldeinformationstyp „Zertifikat“ | <*encoded-pfx-file-content*> | Der base64-codierte Inhalt aus einer Personal Information Exchange-Datei (PFX) |
| **Kennwort** | `password` | Ja, aber nur für den Anmeldeinformationstyp „Zertifikat“ | <*password-for-pfx-file*> | Der Parameter für den Zugriff auf die PFX-Datei |
|||||

Wenn Sie [abgesicherte Parameter](#secure-action-parameters) verwenden, um vertrauliche Informationen zu verarbeiten und zu schützen, z. B. in einer [Azure Resource Manager-Vorlage zur Automatisierung der Bereitstellung](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), können Sie zur Runtime mit Ausdrücken auf diese Parameterwerte zugreifen. In dieser Beispieldefinition einer HTTP-Aktion werden der `type` der Authentifizierung als `ActiveDirectoryOAuth`, der Anmeldeinformationstyp als `Secret` angegeben und die Funktion [parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) verwendet, um die Parameterwerte abzurufen:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "@parameters('endpointUrlParam')",
      "authentication": {
         "type": "ActiveDirectoryOAuth",
         "tenant": "@parameters('tenantIdParam')",
         "audience": "https://management.core.windows.net/",
         "clientId": "@parameters('clientIdParam')",
         "credentialType": "Secret",
         "secret": "@parameters('secretParam')"
     }
   },
   "runAfter": {}
}
```

<a name="raw-authentication"></a>

### <a name="raw-authentication"></a>Raw-Authentifizierung

Sofern die Option **Raw** verfügbar ist, können Sie diesen Authentifizierungstyp verwenden, wenn Sie [Authentifizierungsschemas](https://iana.org/assignments/http-authschemes/http-authschemes.xhtml) verwenden müssen, die nicht das Protokoll [OAuth 2.0](https://oauth.net/2/) befolgen. Bei diesem Typ legen Sie den Wert des Autorisierungsheaders, den Sie mit der ausgehenden Anforderung senden, manuell fest und geben diesen Headerwert in Ihrem Trigger oder Ihrer Aktion an.

Hier ist zum Beispiel ein Beispielheader für eine HTTPS-Anforderung, die das Protokoll [OAuth 1.0](https://tools.ietf.org/html/rfc5849) befolgt:

```text
Authorization: OAuth realm="Photos",
   oauth_consumer_key="dpf43f3p2l4k3l03",
   oauth_signature_method="HMAC-SHA1",
   oauth_timestamp="137131200",
   oauth_nonce="wIjqoS",
   oauth_callback="http%3A%2F%2Fprinter.example.com%2Fready",
   oauth_signature="74KNZJeDHnMBp0EMJ9ZHt%2FXKycU%3D"
```

Geben Sie in dem Trigger oder der Aktion, die die Raw-Authentifizierung unterstützt, diese Eigenschaftswerte an:

| Eigenschaft (Designer) | Eigenschaft (JSON) | Erforderlich | Wert | BESCHREIBUNG |
|---------------------|-----------------|----------|-------|-------------|
| **Authentifizierung** | `type` | Ja | Raw | Der zu verwendende Authentifizierungstyp |
| **Wert** | `value` | Ja | <*authorization-header-value*> | Der für die Authentifizierung zu verwendende Wert des Autorisierungsheaders |
||||||

Wenn Sie [abgesicherte Parameter](#secure-action-parameters) verwenden, um vertrauliche Informationen zu verarbeiten und zu schützen, z. B. in einer [Azure Resource Manager-Vorlage zur Automatisierung der Bereitstellung](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), können Sie zur Runtime mit Ausdrücken auf diese Parameterwerte zugreifen. In dieser Beispieldefinition einer HTTP-Aktion werden der `type` der Authentifizierung als `Raw` angegeben und die Funktion [parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) verwendet, um die Parameterwerte abzurufen:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "@parameters('endpointUrlParam')",
      "authentication": {
         "type": "Raw",
         "value": "@parameters('authHeaderParam')"
      }
   },
   "runAfter": {}
}
```

<a name="managed-identity-authentication"></a>

### <a name="managed-identity-authentication"></a>Authentifizierung der verwalteten Identität

Wenn die Option [Verwaltete Identität](../active-directory/managed-identities-azure-resources/overview.md) verfügbar ist, kann Ihre Logik-App die systemseitig zugewiesene Identität oder eine *einzelne*, manuell erstellte, benutzerseitig zugewiesene Identität verwenden, um den Zugriff auf andere Ressourcen, die von Azure Active Directory (Azure AD) geschützt werden, ohne Anmeldung zu authentifizieren. Azure verwaltet diese Identität für Sie und dient als Hilfe beim Schützen Ihrer Anmeldeinformationen, da Sie keine Geheimnisse angeben oder eine Rotation dafür durchführen müssen. Erfahren Sie mehr zu [Azure-Diensten, die verwaltete Identitäten für die Azure AD-Authentifizierung unterstützen](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication).

1. Bevor Ihre Logik-App eine verwaltete Identität verwenden kann, führen Sie die Schritte in [Authentifizieren und Zugreifen auf Ressourcen mit verwalteten Identitäten in Azure Logic Apps](../logic-apps/create-managed-service-identity.md) aus. Diese Schritte aktivieren die verwaltete Identität in Ihrer Logik-App und richten den Zugriff dieser Identität auf die Zielressource in Azure ein.

1. Bevor eine Azure-Funktion eine verwaltete Identität verwenden kann, aktivieren Sie zuerst [die Authentifizierung für Azure-Funktionen](../logic-apps/logic-apps-azure-functions.md#enable-authentication-for-azure-functions).

1. Geben Sie in dem Trigger oder der Aktion, in dem/der Sie die verwaltete Identität verwenden möchten, diese Eigenschaftswerte an:

   | Eigenschaft (Designer) | Eigenschaft (JSON) | Erforderlich | Wert | BESCHREIBUNG |
   |---------------------|-----------------|----------|-------|-------------|
   | **Authentifizierung** | `type` | Ja | **Verwaltete Identität** <br>oder <br>`ManagedServiceIdentity` | Der zu verwendende Authentifizierungstyp |
   | **Verwaltete Identität** | `identity` | Ja | * **Systemseitig zugewiesene verwaltete Identität** <br>oder <br>`SystemAssigned` <p><p>* <*Name-der-benutzerseitig-zugewiesenen-Identität*> | Die zu verwendende verwaltete Identität |
   | **Zielgruppe** | `audience` | Ja | <*target-resource-ID*> | Die Ressourcen-ID der Zielressource, auf die Sie zugreifen möchten. <p>Beispielsweise macht `https://storage.azure.com/` die [Zugriffstoken](../active-directory/develop/access-tokens.md) für die Authentifizierung für alle Speicherkonten gültig. Sie können jedoch auch für ein bestimmtes Speicherkonto eine Stammdienst-URL angeben, z. B. `https://fabrikamstorageaccount.blob.core.windows.net`. <p>**Hinweis**: Die Eigenschaft **Zielgruppe** kann in einigen Triggern oder Aktionen ausgeblendet sein. Um diese Eigenschaft einzublenden, öffnen Sie für den Trigger oder die Aktion die Liste **Neuen Parameter hinzufügen**, und wählen Sie **Zielgruppe** aus. <p><p>**Wichtig**: Vergewissern Sie sich, dass diese Zielressourcen-ID *genau dem Wert entspricht*, den Azure AD erwartet, einschließlich aller erforderlichen nachgestellten Schrägstriche. Daher erfordert die `https://storage.azure.com/`-Ressourcen-ID für alle Azure Blob Storage-Konten einen nachgestellten Schrägstrich. Allerdings erfordert die Ressourcen-ID für ein bestimmtes Speicherkonto keinen nachgestellten Schrägstrich. Diese Ressourcen-IDs finden Sie unter [Azure-Dienste, die die Azure AD-Authentifizierung unterstützen](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication). |
   |||||

   Wenn Sie [abgesicherte Parameter](#secure-action-parameters) verwenden, um vertrauliche Informationen zu verarbeiten und zu schützen, z. B. in einer [Azure Resource Manager-Vorlage zur Automatisierung der Bereitstellung](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), können Sie zur Runtime mit Ausdrücken auf diese Parameterwerte zugreifen. In dieser Beispieldefinition einer HTTP-Aktion werden der `type` der Authentifizierung als `ManagedServiceIdentity` angegeben und die Funktion [parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) verwendet, um die Parameterwerte abzurufen:

   ```json
   "HTTP": {
      "type": "Http",
      "inputs": {
         "method": "GET",
         "uri": "@parameters('endpointUrlParam')",
         "authentication": {
            "type": "ManagedServiceIdentity",
            "identity": "SystemAssigned",
            "audience": "https://management.azure.com/"
         },
      },
      "runAfter": {}
   }
   ```

<a name="block-connections"></a>

## <a name="block-creating-connections"></a>Blockieren der Verbindungserstellung

Falls Ihre Organisation das Herstellen einer Verbindung mit bestimmten Ressourcen mithilfe der Connectors in Azure Logic Apps nicht erlaubt, können Sie mithilfe von [Azure Policy](../governance/policy/overview.md) für bestimmte Connectors in Logik-App-Workflows die [Funktion zum Erstellen dieser Verbindungen blockieren](../logic-apps/block-connections-connectors.md) Weitere Informationen finden Sie unter [Blockieren der von Connectors in Azure Logic Apps erstellten Verbindungen](../logic-apps/block-connections-connectors.md)

<a name="isolation-logic-apps"></a>

## <a name="isolation-guidance-for-logic-apps"></a>Isolationsanleitung für Logik-Apps

Sie können Azure Logic Apps in [Azure Government](../azure-government/documentation-government-welcome.md) verwenden und dabei alle Auswirkungsstufen in den Regionen unterstützen, die in der [Azure Government Auswirkungsstufe 5-Isolationsanleitung](../azure-government/documentation-government-impact-level-5.md#azure-logic-apps) sowie im [US Department of Defense Cloud Computing Security Requirements Guide (SRG)](https://dl.dod.cyber.mil/wp-content/uploads/cloud/SRG/index.html) beschrieben sind. Um diese Anforderungen zu erfüllen, bietet Ihnen Logic Apps die Möglichkeit, Workflows in einer Umgebung mit dedizierten Ressourcen zu erstellen und auszuführen, damit Sie die Leistungsauswirkungen durch andere Azure-Mandanten auf Ihre Logik-Apps verringern und die Freigabe von Computingressourcen für andere Mandanten vermeiden können.

* Um Ihren eigenen Code auszuführen oder eine XML-Transformation auszuführen, [erstellen und rufen Sie eine Azure-Funktion auf](../logic-apps/logic-apps-azure-functions.md), anstatt die [Inlinecodefunktion](../logic-apps/logic-apps-add-run-inline-code.md) zu verwenden, oder stellen Sie [als Zuordnungen zu verwendende Assemblys bereit](../logic-apps/logic-apps-enterprise-integration-maps.md). Richten Sie außerdem die Hostingumgebung für ihre Funktions-App so ein, dass Sie Ihren Isolationsanforderungen entspricht.

  Um z. B. Anforderungen der Auswirkungsstufe 5 zu erfüllen, erstellen Sie Ihre Funktions-App mit dem [App Service Plan](../azure-functions/functions-scale.md#app-service-plan), der den Tarif [**Isoliert**](../app-service/overview-hosting-plans.md) zusammen mit einer [App Service-Umgebung (ASE)](../app-service/environment/intro.md) verwendet, die ebenfalls den Tarif **Isoliert** verwendet. In dieser Umgebung werden Funktions-Apps auf dedizierte Azure-VMs und in dedizierten virtuellen Azure-Netzwerken ausgeführt, die zusätzlich zur Computeisolation eine Netzwerkisolation für Ihre Apps bieten sowie maximale horizontale Skalierungsmöglichkeiten. Weitere Informationen finden Sie unter [Azure Government-Auswirkungsstufe 5-Isolationsanleitung – Azure Functions](../azure-government/documentation-government-impact-level-5.md#azure-functions).

  Weitere Informationen finden Sie in den folgenden Themen:<p>

  * [Azure App Service-Pläne](../app-service/overview-hosting-plans.md)
  * [Netzwerkoptionen von Azure Functions](../azure-functions/functions-networking-options.md)
  * [Azure Dedicated Hosts für virtuelle Computer](../virtual-machines/windows/dedicated-hosts.md)
  * [Isolation von virtuellen Computern in Azure](../virtual-machines/isolation.md)
  * [Bereitstellen von dedizierten Azure-Diensten in virtuellen Netzwerken](../virtual-network/virtual-network-for-azure-services.md)

* Um Logik-Apps zu erstellen, die auf dedizierten Ressourcen ausgeführt werden und die auf Ressourcen zugreifen können, die von einem virtuellen Azure-Netzwerk geschützt werden, können Sie eine [Integrationsdienstumgebung (Integration Service Environment, ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) erstellen.

  * Einige virtuelle Azure-Netzwerke verwenden private Endpunkte ([Azure Private Link](../private-link/private-link-overview.md)), um den Zugriff auf Azure-PaaS-Dienste wie Azure Storage, Azure Cosmos DB oder Azure SQL-Datenbank sowie auf Partnerdienste oder auf Kundendienste zu ermöglichen, die in Azure gehostet werden. Falls Ihre Logik-Apps Zugriff auf virtuelle Netzwerke mit privaten Endpunkten benötigen, müssen diese Logik-Apps in einer ISE erstellt, bereitgestellt und ausgeführt werden.

  * Für mehr Kontrolle über die von Azure Storage verwendeten Verschlüsselungsschlüssel können Sie mit [Azure Key Vault](../key-vault/general/overview.md) Ihren eigenen Schlüssel einrichten, verwenden und verwalten. Diese Funktion wird auch als „Bring Your Own Key“ (BYOK) bezeichnet, und Ihr Schlüssel wird als „vom Kunden verwalteter Schlüssel“ bezeichnet. Weitere Informationen finden Sie unter [Einrichten von kundenseitig verwalteten Schlüsseln zum Verschlüsseln von ruhenden Daten für Integrationsdienstumgebungen (Integration Service Environment, ISE) in Azure Logic Apps](../logic-apps/customer-managed-keys-integration-service-environment.md).

Weitere Informationen finden Sie in den folgenden Themen:

* [Isolation in der öffentlichen Azure-Cloud](../security/fundamentals/isolation-choices.md)
* [Sicherheit für höchst sensible IaaS-Apps in Azure](/azure/architecture/reference-architectures/n-tier/high-security-iaas)

## <a name="next-steps"></a>Nächste Schritte

* [Azure-Sicherheitsbaseline für Azure Logic Apps](../logic-apps/security-baseline.md)
* [Automatisieren der Bereitstellung für Azure Logic Apps](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)
* [Überwachen von Logik-Apps](../logic-apps/monitor-logic-apps-log-analytics.md)
