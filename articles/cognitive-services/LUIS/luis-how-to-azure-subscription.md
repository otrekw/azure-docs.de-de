---
title: 'Verwenden von Erstellungs- und Laufzeitschlüsseln: LUIS'
description: Wenn Sie Language Understanding (LUIS) zum ersten Mal verwenden, müssen Sie keinen Erstellungsschlüssel erstellen. Wenn Sie die App veröffentlichen möchten und dann Ihren Laufzeitendpunkt verwenden, müssen Sie den Laufzeitschlüssel erstellen und ihn der App zuweisen.
services: cognitive-services
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: how-to
ms.date: 09/07/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: adc0c253648ed7ae869a20b92c42e7f6478501b7
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2020
ms.locfileid: "92151587"
---
# <a name="create-luis-resources"></a>Erstellen von LUIS-Ressourcen

Erstellungs- und Abfragevorhersagelaufzeit-Ressourcen ermöglichen die Authentifizierung bei Ihrer LUIS-App und bei Ihrem Vorhersageendpunkt.

<a name="azure-resources-for-luis"></a>
<a name="programmatic-key" ></a>
<a name="endpoint-key"></a>
<a name="authoring-key"></a>

## <a name="luis-resources"></a>LUIS-Ressourcen

LUIS lässt drei Arten von Azure-Ressourcen sowie eine Azure-fremde Ressource zu:

