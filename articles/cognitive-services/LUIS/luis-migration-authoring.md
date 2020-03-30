---
title: Migrieren zu Azure-Ressource für Erstellung
titleSuffix: Azure Cognitive Services
description: Informationen zum Migrieren zu einem Schlüssel einer Azure-Erstellungsressource
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 02/28/2020
ms.author: diberry
ms.openlocfilehash: ec6f9592a4c149be382fab66cca27d929644d988
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78194508"
---
# <a name="migrate-to-an-azure-resource-authoring-key"></a>Migrieren zu einem Schlüssel einer Azure-Erstellungsressource

Die Erstellungsauthentifizierung von Language Understanding (LUIS) wurde von einem E-Mail-Konto in eine Azure-Ressource geändert. Die Umstellung auf eine Azure-Ressource ist im Moment noch nicht erforderlich, wird in Zukunft aber erzwungen.

## <a name="why-migrate"></a>Vorteile der Migration

Wenn Sie für die Erstellung eine Azure-Ressource verwenden, können Sie als Besitzer der Ressource den Zugriff auf die Erstellung steuern. Sie können Erstellungsressourcen erstellen und benennen, um verschiedene Autorengruppen zu verwalten.

Sie sind beispielsweise Besitzer von zwei LUIS-Apps und haben verschiedene Mitglieder, die gemeinsam an den Anwendungen arbeiten. Sie können zwei verschiedene Ressourcen zur Dokumenterstellung erstellen und jede App jeder einzelnen Ressource zuweisen. Weisen Sie dann jedes Mitglied als Mitwirkenden der richtigen Ressource zur Dokumenterstellung zu, je nachdem, an welcher App das Mitglied arbeitet. Die Autorisierung wird von der Azure-Erstellungsressource gesteuert.

> [!Note]
> Vor der Migration werden Mitautoren auf der LUIS-App-Ebene als _Projektmitarbeiter_ bezeichnet. Nach der Migration wird die Azure-Rolle _Mitwirkender_ für dieselbe Funktionalität, jedoch auf der Azure-Ressourcenebene verwendet.

## <a name="what-is-migrating"></a>Was ist hier mit Migration gemeint?

Migration umfasst:

* Alle Benutzer von LUIS, Besitzer und Mitwirkende.
* **Alle** Apps.
* Eine **unidirektionale** Migration

Der Besitzer kann keine Teilmenge zu migrierender Apps auswählen, und der Prozess kann nicht rückgängig gemacht werden.

Folgendes ist die Migration nicht:

* Ein Prozess, in dem Projektmitarbeiter gesammelt und automatisch in die Azure-Erstellungsressource verschoben oder in dieser hinzugefügt werden. Diesen Schritt müssen Sie als App-Besitzer ausführen. Dieser Schritt erfordert Berechtigungen für die entsprechende Ressource.
* Ein Prozess zum Erstellen und Zuweisen einer Vorhersagelaufzeitressource. Wenn Sie eine Vorhersagelaufzeitressource benötigen, müssen Sie den [gesonderten Prozess](luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) ausführen, der unverändert ist.

## <a name="how-are-the-apps-migrating"></a>Wie werden die Apps migriert?

