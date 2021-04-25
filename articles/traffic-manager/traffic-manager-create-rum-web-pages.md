---
title: Benutzer-Realmessungen mit Webseiten – Azure Traffic Manager
description: In diesem Artikel erfahren Sie, wie Sie Ihre Webseiten für das Senden von Benutzer-Realmessungen an Azure Traffic Manager einrichten.
services: traffic-manager
documentationcenter: traffic-manager
author: duongau
manager: kumud
ms.service: traffic-manager
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 04/06/2021
ms.author: duau
ms.openlocfilehash: 7ec91945c901f46d7036e8c474f9f5f0714ce65e
ms.sourcegitcommit: d63f15674f74d908f4017176f8eddf0283f3fac8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2021
ms.locfileid: "106580377"
---
# <a name="how-to-send-real-user-measurements-to-azure-traffic-manager-using-web-pages"></a>Senden von Benutzer-Realmessungen an Azure Traffic Manager mithilfe von Webseiten

Sie können Ihre Webseiten so konfigurieren, dass Benutzer-Realmessungen an Traffic Manager gesendet werden. Dazu müssen Sie einen entsprechenden Schlüssel abrufen und den generierten Code in eine Webseite einbetten.

## <a name="obtain-a-real-user-measurements-key"></a>Abrufen eines Schlüssels für Benutzer-Realmessungen (RUM)

Die von Ihrer Clientanwendung erfassten und an Traffic Manager gesendeten Messungen werden vom Dienst mithilfe einer eindeutigen Zeichenfolge bestimmt, die **RUM-Schlüssel** (Real User Measurements, Benutzer-Realmessungen) genannt wird. Sie können einen RUM-Schlüssel über das Azure-Portal, eine REST-API oder mithilfe von PowerShell oder Azure CLI abrufen.

So rufen Sie den RUM-Schlüssel im Azure-Portal ab
1. Melden Sie sich im Browser beim Azure-Portal an. Falls Sie noch nicht über ein Konto verfügen, können Sie sich für eine kostenlose einmonatige Testversion registrieren.

1. Suchen Sie über die Suchleiste des Portals nach dem Namen des Traffic Manager-Profils, das Sie ändern möchten, und wählen Sie in den angezeigten Ergebnissen das Traffic Manager-Profil aus.

1. Wählen Sie auf der Seite „Traffic Manager-Profil“ unter **Einstellungen** die Option **Benutzer-Realmessungen** aus.

1. Wählen Sie **Schlüssel generieren** aus, um einen neuen RUM-Schlüssel zu erstellen.

    :::image type="content" source="./media/traffic-manager-create-rum-web-pages/generate-rum-key.png" alt-text="Screenshot des Generierens eines Schlüssels."::: 

   **Abbildung 1: Erstellung eines Schlüssels für Benutzer-Realmessungen**

1. Die Seite zeigt nun den generierten RUM-Schlüssel und einen JavaScript-Codeausschnitt, der in Ihre HTML-Seite eingebettet werden muss.

    :::image type="content" source="./media/traffic-manager-create-rum-web-pages/rum-javascript-code.png" alt-text="Screenshot des generierten JavaScript-Codes."::: 

    **Abbildung 2: Schlüssel für Benutzer-Realmessungen und JavaScript für Messung**
 
1. Wählen Sie die Schaltfläche **Kopieren** aus, um den JavaScript-Code zu kopieren. 

> [!IMPORTANT]
> Verwenden Sie das generierte JavaScript, damit Benutzer-Realmessungen ordnungsgemäß funktionieren. Änderungen an diesem Skript oder den Skripts, die von Benutzer-Realmessungen verwendet werden, können zu einem unvorhersehbarem Verhalten führen.

## <a name="embed-the-code-to-an-html-web-page"></a>Einbetten des Codes in eine HTML-Webseite

Nachdem Sie den RUM-Schlüssel erhalten haben, müssen Sie dieses kopierte JavaScript in eine HTML-Seite einbetten, die Ihre Endbenutzer besuchen. Das Editieren von HTML kann auf viele verschiedene Arten und mit verschiedenen Tools und Workflows erfolgen. Dieses Beispiel zeigt, wie Sie eine HTML-Seite aktualisieren, um dieses Skript hinzuzufügen. Sie können diese Anleitung an Ihren Workflow zur Verwaltung von HTML-Quellcode anpassen.

1. Öffnen Sie die HTML-Seite in einem Text-Editor.

1. Fügen Sie den JavaScript-Code, den Sie im letzten Abschnitt kopiert haben, in den BODY-Abschnitt des HTML-Codes ein. Der kopierte Code befindet sich in Zeile 8 und 9, siehe Abbildung 3.

    :::image type="content" source="./media/traffic-manager-create-rum-web-pages/real-user-measurement-embed-script.png" alt-text="Screenshot des generierten JavaScript-Codes, der in die Webseite eingebettet ist."::: 

    **Abbildung 3: Einfacher HTML-Code mit eingebettetem JavaScript für Benutzer-Realmessungen**

1. Speichern Sie die HTML-Datei, und hosten Sie sie auf einem Webserver, der mit dem Internet verbunden ist.

1. Wenn diese Seite das nächste Mal in einem Webbrowser gerendert wird, wird das referenzierte JavaScript heruntergeladen. Das Skript führt die Mess- und Berichterstellungsvorgänge durch.

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [Benutzer-Realmessungen](traffic-manager-rum-overview.md)
- Informieren Sie sich über die [Funktionsweise von Traffic Manager](traffic-manager-overview.md)
- Informieren Sie sich über die von Traffic Manager unterstützten [Methoden für das Datenverkehrsrouting](traffic-manager-routing-methods.md) .
- Informieren Sie sich über das [Erstellen eines Traffic Manager-Profils](./quickstart-create-traffic-manager-profile.md)
