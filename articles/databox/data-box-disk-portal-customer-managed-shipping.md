---
title: Microsoft Azure Data Box Disk – Selbst verwalteter Versand | Microsoft-Dokumentation
description: Beschreibt den Workflow des selbst verwalteten Versands für Azure Data Box Disk-Geräte.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: how-to
ms.date: 05/20/2020
ms.author: alkohli
ms.openlocfilehash: c4b3479e5728c32e66bc40f950bc948bf61dce42
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91575162"
---
# <a name="use-self-managed-shipping-for-azure-data-box-disk-in-the-azure-portal"></a>Verwenden des selbst verwalteten Versands für Azure Data Box Disk im Azure-Portal

In diesem Artikel werden die Aufgaben beim selbst verwalteten Versand (Bestellung, Abholung und Abgabe) von Azure Data Box Disk beschrieben. Sie können Data Box Disk über das Azure-Portal verwalten.

## <a name="prerequisites"></a>Voraussetzungen

Der selbst verwaltete Versand ist als Option verfügbar, wenn Sie [Azure Data Box Disk bestellen](data-box-disk-deploy-ordered.md). Der selbst verwaltete Versand ist nur in folgenden Regionen verfügbar:

* US Government
* Europa, Westen
* Japan
* Singapur
* Südkorea
* Südafrika
* Indien (Vorschau)

## <a name="use-self-managed-shipping"></a>Verwenden des selbst verwalteten Versands

Beim Aufgeben einer Data Box Disk-Bestellung können Sie die Option für selbst verwalteten Versand wählen.

1. Wählen Sie in Ihrer Azure Data Box Disk-Bestellung unter den **Kontaktdetails**  den Eintrag **+ Lieferadresse hinzufügen** aus.

   ![Screenshot des Auftrags-Assistenten mit dem Schritt „Kontaktdetails“ und hervorgehobener Option „Lieferadresse hinzufügen“](media\data-box-portal-customer-managed-shipping\choose-self-managed-shipping-1.png)

2. Wählen Sie bei der Wahl des Versandtyps die Option **Selbst verwalteter Versand** aus. Diese Option ist nur verfügbar, wenn Sie sich in einer unterstützten Region befinden (siehe die vorstehenden „Voraussetzungen“).

3. Nachdem Sie Ihre Lieferadresse angegeben haben, müssen Sie sie überprüfen und dann Ihre Bestellung abschließen.

   ![Screenshot: Dialogfeld „Lieferadresse hinzufügen“ mit Hervorhebung der gewünschten Versandoption und der Option „Lieferadresse hinzufügen“](media\data-box-portal-customer-managed-shipping\choose-self-managed-shipping-2.png)

4. Sobald das Gerät vorbereitet ist und Sie eine E-Mail-Benachrichtigung erhalten haben, können Sie eine Abholung planen. Wechseln Sie in Ihrer Azure Data Box Disk-Bestellung zu **Übersicht**, und wählen Sie **Abholung planen** aus.

   ![Bestellen eines Data Box-Geräts zur Abholung](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-user-pickup-01b.png)

5. Befolgen Sie die Anweisungen unter **Abholung für Azure planen**. Bevor Sie Ihren Autorisierungscode abrufen können, müssen Sie eine E-Mail an [adbops@microsoft.com](mailto:adbops@microsoft.com) senden, um die Abholung des Geräts aus dem Rechenzentrum Ihrer Region zu planen.

   ![Abholung beauftragen](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-user-pickup-02c.png)

6. Nachdem Sie die Abholung Ihres Geräts geplant haben, können Sie Ihren Autorisierungscode in **Abholung für Azure planen** anzeigen.

   ![Screenshot des Dialogfelds „Abholtermin für Azure vereinbaren“ mit hervorgehobenem Textfeld „Abholautorisierungscode“.](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-authcode-01b.png)

   Notieren Sie sich diesen **Autorisierungscode**.

   Gemäß den Sicherheitsanforderungen muss bei der Planung der Abholung der Name der Person angegeben werden, die das Gerät abholen wird.

   Darüber hinaus müssen Sie Details zu der Person angeben, die zur Abholung in das Rechenzentrum kommen wird. Sie oder Ihre Kontaktperson müssen einen amtlichen Lichtbildausweis mit sich führen, der im Rechenzentrum überprüft wird.

   Außerdem muss die Person, die das Gerät abholt, ebenfalls über den **Autorisierungscode** verfügen. Der Autorisierungscode ist für eine Abholung oder Abgabe eindeutig und wird im Rechenzentrum überprüft.

7. Sobald das Gerät im Rechenzentrum abgeholt wurde, ändert sich der Status Ihrer Bestellung automatisch in **Abgeholt**.

   ![Abgeholt](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-ready-disk-01b.png)

8. Nachdem das Gerät abgeholt wurde, können Sie die Daten in die Data Box Disk(s) an Ihrem Standort kopieren. Nach Abschluss des Datenkopiervorgangs können Sie die Data Box Disk für den Versand vorbereiten.

   Nachdem Sie den Datenkopiervorgang beendet haben, müssen Sie sich an den Betrieb wenden, um einen Termin für die Abgabe zu planen. Sie müssen die Details der Person mitteilen, die für die Abgabe der Disk(s) in das Rechenzentrum kommen wird. Im Rechenzentrum muss bei der Abgabe auch der Autorisierungscode überprüft werden. Der Autorisierungscode für die Abgabe ist im Azure-Portal unter **Abgabe planen** verfügbar.

   > [!NOTE]
   > Versenden Sie den Autorisierungscode nicht per E-Mail. Dieser muss im Datacenter nämlich nur während der Abgabe überprüft werden.

9. Wenn Sie einen Termin für die Abgabe erhalten haben, sollte der Status der Bestellung im Azure-Portal jetzt **Bereit für den Empfang im Azure-Rechenzentrum** lauten.

   ![Screenshot des Dialogfelds „Lieferadresse hinzufügen“ mit Hervorhebung der gewünschten Versandoption und der Option „Lieferadresse hinzufügen“](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-authcode-dropoff-02b.png)

10. Nachdem Ihre ID und Ihr Autorisierungscode überprüft wurden und Sie das Gerät im Rechenzentrum abgegeben haben, sollte der Auftragsstatus **Empfangen** lauten.

    ![Empfang abgeschlossen](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-received-01a.png)

11. Nachdem das Gerät empfangen wurde, wird der Datenkopiervorgang fortgesetzt. Nach Beendigung des Kopiervorgangs ist die Bestellung abgeschlossen.

## <a name="next-steps"></a>Nächste Schritte

* [Schnellstart: Bereitstellen von Azure Data Box Disk über das Azure-Portal](data-box-disk-quickstart-portal.md)
