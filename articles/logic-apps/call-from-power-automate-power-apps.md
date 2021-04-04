---
title: Aufrufen von Logik-Apps über Power Automate und Power Apps
description: Rufen Sie Logik-Apps von Microsoft Power Automate-Flows aus auf, indem Sie Logik-Apps als Connectors exportieren.
services: logic-apps
ms.suite: integration
ms.reviewer: estfan, logicappspm
ms.topic: conceptual
ms.date: 10/05/2020
ms.openlocfilehash: b402dab4c6e94a7634e11f0330b5379315e43abf
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "91762414"
---
# <a name="call-logic-apps-from-power-automate-and-power-apps"></a>Aufrufen von Logik-Apps über Power Automate und Power Apps

Um Ihre Logik-Apps von Microsoft Power Automate und Microsoft Power Apps aus aufzurufen, können Sie Ihre Logik-Apps als Connectors exportieren. Wenn Sie eine Logik-App als benutzerdefinierten Connector in einer Power Automate- oder Power Apps-Umgebung verfügbar machen, können Sie Ihre Logik-App aus Flows dort aufrufen.

Wenn Sie den Flow stattdessen von Power Automate oder Power Apps in Logic Apps migrieren möchten, finden Sie weitere Informationen unter [Exportieren von Flows aus Power Automate und Bereitstellen in Azure Logic Apps](export-from-microsoft-flow-logic-app-template.md).

> [!NOTE]
> Nicht alle Power Automate-Connectors sind in Azure Logic Apps verfügbar. Sie können nur Power Automate-Flows migrieren, die in Azure Logic Apps über äquivalente Connectors verfügen. Beispielsweise sind der Trigger „Schaltfläche“, der Connector „Genehmigung“ und der Connector „Benachrichtigung“ spezifisch für Power Automate. Aktuell werden der Export und die Bereitstellung von auf OpenAPI basierenden Flows in Power Automate als Vorlagen für Logik-Apps nicht unterstützt.
>
> * Informationen dazu, welche Power Automate-Connectors keine Logic Apps-Entsprechungen besitzen, finden Sie unter [Power Automate-Connectors](/connectors/connector-reference/connector-reference-powerautomate-connectors).
>
> * Informationen dazu, welche Logic Apps-Connectors keine Power Automate-Entsprechungen besitzen, finden Sie unter [Logic Apps-Connectors](/connectors/connector-reference/connector-reference-powerautomate-connectors).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* Eine Power Automate- oder Power Apps-Lizenz.

* Eine Logik-App mit einem zu exportierenden Anforderungstrigger.

* Ein Flow in Power Automate oder Power Apps, von dem aus Sie Ihre Logik-App aufrufen möchten.

## <a name="export-your-logic-app-as-a-custom-connector"></a>Exportieren Ihrer Logik-App als benutzerdefinierter Connector

Bevor Sie Ihre Logik-App über Power Automate oder Power Apps aufrufen können, müssen Sie Ihre Logik-App zunächst als benutzerdefinierten Connector exportieren.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Geben Sie in das Suchfeld des Azure-Portals `Logic Apps` ein. Wählen Sie in den Ergebnissen unter **Dienste** die Option **Logic Apps** aus.

1. Wählen Sie die Logik-App aus, die Sie exportieren möchten.

1. Wählen Sie im Menü Ihrer Logik-App die Option **Exportieren** aus.

    :::image type="content" source="./media/call-logic-apps-from-power-automate-power-apps/export-logic-app.png" alt-text="Screenshot der Seite der Logik-App im Azure-Portal, auf der das Menü mit ausgewählter Schaltfläche „Exportieren“ angezeigt wird.":::

1. Geben Sie im Bereich **Exportieren** für **Name** einen Namen für den benutzerdefinierten Connector zu ihrer Logik-App ein. Wählen Sie in der Liste **Umgebung** die Power Automate- oder Power Apps-Umgebung aus, von der aus Sie Ihre Logik-App aufrufen möchten. Wenn Sie fertig sind, wählen Sie **OK**.

    :::image type="content" source="./media/call-logic-apps-from-power-automate-power-apps/export-logic-app2.png" alt-text="Screenshot des Bereichs „Exportieren“ für die Logik-App mit den erforderlichen Feldern für den Namen des benutzerdefinierten Connectors und die Umgebung.":::

1. Um sich zu vergewissern, dass Ihre Logik-App erfolgreich exportiert wurde, überprüfen Sie den Benachrichtigungsbereich.

### <a name="exporting-errors"></a>Fehler beim Exportieren

Im Folgenden werden Fehler aufgeführt, die auftreten können, wenn Sie Ihre Logik-App als benutzerdefinierten Connector exportieren, und Lösungsvorschläge:

* **Fehler beim Abrufen der Umgebungen. Stellen Sie sicher, dass Ihr Konto für Power Automate konfiguriert ist, und versuchen Sie es dann erneut.** : Überprüfen Sie, ob Ihr Azure-Konto über einen Power Automate-Plan verfügt.

