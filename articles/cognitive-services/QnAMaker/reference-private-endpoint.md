---
title: Einrichten eines privaten Endpunkts – QnA Maker
description: Hier erfahren Sie mehr über die in „QnA Maker managed“ verfügbare Option zum Erstellen von privaten Endpunkten.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: reference
ms.date: 01/12/2021
ms.openlocfilehash: 00a55b852158abee01692d3c4ccbf1209c937cf4
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110379519"
---
# <a name="private-endpoints"></a>Private Endpunkte

Ein privater Endpunkt in Azure ist eine Netzwerkschnittstelle, die Sie privat und sicher mit einem von Azure Private Link betriebenen Dienst verbindet. „Benutzerdefinierte Fragen und Antworten“ bietet jetzt Unterstützung für das Erstellen privater Endpunkte im Azure Search-Dienst.

Private Endpunkte werden durch [Azure Private Link](../../private-link/private-link-overview.md) als separater Dienst bereitgestellt. Weitere Informationen zu den Kosten finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/private-link/). 

## <a name="prerequisites"></a>Voraussetzungen
> [!div class="checklist"]
> * Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/cognitive-services/) erstellen, bevor Sie beginnen.
> * Eine im Azure-Portal erstellte [Textanalyse-Ressource](https://ms.portal.azure.com/?quickstart=true#create/Microsoft.CognitiveServicesTextAnalytics) (mit dem Feature „Benutzerdefinierte Fragen und Antworten“). Merken Sie sich die Azure Active Directory-ID, das Abonnement und den Namen der Textanalyse-Ressource, den Sie beim Erstellen der Ressource ausgewählt haben.

## <a name="steps-to-enable-private-endpoint"></a>Schritte zum Aktivieren des privaten Endpunkts
1. Weisen Sie dem Textanalyse-Dienst in der Azure Search-Dienstinstanz die Rolle *Mitwirkender* zu. Dieser Vorgang erfordert die Zugriffsberechtigungen der Rolle *Besitzer* für das Abonnement. Wechseln Sie zur Registerkarte „Identität“ in der Dienstressource, um die Identität abzurufen.

> [!div class="mx-imgBorder"]
> ![Textanalyse: Identität](../QnAMaker/media/qnamaker-reference-private-endpoints/private-endpoints-identity.png)

2. Fügen Sie die oben genannte Identität als *Contributer* (Mitwirkender) hinzu, indem Sie zur Registerkarte „Zugriffssteuerung (IAM)“ des Azure Search-Diensts navigieren.

![IAM für verwalteten Dienst](../QnAMaker/media/qnamaker-reference-private-endpoints/private-endpoint-access-control.png)

3. Klicken Sie auf *Add role assignments* (Rollenzuweisungen hinzufügen), fügen Sie die Identität hinzu, und klicken Sie auf *Speichern*.

![Rollenzuweisung für verwalteten Dienst](../QnAMaker/media/qnamaker-reference-private-endpoints/private-endpoint-role-assignment.png)

4. Navigieren Sie nun in der Azure Search-Dienstinstanz zur Registerkarte *Netzwerk*, und ändern Sie die Einstellung für „Endpunktkonnektivität (Daten)“ von *Öffentlich* in *Privat*. Dieser Vorgang ist ein zeitintensiver Prozess und kann bis zu 30 Minuten in Anspruch nehmen. 

![Registerkarte „Netzwerk“ für verwalteten Azure Search-Dienst](../QnAMaker/media/qnamaker-reference-private-endpoints/private-endpoint-networking.png)

5. Wechseln Sie im Textanalyse-Dienst zur Registerkarte *Netzwerk*. Wählen Sie unter *Zugriff erlauben von* die Option *Ausgewählte Netzwerke und private Endpunkte* aus, und klicken Sie auf *Speichern*.
 
> [!div class="mx-imgBorder"]
> ![Textanalyse: Netzwerk](../QnAMaker/media/qnamaker-reference-private-endpoints/private-endpoint-networking-custom-qna.png)

Dadurch wird eine private Endpunktverbindung zwischen dem Textanalyse-Dienst und der Azure Cognitive Search-Dienstinstanz hergestellt. Sie können die private Endpunktverbindung auf der Registerkarte *Netzwerk* der Azure Cognitive Search-Dienstinstanz überprüfen. Nachdem der gesamte Vorgang abgeschlossen ist, können Sie Ihren Textanalyse-Dienst verwenden. 

![Registerkarte „Netzwerk“ für verwalteten Dienst](../QnAMaker/media/qnamaker-reference-private-endpoints/private-endpoint-networking-3.png)


## <a name="support-details"></a>Details zur Unterstützung
 * Wir unterstützen keine Änderungen am Azure Cognitive Search-Dienst, nachdem Sie den privaten Zugriff auf Ihren Textanalyse-Dienst aktiviert haben. Wenn Sie den Azure Cognitive Search-Dienst über die Registerkarte „Features“ ändern, nachdem Sie den privaten Zugriff aktiviert haben, wird der Textanalyse-Dienst unbrauchbar.
 * Wenn Sie nach dem Einrichten der privaten Endpunktverbindung die Einstellung für das Netzwerk des Azure Cognitive Search-Diensts in „Öffentlich“ ändern, können Sie den Textanalyse-Dienst nicht verwenden. Sie müssen für das Netzwerk des Azure Search-Diensts die Einstellung „Privat“ verwenden, damit die private Endpunktverbindung funktioniert.
