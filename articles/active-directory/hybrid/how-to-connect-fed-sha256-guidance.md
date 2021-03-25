---
title: Ändern des Signaturhashalgorithmus für die Vertrauensstellung der vertrauenden Seite von Microsoft 365 – Azure
description: Diese Seite enthält eine Anleitung zum Ändern des SHA-Algorithmus für die Verbundvertrauensstellung mit Microsoft 365.
keywords: SHA1,SHA256,M365,verbund,aadconnect,adfs,ad fs,sha ändern,verbundvertrauensstellung,vertrauensstellung der vertrauenden Seite
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: cf6880e2-af78-4cc9-91bc-b64de4428bbd
ms.service: active-directory
ms.subservice: hybrid
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 10/26/2018
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6bf9347d4d14e6583febd4ffaf0447e912133b80
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "89660925"
---
# <a name="change-signature-hash-algorithm-for-microsoft-365-relying-party-trust"></a>Ändern des Signaturhashalgorithmus für die Vertrauensstellung der vertrauenden Seite von Microsoft 365 – Azure
## <a name="overview"></a>Übersicht
Active Directory-Verbunddienste (Active Directory Federation Services, AD FS) signieren die eigenen Token in Microsoft Azure Active Directory, um sicherzustellen, dass sie nicht manipuliert werden können. Diese Signatur kann auf SHA-1 oder SHA-256 basieren. Azure Active Directory unterstützt jetzt Token, die mit einem SHA-256-Algorithmus signiert wurden, und es wird empfohlen, den Signaturalgorithmus für Token auf SHA-256 festzulegen, um ein Höchstmaß an Sicherheit zu erzielen. Dieser Artikel erläutert, wie Sie den Tokensignaturalgorithmus auf das sicherere Level SHA-256 festlegen.

>[!NOTE]
>Microsoft empfiehlt die Verwendung von SHA256 als Algorithmus für die Signatur von Tokens, da es sicherer als SHA1 ist. SHA1 ist jedoch nach wie vor eine unterstützte Option.

## <a name="change-the-token-signing-algorithm"></a>Ändern des Tokensignaturalgorithmus
Nachdem Sie den Signaturalgorithmus mit einer der beiden unten aufgeführten Vorgehensweisen festgelegt haben, signiert AD FS die Token für die Vertrauensstellung der vertrauenden Seite von Microsoft 365 mit SHA256. Sie brauchen keine zusätzlichen Konfigurationsänderungen vorzunehmen und können trotz dieser Änderung auf Microsoft 365 oder andere Azure AD-Anwendungen zugreifen.

### <a name="ad-fs-management-console"></a>AD FS-Verwaltungskonsole
1. Öffnen Sie die AD FS-Verwaltungskonsole auf dem primären AD FS-Server.
2. Erweitern Sie den AD FS-Knoten, und klicken Sie auf **Vertrauensstellungen der vertrauenden Seite**.
3. Klicken Sie mit der rechten Maustaste auf die Vertrauensstellung der vertrauenden Seite von Microsoft 365/Azure, und wählen Sie **Eigenschaften** aus.
4. Wählen Sie die Registerkarte **Erweitert** , und wählen Sie den sicheren Hashalgorithmus SHA-256 aus.
5. Klicken Sie auf **OK**.

![SHA-256-Signaturalgorithmus – MMC](./media/how-to-connect-fed-sha256-guidance/mmc.png)

### <a name="ad-fs-powershell-cmdlets"></a>AD FS PowerShell-Cmdlets
1. Öffnen Sie PowerShell mit Administratorrechten auf einem beliebigen AD FS-Server.
2. Legen Sie den sicheren Hashalgorithmus mithilfe des Cmdlets **Set-AdfsRelyingPartyTrust** fest.
   
   <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'https://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## <a name="also-read"></a>Weitere Informationen
* [Reparieren der Microsoft 365-Vertrauensstellung mit Azure AD Connect](how-to-connect-fed-management.md#repairthetrust)

