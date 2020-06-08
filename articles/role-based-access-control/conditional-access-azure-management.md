---
title: Verwalten des Zugriffs auf die Azure-Verwaltung mit bedingtem Zugriff in Azure AD
description: Erfahren Sie, wie Sie den bedingten Zugriff in Azure AD nutzen, um den Zugriff auf die Azure-Verwaltung zu steuern.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: skwan
ms.assetid: 0adc8b11-884e-476c-8c43-84f9bf12a34b
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2019
ms.author: rolyon
ms.reviewer: skwan
ms.openlocfilehash: 2a52635dbaa7a76034f3a535b099320a901e8c07
ms.sourcegitcommit: 0690ef3bee0b97d4e2d6f237833e6373127707a7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83758774"
---
# <a name="manage-access-to-azure-management-with-conditional-access"></a>Verwalten des Zugriffs auf die Azure-Verwaltung mit bedingtem Zugriff

> [!CAUTION]
> Stellen Sie sicher, dass Sie die Funktionsweise des bedingten Zugriffs verstehen, bevor Sie eine Richtlinie einrichten, um den Zugriff auf die Azure-Verwaltung zu steuern. Versichern Sie sich, dass Sie keine Bedingungen erstellen, die Ihren eigenen Zugriff auf das Portal blockieren könnten.

Der bedingte Zugriff in Azure Active Directory (Azure AD) steuert basierend auf den von Ihnen angegebenen Bedingungen den Zugriff auf Cloud-Apps. Um den Zugriff zuzulassen, erstellen Sie basierend darauf, ob die Anforderungen in der Richtlinie erfüllt werden, bedingte Zugriffsrichtlinien, die den Zugriff erlauben oder blockieren. 

Normalerweise verwenden Sie den bedingten Zugriff zum Steuern des Zugriffs auf Ihre Cloud-Apps. Sie können auch Richtlinien einrichten, um den Zugriff auf die Azure-Verwaltung basierend auf bestimmten Bedingungen (wie das Anmelderisiko, der Standort oder das Gerät) zu steuern und um Anforderungen wie die mehrstufige Authentifizierung zu erzwingen.

Klicken Sie für die Erstellung einer Richtlinie für die Azure-Verwaltung unter **Cloud-Apps** auf **Microsoft Azure-Verwaltung**, wenn Sie die App auswählen, für welche die Richtlinie gelten soll.

![Bedingter Zugriff für die Azure-Verwaltung](./media/conditional-access-azure-management/conditional-access-azure-mgmt.png)

Die von Ihnen erstellte Richtlinie gilt für alle Azure-Verwaltungsendpunkte, einschließlich der folgenden:

- Azure-Portal
- Azure Resource Manager-Anbieter
- Klassische Dienstverwaltungs-APIs
- Azure PowerShell
- Administratorportal für Visual Studio-Abonnements
- Azure DevOps
- Azure Data Factory-Portal

Hinweis: Die Richtlinie gilt für Azure PowerShell, von dem die Azure Resource Manager-API aufgerufen wird. Sie gilt nicht für [Azure AD PowerShell](/powershell/azure/active-directory/install-adv2), von dem Microsoft Graph aufgerufen wird.

Weitere Informationen zum Einrichten einer Beispielrichtlinie zum Aktivieren des bedingten Zugriffs für die Microsoft Azure-Verwaltung finden Sie im Artikel [Bedingter Zugriff: Vorschreiben der MFA für die Azure-Verwaltung](../active-directory/conditional-access/howto-conditional-access-policy-azure-management.md).