* **Die aktuelle Logik-App kann nicht exportiert werden. Wählen Sie zum Exportieren eine Logik-App mit einem Anforderungstrigger aus.** : Überprüfen Sie, ob Ihre Logik-App mit einem [Anforderungstrigger](./logic-apps-workflow-actions-triggers.md#request-trigger) beginnt.

## <a name="connect-to-your-logic-app-from-power-automate"></a>Herstellen einer Verbindung mit Ihrer Logik-App von Power Automate aus

So stellen Sie eine Verbindung mit der Logik-App her, die Sie mit Ihrem Power Automate-Flow exportiert haben:

1. Melden Sie sich bei [Power Automate](https://flow.microsoft.com) an.

1. Wählen Sie im Menü der Startseite von **Power Automate** die Option **Meine Flows** aus.

1. Wählen Sie auf der Seite **Flows** den Flow aus, den Sie mit Ihrer Logik-App verbinden möchten.

1. Wählen Sie im Menü der Seite Ihres Flows **Bearbeiten** aus.

1. Wählen Sie im Flow-Editor **&#43; Neuer Schritt** aus.

1. Geben Sie unter **Aktion auswählen** im Suchfeld den Namen Ihres Logik-App-Connectors ein. Wenn Sie optional nur die benutzerdefinierten Connectors in Ihrer Umgebung anzeigen möchten, filtern Sie die Ergebnisse, indem Sie die Registerkarte **Benutzerdefiniert** auswählen.

    :::image type="content" source="./media/call-logic-apps-from-power-automate-power-apps/power-automate-custom-connector-action.png" alt-text="Screenshot des Flow-Editors von Power Automate, der das Hinzufügen eines neuen Schritts für den benutzerdefinierten Connector und die verfügbaren Aktionen anzeigt.":::

1. Wählen Sie die Aktion aus, die Sie mit Ihrem Logik-App-Connector ausführen möchten. 

1. Geben Sie die Informationen an, die die Aktion dem Logik-App-Connector übergibt.

1. Um die Änderungen zu speichern, wählen Sie im Menü des Editors von Power Automate **Speichern** aus.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Suchen Sie im Logic Apps-Dienst die Logik-App, die Sie exportiert haben.

1. Vergewissern Sie sich, dass Ihre Logik-App erwartungsgemäß in Ihrem Power Automate-Flow funktioniert.

## <a name="delete-logic-app-connector-from-power-automate"></a>Löschen des Logik-App-Connectors aus Power Automate

1. Melden Sie sich bei [Power Automate](https://flow.microsoft.com) an.

1. Wählen Sie im Menü der Startseite von **Power Automate** die Option **Daten** &gt; **Benutzerdefinierte Connectors** aus.

1. Suchen Sie in der Liste Ihren benutzerdefinierten Connector, und wählen Sie die Schaltfläche mit den Auslassungspunkten ( **...** ) &gt; **Löschen** aus.

    :::image type="content" source="./media/call-logic-apps-from-power-automate-power-apps/delete-custom-connector.png" alt-text="Screenshot der Power Automate-Seite „Benutzerdefinierte Connectors“ mit den Verwaltungsschaltflächen des benutzerdefinierten Connectors der Logik-App.":::

1. Um das Löschen zu bestätigen, wählen Sie **OK** aus.

## <a name="connect-to-your-logic-app-from-power-apps"></a>Herstellen einer Verbindung mit Ihrer Logik-App von Power Apps aus

So stellen Sie eine Verbindung mit der Logik-App her, die Sie mit Ihrem Power Apps-Flow exportiert haben:

1. Melden Sie sich bei [Power Apps](https://powerapps.microsoft.com/) an.

1. Wählen Sie im Menü der Startseite von **Power Apps** die Option **Flows** aus.

1. Wählen Sie auf der Seite **Flows** den Flow aus, den Sie mit Ihrer Logik-App verbinden möchten.

1. Wählen Sie im Menü auf der Seite Ihres Flows **Bearbeiten** aus.

1. Wählen Sie im Flow-Editor die Schaltfläche **&#43; Neuer Schritt** aus.

1. Geben Sie unter **Aktion auswählen** im neuen Schritt den Namen Ihres Logik-App-Connectors in das Suchfeld ein. Filtern Sie die Ergebnisse optional nach der Registerkarte **Benutzerdefiniert**, um nur benutzerdefinierte Connectors in Ihrer Umgebung anzuzeigen.

    :::image type="content" source="./media/call-logic-apps-from-power-automate-power-apps/power-apps-custom-connector-action.png" alt-text="Screenshot des Flow-Editors von Power Apps, der das Hinzufügen eines neuen Schritts für den benutzerdefinierten Connector und die verfügbaren Aktionen anzeigt.":::

1. Wählen Sie die Aktion aus, die Sie mit dem Connector ausführen möchten. 

1. Konfigurieren Sie, welche Informationen Ihre Aktion dem Logik-App-Connector übergibt.

1. Um die Änderungen zu speichern, wählen Sie im Menü des Editors von Power Apps **Speichern** aus. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Suchen Sie im Logic Apps-Dienst die Logik-App, die Sie exportiert haben.

1. Vergewissern Sie sich, dass Ihre Logik-App wie gewünscht mit dem Flow von Power Apps funktioniert.

## <a name="delete-logic-app-connector-from-power-apps"></a>Löschen des Logik-App-Connectors aus Power Apps

1. Melden Sie sich bei [Power Apps](https://powerapps.microsoft.com) an.

1. Wählen Sie im Menü der Startseite von **Power Apps** die Option **Daten** &gt; **Benutzerdefinierte Connectors** aus.

1. Suchen Sie in der Liste Ihren benutzerdefinierten Connector, und wählen Sie die Schaltfläche mit den Auslassungspunkten ( **...** ) &gt; **Löschen** aus.

    :::image type="content" source="./media/call-logic-apps-from-power-automate-power-apps/delete-custom-connector.png" alt-text="Screenshot der Power Apps-Seite „Benutzerdefinierte Connectors“ mit den Verwaltungsschaltflächen des benutzerdefinierten Connectors der Logik-App.":::

1. Um das Löschen zu bestätigen, wählen Sie **OK** aus.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu [Connectors für Azure Logic Apps](../connectors/apis-list.md)
* Weitere Informationen zu [Azure Logic Apps](../logic-apps/logic-apps-overview.md)
