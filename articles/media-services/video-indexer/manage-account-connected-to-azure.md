---
title: Verwalten eines Video Indexer-Kontos
titleSuffix: Azure Media Services
description: Erfahren Sie, wie Sie ein mit Azure verbundenes Video Indexer-Konto verwalten.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 01/14/2021
ms.author: juliako
ms.openlocfilehash: 25025efe5254442c4862563ce329cfae69e7d033
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98898442"
---
# <a name="manage-a-video-indexer-account-connected-to-azure"></a>Verwalten eines Video Indexer-Kontos mit Azure-Verbindung

In diesem Artikel wird veranschaulicht, wie Sie ein Video Indexer-Konto verwalten, das mit Ihrem Azure-Abonnement und einem Azure Media Services-Konto verbunden ist.

> [!NOTE]
> Sie müssen der Inhaber des Video Indexer-Kontos sein, um die hier beschriebenen Anpassungen an der Kontokonfiguration vornehmen zu können.

## <a name="prerequisites"></a>Voraussetzungen

Verbinden Sie Ihr Video Indexer-Konto mit Azure, wie in [Erstellen eines Video Indexer-Kontos mit Azure-Verbindung](connect-to-azure.md) beschrieben.

Beachten Sie die [Voraussetzungen](connect-to-azure.md#prerequisites-for-connecting-to-azure), und machen Sie sich mit den [Überlegungen](connect-to-azure.md#azure-media-services-considerations) in diesem Artikel vertraut.

## <a name="examine-account-settings"></a>Untersuchen der Kontoeinstellungen

In diesem Abschnitt werden die Einstellungen für Ihr Video Indexer-Konto untersucht.

So zeigen Sie die Einstellungen an:

1. Klicken Sie auf das Benutzersymbol in der oberen rechten Ecke, und wählen Sie **Einstellungen** aus.

    ![Einstellungen in Video Indexer](./media/manage-account-connected-to-azure/select-settings.png)

2. Wählen Sie auf der Seite **Einstellungen** die Registerkarte **Weiter** aus.

Wenn Ihr Video Indexer-Konto mit Azure verbunden ist, sehen Sie Folgendes:

* Der Name des zugrunde liegenden Azure Media Services-Kontos.
* Die Anzahl der aktuell ausgeführten und sich in der Warteschlange befindlichen Indizierungsaufträge.
* Die Anzahl und den Typ der zugeordneten reservierten Einheiten.

Wenn für Ihr Konto Anpassungen erforderlich sind, werden auf der Seite **Einstellungen** entsprechende Fehler- und Warnmeldungen zu Ihrer Kontokonfiguration angezeigt. Die Nachrichten enthalten Links zu den genauen Stellen im Azure-Portal, in denen Sie Änderungen vornehmen müssen. Weitere Informationen finden Sie im folgenden Abschnitt [Fehler und Warnungen](#errors-and-warnings).

## <a name="repair-the-connection-to-azure"></a>Reparieren der Verbindung mit Azure

Im Dialogfeld **Verbindung mit Azure Media Services aktualisieren** Ihrer Seite [Video Indexer](https://www.videoindexer.ai/) werden Sie aufgefordert, Werte für die folgenden Einstellungen anzugeben:

|Einstellung|BESCHREIBUNG|
|---|---|
|Azure-Abonnement-ID|Die Abonnement-ID kann über das Azure-Portal abgerufen werden. Klicken Sie im linken Navigationsbereich auf **Alle Dienste**, und suchen Sie nach „Abonnements“. Wählen Sie **Abonnements** und dann in der Liste Ihrer Abonnements die gewünschte ID aus.|
|Name der Azure Media Services-Ressourcengruppen|Der Name der Ressourcengruppe, in der Sie das Media Services-Konto erstellt haben.|
|Anwendungs-ID|Die Azure AD-Anwendungs-ID (mit Berechtigungen für das angegebene Media Services-Konto), die Sie für dieses Video Indexer-Konto erstellt haben. <br/><br/>Um die App-ID abzurufen, navigieren Sie zum Azure-Portal. Wählen Sie unter dem Media Services-Konto Ihr Konto aus, und wechseln Sie zu **API-Zugriff**. Wählen Sie **Verbindung mit Azure Media Services-API über Dienstprinzipal herstellen** -> **Azure AD-App** aus. Kopieren Sie die relevanten Parameter.|
|Anwendungsschlüssel|Der Ihrem oben angegebenen Media Services-Konto zugeordnete Azure AD-Anwendungsschlüssel. <br/><br/>Um den App-Schlüssel zu erhalten, navigieren Sie zum Azure-Portal. Wählen Sie unter dem Media Services-Konto Ihr Konto aus, und wechseln Sie zu **API-Zugriff**. Wählen Sie **Verbindung mit Media Services-API über Dienstprinzipal herstellen** -> **Anwendung verwalten** -> **Zertifikate und Geheimnisse** aus. Kopieren Sie die relevanten Parameter.|

## <a name="autoscale-reserved-units"></a>Automatische Skalierung reservierter Einheiten

Auf der Seite **Einstellungen** können Sie die automatische Skalierung von reservierten Einheiten (RU) für Medien festlegen. Wenn die Option **Ein** lautet, können Sie die maximale Anzahl der RUs zuweisen und sicherstellen, dass der Video Indexer die RUs automatisch stoppt/startet. Mit dieser Option zahlen Sie nicht zusätzlich für Leerlaufzeiten, aber warten auch nicht darauf, dass die Indizierungsaufträge über einen längeren Zeitraum ausgeführt werden, wenn die Indizierungslast hoch ist.

Bei der automatischen Skalierung ist keine Skalierung unter 1 RU oder über dem Standardlimit das Media Services-Konto möglich. Erstellen Sie einen Service Request, um dieses Limit zu erhöhen. Informationen zu Kontingenten und Einschränkungen sowie zum Öffnen von Supporttickets finden Sie unter [Kontingente und Einschränkungen](../../media-services/previous/media-services-quotas-and-limitations.md).

![Automatische Skalierung reservierter Einheiten in Video Indexer](./media/manage-account-connected-to-azure/autoscale-reserved-units.png)

## <a name="errors-and-warnings"></a>Fehler und Warnungen

Wenn für Ihr Konto einige Anpassungen erforderlich ist, werden auf der Seite **Einstellungen** entsprechende Fehler- und Warnmeldungen zu Ihrer Kontokonfiguration angezeigt. Die Nachrichten enthalten Links zu den genauen Stellen im Azure-Portal, in denen Sie Änderungen vornehmen müssen. Dieser Abschnitt enthält weitere Informationen zu Fehler- und Warnmeldungen.

* EventGrid

    Sie müssen den EventGrid-Ressourcenanbieter über das Azure-Portal registrieren. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) zu **Abonnements** > [Abonnement] > **ResourceProviders** > **Microsoft.EventGrid**. Wählen Sie **Registrieren** aus, falls der Status nicht **Registriert** lautet. Die Registrierung nimmt einige Minuten in Anspruch.

* Streamingendpunkt

    Stellen Sie sicher, dass sich der standardmäßige **Streamingendpunkt** des zugrunde liegenden Media Services-Kontos im Status „Gestartet“ befindet. Andernfalls können Sie keine Videos über dieses Media Services-Konto oder in Video Indexer ansehen.

* Reservierte Einheiten für Medien

    Sie müssen reservierte Einheiten für Medien für Ihre Media Services-Ressource zuordnen, um Video indizieren zu können. Für die optimale Indizierungsleistung wird empfohlen, mindestens 10 reservierte S3-Einheiten zuzuordnen. Preisinformationen finden Sie im Abschnitt mit den häufig gestellten Fragen auf der Seite [Media Services – Preise](https://azure.microsoft.com/pricing/details/media-services/).

## <a name="next-steps"></a>Nächste Schritte

Sie können programmgesteuert mit Ihrem Testkonto oder Ihren Video Indexer-Konten interagieren, die mit Azure verbunden sind. Befolgen Sie hierzu die Anweisungen unter [Verwenden von APIs](video-indexer-use-apis.md).

Verwenden Sie hierfür denselben Azure AD-Benutzer, den Sie beim Herstellen der Verbindung mit Azure genutzt haben.
