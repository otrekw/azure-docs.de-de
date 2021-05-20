---
title: Microsoft Azure Data Box Disk – Selbst verwalteter Versand | Microsoft-Dokumentation
description: Beschreibt den Workflow des selbst verwalteten Versands für Azure Data Box Disk-Geräte.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: how-to
ms.date: 05/08/2021
ms.author: alkohli
ms.openlocfilehash: 6323a077596a7ed13384bfbaace929b270c2d3f5
ms.sourcegitcommit: 1b19b8d303b3abe4d4d08bfde0fee441159771e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2021
ms.locfileid: "109754477"
---
# <a name="use-self-managed-shipping-for-azure-data-box-disk-in-the-azure-portal"></a>Verwenden des selbst verwalteten Versands für Azure Data Box Disk im Azure-Portal

In diesem Artikel werden die Aufgaben beim selbst verwalteten Versand (Bestellung, Abholung und Abgabe) von Azure Data Box Disk beschrieben. Sie können Data Box Disk über das Azure-Portal verwalten.

## <a name="prerequisites"></a>Voraussetzungen

Der selbst verwaltete Versand ist als Option verfügbar, wenn Sie [Azure Data Box Disk bestellen](data-box-disk-deploy-ordered.md). Der selbst verwaltete Versand ist nur in folgenden Regionen verfügbar:

* US Government
* United Kingdom
* Europa, Westen
* Australien
* Japan
* Singapur
* Südkorea
* Südafrika
* Indien (Vorschau)
* Brasilien

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

   **Anweisungen für Brasilien:** Wenn Sie die Abholung eines Geräts in Brasilien planen, fügen Sie die folgenden Informationen in Ihre E-Mail ein. Das Rechenzentrum wird die Abholung planen, nachdem es eine eingehende `Nota Fiscal` empfangen hat. Dies kann bis zu vier Werktage dauern.

   ```
   Subject: Request Azure Data Box Disk pickup for order: <ordername>

   - Order name
   - Company name
   - Company legal name (if different) 
   - Tax ID 
   - Address 
   - Country 
   - Phone number 
   - Contact name of the person who will pick up the Data Box Disk (A government-issued photo ID will be required to validate the contact’s identity upon arrival.)   
   ```

6. Nachdem Sie die Abholung Ihres Geräts geplant haben, können Sie Ihren Autorisierungscode in **Abholung für Azure planen** anzeigen.

   ![Screenshot des Dialogfelds „Abholtermin für Azure vereinbaren“ mit hervorgehobenem Textfeld „Abholautorisierungscode“.](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-authcode-01b.png)

   Notieren Sie sich diesen Autorisierungscode. Die Person, die das Gerät abholt, muss über ihn verfügen.

   Gemäß den Sicherheitsanforderungen müssen bei der Planung der Abholung unbedingt der Name und weitere Informationen zu der Person angegeben werden, die das Gerät abholen wird. Sie oder Ihre Kontaktperson müssen einen amtlichen Lichtbildausweis mit sich führen, der im Rechenzentrum überprüft wird.

7. Holen Sie die Data Box Disk zum geplanten Zeitpunkt im Rechenzentrum ab.

   Die Person, die das Gerät abholt, muss Folgendes mitbringen:

   * Eine Kopie der E-Mail-Bestätigung für den Besuch des Rechenzentrums von Microsoft Operations.

   * Den Autorisierungscode. Die Referenznummer ist für eine Abholung oder Abgabe eindeutig und wird im Rechenzentrum überprüft.

   * Einen amtlichen Lichtbildausweis. Die ID wird im Rechenzentrum überprüft, und der Name und die Details der Person, die das Gerät abholt, müssen beim Planen der Abholung angegeben werden.

   > [!NOTE]
   > Wenn ein geplanter Termin verpasst wird, müssen Sie einen neuen Termin vereinbaren.

8. Nachdem das Gerät im Rechenzentrum abgeholt wurde, ändert sich der Status Ihrer Bestellung automatisch in **Abgeholt**.

   ![Abgeholt](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-ready-disk-01b.png)

9. Nachdem das Gerät abgeholt wurde, können Sie die Daten in die Data Box Disk(s) an Ihrem Standort kopieren. Nach Abschluss des Datenkopiervorgangs können Sie die Data Box Disk für den Versand vorbereiten.

   Nachdem Sie die Datenkopie abgeschlossen haben, wenden Sie sich an den Betrieb (Operations), um einen Termin für die Abholung zu planen. Sie müssen die Details der Person freigeben, die für die Abgabe der Disk(s) in das Rechenzentrum kommen wird. Im Rechenzentrum muss bei der Abgabe auch der Autorisierungscode überprüft werden. Sie finden den Autorisierungscode für die Abgabe im Azure-Portal unter **Abgabe planen**.

   > [!NOTE]
   > Versenden Sie den Autorisierungscode nicht per E-Mail. Er muss im Rechenzentrum nämlich nur während der Abgabe überprüft werden.

   **Anweisungen für Brasilien:** Um die Rückgabe eines Geräts in Brasilien zu planen, senden Sie eine E-Mail mit den folgenden Informationen an [adbops@microsoft.com](mailto:adbops@microsoft.com):

   ```
   Subject: Request Azure Data Box Disk drop-off for order: <ordername>

   - Order name
   - Contact name of the person who will drop off the Data Box Disk (A government-issued photo ID will be required to validate the contact’s identity upon arrival.) 
   - Inbound Nota Fiscal (A copy of the inbound Nota Fiscal will be required at drop-off.)   
   ```

10. Nachdem Sie einen Termin für die Abgabe erhalten haben, sollte der Status der Bestellung im Azure-Portal **Bereit für den Empfang im Azure-Rechenzentrum** lauten.

    ![Screenshot des Dialogfelds „Lieferadresse hinzufügen“ mit Hervorhebung der gewünschten Versandoption und der Option „Lieferadresse hinzufügen“](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-authcode-dropoff-02b.png)

11. Nachdem Ihre ID und Ihr Autorisierungscode überprüft wurden und Sie das Gerät im Rechenzentrum abgegeben haben, sollte der Auftragsstatus **Empfangen** lauten.

    ![Empfang abgeschlossen](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-received-01a.png)

11. Nachdem das Gerät empfangen wurde, wird der Datenkopiervorgang fortgesetzt. Nach Beendigung des Kopiervorgangs ist die Bestellung abgeschlossen.

## <a name="next-steps"></a>Nächste Schritte

* [Schnellstart: Bereitstellen von Azure Data Box Disk über das Azure-Portal](data-box-disk-quickstart-portal.md)
