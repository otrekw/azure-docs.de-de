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
ms.openlocfilehash: bbf0530c1a7f1a747d456d87efc106418f23b7ba
ms.sourcegitcommit: 8dd8d2caeb38236f79fe5bfc6909cb1a8b609f4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2021
ms.locfileid: "98053002"
---
SMB Multichannel für Azure-Dateifreigaben weist derzeit die folgenden Einschränkungen auf:
- Kann nur mit lokal redundanten FileStorage-Konten verwendet werden.
- Wird nur für Windows-Clients unterstützt. 
- Maximale Anzahl von Kanälen ist vier.
- SMB Direct wird nicht unterstützt.
- Private Endpunkte für Speicherkonten werden nicht unterstützt.
- Bei Speicherkonten mit [ identitätsbasierter Authentifizierung](../articles/storage/files/storage-files-active-directory-overview.md) von Windows Server Active Directory-Domänendiensten (AD DS) oder Azure AD DS, die für Azure Files aktiviert sind, könnten SMB-Clients nicht den Datei-Explorer von Windows verwenden, um NTFS-Berechtigungen für Verzeichnisse und Dateien zu konfigurieren.
    - Verwenden Sie stattdessen das Windows-Tool [icacls](/windows-server/administration/windows-commands/icacls) oder den Befehl [Set-ACL](/powershell/module/microsoft.powershell.security/set-acl?view=powershell-7), um Berechtigungen zu konfigurieren.

