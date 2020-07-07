---
title: 'TLS: Hyperscale (Citus) – Azure Database for PostgreSQL'
description: Hier finden Sie Anweisungen und Informationen zum Konfigurieren von Azure Database for PostgreSQL – Hyperscale (Citus) und zugehörigen Anwendungen für die ordnungsgemäße Verwendung von TLS-Verbindungen.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 03/30/2020
ms.openlocfilehash: 791eed9419375c7245488b8ec61a1c5481be382e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "82580566"
---
# <a name="configure-tls-in-azure-database-for-postgresql---hyperscale-citus"></a>Konfigurieren von TLS in Azure Database for PostgreSQL – Hyperscale (Citus)
Verbindungen von Clientanwendungen mit dem Koordinatorknoten von Hyperscale (Citus) erfordern TLS (Transport Layer Security), früher als SSL (Secure Sockets Layer) bezeichnet. Durch das Erzwingen von TLS-Verbindungen zwischen Ihrem Datenbankserver und Ihren Clientanwendungen können Sie sich vor Man-in-the-Middle-Angriffen schützen, indem Sie den Datenstrom zwischen dem Server und Ihrer Anwendung verschlüsseln.

## <a name="enforcing-tls-connections"></a>Erzwingen von TLS-Verbindungen
Bei allen Azure Database for PostgreSQL-Servern, die über das Azure-Portal bereitgestellt werden, ist die Erzwingung von TLS-Verbindungen standardmäßig aktiviert. 

Entsprechend enthalten Verbindungszeichenfolgen, die im Azure-Portal unter dem Server in den Verbindungszeichenfolgen-Einstellungen vorab definiert sind, die erforderlichen Parameter für gängige Sprachen für die Verbindung mit dem Datenbankserver mithilfe von TLS. Der TLS-Parameter variiert je nach Connector, z. B. „ssl=true“ oder „sslmode=require“ oder „sslmode=required“ und weitere Variationen.

## <a name="ensure-your-application-or-framework-supports-tls-connections"></a>Sicherstellen, dass die Anwendung oder das Framework TLS-Verbindungen unterstützt
Einige Anwendungsframeworks, die PostgreSQL für ihre Datenbankdienste verwenden, aktivieren TLS nicht standardmäßig während der Installation. Wenn Ihr PostgreSQL-Server TLS-Verbindungen erzwingt, die Anwendung aber nicht für TLS konfiguriert wurde, kann die Anwendung unter Umständen keine Verbindung mit dem Datenbankserver herstellen. Lesen Sie in der Dokumentation Ihrer Anwendung nach, wie TLS-Verbindungen aktiviert werden.

## <a name="applications-that-require-certificate-verification-for-tls-connectivity"></a>Anwendungen, die eine Zertifikatüberprüfung für TLS-Verbindungen erfordern
In einigen Fällen erfordern Anwendungen eine lokale Zertifikatdatei, die aus der Zertifikatdatei (CER) einer vertrauenswürdigen Zertifizierungsstelle (Certificate Authority, CA) generiert wurde, um eine sichere Verbindung herzustellen. Das Zertifikat für Verbindungen mit Azure Database for PostgreSQL – Hyperscale (Citus) befindet sich unter https://dl.cacerts.digicert.com/DigiCertGlobalRootCA.crt.pem. Laden Sie die Zertifikatdatei herunter, und speichern Sie sie am gewünschten Ort.

### <a name="connect-using-psql"></a>Herstellen einer Verbindung mit psql
Das folgende Beispiel zeigt, wie Sie mit dem Befehlszeilenprogramm „psql“ eine Verbindung mit dem Hyperscale (Citus)-Koordinatorknoten herstellen. Verwenden Sie die Einstellung `sslmode=verify-full` für die Verbindungszeichenfolge, um die Überprüfung des TLS-Zertifikats zu erzwingen. Übergeben Sie den Pfad der lokalen Zertifikatdatei an den Parameter `sslrootcert`.

Unten ist ein Beispiel für die psql-Verbindungszeichenfolge angegeben:
```
psql "sslmode=verify-full sslrootcert=DigiCertGlobalRootCA.crt.pem host=mydemoserver.postgres.database.azure.com dbname=citus user=citus password=your_pass"
```
> [!TIP]
> Vergewissern Sie sich, dass der Wert, den Sie an `sslrootcert` übergeben haben, mit dem Dateipfad für das von Ihnen gespeicherte Zertifikat übereinstimmt.

## <a name="next-steps"></a>Nächste Schritte
Optimieren Sie die Sicherheit mit [Firewallregeln in Azure Database for PostgreSQL – Hyperscale (Citus)](concepts-hyperscale-firewall-rules.md).
