---
title: include file
description: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/30/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: b248bbb526baf355faf2564358884fd83422b037
ms.sourcegitcommit: f27b045f7425d1d639cf0ff4bcf4752bf4d962d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/23/2020
ms.locfileid: "77565083"
---
[Azure Files](../articles/storage/files/storage-files-introduction.md) unterstützt die identitätsbasierte Authentifizierung per SMB (Server Message Block) über [Active Directory (AD)](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview) (Vorschauversion) und [Azure Active Directory Domain Services (Azure AD DS)](../articles/active-directory-domain-services/overview.md) (allgemein verfügbare Version). In diesem Artikel erfahren Sie, wie Azure Files Domänendienste lokal oder in Azure nutzen kann, um den identitätsbasierten Zugriff auf Azure Files über SMB zu unterstützen. Dadurch können Sie Ihre vorhandenen Dateiserver problemlos durch Azure Files ersetzen und ihren bereits vorhandenen Verzeichnisdienst weiterverwenden, ohne den Benutzerzugriff auf Freigaben zu beeinträchtigen. 

Azure Files erzwingt die Autorisierung für den Benutzerzugriff sowohl auf der Freigabe- als auch auf der Verzeichnis-/Dateiebene. Die Berechtigungszuweisung auf der Freigabeebene kann Azure AD-Benutzern oder -Gruppen zugewiesen werden, die über das typische Modell der [rollenbasierten Zugriffssteuerung](../articles/role-based-access-control/overview.md) (Role-Based Access Control, RBAC) verwaltet werden. Bei der RBAC müssen die für den Dateizugriff verwendeten Anmeldeinformationen verfügbar sein oder mit Azure AD synchronisiert werden. Sie können Benutzern oder Gruppen in Azure AD integrierte RBAC-Rollen wie „Leser für Speicherdateidaten-SMB-Freigabe“ zuweisen, um Lesezugriff auf eine Azure-Dateifreigabe zu gewähren.

Auf der Verzeichnis-/Dateiebene unterstützt Azure Files die Beibehaltung, Vererbung und Erzwingung von [Windows-DACLs](https://docs.microsoft.com/windows/win32/secauthz/access-control-lists) (genau wie jeder andere Windows-Dateiserver). Wenn Sie Daten von einer Dateifreigabe über SMB in Azure Files kopieren (oder umgekehrt), können Windows-DACLs auf Wunsch beibehalten werden. Mit Azure Files können Sie ACLs zusammen mit Ihren Daten sichern, ganz gleich, ob Sie die Autorisierung erzwingen möchten. 
