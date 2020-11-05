---
title: SaaS-Fulfillment-APIs V2 im kommerziellen Microsoft-Marketplace
description: Hier erfahren Sie, wie Sie ein SaaS-Angebot in Microsoft AppSource und im Azure Marketplace mithilfe der Fulfillment-APIs der Version 2 erstellen und verwalten.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 06/10/2020
author: mingshen-ms
ms.author: mingshen
ms.openlocfilehash: 06a2a5bbe637cd2366dbdf218c0278cd683635df
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93130033"
---
# <a name="saas-fulfillment-apis-version-2-in-the-commercial-marketplace"></a>SaaS-Fulfillment-APIs (Version 2) im kommerziellen Marketplace

In diesem Artikel werden die APIs ausführlich beschrieben, mit denen Partner ihre SaaS-Angebote in Microsoft AppSource und im Azure Marketplace verkaufen können. Ein Herausgeber muss die Integration in diese APIs implementieren, um ein transaktionsfähiges SaaS-Angebot im Partner Center zu veröffentlichen.

## <a name="managing-the-saas-subscription-life-cycle"></a>Verwalten des Lebenszyklus von SaaS-Abonnements

Der kommerzielle Marketplace verwaltet den gesamten Lebenszyklus eines SaaS-Abonnements nach dem Erwerb durch den Endkunden.  Es werden die Angebotsseite, Fulfillment-APIs, Vorgangs-APIs und der Webhook als Mechanismus verwendet, um die tatsächliche Aktivierung und Nutzung des SaaS-Abonnements, Aktualisierungen und die Kündigung des Abonnements zu steuern.  Die Abrechnung für den Endkunden basiert auf dem Status des SaaS-Abonnements, das Microsoft verwaltet. 

### <a name="states-of-a-saas-subscription"></a>Zustände eines SaaS-Abonnements

Die Zustände eines SaaS-Abonnements und anwendbare Aktionen werden angezeigt.

![Lebenszyklus eines SaaS-Abonnements im Marketplace](./media/saas-subscription-lifecycle-api-v2.png)

#### <a name="purchased-but-not-yet-activated-pendingfulfillmentstart"></a>Gekauft, aber noch nicht aktiviert ( *PendingFulfillmentStart* )

Nachdem ein Endkunde (oder CSP) ein SaaS-Angebot im Marketplace erworben hat, sollte der Herausgeber über den Kauf benachrichtigt werden, damit ein neues SaaS-Konto für den Endkunden auf Herausgeberseite erstellt und konfiguriert wird.

Ablauf für die Kontoerstellung:

1. Der Kunde muss auf die Schaltfläche **Konfigurieren** klicken, die nach dem erfolgreichen Kauf in Microsoft AppSource oder im Azure-Portal für ein SaaS-Angebot verfügbar ist. Diese kann auch in der E-Mail enthalten sein, die der Kunde kurz nach dem Kauf erhält.
2. Anschließend benachrichtigt Microsoft den Partner über den Kauf, indem die URL der Angebotsseite mit dem Tokenparameter (dem Identifizierungstoken für den Kauf im kommerziellen Marketplace) auf der neuen Browserregisterkarte geöffnet wird.

Ein Beispiel für einen solchen Aufruf ist `https://contoso.com/signup?token=<blob>`, wobei die URL der Angebotsseite für dieses SaaS-Angebot im Partner Center als `https://contoso.com/signup` konfiguriert ist. Dieses Token stellt dem Herausgeber eine ID bereit, die den SaaS-Kauf und den Kunden eindeutig identifiziert.

>[!NOTE]
>Der Herausgeber wird erst dann über den SaaS-Kauf benachrichtigt, wenn der Kunde den Konfigurationsprozess auf Microsoft-Seite initiiert.

Die URL der Angebotsseite muss rund um die Uhr aktiv sein und jederzeit neue Aufrufe von Microsoft empfangen können. Wenn die Angebotsseite nicht mehr verfügbar ist, können sich Kunden nicht mehr für den SaaS-Dienst registrieren und mit dessen Nutzung beginnen.

