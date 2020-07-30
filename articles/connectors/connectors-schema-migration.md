---
title: Migrieren von Apps zum aktuellen Schema
description: Migrieren von JSON-Definitionen für den Logik-App-Workflow zur aktuellsten Schema Version der Workflowdefinitionssprache
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/25/2018
ms.openlocfilehash: 114b8b32d4abb1fd9b7e641625cd1b132470bafd
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87281445"
---
# <a name="migrate-logic-apps-to-latest-schema-version"></a>Migrieren von Logik-Apps zur aktuellen Schemaversion

Führen Sie diese Schritte aus, um Ihre vorhandenen Logik-Apps in das neueste Schema zu verschieben: 

1. Öffnen Sie Ihre Logik-App über das [Azure-Portal](https://portal.azure.com) im Logik-App-Designer.

2. Klicken Sie im Menü der Logik-App auf **Übersicht**. Wählen Sie auf der Symbolleiste die Option **Schema aktualisieren** aus.

   > [!NOTE]
   > Wenn Sie **Schema aktualisieren** auswählen, führt Azure Logic Apps automatisch die Migrationsschritte aus und stellt die Codeausgabe für Sie bereit. Diese Ausgabe können Sie zum Aktualisieren der Definition Ihrer Logik-App verwenden. Stellen Sie jedoch sicher, dass Sie die im folgenden Abschnitt beschriebenen **bewährten Methoden** anwenden.

   ![Schema aktualisieren](./media/connectors-schema-migration/update-schema.png)

   Die Seite „Schema aktualisieren“ wird geöffnet und zeigt einen Link zu einem Dokument an, das die Verbesserungen im neuen Schema beschreibt.

## <a name="best-practices"></a>Bewährte Methoden

Hier finden Sie einige bewährte Methoden für das Migrieren von Logik-Apps zur aktuellen Schemaversion:

* Kopieren Sie das migrierte Skript in eine neue Logik-App. Überschreiben Sie die alte Version nicht, bevor Sie Ihre Tests abgeschlossen und sich vergewissert haben, das Ihre migrierte App wie erwartet funktioniert.

* Testen Sie Ihre Logik-App, **bevor** sie in Produktion geht.

* Beginnen Sie nach Abschluss der Migration mit der Aktualisierung Ihrer Logik-Apps, um nach Möglichkeit die [verwalteten APIs](../connectors/apis-list.md) zu nutzen. Verwenden Sie beispielsweise Dropbox v2 ab jetzt überall, wo Sie bisher DropBox v1 verwendet haben.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum [manuellen Migrieren von Logik-Apps](../logic-apps/logic-apps-schema-2016-04-01.md)

