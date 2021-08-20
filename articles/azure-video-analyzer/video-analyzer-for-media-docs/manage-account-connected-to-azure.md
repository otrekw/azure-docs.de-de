---
title: Verwalten eines Kontos für Azure Video Analyzer for Media (früher Video Indexer)
titleSuffix: Azure Video Analyzer for Media
description: Erfahren Sie, wie Sie ein mit Azure verbundenes Konto für Azure Video Analyzer for Media (früher Video Indexer) verwalten.
services: azure-video-analyzer
author: Juliako
manager: femila
ms.topic: article
ms.subservice: azure-video-analyzer-media
ms.date: 01/14/2021
ms.author: juliako
ms.openlocfilehash: 25d4b5a918fbb307698910f0184231689cb84a75
ms.sourcegitcommit: 0af634af87404d6970d82fcf1e75598c8da7a044
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/15/2021
ms.locfileid: "112119395"
---
# <a name="manage-a-video-analyzer-for-media-account-connected-to-azure"></a>Verwalten eines mit Azure verbundenen Video Analyzer for Media-Kontos

In diesem Artikel wird veranschaulicht, wie Sie ein Konto für Azure Video Analyzer for Media (früher Video Indexer) verwalten, das mit Ihrem Azure-Abonnement und einem Azure Media Services-Konto verbunden ist.

> [!NOTE]
> Sie müssen der Besitzer des Video Analyzer for Media-Kontos sein, um die hier beschriebenen Anpassungen an der Kontokonfiguration vornehmen zu können.

## <a name="prerequisites"></a>Voraussetzungen

Verbinden Sie Ihr Video Analyzer for Media-Konto mit Azure (siehe Beschreibung im Artikel zum [Herstellen einer Azure-Verbindung](connect-to-azure.md)).