Der Migrationsprozess wird im [LUIS-Portal](https://www.luis.ai) bereitgestellt.

Sie werden zu einer Migration aufgefordert, wenn Folgendes zutrifft:

* Sie haben Apps im E-Mail-Authentifizierungssystem für Erstellung.
* Und Sie sind der App-Besitzer.

Durch Abbrechen des Fensters können Sie den Migrationsprozess hinauszögern. Sie werden regelmäßig zu einer Migration aufgefordert, bis Sie die Migration ausgeführt haben oder bis der Stichtag für die Migration überschritten ist. Sie können den Migrationsprozess über das Schlosssymbol auf der oberen Navigationsleiste starten.

## <a name="migration-for-the-app-owner"></a>Migration für den App-Besitzer

### <a name="before-you-migrate"></a>Vor der Migration

* **Erforderlich**: Sie benötigen ein [Azure-Abonnement](https://azure.microsoft.com/free/). In einem Schritt des Abonnementprozesses sind Abrechnungsinformationen erforderlich. Sie können jedoch den Free-Tarif (`F0`) verwenden, wenn Sie LUIS verwenden.
* **Optional**: Sichern Sie die Apps, die im LUIS-Portal in der Liste der Apps aufgeführt sind, indem Sie jede App exportieren oder die Export-[API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) verwenden.
* **Optional**: Speichern Sie für jede App deren Liste der Projektmitarbeiter. Diese E-Mail-Liste wird als Bestandteil des Migrationsprozesses bereitgestellt.


**Das Erstellen Ihrer LUIS-App ist kostenlos**, wie der `F0`-Tarif zeigt. Hier finden Sie [weitere Informationen zu Tarifen](luis-boundaries.md#key-limits).

Wenn Sie kein Azure-Abonnement haben, [registrieren Sie sich](https://azure.microsoft.com/free/).

### <a name="migration-steps"></a>Schritte bei der Migration

Führen Sie [diese Migrationsschritte](luis-migration-authoring-steps.md) aus.

### <a name="after-you-migrate"></a>Nach der Migration

Nach dem Migrationsprozess sind Ihre sämtlichen LUIS-Apps einer einzigen LUIS-Erstellungsressource zugewiesen.

Sie können weitere Erstellungsressourcen erstellen und diese im **LUIS-Portal** über die Seite _Verwalten -> Azure-Ressourcen_ zuweisen.

Sie können einer Erstellungsressource Mitwirkende hinzufügen, indem Sie im _Azure-Portal_ zur **Zugriffssteuerung (IAM)** -Seite für diese Ressource wechseln. Weitere Informationen finden Sie unter [Hinzufügen von Mitwirkendenzugriff](luis-migration-authoring-steps.md#after-the-migration-process-add-contributors-to-your-authoring-resource).

|Portal|Zweck|
|--|--|
|[Azure](https://azure.microsoft.com/free/)|* Erstellen von Vorhersage-und Erstellungsressourcen<br>* Zuweisen von Mitwirkenden|
|[LUIS](https://www.luis.ai)|* Migrieren zu neuen Erstellungsressourcen<br>* Zuweisen oder Aufheben der Zuweisung von Vorhersage- und Erstellungsressourcen zu Apps über die Seite **Verwalten -> Azure-Ressourcen**|

## <a name="migration-for-the-app-contributor"></a>Migration für den App-Mitwirkenden

Jeder Benutzer von LUIS muss die Migration durchführen, einschließlich der Projektmitarbeiter/Mitwirkenden. Ein Projektmitarbeiter muss migrieren, um Zugriff auf die App zu erhalten.

> [!Note]
> Wenn der Besitzer der LUIS-App migriert ist und den Projektmitarbeiter als Mitwirkenden an der Azure-Ressource hinzugefügt hat, hat der Mitwirkende bis zur eigenen Migration noch keinen Zugriff auf die App.

### <a name="before-the-app-is-migrated"></a>Vor dem Migrieren der App

Sie können so vorgehen, dass Sie eine App, für die Sie ein Projektmitarbeiter sind, exportieren und dann wieder in LUIS importieren. Beim Importvorgang wird eine neue App mit einer neuen App-ID erstellt, deren Besitzer Sie sind.

### <a name="after-the-app-is-migrated"></a>Nach dem Migrieren der App

Der App-Besitzer muss [Ihre E-Mail-Adresse als Projektmitarbeiter zur Azure-Erstellungsressource hinzufügen](luis-how-to-collaborate.md#add-contributor-to-azure-authoring-resource).

Nach dem Migrationsvorgang sind alle Apps, die Sie besitzen, auf der Seite **Meine Apps** des LUIS-Portals verfügbar.

## <a name="troubleshooting-the-migration-process-for-luis-authoring"></a>Beheben von Problemen beim Migrationsprozess für die LUIS-Dokumenterstellung

* LUIS-Erstellungsschlüssel sind im LUIS-Portal erst sichtbar, nachdem der Migrationsvorgang abgeschlossen wurde. Wenn Sie die Erstellungsschlüssel erstellen, beispielsweise über die LUIS-Befehlszeilenschnittstelle, muss der Benutzer den Migrationsvorgang trotzdem im LUIS-Portal abschließen.
* Wenn ein migrierter Benutzer seiner Azure-Ressource einen nicht migrierten Benutzer als Mitwirkenden hinzufügt, hat der nicht migrierte Benutzer bis zur eigenen Migration keinen Zugriff auf die Apps.
* Wenn ein nicht migrierter Benutzer keine Apps besitzt, sondern ein Projektmitarbeiter von Apps anderer Besitzt ist und die Besitzer den Migrationsprozess durchlaufen haben, muss dieser Benutzer migrieren, um Zugriff auf die Apps zu erhalten.
* Wenn ein nicht migrierter Benutzer seiner App einen anderen migrierten Benutzer als Projektmitarbeiter hinzugefügt hat, tritt ein Fehler auf, da Sie einer App keinen migrierten Benutzer als Projektmitarbeiter hinzufügen können. Der nicht migrierte Benutzer muss dann den Migrationsprozess durchlaufen und eine Azure-Ressource erstellen und dieser Ressource den migrierten Benutzer als Mitwirkenden hinzufügen.

In den folgenden Fällen tritt während der Migration ein Fehler auf:
* Ihr Abonnement ist nicht autorisiert, Cognitive Services-Ressourcen zu erstellen.
* Die Migration wirkt sich negativ auf eine Anwendungsruntime aus. Während der Migration werden Projektmitarbeiter aus Ihren Apps entfernt, und Sie werden als Projektmitarbeiter aus anderen Apps entfernt. Durch diesen Vorgang werden die von Ihnen zugewiesenen Schlüssel ebenfalls entfernt. Die Migration wird blockiert, wenn Sie Schlüssel in anderen Apps zugewiesen haben. Entfernen Sie vor der Migration den Schlüssel, den Sie zugewiesen haben. Wenn Sie wissen, dass der von Ihnen zugewiesene Schlüssel in der Runtime nicht verwendet wird, müssen Sie ihn entfernen, damit die Migration fortgesetzt werden kann.

Greifen Sie auf die Azure-Ressourcenliste Ihrer App im folgenden URL-Format zu:

`https://www.luis.ai/applications/REPLACE-WITH-YOUR-APP-ID/versions/REPLACE-WITH-YOUR-VERSION-ID/manage/resources`

## <a name="next-steps"></a>Nächste Schritte

* [Schritte zum Migrieren zur Azure-Erstellungsressource](luis-migration-authoring-steps.md)
