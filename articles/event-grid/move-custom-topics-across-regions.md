---
title: Verschieben von benutzerdefinierten Azure Event Grid-Themen in eine andere Region
description: In diesem Artikel erfahren Sie, wie Sie benutzerdefinierte Azure Event Grid-Themen aus der aktuellen Region in eine andere Region verschieben.
ms.topic: how-to
ms.custom: subject-moving-resources
ms.date: 08/28/2020
ms.openlocfilehash: d0656a4f6ec1c7431cf7111f786b0f1d779166e3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89145337"
---
# <a name="move-azure-event-grid-custom-topics-to-another-region"></a>Verschieben von benutzerdefinierten Azure Event Grid-Themen in eine andere Region
Sie möchten Ihre Ressourcen ggf. aus verschiedenen Gründen in eine andere Region verschieben. Beispielsweise, um eine neue Azure-Region zu nutzen, interne Richtlinien- und Governanceanforderungen zu erfüllen oder als Reaktion auf Anforderungen der Kapazitätsplanung. 

Folgende allgemeine Schritte werden in diesem Artikel behandelt: 

- **Exportieren der benutzerdefinierten Themaressource** in eine Azure Resource Manager-Vorlage. 

    > [!IMPORTANT]
    > Nur das benutzerdefinierte Thema wird in die Vorlage exportiert. Alle Abonnements für das Thema werden nicht exportiert.
- **Verwenden der Vorlage zum Bereitstellen des benutzerdefinierten Themas** in der Zielregion. 
- **Manuelles Erstellen von Abonnements** in der Zielregion. Wenn Sie das benutzerdefinierte Thema in eine Vorlage in der aktuellen Region exportiert haben, wird nur das Thema exportiert. Abonnements sind nicht in der Vorlage enthalten. Erstellen Sie sie daher manuell, nachdem das benutzerdefinierte Thema in der Zielregion erstellt wurde. 
- **Überprüfen der Bereitstellung**. Vergewissern Sie sich, dass das benutzerdefinierte Thema in der Zielregion erstellt wurde. 
- Um den **Verschiebevorgang abzuschließen**, löschen Sie das benutzerdefinierte Thema aus der Quellregion. 

## <a name="prerequisites"></a>Voraussetzungen
- Schließen Sie den [Schnellstart: Weiterleiten von benutzerdefinierten Ereignissen an einen Webendpunkt](custom-event-quickstart-portal.md) in der Quellregion ab. Führen Sie diesen Schritt aus, um die Schritte in diesem Artikel zu testen. 
- Stellen Sie sicher, dass der Event Grid-Dienst in der Zielregion verfügbar ist. Weitere Informationen finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=event-grid&regions=all).

## <a name="prepare"></a>Vorbereiten
Exportieren Sie zunächst eine Resource Manager-Vorlage für das benutzerdefinierte Thema. 

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) an.
2. Geben Sie in der Suchleiste **Event Grid-Themen** ein, und wählen Sie dann **Event Grid-Themen** aus der Ergebnisliste aus. 

    :::image type="content" source="./media/move-custom-topics-across-regions/search-topics.png" alt-text="Suchen nach und Auswählen von Event Grid-Themen":::
3. Wählen Sie das **Thema** aus, das Sie in eine Resource Manager-Vorlage exportieren möchten. 

    :::image type="content" source="./media/move-custom-topics-across-regions/select-custom-topic.png" alt-text="Suchen nach und Auswählen von Event Grid-Themen":::   
