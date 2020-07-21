---
title: SSL/TLS-Konnektivität in Azure Database for MySQL
description: Informationen zum Konfigurieren von Azure-Datenbank für MySQL und zugehörigen Anwendungen für die richtige Verwendung von SSL-Verbindungen
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 07/03/2020
ms.openlocfilehash: 995fba8a19ba53398d46b2871eb877e13a6eb63f
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86142365"
---
# <a name="ssltls-connectivity-in-azure-database-for-mysql"></a>SSL/TLS-Konnektivität in Azure Database for MySQL

Azure-Datenbank für MySQL unterstützt die Verbindung Ihres Datenbankservers mit Clientanwendungen, die Secure Sockets Layer (SSL) verwenden. Das Erzwingen von SSL-Verbindungen zwischen dem Datenbankserver und Clientanwendungen trägt zum Schutz vor Man-in-the-Middle-Angriffen bei, indem der Datenstrom zwischen dem Server und der Anwendung verschlüsselt wird.

## <a name="ssl-default-settings"></a>SSL-Standardeinstellungen

Der Datenbankdienst sollte standardmäßig so konfiguriert sein, dass beim Herstellen einer Verbindung mit MySQL SSL-Verbindungen erforderlich sind.  Es wird empfohlen, die SSL-Option wenn möglich nicht zu deaktivieren.

Bei der Bereitstellung eines neuen Azure-Datenbank für MySQL-Servers über das Azure-Portal und die CLI wird die Erzwingung von SSL-Verbindungen standardmäßig aktiviert. 

Verbindungszeichenfolgen für verschiedene Programmiersprachen werden im Azure-Portal angezeigt. Diese Verbindungszeichenfolgen schließen die erforderlichen SSL-Parameter für die Verbindung mit Ihrer Datenbank ein. Wählen Sie im Azure-Portal Ihren Server aus. Wählen Sie unter der Überschrift **Einstellungen** die Option **Verbindungszeichenfolgen** aus. Der SSL-Parameter variiert je nach Connector, z.B. „ssl=true“ oder „sslmode=require“ oder „sslmode=required“ und weitere Variationen.

Informationen zum Aktivieren oder Deaktivieren von SSL-Verbindungen bei der Anwendungsentwicklung finden Sie unter [Konfigurieren von SSL](howto-configure-ssl.md).

## <a name="tls-enforcement-in-azure-database-for-mysql"></a>TLS-Erzwingung in Azure Database for MySQL

Azure Database for MySQL unterstützt Verschlüsselung für Clients, die eine Verbindung mit dem Datenbankserver mithilfe von Transport Layer Security (TLS) herstellen. TLS ist ein Standardprotokoll der Branche, das sichere Netzwerkverbindungen zwischen dem Datenbankserver und Clientanwendungen gewährleistet, sodass Sie Konformitätsanforderungen einhalten können.

### <a name="tls-settings"></a>TLS-Einstellungen

Azure Database for MySQL bietet die Möglichkeit, die TLS-Version für die Clientverbindungen vorzuschreiben. Um die TLS-Version zu erzwingen, verwenden Sie die Optionseinstellung **TLS-Mindestversion**. Für diese Optionseinstellung sind die folgenden Werte zulässig:

|  TLS-Mindesteinstellung             | Unterstützte Client-TLS-Version                |
|:---------------------------------|-------------------------------------:|
| TLSEnforcementDisabled (Standardwert) | Kein TLS erforderlich                      |
| TLS1_0                           | TLS 1.0, TLS 1.1, TLS 1.2 und höher           |
| TLS1_1                           | TLS 1.1, TLS 1.2 und höher                   |
| TLS1_2                           | TLS-Version 1.2 und höher                     |


Wenn Sie diese Mindesteinstellung für die TLS-Version beispielsweise auf TLS 1.0 festlegen, bedeutet das, dass Ihr Server Verbindungen von Clients mithilfe von TLS 1.0, 1.1 und 1.2+ zulässt. Alternativ bedeutet die Festlegung auf 1.2, dass Sie nur Verbindungen von Clients mithilfe von TLS 1.2+ zulassen und alle Verbindungen mit TLS 1.0 und TLS 1.1 abgewiesen werden.

> [!Note] 
> Von Azure Database for MySQL wird standardmäßig keine TLS-Mindestversion erzwungen (Einstellung `TLSEnforcementDisabled`).
>
> Wenn Sie eine TLS-Mindestversion erzwingen, kann diese Erzwingung später nicht mehr deaktiviert werden.

Informationen zum Festlegen der TLS-Einstellung für Ihren Azure Database for MySQL finden Sie unter [Konfigurieren der TLS-Einstellung](howto-tls-configurations.md).

## <a name="next-steps"></a>Nächste Schritte

- [Datenverbindungsbibliotheken für Azure-Datenbank für MySQL](concepts-connection-libraries.md)
- Erfahren Sie mehr über das [Konfigurieren von SSL](howto-configure-ssl.md).
- Erfahren Sie mehr über das [Konfigurieren von TLS](howto-tls-configurations.md).
