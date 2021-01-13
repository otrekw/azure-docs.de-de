---
author: mikeparker104
ms.author: miparker
ms.date: 06/02/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 537c86512db4706077d0089a4f71ee945623c26b
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2020
ms.locfileid: "85095302"
---
### <a name="configure-your-notification-hub-with-apns-information"></a>Konfigurieren Ihres Notification Hubs mit APNs-Informationen

Wählen Sie unter **Notification Services** die Option **Apple** aus, und führen Sie dann je nach der Methode, die Sie im Abschnitt [Erstellen eines Zertifikats für Notification Hubs](#creating-a-certificate-for-notification-hubs) ausgewählt haben, die entsprechenden Schritte aus.  

> [!NOTE]
> Verwenden Sie **Produktion** nur dann als **Anwendungsmodus**, wenn Sie Pushbenachrichtigungen an Benutzer senden möchten, die Ihre App im Store erworben haben.

### <a name="option-1-using-a-p12-push-certificate"></a>OPTION 1: Verwenden eines P12-Zertifikats

1. Wählen Sie **Certificate**aus.

1. Wählen Sie das Dateisymbol aus.

1. Wählen Sie die zuvor exportierte P12-Datei aus, und wählen Sie dann **Öffnen**.

1. Geben Sie bei Bedarf das richtige Kennwort an.

1. Wählen Sie den Modus **Sandbox** aus.

1. Wählen Sie **Speichern** aus.

### <a name="option-2-using-token-based-authentication"></a>OPTION 2: Verwenden der tokenbasierten Authentifizierung

1. Wählen Sie **Token** aus.
1. Geben Sie die folgenden Werte ein, die Sie zuvor abgerufen haben:

    - **Schlüssel-ID**
    - **Bundle-ID**
    - **Team-ID**
    - **Token**

1. Wählen Sie **Sandbox** aus.
1. Wählen Sie **Speichern** aus.
