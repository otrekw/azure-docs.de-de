---
title: Kostenloses Testen des Speech-Diensts
titleSuffix: Azure Cognitive Services
description: Die ersten Schritte mit dem Speech-Dienst sind einfach und erschwinglich. Es stehen zwei kostenlose Optionen zur Verfügung, damit Sie herausfinden können, wofür der Dienst verwendet werden kann und ob er für Ihre Anforderungen geeignet ist.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/03/2020
ms.author: erhopf
ms.custom: seodec18, seo-javascript-october2019
ms.openlocfilehash: c15601b4c58ce072881b3c244f10bc196bceb853
ms.sourcegitcommit: c28fc1ec7d90f7e8b2e8775f5a250dd14a1622a6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2020
ms.locfileid: "88167809"
---
# <a name="try-the-speech-service-for-free"></a>Kostenloses Testen des Speech-Diensts

In diesem Artikel wählen Sie eine Option aus, mit der Sie den Speech-Dienst ganz einfach kostenlos testen können, um herauszufinden, wofür der Dienst verwendet werden kann und ob er für Ihre Anforderungen geeignet ist. Wählen Sie je nach Situation und Anwendungsfall eine der beiden folgenden Optionen aus:

- [Option 1](#no-card): Nutzen Sie direkt eine **kostenlose Testversion** der API-Schlüssel ohne Angabe von Kreditkarteninformationen (vorhandenes Azure-Konto erforderlich). Die **kostenlose Testversion** gilt 30 Tage lang, und die Daten werden am Ende gelöscht. Diese Option eignet sich am besten für schnelle Experimente mit dem Dienst.
- [Option 2](#new-resource): Erstellen Sie mit einem **kostenlosen Abonnement** (Kreditkarteninformationen erforderlich) eine neue Speech-Ressource in Azure. Bei einem **kostenlosen Abonnement** gelten hauptsächlich strengere Ratenlimits als bei einem kostenpflichtigen Abonnement. Diese Option ist am besten geeignet, wenn Sie den Dienst testen möchten, aber auch ein zukünftiges Upgrade auf ein kostenpflichtiges Abonnement planen und Ihre Daten dabei weiterverwenden möchten.

## <a name="try-the-speech-service-without-credit-card-info"></a><a id="no-card"></a>Testen des Speech-Diensts ohne Kreditkarteninformationen

Führen Sie die folgenden Schritte aus, um eine 30-tägige kostenlose Testversion zu aktivieren und API-Schlüssel zu erhalten. Der Testzeitraum beginnt sofort nach Abschluss der folgenden Schritte.

1. Navigieren Sie zu [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/).
1. Wählen Sie die Registerkarte **Spracheingabe-APIs** aus.
1. Wählen Sie **API-Schlüssel abrufen** aus.

Ihnen werden Abrechnungsoptionen angezeigt. Wählen Sie die kostenlose Option aus, lesen Sie die Benutzervereinbarung, und bestätigen Sie sie. Ihnen werden Schlüssel angezeigt, die Sie zum Testen des Speech-Diensts für 30 Tage verwenden können.

## <a name="try-the-speech-service-by-creating-an-azure-resource"></a><a id="new-resource"></a>Testen des Speech-Diensts durch Erstellen einer Azure-Ressource

Für die folgenden Schritte benötigen Sie ein Microsoft-Konto und ein Azure-Konto. Wenn Sie kein Microsoft-Konto besitzen, können Sie sich kostenlos im [Microsoft-Kontoportal](https://account.microsoft.com/account) registrieren. Wählen Sie **Mit Microsoft anmelden** und dann **Microsoft-Konto erstellen** aus, wenn Sie zur Anmeldung aufgefordert werden. Führen Sie die Schritte zum Erstellen und Überprüfen Ihres neuen Microsoft-Kontos aus.

Wenn Sie ein Microsoft-Konto besitzen, navigieren Sie zur [Azure-Registrierungsseite](https://azure.microsoft.com/free/ai/), wählen Sie **Kostenlos starten** aus, und erstellen Sie ein neues Azure-Konto mithilfe Ihres Microsoft-Kontos.

> [!NOTE]
> Der Speech-Dienst weist die beiden Dienstebenen „Free“ und „Abonnement“ auf, die mit unterschiedlichen Einschränkungen und Vorteilen verbunden sind. Wenn Sie sich für ein kostenloses Azure-Konto registrieren, beinhaltet dieses ein Guthaben von 200 USD, das Sie bis zu 30 Tage lang auf ein kostenpflichtiges Abonnement für den Speech-Dienst anwenden können.
>
> Wenn Sie die Dienstebene „Free“ des Speech-Dienst für kleine Datenmengen verwenden, können Sie dieses kostenlose Abonnement auch nach Ablauf Ihrer kostenlosen Testversion oder Ihres Dienstguthabens weiterhin nutzen.
>
> Weitere Informationen finden Sie unter [Cognitive Services-Preise: Speech-Dienst](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

### <a name="create-the-resource"></a>Erstellen der Ressource

Gehen Sie wie folgt vor, um Ihrem Azure-Konto eine Ressource des Sprachdiensts (kostenloser oder kostenpflichtiger Tarif) hinzuzufügen.

1. Melden Sie sich mit Ihrem Microsoft-Konto am [Azure-Portal](https://portal.azure.com/) an.

1. Wählen Sie links oben im Portal die Option **Ressource erstellen** aus. Wenn **Ressource erstellen** nicht angezeigt wird, finden Sie die Option jederzeit durch Auswählen des reduzierten Menüs in der linken oberen Ecke:

   ![Reduzierte Navigationsschaltfläche](media/index/collapsed-nav.png)

1. Geben Sie im Fenster **Neu** die Zeichenfolge „Speech“ in das Suchfeld ein, und drücken Sie die EINGABETASTE.

1. Wählen Sie in den Suchergebnissen **Sprache** aus.

   ![Suchergebnisse für „Speech“](media/index/speech-search.png)

1. Wählen Sie **Erstellen** aus, und führen Sie dann folgende Schritte aus:

   - Geben Sie der neuen Ressource einen eindeutigen Namen. Über den Namen können Sie zwischen mehreren Abonnements unterscheiden, die für denselben Dienst gelten.
   - Wählen Sie das Azure-Abonnement aus, dem die neue Ressource zugeordnet ist, um zu bestimmen, wie die Gebühren berechnet werden.
   - Wählen Sie die [Region](regions.md) aus, in der die Ressource verwendet wird.
   - Wählen Sie einen kostenlosen (F0) oder einen kostenpflichtigen (S0) Tarif aus. Wählen Sie **Alle Preisinformationen anzeigen** aus, um vollständige Informationen zu Preisen und Verwendungskontingenten für die einzelnen Tarife zu erhalten.
   - Erstellen Sie eine neue Ressourcengruppe für dieses Abonnement für den Sprachdienst, oder weisen Sie das Abonnement einer vorhandenen Ressourcengruppe zu. Anhand von Ressourcengruppen können Sie Ihre verschiedenen Azure-Abonnements organisieren.
   - Klicken Sie auf **Erstellen**. Dadurch gelangen Sie zur Bereitstellungsübersicht, in der Statusmeldungen zur Bereitstellung angezeigt werden.

> [!NOTE]
> Sie können beliebig viele Abonnements mit dem Tarif „Standard“ in einer oder mehreren Regionen erstellen. Sie können dagegen nur ein Abonnement mit dem Tarif „Free“ erstellen. Modellimplementierungen im Tarif „Free“, die 7 Tage lang ungenutzt bleiben, werden automatisch außer Betrieb genommen.

Es dauert einen Moment, bis die neue Speech-Ressource bereitgestellt wird. Wählen Sie nach Abschluss der Bereitstellung **Zu Ressource wechseln** und dann im linken Navigationsbereich **Schlüssel** aus, um Ihre Abonnementschlüssel für den Speech-Dienst anzuzeigen. Jedem Abonnement sind zwei Schlüssel zugewiesen. Sie können jeden dieser Schlüssel in Ihrem Abonnement verwenden. Um schnell einen Schlüssel zu kopieren und in Ihrem Code-Editor oder an einem anderen Speicherort einzufügen, wählen Sie die Schaltfläche „Kopieren“ neben dem Schlüssel aus, wechseln Sie das Fenster, und fügen Sie den Inhalt der Zwischenablage an der gewünschten Stelle ein

> [!IMPORTANT]
> Diese Abonnementschlüssel werden für den Zugriff auf Ihre Cognitive Service-API verwendet. Geben Sie Ihre Schlüssel nicht weiter. Speichern Sie diese sicher, z. B. mit Azure Key Vault. Es wird außerdem empfohlen, diese Schlüssel regelmäßig neu zu generieren. Für einen API-Aufruf ist nur ein Schlüssel erforderlich. Beim erneuten Generieren des ersten Schlüssels können Sie den zweiten Schlüssel für kontinuierlichen Zugriff auf den Dienst verwenden.

## <a name="switch-to-a-new-subscription"></a>Wechseln zu einem neuen Abonnement

Um von einem Abonnement zu einem anderen zu wechseln (z.B. wenn Ihre kostenlose Testversion abläuft oder wenn Sie Ihre Anwendung veröffentlichen), ersetzen Sie die Region und den Abonnementschlüssel in Ihrem Code durch die Region und den Abonnementschlüssel der neuen Azure-Ressource.

## <a name="about-regions"></a>Informationen zu Regionen

- Wenn Ihre Anwendung ein [Speech SDK](speech-sdk.md) verwendet, geben Sie beim Erstellen einer Sprachkonfiguration den Regionsbezeichner (z. B. `westus`) an.
- Wenn Ihre Anwendung eine der [REST-APIs](rest-apis.md) des Sprachdiensts verwendet, ist die Region Teil des Endpunkt-URIs, den Sie bei der Anforderung verwenden.
- Schlüssel, die für eine Region erstellt wurden, sind nur in dieser Region gültig. Der Versuch, sie mit anderen Regionen zu verwenden, führt zu Authentifizierungsfehlern.

## <a name="next-steps"></a>Nächste Schritte

Arbeiten Sie einen unserer 10-Minuten-Schnellstarts durch, oder sehen Sie sich unsere SDK-Beispiele an:

> [!div class="nextstepaction"]
> [Schnellstart: Erkennen von Sprache mit der Speech CLI](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programmer-tool-spx)
> [Speech SDK-Beispiele](speech-sdk.md#sample-source-code)
