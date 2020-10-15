---
title: Behandeln von Problemen bei der Installation von Azure AD Connect | Microsoft-Dokumentation
description: Dieses Thema enthält Schritte zum Behandeln von Problemen bei der Installation von Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 01/31/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 25314b4a306678dc877a95194907b3d73979e4f8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89275860"
---
# <a name="troubleshoot-azure-ad-connect-install-issues"></a>Problembehandlung: Azure AD Connect-Installationsprobleme

## <a name="recommended-steps"></a>**Empfohlene Schritte**
Überprüfen Sie, welcher [Azure AD Connect-Installationstyp](./how-to-connect-install-select-installation.md) für Sie geeignet ist. Werden die Kriterien für die Express-Installation erfüllt, wird dringend empfohlen, die Express-Installation zu verwenden. Bei der Express-Installation müssen Sie nur geringfügige Optionen angeben, um die Installation abzuschließen. Dadurch reduziert sich die Fehlerwahrscheinlichkeit. 

Wenn die Kriterien für eine Express-Installation jedoch nicht erfüllt werden und Sie die benutzerdefinierte Installation ausführen müssen, finden Sie hier einige bewährte Methoden, denen Sie folgen können, um häufig auftretende Probleme zu vermeiden. Der Einfachheit halber sind hier nur selektive Optionen aufgeführt:

* Stellen Sie sicher, dass Sie für den Computer, auf dem Sie AAD Connect installieren, Administratorrechte haben. Melden Sie sich am Computer mit den gleichen Administratoranmeldeinformationen an.

* Übernehmen Sie auf der folgenden Seite für alle Optionen die Standardeinstellungen. Eine Ausnahme bildet dabei die Option „Vorhandenen SQL Server-Computer verwenden“, wenn Sie vorhandene SQL Server-Computer verwenden möchten. Hier finden Sie [weitere Details](./how-to-connect-install-custom.md) zur Verwendung der Optionen bei einer benutzerdefinierten Installation. 

    ![Vorhandenen SQL Server-Computer verwenden](media/tshoot-connect-install-issues/tshoot-connect-install-issues/useexistingsqlserver.png)

* Wählen Sie auf der folgenden Seite die Option „Neues AD-Konto erstellen“ aus, um mögliche Probleme mit den Berechtigungen eines vorhandenen Kontos zu vermeiden.

    ![AD-Gesamtstrukturkonto](media/tshoot-connect-install-issues/tshoot-connect-install-issues/createnewaccount.png)

### <a name="common-issues"></a>**Allgemeine Probleme**

* [Konnektivitätsprobleme mit lokaler Active Directory-Instanz](./reference-connect-adconnectivitytools.md)

* [Konnektivitätsprobleme mit Azure Active Directory-Onlineinstanz](./tshoot-connect-connectivity.md)

* [Berechtigungsprobleme mit lokaler Active Directory-Instanz](./how-to-connect-configure-ad-ds-connector-account.md)

## <a name="recommended-documents"></a>**Empfohlene Dokumente**
* [Voraussetzungen für Azure AD Connect](./how-to-connect-install-prerequisites.md)
* [Auswählen des Installationstyps für Azure AD Connect](./how-to-connect-install-select-installation.md)
* [Erste Schritte mit Azure AD Connect mit Expresseinstellungen](./how-to-connect-install-express.md)
* [Benutzerdefinierte Installation von Azure AD Connect](./how-to-connect-install-custom.md)
* [Azure AD Connect: Aktualisieren von einer früheren Version auf die aktuelle Version](./how-to-upgrade-previous-version.md).
* [Azure AD Connect: Was ist ein Stagingserver?](./plan-connect-topologies.md#staging-server)
* [Was ist das PowerShell-Modul „ADConnectivityTool“?](./how-to-connect-adconnectivitytools.md)

## <a name="next-steps"></a>Nächste Schritte
- [Azure AD Connect-Synchronisierung](how-to-connect-sync-whatis.md)
- [Was ist eine Hybrididentität?](whatis-hybrid-identity.md)