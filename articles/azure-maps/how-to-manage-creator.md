---
title: Verwalten von Microsoft Azure Maps Creator (Vorschau)
description: In diesem Artikel erfahren Sie, wie Sie Microsoft Azure Maps Creator (Vorschau) verwalten.
author: anastasia-ms
ms.author: v-stharr
ms.date: 02/16/2021
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: d26df4287032bc59cc58dd1d832d9d5a9c40afcd
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100559217"
---
# <a name="manage-azure-maps-creator-preview"></a>Verwalten von Azure Maps Creator (Vorschau) 

> [!IMPORTANT]
> Azure Maps Creator-Dienste befinden sich derzeit in der öffentlichen Vorschau.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Maps Creator ermöglicht die Erstellung privater Gebäudeplandaten. Mit der Azure Maps-API und dem Modul für Gebäudepläne können Sie interaktive und dynamische Gebäudeplan-Webanwendungen entwickeln. Aktuell steht Creator nur in den USA im Tarif S1 zur Verfügung.

In diesem Artikel erfahren Sie Schritt für Schritt, wie Sie eine Creator-Ressource in einem Azure Maps-Konto erstellen und löschen.

## <a name="create-creator-preview-resource"></a>Erstellen einer Creator-Ressource (Vorschau)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)

2. Wählen Sie Ihr Azure Maps-Konto aus. Sollte Ihr Azure Maps-Konto unter **Kürzlich verwendete Ressourcen** nicht angezeigt werden, navigieren Sie zum Azure-Portalmenü. Wählen Sie **Alle Ressourcen**. Navigieren Sie zu Ihrem Azure Maps-Konto, und wählen Sie es aus.

    ![Startseite des Azure Maps-Portals](./media/how-to-manage-creator/select-maps-account.png)

3. Navigieren Sie auf der Seite des Azure Maps-Kontos unter **Creator** zur Option **Übersicht**. Wählen Sie **Erstellen** aus, um eine Azure Maps Creator-Ressource zu erstellen.

    ![Azure Maps Creator-Erstellungsseite](./media/how-to-manage-creator/creator-blade-settings.png)

4. Geben Sie den Namen und den Standort für Ihre Creator-Ressource ein. Aktuell wird Creator nur in den USA unterstützt. Klicken Sie auf **Überprüfen + erstellen**.

   ![Seite zum Eingeben der Creator-Kontoinformationen](./media/how-to-manage-creator/creator-creation-dialog.png)

5. Überprüfen Sie die Einstellungen, und wählen Sie **Erstellen** aus.

    ![Seite zum Bestätigen der Creator-Kontoeinstellungen](./media/how-to-manage-creator/creator-create-dialog.png)

6. Nach Abschluss der Bereitstellung wird eine Seite mit einer Erfolgs- oder Fehlermeldung angezeigt.

   ![Statusseite für die Ressourcenbereitstellung](./media/how-to-manage-creator/creator-resource-created.png)

7. Wählen Sie **Zu Ressource wechseln** aus. Auf der Ansichtsseite Ihrer Creator-Ressource werden der Status Ihrer Creator-Ressource und die gewählte demografische Region angezeigt.

    ![Creator-Statusseite](./media/how-to-manage-creator/creator-resource-view.png)

   >[!NOTE]
   >Auf der Creator-Ressourcenseite können Sie wieder zum zugehörigen Azure Maps-Konto navigieren, indem Sie „Azure Maps-Konto“ auswählen.

## <a name="delete-creator-preview-resource"></a>Löschen einer Creator-Ressource (Vorschau)

Navigieren Sie zum Löschen der Creator-Ressource zu Ihrem Azure Maps-Konto. Wählen Sie unter **Creator** die Option **Übersicht** aus. Klicken Sie auf die Schaltfläche **Löschen**.

>[!WARNING]
>Wenn Sie die Creator-Ressource Ihres Azure Maps-Kontos löschen, werden auch die mit Creator-Diensten erstellten Datasets, Kachelsets und Featurezustandssets gelöscht.

![Creator-Seite mit Schaltfläche „Löschen“](./media/how-to-manage-creator/creator-delete.png)

Wählen Sie die Schaltfläche **Löschen** aus, und geben Sie Ihren Creator-Namen ein, um den Löschvorgang zu bestätigen. Nach dem Löschen der Ressource wird eine Bestätigungsseite wie die folgende angezeigt:

![Creator-Seite mit Löschbestätigung](./media/how-to-manage-creator/creator-confirm-delete.png)

## <a name="authentication"></a>Authentifizierung

Für Creator (Vorschau) gelten die Zugriffssteuerungseinstellungen (IAM) von Azure Maps. Alle API-Aufrufe für Datenzugriff müssen mit Authentifizierungs- und Autorisierungsregeln gesendet werden.

Creator-Nutzungsdaten fließen in Ihre Azure Maps-Nutzungsdiagramme und in das Aktivitätsprotokoll mit ein.  Weitere Informationen finden Sie unter [Verwalten der Authentifizierung in Azure Maps](./how-to-manage-authentication.md).

## <a name="access-to-creator-services"></a>Zugreifen auf Creator-Dienste

Die Creator-Dienste (Vorschau) und Dienste, die in Creator gehostete Daten verwenden (z. B. Renderingdienst), sind über eine geografische URL zugänglich. Die geografische URL wird durch den Standort bestimmt, der bei der Erstellung ausgewählt wird. Wenn Creator also beispielsweise an einem geografischen Standort in den USA erstellt wird, müssen alle für den Konvertierungsdienst bestimmten Aufrufe an `us.atlas.microsoft.com/conversion/convert` übermittelt werden.

Darüber hinaus sollten alle in Creator importierten Daten an den gleichen geografischen Standort hochgeladen werden, an dem sich auch die Creator-Ressource befindet. Wenn Creator also beispielsweise in den USA bereitgestellt wurde, sollten alle Rohdaten über `us.atlas.microsoft.com/mapData/upload` hochgeladen werden.

## <a name="next-steps"></a>Nächste Schritte

Einführung in Creator-Dienste (Vorschau) für Gebäudepläne:

> [!div class="nextstepaction"]
> [Datenupload](creator-indoor-maps.md#upload-a-drawing-package)

> [!div class="nextstepaction"]
> [Datenkonvertierung](creator-indoor-maps.md#convert-a-drawing-package)

> [!div class="nextstepaction"]
> [Dataset](creator-indoor-maps.md#datasets)

> [!div class="nextstepaction"]
> [Kachelset](creator-indoor-maps.md#tilesets)

> [!div class="nextstepaction"]
> [Featurezustandsset](creator-indoor-maps.md#feature-statesets)

In den folgenden Artikeln erfahren Sie, wie Sie mithilfe der Creator-Dienste (Vorschau) Gebäudepläne in Ihrer Anwendung rendern:

> [!div class="nextstepaction"]
> [Verwenden von Creator zum Erstellen von Gebäudeplänen](tutorial-creator-indoor-maps.md)

> [!div class="nextstepaction"]
> [Implementieren dynamischer Stile für Creator-Gebäudepläne](indoor-map-dynamic-styling.md)

> [!div class="nextstepaction"]
> [Verwenden des Moduls „Gebäudepläne“ von Azure Maps](how-to-use-indoor-module.md)