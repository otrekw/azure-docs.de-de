---
title: Microsoft Azure Data Box – Selbst verwalteter Versand | Microsoft-Dokumentation
description: Beschreibt den Workflow des selbst verwalteten Versands für Azure Data Box-Geräte.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: how-to
ms.date: 07/22/2021
ms.author: alkohli
ms.openlocfilehash: cafea36f45dcc063fadc1562428735ee0ac9fe20
ms.sourcegitcommit: 6f21017b63520da0c9d67ca90896b8a84217d3d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2021
ms.locfileid: "114652818"
---
# <a name="use-self-managed-shipping-for-azure-data-box-in-the-azure-portal"></a>Verwenden des selbst verwalteten Versands für Azure Data Box im Azure-Portal

In diesem Artikel werden die Aufgaben beim selbst verwalteten Versand (Bestellung, Abholung und Abgabe) eines Azure Data Box-Geräts beschrieben. Sie können das Data Box-Gerät über das Azure-Portal verwalten.

> [!NOTE]
> Antworten auf häufig gestellte Fragen zu Data Box-Bestellungen und -Lieferungen finden Sie unter [Häufig gestellte Fragen zu Data Box](data-box-faq.yml).

## <a name="prerequisites"></a>Voraussetzungen

Der selbst verwaltete Versand ist als Option verfügbar, wenn Sie [Azure Data Box bestellen](data-box-deploy-ordered.md). Der selbst verwaltete Versand ist nur in folgenden Regionen verfügbar:

* US Government
* United Kingdom
* Europa, Westen
* Japan
* Singapur
* Südkorea
* Indien
* Südafrika
* Australien
* Brasilien

## <a name="use-self-managed-shipping"></a>Verwenden des selbst verwalteten Versands

Beim Aufgeben einer Data Box-Bestellung können Sie die Option für selbst verwalteten Versand auswählen.

1. Wählen Sie in Ihrer Azure Data Box-Bestellung unter den **Kontaktdetails**  den Eintrag **+ Lieferadresse hinzufügen** aus.
 
   ![„Selbstverwalteter Versand“: „Lieferadresse hinzufügen“](media\data-box-portal-customer-managed-shipping\choose-self-managed-shipping-1.png)

2. Wählen Sie bei der Wahl der Versandart die Option **Selbstverwalteter Versand** aus. Diese Option ist nur verfügbar, wenn Sie sich in einer unterstützten Region befinden (siehe die vorstehenden „Voraussetzungen“).

3. Nachdem Sie Ihre Lieferadresse angegeben haben, müssen Sie sie überprüfen und dann Ihre Bestellung abschließen.

   ![„Selbstverwalteter Versand“: Adresse überprüfen und hinzufügen](media\data-box-portal-customer-managed-shipping\choose-self-managed-shipping-2.png)

4. Sobald das Gerät vorbereitet ist und Sie eine E-Mail-Benachrichtigung dafür erhalten, können Sie eine Abholung planen.

   Wechseln Sie in Ihrer Azure Data Box-Bestellung zu **Übersicht**, und wählen Sie **Abholung planen** aus.

   ![Data Box-Auftrag: Option „Abholung planen“](media\data-box-portal-customer-managed-shipping\data-box-portal-schedule-pickup-01.png)

5. Befolgen Sie die Anweisungen unter **Abholung für Azure planen**.

   Bevor Sie Ihren Autorisierungscode abrufen können, müssen Sie eine E-Mail an [adbops@microsoft.com](mailto:adbops@microsoft.com) senden, um die Abholung des Geräts aus dem Rechenzentrum Ihrer Region zu planen.

   ![Anweisungen zu „Abholung für Azure planen“](media\data-box-portal-customer-managed-shipping\data-box-portal-schedule-pickup-email-01.png)

   **Anweisungen für Brasilien:** Wenn Sie die Abholung eines Geräts in Brasilien planen, fügen Sie die folgenden Informationen in Ihre E-Mail ein. Das Rechenzentrum wird die Abholung planen, nachdem es eine eingehende `Nota Fiscal` empfangen hat. Dies kann bis zu vier Werktage dauern.

   ```
   Subject: Request Azure Data Box Disk pickup for order: <ordername>

   - Order name
   - Company name
   - Company legal name (if different) 
   - CNPJ (Business Tax ID, format: 00.000.000/0000-00) or CPF (Individual Tax ID, format: 000.000.000-00)
   - Address
   - Country 
   - Phone number 
   - Contact name of the person who will pick up the Data Box Disk (A government-issued photo ID will be required to validate the contact’s identity upon arrival.)   
   ```

