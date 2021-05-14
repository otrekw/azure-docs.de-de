---
title: include file
description: include file
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 04/26/2021
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 04ded4340eef0bfe5bc91ee2e3f2552975df2e05
ms.sourcegitcommit: 5f785599310d77a4edcf653d7d3d22466f7e05e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2021
ms.locfileid: "108065625"
---
SMB Multichannel für Azure-Dateifreigaben weist derzeit die folgenden Einschränkungen auf:
- Wird nur für Windows-Clients unterstützt. 
- Maximale Anzahl von Kanälen ist vier.
- SMB Direct wird nicht unterstützt.
- Private Endpunkte für Speicherkonten werden nicht unterstützt.
- Bei Speicherkonten mit [ identitätsbasierter Authentifizierung](../articles/storage/files/storage-files-active-directory-overview.md) von Windows Server Active Directory-Domänendiensten (AD DS) oder Azure AD DS, die für Azure Files aktiviert sind, könnten SMB-Clients nicht den Datei-Explorer von Windows verwenden, um NTFS-Berechtigungen für Verzeichnisse und Dateien zu konfigurieren.
    - Verwenden Sie stattdessen das Windows-Tool [icacls](/windows-server/administration/windows-commands/icacls) oder den Befehl [Set-ACL](/powershell/module/microsoft.powershell.security/set-acl), um Berechtigungen zu konfigurieren.