Anschließend muss das *Token* vom Herausgeber zurück an Microsoft übergeben werden, indem die [SaaS-Auflösungs-API](#resolve-a-purchased-subscription) als Wert des Headerparameters `x-ms-marketplace-token header` aufgerufen wird.  Durch den Aufruf der Auflösungs-API wird das Token durch die Details des SaaS-Kaufs ersetzt, z. B. die eindeutige ID des Kaufs, die ID des erworbenen Angebots, die ID des erworbenen Plans usw.

Auf der Angebotsseite sollte der Kunde über einmaliges Anmelden (Single Sign-On, SSO) von Azure Active Directory (Azure AD) beim neuen oder vorhandenen SaaS-Konto angemeldet sein.

Der Herausgeber sollte die SSO-Anmeldung implementieren, um die von Microsoft für diesen Ablauf benötigte Benutzerfunktion bereitzustellen.  Stellen Sie sicher, dass Sie beim Konfigurieren von SSO eine mehrinstanzenfähige Azure AD-Anwendung verwenden und sowohl Geschäfts-, Schul- oder Unikonten als auch persönliche Microsoft-Konten zulassen.  Diese Anforderung gilt nur für die Angebotsseite und für Benutzer, die an den SaaS-Dienst weitergeleitet werden, wenn sie bereits mit Microsoft-Anmeldeinformationen angemeldet sind. Sie gilt nicht für alle Anmeldungen beim SaaS-Dienst.

> [!NOTE]
>Wenn die SSO-Anmeldung erfordert, dass ein Administrator die Berechtigung für eine App erteilt, muss in der Beschreibung des Angebots im Partner Center angegeben sein, dass ein Zugriff auf Administratorebene erforderlich ist. Dadurch wird den [Zertifizierungsrichtlinien für den kommerziellen Marketplace](/legal/marketplace/certification-policies#10003-authentication-options) entsprochen.

Nach der Anmeldung sollte der Kunde die SaaS-Konfiguration auf Herausgeberseite vervollständigen. Anschließend muss der Herausgeber die [API zum Aktivieren des Abonnements](#activate-a-subscription) aufrufen, um ein Signal an den Marketplace zu senden, dass die Bereitstellung des SaaS-Kontos abgeschlossen ist.
Dadurch wird der Abrechnungszeitraum des Kunden gestartet. Wird die API zum Aktivieren des Abonnements nicht erfolgreich aufgerufen, wird dem Kunden der Kauf nicht in Rechnung gestellt.


![API-Aufrufe für ein Bereitstellungsszenario](./media/saas-update-api-v2-calls-from-saas-service-a.png) 

#### <a name="active-subscribed"></a>Aktiv (Subscribed)

Dieser Status ist der stabile Zustand eines bereitgestellten SaaS-Abonnements. Nachdem der Aufruf der [API zum Aktivieren des Abonnements](#activate-a-subscription) auf Microsoft-Seite verarbeitet wurde, wird das SaaS-Abonnement als abonniert gekennzeichnet. Der SaaS-Dienst kann jetzt vom Kunden auf der Website des Herausgebers verwendet werden und wird dem Kunden in Rechnung gestellt.

Wenn das SaaS-Abonnement bereits aktiv ist und der Kunde die SaaS-Funktion **Verwalten** im Azure-Portal oder M365 Admin Center auswählt, wird die **URL der Angebotsseite** erneut von Microsoft mit demselben *token* -Parameter wie beim Aktivierungsablauf aufgerufen.  Der Herausgeber sollte zwischen neuen Käufen und der Verwaltung vorhandener SaaS-Konten unterscheiden und diesen Aufruf der URL der Angebotsseite entsprechend handhaben.

#### <a name="being-updated-subscribed"></a>Wird aktualisiert (Subscribed)

Diese Aktion bedeutet, dass eine Aktualisierung für ein vorhandenes aktives SaaS-Abonnement sowohl von Microsoft als auch vom Herausgeber verarbeitet wird. Eine solche Aktualisierung kann auf folgende Arten initiiert werden:

- vom Kunden über den kommerziellen Marketplace
- vom CSP über den kommerziellen Marketplace
- vom Kunden über die SaaS-Website des Herausgebers (gilt nicht für vom CSP getätigte Käufe)

Für ein SaaS-Abonnement sind zwei Arten von Aktualisierungen verfügbar:

- Aktualisierung des Plans, wenn der Kunde einen anderen Plan für das Abonnement auswählt
- Aktualisierung der Menge, wenn der Kunde die Anzahl erworbener Arbeitsplätze für das Abonnement ändert

Nur ein aktives Abonnement kann aktualisiert werden. Während das Abonnement aktualisiert wird, lautet der Status auf Microsoft-Seite weiterhin „Aktiv“.

##### <a name="update-initiated-from-the-marketplace"></a>Über den Marketplace initiierte Aktualisierung

Bei diesem Ablauf ändert der Kunde den Abonnementplan oder die Menge der Arbeitsplätze über das M365 Admin Center.  

1. Nachdem eine Aktualisierung eingegeben wurde, ruft Microsoft die Webhook-URL des Herausgebers, die im Feld **Verbindungswebhook** im Partner Center konfiguriert ist, mit einem geeigneten Wert für *action* und anderen relevanten Parametern auf.  
1. Auf Herausgeberseite müssen die erforderlichen Änderungen am SaaS-Dienst vorgenommen werden, und Microsoft muss durch Aufrufen der [API zum Aktualisieren des Vorgangsstatus](#update-the-status-of-an-operation) benachrichtigt werden, sobald die Änderung abgeschlossen ist.
1. Wenn der Patch mit dem Status „Fehler“ gesendet wird, wird der Aktualisierungsvorgang auf Microsoft-Seite nicht abgeschlossen.  Das SaaS-Abonnement weist weiterhin den vorhandenen Plan und die vorhandene Menge von Arbeitsplätzen auf.

Die Abfolge der API-Aufrufe für ein über den Marketplace initiiertes Aktualisierungsszenario ist nachfolgend dargestellt.

![API-Aufrufe für eine über den Marketplace initiierte Aktualisierung](./media/saas-update-status-api-v2-calls-marketplace-side.png)

##### <a name="update-initiated-from-the-publisher"></a>Über den Herausgeber initiierte Aktualisierung

Bei diesem Ablauf ändert der Kunde den Abonnementplan oder die Menge der erworbenen Arbeitsplätze über den SaaS-Dienst selbst. 

1. Der Herausgebercode muss die [Planänderungs-API](#change-the-plan-on-the-subscription) und/oder die [Mengenänderungs-API](#change-the-quantity-of-seats-on-the-saas-subscription) aufrufen, bevor die angeforderte Änderung auf Herausgeberseite vorgenommen wird. 

1. Microsoft wendet die Änderung auf das Abonnement an und fordert dann den Herausgeber über den **Verbindungswebhook** auf, die gleiche Änderung vorzunehmen.  

1. Erst dann sollte der Herausgeber die erforderliche Änderung am SaaS-Abonnement durchführen und Microsoft durch Aufrufen der [API zum Aktualisieren des Vorgangsstatus](#update-the-status-of-an-operation) benachrichtigen, sobald die Änderung erfolgt ist.

Die Abfolge der API-Aufrufe für das über die Herausgeberseite initiierte Aktualisierungsszenario ist nachfolgend dargestellt.

![API-Aufrufe für eine über die Herausgeberseite initiierte Aktualisierung](./media/saas-update-status-api-v2-calls-publisher-side.png)

#### <a name="suspended-suspended"></a>Ausgesetzt ( *Suspended* )

Dieser Status gibt an, dass die Zahlung eines Kunden für den SaaS-Dienst noch nicht eingegangen ist. Der Herausgeber wird von Microsoft über diese Änderung des SaaS-Abonnementstatus benachrichtigt. Die Benachrichtigung erfolgt über einen Aufruf an den Webhook, wobei der *action* -Parameter auf *Suspended* festgelegt ist.

Der Herausgeber kann auf Wunsch Änderungen am SaaS-Dienst auf Herausgeberseite vornehmen. Es wird empfohlen, dass der Herausgeber diese Informationen dem jeweiligen Kunden bereitstellt und den Zugriff des Kunden auf den SaaS-Dienst einschränkt oder blockiert.  Es besteht die Möglichkeit, dass die Zahlung niemals eingeht.

Microsoft gewährt dem Kunden eine Karenzzeit von 30 Tagen, bevor das Abonnement automatisch gekündigt wird. Wenn sich ein Abonnement im Status „Ausgesetzt“ befindet, ist Folgendes gegeben:

* Das SaaS-Konto muss vom ISV in einem wiederherstellbaren Zustand beibehalten werden. Die volle Funktionsfähigkeit kann ohne Verlust von Daten oder Einstellungen wiederhergestellt werden.
* Sie erhalten eine Reaktivierungsanforderung für dieses Abonnement, wenn die Zahlung während der Karenzzeit eingeht, oder eine Anforderung zum Aufheben der Bereitstellung am Ende der Karenzzeit. Beides erfolgt über den Webhookmechanismus.

Der Abonnementstatus wird auf Microsoft-Seite in „Ausgesetzt“ geändert, bevor der Herausgeber eine Aktion ausführt. Nur aktive Abonnements können ausgesetzt werden.

#### <a name="reinstated-suspended"></a>Reaktiviert ( *Suspended* )

Das Abonnement wird reaktiviert.

Diese Aktion gibt an, dass die Zahlungsmethode des Kunden wieder gültig ist und eine Zahlung für das SaaS-Abonnement erfolgt.  Das Abonnement wird reaktiviert. In diesem Fall: 

1. Microsoft ruft den Webhook mit einem *action* -Parameter auf, der auf den Wert *Reinstate* festgelegt ist.  
1. Der Herausgeber stellt sicher, dass dieses Abonnement auf Herausgeberseite wieder voll funktionsfähig ist.
1. Der Herausgeber ruft die [Patchvorgang-API](#update-the-status-of-an-operation) mit dem Status „Erfolgreich“ auf.  
1. Die Reaktivierung ist nun erfolgreich, und dem Kunden wird das SaaS-Abonnement wieder in Rechnung gestellt. 
1. Wenn der Patch mit dem Status „Fehler“ gesendet wird, wird der Reaktivierungsvorgang auf Microsoft-Seite nicht abgeschlossen. Das Abonnement bleibt ausgesetzt.

Wenn der Patch mit dem Status „Fehler“ gesendet wird, wird der Reaktivierungsvorgang auf Microsoft-Seite nicht abgeschlossen.  Das Abonnement bleibt ausgesetzt.

Nur ein ausgesetztes Abonnement kann reaktiviert werden.  Während der Reaktivierung eines SaaS-Abonnements verbleibt der Status auf „Ausgesetzt“.  Sobald dieser Vorgang abgeschlossen ist, lautet der Status des Abonnements wieder „Aktiv“.

#### <a name="renewed-subscribed"></a>Verlängert ( *Subscribed* )

Am Ende der Abonnementlaufzeit (nach einem Monat oder einem Jahr) wird das SaaS-Abonnement automatisch von Microsoft verlängert.  Die Standardeinstellung für die automatische Verlängerung lautet *true* für alle SaaS-Abonnements. Aktive SaaS-Abonnements werden weiter in regelmäßigen Abständen verlängert. Microsoft benachrichtigt den Herausgeber nicht, wenn ein Abonnement verlängert wird. Ein Kunde kann die automatische Verlängerung für ein SaaS-Abonnement über das M365-Verwaltungsportal oder das Azure-Portal deaktivieren.  In diesem Fall wird das SaaS-Abonnement am Ende des aktuellen Abrechnungszeitraums automatisch gekündigt.  Kunden können das SaaS-Abonnement auch zu jedem beliebigen Zeitpunkt kündigen.

Nur aktive Abonnements werden automatisch verlängert.  Abonnements bleiben während des Verlängerungsvorgangs und bei erfolgreicher automatischer Verlängerung aktiv.  Nach der Verlängerung werden das Start- und Enddatum der Abonnementlaufzeit auf die Datumsangaben der neuen Laufzeit aktualisiert.

Wenn bei einer automatischen Verlängerung aufgrund eines Zahlungsproblems ein Fehler auftritt, wird das Abonnement ausgesetzt.  Der Herausgeber wird benachrichtigt.

#### <a name="canceled-unsubscribed"></a>Gekündigt ( *Unsubscribed* ) 

Abonnements erreichen diesen Status entweder als Reaktion auf eine explizite Aktion des Kunden oder CSP oder durch Kündigung eines Abonnements über die Website des Herausgebers, das Azure-Portal oder das M365 Admin Center.  Ein Abonnement kann auch implizit bei Nichtbezahlung der fälligen Beträge gekündigt werden, nachdem es 30 Tage lang den ausgesetzten Status aufwies.

Nachdem ein Webhookaufruf zur Kündigung empfangen wurde, muss der Herausgeber die Kundendaten für die Wiederherstellung auf Anforderung mindestens sieben Tage lang aufbewahren. Erst dann können Kundendaten gelöscht werden.

Ein SaaS-Abonnement kann jederzeit während des Lebenszyklus gekündigt werden. Nach der Kündigung kann ein Abonnement nicht reaktiviert werden.

## <a name="api-reference"></a>API-Referenz

In diesem Abschnitt werden die SaaS-Abonnement-APIs und SaaS-Vorgangs-APIs beschrieben.

**Abonnement-APIs** sollten zur Handhabung des Lebenszyklus des SaaS-Abonnements vom Kauf bis zur Kündigung verwendet werden.

**Vorgangs-APIs** sollten für Folgendes verwendet werden:

* Überprüfen und Bestätigen der verarbeiteten Webhookaufrufe
* Abrufen einer Liste von ausstehenden Vorgängen für Apps, die auf Bestätigung durch den Herausgeber warten

### <a name="enforcing-tls-12-note"></a>Hinweis zum Erzwingen von TLS 1.2

Die TLS-Version 1.2 wird bald als Mindestversion für die HTTPS-Kommunikation erzwungen. Stellen Sie sicher, dass Sie diese TLS-Version in Ihrem Code verwenden.  TLS-Version 1.0 und 1.1 werden bald nicht mehr unterstützt.

### <a name="subscription-apis"></a>Abonnement-APIs

#### <a name="resolve-a-purchased-subscription"></a>Auflösen eines erworbenen Abonnements

Der Auflösungsendpunkt ermöglicht es dem Herausgeber, das Identifizierungstoken für den Marketplace-Kauf (im Abschnitt [Gekauft, aber noch nicht aktiviert](#purchased-but-not-yet-activated-pendingfulfillmentstart) als *Token* bezeichnet) gegen eine persistente ID für das erworbene SaaS-Abonnement und dessen Details auszutauschen.

Wenn ein Kunde an die URL der Angebotsseite des Partners weitergeleitet wird, wird das Identifizierungstoken des Kunden in diesem URL-Aufruf als *token* -Parameter übergeben. Es wird erwartet, dass der Partner dieses Token verwendet und dessen Auflösung anfordert. Die Antwort der Auflösungs-API enthält die SaaS-Abonnement-ID und andere Details zur eindeutigen Identifizierung des Kaufs. Das mit dem Aufruf der URL der Angebotsseite angegebene *Token* ist in der Regel 24 Stunden gültig. Wenn das von Ihnen erhaltene *Token* bereits abgelaufen ist, wird empfohlen, dem Endkunden die folgenden Anweisungen bereitzustellen:

„Dieser Kauf konnte nicht identifiziert werden. Öffnen Sie dieses SaaS-Abonnement noch einmal im Azure-Portal oder im M365 Admin Center, und klicken Sie erneut auf die Schaltfläche „Konto konfigurieren“ oder „Konto verwalten“.“

Durch das Aufrufen der Auflösungs-API werden Abonnementdetails und der Status für SaaS-Abonnements in allen unterstützten Status zurückgegeben.

##### <a name="posthttpsmarketplaceapimicrosoftcomapisaassubscriptionsresolveapi-versionapiversion"></a>Post`https://marketplaceapi.microsoft.com/api/saas/subscriptions/resolve?api-version=<ApiVersion>`

*Abfrageparameter:*

|  Parameter         | Wert            |
|  ---------------   |  ---------------  |
|  `ApiVersion`        |  Verwenden Sie 2018-08-31.   |

*Anforderungsheader:*

|  Parameter         | Wert             |
|  ---------------   |  ---------------  |
|  `content-type`      | `application/json` |
|  `x-ms-requestid`    |  Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
|  `x-ms-correlationid` |  Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
|  `authorization`     |  Ein eindeutiges Zugriffstoken, das den Herausgeber identifiziert, der diesen API-Aufruf sendet. Das Format ist `"Bearer <accessaccess_token>"`, wenn der Tokenwert vom Herausgeber abgerufen wird, wie es unter [Abrufen eines Tokens basierend auf der Azure AD-App](./pc-saas-registration.md#get-the-token-with-an-http-post) beschrieben ist. |
|  `x-ms-marketplace-token`  | Der aufzulösende *token* -Parameter zur Identifizierung des Marketplace-Kaufs.  Das Token wird im Aufruf der URL der Angebotsseite übergegeben, wenn der Kunde zur Website des SaaS-Partners weitergeleitet wird (z. B. `https://contoso.com/signup?token=<token><authorization_token>`). <br> <br>  *Hinweis:* Der *token* -Wert ist als Teil der URL der Angebotsseite codiert und muss daher decodiert werden, bevor er als Parameter in diesem API-Aufruf verwendet wird.  <br> <br> Ein Beispiel für eine codierte Zeichenfolge in der URL sieht wie folgt aus: `contoso.com/signup?token=ab%2Bcd%2Fef`. Hierbei ist `ab%2Bcd%2Fef` das Token.  Das gleiche Token sieht decodiert wie folgt aus: `Ab+cd/ef` |
| | |

*Antwortcodes:*

Code: 200 – Gibt eindeutige SaaS-Abonnement-IDs basierend auf dem bereitgestellten `x-ms-marketplace-token` zurück.

Beispiel für einen Antworttext:

```json
{
  "id": "<guid>", // purchased SaaS subscription ID
  "subscriptionName": "Contoso Cloud Solution", // SaaS subscription name
  "offerId": "offer1", // purchased offer ID
  "planId": "silver", // purchased offer's plan ID
  "quantity": "20", // number of purchased seats, might be empty if the plan is not per seat
  "subscription": { // full SaaS subscription details, see Get Subscription APIs response body for full description
    "id": "<guid>",
    "publisherId": "contoso",
    "offerId": "offer1",
    "name": "Contoso Cloud Solution",
    "saasSubscriptionStatus": " PendingFulfillmentStart ",
    "beneficiary": {
      "emailId": "test@test.com",
      "objectId": "<guid>",
      "tenantId": "<guid>",
      "pid": "<ID of the user>"
    },
    "purchaser": {
      "emailId": "test@test.com",
      "objectId": "<guid>",
      "tenantId": "<guid>",
      "pid": "<ID of the user>"
    },
    "planId": "silver",
    "term": {
      "termUnit": "P1M",
      "startDate": "2019-05-31",
      "endDate": "2019-06-29"
    },
    "isTest": true,
    "isFreeTrial": false,
    "allowedCustomerOperations": ["Delete", "Update", "Read"],
    "sandboxType": "None",
    "sessionMode": "None"
  }
}

```

Code: 400 – Ungültige Anforderung. `x-ms-marketplace-token` fehlt, ist falsch formatiert, ungültig oder abgelaufen.

Code: 403 – Unzulässig. Das Autorisierungstoken ist ungültig, abgelaufen oder wurde nicht angegeben.  Die Anforderung versucht, auf ein SaaS-Abonnement für ein Angebot zuzugreifen, das mit einer anderen Azure AD-App-ID veröffentlicht wurde als der, die zum Erstellen des Autorisierungstokens verwendet wurde.

Dieser Fehler ist häufig ein Symptom dafür, dass die [SaaS-Registrierung](pc-saas-registration.md) nicht ordnungsgemäß ausgeführt wird.

Code: 500 – Interner Serverfehler.  Wiederholen Sie den API-Aufruf.  Wenn der Fehler weiterhin auftritt, wenden Sie sich an den [Microsoft-Support](https://partner.microsoft.com/support/v2/?stage=1).

#### <a name="activate-a-subscription"></a>Aktivieren eines Abonnements

Nachdem das SaaS-Konto für einen Endkunden konfiguriert wurde, muss der Herausgeber die API zum Aktivieren des Abonnements auf Microsoft-Seite aufrufen.  Dem Kunden werden erst dann Beträge in Rechnung gestellt, wenn dieser API-Aufruf erfolgreich war.

##### <a name="posthttpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidactivateapi-versionapiversion"></a>Post`https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/activate?api-version=<ApiVersion>`

*Abfrageparameter:*

|  Parameter         | Wert             |
|  --------   |  ---------------  |
| `ApiVersion`  |  Verwenden Sie 2018-08-31.   |
| `subscriptionId` | Ein eindeutiger Bezeichner des erworbenen SaaS-Abonnements.  Diese ID wird abgerufen, nachdem das Marketplace-Autorisierungstoken mithilfe der [Auflösungs-API](#resolve-a-purchased-subscription) aufgelöst wurde.
 |

*Anforderungsheader:*

|  Parameter         | Wert             |
|  ---------------   |  ---------------  |
| `content-type`       |  `application/json`  |
| `x-ms-requestid`     |  Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID.  Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
| `x-ms-correlationid` |  Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client.  Diese Zeichenfolge korreliert alle Ereignisse des Clientvorgangs mit Ereignissen auf der Serverseite.  Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
| `authorization`      |  Ein eindeutiges Zugriffstoken, das den Herausgeber identifiziert, der diesen API-Aufruf sendet. Das Format ist `"Bearer <access_token>"`, wenn der Tokenwert vom Herausgeber abgerufen wird, wie es unter [Abrufen eines Tokens basierend auf der Azure AD-App](./pc-saas-registration.md#get-the-token-with-an-http-post) beschrieben ist. |

*Beispiel einer Anforderungsnutzlast:*

```json
{  // needed for validation of the activation request
  "planId": "gold", // purchased plan, cannot be empty
  "quantity": "" // purchased number of seats, can be empty if plan is not per seat
}
```

*Antwortcodes:*

Code: 200 – Das Abonnement wurde auf Microsoft-Seite als abonniert gekennzeichnet.

Es ist kein Antworttext für diesen Aufruf vorhanden.

Code: 400 – Ungültige Anforderung: Fehler bei der Überprüfung.

* `planId` ist nicht in der Anforderungsnutzlast vorhanden.
* `planId` in der Anforderungsnutzlast stimmt nicht mit der erworbenen überein.
* `quantity` in der Anforderungsnutzlast stimmt nicht mit der erworbenen überein.
* Das SaaS-Abonnement weist den Status „Abonniert“ oder „Ausgesetzt“ auf.

Code: 403 – Unzulässig. Das Autorisierungstoken ist ungültig, abgelaufen oder wurde nicht angegeben. Die Anforderung versucht, auf ein SaaS-Abonnement für ein Angebot zuzugreifen, das mit einer anderen Azure AD-App-ID veröffentlicht wurde als der, die zum Erstellen des Autorisierungstokens verwendet wurde.

Dieser Fehler ist häufig ein Symptom dafür, dass die [SaaS-Registrierung](pc-saas-registration.md) nicht ordnungsgemäß ausgeführt wird.

Code: 404 – Nicht gefunden. Das SaaS-Abonnement weist den Status „Gekündigt“ auf.

Code: 500 – Interner Serverfehler.  Wiederholen Sie den API-Aufruf.  Wenn der Fehler weiterhin auftritt, wenden Sie sich an den [Microsoft-Support](https://partner.microsoft.com/support/v2/?stage=1).

#### <a name="get-list-of-all-subscriptions"></a>Abrufen einer Liste aller Abonnements

Ruft eine Liste aller erworbenen SaaS-Abonnements für alle Angebote ab, die vom Herausgeber im Marketplace veröffentlicht wurden.  SaaS-Abonnements in allen möglichen Status werden zurückgegeben. Gekündigte SaaS-Abonnements werden ebenfalls zurückgegeben, da diese Informationen auf Microsoft-Seite nicht gelöscht werden.

Diese API gibt paginierte Ergebnisse zurück. Die Seitengröße ist 100.

##### <a name="gethttpsmarketplaceapimicrosoftcomapisaassubscriptionsapi-versionapiversion"></a>Get`https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=<ApiVersion>`

*Abfrageparameter:*

|  Parameter         | Wert             |
|  --------   |  ---------------  |
| `ApiVersion`  |  Verwenden Sie 2018-08-31.  |
| `continuationToken`  | Dieser Parameter ist optional. Wenn Sie die erste Seite der Ergebnisse abrufen möchten, lassen Sie den Parameter leer.  Verwenden Sie den Wert, der im Parameter `@nextLink` zurückgegeben wird, um die nächste Seite abzurufen. |

*Anforderungsheader:*

|  Parameter         | Wert             |
|  ---------------   |  ---------------  |
| `content-type`       |  `application/json`  |
| `x-ms-requestid`     |  Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
| `x-ms-correlationid` |  Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client.  Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen.  Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
| `authorization`      |  Ein eindeutiges Zugriffstoken, das den Herausgeber identifiziert, der diesen API-Aufruf sendet.  Das Format ist `"Bearer <access_token>"`, wenn der Tokenwert vom Herausgeber abgerufen wird, wie es unter [Abrufen eines Tokens basierend auf der Azure AD-App](./pc-saas-registration.md#get-the-token-with-an-http-post) beschrieben ist. |

*Antwortcodes:*

Code: 200 – Gibt die Liste aller vorhandenen Abonnements für alle Angebote dieses Herausgebers basierend auf dessen Autorisierungstoken zurück.

*Beispiel für einen Antworttext:*

```json
{
  "subscriptions": [
    {
      "id": "<guid>", // purchased SaaS subscription ID
      "name": "Contoso Cloud Solution", // SaaS subscription name
      "publisherId": "contoso", // publisher ID
      "offerId": "offer1", // purchased offer ID
      "planId": "silver", // purchased plan ID
      "quantity": "10", // purchased amount of seats, will be empty if plan is not per seat
      "beneficiary": { // email address, user ID and tenant ID for which SaaS subscription was purchased.
        "emailId": " test@contoso.com",
        "objectId": "<guid>",
        "tenantId": "<guid>",
        "pid": "<ID of the user>"
      },
      "purchaser": { // email address, user ID and tenant ID that purchased the SaaS subscription. These could be different from beneficiary information for reseller (CSP) purchase
        "emailId": " test@contoso.com",
        "objectId": "<guid>",
        "tenantId": "<guid>",
        "pid": "<ID of the user>"
      },
      "term": { // The period for which the subscription was purchased.
        "startDate": "2019-05-31", //format: YYYY-MM-DD. This is the date when the subscription was activated by the ISV and the billing started. This field is relevant only for Active and Suspended subscriptions.
        "endDate": "2019-06-30", // This is the last day the subscription is valid. Unless stated otherwise, the automatic renew will happen the next day. This field is relevant only for Active and Suspended subscriptions.
        "termUnit": "P1M" // where P1M is monthly and P1Y is yearly. Also reflected in the startDate and endDate values
      },
      "allowedCustomerOperations": ["Read", "Update", "Delete"], // Indicates operations allowed on the SaaS subscription for beneficiary. For CSP-initiated purchases, this will always be "Read" because the customer cannot update or delete subscription in this flow.  Purchaser can perform all operations on the subscription.
      "sessionMode": "None", // not relevant
      "isFreeTrial": true, // true - the customer subscription is currently in free trial, false - the customer subscription is not currently in free trial. (Optional field -– if not returned, the value is false.)
      "isTest": false, // not relevant
      "sandboxType": "None", // not relevant
      "saasSubscriptionStatus": "Subscribed" // Indicates the status of the operation. Can be one of the following: PendingFulfillmentStart, Subscribed, Suspended or Unsubscribed.
    },
    // next SaaS subscription details, might be a different offer
    {
      "id": "<guid1>",
      "name": "Contoso Cloud Solution1",
      "publisherId": "contoso",
      "offerId": "offer2",
      "planId": "gold",
      "quantity": "",
      "beneficiary": {
        "emailId": " test@contoso.com",
        "objectId": "<guid>",
        "tenantId": "<guid>",
        "pid": "<ID of the user>"
      },
      "purchaser": {
        "emailId": "purchase@csp.com ",
        "objectId": "<guid>",
        "tenantId": "<guid>",
        "pid": "<ID of the user>"
      },
      "term": {
        "startDate": "2019-05-31",
        "endDate": "2020-04-30",
        "termUnit": "P1Y"
      },
      "allowedCustomerOperations": ["Read"],
      "sessionMode": "None",
      "isFreeTrial": false,
      "isTest": false,
      "sandboxType": "None",
      "saasSubscriptionStatus": "Suspended"
    }
  ],
  "@nextLink": "https:// https://marketplaceapi.microsoft.com/api/saas/subscriptions/?continuationToken=%5b%7b%22token%22%3a%22%2bRID%3a%7eYeUDAIahsn22AAAAAAAAAA%3d%3d%23RT%3a1%23TRC%3a2%23ISV%3a1%23FPC%3aAgEAAAAQALEAwP8zQP9%2fFwD%2b%2f2FC%2fwc%3d%22%2c%22range%22%3a%7b%22min%22%3a%22%22%2c%22max%22%3a%2205C1C9CD673398%22%7d%7d%5d&api-version=2018-08-31" // url that contains continuation token to retrieve next page of the SaaS subscriptions list, if empty or absent, this is the last page. ISV can use this url as is to retrieve the next page or extract the value of continuation token from this url.
}
```

Wenn keine erworbenen SaaS-Abonnements für diesen Herausgeber gefunden werden, wird ein leerer Antworttext zurückgegeben.

Code: 403 – Unzulässig. Das Autorisierungstoken ist nicht verfügbar, ungültig oder abgelaufen.

Dieser Fehler ist häufig ein Symptom dafür, dass die [SaaS-Registrierung](pc-saas-registration.md) nicht ordnungsgemäß ausgeführt wird. 

Code: 500 – Interner Serverfehler. Wiederholen Sie den API-Aufruf.  Wenn der Fehler weiterhin auftritt, wenden Sie sich an den [Microsoft-Support](https://partner.microsoft.com/support/v2/?stage=1).

#### <a name="get-subscription"></a>Abrufen eines Abonnements

Ruft ein angegebenes erworbenes SaaS-Abonnement für ein SaaS-Angebot ab, das vom Herausgeber im Marketplace veröffentlicht wurde. Verwenden Sie diesen Aufruf, um alle verfügbaren Informationen für ein bestimmtes SaaS-Abonnement anhand seiner ID abzurufen, anstatt die API zum Abrufen der Liste aller Abonnements aufzurufen.

##### <a name="get-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidapi-versionapiversion"></a>Get `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`

*Abfrageparameter:*

|  Parameter         | Wert             |
|  ---------------   |  ---------------  |
| `ApiVersion`        |   Verwenden Sie 2018-08-31. |
| `subscriptionId`     |  Ein eindeutiger Bezeichner des erworbenen SaaS-Abonnements.  Diese ID wird abgerufen, nachdem das Marketplace-Autorisierungstoken mithilfe der Auflösungs-API aufgelöst wurde. |

*Anforderungsheader:*

|  Parameter         | Wert             |
|  ---------------   |  ---------------  |
|  `content-type`      |  `application/json`  |
|  `x-ms-requestid`    |  Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
|  `x-ms-correlationid` |  Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client.  Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen.  Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
|  `authorization`     | Ein eindeutiges Zugriffstoken, das den Herausgeber identifiziert, der diesen API-Aufruf sendet. Das Format ist `"Bearer <access_token>"`, wenn der Tokenwert vom Herausgeber abgerufen wird, wie es unter [Abrufen eines Tokens basierend auf der Azure AD-App](./pc-saas-registration.md#get-the-token-with-an-http-post) beschrieben ist.  |

*Antwortcodes:*

Code: 200 – Gibt basierend auf der bereitgestellten `subscriptionId` Details für ein SaaS-Abonnement zurück.

*Beispiel für einen Antworttext:*

```json
{
  "id": "<guid>", // purchased SaaS subscription ID
  "name": "Contoso Cloud Solution", // SaaS subscription name
  "publisherId": "contoso", // publisher ID
  "offerId": "offer1", // purchased offer ID
  "planId": "silver", // purchased plan ID
  "quantity": "10", // purchased amount of seats, will be empty if plan is not per seat
  "beneficiary": { // email address, user ID and tenant ID for which SaaS subscription is purchased.
    "emailId": "test@contoso.com",
    "objectId": "<guid>",
    "tenantId": "<guid>",
    "pid": "<ID of the user>"
  },
  "purchaser": { // email address ,user ID and tenant ID that purchased the SaaS subscription.  These could be different from beneficiary information for reseller (CSP) scenario
    "emailId": "test@test.com",
    "objectId": "<guid>",
    "tenantId": "<guid>",
    "pid": "<ID of the user>"
  },
  "allowedCustomerOperations": ["Read", "Update", "Delete"], // Indicates operations allowed on the SaaS subscription for beneficiary.  For CSP-initiated purchases, this will always be "Read" because the customer cannot update or delete subscription in this flow.  Purchaser can perform all operations on the subscription.
  "sessionMode": "None", // not relevant
  "isFreeTrial": false, // true - the customer subscription is currently in free trial, false - the customer subscription is not currently in free trial. Optional field – if not returned the value is false.
  "isTest": false, // not relevant
  "sandboxType": "None", // not relevant
  "saasSubscriptionStatus": " Subscribed ", // Indicates the status of the operation: PendingFulfillmentStart, Subscribed, Suspended or Unsubscribed.
  "term": { // the period for which the subscription was purchased
    "startDate": "2019-05-31", //format: YYYY-MM-DD. This is the date when the subscription was activated by the ISV and the billing started. This field is relevant only for Active and Suspended subscriptions.
    "endDate": "2019-06-29", // This is the last day the subscription is valid. Unless stated otherwise, the automatic renew will happen the next day. This field is relevant only for Active and Suspended subscriptions.
    "termUnit": "P1M" //where P1M is monthly and P1Y is yearly. Also reflected in the startDate and endDate values.
  }
}
```

Code: 403 – Unzulässig. Das Autorisierungstoken ist ungültig, abgelaufen oder wurde nicht angegeben. Die Anforderung versucht, auf ein SaaS-Abonnement für ein Angebot zuzugreifen, das mit einer anderen Azure AD-App-ID veröffentlicht ist als der, die zum Erstellen des Autorisierungstokens verwendet wurde.

Dieser Fehler ist häufig ein Symptom dafür, dass die [SaaS-Registrierung](pc-saas-registration.md) nicht ordnungsgemäß ausgeführt wird. 

Code: 404 – Nicht gefunden.  Das SaaS-Abonnement mit der angegebenen `subscriptionId` wurde nicht gefunden.

Code: 500 – Interner Serverfehler.  Wiederholen Sie den API-Aufruf.  Wenn der Fehler weiterhin auftritt, wenden Sie sich an den [Microsoft-Support](https://partner.microsoft.com/support/v2/?stage=1).

#### <a name="list-available-plans"></a>Auflisten verfügbarer Pläne

Ruft alle Pläne für ein SaaS-Angebot ab, das durch die `subscriptionId` eines bestimmten Erwerbs dieses Angebots identifiziert wird.  Verwenden Sie diesen Aufruf, um eine Liste aller privaten und öffentlichen Pläne abzurufen, die der Begünstigte eines SaaS-Abonnements für das Abonnement aktualisieren kann.  Die zurückgegebenen Pläne sind in derselben Region wie der bereits erworbene Plan verfügbar.

Dieser Aufruf gibt eine Liste der Pläne zurück, die für diesen Kunden zusätzlich zu dem bereits erworbenen Plan verfügbar sind.  Die Liste kann einem Endkunden auf der Website des Herausgebers angezeigt werden.  Ein Endkunde kann den Abonnementplan in einen beliebigen Plan in der zurückgegebenen Liste ändern.  Beim Ändern des Plans in einen nicht auf der Liste aufgeführten Plan tritt ein Fehler auf.

##### <a name="get-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidlistavailableplansapi-versionapiversion"></a>Get `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/listAvailablePlans?api-version=<ApiVersion>`

*Abfrageparameter:*

|  Parameter         | Wert             |
|  ---------------   |  ---------------  |
|  `ApiVersion`        |  Verwenden Sie 2018-08-31.  |
|  `subscriptionId`    |  Ein eindeutiger Bezeichner des erworbenen SaaS-Abonnements.  Diese ID wird abgerufen, nachdem das Marketplace-Autorisierungstoken mithilfe der Auflösungs-API aufgelöst wurde. |

*Anforderungsheader:*

|  Parameter         | Wert             |
|  ---------------   |  ---------------  |
|   `content-type`     |  `application/json` |
|   `x-ms-requestid`   |  Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID.  Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
|  `x-ms-correlationid`  |  Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client.  Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen.  Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
|  `authorization`     |  Ein eindeutiges Zugriffstoken, das den Herausgeber identifiziert, der diesen API-Aufruf sendet.  Das Format ist `"Bearer <access_token>"`, wenn der Tokenwert vom Herausgeber abgerufen wird, wie es unter [Abrufen eines Tokens basierend auf der Azure AD-App](./pc-saas-registration.md#get-the-token-with-an-http-post) beschrieben ist.  |

*Antwortcodes:*

Code: 200 – Gibt eine Liste aller verfügbaren Pläne für ein vorhandenes SaaS-Abonnement einschließlich des bereits erworbenen Plans zurück.

Beispiel für einen Antworttext:

```json
{
  "plans": [
    {
      "planId": "Platinum001",
      "displayName": "Private platinum plan for Contoso", // display name of the plan as it appears in the marketplace
      "isPrivate": true //true or false
    },
    {
      "planId": "gold",
      "displayName": "Gold plan for Contoso",
      "isPrivate": false //true or false
    }
  ]
}
```

Wenn `subscriptionId` nicht gefunden wird, wird ein leerer Antworttext zurückgegeben.

Code: 403 – Unzulässig. Das Autorisierungstoken ist ungültig, abgelaufen oder wurde nicht angegeben.  Die Anforderung versucht möglicherweise, auf ein SaaS-Abonnement für ein Angebot zuzugreifen, das mit einer anderen Azure AD-App-ID veröffentlicht ist als der, die zum Erstellen des Autorisierungstokens verwendet wurde.

Dieser Fehler ist häufig ein Symptom dafür, dass die [SaaS-Registrierung](pc-saas-registration.md) nicht ordnungsgemäß ausgeführt wird. 

Code: 500 – Interner Serverfehler.  Wiederholen Sie den API-Aufruf.  Wenn der Fehler weiterhin auftritt, wenden Sie sich an den [Microsoft-Support](https://partner.microsoft.com/support/v2/?stage=1).

#### <a name="change-the-plan-on-the-subscription"></a>Ändern des Plans für das Abonnement

Aktualisiert den vorhandenen Plan, der für ein SaaS-Abonnement erworben wurde, auf einen neuen Plan (öffentlich oder privat).  Der Herausgeber muss diese API aufrufen, wenn ein Plan auf Herausgeberseite für ein im Marketplace erworbenes SaaS-Abonnement geändert wird.

Diese API kann nur für aktive Abonnements aufgerufen werden.  Jeder Plan kann in einen anderen vorhandenen Plan (öffentlich oder privat), aber nicht in sich selbst geändert werden.  Bei privaten Plänen muss der Mandant des Kunden im Partner Center als Teil der Zielgruppe für den Plan definiert werden.

##### <a name="patch-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidapi-versionapiversion"></a>Patch `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`

*Abfrageparameter:*

|  Parameter         | Wert             |
|  ---------------   |  ---------------  |
|  `ApiVersion`        |  Verwenden Sie 2018-08-31.  |
| `subscriptionId`     | Ein eindeutiger Bezeichner des erworbenen SaaS-Abonnements.  Diese ID wird abgerufen, nachdem das Marketplace-Autorisierungstoken mithilfe der Auflösungs-API aufgelöst wurde. |

*Anforderungsheader:*
 
|  Parameter         | Wert             |
|  ---------------   |  ---------------  |
|  `content-type`      | `application/json`  |
|  `x-ms-requestid`    | Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
|  `x-ms-correlationid`  | Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client.  Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen.  Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
|  `authorization`     |  Ein eindeutiges Zugriffstoken, das den Herausgeber identifiziert, der diesen API-Aufruf sendet.  Das Format ist `"Bearer <access_token>"`, wenn der Tokenwert vom Herausgeber abgerufen wird, wie es unter [Abrufen eines Tokens basierend auf der Azure AD-App](./pc-saas-registration.md#get-the-token-with-an-http-post) beschrieben ist. |

*Beispiel einer Anforderungsnutzlast:*

```json
{
  "planId": "gold" // the ID of the new plan to be purchased
}
```

*Antwortcodes:*

Code: 202 – Die Anforderung zum Ändern des Plans wurde akzeptiert und asynchron verarbeitet.  Vom Partner wird erwartet, dass er die **Standort-URL für den Vorgang** abfragt, um zu ermitteln, ob die Änderungsanforderung für den Plan erfolgreich war oder fehlgeschlagen ist.  Die Abfrage sollte alle paar Sekunden durchgeführt werden, bis der endgültige Status „Fehler“, „Erfolgreich“ oder „Konflikt“ für den Vorgang empfangen wird.  Der endgültige Vorgangsstatus sollte schnell zurückgegeben werden, doch kann dies in einigen Fällen mehrere Minuten dauern.

Der Partner erhält auch eine Webhookbenachrichtigung, wenn die Aktion auf Marketplace-Seite erfolgreich abgeschlossen werden kann.  Erst dann sollte der Herausgeber die Planänderung auf Herausgeberseite vornehmen.

*Antwortheader:*

|  Parameter         | Wert             |
|  ---------------   |  ---------------  |
|  `Operation-Location`        |  URL zum Abrufen des Vorgangsstatus.  Beispiel: `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations/<operationId>?api-version=2018-08-31`. |

Code: 400 – Ungültige Anforderung: Fehler bei der Überprüfung.

* Der neue Plan ist nicht vorhanden oder für dieses bestimmte SaaS-Abonnement nicht verfügbar.
* Es wird versucht, eine Änderung in den gleichen Plan durchzuführen.
* Der Status des SaaS-Abonnements ist nicht „Abonniert“.
* Der Aktualisierungsvorgang für ein SaaS-Abonnement ist nicht in `allowedCustomerOperations` enthalten.

Code: 403 – Unzulässig. Das Autorisierungstoken ist ungültig, abgelaufen oder wurde nicht angegeben.  Die Anforderung versucht, auf ein SaaS-Abonnement für ein Angebot zuzugreifen, das mit einer anderen Azure AD-App-ID veröffentlicht ist als der, die zum Erstellen des Autorisierungstokens verwendet wurde.

Dieser Fehler ist häufig ein Symptom dafür, dass die [SaaS-Registrierung](pc-saas-registration.md) nicht ordnungsgemäß ausgeführt wird.

Code: 404 – Nicht gefunden.  Das SaaS-Abonnement mit `subscriptionId` wurde nicht gefunden.

Code: 500 – Interner Serverfehler.  Wiederholen Sie den API-Aufruf.  Wenn der Fehler weiterhin auftritt, wenden Sie sich an den [Microsoft-Support](https://partner.microsoft.com/support/v2/?stage=1).

>[!NOTE]
>Es kann jeweils nur der Plan oder die Menge von Arbeitsplätzen geändert werden, nicht beides gleichzeitig.

>[!Note]
>Diese API kann nur aufgerufen werden, nachdem eine explizite Genehmigung vom Endkunden für die Änderung empfangen wurde.

#### <a name="change-the-quantity-of-seats-on-the-saas-subscription"></a>Ändern der Menge von Arbeitsplätzen für das SaaS-Abonnement

Aktualisiert (erhöht oder verringert) die Menge von Arbeitsplätzen, die für ein SaaS-Abonnement erworben wurden.  Der Herausgeber muss diese API aufrufen, wenn die Menge der Arbeitsplätze für ein im Marketplace erstelltes SaaS-Abonnement über die Herausgeberseite geändert wird.

Die Menge der Arbeitsplätze darf den im aktuellen Plan zulässigen Wert nicht übersteigen.  In diesem Fall sollte zuerst der Plan und dann die Menge geändert werden.

##### <a name="patchhttpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidapi-versionapiversion"></a>Patch`https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`

*Abfrageparameter:*

|  Parameter         | Wert             |
|  ---------------   |  ---------------  |
|  `ApiVersion`        |  Verwenden Sie 2018-08-31.  |
|  `subscriptionId`     | Ein eindeutiger Bezeichner des erworbenen SaaS-Abonnements.  Diese ID wird abgerufen, nachdem das Marketplace-Autorisierungstoken mithilfe der Auflösungs-API aufgelöst wurde.  |

*Anforderungsheader:*
 
|  Parameter         | Wert             |
|  ---------------   |  ---------------  |
|  `content-type`      | `application/json`  |
|  `x-ms-requestid`    | Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID.  Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
|  `x-ms-correlationid`  | Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client.  Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen.  Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
|  `authorization`     | Ein eindeutiges Zugriffstoken, das den Herausgeber identifiziert, der diesen API-Aufruf sendet.  Das Format ist `"Bearer <access_token>"`, wenn der Tokenwert vom Herausgeber abgerufen wird, wie es unter [Abrufen eines Tokens basierend auf der Azure AD-App](./pc-saas-registration.md#get-the-token-with-an-http-post) beschrieben ist.  |

*Beispiel einer Anforderungsnutzlast:*

```json
{
  "quantity": 5 // the new amount of seats to be purchased
}
```

*Antwortcodes:*

Code: 202 – Die Anforderung zum Ändern der Menge wurde akzeptiert und asynchron verarbeitet. Vom Partner wird erwartet, dass er die **Standort-URL für den Vorgang** abfragt, um zu ermitteln, ob die Änderungsanforderung für die Menge erfolgreich war oder fehlgeschlagen ist.  Die Abfrage sollte alle paar Sekunden durchgeführt werden, bis der endgültige Status „Fehler“, „Erfolgreich“ oder „Konflikt“ für den Vorgang empfangen wird.  Der endgültige Vorgangsstatus sollte schnell zurückgegeben werden, doch kann dies in einigen Fällen mehrere Minuten dauern.

Der Partner erhält auch eine Webhookbenachrichtigung, wenn die Aktion auf Marketplace-Seite erfolgreich abgeschlossen werden kann.  Erst dann sollte der Herausgeber die Änderung der Menge auf Herausgeberseite vornehmen.

*Antwortheader:*

|  Parameter         | Wert             |
|  ---------------   |  ---------------  |
|  `Operation-Location`        |  Link zu einer Ressource, um den Vorgangsstatus abzurufen.  Beispiel: `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations/<operationId>?api-version=2018-08-31`.  |

Code: 400 – Ungültige Anforderung: Fehler bei der Überprüfung.

* Die neue Menge ist größer oder kleiner als das Limit für den aktuellen Plan.
* Die neue Menge fehlt.
* Es wird versucht, eine Änderung in die gleiche Menge durchzuführen.
* Der Status des SaaS-Abonnements ist nicht „Abonniert“.
* Der Aktualisierungsvorgang für ein SaaS-Abonnement ist nicht in `allowedCustomerOperations` enthalten.

Code: 403 – Unzulässig.  Das Autorisierungstoken ist ungültig, abgelaufen oder wurde nicht angegeben.  Die Anforderung versucht, auf ein Abonnement zuzugreifen, das nicht dem aktuellen Herausgeber gehört.

Dieser Fehler ist häufig ein Symptom dafür, dass die [SaaS-Registrierung](pc-saas-registration.md) nicht ordnungsgemäß ausgeführt wird. 

Code: 404 – Nicht gefunden.  Das SaaS-Abonnement mit `subscriptionId` wurde nicht gefunden.

Code: 500 – Interner Serverfehler.  Wiederholen Sie den API-Aufruf.  Wenn der Fehler weiterhin auftritt, wenden Sie sich an den [Microsoft-Support](https://partner.microsoft.com/support/v2/?stage=1).

>[!Note]
>Es kann jeweils nur ein Plan oder eine Menge geändert werden, nicht beides gleichzeitig.

>[!Note]
>Diese API kann nur aufgerufen werden, nachdem eine explizite Genehmigung vom Endkunden für die Änderung empfangen wurde.

#### <a name="cancel-a-subscription"></a>Kündigen eines Abonnements

Kündigt ein angegebenes SaaS-Abonnement.  Der Herausgeber muss diese API nicht verwenden, und es wird empfohlen, die Kunden zum Kündigen von SaaS-Abonnements an den Marketplace weiterzuleiten.

Wenn der Herausgeber beschließt, die Kündigung von im Marketplace erworbenen SaaS-Abonnements auf seiner Website zu implementieren, muss er diese API aufrufen.  Nach Abschluss dieses Aufrufs lautet der Status des Abonnements auf Microsoft-Seite *Gekündigt*.

Wenn ein Abonnement innerhalb der folgenden Karenzzeiten gekündigt wird, wird es dem Kunden nicht in Rechnung gestellt:

* Bei einem Monatsabonnement 24 Stunden nach Aktivierung
* Bei einem Jahresabonnement 14 Tage nach Aktivierung

Bei Kündigung eines Abonnements nach Ablauf der oben genannten Karenzzeiten wird das Abonnement dem Kunden in Rechnung gestellt.  Nach erfolgreicher Kündigung hat der Kunde sofort keinen Zugriff mehr auf das SaaS-Abonnement auf Microsoft-Seite.

##### <a name="deletehttpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidapi-versionapiversion"></a>Delete`https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`

*Abfrageparameter:*

|  Parameter         | Wert             |
|  ---------------   |  ---------------  |
|  `ApiVersion`        |  Verwenden Sie 2018-08-31.  |
|  `subscriptionId`     | Ein eindeutiger Bezeichner des erworbenen SaaS-Abonnements.  Diese ID wird abgerufen, nachdem das Marketplace-Autorisierungstoken mithilfe der Auflösungs-API aufgelöst wurde.  |

*Anforderungsheader:*
 
|  Parameter         | Wert             |
|  ---------------   |  ---------------  |
|  `content-type`      | `application/json`  |
|  `x-ms-requestid`    | Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID.  Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
|  `x-ms-correlationid`  | Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client.  Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen.  Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
|  `authorization`     |  Ein eindeutiges Zugriffstoken, das den Herausgeber identifiziert, der diesen API-Aufruf sendet.  Das Format ist `"Bearer <access_token>"`, wenn der Tokenwert vom Herausgeber abgerufen wird, wie es unter [Abrufen eines Tokens basierend auf der Azure AD-App](./pc-saas-registration.md#get-the-token-with-an-http-post) beschrieben ist. |

*Antwortcodes:*

Code: 202 – Die Anforderung zum Kündigen wurde akzeptiert und asynchron verarbeitet.  Vom Partner wird erwartet, dass er die **Standort-URL für den Vorgang** abfragt, um zu ermitteln, ob diese Anforderung erfolgreich war oder fehlgeschlagen ist.  Die Abfrage sollte alle paar Sekunden durchgeführt werden, bis der endgültige Status „Fehler“, „Erfolgreich“ oder „Konflikt“ für den Vorgang empfangen wird.  Der endgültige Vorgangsstatus sollte schnell zurückgegeben werden, doch kann dies in einigen Fällen mehrere Minuten dauern.

Der Partner erhält auch eine Webhookbenachrichtigung, wenn die Aktion auf Marketplace-Seite erfolgreich abgeschlossen wurde.  Erst dann sollte der Herausgeber das Abonnement auf Herausgeberseite kündigen.

*Antwortheader:*

|  Parameter         | Wert             |
|  ---------------   |  ---------------  |
|  `Operation-Location`        |  Link zu einer Ressource, um den Vorgangsstatus abzurufen.  Beispiel: `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations/<operationId>?api-version=2018-08-31`. |

Code: 400 – Ungültige Anforderung.  „Delete“ ist nicht in der Liste `allowedCustomerOperations` für dieses SaaS-Abonnement enthalten.

Code: 403 – Unzulässig.  Das Autorisierungstoken ist ungültig, abgelaufen oder nicht verfügbar. Die Anforderung versucht, auf ein SaaS-Abonnement für ein Angebot zuzugreifen, das mit einer anderen Azure AD-App-ID veröffentlicht ist als der, die zum Erstellen des Autorisierungstokens verwendet wurde.

Dieser Fehler ist häufig ein Symptom dafür, dass die [SaaS-Registrierung](pc-saas-registration.md) nicht ordnungsgemäß ausgeführt wird.

Code: 404 – Nicht gefunden.  Das SaaS-Abonnement mit `subscriptionId` wurde nicht gefunden.

Code: 500 – Interner Serverfehler. Wiederholen Sie den API-Aufruf.  Wenn der Fehler weiterhin auftritt, wenden Sie sich an den [Microsoft-Support](https://partner.microsoft.com/support/v2/?stage=1).

### <a name="operations-apis"></a>Vorgangs-APIs

#### <a name="list-outstanding-operations"></a>Auflisten ausstehender Vorgänge 

Ruft die Liste der ausstehenden Vorgänge für das angegebene SaaS-Abonnement ab.  Zurückgegebene Vorgänge sollten vom Herausgeber bestätigt werden, indem er die [API zum Patchen von Vorgängen](#update-the-status-of-an-operation) aufruft.

Zurzeit werden nur **Reaktivierungsvorgänge** als Antwort für diesen API-Aufruf zurückgegeben.

##### <a name="get-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidoperationsapi-versionapiversion"></a>Get `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations?api-version=<ApiVersion>`

*Abfrageparameter:*

|  Parameter         | Wert             |
|  ---------------   |  ---------------  |
|    `ApiVersion`    |  Verwenden Sie 2018-08-31.         |
|    `subscriptionId` | Ein eindeutiger Bezeichner des erworbenen SaaS-Abonnements.  Diese ID wird abgerufen, nachdem das Marketplace-Autorisierungstoken mithilfe der Auflösungs-API aufgelöst wurde.  |

*Anforderungsheader:*
 
|  Parameter         | Wert             |
|  ---------------   |  ---------------  |
|  `content-type`     |  `application/json` |
|  `x-ms-requestid`    |  Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID.  Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
|  `x-ms-correlationid` |  Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client.  Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen.  Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
|  `authorization`     |  Das Format ist `"Bearer <access_token>"`, wenn der Tokenwert vom Herausgeber abgerufen wird, wie es unter [Abrufen eines Tokens basierend auf der Azure AD-App](./pc-saas-registration.md#get-the-token-with-an-http-post) beschrieben ist.  |

*Antwortcodes:*

Code: 200 – Gibt einen ausstehenden Reaktivierungsvorgang für das angegebene SaaS-Abonnement zurück.

*Beispiel einer Antwortnutzlast:*

```json
{
  "operations": [
    {
      "id": "<guid>", //Operation ID, should be provided in the operations patch API call
      "activityId": "<guid>", //not relevant
      "subscriptionId": "<guid>", // subscriptionId of the SaaS subscription that is being reinstated
      "offerId": "offer1", // purchased offer ID
      "publisherId": "contoso",
      "planId": "silver", // purchased plan ID
      "quantity": "20", // purchased amount of seats, will be empty is not relevant
      "action": "Reinstate",
      "timeStamp": "2018-12-01T00:00:00", // UTC
      "status": "InProgress" // the only status that can be returned in this case
    }
  ]
}
```

Gibt eine leere JSON-Datei zurück, wenn keine Reaktivierungsvorgänge ausstehen.

Code: 400 – Ungültige Anforderung: Fehler bei der Überprüfung.

Code: 403 – Unzulässig. Das Autorisierungstoken ist ungültig, abgelaufen oder wurde nicht angegeben.  Die Anforderung versucht, auf ein SaaS-Abonnement für ein Angebot zuzugreifen, das mit einer anderen Azure AD-App-ID veröffentlicht ist als der, die zum Erstellen des Autorisierungstokens verwendet wurde.

Dieser Fehler ist häufig ein Symptom dafür, dass die [SaaS-Registrierung](pc-saas-registration.md) nicht ordnungsgemäß ausgeführt wird. 

Code: 404 – Nicht gefunden.  Das SaaS-Abonnement mit `subscriptionId` wurde nicht gefunden.

Code: 500 – Interner Serverfehler. Wiederholen Sie den API-Aufruf.  Wenn der Fehler weiterhin auftritt, wenden Sie sich an den [Microsoft-Support](https://partner.microsoft.com/support/v2/?stage=1).

#### <a name="get-operation-status"></a>Abrufen des Vorgangsstatus

Ermöglicht dem Herausgeber das Nachverfolgen des Status des angegebenen asynchronen Vorgangs:  **Unsubscribe** (Kündigen), **ChangePlan** (Plan ändern) oder **ChangeQuantity** (Menge ändern).

Die `operationId` für diesen API-Aufruf kann von dem Wert abgerufen werden, der von **Operation-Location** zurückgegeben wird, dem API-Aufruf zum Abrufen ausstehender Vorgänge oder dem `<id>`-Parameterwert, der in einem Webhookaufruf empfangen wird.

##### <a name="get-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidoperationsoperationidapi-versionapiversion"></a>Get `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations/<operationId>?api-version=<ApiVersion>`

*Abfrageparameter:*

|  Parameter         | Wert             |
|  ---------------   |  ---------------  |
|  `ApiVersion`        |  Verwenden Sie 2018-08-31.  |
|  `subscriptionId`    |  Ein eindeutiger Bezeichner des erworbenen SaaS-Abonnements.  Diese ID wird abgerufen, nachdem das Marketplace-Autorisierungstoken mithilfe der Auflösungs-API aufgelöst wurde. |
|  `operationId`       |  Der eindeutige Bezeichner des Vorgangs, der gerade abgerufen wird. |

*Anforderungsheader:*

|  Parameter         | Wert             |
|  ---------------   |  ---------------  |
|  `content-type`      |  `application/json`   |
|  `x-ms-requestid`    |  Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID.  Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
|  `x-ms-correlationid` |  Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client.  Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen.  Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
|  `authorization`     |  Ein eindeutiges Zugriffstoken, das den Herausgeber identifiziert, der diesen API-Aufruf sendet.  Das Format ist `"Bearer <access_token>"`, wenn der Tokenwert vom Herausgeber abgerufen wird, wie es unter [Abrufen eines Tokens basierend auf der Azure AD-App](./pc-saas-registration.md#get-the-token-with-an-http-post) beschrieben ist.  |

*Antwortcodes:*

Code: 200 – Ruft Details für den angegebenen SaaS-Vorgang ab. 

*Beispiel einer Antwortnutzlast:*

```json
Response body:
{
  "id  ": "<guid>", //Operation ID, should be provided in the patch operation API call
  "activityId": "<guid>", //not relevant
  "subscriptionId": "<guid>", // subscriptionId of the SaaS subscription for which this operation is relevant
  "offerId": "offer1", // purchased offer ID
  "publisherId": "contoso",
  "planId": "silver", // purchased plan ID
  "quantity": "20", // purchased amount of seats
  "action": "ChangePlan", // Can be ChangePlan, ChangeQuantity or Reinstate
  "timeStamp": "2018-12-01T00:00:00", // UTC
  "status": "InProgress", // Possible values: NotStarted, InProgress, Failed, Succeed, Conflict (new quantity / plan is the same as existing)
  "errorStatusCode": "",
  "errorMessage": ""
}
```

Code: 403 – Unzulässig. Das Autorisierungstoken ist ungültig, abgelaufen oder wurde nicht angegeben.  Die Anforderung versucht, auf ein SaaS-Abonnement für ein Angebot zuzugreifen, das mit einer anderen Azure AD-App-ID veröffentlicht ist als der, die zum Erstellen des Autorisierungstokens verwendet wurde.

Dieser Fehler ist häufig ein Symptom dafür, dass die [SaaS-Registrierung](pc-saas-registration.md) nicht ordnungsgemäß ausgeführt wird. 

Code: 404 – Nicht gefunden.  

* Abonnement mit `subscriptionId` wurde nicht gefunden.
* Vorgang mit `operationId` wurde nicht gefunden.

Code: 500 – Interner Serverfehler.  Wiederholen Sie den API-Aufruf.  Wenn der Fehler weiterhin auftritt, wenden Sie sich an den [Microsoft-Support](https://partner.microsoft.com/support/v2/?stage=1).

#### <a name="update-the-status-of-an-operation"></a>Aktualisieren des Status eines Vorgangs

Aktualisiert den Status eines ausstehenden Vorgangs, um anzugeben, ob der Vorgang auf Herausgeberseite erfolgreich oder fehlerhaft war.

Die `operationId` für diesen API-Aufruf kann von dem Wert abgerufen werden, der von **Operation-Location** zurückgegeben wird, dem API-Aufruf zum Abrufen ausstehender Vorgänge oder dem `<id>`-Parameterwert, der in einem Webhookaufruf empfangen wird.

##### <a name="patch-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidoperationsoperationidapi-versionapiversion"></a>Patch `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations/<operationId>?api-version=<ApiVersion>`

*Abfrageparameter:*

|  Parameter         | Wert             |
|  ---------------   |  ---------------  |
|   `ApiVersion`       |  Verwenden Sie 2018-08-31.  |
|   `subscriptionId`   |  Ein eindeutiger Bezeichner des erworbenen SaaS-Abonnements.  Diese ID wird abgerufen, nachdem das Marketplace-Autorisierungstoken mithilfe der Auflösungs-API aufgelöst wurde.  |
|   `operationId`      |  Der eindeutige Bezeichner des Vorgangs, der gerade abgeschlossen wird. |

*Anforderungsheader:*

|  Parameter         | Wert             |
|  ---------------   |  ---------------  |
|   `content-type`   | `application/json`   |
|   `x-ms-requestid`   |  Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID.  Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
|   `x-ms-correlationid` |  Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client.  Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen.  Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
|  `authorization`     |  Ein eindeutiges Zugriffstoken, das den Herausgeber identifiziert, der diesen API-Aufruf sendet.  Das Format ist `"Bearer <access_token>"`, wenn der Tokenwert vom Herausgeber abgerufen wird, wie es unter [Abrufen eines Tokens basierend auf der Azure AD-App](./pc-saas-registration.md#get-the-token-with-an-http-post) beschrieben ist. |

*Beispiel einer Anforderungsnutzlast:*

```json
{
  "status": "Success" // Allowed Values: Success/Failure. Indicates the status of the operation on ISV side.
}
```

*Antwortcodes:*

Code: 200 – Ein Aufruf, um über den Abschluss eines Vorgangs auf der Seite des Partners zu informieren.  Diese Antwort kann z. B. den Abschluss einer Änderung der Arbeitsplätze oder Pläne auf Herausgeberseite anzeigen.

Code: 403 – Unzulässig.  Das Autorisierungstoken ist nicht verfügbar, ungültig oder abgelaufen. Die Anforderung versucht möglicherweise, auf ein Abonnement zuzugreifen, das nicht dem aktuellen Herausgeber gehört.
Unzulässig.  Das Autorisierungstoken ist ungültig, abgelaufen oder wurde nicht angegeben.  Die Anforderung versucht, auf ein SaaS-Abonnement für ein Angebot zuzugreifen, das mit einer anderen Azure AD-App-ID veröffentlicht ist als der, die zum Erstellen des Autorisierungstokens verwendet wurde.

Dieser Fehler ist häufig ein Symptom dafür, dass die [SaaS-Registrierung](pc-saas-registration.md) nicht ordnungsgemäß ausgeführt wird.

Code: 404 – Nicht gefunden.

* Abonnement mit `subscriptionId` wurde nicht gefunden.
* Vorgang mit `operationId` wurde nicht gefunden.

Code: 409 – Konflikt.  Es wurde z. B. bereits eine neuere Transaktion abgeschlossen.

Code: 500 – Interner Serverfehler.  Wiederholen Sie den API-Aufruf.  Wenn der Fehler weiterhin auftritt, wenden Sie sich an den [Microsoft-Support](https://partner.microsoft.com/support/v2/?stage=1).

## <a name="implementing-a-webhook-on-the-saas-service"></a>Implementieren eines Webhooks für den SaaS-Dienst

Beim Erstellen eines transaktionsfähigen SaaS-Angebots im Partner Center stellt der Partner die URL des **Verbindungswebhooks** bereit, die als HTTP-Endpunkt verwendet werden soll.  Dieser Webhook wird von Microsoft mithilfe des HTTP-Aufrufs POST aufgerufen, um die Herausgeberseite über die folgenden Ereignissen zu informieren, die auf Microsoft-Seite auftreten:

* Wenn sich das SaaS-Abonnement im Status „Abonniert“ befindet:
    * ChangePlan (Plan ändern) 
    * ChangeQuantity (Menge ändern)
    * Angehalten
    * Abbestellen
* Wenn sich das SaaS-Abonnement im Status „Ausgesetzt“ befindet:
    * Reinstate (Reaktivieren)
    * Abbestellen

Der Herausgeber muss einen Webhook im SaaS-Dienst implementieren, um den Status des SaaS-Abonnements mit der Microsoft-Seite konsistent zu halten.  Der SaaS-Dienst muss die API zum Abrufen des Vorgangs aufrufen, um den Webhookaufruf und die Nutzlastdaten zu überprüfen und zu autorisieren, bevor Aktionen auf Grundlage der Webhookbenachrichtigung durchgeführt werden.  Der Herausgeber sollte eine HTTP-200-Antwort an Microsoft zurückgeben, sobald der Webhookaufruf verarbeitet wird.  Dieser Wert bestätigt, dass der Webhookaufruf vom Herausgeber erfolgreich empfangen wurde.

>[!Note]
>Die Webhook-URL muss rund um die Uhr aktiv sein und jederzeit neue Aufrufe von Microsoft empfangen können.  Microsoft verfügt über eine Wiederholungsrichtlinie für den Webhookaufruf (500 Wiederholungen in acht Stunden), doch wenn der Herausgeber den Aufruf nicht akzeptiert und keine Antwort zurückgibt, schlägt der Vorgang, über den der Webhook benachrichtigt wird, schließlich auf Microsoft-Seite fehl.

*Beispiele für Webhooknutzlast:*

```json
// end customer changed a quantity of purchased seats for a plan on Microsoft side
{
  "id": "<guid>", // this is the operation ID to call with get operation API
  "activityId": "<guid>", // do not use
  "subscriptionId": "guid", // The GUID identifier for the SaaS resource which status changes
  "publisherId": "contoso", // A unique string identifier for each publisher
  "offerId": "offer1", // A unique string identifier for each offer
  "planId": "silver", // the most up-to-date plan ID
  "quantity": " 25", // the most up-to-date number of seats, can be empty if not relevant
  "timeStamp": "2019-04-15T20:17:31.7350641Z", // UTC time when the webhook was called
  "action": "ChangeQuantity", // the operation the webhook notifies about
  "status": "Success" // Can be either InProgress or Success
}
```

```json
// end customer's payment instrument became valid again, after being suspended, and the SaaS subscription is being reinstated
{
  "id": "<guid>",
  "activityId": "<guid>",
  "subscriptionId": "<guid>",
  "publisherId": "contoso",
  "offerId": "offer2 ",
  "planId": "gold",
  "quantity": " 20",
  "timeStamp": "2019-04-15T20:17:31.7350641Z",
  "action": "Reinstate",
  "status": "In Progress"
}
```

## <a name="development-and-testing"></a>Entwickeln und Testen

Zu Beginn des Entwicklungsprozesses wird empfohlen, API-Pseudoantworten auf Herausgeberseite zu erstellen.  Diese Antworten können auf Beispielantworten in diesem Dokument basieren.

Wenn der Herausgeber für End-to-End-Tests bereit ist, sollten folgende Schritte ausgeführt werden:

* Veröffentlichen Sie ein SaaS-Angebot für eine begrenzte Vorschauzielgruppe, und belassen Sie es in der Vorschauphase.
* Dieses Angebot sollte einen Plan mit einem Preis von 0 (null) aufweisen, damit keine tatsächlichen Abrechnungskosten beim Testen entstehen.  Eine andere Möglichkeit besteht darin, einen anderen Preis als 0 (null) festzulegen und alle Testkäufe innerhalb von 24 Stunden zu stornieren.
* Stellen Sie sicher, dass alle Abläufe von Anfang bis Ende aufgerufen werden, wie es beim Kaufen des Angebots durch einen Kunden der Fall wäre.
* Wenn der Partner den vollständigen Kauf- und Abrechnungsablauf testen möchte, verwenden Sie hierfür ein Angebot mit einem höheren Preis als 0 (null).  Der Kauf wird abgerechnet, und eine Rechnung wird generiert.

Je nachdem, wo das Angebot veröffentlicht wird, kann ein Kaufablauf über das Azure-Portal oder Microsoft AppSource ausgelöst werden.

Die Aktionen *Plan ändern* , *Menge ändern* und *Kündigen* werden auf Herausgeberseite getestet.  Von Microsoft-Seite kann *Kündigen* sowohl über das Azure-Portal als auch das Admin Center (das Portal, in dem Microsoft AppSource-Käufe verwaltet werden) ausgelöst werden.  *Menge und Plan ändern* kann nur über das Admin Center ausgelöst werden.

## <a name="get-support"></a>Support

Supportoptionen für Herausgeber finden Sie unter [Support für das Programm „Kommerzieller Marketplace“ im Partner Center](../support.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Optionen für SaaS-Angebote im kommerziellen Marketplace finden Sie unter [APIs für getaktete Abrechnung im Marketplace](marketplace-metering-service-apis.md).

Überprüfen und verwenden Sie das [SaaS SDK](https://github.com/Azure/Microsoft-commercial-marketplace-transactable-SaaS-offer-SDK), das auf den in diesem Dokument beschriebenen APIs basiert.