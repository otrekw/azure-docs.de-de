---
title: TLS – Azure Database for PostgreSQL (Einzelserver)
description: Hier finden Sie Anweisungen und Informationen zum Konfigurieren der TLS-Konnektivität für Azure Database for PostgreSQL (Einzelserver).
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: d0482e5205b97b5c57c41e0ba98fb9ca819e5d5f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82141742"
---
# <a name="configure-tls-connectivity-in-azure-database-for-postgresql---single-server"></a>Konfigurieren der TLS-Konnektivität in Azure Database for PostgreSQL (Einzelserver)

Azure Database for PostgreSQL bevorzugt das Herstellen einer Verbindung zwischen Ihren Clientanwendungen und dem PostgreSQL-Dienst über TLS (Transport Layer Security), ehemals als SSL (Secure Sockets Layer) bezeichnet. Durch das Erzwingen von TLS-Verbindungen zwischen Ihrem Datenbankserver und Ihren Clientanwendungen können Sie sich vor Man-in-the-Middle-Angriffen schützen, indem Sie den Datenstrom zwischen dem Server und Ihrer Anwendung verschlüsseln.

Standardmäßig ist der PostgreSQL-Datenbankdienst so konfiguriert, dass TLS-Verbindungen erforderlich sind. Sie können das Erfordern von TLS deaktivieren, wenn Ihre Clientanwendung keine TLS-Verbindungen unterstützt.

## <a name="enforcing-tls-connections"></a>Erzwingen von TLS-Verbindungen

Bei allen Azure Database for PostgreSQL-Servern, die über das Azure-Portal und die CLI bereitgestellt werden, ist die Erzwingung von TLS-Verbindungen standardmäßig aktiviert. 

Entsprechend enthalten Verbindungszeichenfolgen, die im Azure-Portal unter dem Server in den Verbindungszeichenfolgen-Einstellungen vorab definiert sind, die erforderlichen Parameter für gängige Sprachen für die Verbindung mit dem Datenbankserver mithilfe von TLS. Der TLS-Parameter variiert je nach Connector, z. B. „ssl=true“ oder „sslmode=require“ oder „sslmode=required“ und weitere Variationen.

## <a name="configure-enforcement-of-tls"></a>Konfigurieren der Erzwingung von TLS

Sie können die Erzwingung von TLS-Verbindungen optional deaktivieren. Microsoft Azure empfiehlt, die Einstellung **SSL-Verbindung erzwingen** immer zu aktivieren, um die Sicherheit zu erhöhen.

### <a name="using-the-azure-portal"></a>Verwenden des Azure-Portals

Rufen Sie Ihren Azure-Datenbank für PostgreSQL-Server auf, und klicken Sie auf **Verbindungssicherheit**. Verwenden Sie die Umschaltfläche, um die Einstellung **SSL-Verbindung erzwingen** zu aktivieren/deaktivieren. Klicken Sie dann auf **Speichern**.

![Verbindungssicherheit: Erzwingung von TLS/SSL deaktivieren](./media/concepts-ssl-connection-security/1-disable-ssl.png)

Bestätigen Sie die Einstellung auf der Seite **Übersicht**, indem Sie den Indikator für den **SSL-Erzwingungsstatus** anzeigen.

### <a name="using-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle

Sie können den Parameter **ssl-enforcement** in der Azure CLI mit den Werten `Enabled` bzw. `Disabled` aktivieren oder deaktivieren.

```azurecli
az postgres server update --resource-group myresourcegroup --name mydemoserver --ssl-enforcement Enabled
```

## <a name="ensure-your-application-or-framework-supports-tls-connections"></a>Sicherstellen, dass die Anwendung oder das Framework TLS-Verbindungen unterstützt

Einige Anwendungsframeworks, die PostgreSQL für ihre Datenbankdienste verwenden, aktivieren TLS nicht standardmäßig während der Installation. Wenn Ihr PostgreSQL-Server TLS-Verbindungen erzwingt, die Anwendung aber nicht für TLS konfiguriert wurde, kann die Anwendung unter Umständen keine Verbindung mit dem Datenbankserver herstellen. Lesen Sie in der Dokumentation Ihrer Anwendung nach, wie TLS-Verbindungen aktiviert werden.

## <a name="applications-that-require-certificate-verification-for-tls-connectivity"></a>Anwendungen, die eine Zertifikatüberprüfung für TLS-Verbindungen erfordern

In einigen Fällen erfordern Anwendungen eine lokale Zertifikatdatei, die aus der Zertifikatdatei (CER) einer vertrauenswürdigen Zertifizierungsstelle (Certificate Authority, CA) generiert wurde, um eine sichere Verbindung herzustellen. Das Zertifikat für die Verbindungsherstellung mit einem Azure Database for PostgreSQL-Server befindet sich unter https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem. Laden Sie die Zertifikatdatei herunter, und speichern Sie sie am gewünschten Ort.

### <a name="connect-using-psql"></a>Herstellen einer Verbindung mit psql

Das folgende Beispiel zeigt, wie Sie einen PostgreSQL-Server mithilfe des psql-Befehlszeilen-Hilfsprogramms verbinden können. Verwenden Sie die Einstellung `sslmode=verify-full` für die Verbindungszeichenfolge, um die Überprüfung des TLS-/SSL-Zertifikats zu erzwingen. Übergeben Sie den Pfad der lokalen Zertifikatdatei an den Parameter `sslrootcert`.

Der folgende Befehl ist ein Beispiel für die psql-Verbindungszeichenfolge:

```shell
psql "sslmode=verify-full sslrootcert=BaltimoreCyberTrustRoot.crt host=mydemoserver.postgres.database.azure.com dbname=postgres user=myusern@mydemoserver"
```

> [!TIP]
> Vergewissern Sie sich, dass der Wert, den Sie an `sslrootcert` übergeben haben, mit dem Dateipfad für das von Ihnen gespeicherte Zertifikat übereinstimmt.

## <a name="next-steps"></a>Nächste Schritte

Überprüfen verschiedener Anwendungskonnektivitätsoptionen gemäß [Datenverbindungsbibliotheken für Azure Database for PostgreSQL](concepts-connection-libraries.md).
