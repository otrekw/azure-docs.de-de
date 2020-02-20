---
title: 'Tutorial: Erstellen und Verwalten von exportierten Daten aus Azure Cost Management'
description: Dieser Artikel erläutert, wie Sie aus Azure Cost Management exportierte Daten erstellen und verwalten können, um sie in externen Systemen zu verwenden.
author: bandersmsft
ms.author: banders
ms.date: 02/12/2020
ms.topic: tutorial
ms.service: cost-management-billing
ms.reviewer: adwise
ms.custom: seodec18
ms.openlocfilehash: 40846de9ee8eec3225a05e3c2c68c3ad5af02042
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2020
ms.locfileid: "77201015"
---
# <a name="tutorial-create-and-manage-exported-data"></a>Tutorial: Erstellen und Verwalten von exportierten Daten

Wenn Sie das Tutorial zur Kostenanalyse gelesen haben, dann sind Sie bereits mit dem manuellen Herunterladen Ihrer Daten zum Cost Management vertraut. Sie können jedoch eine wiederkehrende Aufgabe erstellen, die Ihre Cost Management-Daten täglich, wöchentlich oder monatlich automatisch in Azure Storage exportiert. Die Daten werden im CSV-Format exportiert und enthalten alle Informationen, die von Cost Management gesammelt wurden. Sie können dann die exportierten Daten in Azure Storage mit externen Systemen verwenden und mit Ihren eigenen benutzerdefinierten Daten kombinieren. Darüber hinaus können Sie Ihre exportierten Daten in einem externen System wie einem Dashboard oder einem anderen Finanzsystem verwenden.

Sehen Sie sich das Video [How to schedule exports to storage with Azure Cost Management (Planen von Exporten in den Speicher mit Azure Cost Management)](https://www.youtube.com/watch?v=rWa_xI1aRzo) an. Darin wird gezeigt, wie ein geplanter Export Ihrer Azure-Kostendaten in Azure Storage erstellt wird.

Die Beispiele in diesem Tutorial zeigen Ihnen, wie Sie Ihre Cost Management-Daten exportieren und dann überprüfen, ob die Daten erfolgreich exportiert wurden.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines täglichen Exports
> * Überprüfen, ob Daten gesammelt wurden

## <a name="prerequisites"></a>Voraussetzungen
Datenexport ist für verschiedene Azure-Kontotypen einschließlich [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)-Kunden und Kunden mit [Microsoft-Kundenvereinbarung](get-started-partners.md) verfügbar. Die vollständige Liste der unterstützten Kontotypen finden Sie unter [Grundlegendes zu Cost Management-Daten](understand-cost-mgt-data.md). Die folgenden Azure-Berechtigungen oder Bereiche werden pro Abonnement für den Datenexport nach Benutzer und Gruppe unterstützt. Weitere Informationen zu Bereichen finden Sie unter [Verstehen von und Arbeiten mit Bereichen](understand-work-scopes.md).

- Besitzer – kann geplante Exporte für ein Abonnement erstellen, ändern oder löschen.
- Mitwirkende – kann eigene geplante Exporte erstellen, ändern oder löschen. Kann den Namen der von anderen Personen erstellten Exporte ändern.
- Leser – kann Exporte planen, für die er die Berechtigung hat.

Für Azure Storage-Konten:
- Um das konfigurierte Speicherkonto zu ändern, sind unabhängig von den Berechtigungen für den Export Schreibberechtigungen erforderlich.
- Ihr Azure Storage-Konto muss für Blob oder File Storage konfiguriert werden.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure
Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com/) beim Azure-Portal an.

## <a name="create-a-daily-export"></a>Erstellen eines täglichen Exports

Zum Erstellen oder Anzeigen eines Datenexports bzw. Planen eines Exports öffnen Sie den gewünschten Bereich im Azure-Portal, und wählen Sie **Kostenanalyse** im Menü aus. Navigieren Sie beispielsweise zu **Abonnements**, und wählen Sie dann ein Abonnement in der Liste und **Kostenanalyse** im Menü aus. Wählen Sie am oberen Rand der Seite „Kostenanalyse“ die Option **Exportieren** dann eine Exportoption aus. Wählen Sie beispielsweise **Export planen** aus.  

> [!NOTE]
> - Neben Abonnements können Sie Exporte von Ressourcengruppen, Konten, Abteilungen und Registrierungen erstellen. Weitere Informationen zu Bereichen finden Sie unter [Verstehen von und Arbeiten mit Bereichen](understand-work-scopes.md).
>- Wenn Sie als Partner im Abrechnungskontobereich oder beim Mandanten eines Kunden angemeldet sind, können Sie Daten in ein Azure Storage-Konto exportieren, das mit dem Partnerspeicherkonto verknüpft ist. Sie müssen jedoch über ein aktives Abonnement in Ihrem CSP-Mandanten verfügen.
>


Wählen Sie **Hinzufügen** aus, geben Sie einen Namen für den Export ein, und wählen Sie die Option **Täglicher Export der Kosten für bisherigen Kalendermonat** aus. Wählen Sie **Weiter** aus.

![Beispiel für neuen Export mit Angabe des Exporttyps](./media/tutorial-export-acm-data/basics_exports.png)

Geben Sie das Abonnement für Ihr Azure-Speicherkonto an, und wählen Sie dann Ihr Speicherkonto aus.  Geben Sie den Speichercontainer und den Verzeichnispfad an, unter dem die Exportdatei gespeichert werden soll. Wählen Sie **Weiter** aus.

![Beispiel für neuen Export mit Angabe der Speicherkontodetails](./media/tutorial-export-acm-data/storage_exports.png)

Überprüfen Sie die Exportdetails, und wählen Sie **Erstellen** aus.

