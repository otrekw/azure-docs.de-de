---
title: Anmelden beim LUIS-Portal
description: Wenn Sie ein neuer Benutzer sind, der sich beim LUIS-Portal anmeldet, wird der Anmeldevorgang je nach Ihrem aktuellen Benutzerkonto leicht abweichen.
services: cognitive-services
ms.custom: ''
ms.service: cognitive-services
ms.date: 09/08/2020
ms.topic: how-to
ms.author: a-sakand
author: skandil
ms.openlocfilehash: 58212fc35df7f230682d4f80b5eadf6cd8ca0902
ms.sourcegitcommit: 3c66bfd9c36cd204c299ed43b67de0ec08a7b968
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2020
ms.locfileid: "90007942"
---
# <a name="sign-in-to-luis-portal"></a>Anmelden beim LUIS-Portal

Wenn Sie ein neuer Benutzer sind, der sich beim LUIS-Portal anmeldet, wird der Anmeldevorgang je nach Ihrem aktuellen Benutzerkonto leicht abweichen:
  * Einem Azure-Abonnement zugeordnet
  * Keinem Azure-Abonnement zugeordnet

## <a name="determine-account-type"></a>Ermitteln des Kontotyps

Wenn Sie sich zum ersten Mal beim LUIS-Portal anmelden, bestimmen Sie mithilfe der folgenden visuellen Indikatoren den Kontotyp.

### <a name="account-without-azure-subscription"></a>Konto ohne Azure-Abonnement

Bei einem Konto, das keinem Azure-Abonnement zugeordnet ist, wird in der Navigationsleiste oben rechts das Azure-Symbol angezeigt. Sobald Sie zu dem zugeordneten Kontotyp migrieren, wird das Symbol nicht mehr angezeigt.

:::image type="content" source="media/sign-in/sign-in-with-account-without-azure-subscription.png" alt-text="Teilscreenshot der LUIS-Navigationsleiste mit Azure-Symbol.":::

### <a name="account-with-azure-subscription"></a>Konto mit Azure-Abonnement

Mit einem Konto, das einem Azure-Abonnement zugeordnet ist, können Sie Ihr Abonnement und die zu verwendende Azure-Ressource auswählen.

:::image type="content" source="media/sign-in/resource-selection.png" alt-text="Teilscreenshot der LUIS-Navigationsleiste mit Azure-Symbol.":::

## <a name="sign-in-with-account-associated-with-an-azure-subscription"></a>Anmelden mit einem Konto, das einem Azure-Abonnement zugeordnet ist

1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, und stimmen Sie den Nutzungsbedingungen zu.

