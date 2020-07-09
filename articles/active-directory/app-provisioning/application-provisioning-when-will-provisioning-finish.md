---
title: Die Benutzerbereitstellung für eine Azure AD-Katalog-App dauert mehrere Stunden
description: Erfahren Sie, wie Sie herausfinden, warum die Bereitstellung für die Anwendung möglicherweise länger als erwartet dauert.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.topic: troubleshooting
ms.date: 07/11/2017
ms.author: kenwith
ms.reviewer: asteen
ms.openlocfilehash: f0f572e5153b6e79bf7731d0d57b66e601a0054c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84781972"
---
# <a name="user-provisioning-to-an-azure-ad-gallery-application-is-taking-hours-or-more"></a>Die Benutzerbereitstellung für eine Azure AD-Kataloganwendung dauert Stunden oder länger

Bei der ersten Aktivierung der automatischen Bereitstellung für eine Anwendung kann der Startzyklus zwischen 20 Minuten und mehreren Stunden dauern. Dies hängt von der Größe des Azure AD-Verzeichnisses und der Anzahl der Benutzer im Bereitstellungsbereich ab. 

Nachfolgende Synchronisierungen nach dem Startzyklus werden schneller durchgeführt, weil der Bereitstellungsdienst Wasserzeichen speichert, die den Status beider Systeme nach dem Startzyklus darstellen. Dadurch verbessert sich die Leistung nachfolgender Synchronisierungen.

## <a name="how-to-improve-provisioning-performance"></a>Verbessern der Bereitstellungsleistung

Wenn der Startzyklus länger als einige Stunden dauert, haben Sie eine Möglichkeit, die Leistung zu verbessern:

-   **Benutzerbereichsfilter** Mit Bereichsfiltern können Sie die Daten, die der Bereitstellungsdienst aus Azure AD extrahiert, fein abstimmen, indem Sie Benutzer anhand bestimmter Attributwerte herausfiltern. Weitere Informationen zu Bereichsfiltern finden Sie unter [Attributbasierte Anwendungsbereitstellung mit Bereichsfiltern](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

## <a name="next-steps"></a>Nächste Schritte
[Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](user-provisioning.md)

