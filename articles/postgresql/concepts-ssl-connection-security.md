---
title: SSL (Azure Database for PostgreSQL, Einzelserver)
description: Hier finden Sie Anweisungen und Informationen zum Konfigurieren der SSL-Konnektivität für Azure Database for PostgreSQL (Einzelserver).
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 303da4dcb68a79e69254f6610afc0003bf0aa22c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79476999"
---
# <a name="configure-ssl-connectivity-in-azure-database-for-postgresql---single-server"></a>Konfigurieren von SSL-Verbindungen in Azure Database for PostgreSQL – Einzelserver

Azure-Datenbank für PostgreSQL stellt Verbindungen zwischen Clientanwendungen und dem PostgreSQL-Dienst hauptsächlich mittels Secure Sockets Layer (SSL) her. Das Erzwingen von SSL-Verbindungen zwischen dem Datenbankserver und Clientanwendungen trägt zum Schutz vor Man-in-the-Middle-Angriffen bei, indem der Datenstrom zwischen dem Server und der Anwendung verschlüsselt wird.

Standardmäßig ist der PostgreSQL-Datenbankdienst so konfiguriert, dass SSL-Verbindungen erforderlich sind. Sie können angeben, dass die SSL-Erzwingung deaktiviert werden soll, wenn Ihre Clientanwendung die SSL-Konnektivität nicht unterstützt.

## <a name="enforcing-ssl-connections"></a>Erzwingen von SSL-Verbindungen

Bei allen Azure-Datenbank für PostgreSQL-Servern, die über das Azure-Portal und die CLI bereitgestellt werden, ist die Erzwingung von SSL-Verbindungen standardmäßig aktiviert. 

Entsprechend enthalten Verbindungszeichenfolgen, die im Azure-Portal unter dem Server in den Verbindungszeichenfolgen-Einstellungen vorab definiert sind, die erforderlichen Parameter für gängige Sprachen für die Verbindung mit dem Datenbankserver mithilfe von SSL. Der SSL-Parameter variiert je nach Connector, z.B. „ssl=true“ oder „sslmode=require“ oder „sslmode=required“ und weitere Variationen.

## <a name="configure-enforcement-of-ssl"></a>Konfigurieren der Erzwingung von SSL

Sie können die Erzwingung von SSL-Verbindungen optional deaktivieren. Microsoft Azure empfiehlt, die Einstellung **SSL-Verbindung erzwingen** immer zu aktivieren, um die Sicherheit zu erhöhen.

### <a name="using-the-azure-portal"></a>Verwenden des Azure-Portals

Rufen Sie Ihren Azure-Datenbank für PostgreSQL-Server auf, und klicken Sie auf **Verbindungssicherheit**. Verwenden Sie die Umschaltfläche, um die Einstellung **SSL-Verbindung erzwingen** zu aktivieren/deaktivieren. Klicken Sie dann auf **Speichern**.

![Verbindungssicherheit – Erzwingung von SSL deaktivieren](./media/concepts-ssl-connection-security/1-disable-ssl.png)

Bestätigen Sie die Einstellung auf der Seite **Übersicht**, indem Sie den Indikator für den **SSL-Erzwingungsstatus** anzeigen.

### <a name="using-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle

Sie können den Parameter **ssl-enforcement** in der Azure CLI mit den Werten `Enabled` bzw. `Disabled` aktivieren oder deaktivieren.

```azurecli
az postgres server update --resource-group myresourcegroup --name mydemoserver --ssl-enforcement Enabled
```

## <a name="ensure-your-application-or-framework-supports-ssl-connections"></a>Sicherstellen, dass die Anwendung oder das Framework SSL-Verbindungen unterstützt

Einige Anwendungsframeworks, die PostgreSQL für ihre Datenbankdienste verwenden, aktivieren SSL nicht standardmäßig während der Installation. Wenn Ihr PostgreSQL-Server SSL-Verbindungen erzwingt, die Anwendung aber nicht für SSL konfiguriert wurde, kann die Anwendung unter Umständen keine Verbindung mit dem Datenbankserver herstellen. Lesen Sie in der Dokumentation Ihrer Anwendung nach, wie SSL-Verbindungen aktiviert werden.

## <a name="applications-that-require-certificate-verification-for-ssl-connectivity"></a>Anwendungen, die eine Zertifikatüberprüfung für SSL-Verbindungen erfordern

In einigen Fällen erfordern Anwendungen eine lokale Zertifikatdatei, die aus der Zertifikatdatei (CER) einer vertrauenswürdigen Zertifizierungsstelle (Certificate Authority, CA) generiert wurde, um eine sichere Verbindung herzustellen. Das Zertifikat für die Verbindungsherstellung mit einem Azure Database for PostgreSQL-Server befindet sich unter https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem. Laden Sie die Zertifikatdatei herunter, und speichern Sie sie am gewünschten Ort.

### <a name="connect-using-psql"></a>Herstellen einer Verbindung mit psql

Das folgende Beispiel zeigt, wie Sie einen PostgreSQL-Server mithilfe des psql-Befehlszeilen-Hilfsprogramms verbinden können. Verwenden Sie die Einstellung `sslmode=verify-full` für die Verbindungszeichenfolge, um die Überprüfung des SSL-Zertifikats zu erzwingen. Übergeben Sie den Pfad der lokalen Zertifikatdatei an den Parameter `sslrootcert`.

Der folgende Befehl ist ein Beispiel für die psql-Verbindungszeichenfolge:

```shell
psql "sslmode=verify-full sslrootcert=BaltimoreCyberTrustRoot.crt host=mydemoserver.postgres.database.azure.com dbname=postgres user=myusern@mydemoserver"
```

> [!TIP]
> Vergewissern Sie sich, dass der Wert, den Sie an `sslrootcert` übergeben haben, mit dem Dateipfad für das von Ihnen gespeicherte Zertifikat übereinstimmt.

## <a name="next-steps"></a>Nächste Schritte

Überprüfen verschiedener Anwendungskonnektivitätsoptionen gemäß [Datenverbindungsbibliotheken für Azure Database for PostgreSQL](concepts-connection-libraries.md).