1. Für die Registrierung haben Sie zwei Möglichkeiten:

    * Fahren Sie mit der Verwendung einer Azure-Ressource fort. Dies ist der empfohlene Pfad und bald der einzig verfügbare Pfad. Dieser Pfad ermöglicht es Ihnen, Ihr LUIS-Konto mit einer Azure-Ressource zu verknüpfen, während Sie sich registrieren, indem Sie entweder eine bestehende Ressource in Ihrem Abonnement auswählen oder eine neue Ressource erstellen. Dies ist gleichbedeutend mit der Registrierung bei bestehender Migration ohne die Notwendigkeit, sich später dem [Migrationsprozess](luis-migration-authoring.md#what-is-migration) zu unterziehen. Alle Benutzer müssen die Migration bis zum 2. November 2020 abgeschlossen haben.

    * Fahren Sie mithilfe des Start- oder Testschlüssels fort. Dieser Pfad ermöglicht es Ihnen, sich mit der bereitgestellten Start- oder Testressource bei LUIS anzumelden, ohne Ressourcen erstellen zu müssen. Wenn Sie diesen Pfad wählen, müssen Sie schließlich [Ihr Konto migrieren](luis-migration-authoring.md#migration-steps) und Ihre Anwendungen mit einer Erstellungsressource verknüpfen. Deshalb wird empfohlen, den Pfad auszuwählen, bei dem Sie mit Ihrer Azure-Ressource fortfahren.

    [Erfahren Sie mehr über Erstellungs- und Startschlüssel](luis-how-to-azure-subscription.md#luis-resources). Beide Ressourcen bieten Ihnen 1 Million kostenlose Erstellungsressourcen und 1000 kostenlose Vorhersageressourcen für Endpunkte.

    :::image type="content" source="media/sign-in/signup-landing-page.png" alt-text="Teilscreenshot der LUIS-Navigationsleiste mit Azure-Symbol.":::

1. Verwenden einer vorhandenen Erstellungsressource

    :::image type="content" source="media/sign-in/signup-choose-resource.png" alt-text="Teilscreenshot der LUIS-Navigationsleiste mit Azure-Symbol.":::

    Wenn Sie bereits LUIS-Erstellungsressourcen in Ihrem Abonnement verwenden und diese während der Anmeldung mit Ihrem LUIS-Konto verknüpfen, wählen Sie die Option **Vorhandene Erstellungsressource verwenden**, und geben Sie die folgenden Informationen an:

    * **Mandant**: der Mandant, dem Ihr Azure-Abonnement zugeordnet ist Sie können die Mandanten nicht über das bestehende Fenster wechseln. Sie können die Mandanten wechseln, indem Sie den äußersten rechten Avatar auswählen, der Ihre Initialen in der oberen Leiste enthält.
    * **Abonnementname**: Das Abonnement, das der Ressource zugeordnet wird. Wenn Sie über mehr als ein Abonnement verfügen, das zu Ihrem Mandanten gehört, wählen Sie das gewünschte Abonnement aus der Dropdownliste aus.
    * **Ressourcenname**: Die Erstellungsressource, die Sie Ihrem Konto zuordnen möchten.

    > [!Note]
    > Wenn die gesuchte Erstellungsressource in der Dropdownliste abgeblendet ist, bedeutet dies, dass Sie sich bei einem anderen regionalen Portal angemeldet haben. [Grundlegendes zum Konzept regionaler Portale](luis-reference-regions.md#luis-authoring-regions).

1. Erstellen einer neuen Erstellungsressource

    :::image type="content" source="media/sign-in/signup-create-resource.png" alt-text="Teilscreenshot der LUIS-Navigationsleiste mit Azure-Symbol.":::

    Geben Sie beim **Erstellen einer neuen Erstellungsressource** die folgenden Informationen an:

    * **Mandant**: der Mandant, dem Ihr Azure-Abonnement zugeordnet ist Sie können die Mandanten nicht über das bestehende Fenster wechseln. Sie können die Mandanten wechseln, indem Sie den äußersten rechten Avatar auswählen, der Ihre Initialen in der oberen Leiste enthält.
    * **Ressourcenname**: Ein von Ihnen gewählter benutzerdefinierter Name, der als Teil der URL für Ihre Erstellungstransaktionen verwendet wird. Ihr Ressourcenname darf nur alphanumerische Zeichen und „-“ enthalten und darf nicht mit „-“ beginnen oder enden. Wenn andere Symbole im Namen enthalten sind, wird beim Erstellen einer Ressource ein Fehler auftreten.
    * **Abonnementname**: Das Abonnement, das der Ressource zugeordnet wird. Wenn Sie über mehr als ein Abonnement verfügen, das zu Ihrem Mandanten gehört, wählen Sie das gewünschte Abonnement aus der Dropdownliste aus.
    * **Ressourcengruppe**: Ein benutzerdefinierter Ressourcengruppenname, den Sie in Ihrem Abonnement auswählen. Mit Ressourcengruppen können Sie Azure-Ressourcen für den Zugriff und die Verwaltung gruppieren. Wenn Sie derzeit nicht über eine Ressourcengruppe in Ihrem Abonnement verfügen, können Sie im LUIS-Portal keine Ressourcengruppe erstellen. Wechseln Sie zum [Azure-Portal](https://ms.portal.azure.com/#create/Microsoft.ResourceGroup), um eine zu erstellen, und wechseln Sie dann zu LUIS, um den Anmeldevorgang fortzusetzen.

1. Nachdem Sie Ihren Pfad ausgewählt haben, kann es einige Sekunden dauern, bis ein Hinweis angezeigt wird, dass Ihr Konto erfolgreich migriert wurde. Schließen Sie den Vorgang durch Auswahl von **Fortfahren** ab.

    :::image type="content" source="media/sign-in/signup-confirm-2.png" alt-text="Teilscreenshot der LUIS-Navigationsleiste mit Azure-Symbol.":::

    > [!Note]
    > Wenn Sie über ein Abonnement und mindestens eine Erstellungsressource in der Region verfügen, die mit der Region übereinstimmt, für die Sie sich im Portal registrieren, melden Sie sich möglicherweise automatisch migriert und einer Ressource zugeordnet bei LUIS an, ohne dass Sie sich für einen Pfad entscheiden müssen.


## <a name="sign-in-with-user-account-not-associated-with-an-azure-subscription"></a>Anmelden mit einem Benutzerkonto, das keinem Azure-Abonnement zugeordnet ist

1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, und aktivieren Sie die Option, dass Sie den Nutzungsbedingungen zustimmen.

1. Schließen Sie den Vorgang durch Auswahl von **Fortfahren** ab. Sie melden sich automatisch mit einem Test-/Startschlüssel an. Das bedeutet, dass Sie schließlich [Ihr Konto migrieren](luis-migration-authoring.md#migration-steps) und Ihre Anwendungen mit einer Erstellungsressource verknüpfen müssen. Sie müssen sich für eine [kostenlose Azure-Testversion](https://azure.microsoft.com/free/) anmelden, um sich dem Migrationsprozess zu unterziehen.

    :::image type="content" source="media/sign-in/signup-no-subscription.png" alt-text="Teilscreenshot der LUIS-Navigationsleiste mit Azure-Symbol.":::

## <a name="troubleshooting"></a>Problembehandlung

* Wenn Sie eine Erstellungsressource über das Azure-Portal in einer anderen Region erstellen, die nicht dem Portal entspricht, bei dem Sie sich anmelden, wird die Erstellungsressource abgeblendet dargestellt.
* Achten Sie beim Erstellen einer neuen Ressource darauf, dass der Ressourcenname nur alphanumerische Zeichen und „-“, enthält und weder mit „-“ beginnen oder enden darf. Andernfalls schlägt der Vorgang fehl.
* Stellen Sie sicher, dass Sie über die [entsprechenden Berechtigungen für Ihr Abonnement verfügen, um eine Azure-Ressource](../../role-based-access-control/rbac-and-directory-admin-roles.md#azure-roles) zu erstellen. Wenn Sie nicht über die entsprechenden Berechtigungen verfügen, wenden Sie sich an den Administrator Ihres Abonnements, um ausreichende Berechtigungen zu erhalten.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie [eine neue App starten](luis-how-to-start-new-app.md).
