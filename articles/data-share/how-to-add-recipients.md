---
title: Hinzufügen von Empfängern in Azure Data Share
description: Erfahren Sie, wie Sie einer vorhandenen Datenfreigabe in Azure Data Share Empfänger hinzufügen.
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: how-to
ms.date: 12/17/2020
ms.openlocfilehash: a8e3dac620873ab11ae24395310066037f6d2df4
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "97680644"
---
# <a name="how-to-add-a-recipient-to-your-share"></a>Hinzufügen eines Empfängers zur Freigabe

Sie können einen Empfänger beim Erstellen einer neuen Freigabe oder in einer vorhandenen Freigabe hinzufügen. Auf der Azure Data Share-Benutzeroberfläche können Sie einen Empfänger mithilfe der E-Mail-Adresse für die Azure-Anmeldung des Benutzers hinzufügen.  Bei der API können Sie eine Kombination aus Benutzer-/Dienstprinzipal- und Mandanten-ID verwenden. Wenn eine Mandanten-ID angegeben wird, kann die Einladung nur in diesem Mandanten akzeptiert werden. Außerdem können Sie über die API eine Einladung erstellen, ohne eine E-Mail an den Empfänger zu senden. 

## <a name="add-recipient-to-an-existing-share"></a>Hinzufügen eines Empfängers zu einer vorhandenen Freigabe

Navigieren Sie in Azure Data Share zur gesendeten Freigabe, und wählen Sie die Registerkarte **Einladungen** aus. Hier werden alle Empfänger von Einladungen zu dieser Datenfreigabe aufgeführt. Um einen neuen Empfänger hinzuzufügen, klicken Sie auf **Empfänger hinzufügen**.

![Screenshot mit ausgewählter Option „Empfänger hinzufügen“.](./media/how-to/how-to-add-recipients/add-recipient.png)

Im rechten Bereich der Seite wird ein Panel geöffnet. Klicken Sie auf **Empfänger hinzufügen**, und geben Sie in der leeren Zeile die E-Mail-Adresse Ihres neuen Empfängers ein. Stellen Sie sicher, dass Sie die E-Mail-Adresse für die Azure-Anmeldung des Empfängers verwenden. (Ein E-Mail-Alias funktioniert nicht.) 

![Screenshot des Bereichs „Empfänger hinzufügen“, in dem Sie eine Einladung hinzufügen und senden können.](./media/how-to/how-to-add-recipients/add-recipient-side.png)

Klicken Sie auf **Bestätigen und Einladung senden**. Die neuen Empfänger erhalten Einladungs-E-Mails für diese Freigabe.

## <a name="next-steps"></a>Nächste Schritte
Informationen zum [Löschen einer Einladung oder einer Freigabe](how-to-delete-invitation.md).
