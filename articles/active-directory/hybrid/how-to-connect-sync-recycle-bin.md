---
title: 'Azure AD Connect-Synchronisierung: Aktivieren des Active Directory-Papierkorbs | Microsoft-Dokumentation'
description: In diesem Thema wird die Verwendung des AD-Papierkorbfeatures für Azure AD Connect empfohlen.
services: active-directory
keywords: AD-Papierkorb, versehentliches Löschen, Quellanker
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: afec4207-74f7-4cdd-b13a-574af5223a90
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 12/17/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 12073a75cd248c9226c7ce5ecc21b64617823b32
ms.sourcegitcommit: c94a177b11a850ab30f406edb233de6923ca742a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2020
ms.locfileid: "89279634"
---
# <a name="azure-ad-connect-sync-enable-ad-recycle-bin"></a>Azure AD Connect-Synchronisierung: Aktivieren des AD-Papierkorbs
Es wird empfohlen, den Active Directory-Papierkorb für Ihre lokalen Active Directory-Instanzen, die mit Azure AD synchronisiert werden, zu aktivieren. 

Wenn Sie versehentlich ein lokales AD-Benutzerobjekt löschen und mit dem Feature wiederherstellen, stellt Azure AD das entsprechende Azure AD-Benutzerobjekt wieder her.  Informationen zum AD-Papierkorbfeature finden Sie im Artikel [Scenario Overview for Restoring Deleted Active Directory Objects](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd379542(v=ws.10)) (Szenarioübersicht über das Wiederherstellen gelöschter Active Directory-Objekte).

## <a name="benefits-of-enabling-the-ad-recycle-bin"></a>Vorteile der Aktivierung des AD-Papierkorbs
Dieses Feature unterstützt Sie folgendermaßen beim Wiederherstellen von Azure AD-Benutzerobjekten:

* Wenn Sie versehentlich ein lokales AD-Benutzerobjekt löschen, wird das entsprechende Azure AD-Benutzerobjekt im nächsten Synchronisierungszyklus gelöscht. Standardmäßig bewahrt Azure AD das gelöschte Azure AD-Benutzerobjekt für 30 Tage als „vorläufig gelöscht“ auf.

* Wenn Sie den lokalen AD-Papierkorb aktiviert haben, können Sie das gelöschte lokale AD-Benutzerobjekt wiederherstellen, ohne den Quellanker-Wert ändern zu müssen. Wenn das wiederhergestellte lokale AD-Benutzerobjekt mit Azure AD synchronisiert wird, stellt Azure AD das zugehörige, vorläufig gelöschte Azure AD-Benutzerobjekt wieder her. Informationen zum Quellanker-Attribut finden Sie im Artikel [Azure AD Connect: Designkonzepte](./plan-connect-design-concepts.md#sourceanchor).

* Wenn Sie das Feature für den lokalen AD-Papierkorb nicht aktiviert haben, müssen Sie möglicherweise ein AD-Benutzerobjekt erstellen, um das gelöschte Objekt zu ersetzen. Wenn der Azure AD Connect-Synchronisierungsdienst für die Verwendung von vom System generierten AD-Attributen (z.B. „ObjectGuid“) für das Quellanker-Attribut konfiguriert wurde, hat das neu erstellte AD-Benutzerobjekt nicht denselben Quellanker-Wert wie das gelöschte AD-Benutzerobjekt. Wenn das neu erstellte AD-Benutzerobjekt mit Azure AD synchronisiert wurde, erstellt Azure AD ein neues Azure AD-Benutzerobjekt, anstatt das vorläufig gelöschte Azure AD-Benutzerobjekt wiederherzustellen.

> [!NOTE]
> Standardmäßig behält Azure AD gelöschte Azure AD-Benutzerobjekte im vorläufig gelöschten Zustand für 30 Tage bei, bevor sie dauerhaft gelöscht werden. Allerdings können Administratoren das Löschen dieser Objekte beschleunigen. Sobald die Objekte dauerhaft gelöscht wurden, können sie nicht mehr wiederhergestellt werden, selbst wenn das Feature für den lokalen AD-Papierkorb aktiviert ist.

## <a name="next-steps"></a>Nächste Schritte
**Übersichtsthemen**

* [Azure AD Connect-Synchronisierung: Grundlagen und Anpassung der Synchronisierung](how-to-connect-sync-whatis.md)

* [Integrieren lokaler Identitäten in Azure Active Directory](whatis-hybrid-identity.md)