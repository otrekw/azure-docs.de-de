---
title: Verwalten von Logik-Apps im Azure-Portal
description: Sie können Logik-Apps über das Azure-Portal bearbeiten, aktivieren, deaktivieren oder löschen.
services: logic-apps
ms.suite: integration
author: lauradolan
ms.author: ladolan
ms.reviewer: estfan, logicappspm
ms.topic: article
ms.custom: mvc
ms.date: 04/13/2020
ms.openlocfilehash: f726ca90c215c4aff3734bd8022bbc1ad4dc5f87
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81415973"
---
# <a name="manage-logic-apps-in-the-azure-portal"></a>Verwalten von Logik-Apps im Azure-Portal

Zum Verwalten von Logik-Apps können Sie das [Azure-Portal](https://portal.azure.com) oder [Visual Studio](manage-logic-apps-with-visual-studio.md) verwenden. In diesem Artikel wird gezeigt, wie Sie Logik-Apps im Azure-Portal bearbeiten, deaktivieren, aktivieren oder löschen. Falls Sie noch nicht mit Logik-Apps vertraut sind, finden Sie weitere Informationen unter [Was ist Azure Logic Apps?](logic-apps-overview.md)

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* Eine vorhandene Logik-App. Informationen zum Erstellen einer Logik-App über das Azure-Portal finden Sie unter [Schnellstart: Erstellen Ihres ersten Workflows mithilfe von Azure Logic Apps – Azure-Portal](quickstart-create-first-logic-app-workflow.md).

<a name="find-logic-app"></a>

## <a name="find-your-logic-apps"></a>Suchen nach Ihren Logik-Apps

Führen Sie diese Schritte aus, um nach Ihrer Logik-App zu suchen und sie zu öffnen:

1. Melden Sie sich mit Ihrem Azure-Konto beim [Azure-Portal](https://portal.azure.com) an.

1. Geben Sie in der Azure-Suchleiste `logic apps` ein, und wählen Sie **Logic Apps** aus.

   ![Suchen und Auswählen von „Logic Apps“](./media/manage-logic-apps-with-azure-portal/find-select-logic-apps.png)

1. Suchen Sie auf der Seite **Logic Apps** die Logik-App, die Sie verwalten möchten, und wählen Sie sie aus.

   Sobald der Bereich **Übersicht** der Logik-App geöffnet wird, können Sie die Liste auf der Seite **Logic Apps** wie folgt filtern:

   * Suchen nach Logik-Apps anhand des Namens
   * Filtern von Logik-Apps nach Abonnement, Ressourcengruppe, Speicherort und Tags
   * Gruppieren von Logik-Apps nach Ressourcengruppe, Typ, Abonnement und Speicherort

## <a name="view-logic-app-properties"></a>Anzeigen von Logik-App-Eigenschaften

1. [Suchen Sie im Azure-Portal nach Ihrer Logik-App](#find-logic-app), und öffnen Sie sie.

1. Wählen Sie im Menü Ihrer Logik-App unter **Einstellungen** die Option **Eigenschaften** aus.

1. Im Bereich **Eigenschaften** finden Sie die folgenden Informationen zu Ihrer Logik-App. Sie können diese Informationen bei Bedarf kopieren:

   * **Name**
   * **Ressourcen-ID**
   * **Ressourcengruppe**
   * **Location**
   * **Typ** 
   * **Abonnementname**
   * **Abonnement-ID**
   * **Zugriffsendpunkt**
   * **Ausgehende IP-Adressen der Runtime**
   * **Zugriffsendpunkt-IP-Adressen**
   * **Ausgehende IP-Adressen des Connectors**

## <a name="disable-or-enable-logic-apps"></a>Deaktivieren oder Aktivieren von Logik-Apps

Im Azure-Portal können Sie eine [einzelne Logik-App](#disable-enable-single-logic-app) oder [mehrere Logik-Apps gleichzeitig](#disable-or-enable-multiple-logic-apps) aktivieren oder deaktivieren. Sie können [Logik-Apps auch in Visual Studio aktivieren oder deaktivieren](manage-logic-apps-with-visual-studio.md#disable-or-enable-logic-app).

Das Deaktivieren Ihrer Logik-App wirkt sich wie folgt auf Ihre Workflowinstanzen und -ausführungen aus:

* Alle in Bearbeitung befindlichen und ausstehenden Ausführungen werden bis zum Ende fortgesetzt. Abhängig von der Anzahl dieser Ausführungen kann dies einige Zeit in Anspruch nehmen.

* Das Logic Apps-Modul erstellt keine neuen Workflowinstanzen und führt keine neuen Workflowinstanzen aus.

* Der Trigger wird nicht ausgelöst, wenn die definierten Bedingungen beim nächsten Mal erfüllt werden.

* Der Triggerstatus merkt sich den Punkt, an dem die Logik-App angehalten wurde. Wenn Sie die Logik-App erneut aktivieren, wird der Trigger für alle nicht verarbeiteten Elemente seit der letzten Ausführung ausgelöst.

  Um zu verhindern, dass Ihre Logik-App für nicht verarbeitete Elemente seit der letzten Ausführung ausgelöst wird, löschen Sie den Triggerstatus, bevor Sie die Logik-App erneut aktivieren:

  1. [Suchen Sie im Azure-Portal nach Ihrer Logik-App](#find-logic-app), und öffnen Sie sie.

  1. Bearbeiten Sie einen beliebigen Teil des Triggers Ihrer Logik-App.

  1. Speichern Sie die Änderungen. Durch diesen Schritt wird der aktuelle Status Ihres Triggers zurückgesetzt.

  1. [Aktivieren Sie Ihre Logik-App erneut](#disable-enable-single-logic-app).

<a name="disable-enable-single-logic-app"></a>

### <a name="disable-or-enable-single-logic-app"></a>Deaktivieren oder Aktivieren einer einzelnen Logik-App

1. [Suchen Sie im Azure-Portal nach Ihrer Logik-App](#find-logic-app), und öffnen Sie sie.

1. Wählen Sie im Menü Ihrer Logik-App die Option **Übersicht** aus. Wählen Sie eine der folgenden Optionen aus:

   * Wählen Sie in der Symbolleiste die Option **Deaktivieren** aus.

     ![Deaktivieren einer einzelnen Logik-App im Azure-Portal](./media/manage-logic-apps-with-azure-portal/disable-single-logic-app.png)

     Wenn Ihre Logik-App bereits deaktiviert ist, ist nur die Option **Aktivieren** verfügbar.

   * Wählen Sie auf der Symbolleiste **Aktivieren** aus.

     ![Aktivieren einer einzelnen Logik-App im Azure-Portal](./media/manage-logic-apps-with-azure-portal/enable-single-logic-app.png)

     Wenn Ihre Logik-App bereits aktiviert ist, ist nur die Option **Deaktivieren** verfügbar. 

   Im Azure-Portal wird in der Azure-Hauptsymbolleiste in einer Benachrichtigung angezeigt, ob Ihr Vorgang erfolgreich war oder ein Fehler aufgetreten ist.

   ![Benachrichtigung zur Bestätigung des Vorgangsstatus](./media/manage-logic-apps-with-azure-portal/operation-confirmation-notification.png)

<a name="disable-or-enable-multiple-logic-apps"></a>

### <a name="disable-or-enable-multiple-logic-apps"></a>Deaktivieren oder Aktivieren mehrerer Logik-Apps

1. [Suchen Sie im Azure-Portal nach den Logik-Apps](#find-logic-app), die Sie deaktivieren oder aktivieren möchten.

1. Um zu ermitteln, ob eine Logik-App aktuell aktiviert oder deaktiviert ist, überprüfen Sie auf der Seite **Logic Apps** die Spalte **Status** für die jeweilige Logik-App. 

   ![Spalte „Status“ von Logik-Apps](./media/manage-logic-apps-with-azure-portal/view-logic-app-status.png)

   Wenn die Spalte **Status** nicht sichtbar ist, wählen Sie in der **Logic Apps**-Symbolleiste die Option **Preview testen** aus.

   ![Vorschau aktivieren](./media/manage-logic-apps-with-azure-portal/select-try-preview.png)

1. Wählen Sie in der Spalte mit den Kontrollkästchen die Logik-Apps aus, die Sie deaktivieren oder aktivieren möchten. Wählen Sie in der Symbolleiste die Option **Deaktivieren** oder **Aktivieren** aus.

   ![Aktivieren oder Deaktivieren mehrerer Logik-Apps im Azure-Portal](./media/manage-logic-apps-with-azure-portal/enable-disable-multiple-logic-apps.png)

1. Wenn das Bestätigungsfeld angezeigt wird, wählen Sie **Ja**, um fortzufahren.

   Im Azure-Portal wird in der Azure-Hauptsymbolleiste in einer Benachrichtigung angezeigt, ob Ihr Vorgang erfolgreich war oder ein Fehler aufgetreten ist.

## <a name="delete-logic-apps"></a>Löschen von Logik-Apps

Im Azure-Portal können Sie eine [einzelne Logik-App](#delete-single-logic-app) oder [mehrere Logik-Apps gleichzeitig](#delete-multiple-logic-apps) löschen. Sie können [Ihre Logik-App auch in Visual Studio löschen](manage-logic-apps-with-visual-studio.md#delete-your-logic-app).

Das Löschen Ihrer Logik-App wirkt sich wie folgt auf Ihre Workflowinstanzen aus:

* Alle in Bearbeitung befindlichen und ausstehenden Ausführungen werden bis zum Ende fortgesetzt. Abhängig von der Anzahl dieser Ausführungen kann dies einige Zeit in Anspruch nehmen.

* Das Logic Apps-Modul erstellt keine neuen Workflowinstanzen und führt keine neuen Workflowinstanzen aus.

<a name="delete-single-logic-app"></a>

### <a name="delete-single-logic-app"></a>Löschen einer einzelnen Logik-App

1. [Suchen Sie im Azure-Portal nach Ihrer Logik-App](#find-logic-app), und öffnen Sie sie.

1. Wählen Sie im Menü Ihrer Logik-App die Option **Übersicht** aus. Wählen Sie in der Symbolleiste Ihrer Logik-App die Option **Löschen** aus.

   ![Auswahl der Option „Löschen“ in der Symbolleiste der Logik-App](./media/manage-logic-apps-with-azure-portal/delete-single-logic-app.png)

1. Wenn das Bestätigungsfeld angezeigt wird, geben Sie den Namen Ihrer Logik-App ein, und wählen Sie **Löschen** aus.

   ![Bestätigen des Löschvorgangs für Ihre Logik-App](./media/manage-logic-apps-with-azure-portal/delete-confirmation-single-logic-app.png)

   Im Azure-Portal wird in der Azure-Hauptsymbolleiste in einer Benachrichtigung angezeigt, ob Ihr Vorgang erfolgreich war oder ein Fehler aufgetreten ist.

<a name="delete-multiple-logic-apps"></a>

### <a name="delete-multiple-logic-apps"></a>Löschen mehrerer Logik-Apps

1. [Suchen Sie im Azure-Portal nach den Logik-Apps](#find-logic-app), die Sie löschen möchten.

1. Wählen Sie in der Spalte mit den Kontrollkästchen die Logik-Apps aus, die Sie löschen möchten. Wählen Sie in der Symbolleiste die Option **Löschen** aus.

   ![Löschen mehrerer Logik-Apps](./media/manage-logic-apps-with-azure-portal/delete-multiple-logic-apps.png)

1. Wenn das Bestätigungsfeld angezeigt wird, geben Sie `yes` ein, und wählen Sie **Löschen** aus.

   ![Bestätigen des Löschvorgangs für Ihre Logik-Apps](./media/manage-logic-apps-with-azure-portal/delete-confirmation-multiple-logic-apps.png)

   Im Azure-Portal wird in der Azure-Hauptsymbolleiste in einer Benachrichtigung angezeigt, ob Ihr Vorgang erfolgreich war oder ein Fehler aufgetreten ist.

<a name="manage-logic-app-versions"></a>

## <a name="manage-logic-app-versions"></a>Verwalten von Logik-App-Versionen

Sie können das Azure-Portal für die Versionskontrolle Ihrer Logik-Apps verwenden. Sie können den Versionsverlauf Ihrer Logik-App suchen und frühere Versionen höherstufen.

<a name="find-version-history"></a>

### <a name="find-and-view-previous-versions"></a>Suchen und Anzeigen früherer Versionen

1. [Suchen Sie im Azure-Portal nach der Logik-App](#find-logic-app), die Sie verwalten möchten.

1. Klicken Sie im Menü Ihrer Logik-App unter **Entwicklungstools** auf **Versionen**.

   ![Wählen Sie im Menü Ihrer Logik-App unter „Entwicklungstools“ die Option „Versionen“ aus.](./media/manage-logic-apps-with-azure-portal/logic-apps-menu-versions.png)

1. Wählen Sie in der Liste die **Version** Ihrer Logik-App aus, die Sie verwalten möchten. Sie können den **Version**sbezeichner in die Suchleiste eingeben, um die Liste zu filtern.

1. Auf der Seite **Verlaufsversion** werden die Details der früheren Version im schreibgeschützten Modus angezeigt. Sie können zwischen den Modi Logic Apps-**Designer** und **Codeansicht** auswählen.

   ![Seite „Verlaufsversion“ für Logik-App mit Codeansicht und Logic Apps-Designer-Ansicht](./media/manage-logic-apps-with-azure-portal/history-version.png)

<a name="promote-previous-versions"></a>

### <a name="promote-previous-versions"></a>Höherstufen früherer Versionen

1. Suchen Sie im Versionsverlauf ihrer Logik-App [die Version, und wählen Sie die aus, die Sie höherstufen möchten](#find-version-history).

1. Wählen Sie auf der Seite der **Verlaufsversion** den Befehl **Höherstufen** aus.

   ![Schaltfläche „Höherstufen“ im Versionsverlauf der Logik-App](./media/manage-logic-apps-with-azure-portal/promote-button.png)

1. Bearbeiten Sie auf der Seite **Logic Apps-Designer**, die geöffnet wird, nach Bedarf die Version, die Sie höherstufen möchten. Sie können zwischen den Modi **Designer** und **Codeansicht** wechseln. Sie können auch **Parameter**, **Vorlagen** und **Connectors** aktualisieren.

   ![Seite „Logic Apps-Designer“ zum Höherstufen einer früheren Version](./media/manage-logic-apps-with-azure-portal/promote-page.png)

1. Wählen Sie zum Speichern jeglicher Updates und zum Abschließen der Höherstufung der früheren Version **Speichern** aus. (Oder wählen Sie, um Ihre Änderungen abzubrechen, **Verwerfen** aus.) 

   Wenn Sie den [Versionsverlauf Ihrer Logik-App erneut anzeigen](#find-version-history), wird die höhergestufte Version am Anfang der Liste angezeigt, und sie verfügt über einen neuen Bezeichner.

## <a name="next-steps"></a>Nächste Schritte

* [Überwachen von Logik-Apps](monitor-logic-apps.md)
