---
title: Verwalten des Zugriffs auf Azure-Ressourcen mit Azure AD und PIM
description: Hier erfahren Sie mehr über das Verwalten des Zugriffs auf Azure-Ressourcen mithilfe von Azure AD Privileged Identity Management (PIM) und rollenbasierter Zugriffskontrolle (RBAC).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: skwan
ms.assetid: ba06b8dd-4a74-4bda-87c7-8a8583e6fd14
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/30/2018
ms.author: rolyon
ms.reviewer: skwan
ms.openlocfilehash: 53c6375ce04425eae27f4c935e613cc27d058dc8
ms.sourcegitcommit: b95983c3735233d2163ef2a81d19a67376bfaf15
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2020
ms.locfileid: "77138034"
---
# <a name="manage-access-to-azure-resources-with-azure-ad-privileged-identity-management"></a>Verwalten des Zugriffs auf Azure-Ressourcen mit Azure AD Privileged Identity Management

Um privilegierte Konten vor bösartigen Cyberangriffen zu schützen, können Sie mit Azure Active Directory Privileged Identity Management (PIM) die Dauer der Aktivierung von Berechtigungen verkürzen und sich mittels Berichten und Warnungen einen besseren Überblick über deren Verwendung verschaffen. Hierfür werden Benutzern von PIM mit Beschränkungen auferlegt, indem ihnen Berechtigungen nur gemäß des Just-in-Time-Prinzips (JIT) zugeteilt oder nur für eine kurze Dauer zugewiesen und danach automatisch widerrufen werden. 

Sie können PIM nun mit der rollenbasierten Zugriffssteuerung in Azure (Role-Based Access Control, RBAC) verwenden, um den Zugriff auf Azure-Ressourcen zu verwalten, zu steuern und zu überwachen. PIM kann die Mitgliedschaft von integrierten und benutzerdefinierten Rollen verwalten, um Sie bei Folgendem zu unterstützen: 

- Bedarfsgesteuertes Aktivieren des Just-in-Time-Zugriffs auf Azure-Ressourcen
- Automatisches Ablaufen des Ressourcenzugriffs für zugewiesene Benutzer und Gruppen
- Zuweisen des temporären Zugriffs auf Azure-Ressourcen für kurze Aufgaben oder Bereitschaftszeitpläne
- Empfangen von Benachrichtigungen, wenn neuen Benutzern oder Gruppen Ressourcenzugriff zugewiesen wird und sie geeignete Zuweisungen aktivieren

Weitere Informationen finden Sie unter [Was ist Azure AD Privileged Identity Management?](../active-directory/privileged-identity-management/pim-configure.md).