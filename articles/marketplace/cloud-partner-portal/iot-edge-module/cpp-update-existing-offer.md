---
title: Aktualisieren eines vorhandenen Azure IoT Edge-Modulangebots | Azure Marketplace
description: Hier wird erläutert, wie Sie ein vorhandenes IoT Edge-Modulangebot im Azure Marketplace aktualisieren.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: dsindona
ms.openlocfilehash: 019711f35d249e6684013feddb6b453d509f66d4
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "80985353"
---
# <a name="update-an-existing-iot-edge-module-offer"></a>Aktualisieren eines vorhandenen IoT Edge-Modulangebots

>[!Important]
>Ab dem 13. April 2020 beginnen wir mit der Umstellung der Verwaltung Ihrer IoT Edge-Modulangebote auf Partner Center. Nach der Migration erstellen und verwalten Sie Ihre Angebote im Partner Center. Befolgen Sie zum Verwalten Ihrer migrierten Angebote die Anweisungen unter [Erstellen eines IoT Edge-Modulangebots](https://aka.ms/AzureCreateIoT).

In diesem Artikel werden die Schritte der verschiedenen Aspekte beim Aktualisieren Ihres IoT Edge-Modulangebots im [Cloud-Partnerportal](https://cloudpartner.azure.com/) und beim anschließenden erneuten Veröffentlichen des Angebots erläutert.

Sie könnten Ihr Angebot beispielsweise aus den folgenden Gründen aktualisieren:

-  Hinzufügen einer neuen IoT Edge-Modulimageversion zu vorhandenen SKUs
-  Hinzufügen von neuen SKUs
-  Aktualisieren von Marketplace-Metadaten für das Angebot oder einzelne SKUs

Um Ihnen bei diesen Änderungen zu helfen, bietet das Portal die Features **Vergleichen** und **Verlauf**.  


## <a name="unpermitted-changes-to-iot-edge-module-offer-or-sku"></a>Nicht zulässige Änderungen am IoT Edge-Modulangebot oder der SKU

Es gibt Attribute eines IoT Edge-Modulangebots oder einer SKU, die nicht geändert werden können, nachdem das Angebot im Azure Marketplace live geschaltet wurde. Sie können die folgenden Einstellungen nicht ändern:

-  **Angebots-ID** und **Herausgeber-ID** des Angebots
-  **SKU-ID** von vorhandenen SKUs
-  Versionstags, z.B.: `1.0.1`
-  Abrechnungs-/Lizenzmodelländerungen an vorhandenen SKUs

## <a name="common-update-operations"></a>Gängige Aktualisierungsvorgänge

Die folgenden Aktualisierungsvorgänge werden häufig durchgeführt.

### <a name="update-the-iot-edge-module-image-version-for-a-sku"></a>Aktualisieren der IoT Edge-Modulimageversion für eine SKU

Ein IoT Edge-Modulimage wird üblicherweise regelmäßig mit Sicherheitspatches, zusätzlichen Features usw. aktualisiert. In diesem Szenario aktualisieren Sie mithilfe der folgenden Schritte das IoT Edge-Modulimage, das Ihre SKU referenziert:

1.  Melden Sie sich beim [Cloud-Partnerportal](https://cloudpartner.azure.com/) an.

2.  Suchen Sie unter **Alle Angebote** nach dem Angebot, das Sie aktualisieren möchten.

3.  Wählen Sie auf der Registerkarte **SKUs** die SKU aus, die mit dem zu aktualisierenden IoT Edge-Modulimage verknüpft ist.

4.  Wählen Sie unter **Edge-Modulimage** die Option **+ Neue Imageversion** aus, um ein neues IoT Edge-Modulimage hinzuzufügen.

5.  Stellen Sie die neuen **Imageversionen** für das IoT Edge-Modul bereit. Für die Imageversion müssen die gleichen Tagrichtlinien wie für frühere Versionen gelten. Versionstags sollten im Format „X.Y.Z“ angegeben werden. „X“, „Y“ und „Z“ sind dabei ganze Zahlen. Stellen Sie sicher, dass die neue Version, die Sie bereitstellen, größer als alle früheren Versionen ist.

6.  Wählen Sie **Veröffentlichen** aus, um den Workflow zum Veröffentlichen Ihrer neuen IoT Edge-Modulversion im Azure Marketplace zu starten.

### <a name="add-a-new-sku"></a>Hinzufügen einer neuen SKU

Führen Sie die folgenden Schritte aus, um eine neue SKU für Ihr Angebot zur Verfügung zu stellen: 

1.  Melden Sie sich beim [Cloud-Partnerportal](https://cloudpartner.azure.com/) an.

2.  Suchen Sie unter **Alle Angebote** nach dem Angebot, das Sie aktualisieren möchten.

3.  Wählen Sie auf der Registerkarte **SKUs** die Option **Neue SKU hinzufügen** aus, und geben Sie im Popupfenster eine **SKU-ID** an.

4.  Veröffentlichen Sie das IoT Edge-Modul mit den Schritten unter [Veröffentlichen eines IoT Edge-Moduls im Azure Marketplace](./cpp-publish-offer.md) erneut.

5.  Wählen Sie **Veröffentlichen** aus, um den Workflow zum Veröffentlichen Ihrer neuen SKU zu starten.


### <a name="update-offer-marketplace-metadata"></a>Aktualisieren der Marketplace-Metadaten eines Angebots

Führen Sie die folgenden Schritte aus, um die mit Ihrem Angebot verknüpften Marketplace-Metadaten zu aktualisieren. (Beispiel: Firmenname, Logos usw.)

1.  Melden Sie sich beim [Cloud-Partnerportal](https://cloudpartner.azure.com/) an.

2.  Suchen Sie unter **Alle Angebote** nach dem Angebot, das aktualisiert werden soll.

3.  Wechseln Sie zur Registerkarte **Marketplace**. Verwenden Sie die Anweisungen im Artikel [Veröffentlichen eines IoT Edge-Moduls im Azure Marketplace](./cpp-publish-offer.md), um Änderungen an Metadaten vorzunehmen.

4.  Wählen Sie **Veröffentlichen** aus, um den Workflow zum Veröffentlichen Ihrer Änderungen zu starten.

## <a name="compare-feature"></a>Vergleichen-Funktion

Wenn Sie Änderungen an einem veröffentlichten Angebot vornehmen, können Sie die Funktion **Vergleichen** verwenden, um die vorgenommenen Änderungen zu prüfen. 

**So verwenden Sie die Funktion „Vergleichen“**

1.  Wählen Sie zu einem beliebigen Zeitpunkt im Bearbeitungsprozess **Vergleichen** für Ihr Angebot aus.

    ![Schaltfläche des Features „Vergleichen“](./media/iot-edge-module-compare.png)


2.  Sehen Sie sich die Versionen der Marketingressourcen und Metadaten nebeneinander an.


## <a name="history-of-publishing-actions"></a>Verlauf der Veröffentlichungsaktionen

Um bereits ausgeführte Veröffentlichungsaktivitäten anzuzeigen, wählen Sie im Cloud-Partnerportal auf der linken Navigationsmenüleiste die Registerkarte **Verlauf** aus. Sie sehen mit Zeitstempeln versehene Aktionen, die während der Lebensdauer Ihrer Azure Marketplace-Angebote ausgeführt wurden.  <!-- Need to find correct link here:  legal time windowsFor more information, see [History page](cpp-history-page.md) -->
