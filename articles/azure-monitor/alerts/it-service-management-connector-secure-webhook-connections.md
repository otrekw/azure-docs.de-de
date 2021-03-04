---
title: ITSM-Connector – Secure Export in Azure Monitor
description: In diesem Artikel erfahren Sie, wie Sie Ihre ITSM-Produkte/-Dienste mit Secure Export in Azure Monitor verbinden, um ITSM-Arbeitselemente zentral zu überwachen und zu verwalten.
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 09/08/2020
ms.openlocfilehash: 330b97e66ba1a9b95f2ef7a34a7ea046414b8bd8
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102037511"
---
# <a name="connect-azure-to-itsm-tools-by-using-secure-export"></a>Verbinden von Azure mit ITSM-Tools unter Verwendung von Secure Export

In diesem Artikel erfahren Sie, wie Sie die Verbindung zwischen Ihrem ITSM-Produkt oder -Dienst (IT-Service-Management) mithilfe von Secure Export konfigurieren.

Secure Export ist eine aktualisierte Version des [ITSM-Connectors (ITSMC)](./itsmc-overview.md). Beide Versionen ermöglichen es Ihnen, Arbeitselemente in einem ITSM-Tool zu erstellen, wenn Azure Monitor Warnungen sendet. Diese Funktionalität umfasst Metrik-, Protokoll- und Aktivitätsprotokollwarnungen.

Der ITSM-Connector verwendet Anmeldeinformationen aus Benutzername und Kennwort. Da Secure Export Azure Active Directory (Azure AD) nutzt, bietet diese Variante eine strengere Authentifizierung. Azure AD ist der cloudbasierte Identitäts- und Zugriffsverwaltungsdienst von Microsoft. Über diesen Dienst können Benutzer sich anmelden und auf interne oder externe Ressourcen zugreifen. Bei Verwendung von Azure AD mit ITSM können Azure-Warnungen (anhand der Anwendungs-ID von Azure AD) identifiziert werden, die an das externe System gesendet wurden.

> [!NOTE]
> Die Option zum Herstellen einer Verbindung zwischen Azure und ITSM-Tools über Secure Export befindet sich in der Vorschauphase.

## <a name="secure-export-architecture"></a>Secure Export-Architektur

Mit der Secure Export-Architektur werden die folgenden neuen Funktionen eingeführt:

* **Neue Aktionsgruppe**: Warnungen werden nicht über die ITSM-Aktionsgruppe, die ITSMC verwendet, sondern über die Aktionsgruppe für sichere Webhooks an das ITSM-Tool gesendet.
* **Azure AD-Authentifizierung**: Die Authentifizierung erfolgt nicht über Anmeldeinformationen aus Benutzername und Kennwort, sondern über Azure AD.

## <a name="secure-export-data-flow"></a>Secure Export-Datenfluss

Nachfolgend sind die Schritte des Secure Export-Datenflusses gezeigt:

1. Azure Monitor sendet eine Warnung, die für die Verwendung von Secure Export konfiguriert ist.
2. Die Nutzdaten der Warnung werden von einer „Sicherer Webhook“-Aktion an das ITSM-Tool gesendet.
3. Die ITSM-Anwendung überprüft mit Azure AD, ob die Warnung autorisiert ist und vom ITSM-Tool empfangen werden darf.
4. Wenn die Warnung autorisiert ist, führt die Anwendung folgende Schritte aus:
   
   1. Erstellen eines Arbeitselements (z. B. einen Incident) im ITSM-Tool.
   2. Binden der ID des Konfigurationselements an die Kundenverwaltungsdatenbank (Customer Management Database, CMDB).

![Die Abbildung zeigt, wie das ITSM-Tool mit Azure AD, Azure-Warnungen und einer Aktionsgruppe kommuniziert.](media/it-service-management-connector-secure-webhook-connections/secure-export-diagram.png)

## <a name="benefits-of-secure-export"></a>Vorteile von Secure Export

Die Integration hat unter anderem folgende Vorteile:

* **Bessere Authentifizierung**: Azure AD bietet eine sicherere Authentifizierung ohne die Timeouts, die bei ITSMC häufig auftreten.
* **Warnungen werden im ITSM-Tool behandelt**: Bei Metrikwarnungen werden die Status „ausgelöst“ und „behoben“ verwendet. Wenn die Bedingung erfüllt ist, lautet der Warnungsstatus „ausgelöst“. Ist die Bedingung nicht mehr erfüllt, lautet der Status der Warnung „behoben“. In ITSMC können Warnungen nicht automatisch behandelt werden. Bei Secure Export wird der Status „behoben“ an das ITSM-Tool übermittelt und automatisch aktualisiert.
* **[Einheitliches Warnungsschema](./alerts-common-schema.md)** : In ITSMC variiert das Schema der Nutzdaten einer Warnung basierend auf dem Warnungstyp. In Secure Export wird ein einheitliches Schema für alle Warnungstypen verwendet. Dieses gemeinsame Schema umfasst das Konfigurationselement für alle Warnungstypen. Sämtliche Warnungstypen können ihr Konfigurationselement an die CMDB binden.

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen von ITSM-Arbeitselementen aus Azure-Warnungen](./itsmc-overview.md)