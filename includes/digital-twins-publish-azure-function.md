---
author: baanders
description: Includedatei zur Veröffentlichung einer Azure-Funktion über Visual Studio
ms.service: digital-twins
ms.topic: include
ms.date: 1/21/2021
ms.author: baanders
ms.openlocfilehash: 63b393f519ad29baa05fef046ee1e8ba9e5330d8
ms.sourcegitcommit: 75041f1bce98b1d20cd93945a7b3bd875e6999d0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2021
ms.locfileid: "98701157"
---
Klicken Sie im *Projektmappen-Explorer* mit der rechten Maustaste auf das Projekt, und wählen Sie **Veröffentlichen** aus, um das Projekt in einer Funktions-App in Azure zu veröffentlichen.

> [!IMPORTANT] 
> Für die Veröffentlichung einer Funktions-App in Azure fallen zusätzliche Gebühren für Ihr Abonnement an, unabhängig von Azure Digital Twins.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-1.png" alt-text="Screenshot: Visual Studio mit dem Kontextmenü für die Projektmappe. Die Menüoption „Veröffentlichen“ ist hervorgehoben.":::

Behalten Sie auf der angezeigten Seite *Veröffentlichen* die Auswahl des Standardziels **Azure** bei, und wählen Sie die Option *Weiter* aus. 

Wenn Sie ein bestimmtes Ziel angeben möchten, müssen Sie **Azure-Funktions-App (Windows)** und dann *Weiter* auswählen.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-2.png" alt-text="Screenshot: Visual Studio mit dem Dialogfeld „Azure-Funktion veröffentlichen“. Auf der Seite „Spezifisches Ziel“ ist die Option „Azure-Funktions-App (Windows)“ ausgewählt.":::

Wählen Sie auf der Seite *Functions-Instanz* Ihr Abonnement aus. In einem Feld sollten die *Ressourcengruppen* Ihres Abonnements angezeigt werden.

Wählen Sie die Ressourcengruppe Ihrer Instanz aus, und klicken Sie auf *+* , um eine neue Azure-Funktion zu erstellen.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-3.png" alt-text="Screenshot: Visual Studio mit dem Dialogfeld „Azure-Funktion veröffentlichen“. Auf der Seite „Functions-Instanz“ ist die Plusschaltfläche zum Erstellen einer neuen Funktion hervorgehoben.":::

Füllen Sie die Felder im Fenster *Funktions-App (Windows): Neu erstellen* wie folgt aus:
* **Name** ist der Name des Verbrauchsplans, der von Azure zum Hosten Ihrer Azure Functions-App verwendet wird. Dies ist dann auch der Name der Funktions-App, in der Ihre eigentliche Funktion enthalten ist. Sie können Ihren eigenen eindeutigen Wert auswählen oder den Standardvorschlag übernehmen.
* Achten Sie darauf, dass unter **Abonnement** das gewünschte Abonnement angegeben ist. 
* Achten Sie darauf, dass unter **Ressourcengruppe** die gewünschte Ressourcengruppe angegeben ist.
* Behalten Sie unter **Plantyp** die Option *Verbrauch* bei.
* Wählen Sie den **Standort** aus, der dem Standort Ihrer Ressourcengruppe entspricht.
* Erstellen Sie eine neue **Azure Storage**-Ressource, indem Sie den Link *Neu...* verwenden. Geben Sie denselben Standort wie für Ihre Ressourcengruppe an, behalten Sie die anderen Standardwerte bei, und klicken Sie auf „OK“.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-4.png" alt-text="Screenshot: Visual Studio mit dem Dialogfeld „Azure-Funktion veröffentlichen“. Die Details einer neuen Funktions-App werden ausgefüllt, einschließlich Name, Abonnement, Ressourcengruppe, Plantyp, Standort und Azure Storage.":::

Wählen Sie anschließend **Erstellen**.

Sie sollten wieder auf die Seite *Functions-Instanz* gelangen, auf der Ihre neue Funktions-App jetzt unterhalb Ihrer Ressourcengruppe angezeigt wird. Klicken Sie auf *Fertig stellen*.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-5.png" alt-text="Azure-Funktion „Veröffentlichen“ in Visual Studio: Functions-Instanz (nach Funktions-App)":::

Vergewissern Sie sich im Bereich *Veröffentlichen*, der im Visual Studio-Fenster geöffnet wird, dass alle Informationen stimmen, und wählen Sie **Veröffentlichen** aus.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-6.png" alt-text="Screenshot: Visual Studio mit dem Dialogfeld „Azure-Funktion veröffentlichen“. Die neue Funktions-App wird in der Liste der Funktions-Apps angezeigt, und es gibt eine Schaltfläche zum Fertigstellen.":::

> [!NOTE]
> Gehen Sie wie folgt vor, wenn dieses Popupfenster angezeigt wird: :::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-7.png" alt-text="Screenshot: Visual Studio-Popupfenster „Anmeldeinformationen veröffentlichen“. Es enthält Felder für Benutzername und Kennwort sowie eine Schaltfläche zum Abrufen von Anmeldeinformationen aus Azure." border="false":::
> Wählen Sie die Option **Attempt to retrieve credentials from Azure** (Versuchtes Abrufen von Anmeldeinformationen aus Azure) und dann **Speichern** aus.
>
> Gehen Sie wie folgt vor, falls Warnungen wie *Upgrade für Functions-Version in Azure* oder *Ihre Version der Functions-Runtime stimmt nicht mit der in Azure ausgeführten Version überein* angezeigt werden:
>
> Befolgen Sie die Anweisungen zum Durchführen des Upgrades auf die neueste Version der Azure Functions-Runtime. Dieses Problem kann bei Verwendung einer älterer Visual Studio-Version auftreten.

Damit Ihre Funktions-App auf Azure Digital Twins zugreifen kann, muss sie über eine vom System verwaltete Identität und Zugriffsrechte auf Ihre Azure Digital Twins-Instanz verfügen. Dies wird im nächsten Schritt eingerichtet.
