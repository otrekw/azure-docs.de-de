---
title: 'Verschlüsselte Konnektivität mit TLS/SSL in Azure Database for PostgreSQL: Flexible Server'
description: 'Hier finden Sie Anweisungen und Informationen zum Herstellen einer Verbindung unter Verwendung von TLS/SSL in Azure Database for PostgreSQL: Flexible Server.'
author: ambhatna
ms.author: ambhatna
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/22/2020
ms.openlocfilehash: 0ba04ff14e62cd411515a765dc95ef7a3e72b709
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90931220"
---
# <a name="encrypted-connectivity-using-transport-layer-security-in-azure-database-for-postgresql---flexible-server"></a>Verschlüsselte Konnektivität mit Transport Layer Security in Azure Database for PostgreSQL: Flexible Server

> [!IMPORTANT]
> Azure Database for PostgreSQL: Flexible Server befindet sich in der Vorschau.

Azure Database for PostgreSQL: Flexible Server unterstützt das Herstellen einer Verbindung zwischen Ihren Clientanwendungen und dem PostgreSQL-Dienst über TLS (Transport Layer Security), ehemals als SSL (Secure Sockets Layer) bezeichnet. TLS ist ein Standardprotokoll der Branche, das verschlüsselte Netzwerkverbindungen zwischen dem Datenbankserver und Clientanwendungen gewährleistet, sodass Sie Konformitätsanforderungen einhalten können.

Azure Database for PostgreSQL: Flexible Server unterstützt verschlüsselte Verbindungen mit Transport Layer Security (TLS 1.2+), und alle eingehenden Verbindungen mit TLS 1.0 und TLS 1.1 werden verweigert. Für alle Flexible Server-Instanzen ist die Durchsetzung von TLS-Verbindungen aktiviert, und Sie können TLS/SSL für die Verbindung zur Flexible Server-Instanz nicht deaktivieren.

## <a name="applications-that-require-certificate-verification-for-tlsssl-connectivity"></a>Anwendungen, die eine Zertifikatüberprüfung für TLS/SSL-Verbindungen erfordern
In einigen Fällen erfordern Anwendungen eine lokale Zertifikatdatei, die auf der Grundlage der Zertifikatdatei einer vertrauenswürdigen Zertifizierungsstelle (Certificate Authority, CA) generiert wurde, um eine sichere Verbindung herzustellen. Azure Database for PostgreSQL: Flexible Server verwendet *DigiCert Global Root CA*. Laden Sie dieses für die Kommunikation über SSL erforderliche Zertifikat von [DigiCert Global Root CA](https://dl.cacerts.digicert.com/DigiCertGlobalRootCA.crt.pem) herunter, und speichern Sie die Zertifikatsdatei an Ihrem bevorzugten Speicherort. In diesem Tutorial wird beispielsweise `c:\ssl` verwendet.


### <a name="connect-using-psql"></a>Herstellen einer Verbindung mit psql
Wenn Sie Ihre Flexible Server-Instanz mit *Privater Zugriff (VNET-Integration)* erstellt haben, müssen Sie eine Verbindung mit Ihrem Server über eine Ressource innerhalb desselben virtuellen Netzwerks herstellen. Sie können einen virtuellen Computer erstellen und zum virtuellen Netzwerk hinzufügen, das mit Ihrer Flexible Server-Instanz erstellt wurde.

Wenn Sie Ihre Flexible Server-Instanz mit *Öffentlicher Zugriff (zulässige IP-Adressen)* erstellt haben, können Sie die lokale IP-Adresse der Liste der Firewallregeln auf Ihrem Server hinzufügen.

Das folgende Beispiel zeigt, wie Sie einen Server mithilfe der psql-Befehlszeilenschnittstelle verbinden können. Verwenden Sie die Einstellung `sslmode=verify-full` für die Verbindungszeichenfolge, um die Überprüfung des TLS-/SSL-Zertifikats zu erzwingen. Übergeben Sie den Pfad der lokalen Zertifikatdatei an den Parameter `sslrootcert`.

```bash
 psql "sslmode=verify-full sslrootcert=c:\ssl\DigiCertGlobalRootCA.crt.pem host=mydemoserver.postgres.database.azure.com dbname=postgres user=myadmin"
```
> [!Note]
> Vergewissern Sie sich, dass der Wert, den Sie an `sslrootcert` übergeben haben, mit dem Dateipfad für das von Ihnen gespeicherte Zertifikat übereinstimmt.

## <a name="ensure-your-application-or-framework-supports-tls-connections"></a>Sicherstellen, dass die Anwendung oder das Framework TLS-Verbindungen unterstützt

Einige Anwendungsframeworks, die PostgreSQL für ihre Datenbankdienste verwenden, aktivieren TLS nicht standardmäßig während der Installation. Ihr PostgreSQL-Server erzwingt TLS-Verbindungen. Wenn die Anwendung aber nicht für TLS konfiguriert wurde, kann die Anwendung unter Umständen keine Verbindung mit Ihrem Datenbankserver herstellen. Lesen Sie in der Dokumentation Ihrer Anwendung nach, wie TLS-Verbindungen aktiviert werden.

## <a name="next-steps"></a>Nächste Schritte
- [Erstellen und Verwalten von virtuellen Netzwerken für Azure Database for PostgreSQL Flexible Server mithilfe der Azure CLI](./how-to-manage-virtual-network-cli.md)
- Erfahren Sie mehr über [Netzwerke in Azure Database for PostgreSQL: Flexible Server](./concepts-networking.md).
- Erfahren Sie mehr über [Firewallregeln für Azure Database for PostgreSQL: Flexible Server](./concepts-networking.md#public-access-allowed-ip-addresses).
