---
title: Die Benutzerbereitstellung für eine Azure AD-Katalog-App dauert mehrere Stunden
description: Erfahren Sie, wie Sie herausfinden, warum die Bereitstellung für die Anwendung möglicherweise länger als erwartet dauert.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: bb22d19733fbeb162fba55dd732c10e552879c78
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "77522644"
---
# <a name="user-provisioning-to-an-azure-ad-gallery-application-is-taking-hours-or-more"></a>Die Benutzerbereitstellung für eine Azure AD-Kataloganwendung dauert Stunden oder länger

Bei der ersten Aktivierung der automatischen Bereitstellung für eine Anwendung kann der Startzyklus zwischen 20 Minuten und mehreren Stunden dauern. Dies hängt von der Größe des Azure AD-Verzeichnisses und der Anzahl der Benutzer im Bereitstellungsbereich ab. 

Nachfolgende Synchronisierungen nach dem Startzyklus werden schneller durchgeführt, weil der Bereitstellungsdienst Wasserzeichen speichert, die den Status beider Systeme nach dem Startzyklus darstellen. Dadurch verbessert sich die Leistung nachfolgender Synchronisierungen.

## <a name="how-to-improve-provisioning-performance"></a>Verbessern der Bereitstellungsleistung

Wenn der Startzyklus länger als einige Stunden dauert, haben Sie eine Möglichkeit, die Leistung zu verbessern:

-   **Benutzerbereichsfilter** Mit Bereichsfiltern können Sie die Daten, die der Bereitstellungsdienst aus Azure AD extrahiert, fein abstimmen, indem Sie Benutzer anhand bestimmter Attributwerte herausfiltern. Weitere Informationen zu Bereichsfiltern finden Sie unter [Attributbasierte Anwendungsbereitstellung mit Bereichsfiltern](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

## <a name="next-steps"></a>Nächste Schritte
[Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](user-provisioning.md)

