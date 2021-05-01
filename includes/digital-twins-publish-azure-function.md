---
author: baanders
description: Includedatei zur Veröffentlichung einer Azure-Funktion über Visual Studio
ms.service: digital-twins
ms.topic: include
ms.date: 1/21/2021
ms.author: baanders
ms.openlocfilehash: ddc56ab05a087c9e86d67a13aebcfb8e65fbd78f
ms.sourcegitcommit: afb79a35e687a91270973990ff111ef90634f142
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2021
ms.locfileid: "107480733"
---
Beginnen Sie im Projektmappen-Explorer, um das Projekt in einer Funktions-App in Azure zu veröffentlichen. Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie dann **Veröffentlichen** aus.

> [!IMPORTANT] 
> Für die Veröffentlichung einer Funktions-App in Azure fallen zusätzliche Gebühren für Ihr Abonnement an, unabhängig von Azure Digital Twins.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-1.png" alt-text="Screenshot: Visual Studio mit dem Kontextmenü für die Projektmappe. Im Menü ist die Option „Veröffentlichen“ hervorgehoben.":::

Behalten Sie auf der daraufhin geöffneten Seite **Veröffentlichen** die Auswahl des Standardziels **Azure** bei. Wählen Sie **Weiter** aus. 

Wenn Sie ein bestimmtes Ziel angeben möchten, müssen Sie **Azure-Funktions-App (Windows)** und **Weiter** auswählen.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-2.png" alt-text="Screenshot: Visual Studio mit dem Dialogfeld zum Veröffentlichen der Azure-Funktion. Auf der Seite „Bestimmtes Ziel“ ist „Azure-Funktions-App (Windows)“ ausgewählt.":::

Wählen Sie auf der Registerkarte **Functions-Instanz** Ihr Abonnement aus. Wählen Sie dann das Plussymbol (+) aus, um eine neue Funktion zu erstellen.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-3.png" alt-text="Screenshot: Visual Studio mit dem Dialogfeld zum Veröffentlichen der Azure-Funktion. Das Plussymbol ist hervorgehoben.":::

Füllen Sie im Fenster **Funktions-App (Windows): Neu erstellen** die folgenden Felder aus:
* **Name** ist der Name des Verbrauchsplans, der von Azure zum Hosten Ihrer Azure Functions-App verwendet wird. Dieser Name gilt auch für die Funktions-App, in der Ihre eigentliche Funktion enthalten ist. Sie können einen eindeutigen Wert auswählen oder den Standardvorschlag übernehmen.
* Achten Sie darauf, dass unter **Abonnement** das gewünschte Abonnement angegeben ist. 
* Achten Sie darauf, dass unter **Ressourcengruppe** die gewünschte Ressourcengruppe angegeben ist.
* Behalten Sie unter **Plantyp** die ausgewählte Option **Verbrauch** bei.
* Wählen Sie den **Standort** Ihrer Ressourcengruppe aus.
* Erstellen Sie eine neue **Azure Storage**-Ressource, indem Sie den Link **Neu** auswählen. Geben Sie denselben Standort wie für Ihre Ressourcengruppe an, behalten Sie die anderen Standardwerte bei, und wählen Sie dann **OK** aus.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-4.png" alt-text="Screenshot: Visual Studio mit dem Dialogfeld zum Veröffentlichen der Azure-Funktion. Die Details einer neuen Funktions-App werden ausgefüllt, einschließlich Name, Abonnement, Ressourcengruppe, Plantyp, Standort und Azure Storage.":::

Klicken Sie anschließend auf **Erstellen**.

Nachdem der App-Dienst erstellt wurde, wird die Registerkarte **Functions-Instanz** geöffnet. Ihre neue Funktions-App wird im Bereich **Funktions-Apps** unter Ihrer Ressourcengruppe angezeigt. Wählen Sie **Fertig stellen** aus.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-5.png" alt-text="Screenshot: Visual Studio mit dem Dialogfeld zum Veröffentlichen der Azure-Funktion. Die Registerkarte „Functions-Instanz“ ist ausgewählt. Die neue Funktions-App wird unter der Ressourcengruppe angezeigt.":::

Vergewissern Sie sich im Bereich **Veröffentlichen**, der im Visual Studio-Fenster geöffnet wird, dass alle Informationen stimmen. Wählen Sie anschließend **Veröffentlichen** aus.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-6.png" alt-text="Screenshot: Visual Studio mit dem Bereich „Veröffentlichen“. Die Schaltfläche „Veröffentlichen“ ist hervorgehoben.":::

> [!NOTE]
> Wird ein Popupfenster wie im folgenden Beispiel angezeigt, wählen Sie **Versuch des Abrufs der Anmeldeinformationen aus Azure** und dann **Speichern** aus.
> :::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-7.png" alt-text="Screenshot: Visual Studio-Popupfenster „Anmeldeinformationen veröffentlichen“. Es enthält Felder für Benutzername und Kennwort sowie eine Schaltfläche zum Abrufen von Anmeldeinformationen aus Azure." border="false":::
>
> Wenn eine der folgenden Warnungen angezeigt wird, befolgen Sie die Anweisungen zum Aktualisieren auf die neueste Azure Functions-Runtimeversion:
> * „Upgrade für Functions-Version in Azure“
> * „Ihre Version der Functions-Runtime stimmt nicht mit der in Azure ausgeführten Version überein.“
>
> Diese Warnungen werden möglicherweise angezeigt, wenn Sie eine alte Version von Visual Studio nutzen.

Ihre Functions-App ist jetzt in Azure veröffentlicht.
