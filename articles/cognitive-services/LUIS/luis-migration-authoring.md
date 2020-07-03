---
title: Migrieren zu Azure-Ressource für Erstellung 2
titleSuffix: Azure Cognitive Services
description: Informationen zum Migrieren zu einem Schlüssel einer Azure-Erstellungsressource 2.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: how-to
ms.date: 06/17/2020
ms.author: diberry
ms.openlocfilehash: 5f0778436db7bd8c3a09e3ba346d8a9a6c4af454
ms.sourcegitcommit: 55b2bbbd47809b98c50709256885998af8b7d0c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84983731"
---
# <a name="migrate-to-an-azure-resource-authoring-key"></a>Migrieren zu einem Schlüssel einer Azure-Erstellungsressource

Die Erstellungsauthentifizierung von Language Understanding (LUIS) wurde von einem E-Mail-Konto in eine Azure-Ressource geändert. Die Umstellung auf eine Azure-Ressource ist im Moment noch nicht erforderlich, wird in Zukunft aber erzwungen.


## <a name="what-is-migration"></a>Was ist Migration?

Migration ist der Prozess des Änderns der Erstellungsauthentifizierung von einem E-Mail-Konto in eine Azure-Ressource. Ihr Konto wird nach der Migration mit einem Azure-Abonnement und einer Azure-Erstellungsressource verknüpft. **Alle LUIS-Benutzer (Besitzer oder Projektmitarbeiter) müssen letztendlich migriert werden.** Eine Migration muss über das LUIS-Portal erfolgen. Wenn Sie die Erstellungsschlüssel erstellen, beispielsweise über die LUIS-Befehlszeilenschnittstelle, müssen Sie den Migrationsvorgang trotzdem im LUIS-Portal abschließen. Nach einer Migration können Sie weiterhin Mitautoren bei Ihren Anwendungen haben, doch diese werden auf der Azure-Ressourcenebene hinzugefügt anstatt auf Anwendungsebene.

> [!Note]
> Vor der Migration werden Mitautoren auf der LUIS-App-Ebene als _Projektmitarbeiter_ bezeichnet. Nach der Migration wird die Azure-Rolle _Mitwirkender_ für dieselbe Funktionalität, jedoch auf der Azure-Ressourcenebene verwendet.

## <a name="note-before-you-migrate"></a>Vor der Migration zu beachten

* Migration ist ein **unidirektionaler** Prozess. Sie können nach der Migration nicht mehr zum vorherigen Zustand zurückkehren.
* Anwendungen werden **automatisch zusammen mit Ihnen migriert**, wenn Sie der **Besitzer** der Anwendung sind.
* Der Besitzer kann keine Teilmenge zu migrierender Apps auswählen, und der Prozess kann nicht rückgängig gemacht werden.
* Anwendungen **verschwinden auf Seiten der Projektmitarbeiter**, nachdem der **Besitzer migriert** wurde.
* Besitzer werden aufgefordert, E-Mails an Projektmitarbeiter zu senden, um sie über die Migration zu informieren.
* Anwendungen werden **nicht zusammen mit Ihnen migriert**, wenn Sie ein **Projektmitarbeiter** der Anwendung sind.
* Es gibt keine Möglichkeit für einen Besitzer, herauszufinden, ob seine Projektmitarbeiter migriert wurden.
* **Eine Migration sammelt nicht automatisch** Projektmitarbeiter und verschiebt diese in die Azure-Erstellungsressource oder fügt sie dieser hinzu. Der App-Besitzer ist derjenige, der diesen Schritt nach der Migration ausführen muss. Dieser Schritt erfordert [Berechtigungen für die Azure-Erstellungsressource](https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-collaborate).
* Nachdem sie der Azure-Ressource zugewiesen wurden, müssen **Projektmitarbeiter migrieren, um auf Anwendungen zugreifen zu können**. Andernfalls haben Sie keinen Zugriff, um die Anwendungen zu erstellen.
* Ein migrierter Benutzer kann nicht als Projektmitarbeiter der Anwendung hinzugefügt werden.
* Wenn **Sie Vorhersageschlüssel besitzen, die Anwendungen zugewiesen sind, die sich im Besitz eines anderen Benutzers befinden**, **blockiert dies die Migration** für sowohl den Besitzer als auch die Projektmitarbeiter. Empfehlungen finden Sie weiter unten.

