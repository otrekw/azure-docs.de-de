---
title: Hinzufügen und Verwalten von Azure AD-Anwendungen – Azure Marketplace
description: Hier erfahren Sie, wie Sie Azure AD-Anwendungen für das Programm „Kommerzieller Marketplace“ in Partner Center hinzufügen und verwalten.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
ms.date: 04/06/2021
author: varsha-sarah
ms.author: vavargh
ms.custom: contperf-fy21q2
ms.openlocfilehash: be527647466ad76455585e16baabb26e39e42193
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107107988"
---
# <a name="add-and-manage-azure-ad-applications"></a>Hinzufügen und Verwalten von Azure AD-Anwendungen

**Geeignete Rollen**

- Besitzer
- Manager

Sie können Anwendungen oder Diensten, die zum Azure Active Directory (Azure AD) Ihres Unternehmen gehören, Zugriff auf Ihr Partner Center-Konto gewähren.

## <a name="add-existing-azure-ad-applications"></a>Hinzufügen vorhandener Azure AD-Anwendungen

So fügen Sie Anwendungen hinzu, die bereits im Azure Active Directory Ihres Unternehmens vorhanden sind

1. Wählen Sie auf der Seite **Benutzer** (unter **Kontoeinstellungen**) die Option **Azure AD-Anwendungen hinzufügen** aus.
1. Wählen Sie eine oder mehrere Azure AD-Anwendungen aus der angezeigten Liste aus. Über das Suchfeld können Sie bestimmte Azure AD-Anwendungen suchen. Wenn Sie mehrere Azure AD-Anwendungen auswählen, die Ihrem Partner Center-Konto hinzugefügt werden sollen, müssen Sie diesen dieselbe Rolle oder denselben Satz benutzerdefinierter Berechtigungen zuweisen. Um mehrere Azure AD-Anwendungen mit unterschiedlichen Rollen/Berechtigungen hinzuzufügen, wiederholen Sie diese Schritte für jede Rolle bzw. für jeden Satz benutzerdefinierter Berechtigungen.
1. Wenn Sie alle Azure AD-Anwendungen ausgewählt haben, wählen Sie **Ausgewählte hinzufügen** aus.
1. Geben Sie im Abschnitt **Rollen** die Rolle(n) oder die benutzerdefinierten Berechtigungen für die ausgewählten Azure AD-Anwendungen an.
1. Wählen Sie **Speichern** aus.

## <a name="add-new-azure-ad-applications"></a>Hinzufügen neuer Azure AD-Anwendungen

Wenn Sie einem neuen Azure AD-Anwendungskonto Partner Center-Zugriff gewähren möchten, erstellen Sie dieses im Abschnitt **Benutzer**. Hierdurch wird ein neues Konto im Geschäftskonto (Azure AD-Mandant) Ihres Unternehmens und nicht nur in Ihrem Partner Center-Konto erstellt. Wenn Sie diese Azure AD-Anwendung in erster Linie für die Partner Center-Authentifizierung verwenden und die Benutzer keinen direkten Zugriff benötigen, können Sie alle gültigen Adressen für die **Antwort-URL** und den **App-ID-URI** eingeben, sofern diese Werte noch nicht von einer anderen Azure AD-Anwendung in Ihrem Verzeichnis verwendet werden.

1. Wählen Sie auf der Seite **Benutzer** (unter **Kontoeinstellungen**) die Option **Azure AD-Anwendungen hinzufügen** aus.
1. Wählen Sie auf der nächsten Seite **Neue Azure AD-Anwendung** aus.
1. Geben Sie die **Antwort-URL** für die neue Azure AD-Anwendung ein. Dies ist die URL, über die sich die Benutzer anmelden und Ihre Azure AD-Anwendung verwenden können (auch bekannt als App-URL oder Anmelde-URL). Die *Antwort-URL* darf nicht länger als 256 Zeichen sein und muss in Ihrem Verzeichnis eindeutig sein.
1. Geben Sie den **App-ID-URI** für die neue Azure AD-Anwendung ein. Dies ist ein logischer Bezeichner für die Azure AD-Anwendung, der angezeigt wird, wenn eine Anforderung für einmaliges Anmelden an Azure AD gesendet wird. Der *App-ID-URI* muss für jede Azure AD-Anwendung in Ihrem Verzeichnis eindeutig sein. Diese ID darf nicht länger als 256 Zeichen sein. Weitere Informationen zum App-ID-URI finden Sie unter [Integrieren von Anwendungen in Azure Active Directory](/azure/active-directory/develop/quickstart-modify-supported-accounts.md#change-the-application-registration-to-support-different-accounts).
1. Geben Sie im Abschnitt **Rollen** die Rolle(n) oder die benutzerdefinierten Berechtigungen für die Azure AD-Anwendung an.
1. Wählen Sie **Speichern** aus.

Nachdem Sie eine Azure AD-Anwendung hinzugefügt oder erstellt haben, können Sie zum Abschnitt **Benutzer** zurückkehren und den Anwendungsnamen auswählen, um die Einstellungen für die Anwendung zu überprüfen, zum Beispiel die Mandanten-ID, Client-ID, Antwort-URL und App-ID-URI.

## <a name="remove-an-azure-ad-application"></a>Entfernen einer Azure AD-Anwendung

Um eine Anwendung aus Ihrem Geschäftskonto (Azure AD-Mandant) zu entfernen, wechseln Sie zu **Benutzer** (unter **Kontoeinstellungen**), wählen Sie über das Kontrollkästchen in der Spalte ganz rechts die Anwendung aus, die Sie entfernen möchten, und wählen Sie anschließend aus den verfügbaren Aktionen **Entfernen** aus. In einem Popupfenster können Sie bestätigen, dass Sie die ausgewählten Anwendungen entfernen möchten.

## <a name="manage-keys-for-an-azure-ad-application"></a>Verwalten von Schlüsseln für eine Azure AD-Anwendung

Wenn Ihre Azure AD-Anwendung Daten in Microsoft Azure AD liest und schreibt, benötigt sie einen Schlüssel. Sie können Schlüssel für eine Azure AD-Anwendung erstellen, indem Sie die zugehörigen Informationen in Partner Center bearbeiten. Außerdem können Sie nicht mehr benötigte Schlüssel entfernen.

1. Wählen Sie auf der Seite **Benutzer** (unter **Kontoeinstellungen**) den Namen der Azure AD-Anwendung aus. Alle aktiven Schlüssel der Azure AD-Anwendung werden angezeigt, einschließlich des Datums, an dem der Schlüssel erstellt wurde, und des Ablaufdatums (50).
1. Um einen Schlüssel zu entfernen, der nicht mehr benötigt wird, wählen Sie **Entfernen** aus.
1. Wählen Sie zum Hinzufügen eines neuen Schlüssels **Neuen Schlüssel hinzufügen** aus.
1. In einem Bildschirm werden die **Client-ID** und die **Schlüsselwerte** angezeigt. Da Sie nach Verlassen der Seite nicht mehr auf diese Informationen zugreifen können, sollten Sie sie unbedingt drucken oder kopieren.
1. Wenn Sie weitere Schlüssel erstellen möchten, wählen Sie **Weiteren Schlüssel hinzufügen** aus.
