---
title: Azure AD Connect Health-Anweisungen für den Datenabruf | Microsoft-Dokumentation
description: Auf dieser Seite wird beschrieben, wie Sie Daten aus Azure AD Connect Health abrufen.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
ms.service: active-directory
ms.subservice: hybrid
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 09/02/2020
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8d147d2c094923e971e52e1dbfe3f7a19776d38c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89463247"
---
# <a name="azure-ad-connect-health-instructions-for-data-retrieval"></a>Azure AD Connect Health-Anweisungen für den Datenabruf

In diesem Dokument wird beschrieben, wie Sie mit Azure AD Connect Daten aus Azure AD Connect Health abrufen.

[!INCLUDE [active-directory-app-provisioning.md](../../../includes/gdpr-intro-sentence.md)]

## <a name="retrieve-all-email-addresses-for-users-configured-for-health-alerts"></a>Rufen Sie alle E-Mail-Adressen für Benutzer ab, für die Integritätswarnungen konfiguriert sind.

Führen Sie die folgenden Schritte aus, um die E-Mail-Adressen aller Benutzer abzurufen, für die in Azure AD Connect Health der Empfang von Warnungen konfiguriert ist.

1.  Beginnen Sie auf dem Blatt „Azure Active Directory Connect Health“. Wählen Sie auf der linken Navigationsleiste **Synchronisierungsdienste** aus.
 ![Synchronisierungsdienste](./media/how-to-connect-health-data-retrieval/retrieve1.png)

2.  Klicken Sie auf die Kachel **Warnungen**.</br>
 ![Warnung](./media/how-to-connect-health-data-retrieval/retrieve3.png)

3.  Klicken Sie auf **Benachrichtigungseinstellungen**.
 ![Benachrichtigung](./media/how-to-connect-health-data-retrieval/retrieve4.png)

4.  Auf dem Blatt **Benachrichtigungseinstellung** finden Sie die Liste der E-Mail-Adressen, die als Empfänger für Benachrichtigungen zu Integritätswarnungen aktiviert sind.
 ![E-Mails](./media/how-to-connect-health-data-retrieval/retrieve5a.png)
 
## <a name="retrieve-accounts-that-were-flagged-with-ad-fs-bad-password-attempts"></a>Abrufen von Konten, die mit ungültigen Versuchen der AD FS-Kennworteingabe gekennzeichnet sind

Führen Sie die folgenden Schritte aus, um Konten abzurufen, die mit ungültigen Versuchen der AD FS-Kennworteingabe gekennzeichnet sind.

1.  Beginnen Sie auf dem Blatt „Azure Active Directory Health“. Wählen Sie **Synchronisierungsfehler** aus.
 ![Synchronisierungsfehler](./media/how-to-connect-health-data-retrieval/retrieve6.png)

2.  Klicken Sie auf dem Blatt **Synchronisierungsfehler** auf **Exportieren**. Dadurch wird eine Liste der aufgezeichneten Synchronisierungsfehler exportiert.
 ![Export](./media/how-to-connect-health-data-retrieval/retrieve7.png)

## <a name="next-steps"></a>Nächste Schritte
* [Azure AD Connect Health](./whatis-azure-ad-connect.md)
* [Installieren des Azure AD Connect Health-Agents](how-to-connect-health-agent-install.md)
* [Azure AD Connect Health-Vorgänge](how-to-connect-health-operations.md)
* [Azure AD Connect Health – FAQ](reference-connect-health-faq.md)
* [Azure AD Connect Health: Versionsverlauf](reference-connect-health-version-history.md)