4. Wählen Sie auf der Seite **Event Grid-Thema** die Option **Vorlage exportieren** unter **Einstellungen** im linken Menü aus, und wählen Sie dann **Herunterladen** auf der Symbolleiste aus. 

    :::image type="content" source="./media/move-custom-topics-across-regions/export-template-download.png" alt-text="Suchen nach und Auswählen von Event Grid-Themen"
    ```
1. **Speichern** Sie die Vorlage. 

## <a name="recreate"></a>Neu erstellen 
Stellen Sie die Vorlage zum Erstellen eines benutzerdefinierten Themas in der Zielregion bereit. 

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**.
2. Geben Sie in **Marketplace durchsuchen** den Begriff **Vorlagenbereitstellung** ein, und drücken Sie dann die **EINGABETASTE**.
3. Wählen Sie **Vorlagenbereitstellung** aus.
4. Klicken Sie auf **Erstellen**.
5. Wählen Sie **Eigene Vorlage im Editor erstellen**.
6. Wählen Sie **Datei laden** aus, und befolgen Sie dann die Anweisungen zum Laden der Datei **template.json**, die Sie im vorherigen Abschnitt heruntergeladen haben.
7. Wählen Sie **Speichern** aus, um die Vorlage zu speichern. 
8. Gehen Sie auf der Seite **Benutzerdefinierte Bereitstellung** wie folgt vor: 
    1. Wählen Sie ein Azure-**Abonnement** aus. 
    1. Wählen Sie eine vorhandene **Ressourcengruppe** in der Zielregion aus, oder erstellen Sie eine Ressourcengruppe. 
    1. Wählen als **Region** die Zielregion aus. Wenn Sie eine vorhandene Ressourcengruppe ausgewählt haben, ist diese Einstellung schreibgeschützt. 
    1. Geben Sie als **Themennamen** einen neuen Namen für das Thema ein. 
    1. Wählen Sie am unteren Rand der Seite die Option **Bewerten + erstellen** aus. 
    
        :::image type="content" source="./media/move-custom-topics-across-regions/deploy-template.png" alt-text="Suchen nach und Auswählen von Event Grid-Themen":::
    1. Überprüfen Sie die Einstellungen auf der Seite **Überprüfen und erstellen**, und wählen Sie **Erstellen** aus. 

## <a name="verify"></a>Überprüfung

1. Wählen Sie nach der erfolgreichen Bereitstellung **Zu Ressource wechseln** aus. 

    :::image type="content" source="./media/move-custom-topics-across-regions/navigate-custom-topic.png" alt-text="Suchen nach und Auswählen von Event Grid-Themen":::
1. Vergewissern Sie sich, dass die Seite **Event Grid-Thema** für das benutzerdefinierte Thema angezeigt wird.   
1. Führen Sie die Schritte unter [Weiterleiten von benutzerdefinierten Ereignissen an einen Webendpunkt](custom-event-quickstart-portal.md#send-an-event-to-your-topic) aus, um Ereignisse an das Thema zu senden. Bestätigen Sie, dass der Webhookereignishandler aufgerufen wird. 

## <a name="discard-or-clean-up"></a>Verwerfen oder Bereinigen
Um den Verschiebevorgang abzuschließen, löschen Sie das benutzerdefinierte Thema in der Quellregion.  

Wenn Sie von vorn beginnen möchten, löschen Sie das Thema in der Zielregion, und wiederholen Sie die Schritte in den Abschnitten [Vorbereiten](#prepare) und [Neu erstellen](#recreate) dieses Artikels.

So löschen Sie ein benutzerdefiniertes Themas über das Azure-Portal:

1. Geben Sie im Suchfenster oben im Azure-Portal **Event Grid-Themen** ein, und wählen Sie in den Suchergebnissen **Event Grid-Themen** aus. 
2. Wählen Sie das zu löschende Thema und dann auf der Symbolleiste **Löschen** aus. 
3. Geben Sie auf der Bestätigungsseite den Namen der Ressourcengruppe ein, und wählen Sie **Löschen** aus.  

So löschen Sie die Ressourcengruppe, die das benutzerdefinierte Thema enthält, mithilfe des Azure-Portals:

1. Geben Sie im Suchfenster oben im Azure-Portal **Ressourcengruppen** ein, und wählen Sie in den Suchergebnissen **Ressourcengruppen** aus. 
2. Wählen Sie die zu löschende Ressourcengruppe und dann auf der Symbolleiste **Löschen** aus. 
3. Geben Sie auf der Bestätigungsseite den Namen der Ressourcengruppe ein, und wählen Sie **Löschen** aus.  

## <a name="next-steps"></a>Nächste Schritte
Sie haben gelernt, wie ein benutzerdefiniertes Event Grid-Thema aus einer Region in eine andere Region verschoben wird. In den folgenden Artikeln finden Sie Informationen zum regionsübergreifenden Verschieben von Systemthemen, Domänen und Partnernamespaces.

- [Regionsübergreifendes Verschieben von Systemthemen](move-system-topics-across-regions.md). 
- [Regionsübergreifendes Verschieben von Domänen](move-domains-across-regions.md). 
- [Regionsübergreifendes Verschieben von Partnernamespaces](move-partner-namespaces-across-regions.md).

Weitere Informationen zum Verschieben von Ressourcen zwischen Regionen und zur Notfallwiederherstellung in Azure finden Sie im folgenden Artikel: [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../azure-resource-manager/management/move-resource-group-and-subscription.md).