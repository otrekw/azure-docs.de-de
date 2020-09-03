---
title: 'Hybrididentität: Vergleich von Tools für die Verzeichnisintegration | Microsoft-Dokumentation'
description: Diese Seite enthält eine umfassende Tabelle mit einem Vergleich der verschiedenen Tools für die Verzeichnisintegration.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 1e62a4bd-4d55-4609-895e-70131dedbf52
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/07/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: d9c8f5a75fea6ddd85026407fe4784c8a4589e33
ms.sourcegitcommit: c94a177b11a850ab30f406edb233de6923ca742a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2020
ms.locfileid: "89278308"
---
# <a name="hybrid-identity-directory-integration-tools-comparison"></a>Vergleich von Tools für die Verzeichnisintegration für Hybrid-Identitäten
Im Laufe der Jahre sind die Tools für die Verzeichnisintegration umfangreicher geworden und wurden weiterentwickelt.  


- [FIM](/previous-versions/windows/desktop/forefront-2010/ff182370(v=vs.100)) und [MIM](/microsoft-identity-manager/microsoft-identity-manager-2016) werden weiterhin unterstützt; sie ermöglichen hauptsächlich die Synchronisierung zwischen lokalen Systemen.   Der [FIM Windows Azure AD Connector](/previous-versions/mim/dn511001(v=ws.10)) wird sowohl in FIM als auch in MIM unterstützt, er wird jedoch für neue Bereitstellungen nicht empfohlen. Kunden mit lokalen Ressourcen wie Notes oder SAP HCM sollten MIM verwenden, um Active Directory Domain Services (AD DS) zu füllen und anschließend mit Azure AD Connect Sync oder der Azure AD Connect-Cloudbereitstellung die Synchronisierung von AD DS zu Azure AD ausführen.
- [Azure AD Connect Sync](how-to-connect-sync-whatis.md) umfasst die Komponenten und Funktionen, die zuvor in DirSync und Azure AD Sync für die Synchronisierung zwischen AD DS-Gesamtstrukturen und Azure AD veröffentlicht wurden.  
- Die [Azure AD Connect-Cloudbereitstellung](../cloud-provisioning/what-is-cloud-provisioning.md) ist ein neuer Microsoft-Agent für die Synchronisierung von AD DS mit Azure AD. Dieser eignet sich für Szenarien wie z. B. Fusionen und Übernahmen, bei denen die AD-Gesamtstrukturen des erworbenen Unternehmens von den AD-Gesamtstrukturen des übergeordneten Unternehmens isoliert sind.

Weitere Informationen über die Unterschiede zwischen Azure AD Connect Sync und der Azure AD Connect-Cloudbereitstellung finden Sie im Artikel [Was ist die Azure AD Connect-Cloudbereitstellung?](../cloud-provisioning/what-is-cloud-provisioning.md).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](whatis-hybrid-identity.md).