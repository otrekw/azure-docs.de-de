---
title: Exportieren der Azure Cosmos DB-Emulatorzertifikate
description: Wenn Sie in Sprachen und Laufzeiten entwickeln, die nicht den Windows-Zertifikatspeicher verwenden, müssen Sie die TLS/SSL-Zertifikate exportieren und verwalten. Dieser Artikel bietet Schrittanleitungen dafür.
ms.service: cosmos-db
ms.topic: how-to
ms.date: 05/23/2019
author: deborahc
ms.author: dech
ms.custom: devx-track-python, devx-track-java
ms.openlocfilehash: e1321c0d5b1f83ffcfd3f46384dfb3af792c9b8b
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87373095"
---
# <a name="export-the-azure-cosmos-db-emulator-certificates-for-use-with-java-python-and-nodejs"></a>Exportieren der Azure Cosmos DB-Emulatorzertifikate für die Verwendung mit Java, Python und Node.js

[**Emulator herunterladen**](https://aka.ms/cosmosdb-emulator)

Der Azure Cosmos DB-Emulator bietet eine lokale Umgebung, die zu Entwicklungszwecken den Azure Cosmos DB-Dienst einschließlich der Verwendung von TLS-Verbindungen emuliert. Dieser Artikel zeigt, wie Sie die TLS/SSL-Zertifikate exportieren, um sie in Sprachen und Laufzeiten zu verwenden, die nicht im Windows-Zertifikatspeicher integriert sind. Beispiele sind die Sprachen Java, die einen eigenen [Zertifikatspeicher](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) verwendet, Python, die [Wrapper für Socketobjekte](https://docs.python.org/2/library/ssl.html) verwendet, und Node.js, die [tlsSocket](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback) verwendet. Weitere Informationen zum Emulator finden Sie unter [Verwenden des Azure Cosmos DB-Emulators für Entwicklungs- und Testzwecke](./local-emulator.md).

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Rotieren von Zertifikaten
> * Exportieren von TLS/SSL-Zertifikaten
> * Erlernen der Verwendung des Zertifikats in Java, Python und Node.js

## <a name="certification-rotation"></a>Rotierende Zertifizierung

Die Zertifikate im lokalen Azure Cosmos DB-Emulator werden bei der ersten Ausführung des Emulators generiert. Es gibt zwei Zertifikate. Das eine Zertifikat dient zum Herstellen der Verbindung mit dem lokalen Emulator, das andere zum Verwalten der Geheimnisse innerhalb des Emulators. Das Zertifikat, das Sie exportieren, ist das Verbindungszertifikat mit dem Anzeigenamen „DocumentDBEmulatorCertificate“.

Sie können beide Zertifikate erneut generieren, indem Sie im Azure Cosmos DB-Emulator, der in der Windows-Taskleiste ausgeführt wird, auf **Daten zurücksetzen** klicken (siehe Abbildung unten). Wenn Sie die Zertifikate, die Sie erneut generieren, im Java-Zertifikatspeicher installiert oder an anderer Stelle verwendet haben, müssen Sie diese aktualisieren. Andernfalls kann Ihre Anwendung keine Verbindung mehr mit dem lokalen Emulator herstellen.

:::image type="content" source="./media/local-emulator-export-ssl-certificates/database-local-emulator-reset-data.png" alt-text="Zurücksetzen von Daten im lokalen Azure Cosmos DB-Emulator":::

## <a name="how-to-export-the-azure-cosmos-db-tlsssl-certificate"></a>Exportieren des Azure Cosmos DB-TLS/SSL-Zertifikats

1. Starten Sie den Windows-Zertifikat-Manager, indem Sie „certlm.msc“ ausführen. Navigieren Sie zum Ordner „Persönlich > Zertifikate“, und öffnen Sie das Zertifikat mit dem Anzeigenamen **DocumentDbEmulatorCertificate**.

    :::image type="content" source="./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-1.png" alt-text="Exportieren im lokalen Azure Cosmos DB-Emulator, Schritt 1":::

2. Klicken Sie nacheinander auf **Details** und **OK**.

    :::image type="content" source="./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-2.png" alt-text="Exportieren im lokalen Azure Cosmos DB-Emulator, Schritt 2":::

3. Klicken Sie auf **In Datei kopieren...** .

    :::image type="content" source="./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-3.png" alt-text="Exportieren im lokalen Azure Cosmos DB-Emulator, Schritt 3":::

4. Klicken Sie auf **Weiter**.

    :::image type="content" source="./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-4.png" alt-text="Exportieren im lokalen Azure Cosmos DB-Emulator, Schritt 4":::

5. Klicken Sie auf **Nein, privaten Schlüssel nicht exportieren** und dann auf **Weiter**.

    :::image type="content" source="./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-5.png" alt-text="Exportieren im lokalen Azure Cosmos DB-Emulator, Schritt 5":::

6. Klicken Sie auf **Base-64-codiert X.509 (.CER)** und dann auf **Weiter**.

    :::image type="content" source="./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-6.png" alt-text="Exportieren im lokalen Azure Cosmos DB-Emulator, Schritt 6":::

7. Benennen Sie das Zertifikat. Wählen Sie in diesem Fall den Namen **documentdbemulatorcert**, und klicken Sie auf **Weiter**.

    :::image type="content" source="./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-7.png" alt-text="Exportieren im lokalen Azure Cosmos DB-Emulator, Schritt 7":::

8. Klicken Sie auf **Fertig stellen**.

    :::image type="content" source="./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-8.png" alt-text="Exportieren im lokalen Azure Cosmos DB-Emulator, Schritt 8":::

## <a name="how-to-use-the-certificate-in-java"></a>Verwenden des Zertifikats in Java

Wenn Sie Java-Anwendungen oder MongoDB-Anwendungen ausführen, die den Java-Client verwenden, ist es einfacher, das Zertifikat im standardmäßigen Java-Zertifikatspeicher zu installieren, als die Flags `-Djavax.net.ssl.trustStore=<keystore> -Djavax.net.ssl.trustStorePassword="<password>"` zu übergeben. Die enthaltene Java-Demoanwendung (`https://localhost:8081/_explorer/index.html`) beispielsweise ist vom Standardzertifikatspeicher abhängig.

Befolgen Sie die Anweisungen unter [Hinzufügen eines Zertifikats zum Java CA-Zertifikatspeicher](https://docs.microsoft.com/azure/java-add-certificate-ca-store), um das X.509-Zertifikat in den standardmäßigen Java-Zertifikatspeicher zu importieren. Denken Sie daran, dass Sie im Verzeichnis „%JAVA_HOME%“ arbeiten, wenn Sie „keytool“ ausführen.

Alternativ können Sie ein Bash-Skript erstellen und ausführen, um diesen Vorgang zu automatisieren:
```bash
#!/bin/bash

# If emulator was started with /AllowNetworkAccess, replace the below with the actual IP address of it:
EMULATOR_HOST=localhost
EMULATOR_PORT=8081
EMULATOR_CERT_PATH=/tmp/cosmos_emulator.cert
openssl s_client -connect ${EMULATOR_HOST}:${EMULATOR_PORT} </dev/null | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > $EMULATOR_CERT_PATH
# delete the cert if already exists
sudo $JAVA_HOME/bin/keytool -cacerts -delete -alias cosmos_emulator
# import the cert
sudo $JAVA_HOME/bin/keytool -cacerts -importcert -alias cosmos_emulator -file $EMULATOR_CERT_PATH
```

Sobald das TLS/SSL-Zertifikat „CosmosDBEmulatorCertificate“ installiert ist, sollte Ihre Anwendung eine Verbindung mit dem lokalen Azure Cosmos DB-Emulator herstellen und diesen verwenden können. Wenn Probleme auftreten, finden Sie möglicherweise eine Lösung im Dokument [Debugging SSL/TLS Connections](https://docs.oracle.com/javase/7/docs/technotes/guides/security/jsse/ReadDebug.html) (Debuggen von SSL-/TLS-Verbindungen). Es ist sehr wahrscheinlich, dass das Zertifikat nicht im Speicher „%JAVA_HOME%/jre/lib/security/cacerts“ installiert ist. Wenn Sie beispielsweise mehrere Java-Versionen installiert haben, verwendet Ihre Anwendung möglicherweise einen anderen cacerts-Speicher als den, den Sie aktualisiert haben.

## <a name="how-to-use-the-certificate-in-python"></a>Verwenden des Zertifikats in Python

Das [Python SDK (ab Version 2.0.0)](sql-api-sdk-python.md) für die SQL-API verwendet beim Herstellen einer Verbindung mit dem lokalen Emulator standardmäßig nicht das TLS/SSL-Zertifikat. Wenn Sie die TLS-Validierung verwenden möchten, befolgen Sie die Beispiele in der Dokumentation zu [Python-Wrappern für Socketobjekte](https://docs.python.org/2/library/ssl.html).

## <a name="how-to-use-the-certificate-in-nodejs"></a>Verwenden des Zertifikats in Node.js

Das [Node.js SDK (ab Version 1.10.1)](sql-api-sdk-node.md) für die SQL-API verwendet beim Herstellen einer Verbindung mit dem lokalen Emulator standardmäßig nicht das TLS/SSL-Zertifikat. Wenn Sie die TLS-Validierung verwenden möchten, befolgen Sie die Beispiele in der [Node.js](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback)-Dokumentation.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die folgenden Aufgaben ausgeführt:

> [!div class="checklist"]
> * Rotieren von Zertifikaten
> * Exportieren des TLS/SSL-Zertifikats
> * Erlernen der Verwendung des Zertifikats in Java, Python und Node.js

Sie können jetzt mit dem Abschnitt „Konzepte“ fortfahren, um weitere Informationen zu Azure Cosmos DB zu erhalten. 

> [!div class="nextstepaction"]
>[Einstellbare Datenkonsistenzebenen in Azure Cosmos DB](../cosmos-db/consistency-levels.md)
