---
title: include file
description: include file
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 09/16/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 06db7bcb5698f152dd5062762fdb3d59ae326e22
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102603391"
---
SMB Multichannel für Azure-Dateifreigaben weist derzeit die folgenden Einschränkungen auf:
- Kann nur mit lokal redundanten FileStorage-Konten verwendet werden.
- Wird nur für Windows-Clients unterstützt. 
- Maximale Anzahl von Kanälen ist vier.
- SMB Direct wird nicht unterstützt.
- Private Endpunkte für Speicherkonten werden nicht unterstützt.
- Bei Speicherkonten mit [ identitätsbasierter Authentifizierung](../articles/storage/files/storage-files-active-directory-overview.md) von Windows Server Active Directory-Domänendiensten (AD DS) oder Azure AD DS, die für Azure Files aktiviert sind, könnten SMB-Clients nicht den Datei-Explorer von Windows verwenden, um NTFS-Berechtigungen für Verzeichnisse und Dateien zu konfigurieren.
    - Verwenden Sie stattdessen das Windows-Tool [icacls](/windows-server/administration/windows-commands/icacls) oder den Befehl [Set-ACL](/powershell/module/microsoft.powershell.security/set-acl), um Berechtigungen zu konfigurieren.

