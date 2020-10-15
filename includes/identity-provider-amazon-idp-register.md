---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 05/04/2020
ms.author: mimart
ms.openlocfilehash: de1b67265c5c3b3f7247b7f21506eed88abfd550
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "82900365"
---
1. Melden Sie sich mit den Anmeldeinformationen für Ihr Amazon-Konto bei der [Amazon Developer Konsole](https://developer.amazon.com/dashboard) an.
1. Wenn Sie dies nicht bereits getan haben, klicken Sie auf **Sign Up**, führen Sie die Schritte zur Registrierung von Entwicklern aus, und akzeptieren Sie die Richtlinie.
1. Klicken Sie im Dashboard auf **Login with Amazon** (Anmeldung mit Amazon).
1. Wählen Sie **Create a New Security Profile**.
1. Geben Sie Werte für **Security Profile Name** (Sicherheitsprofilname), **Security Profile Description** (Sicherheitsprofilbeschreibung) und **Consent Privacy Notice URL** (URL zur Zustimmung zum Datenschutzhinweis) an, z. B. `https://www.contoso.com/privacy`. Bei der URL für den Datenschutzhinweis handelt es sich um eine von Ihnen verwaltete Seite, auf der Datenschutzinformationen für Benutzer bereitgestellt werden. Klicken Sie anschließend auf **Speichern**.
1. Klicken Sie im Abschnitt **Login with Amazon Configurations** (Anmeldung mit Amazon-Konfigurationen) auf den von Ihnen erstellten **Sicherheitsprofilnamen** und das Symbol bei **Manage** (Verwalten), und wählen Sie **Web Settings** (Webeinstellungen) aus.
1. Kopieren Sie im Abschnitt **Web Settings** (Webeinstellungen) den Wert für **Client ID** (Client-ID). Wählen Sie **Show Secret** (Geheimnis anzeigen) aus, um den geheimen Clientschlüssel abzurufen, und kopieren Sie ihn dann. Sie benötigen beide Angaben, um ein Amazon-Konto als Identitätsanbieter in Ihrem Mandanten zu konfigurieren. **geheime Clientschlüssel** ist eine wichtige Sicherheitsanmeldeinformation.
1. Klicken Sie im Abschnitt **Web Settings** (Webeinstellungen) auf **Bearbeiten**. Geben Sie bei **Allowed Origins** (Zugelassene Ursprünge) und **Allowed Return URLs** (Zugelassene Rückgabe-URLs) die entsprechenden URLs ein (siehe oben). 
1. Klicken Sie auf **Speichern**.