Ihr neuer Export wird in der Liste der Exporte angezeigt. Neue Exporte sind standardmäßig aktiviert. Wenn Sie einen geplanten Export deaktivieren oder löschen möchten, wählen Sie ein beliebiges Element in der Liste und anschließend entweder **Deaktivieren** oder **Löschen** aus.

Zunächst kann es ein bis zwei Stunden dauern, bis der Export ausgeführt wird. Es kann jedoch bis zu vier Stunden dauern, bis die Daten in exportierten Dateien angezeigt werden.

### <a name="export-schedule"></a>Exportzeitplan

Uhrzeit und Wochentag der ersten Erstellung eines Exports wirken sich auf geplante Exporte aus. Wenn Sie einen geplanten Export erstellen, werden alle folgenden Exportvorgänge mit der gleichen Häufigkeit ausgeführt. Legen Sie für einen Export für den bisherigen Kalendermonat beispielsweise als Häufigkeit „täglich“ fest, und der Export wird täglich ausgeführt. Das Gleiche gilt für einen wöchentlichen Export: Der wöchentliche Export wird jede Woche am selben geplanten Tag ausgeführt. Die genaue Bereitstellungszeit des Exports wird nicht garantiert, und die exportierten Daten sind innerhalb von vier Stunden ab dem Ausführungszeitpunkt verfügbar.
Bei jedem Export wird eine neue Datei erstellt, sodass ältere Exporte nicht überschrieben werden.

Es gibt zwei Typen von Exportoptionen:

**Täglicher Export der Kosten für bisherigen Kalendermonat**: Der erste Export wird sofort ausgeführt. Nachfolgende Exporte werden am nächsten Tag zur gleichen Zeit wie der erste Export ausgeführt. Die aktuellen Daten werden aus vorhergehenden täglichen Exporten aggregiert.

**Benutzerdefiniert**: Damit können Sie wöchentliche und monatliche Exporte mit Optionen für die bisherige Woche und dem bisherigen Monat planen. *Der erste Export wird sofort ausgeführt.*

Wenn Sie über ein Abonnement mit nutzungsbasierter Bezahlung, ein MSDN-Abonnement oder ein Visual Studio-Abonnement verfügen, stimmt der Abrechnungszeitraum für Ihre Rechnung möglicherweise nicht mit dem Kalendermonat überein. Bei diesen Arten von Abonnements und Ressourcengruppen können Sie einen Export erstellen, der Ihrem Rechnungszeitraum oder Kalendermonaten entspricht. Navigieren Sie zum Erstellen eines Exports, der Ihrem Rechnungsmonat entspricht, zu **Benutzerdefiniert**, und wählen Sie dann die Option **Bisheriger Abrechnungszeitraum**.  Wählen Sie **Monat bis Datum**, um einen Export zu erstellen, der dem Kalendermonat entspricht.
>
>

![Neuer Export: Registerkarte „Grundlagen“, die eine benutzerdefinierte Auswahl für wöchentliche Exporte seit Wochenbeginn zeigt](./media/tutorial-export-acm-data/tutorial-export-schedule-weekly-week-to-date.png)

## <a name="verify-that-data-is-collected"></a>Überprüfen, ob Daten gesammelt wurden

Sie können ganz einfach überprüfen, ob Ihre Cost Management-Daten erfasst werden, und die exportierte CSV-Datei mit dem Azure Storage-Explorer anzeigen.

Wählen Sie in der Exportliste den Namen des Speicherkontos aus. Wählen Sie auf der Seite des Speicherkontos die Option „Im Explorer öffnen“ aus Wenn ein Bestätigungsdialogfeld angezeigt wird, wählen Sie **Ja** aus, um die Datei in Azure Storage-Explorer zu öffnen.

![Speicherkontoseite mit Beispielinformationen und Link „In Explorer öffnen“](./media/tutorial-export-acm-data/storage-account-page.png)

Navigieren Sie im Storage-Explorer zu dem Container, den Sie öffnen möchten, und wählen Sie den Ordner aus, der dem aktuellen Monat entspricht. Es wird eine Liste der CSV-Dateien angezeigt. Wählen Sie eine Datei und anschließend **Öffnen** aus.

![In Storage-Explorer angezeigte Beispielinformationen](./media/tutorial-export-acm-data/storage-explorer.png)

Die Datei wird mit dem Programm oder der Anwendung geöffnet, die zum Öffnen von CSV-Dateierweiterungen ausgewählt ist. Hier sehen Sie ein Beispiel in Excel.

![Beispiel für in Excel angezeigte exportierte CSV-Daten](./media/tutorial-export-acm-data/example-export-data.png)


## <a name="access-exported-data-from-other-systems"></a>Zugreifen auf exportierte Daten über andere Systeme

Einer der Gründe für den Export Ihrer Daten aus Cost Management ist der Zugriff auf die Daten über externe Systeme. Sie können ein Dashboard oder ein anderes Finanzsystem verwenden. Derartige Systeme unterscheiden sich stark, sodass es nicht praktikabel wäre, ein Beispiel zu zeigen.  Sie können jedoch mit dem Zugriff auf Ihre Daten aus Ihren Anwendungen unter [Einführung in Azure Storage](../../storage/common/storage-introduction.md) beginnen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines täglichen Exports
> * Überprüfen, ob Daten gesammelt wurden

Fahren Sie mit dem nächsten Tutorial fort, um sich Möglichkeiten der Optimierung und Steigerung der Effizienz anzuschauen, indem ungenutzte oder nicht ausreichend genutzte Ressourcen ermittelt werden.

> [!div class="nextstepaction"]
> [Prüfen und Reagieren auf Empfehlungen zur Optimierung](tutorial-acm-opt-recommendations.md)