Beachten Sie die [Voraussetzungen](connect-to-azure.md#prerequisites-for-connecting-to-azure), und machen Sie sich mit den [Überlegungen](connect-to-azure.md#azure-media-services-considerations) in diesem Artikel vertraut.

## <a name="examine-account-settings"></a>Untersuchen der Kontoeinstellungen

In diesem Abschnitt werden die Einstellungen Ihres Video Analyzer for Media-Kontos untersucht.

So zeigen Sie die Einstellungen an:

1. Klicken Sie auf das Benutzersymbol in der oberen rechten Ecke, und wählen Sie **Einstellungen** aus.

    ![Einstellungen in Video Analyzer for Media](./media/manage-account-connected-to-azure/select-settings.png)

2. Wählen Sie auf der Seite **Einstellungen** die Registerkarte **Weiter** aus.

Wenn Ihr Video Indexer-Konto mit Azure verbunden ist, sehen Sie Folgendes:

* Der Name des zugrunde liegenden Azure Media Services-Kontos.
* Die Anzahl der aktuell ausgeführten und sich in der Warteschlange befindlichen Indizierungsaufträge.
* Die Anzahl und den Typ der zugeordneten reservierten Einheiten.

Wenn für Ihr Konto Anpassungen erforderlich sind, werden auf der Seite **Einstellungen** entsprechende Fehler- und Warnmeldungen zu Ihrer Kontokonfiguration angezeigt. Die Nachrichten enthalten Links zu den genauen Stellen im Azure-Portal, in denen Sie Änderungen vornehmen müssen. Weitere Informationen finden Sie im folgenden Abschnitt [Fehler und Warnungen](#errors-and-warnings).

## <a name="repair-the-connection-to-azure"></a>Reparieren der Verbindung mit Azure

Im Dialogfeld **Verbindung mit Azure Media Services aktualisieren** Ihrer [Video Analyzer for Media](https://www.videoindexer.ai/)-Seite werden Sie aufgefordert, Werte für die folgenden Einstellungen anzugeben:

|Einstellung|BESCHREIBUNG|
|---|---|
|Azure-Abonnement-ID|Die Abonnement-ID kann über das Azure-Portal abgerufen werden. Klicken Sie im linken Navigationsbereich auf **Alle Dienste**, und suchen Sie nach „Abonnements“. Wählen Sie **Abonnements** und dann in der Liste Ihrer Abonnements die gewünschte ID aus.|
|Name der Azure Media Services-Ressourcengruppen|Der Name der Ressourcengruppe, in der Sie das Media Services-Konto erstellt haben.|
|Anwendungs-ID|Die Azure AD-Anwendungs-ID (mit Berechtigungen für das angegebene Media Services-Konto), die Sie für dieses Video Analyzer for Media-Konto erstellt haben. <br/><br/>Um die App-ID abzurufen, navigieren Sie zum Azure-Portal. Wählen Sie unter dem Media Services-Konto Ihr Konto aus, und wechseln Sie zu **API-Zugriff**. Wählen Sie **Verbindung mit Azure Media Services-API über Dienstprinzipal herstellen** -> **Azure AD-App** aus. Kopieren Sie die relevanten Parameter.|
|Anwendungsschlüssel|Der Ihrem oben angegebenen Media Services-Konto zugeordnete Azure AD-Anwendungsschlüssel. <br/><br/>Um den App-Schlüssel zu erhalten, navigieren Sie zum Azure-Portal. Wählen Sie unter dem Media Services-Konto Ihr Konto aus, und wechseln Sie zu **API-Zugriff**. Wählen Sie **Verbindung mit Media Services-API über Dienstprinzipal herstellen** -> **Anwendung verwalten** -> **Zertifikate und Geheimnisse** aus. Kopieren Sie die relevanten Parameter.|

## <a name="autoscale-reserved-units"></a>Automatische Skalierung reservierter Einheiten

Auf der Seite **Einstellungen** können Sie die automatische Skalierung von reservierten Einheiten (RU) für Medien festlegen. Wenn die Option **Ein** lautet, können Sie die maximale Anzahl der RUs zuweisen und sicherstellen, dass Video Analyzer for Media die RUs automatisch stoppt/startet. Mit dieser Option zahlen Sie nicht zusätzlich für Leerlaufzeiten, aber warten auch nicht darauf, dass die Indizierungsaufträge über einen längeren Zeitraum ausgeführt werden, wenn die Indizierungslast hoch ist.

Bei der automatischen Skalierung ist keine Skalierung unter 1 RU oder über dem Standardlimit das Media Services-Konto möglich. Erstellen Sie einen Service Request, um dieses Limit zu erhöhen. Informationen zu Kontingenten und Einschränkungen sowie zum Öffnen von Supporttickets finden Sie unter [Kontingente und Einschränkungen](../../media-services/previous/media-services-quotas-and-limitations.md).

![Automatische Skalierung reservierter Einheiten in Video Analyzer for Media](./media/manage-account-connected-to-azure/autoscale-reserved-units.png)

## <a name="errors-and-warnings"></a>Fehler und Warnungen

Wenn für Ihr Konto einige Anpassungen erforderlich ist, werden auf der Seite **Einstellungen** entsprechende Fehler- und Warnmeldungen zu Ihrer Kontokonfiguration angezeigt. Die Nachrichten enthalten Links zu den genauen Stellen im Azure-Portal, in denen Sie Änderungen vornehmen müssen. Dieser Abschnitt enthält weitere Informationen zu Fehler- und Warnmeldungen.

* EventGrid

    Sie müssen den EventGrid-Ressourcenanbieter über das Azure-Portal registrieren. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) zu **Abonnements** > [Abonnement] > **ResourceProviders** > **Microsoft.EventGrid**. Wählen Sie **Registrieren** aus, falls der Status nicht **Registriert** lautet. Die Registrierung nimmt einige Minuten in Anspruch.

* Streamingendpunkt

    Stellen Sie sicher, dass sich der standardmäßige **Streamingendpunkt** des zugrunde liegenden Media Services-Kontos im Status „Gestartet“ befindet. Andernfalls können Sie keine Videos über dieses Media Services-Konto oder in Video Analyzer for Media ansehen.

* Reservierte Einheiten für Medien

    Sie müssen reservierte Einheiten für Medien für Ihre Media Services-Ressource zuordnen, um Video indizieren zu können. Für die optimale Indizierungsleistung wird empfohlen, mindestens 10 reservierte S3-Einheiten zuzuordnen. Preisinformationen finden Sie im Abschnitt mit den häufig gestellten Fragen auf der Seite [Media Services – Preise](https://azure.microsoft.com/pricing/details/media-services/).

## <a name="next-steps"></a>Nächste Schritte

Sie können programmgesteuert mit Ihrem Testkonto oder Ihren Video Analyzer for Media-Konten interagieren, die mit Azure verbunden sind. Befolgen Sie hierzu die Anweisungen unter [Verwenden von APIs](video-indexer-use-apis.md).

Verwenden Sie hierfür denselben Azure AD-Benutzer, den Sie beim Herstellen der Verbindung mit Azure genutzt haben.