6. Nachdem Sie die Abholung Ihres Geräts geplant haben, können Sie Ihren Geräteautorisierungscode im Bereich **Abholung für Azure planen** anzeigen.

   ![Anzeigen Ihres Geräteautorisierungscodes](media\data-box-portal-customer-managed-shipping\data-box-portal-auth-01b.png)

   Notieren Sie sich diesen **Autorisierungscode**. Die Person, die das Gerät abholt, muss ihn vorlegen.

   Gemäß den Sicherheitsanforderungen müssen bei der Planung der Abholung unbedingt der Name und weitere Informationen zu der Person angegeben werden, die das Gerät abholen wird. Sie oder Ihre Kontaktperson müssen einen amtlichen Lichtbildausweis mit sich führen, der im Rechenzentrum überprüft wird.

7. Holen Sie die Data Box zum geplanten Zeitpunkt im Rechenzentrum ab.

   Die Person, die das Gerät abholt, muss Folgendes mitbringen:

   * Eine Kopie der E-Mail-Bestätigung für den Besuch des Rechenzentrums von Microsoft Operations.

   * Den Autorisierungscode. Die Referenznummer ist für eine Abholung oder Abgabe eindeutig und wird im Rechenzentrum überprüft.

   * Einen amtlichen Lichtbildausweis. Die ID wird im Rechenzentrum überprüft, und der Name und die Details der Person, die das Gerät abholt, müssen beim Planen der Abholung angegeben werden.

   > [!NOTE]
   > Wenn ein geplanter Termin verpasst wird, müssen Sie einen neuen Termin vereinbaren.

8. Sobald das Gerät im Rechenzentrum abgeholt wurde, ändert sich der Status Ihrer Bestellung automatisch in **Abgeholt**.

    ![Ein Auftrag mit dem Zustand „Abgeholt“](media\data-box-portal-customer-managed-shipping\data-box-portal-picked-up-boxed-01.png)

9. Nachdem das Gerät abgeholt wurde, kopieren Sie die Daten in die Data Box an Ihrem Standort. Nach Abschluss des Datenkopiervorgangs können Sie die Data Box für den Versand vorbereiten. Weitere Informationen finden Sie unter [Für den Versand vorbereiten](data-box-deploy-picked-up.md#prepare-to-ship).

   Der Schritt **Versandvorbereitung** muss ohne kritische Fehler abgeschlossen werden. Andernfalls müssen Sie diesen Schritt wiederholen, nachdem Sie die erforderlichen Korrekturen vorgenommen haben. Nach erfolgreichem Abschluss des Schritts **Versandvorbereitung** können Sie den Autorisierungscode für die Abgabe auf der lokalen Benutzeroberfläche anzeigen.

   > [!NOTE]
   > Versenden Sie den Autorisierungscode nicht per E-Mail. Dieser muss im Datacenter nämlich nur während der Abgabe überprüft werden.

   **Anweisungen für Brasilien:** Um die Rückgabe eines Geräts in Brasilien zu planen, senden Sie eine E-Mail mit den folgenden Informationen an [adbops@microsoft.com](mailto:adbops@microsoft.com):

   ```
   Subject: Request Azure Data Box Disk drop-off for order: <ordername>

   - Order name
   - Contact name of the person who will drop off the Data Box Disk (A government-issued photo ID will be required to validate the contact’s identity upon arrival.) 
   - Inbound Nota Fiscal (A copy of the inbound Nota Fiscal will be required at drop-off.)   
   ```

10. Wenn Sie einen Termin für die Abgabe erhalten haben, sollte der Status der Bestellung im Azure-Portal **Bereit für den Empfang im Azure-Rechenzentrum** lauten. Befolgen Sie die Anweisungen unter **Abgabe planen**, um das Gerät zurückzusenden.

    ![Anweisungen für Geräteabgabe](media\data-box-portal-customer-managed-shipping\data-box-portal-received-complete-02b.png)

11. Nachdem Ihre ID und Ihr Autorisierungscode überprüft wurden und Sie das Gerät im Rechenzentrum abgegeben haben, sollte der Auftragsstatus **Empfangen** lauten.

    ![Ein Auftrag mit dem Status „Empfangen“](media\data-box-portal-customer-managed-shipping\data-box-portal-received-complete-01.png)

12. Nachdem das Gerät empfangen wurde, wird der Datenkopiervorgang fortgesetzt. Nach Beendigung des Kopiervorgangs ist die Bestellung abgeschlossen.

## <a name="next-steps"></a>Nächste Schritte

* [Erste Schritte mit Azure Data Box](data-box-quickstart-portal.md)