> [!Note]
> Wenn Sie eine Vorhersagelaufzeitressource erstellen müssen, gibt es [einen gesonderten Prozess](luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal), um diese zu erstellen.

## <a name="migration-prerequisites"></a>Voraussetzungen für die Migration

* Sie müssen einem gültigen Azure-Abonnement zugeordnet sein. Bitten Sie Ihren Mandantenadministrator, Sie dem Abonnement hinzuzufügen, oder registrieren Sie sich [hier](https://azure.microsoft.com/free/) für ein kostenloses.
* Sie müssen eine LUIS Azure-Erstellungsressource im LUIS-Portal oder im Azure-Portal erstellen. Das Erstellen einer Erstellungsressource im LUIS-Portal ist Bestandteil des Migrationsflusses, der im nächsten Abschnitt erläutert wird.
* Wenn Sie **Projektmitarbeiter an Anwendungen** sind, werden die Anwendungen nicht automatisch migriert. Es wird **empfohlen, diese Anwendungen zu sichern**, indem Sie sie exportieren oder die [Export-API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) verwenden. Sie können die App nach der Migration wieder zurück in LUIS importieren. Beim Importvorgang wird eine neue App mit einer neuen App-ID erstellt, deren Besitzer Sie sind.
* Wenn Sie der **Besitzer der Anwendungs** sind, müssen Sie Ihre Apps nicht exportieren, da sie automatisch migriert werden. Es wird **empfohlen, die Liste der Projektmitarbeiter jeder App zu speichern**. Eine E-Mail-Vorlage, die diese Liste enthält, wird optional als Bestandteil des Migrationsprozesses bereitgestellt.


|Portal|Zweck|
|--|--|
|[Azure](https://azure.microsoft.com/free/)|* Erstellen von Vorhersage-und Erstellungsressourcen<br>* Zuweisen von Projektmitarbeitern zu Ressourcen|
|[LUIS](https://www.luis.ai)|* Migrieren zu neuen Erstellungsressourcen<br>* Erstellen einer neuen Erstellungsressource im Migrationsfluss<br>* Zuweisen oder Aufheben der Zuweisung von Vorhersage- und Erstellungsressourcen zu Apps über die Seite **Verwalten -> Azure-Ressourcen** <br>* Verschieben von Anwendungen aus einer Erstellungsressource in eine andere  |

> [!Note]
> **Das Erstellen Ihrer LUIS-App ist kostenlos**, wie der `F0`-Tarif zeigt. Hier finden Sie [weitere Informationen zu Tarifen](luis-limits.md#key-limits).


## <a name="migration-steps"></a>Schritte bei der Migration

1. Im LUIS-Portal, an dem Sie arbeiten, können Sie den Migrationsprozess über das Symbol **Azure** auf der oberen Symbolleiste beginnen.

   > [!div class="mx-imgBorder"]
   > ![Migrationssymbol](./media/migrate-authoring-key/migration-button.png)

2. Im Popupfenster für Migrationen können Sie die Migration fortsetzen oder später vornehmen. Wählen Sie **Jetzt migrieren** aus.

   > [!div class="mx-imgBorder"]
   > ![Erstes Popupfenster im Migrationsprozess, wählen Sie „Jetzt migrieren“ aus.](./media/migrate-authoring-key/prompt-when-migrating-2.png)

3. Gibt es für eine Ihrer Apps Projektmitarbeiter, werden Sie optional aufgefordert, **diesen Mitarbeitern eine E-Mail zu senden**, in der sie über die Migration informiert werden. Dieser Schritt ist optional.

   Für jeden Projektmitarbeiter und jede App wird die Standard-E-Mail-Anwendung mit einer einfach formatierten E-Mail geöffnet. Sie können die E-Mail vor dem Senden bearbeiten. Die E-Mail-Vorlage enthält die genaue App-ID und den App-Namen.
   
   ```html
   Dear Sir/Madam,

   I will be migrating my LUIS account to Azure. Consequently, you will no longer have access to the following app:

   App Id: <app-ID-omitted>
   App name: Human Resources
      
   Thank you
   ```
   
   > [!Note]
   > Nachdem Sie Ihr Konto zu Azure migriert haben, sind Ihre Apps nicht mehr für Projektmitarbeiter verfügbar.

4. Wenn Sie ein Projektmitarbeiter an einer Anwendung sind, werden Sie optional aufgefordert, **eine Kopie der Apps zu exportieren**, indem Sie diese Option während des Migrationsflusses auswählen. Nachdem Sie die Option ausgewählt haben, finden Sie die folgende Seite, auf der Sie links auf die Schaltfläche „Herunterladen“ klicken, um die gewünschten Apps zu exportieren. Sie können diese Apps nach der Migration wieder zurück importieren, da sie nicht automatisch mit Ihnen migriert werden. Dieser Schritt ist optional.

   > [!div class="mx-imgBorder"]
   > ![Aufforderung zum Exportieren Ihrer Anwendung.](./media/migrate-authoring-key/export-app-for-collabs-2.png)

5. Sie können sich entscheiden, eine neue LUIS-Erstellungsressource zu erstellen oder zu einer vorhandenen Erstellungsressource zu migrieren, wenn Sie bereits eine in Azure erstellt haben. Wählen Sie die gewünschte Option aus, indem Sie unten die entsprechende Schaltfläche auswählen.

   > [!div class="mx-imgBorder"]
   > ![Erstellen einer Erstellungsressource](./media/migrate-authoring-key/choose-existing-authoring-resource.png)

### <a name="create-new-authoring-resource-from-luis-to-migrate"></a>Erstellen einer neuen Erstellungsressource aus LUIS für die Migration

Wenn Sie eine neue Erstellungsressource erstellen möchten, wählen Sie **Neue Erstellungsressource erstellen** aus, und geben Sie im nächsten Fenster die folgenden Informationen an.

> [!div class="mx-imgBorder"]
> ![Erstellen einer Erstellungsressource](./media/migrate-authoring-key/create-new-authoring-resource-2.png)

* **Ressourcenname**: ein von Ihnen gewählter benutzerdefinierter Name, der als Teil der URL für Ihre Abfragen für Erstellungs- und Vorhersageendpunkte verwendet wird
* **Abonnementname**: Das Abonnement, das der Ressource zugeordnet wird. Wenn Sie über mehr als ein Abonnement verfügen, das zu Ihrem Mandanten gehört, wählen Sie das gewünschte Abonnement aus der Dropdownliste aus.
* **Ressourcengruppe**: Ein benutzerdefinierter Ressourcengruppenname, den Sie aus der Dropdownliste auswählen. Mit Ressourcengruppen können Sie Azure-Ressourcen für den Zugriff und die Verwaltung gruppieren.
* **Mandant**: der Mandant, dem Ihr Azure-Abonnement zugeordnet ist Diese Einstellung wird standardmäßig auf den Mandanten festgelegt, den Sie gerade auswählen. Sie können die Mandanten wechseln, indem Sie den äußersten rechten Avatar auswählen, der Ihre Initialen enthält.

Nachdem Sie die oben stehenden Informationen eingegeben haben, wählen Sie **Fertig** aus.

Beachten Sie, dass Sie pro Abonnement pro Region über 10 kostenlose Erstellungsressourcen verfügen können. Wenn Ihr Abonnement mehr als 10 Erstellungsressourcen in derselben Region besitzt, können Sie keine neue erstellen.

* Wenn die Erstellungsressource erstellt ist, wird die Erfolgsmeldung angezeigt. Wählen Sie **Schließen** aus, um das Popupfenster zu schließen.

  > [!div class="mx-imgBorder"]
  > ![Ihre Erstellungsressource wurde erfolgreich erstellt.](./media/migrate-authoring-key/migration-success-2.png)

### <a name="use-existing-authoring-resource-to-migrate"></a>Verwenden einer vorhandenen Erstellungsressource für die Migration

Wenn Ihr Abonnement bereits einer LUIS Azure-Erstellungsressource zugeordnet ist, oder wenn Sie eine über das Azure-Portal erstellt haben und zu dieser migrieren möchten, anstatt eine neue Ressource zu erstellen, wählen Sie **Vorhandene Erstellungsressource verwenden** aus, und geben Sie im nächsten Fenster die folgenden Informationen an.

> [!div class="mx-imgBorder"]
>![Erstellen einer Erstellungsressource](./media/migrate-authoring-key/choose-existing-authoring-resource-2.png)

* **Mandant**: der Mandant, dem Ihr Azure-Abonnement zugeordnet ist Diese Einstellung wird standardmäßig auf den Mandanten festgelegt, den Sie gerade auswählen.
* **Abonnementname**: Das Abonnement, das der Ressource zugeordnet wird. Wenn Sie über mehr als ein Abonnement verfügen, das zu Ihrem Mandanten gehört, wählen Sie das gewünschte Abonnement aus der Dropdownliste aus.
* **Ressourcenname**: Wählen Sie die Erstellungsressource aus, zu der Sie migrieren möchten.

> [!Note]
> Wenn Ihre Erstellungsressource nicht in der Dropdownliste angezeigt wird, stellen Sie sicher, dass Sie sie an der **richtigen Stelle** erstellt haben, wie in dem LUIS-Portal, bei dem Sie angemeldet sind. Stellen Sie außerdem sicher, dass das, was Sie erstellt haben, tatsächlich eine **Erstellungsressource** ist und keine **Vorhersageressource**.


* Überprüfen Sie den Namen Ihrer Erstellungsressource, und klicken Sie auf die Schaltfläche **Jetzt migrieren**.

 > [!div class="mx-imgBorder"]
 > ![Erstellen einer Erstellungsressource](./media/migrate-authoring-key/choose-authoring-resource-and-migrate-2.png)

* Die Erfolgsmeldung wird angezeigt. Wählen Sie **Schließen** aus, um das Popupfenster zu schließen.

 > [!div class="mx-imgBorder"]
 > ![Ihre Erstellungsressource wurde erfolgreich erstellt.](./media/migrate-authoring-key/migration-success-2.png)

## <a name="using-apps-after-migration"></a>Verwenden von Apps nach der Migration

* Im Anschluss an den Migrationsprozess sind alle Ihre LUIS-Apps, deren Besitzer Sie sind, jetzt einer einzigen LUIS-Erstellungsressource zugewiesen.
* In der Liste **Meine Apps** werden die Apps angezeigt, die zur neuen Erstellungsressource migriert wurden.
* Bevor Sie auf Ihre Apps zugreifen, wählen Sie das Abonnement und die LUIS-Erstellungsressource aus, um die Apps anzuzeigen, die Sie erstellen können.

 > [!div class="mx-imgBorder"]
 > ![Wählen Sie das Abonnement und die LUIS-Erstellungsressource aus, um die Apps anzuzeigen, die Sie erstellen können.](./media/create-app-in-portal-select-subscription-luis-resource.png)

* Es ist nicht erforderlich, dass Sie den Schlüssel der Erstellungsressource wissen, um Ihre Apps im LUIS-Portal weiter bearbeiten zu können.
* Wenn Sie Ihre Apps programmgesteuert bearbeiten möchten, benötigen Sie die Erstellungsschlüsselwerte. Diese Werte werden im LUIS-Portal auf der Seite **Verwalten-> Azure-Ressourcen** angezeigt und sind auch im Azure-Portal auf der **Schlüssel**-Seite der Ressource verfügbar. Sie können auch weitere Erstellungsressourcen erstellen und diese auf derselben Seite zuweisen.

 > [!div class="mx-imgBorder"]
 > ![Verwalten einer Erstellungsressource.](./media/migrate-authoring-key/manage-authoring-resource-2.png)

## <a name="add-contributors-to-authoring-resources"></a>Hinzufügen von Projektmitarbeitern zu Erstellungsressourcen

[!INCLUDE [Manage contributors for the Azure authoring resource for language understanding](./includes/manage-contributors-authoring-resource.md)]

Erfahren Sie, wie Sie in Ihrer Erstellungsressource [Projektmitarbeiter hinzufügen](luis-how-to-collaborate.md). Projektmitarbeiter erhalten Zugriff auf alle Anwendungen unter dieser Ressource.

Sie können einer Erstellungsressource Mitwirkende hinzufügen, indem Sie im _Azure-Portal_ zur **Zugriffssteuerung (IAM)** -Seite für diese Ressource wechseln. Weitere Informationen finden Sie unter [Hinzufügen von Mitwirkendenzugriff](luis-migration-authoring-steps.md#after-the-migration-process-add-contributors-to-your-authoring-resource).

> [!Note]
> Wenn der Besitzer der LUIS-App migriert ist und den Projektmitarbeiter als Mitwirkenden an der Azure-Ressource hinzugefügt hat, hat der Mitwirkende bis zur eigenen Migration noch keinen Zugriff auf die App.

## <a name="luis-portal-migration-reminders"></a>Migrationserinnerungen des LUIS-Portals

Der Migrationsprozess wird im [LUIS-Portal](https://www.luis.ai) bereitgestellt.

Sie werden zu einer Migration aufgefordert, wenn Folgendes zutrifft:
* Sie haben Apps im E-Mail-Authentifizierungssystem für Erstellung.
* Und Sie sind der App-Besitzer.

Sie werden wöchentlich aufgefordert, Ihre Apps zu migrieren. Sie können dieses Fenster abbrechen, ohne eine Migration vorzunehmen. Wenn Sie eine Migration vor dem nächsten geplanten Zeitraum vornehmen möchten, können Sie den Migrationsvorgang über das Symbol **Azure** auf der oberen Symbolleiste des LUIS-Portals beginnen.

Durch Abbrechen des Fensters können Sie den Migrationsprozess hinauszögern. Sie werden regelmäßig zu einer Migration aufgefordert, bis Sie die Migration ausgeführt haben oder bis der Stichtag für die Migration überschritten ist. Sie können den Migrationsprozess über das Schlosssymbol auf der oberen Navigationsleiste starten.

## <a name="prediction-resources-blocking-migration"></a>Vorhersageressourcen, die die Migration blockieren
Die Migration wirkt sich negativ auf eine Anwendungsruntime aus. Während der Migration werden Projektmitarbeiter aus Ihren Apps entfernt, und Sie werden als Projektmitarbeiter aus anderen Apps entfernt. Dieser Vorgang bedeutet, dass die Schlüssel, die ein Projektmitarbeiter zuweist, ebenfalls entfernt werden, wodurch Ihre Anwendung möglicherweise unterbrochen wird, wenn sie sich in der Produktion befindet. Dies ist der Grund, warum wir die Migration blockieren, bis Sie Projektmitarbeiter oder Schlüssel, die ihnen zugewiesen sind, manuell entfernt haben.

### <a name="when-does-prediction-resources-block-migration"></a>Wann blockieren Vorhersageressourcen die Migration?
* Die Migration wird blockiert, wenn Sie in Apps, deren Besitzer Sie nicht sind, Vorhersage-/Laufzeitressourcen zugewiesen haben.
* Die Migration wird blockiert, wenn andere Benutzer Apps, deren Besitzer Sie sind, Vorhersage-/Laufzeitressourcen zugewiesen haben.

### <a name="recommended-steps-to-do-if-you-are-the-owner-of-the-app"></a>Empfohlene, durchzuführende Schritte, wenn Sie der Besitzer der App sind
Wenn Sie Besitzer einiger Anwendungen sind und Projektmitarbeiter haben, die dieser Anwendung Vorhersage-/Laufzeitschlüssel zugewiesen haben, wird bei der Migration eine Fehlermeldung angezeigt, in der die Anwendungs-IDs aufgelistet werden, denen Vorhersageschlüssel zugewiesen sind, deren Besitzer andere Benutzer sind.

Folgendes wird empfohlen:
* Benachrichtigen Sie Projektmitarbeiter über die Migration.
* Entfernen Sie alle Projektmitarbeiter aus den in der Fehlermeldung angezeigten Anwendungen.
* Durchlaufen Sie den Migrationsprozess, der erfolgreich absolviert werden sollte, wenn Sie Projektmitarbeiter manuell entfernen.
* Weisen Sie Projektmitarbeiter Ihrer neuen Erstellungsressource als Mitwirkende zu.
* Projektmitarbeiter müssen die Vorhersageressourcen migrieren und sie den Anwendungen wieder zuweisen.
Beachten Sie, dass dies eine vorübergehende Unterbrechung der Anwendung verursachen wird, bis die Vorhersageressourcen erneut zugewiesen sind.

Eine weitere Lösung besteht darin, dass Projektmitarbeiter vor der Migration des Besitzers App-Besitzer als Mitwirkende in ihren Azure-Abonnements über das Azure-Portal hinzufügen können. Dadurch erhält der Besitzer Zugriff auf die Laufzeitvorhersageressource. Wenn der Besitzer mithilfe des neuen Abonnements migriert, dem er hinzugefügt wurde (das sich unter einem neuen Mandanten befindet), wird hierdurch nicht nur die Blockierung des Migrationsprozesses für den Projektmitarbeiter und den App-Besitzer aufgehoben, sondern es wird auch eine reibungslose Migration von Apps ermöglicht, wobei der Vorhersageschlüssel diesen immer noch zugewiesen ist und die Apps nicht unterbricht.


### <a name="recommended-steps-to-do-if-you-are-a-collaborator-on-an-app"></a>Empfohlene, durchzuführende Schritte, wenn Sie Projektmitarbeiter an einer App sind
Wenn Sie Projektmitarbeiter bei Anwendungen sind und dieser Anwendung Vorhersage-/Laufzeitschlüssel zugewiesen haben, wird bei der Migration eine Fehlermeldung angezeigt, in der die Anwendungs-IDs und Schlüsselpfade aufgelistet werden, die die Migration blockieren.

Folgendes wird empfohlen:
* Exportieren Sie Anwendungen als Sicherung. Dieser Schritt wird optional im Migrationsprozess bereitgestellt.
* Heben Sie die Zuweisung der Vorhersageressourcen über die Seite **Verwalten -> Azure-Ressourcen** auf.
* Durchlaufen Sie den Migrationsprozess.
* Importieren Sie die Anwendungen nach der Migration wieder.
* Weisen Sie Ihren Anwendungen wieder Vorhersageschlüssel zu über die Seite **Verwalten > Azure-Ressourcen**.

> [!Note]
> Wenn Sie Ihre Anwendungen nach der Migration wieder zurück importieren, verfügen diese über andere App-IDs und unterscheiden sich von den in der Produktion. Sie sind jetzt der Besitzer dieser Anwendungen.

## <a name="troubleshooting-migration-process"></a>Problembehandlung des Migrationsprozesses

Wenn Sie eine Migration versuchen, aber Ihr Azure-Abonnement nicht in der Dropdownliste finden können:
* Stellen Sie sicher, dass Sie ein gültiges Azure-Abonnement besitzen, das autorisiert ist, Cognitive Services-Ressourcen zu erstellen. Wechseln Sie zum [Azure-Portal](https://ms.portal.azure.com), und überprüfen Sie den Status des Abonnements. Wenn Sie keins besitzen, [erstellen Sie ein kostenloses Abonnement](https://azure.microsoft.com/free/).
* Stellen Sie sicher, dass Sie sich im richtigen Mandanten befinden, der Ihrem gültigen Abonnement zugeordnet ist. Sie können die Mandanten auf der folgenden Symbolleiste über den weiter links stehenden Avatar wechseln: ![Mandanten wechseln.](./media/migrate-authoring-key/switch-user-tenant-2.png)

 Wenn Sie bereits über eine Erstellungsressource verfügen, diese jedoch nicht finden können, wenn Sie die Option „Vorhandene Erstellungsressource verwenden“ auswählen:
* Ihre Ressource wurde wahrscheinlich an einem anderen Ort als dem Portal erstellt, bei dem Sie angemeldet sind. Überprüfen Sie die [LUIS-Erstellungsregionen und die Portale](https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-authoring-regions).
* Erstellen einer neuen Ressource über das LUIS-Portal

Wenn Sie die Option „Neue Erstellungsressource erstellen“ auswählen und die Migration mit der Fehlermeldung „Fehler beim Abrufen der Azure-Informationen des Benutzers. Versuchen Sie es später noch mal.“ fehlschlägt:
* Ihr Abonnement kann pro Region pro Abonnement über 10 oder mehr Erstellungsressourcen verfügen. Sollte dies der Fall sein, können Sie keine neue Erstellungsressource erstellen.
* Migrieren Sie, indem Sie „Vorhandene Erstellungsressource verwenden“ und dann eine der vorhandenen Ressourcen auswählen, die sich unter Ihrem Abonnement befinden.

Wenn der folgende Fehler angezeigt wird, überprüfen Sie den [Abschnitt „Empfohlene, durchzuführende Schritte, wenn Sie der Besitzer der App sind“], ![Migration schlägt für Besitzer fehl](./media/migrate-authoring-key/migration-failed-for-owner-2.png).

Wenn der folgende Fehler angezeigt wird, überprüfen Sie den [Abschnitt „Empfohlene, durchzuführende Schritte, wenn Sie Projektmitarbeiter an einer App sind], ![Migration schlägt für Projektmitarbeiter fehl](./media/migrate-authoring-key/migration-failed-for-collab-2.png).


## <a name="next-steps"></a>Nächste Schritte

* Lesen Sie die [Konzepte](luis-concept-keys.md) über Erstellungs- und Laufzeitschlüssel.
* Erfahren Sie, [wie Sie Schlüssel zuweisen](luis-how-to-azure-subscription.md) und [Mitwirkende](luis-how-to-collaborate.md) hinzufügen.