|Resource|Zweck|Cognitive Service `kind`|Cognitive Service `type`|
|--|--|--|--|
|Erstellungsressource|Ermöglicht Ihnen das Erstellen, Verwalten, Trainieren, Testen und Veröffentlichen von Anwendungen. [Erstellen Sie eine LUIS-Erstellungsressource](luis-how-to-azure-subscription.md#create-luis-resources-in-azure-portal), wenn Sie LUIS-Apps programmgesteuert oder über das LUIS-Portal erstellen möchten. Sie müssen zunächst [Ihr LUIS-Konto migrieren](luis-migration-authoring.md#what-is-migration), damit Sie Ihre Azure-Erstellungsressourcen mit der Anwendung verknüpfen können. Sie können die Berechtigungen für die Erstellungsressource steuern, indem Sie Personen [die Rolle „Mitwirkender“](#contributions-from-other-authors) zuweisen. <br><br> Für die LUIS-Erstellungsressource ist ein Tarif verfügbar:<br> Die * **F0-Erstellungsressource „Free“** , die Ihnen monatlich 1 Mio. kostenlose Erstellungstransaktionen und 1.000 kostenlose Testanforderungen für den Vorhersageendpunkt bietet |`LUIS.Authoring`|`Cognitive Services`|
|Vorhersageressource| Nachdem Sie die LUIS-Anwendung veröffentlicht haben, verwenden Sie die Vorhersageressource/den Vorhersageschlüssel, um Anforderungen für den Vorhersageendpunkt abzufragen. Erstellen Sie eine LUIS-Vorhersageressource, bevor Ihre Client-App Vorhersagen über die 1.000 Anforderungen hinaus anfordert, die durch die Erstellungs- oder Startressource zur Verfügung gestellt werden. <br><br> Für die Vorhersageressource sind zwei Tarife verfügbar:<br> Die * **F0-Vorhersageressource „Free“** , die Ihnen monatlich 10.000 kostenlose Anforderungen für den Vorhersageendpunkt bietet<br> Die * **S0-Vorhersageressource „Standard“** , die der kostenpflichtige Tarif ist [Weitere Preisinformationen](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/)|`LUIS`|`Cognitive Services`|
|Start-/Testressource|Ermöglicht Ihnen das Erstellen, Verwalten, Trainieren, Testen und Veröffentlichen von Anwendungen. Sie wird standardmäßig erstellt, wenn Sie bei der ersten Registrierung bei LUIS als Option die Startressource auswählen. Der Startschlüssel wird jedoch letztendlich nicht mehr unterstützt, und alle LUIS-Benutzer müssen [ihre Konten migrieren](luis-migration-authoring.md#what-is-migration) und ihre LUIS-Anwendungen mit einer Erstellungsressource verknüpfen. Im Gegensatz zur Erstellungsressource erhalten Sie mit dieser Ressource keine Berechtigungen für die rollenbasierte Zugriffssteuerung in Azure. <br><br> Wie die Erstellungsressource bietet Ihnen die Startressource 1 Mio. kostenlose Erstellungstransaktionen und 1.000 kostenlose Testanforderungen für den Vorhersageendpunkt.|-|Keine Azure-Ressource|
|[Cognitive Service-Ressourcenschlüssel für mehrere Dienste](../cognitive-services-apis-create-account-cli.md?tabs=windows#create-a-cognitive-services-resource)|Anforderungen von Abfragevorhersageendpunkten, die mit LUIS und anderen unterstützten Cognitive Services geteilt werden.|`CognitiveServices`|`Cognitive Services`|


> [!Note]
> LUIS stellt zwei Typen von F0-Ressourcen (kostenloser Tarif) bereit. Eine für Erstellungstransaktionen und eine für Vorhersagetransaktionen. Wenn das Kontingent kostenloser Vorhersagetransaktionen erschöpft ist, stellen Sie sicher, dass Sie die F0-Vorhersageressource verwenden, die Ihnen monatlich 10.000 kostenlose Transaktionen bietet, und nicht die Erstellungsressource, die Ihnen monatlich 1.000 Vorhersagetransaktionen bietet.

Nach Abschluss der Azure-Ressourcenerstellung können Sie der App im LUIS-Portal die [Ressource zuweisen](#assign-a-resource-to-an-app).

Es ist wichtig, LUIS-Apps in den [Regionen](luis-reference-regions.md#publishing-regions) zu erstellen, in denen sie veröffentlicht und abgefragt werden sollen.

## <a name="resource-ownership"></a>Ressourcenbesitz

Eine Azure-Ressource (beispielsweise eine LUIS-Ressource) gehört zu dem Abonnement, das die Ressource enthält.

Der Besitz einer Ressource kann wie folgt übertragen werden:
* Übertragen des [Besitzes](../../cost-management-billing/manage/billing-subscription-transfer.md) Ihres Abonnements
* Exportieren Sie die LUIS-App als Datei, und importieren Sie sie anschließend in einem anderen Abonnement. Die Exportfunktion finden Sie im LUIS-Portal auf der Seite **Meine Apps** .


## <a name="resource-limits"></a>Ressourceneinschränkungen

### <a name="authoring-key-creation-limits"></a>Grenzwerte für die Erstellung von Erstellungsschlüsseln

Sie können bis zu 10 Erstellungsschlüssel pro Region und Abonnement erstellen.

Lesen Sie unter [Schlüsselgrenzwerte](luis-limits.md#key-limits) und [Azure-Regionen](luis-reference-regions.md) nach.

Veröffentlichungsregionen unterscheiden sich von Erstellungsregionen. Stellen Sie sicher, dass Sie eine App in der Erstellungsregion erstellen, die der Veröffentlichungsregion entspricht, in der sich Ihre Clientanwendung befinden soll.

### <a name="key-usage-limit-errors"></a>Fehler im Zusammenhang mit dem Schlüsselnutzungslimit

Nutzungslimits sind tarifabhängig.

Wenn Sie Ihr TPS-Kontingent (Transaktionen pro Sekunde) überschreiten, erhalten Sie den HTTP-Fehler 429. Wenn Sie Ihr TPM-Kontingent (Transaktionen pro Monat) überschreiten, erhalten Sie den HTTP-Fehler 403.


### <a name="reset-authoring-key"></a>Zurücksetzen des Authoringschlüssels

Apps, die [zur Erstellungsressource migriert](luis-migration-authoring.md) wurden: Wurde Ihr Erstellungsschlüssel kompromittiert, setzen Sie den Schlüssel für diese Erstellungsressource im Azure-Portal auf der Seite **Schlüssel** zurück.

Apps, die noch nicht migriert wurden: Der Schlüssel wird für Ihre Apps im LUIS-Portal zurückgesetzt. Wenn Sie Ihre Apps mit den Erstellungs-APIs schreiben, müssen Sie den Wert von „Ocp-Apim-Subscription-Key“ in den neuen Schlüssel ändern.

### <a name="regenerate-azure-key"></a>Erneutes Generieren von Azure-Schlüsseln

Generieren Sie die Azure-Schlüssel im Azure-Portal auf der Seite **Schlüssel** erneut.


<a name="securing-the-endpoint"></a>

## <a name="app-ownership-access-and-security"></a>App-Besitz, -Zugriff und -Sicherheit

Eine App ist durch ihre Azure-Ressourcen definiert, die durch das Abonnement des Besitzers bestimmt sind.

Sie können Ihre LUIS-App verschieben. Verwenden Sie die folgenden Dokumentationsressourcen im Azure-Portal oder in der Azure-Befehlszeilenschnittstelle:

* [Move app to another LUIS authoring Azure resource](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/apps-move-app-to-another-luis-authoring-azure-resource)
* [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../../azure-resource-manager/management/move-resource-group-and-subscription.md)
* [Anleitung zum Verschieben von App Service-Ressourcen](../../azure-resource-manager/management/move-limitations/app-service-move-limitations.md)


### <a name="contributions-from-other-authors"></a>Beiträge von anderen Autoren

Apps, die [zur Erstellungsressource migriert](luis-migration-authoring.md) wurden: _Mitwirkende_ werden im Azure-Portal für die Erstellungsressource über die Seite **Zugriffssteuerung (IAM)** verwaltet. Erfahren Sie, [wie Sie einen Benutzer hinzufügen](luis-how-to-collaborate.md), indem Sie die E-Mail-Adresse des Projektmitarbeiters und die Rolle _Mitwirkender_ verwenden.

Apps, die noch nicht migriert wurden: Alle _Projektmitarbeiter_ werden im LUIS-Portal über die Seite **Verwalten > Projektmitarbeiter** verwaltet.

### <a name="query-prediction-access-for-private-and-public-apps"></a>Abfragevorhersagezugriff für private und öffentliche Apps

Bei einer **privaten** App ist der Zugriff auf die Abfragevorhersagelaufzeit für Besitzer und Mitwirkende möglich. Für eine **öffentliche** App ist der Laufzeitzugriff für jede Person möglich, die über eine eigene Azure [Cognitive Services](../cognitive-services-apis-create-account.md)- oder [LUIS](#create-resources-in-the-azure-portal)-Laufzeitressource und die ID der öffentlichen App verfügt.

Zurzeit gibt es keinen Katalog mit öffentlichen Apps.

### <a name="authoring-permissions-and-access"></a>Erstellungsberechtigungen und Zugriff
Der Zugriff auf die App aus dem [LUIS](luis-reference-regions.md#luis-website)-Portal oder über die [Erstellungs-APIs](https://go.microsoft.com/fwlink/?linkid=2092087) wird von der Azure-Erstellungsressource gesteuert.

Der Besitzer und alle Mitwirkenden haben Zugriff für die Erstellung der App.

|Der Zugriff für die Erstellung umfasst|Notizen|
|--|--|
|Hinzufügen oder Entfernen von Endpunktschlüsseln||
|Exportieren von Versionen||
|Exportieren von Endpunktprotokollen||
|Importieren von Versionen||
|Öffentlichmachen von Apps|Wenn eine App öffentlich ist, kann jede Person mit einem Erstellungs- oder Endpunktschlüssel die App abfragen.|
|Ändern von Modellen|
|Veröffentlichen|
|Überprüfen von Endpunktäußerungen für das [aktive Lernen](luis-how-to-review-endpoint-utterances.md)|
|Trainieren|

<a name="prediction-endpoint-runtime-key"></a>

### <a name="prediction-endpoint-runtime-access"></a>Laufzeitzugriff auf Vorhersageendpunkt

Der Zugriff zum Abfragen des Vorhersageendpunkts wird von einer Einstellung auf der Seite **Anwendungsinformationen** im Abschnitt **Verwalten** gesteuert.

|[Privater Endpunkt](#runtime-security-for-private-apps)|[Öffentlicher Endpunkt](#runtime-security-for-public-apps)|
|:--|:--|
|Für Besitzer und Mitwirkende verfügbar|Für Besitzer, Mitwirkende und alle Personen verfügbar, denen die App-ID bekannt ist|

Sie können steuern, wer Ihren LUIS-Laufzeitschlüssel sieht, indem Sie ihn in einer Server-zu-Server-Umgebung aufrufen. Wenn Sie LUIS über einen Bot verwenden, ist die Verbindung zwischen dem Bot und LUIS bereits sicher. Wenn Sie den LUIS-Endpunkt direkt aufrufen, sollten Sie eine serverseitige API (z.B. als Azure-[Funktion](https://azure.microsoft.com/services/functions/)) mit kontrolliertem Zugang (z.B. [AAD](https://azure.microsoft.com/services/active-directory/)) erstellen. Wenn die serverseitige API aufgerufen und authentifiziert und die Autorisierung überprüft wurde, übergeben Sie den Aufruf an LUIS. Durch diese Strategie werden zwar Man-in-the-Middle-Angriffe nicht verhindert, sie verbirgt jedoch Ihre Schlüssel- und Endpunkt-URL vor Ihren Benutzern. Außerdem ermöglicht sie das Nachverfolgen des Zugriffs und das Hinzufügen einer Protokollierung von Endpunktantworten (beispielsweise [Application Insights](https://azure.microsoft.com/services/application-insights/)).

### <a name="runtime-security-for-private-apps"></a>Laufzeitsicherheit für private Apps

Die Laufzeitumgebung einer privaten App ist nur für Folgendes verfügbar:

|Schlüssel und Benutzer|Erklärung|
|--|--|
|Erstellungsschlüssel des Besitzers| Bis zu 1.000 Endpunkttreffer|
|Erstellungsschlüssel für Projektmitarbeiter/Mitwirkende| Bis zu 1.000 Endpunkttreffer|
|Jeder Schlüssel, der LUIS von einem Ersteller oder Projektmitarbeiter/Mitwirkenden zugewiesen wurde|Basierend auf dem Tarif für die Schlüsselverwendung|

### <a name="runtime-security-for-public-apps"></a>Laufzeitsicherheit für öffentliche Apps

Wenn eine App als öffentlich konfiguriert ist, können _alle_ gültigen LUIS-Erstellungsschlüssel oder LUIS-Endpunktschlüssel Ihre App abfragen, solange der Schlüssel nicht das gesamte Endpunktkontingent verbraucht hat.

Ein Benutzer, der nicht Besitzer oder Mitwirkender ist, hat nur dann Zugriff auf die Laufzeitumgebung einer öffentlichen App, wenn er die App-ID hat. LUIS verfügt nicht über einen öffentlichen _Markt_ oder eine andere Möglichkeit der Suche nach einer öffentlichen App.

Eine öffentliche App wird in allen Regionen veröffentlicht, so dass ein Benutzer mit einem regionsbasierten LUIS-Ressourcenschlüssel auf die App in jeder Region zugreifen kann, die dem Ressourcenschlüssel zugeordnet ist.


### <a name="securing-the-query-prediction-endpoint"></a>Schützen des Abfragevorhersage-Endpunkts

Sie können steuern, wer Ihren LUIS-Laufzeitschlüssel für den Vorhersageendpunkt sehen kann, indem Sie ihn in einer Server-zu-Server-Umgebung aufrufen. Wenn Sie LUIS über einen Bot verwenden, ist die Verbindung zwischen dem Bot und LUIS bereits sicher. Wenn Sie den LUIS-Endpunkt direkt aufrufen, sollten Sie eine serverseitige API (z.B. als Azure-[Funktion](https://azure.microsoft.com/services/functions/)) mit kontrolliertem Zugang (z.B. [AAD](https://azure.microsoft.com/services/active-directory/)) erstellen. Wenn die serverseitige API aufgerufen und die Authentifizierung und Autorisierung überprüft wurden, übergeben Sie den Aufruf an LUIS. Durch diese Strategie werden zwar Man-in-the-Middle-Angriffe nicht verhindert, sie verbirgt jedoch Ihren Endpunkt vor Ihren Benutzern. Außerdem ermöglicht sie das Nachverfolgen des Zugriffs und das Hinzufügen einer Protokollierung von Endpunktantworten (beispielsweise [Application Insights](https://azure.microsoft.com/services/application-insights/)).

<a name="starter-key"></a>

## <a name="sign-in-to-luis-portal-and-begin-authoring"></a>Anmelden beim LUIS-Portal und Beginnen mit der Erstellung

1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, und stimmen Sie den Nutzungsbedingungen zu.
1. Wählen Sie für Ihre LUIS-App zunächst Ihren Azure-LUIS-Erstellungsschlüssel aus.

   ![Typ der Language Understanding-Erstellungsressource auswählen](./media/luis-how-to-azure-subscription/sign-in-create-resource.png)

1. Wenn Sie die Ressourcenauswahl getroffen haben, [erstellen Sie eine neue App](luis-how-to-start-new-app.md#create-new-app-in-luis).


<a name="create-azure-resources"></a>
<a name="create-resources-in-the-azure-portal"></a>

[!INCLUDE [Create LUIS resource in Azure portal](includes/create-luis-resource.md)]

### <a name="create-resources-in-azure-cli"></a>Erstellen von Ressourcen in der Azure CLI

Verwenden Sie die [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), um die einzelnen Ressourcen individuell zu erstellen.

Ressource `kind`:

* Erstellung: `LUIS.Authoring`
* Vorhersage: `LUIS`

1. Melden Sie sich bei der Azure CLI an:

    ```azurecli
    az login
    ```

    Dadurch wird ein Browser geöffnet, in dem Sie das richtige Konto auswählen und die Authentifizierung bereitstellen können.

1. Erstellen Sie eine **LUIS-Erstellungsressource** der Art `LUIS.Authoring` mit dem Namen `my-luis-authoring-resource` in der _vorhandenen_ Ressourcengruppe mit dem Namen `my-resource-group` für die Region `westus`.

    ```azurecli
    az cognitiveservices account create -n my-luis-authoring-resource -g my-resource-group --kind LUIS.Authoring --sku F0 -l westus --yes
    ```

1. Erstellen Sie eine **LUIS-Vorhersagen-Endpunktressource** der Art `LUIS` mit dem Namen `my-luis-prediction-resource` in der _vorhandenen_ Ressourcengruppe mit dem Namen `my-resource-group` für die Region `westus`. Wenn Sie einen höheren Durchsatz als den Free-Tarif wünschen, ändern Sie `F0` in `S0`. Weitere Informationen zu [Tarifen und Durchsatz](luis-limits.md#key-limits).

    ```azurecli
    az cognitiveservices account create -n my-luis-prediction-resource -g my-resource-group --kind LUIS --sku F0 -l westus --yes
    ```

    > [!Note]
    > Diese Schlüssel werden vom LUIS-Portal **nicht** verwendet, solange sie nicht im LUIS-Portal auf den **Verwalten -> Azure-Ressourcen** verwendet werden.

<a name="assign-an-authoring-resource-in-the-luis-portal-for-all-apps"></a>

### <a name="assign-resource-in-the-luis-portal"></a>Zuweisen von Ressourcen im LUIS-Portal

Sie können eine Erstellungsressource für eine einzelne App oder für alle Apps in LUIS zuweisen. In der folgenden Vorgehensweise werden alle Apps einer einzelnen Erstellungsressource zugewiesen.

1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an.
1. Wählen Sie in der oberen Navigationsleiste ganz rechts Ihr Benutzerkonto aus, und wählen Sie dann **Einstellungen** aus.
1. Wählen Sie auf der Seite **Benutzereinstellungen** die Option **Erstellungsressource hinzufügen** aus, und wählen Sie dann eine vorhandene Erstellungsressource aus. Wählen Sie **Speichern** aus.

## <a name="assign-a-resource-to-an-app"></a>Zuweisen einer Ressource zu einer App

Beachten Sie Folgendes: Wenn Sie über kein Azure-Abonnement verfügen, können Sie keine neue Ressource zuweisen oder erstellen. Zunächst müssen Sie eine [kostenlose Azure-Testversion](https://azure.microsoft.com/en-us/free/) erstellen und dann zu LUIS zurückkehren, um über das Portal eine neue Ressource zu erstellen.

Mit dem folgenden Verfahren können Sie eine Erstellungs- oder Vorhersageressource für eine Anwendung zuweisen oder erstellen:

1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, und wählen Sie dann in der Liste **Meine Apps** eine App aus.
1. Navigieren Sie zur Seite **Verwalten > Azure-Ressourcen** .

    ![„Verwalten -> Azure-Ressourcen“ im LUIS-Portal auswählen, um der App eine Ressource zuzuweisen.](./media/luis-how-to-azure-subscription/manage-azure-resources-prediction.png)

1. Wählen Sie die Registerkarte „Vorhersageressource“ oder „Erstellungsressource“ aus, und wählen Sie dann die Schaltfläche **Vorhersageressource hinzufügen** oder **Erstellungsressource hinzufügen** aus.
1. Wählen Sie die Felder im Formular aus, um die richtige Ressource zu finden, und wählen Sie dann **Speichern** aus.
1. Wenn Sie über keine Ressource verfügen, können Sie eine erstellen, indem Sie im unteren Bereich des Fensters „Create a new LUIS resource?“ (Neue LUIS-Ressource erstellen?) auswählen.


### <a name="assign-query-prediction-runtime-resource-without-using-luis-portal"></a>Zuweisen einer Abfragevorhersagelaufzeit-Ressource ohne Verwendung des LUIS-Portals

Möglicherweise soll zu Automatisierungszwecken, etwa wegen einer CI/CD-Pipeline, die Zuweisung einer LUIS-Laufzeitressource zu einer LUIS-App automatisiert werden. Die folgenden Schritte müssen dazu ausgeführt werden:

1. Rufen Sie ein Azure Resource Manager-Token von dieser [Website](https://resources.azure.com/api/token?plaintext=true) ab. Dieses Token läuft ab, daher sollten Sie es sofort verwenden. Die Anforderung gibt ein Azure Resource Manager-Token zurück.

    ![Anfordern und Empfangen eines Azure Resource Manager-Tokens](./media/luis-manage-keys/get-arm-token.png)

1. Verwenden Sie das Token zum abonnementübergreifenden Anfordern der LUIS-Laufzeitressourcen über die [API zum Abrufen von LUIS-Azure-Konten](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5be313cec181ae720aa2b26c), auf die Ihr Benutzerkonto Zugriff hat.

    Diese POST-API erfordert folgende Einstellungen:

    |Header|Wert|
    |--|--|
    |`Authorization`|Der Wert von `Authorization` ist `Bearer {token}`. Beachten Sie, dass dem Tokenwert das Wort `Bearer` und ein Leerzeichen vorangestellt werden müssen.|
    |`Ocp-Apim-Subscription-Key`|Ihr Erstellungsschlüssel.|

    Diese API gibt ein Array von JSON-Objekten aus Ihren LUIS-Abonnements, einschließlich Abonnement-ID, Ressourcengruppe und Ressourcennamen als Kontonamen zurück. Finden Sie im Array das Element, das die LUIS-Ressource darstellt, die der LUIS-App zugewiesen werden soll.

1. Weisen Sie das Token mit der API zum [Zuweisen eines LUIS-Azure-Kontos zu einer Anwendung](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5be32228e8473de116325515) der LUIS-Ressource zu.

    Diese POST-API erfordert folgende Einstellungen:

    |type|Einstellung|Wert|
    |--|--|--|
    |Header|`Authorization`|Der Wert von `Authorization` ist `Bearer {token}`. Beachten Sie, dass dem Tokenwert das Wort `Bearer` und ein Leerzeichen vorangestellt werden müssen.|
    |Header|`Ocp-Apim-Subscription-Key`|Ihr Erstellungsschlüssel.|
    |Header|`Content-type`|`application/json`|
    |Abfragezeichenfolge|`appid`|die LUIS-App-ID
    |Body||{"AzureSubscriptionId":"ddda2925-af7f-4b05-9ba1-2155c5fe8a8e",<br>"ResourceGroup": "resourcegroup-2",<br>"AccountName": "luis-uswest-S0-2"}|

    Wenn diese API erfolgreich ist, wird der Status „201 – erstellt“ zurückgegeben.

## <a name="unassign-resource"></a>Aufheben der Zuweisung einer Ressource

1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, und wählen Sie dann in der Liste **Meine Apps** eine App aus.
1. Navigieren Sie zur Seite **Verwalten -> Azure-Ressourcen** .
1. Wählen Sie die Registerkarte „Vorhersageressourcen“ oder „Erstellungsressource“ aus, und wählen Sie dann die Schaltfläche **Ressourcenzuweisung aufheben** aus.

Wenn Sie die Zuweisung einer Ressource aufheben, wird diese nicht aus Azure gelöscht. Lediglich die Verknüpfung mit LUIS wird aufgehoben.


## <a name="delete-account"></a>Konto löschen

Informationen zu den Daten, die beim Löschen Ihres Kontos gelöscht werden, finden Sie unter [Datenspeicherung und -löschung](luis-concept-data-storage.md#accounts).

## <a name="change-pricing-tier"></a>Ändern des Tarifs

1.  Suchen Sie in [Azure](https://portal.azure.com) nach Ihrem LUIS-Abonnement. Wählen Sie das LUIS-Abonnement aus.
    ![Suchen nach Ihrem LUIS-Abonnement](./media/luis-usage-tiers/find.png)
1.  Wählen Sie **Tarif** aus, um die verfügbaren Tarife anzuzeigen.
    ![Anzeigen der Tarife](./media/luis-usage-tiers/subscription.png)
1.  Wählen Sie den Tarif aus, und wählen Sie dann **Auswählen** aus, um die Änderung zu speichern.
    ![Ändern Ihres LUIS-Tarifs](./media/luis-usage-tiers/plans.png)
1.  Wenn die Änderung des Tarifs abgeschlossen ist, wird der neue Tarif in einem Popupfenster bestätigt.
    ![Überprüfen Ihres LUIS-Tarifs](./media/luis-usage-tiers/updated.png)
1. Denken Sie daran, [diesen Endpunktschlüssel](#assign-a-resource-to-an-app) auf der Seite **Publish** (Veröffentlichen) zuzuweisen und für alle Endpunktabfragen zu verwenden.

## <a name="viewing-azure-resource-metrics"></a>Anzeigen von Azure-Ressourcenmetriken

### <a name="viewing-azure-resource-summary-usage"></a>Anzeigen der Nutzungszusammenfassung von Azure-Ressourcen
Sie können in Azure LUIS-Nutzungsinformationen anzeigen. Auf der Seite **Übersicht** werden aktuelle zusammenfassende Informationen angezeigt, einschließlich Aufrufen und Fehlern. Wenn Sie eine LUIS-Endpunktanforderung ausführen, kann es bis zu fünf Minuten dauern, bis die Nutzung auf der Seite **Übersicht** angezeigt wird.

![Anzeigen der Nutzungszusammenfassung](./media/luis-usage-tiers/overview.png)

### <a name="customizing-azure-resource-usage-charts"></a>Anpassen von Nutzungsdiagrammen für Azure-Ressourcen
Metriken erlauben eine detailliertere Darstellung der Daten.

![Standardmetriken](./media/luis-usage-tiers/metrics-default.png)

Sie können Ihre Metrikdiagramme für Zeitraum und Metriktyp konfigurieren.

![Benutzerdefinierte Metriken](./media/luis-usage-tiers/metrics-custom.png)

### <a name="total-transactions-threshold-alert"></a>Warnung für Gesamttransaktions-Schwellenwert
Wenn Sie informiert werden möchten, wenn Sie einen bestimmten Transaktionsschwellenwert erreicht haben, z.B. 10.000 Transaktionen, können Sie eine Warnung erstellen.

![Standardwarnungen](./media/luis-usage-tiers/alert-default.png)

Fügen Sie eine Metrikwarnung für die Metrik der **Summe der Aufrufe** für einen bestimmten Zeitraum hinzu. Fügen Sie die E-Mail-Adressen aller Personen hinzu, die die Warnung erhalten sollen. Fügen Sie Webhooks für alle Systeme hinzu, die die Warnung erhalten sollen. Sie können auch eine Logik-App ausführen, wenn die Warnung ausgelöst wird.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, [wie Sie Versionen verwenden](luis-how-to-manage-versions.md), um den Lebenszyklus ihrer App zu steuern.
* Migrieren Sie zu der neuen [Erstellungsressource](luis-migration-authoring.md).
