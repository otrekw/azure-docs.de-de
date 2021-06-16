---
title: Testen der Benutzeroberflächen-Definitionsdatei
description: Hier wird beschrieben, wie Sie die Benutzeroberfläche zum Erstellen Ihrer Azure Managed Applications-Instanz über das Portal testen.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/04/2021
ms.author: tomfitz
ms.openlocfilehash: 55a966c17f3bcd51f354198e36e03071efd4834d
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111951467"
---
# <a name="test-your-portal-interface-for-azure-managed-applications"></a>Testen Ihrer Portalschnittstelle für Azure Managed Applications

Nachdem Sie [die Datei „createUiDefinition.json“](create-uidefinition-overview.md) für Ihre verwaltete Anwendung erstellt haben, müssen Sie die Benutzeroberfläche testen. Um das Testen zu vereinfachen, verwenden Sie eine Sandbox-Umgebung, die Ihre Datei in das Portal lädt. Sie müssen Ihre verwaltete Anwendung nicht tatsächlich bereitstellen. Die Sandbox bietet Ihre Benutzeroberfläche in der aktuellen, Vollbildportalerfahrung dar. Die Sandbox ist die empfohlene Methode, um eine Vorschau der Schnittstelle anzuzeigen.

## <a name="prerequisites"></a>Voraussetzungen

* Die Datei **createUiDefinition.json**. Falls Sie nicht über diese Datei verfügen, kopieren Sie die [Beispieldatei](https://github.com/Azure/azure-quickstart-templates/blob/master/demos/100-marketplace-sample/createUiDefinition.json).

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="use-sandbox"></a>Verwenden der Sandbox

1. Öffnen Sie die [Sandboc für das Erstellen von Benutzeroberflächendefinitionen](https://portal.azure.com/?feature.customPortal=false&#blade/Microsoft_Azure_CreateUIDef/SandboxBlade).

   ![Anzeigen der Sandbox](./media/test-createuidefinition/show-sandbox.png)

1. Ersetzen Sie die leere Definition durch den Inhalt Ihrer Datei „createUiDefinition.json“. Wählen Sie **Vorschau** aus.

   ![Auswählen von „Vorschau“](./media/test-createuidefinition/select-preview.png)

1. Das von Ihnen erstellte Formular wird angezeigt. Sie können die Benutzererfahrung schrittweise durchlaufen und die Werte eingeben.

   ![Anzeigen des Formulars](./media/test-createuidefinition/show-ui-form.png)

### <a name="troubleshooting"></a>Problembehandlung

Wenn Ihr Formular nach Auswahl von **Vorschau** nicht angezeigt wird, liegt vielleicht ein Syntaxfehler vor. Suchen Sie nach dem rote Kennzeichen in der rechten Scrollleiste, und navigieren Sie zu ihm.

![Anzeigen eines Syntaxfehlers](./media/test-createuidefinition/show-syntax-error.png)

Wenn Ihr Formular nicht angezeigt und stattdessen das Symbol einer Wolke mit Tränen angezeigt wird, enthält Ihr Formular einen Fehler, z. B. eine fehlende Eigenschaft. Öffnen Sie die Web Developer Tools in Ihrem Browser. In der **Konsole** werden wichtige Meldungen zur Benutzeroberfläche angezeigt.

![Anzeigen des Fehlers](./media/test-createuidefinition/show-error.png)

## <a name="test-your-solution-files"></a>Testen Ihrer Lösungsdateien

Sie haben überprüft, ob die Portalbenutzeroberfläche wie erwartet funktioniert. Vergewissern Sie sich nun, dass die Datei „createUiDefinition“ ordnungsgemäß in die Datei „mainTemplate.json“ integriert wurde. Sie können einen Überprüfungsskripttest ausführen, um den Inhalt der Lösungsdateien, einschließlich der Datei „createUiDefinition“, zu testen. Das Skript überprüft die JSON-Syntax, sucht in den Textfeldern nach regulären Ausdrücken und stellt sicher, dass die Ausgabewerte der Portalbenutzeroberfläche mit den Parametern der Vorlage übereinstimmen. Informationen zum Ausführen dieses Skripts finden Sie unter [Run static validation checks for templates](https://aka.ms/arm-ttk) (Ausführen von statischen Überprüfungen für Vorlagen).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie die Portalbenutzeroberfläche überprüft haben, lesen Sie mehr darüber, wie Sie Ihre [verwaltete Azure-Anwendung im Marketplace verfügbar machen](../../marketplace/azure-app-offer-setup.md).