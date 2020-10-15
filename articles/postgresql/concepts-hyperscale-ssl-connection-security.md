---
title: 'Transport Layer Security (TLS): Hyperscale (Citus) – Azure Database for PostgreSQL'
description: Hier finden Sie Anweisungen und Informationen zum Konfigurieren von Azure Database for PostgreSQL – Hyperscale (Citus) und zugehörigen Anwendungen für die ordnungsgemäße Verwendung von TLS-Verbindungen.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 07/16/2020
ms.openlocfilehash: 659f86a107e4b08db4ec5195635ea32d2260d677
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87071458"
---
# <a name="configure-tls-in-azure-database-for-postgresql---hyperscale-citus"></a>Konfigurieren von TLS in Azure Database for PostgreSQL – Hyperscale (Citus)
Der Koordinatorknoten für Hyperscale (Citus) erfordert, dass Clientanwendungen eine Verbindung mit Transport Layer Security (TLS) herstellen. Durch Erzwingen von TLS zwischen Datenbankserver und Clientanwendungen bleiben vertrauliche Daten auch während der Übertragung sicher. Die weiter unten beschriebenen zusätzlichen Überprüfungseinstellungen schützen auch vor „Man-in-the-Middle“-Angriffen.

## <a name="enforcing-tls-connections"></a>Erzwingen von TLS-Verbindungen
Anwendungen verwenden eine so genannte Verbindungszeichenfolge, um die Zieldatenbank zu identifizieren und die Einstellungen für eine Verbindung zu ermitteln. Verschiedene Clients erfordern verschiedene Einstellungen. Eine Liste der Verbindungszeichenfolgen, die von gängigen Clients verwendet werden, finden Sie im Abschnitt **Verbindungszeichenfolgen** für Ihre Servergruppe im Azure-Portal.

Die TLS-Parameter `ssl` und `sslmode` variieren je nach Funktionen des Connectors, z. B. `ssl=true` oder `sslmode=require` oder `sslmode=required`.

## <a name="ensure-your-application-or-framework-supports-tls-connections"></a>Sicherstellen, dass die Anwendung oder das Framework TLS-Verbindungen unterstützt
Einige Anwendungsframeworks aktivieren TLS nicht standardmäßig für PostgreSQL-Verbindungen. Ohne sichere Verbindung kann eine Anwendung jedoch keine Verbindung mit einem Koordinatorknoten für Hyperscale (Citus) herstellen. Lesen Sie in der Dokumentation Ihrer Anwendung nach, wie TLS-Verbindungen aktiviert werden.

## <a name="applications-that-require-certificate-verification-for-tls-connectivity"></a>Anwendungen, die eine Zertifikatüberprüfung für TLS-Verbindungen erfordern
In einigen Fällen erfordern Anwendungen eine lokale Zertifikatdatei, die aus der Zertifikatdatei (CER) einer vertrauenswürdigen Zertifizierungsstelle (Certificate Authority, CA) generiert wurde, um eine sichere Verbindung herzustellen. Das Zertifikat für Verbindungen mit Azure Database for PostgreSQL – Hyperscale (Citus) befindet sich unter https://cacerts.digicert.com/DigiCertGlobalRootCA.crt.pem. Laden Sie die Zertifikatdatei herunter, und speichern Sie sie am gewünschten Ort.

> [!NOTE]
>
> Um die Echtheit des Zertifikats zu überprüfen, können Sie mithilfe des OpenSSL-Befehlszeilentools den SHA-256-Fingerabdruck verifizieren:
>
> ```sh
> openssl x509 -in DigiCertGlobalRootCA.crt.pem -noout -sha256 -fingerprint
>
> # should output:
> # 43:48:A0:E9:44:4C:78:CB:26:5E:05:8D:5E:89:44:B4:D8:4F:96:62:BD:26:DB:25:7F:89:34:A4:43:C7:01:61
> ```

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
