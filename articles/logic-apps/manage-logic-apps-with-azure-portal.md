---
title: Verwalten von Logik-Apps im Azure-Portal
description: Sie können Logik-Apps über das Azure-Portal bearbeiten, aktivieren, deaktivieren oder löschen.
services: logic-apps
ms.suite: integration
author: lauradolan
ms.author: ladolan
ms.reviewer: estfan, jonfan, logicappspm
ms.topic: conceptual
ms.custom: mvc
ms.date: 04/23/2021
ms.openlocfilehash: 4c4a3b1e5d165681e921d2fadeadc5dea9633d41
ms.sourcegitcommit: 62e800ec1306c45e2d8310c40da5873f7945c657
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108162903"
---
# <a name="manage-logic-apps-in-the-azure-portal"></a>Verwalten von Logik-Apps im Azure-Portal

Zum Verwalten von Logik-Apps können Sie das [Azure-Portal](https://portal.azure.com) oder [Visual Studio](manage-logic-apps-with-visual-studio.md) verwenden. In diesem Artikel wird gezeigt, wie Sie Logik-Apps im Azure-Portal bearbeiten, deaktivieren, aktivieren oder löschen. Falls Sie noch nicht mit Logik-Apps vertraut sind, finden Sie weitere Informationen unter [Was ist Azure Logic Apps?](logic-apps-overview.md)

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* Eine vorhandene Logik-App. Informationen zum Erstellen einer Logik-App über das Azure-Portal finden Sie unter [Schnellstart: Erstellen Ihres ersten Workflows mithilfe von Azure Logic Apps – Azure-Portal](quickstart-create-first-logic-app-workflow.md).

<a name="find-logic-app"></a>

## <a name="find-and-open-a-logic-app"></a>Suchen und Öffnen einer Logik-App

1. Melden Sie sich mit Ihrem Azure-Konto beim [Azure-Portal](https://portal.azure.com) an.

1. Geben Sie in das Suchfeld des Portals `logic apps` ein, und klicken Sie auf **Logik-Apps**.

1. Suchen Sie nach Ihrer Logik-App, indem Sie die Liste der Logik-Apps durchsuchen oder filtern.

1. Um Ihre Logik-App zu öffnen, wählen Sie die App aus, die Sie verwalten möchten.

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

<a name="disable-enable-logic-apps"></a>

## <a name="disable-or-enable-logic-apps"></a>Deaktivieren oder Aktivieren von Logik-Apps

Um zu verhindern, dass der Trigger das nächste Mal ausgelöst wird, wenn die Triggerbedingung erfüllt ist, deaktivieren Sie Ihre Logik-App. Im Azure-Portal können Sie eine [einzelne Logik-App](#disable-enable-single-logic-app) oder [mehrere Logik-Apps gleichzeitig](#disable-or-enable-multiple-logic-apps) aktivieren oder deaktivieren. Das Deaktivieren einer Logik-App wirkt sich wie folgt auf Workflowinstanzen aus:

* Die Logic  Apps-Dienste setzen alle ausgeführten und ausstehenden Ausführungen fort, bis sie abgeschlossen sind. Basierend auf dem Volume oder Backlog kann es einige Zeit dauern, bis dieser Prozess abgeschlossen ist.

* Der Logic Apps-Dienst erstellt keine neuen Workflowinstanzen und führt keine neuen Workflowinstanzen aus.

* Der Trigger wird nicht ausgelöst, wenn die definierten Bedingungen beim nächsten Mal erfüllt werden. Der Triggerstatus merkt sich den Punkt, an dem die Logik-App angehalten wurde. Wenn Sie die Logik-App erneut aktivieren, wird der Trigger für alle nicht verarbeiteten Elemente seit der letzten Ausführung ausgelöst.

  Wenn Sie die Auslösung eines Triggers für nicht verarbeitete Elemente seit der letzten Ausführung verhindern möchten, löschen Sie den Triggerstatus, bevor Sie die Logik-App wieder aktivieren:

  1. Bearbeiten Sie in der Logik-App einen beliebigen Teil des Workflowtriggers.
  1. Speichern Sie die Änderungen. Durch diesen Schritt wird der aktuelle Status Ihres Triggers zurückgesetzt.
  1. [Aktivieren Sie Ihre Logik-App wieder](#disable-enable-single-logic-app).

<a name="disable-enable-single-logic-app"></a>

### <a name="disable-or-enable-a-single-logic-app"></a>Deaktivieren oder Aktivieren einer einzelnen Logik-App

1. [Suchen Sie im Azure-Portal nach Ihrer Logik-App](#find-logic-app), und öffnen Sie sie.

1. Wählen Sie im Menü Ihrer Logik-App die Option **Übersicht** aus.

   * Um Ihre Logik-App zu deaktivieren, wählen Sie in der Symbolleiste des Übersichtsbereichs **Deaktivieren** aus.
   * Um Ihre Logik-App zu aktivieren, wählen Sie in der Symbolleiste des Übersichtsbereichs **Aktivieren** aus.

     > [!NOTE]
     > Wenn Ihre Logik-App bereits deaktiviert ist, ist nur die Option **Aktivieren** verfügbar.
     > Wenn Ihre Logik-App bereits aktiviert ist, ist nur die Option **Deaktivieren** verfügbar.

1. Um zu überprüfen, ob Ihr Vorgang erfolgreich war oder fehlgeschlagen ist, öffnen Sie auf der Azure-Hauptsymbolleiste die Liste **Benachrichtigungen** (Glockensymbol).

<a name="disable-or-enable-multiple-logic-apps"></a>

### <a name="disable-or-enable-multiple-logic-apps"></a>Deaktivieren oder Aktivieren mehrerer Logik-Apps

1. Geben Sie `logic apps` in das Suchfeld des Azure-Portals ein, und klicken Sie auf **Logik-Apps**.

1. Überprüfen Sie auf der Seite **Logik-Apps** die Spalte **Status** der Logik-Apps.

1. Wählen Sie in der Spalte mit den Kontrollkästchen die Logik-Apps aus, die Sie starten oder stoppen möchten.

   * Um die ausgewählten ausgeführten Logik-Apps zu beenden, wählen Sie auf der Symbolleiste des Übersichtsbereichs **Deaktivieren/Beenden** aus. Bestätigen Sie Ihre Auswahl.
   * Um die ausgewählten beendeten Logik-Apps zu starten, wählen Sie auf der Symbolleiste des Übersichtsbereichs die Option **Aktivieren/Starten** aus.

1. Um zu überprüfen, ob Ihr Vorgang erfolgreich war oder fehlgeschlagen ist, öffnen Sie auf der Azure-Hauptsymbolleiste die Liste **Benachrichtigungen** (Glockensymbol).

   > [!TIP]
   > Wenn der korrekte Status nicht angezeigt wird, aktualisieren Sie die Seite.

<a name="delete-logic-apps"></a>

## <a name="delete-logic-apps"></a>Löschen von Logik-Apps

Sie können eine einzelne Logik-Apps oder mehrere Logik-Apps gleichzeitig löschen. Das Löschen einer Logik-App wirkt sich wie folgt auf Workflow-Instanzen aus:

* Der Logic Apps-Dienst unterbricht alle aktiven und ausstehenden Ausführungen so gut wie möglich.

  Selbst bei einer großen Menge oder einem umfangreichen Backlog werden die meisten Ausführungen abgebrochen, bevor sie abgeschlossen oder gestartet werden. Es kann jedoch einige Zeit dauern, bis der Abbruchvorgang abgeschlossen ist. In der Zwischenzeit werden möglicherweise einige Ausführungen gestartet, während der Dienst den Abbruchprozess durchläuft.

* Der Logic Apps-Dienst erstellt keine neuen Workflowinstanzen und führt keine neuen Workflowinstanzen aus.

* Wenn Sie einen Workflow löschen und dann denselben Workflow neu erstellen, hat der neu erstellte Workflow nicht die gleichen Metadaten wie der gelöschte Workflow. Sie müssen jeden Workflow, der den gelöschten Workflow aufgerufen hat, neu speichern. Auf diese Weise ruft der Aufrufer die richtigen Informationen für den neu erstellten Workflow ab. Andernfalls schlagen Aufrufe des neu erstellten Workflows mit einem `Unauthorized`-Fehler fehl. Dieses Verhalten gilt auch für Workflows, die Artefakte in Integrationskonten und Workflows verwenden, welche Azure-Funktionen aufrufen.

1. Geben Sie `logic apps` in das Suchfeld des Azure-Portals ein, und klicken Sie auf **Logik-Apps**.

1. Wählen Sie in der Liste **Logik-Apps** in der Kontrollkästchenspalte eine einzelne oder mehrere zu löschende Logik-Apps aus. Wählen Sie in der Symbolleiste die Option **Löschen** aus.

1. Wenn das Bestätigungsfeld angezeigt wird, geben Sie `yes` ein, und wählen Sie **Löschen** aus.

1. Um zu überprüfen, ob Ihr Vorgang erfolgreich war oder fehlgeschlagen ist, öffnen Sie auf der Azure-Hauptsymbolleiste die Liste **Benachrichtigungen** (Glockensymbol).

<a name="manage-logic-app-versions"></a>

## <a name="manage-logic-app-versions"></a>Verwalten von Logik-App-Versionen

Sie können das Azure-Portal für die Versionskontrolle Ihrer Logik-Apps verwenden. Sie können den Versionsverlauf Ihrer Logik-App suchen und frühere Versionen höherstufen.

<a name="find-version-history"></a>

### <a name="find-and-view-previous-versions"></a>Suchen und Anzeigen früherer Versionen

1. [Suchen Sie im Azure-Portal nach Ihrer Logik-App](#find-logic-app), und öffnen Sie sie.

1. Klicken Sie im Menü Ihrer Logik-App unter **Entwicklungstools** auf **Versionen**.

   ![Screenshot einer Logik-App im Azure-Portal mit Auswahl der Versionsseite unter den Entwicklungstools](./media/manage-logic-apps-with-azure-portal/logic-apps-menu-versions.png)

1. Wählen Sie in der Liste die **Version** Ihrer Logik-App aus, die Sie verwalten möchten. Sie können den **Version** sbezeichner in die Suchleiste eingeben, um die Liste zu filtern.

1. Auf der Seite **Verlaufsversion** werden die Details der früheren Version im schreibgeschützten Modus angezeigt. Sie können zwischen den Modi Logic Apps-**Designer** und **Codeansicht** auswählen.

   ![Screenshot der Versionsverlaufsseite in Logic Apps mit Codeansicht und Optionen für den Ansicht-Designer](./media/manage-logic-apps-with-azure-portal/history-version.png)

<a name="promote-previous-versions"></a>

### <a name="promote-previous-versions"></a>Höherstufen früherer Versionen

1. Suchen Sie im Versionsverlauf ihrer Logik-App [die Version, und wählen Sie die aus, die Sie höherstufen möchten](#find-version-history).

1. Wählen Sie auf der Seite der **Verlaufsversion** den Befehl **Höherstufen** aus.

   ![Screenshot des Versionsverlaufs in Logic Apps mit Schaltfläche „Höher stufen“ einer vorherigen Version](./media/manage-logic-apps-with-azure-portal/promote-button.png)

1. Bearbeiten Sie auf der Seite **Logic Apps-Designer**, die geöffnet wird, nach Bedarf die Version, die Sie höherstufen möchten. Sie können zwischen den Modi **Designer** und **Codeansicht** wechseln. Sie können auch **Parameter**, **Vorlagen** und **Connectors** aktualisieren.

   ![Screenshot des Logic Apps-Designers mit der Schaltfläche „Höher stufen“ einer Logik-App](./media/manage-logic-apps-with-azure-portal/promote-page.png)

1. Wählen Sie zum Speichern jeglicher Updates und zum Abschließen der Höherstufung der früheren Version **Speichern** aus. (Oder wählen Sie, um Ihre Änderungen abzubrechen, **Verwerfen** aus.) 

   Wenn Sie den [Versionsverlauf Ihrer Logik-App erneut anzeigen](#find-version-history), wird die höhergestufte Version am Anfang der Liste angezeigt, und sie verfügt über einen neuen Bezeichner.

## <a name="next-steps"></a>Nächste Schritte

* [Überwachen von Logik-Apps](monitor-logic-apps.md)
