---
title: Deinstallieren von Azure AD Connect
description: In diesem Dokument wird beschrieben, wie Sie Azure AD Connect deinstallieren.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/09/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: f5eb537a70c69745c8067ffb71cfb895a0875945
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "96934136"
---
# <a name="uninstall-azure-ad-connect"></a>Deinstallieren von Azure AD Connect

In diesem Dokument wird beschrieben, wie Sie Azure AD Connect richtig deinstallieren.

## <a name="uninstall-azure-ad-connect-from-the-server"></a>Deinstallieren von Azure AD Connect vom Server
Als Erstes müssen Sie Azure AD Connect von dem Server entfernen, auf dem der Dienst ausgeführt wird.  Führen Sie die folgenden Schritte durch:

 1. Navigieren Sie auf dem Server, auf dem Azure AD Connect ausgeführt wird, zu **Systemsteuerung**.
 2. Klicken Sie auf **Programm deinstallieren**
 ![Programm deinstallieren](media/how-to-connect-uninstall/uninstall-1.png).</br>
 
 3. Wählen Sie **Azure AD Connect** aus.
 ![Auswählen von Azure AD Connect](media/how-to-connect-uninstall/uninstall-2.png)</br>
 
 4. Wenn Sie zur Bestätigung aufgefordert werden, klicken Sie auf **Ja**.
 5. Nach dieser Bestätigung wird der Bildschirm von Azure AD Connect angezeigt.  Klicken Sie auf **Entfernen**.
 ![Remove](media/how-to-connect-uninstall/uninstall-3.png)</br>
 
 6. Klicken Sie nach Abschluss der Aktion auf **Beenden**.
 7. ![Beenden](media/how-to-connect-uninstall/uninstall-4.png)</br>
 
 8. Klicken Sie in der **Systemsteuerung** auf **Aktualisieren**. Alle Komponenten sollten entfernt worden sein.


## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](whatis-hybrid-identity.md).
- [Installieren von Azure AD Connect mithilfe einer vorhandenen ADSync-Datenbank](how-to-connect-install-existing-database.md)
- [Installieren von Azure AD Connect mit delegierten SQL-Administratorberechtigungen](how-to-connect-install-sql-delegation.md)

