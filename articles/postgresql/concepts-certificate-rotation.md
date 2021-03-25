---
title: Rotation für Zertifikate für Azure Database for PostgreSQL-Einzelserver
description: Hier erfahren Sie mehr über die bevorstehenden Änderungen an Stammzertifikaten im Zusammenhang mit Azure Database for PostgreSQL-Einzelserver.
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/02/2020
ms.openlocfilehash: 6dc687879eb646b4abd081b40bce292d20ff3186
ms.sourcegitcommit: dac05f662ac353c1c7c5294399fca2a99b4f89c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102123988"
---
# <a name="understanding-the-changes-in-the-root-ca-change-for-azure-database-for-postgresql-single-server"></a>Grundlegendes zu den Änderungen im Zusammenhang mit der Stammzertifizierungsstelle für Azure Database for PostgreSQL-Einzelserver

Die Änderung des Stammzertifikats am **15. Februar 2021 (15.02.2021)** wurde für Azure Database for PostgreSQL Single Server im Rahmen der standardmäßigen Wartung und bewährten Sicherheitsmethoden erfolgreich abgeschlossen. In diesem Artikel erhalten Sie ausführlichere Informationen zu den Änderungen, den betroffenen Ressourcen sowie den Schritten, die erforderlich sind, um sicherzustellen, dass die Konnektivität zwischen Ihrer Anwendung und dem Datenbankserver bestehen bleibt.

## <a name="why-root-certificate-update-is-required"></a>Warum muss das Stammzertifikat aktualisiert werden?

Azure Database for PostgreSQL-Kunden können nur [dieses vordefinierte Zertifikat](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) verwenden, um eine Verbindung mit ihrem PostgreSQL-Server herzustellen. Im [ZA-/Browserforum](https://cabforum.org/) wurden jedoch kürzlich Berichte veröffentlicht, in denen mehrere von Zertifizierungsstellenanbietern ausgestellte Zertifikate aufgeführt sind, die nicht konform sind.

Gemäß den Konformitätsanforderungen der Branche haben Zertifizierungsstellenanbieter damit begonnen, Zertifizierungsstellenzertifikate für nicht konforme Zertifizierungsstellen zu sperren. Daher müssen Server Zertifikate verwenden, die von konformen Zertifizierungsstellen ausgestellt und durch Zertifizierungsstellenzertifikate von diesen konformen Zertifizierungsstellen signiert wurden. Da von Azure Database for MySQL eines dieser nicht konformen Zertifikate verwendet wurde, musste das Zertifikat zur konformen Version rotiert werden, um die potenzielle Bedrohung für Ihre MySQL-Server zu minimieren.

Der Rollout des neuen Zertifikats hat am 15. Februar 2021 (15.02.2021) begonnen, und es ist ab diesem Datum gültig. 

## <a name="what-change-was-performed-on-february-15-2021-02152021"></a>Welche Änderung wurde am 15. Februar 2021 (15.02.2021) durchgeführt?

Am 15. Februar 2021 wurde das [BaltimoreCyberTrustRoot-Stammzertifikat](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) durch eine **konforme Version** desselben [BaltimoreCyberTrustRoot-Stammzertifikats](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) ersetzt, um sicherzustellen, dass Bestandskunden keine Änderungen vornehmen müssen und keine Auswirkungen auf ihre Verbindungen mit dem Server bestehen. Während dieser Änderung wurde das [BaltimoreCyberTrustRoot-Stammzertifikat](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) **nicht** durch [DigiCertGlobalRootG2](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem) ersetzt. Diese Änderung wird zurückgestellt, um den Kunden mehr Zeit für die Umstellung zu geben.

## <a name="do-i-need-to-make-any-changes-on-my-client-to-maintain-connectivity"></a>Muss ich Änderungen an meinem Client vornehmen, um die Konnektivität aufrechtzuerhalten?

Es ist keine Änderung auf Clientseite erforderlich. Wenn Sie die frühere Empfehlung unten befolgt haben, können Sie weiterhin eine Verbindung herstellen, solange das **BaltimoreCyberTrustRoot-Zertifikat nicht aus dem kombinierten Zertifizierungsstellenzertifikat entfernt wird**. **Es wird empfohlen, das BaltimoreCyberTrustRoot-Zertifikat bis auf Weiteres nicht aus dem kombinierten Zertifizierungsstellenzertifikat zu entfernen, um die Konnektivität aufrechtzuerhalten.**

### <a name="previous-recommendation"></a>Frühere Empfehlung

*   Laden Sie die Stammzertifizierungsstellen BaltimoreCyberTrustRoot und DigiCertGlobalRootG2 über die folgenden Links herunter:
    *   https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem
    *   https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem

*   Generieren Sie einen kombinierten Zertifizierungsstellen-Zertifikatspeicher, in dem sowohl **BaltimoreCyberTrustRoot** als auch **DigiCertGlobalRootG2** enthalten sind.
    *   Führen Sie für Java-Benutzer (PostgreSQL-JDBC) mit DefaultJavaSSLFactory Folgendes aus:

          ```console
          keytool -importcert -alias PostgreSQLServerCACert  -file D:\BaltimoreCyberTrustRoot.crt.pem  -keystore truststore -storepass password -noprompt
          ```

          ```console
          keytool -importcert -alias PostgreSQLServerCACert2  -file D:\DigiCertGlobalRootG2.crt.pem -keystore truststore -storepass password  -noprompt
          ```

          Ersetzen Sie dann die ursprüngliche Keystore-Datei durch die neu generierte Datei:
        *   System.setProperty("javax.net.ssl.trustStore","Pfad_zur_Truststore-Datei"); 
        *   System.setProperty("javax.net.ssl.trustStorePassword","Kennwort");
        
    *   Stellen Sie für .NET-Benutzer (Npgsql) unter Windows sicher, dass **Baltimore CyberTrust Root** und **DigiCert Global Root G2** als vertrauenswürdige Stammzertifizierungsstellen im Windows-Zertifikatspeicher vorhanden sind. Ist eines der Zertifikate nicht vorhanden, importieren Sie das fehlende Zertifikat.

        ![Azure Database for PostgreSQL-.NET-Zertifikat](media/overview/netconnecter-cert.png)

    *   Stellen Sie bei .NET-Benutzern (Npgsql) unter Linux, die SSL_CERT_DIR verwenden, sicher, dass **BaltimoreCyberTrustRoot** und **DigiCertGlobalRootG2** in dem durch SSL_CERT_DIR angegebenen Verzeichnis enthalten sind. Ist eines der Zertifikate nicht vorhanden, erstellen Sie die fehlende Zertifikatsdatei.

    *   Für andere PostgreSQL-Client Benutzer können Sie zwei ZS-Zertifikats Dateien wie in diesem Format weiter unten zusammenführen.

        </br>-----BEGIN CERTIFICATE-----
 </br>(Root CA1: BaltimoreCyberTrustRoot.crt.pem)
 </br>-----END CERTIFICATE-----
 </br>-----BEGIN CERTIFICATE-----
 </br>(Root CA2: DigiCertGlobalRootG2.crt.pem)
 </br>-----END CERTIFICATE-----

*   Ersetzen Sie die ursprüngliche PEM-Datei der Stammzertifizierungsstelle durch die kombinierte Datei der Stammzertifizierungsstelle, und starten Sie die Anwendung bzw. den Client neu.
*    Nachdem das neue Zertifikat auf der Serverseite bereitgestellt wurde, können Sie die PEM-Datei der Zertifizierungsstelle in Zukunft in „DigiCertGlobalRootG2.crt.pem“ ändern.

> [!NOTE]
> Löschen oder ändern Sie das **Baltimore-Zertifikat** erst, nachdem die Zertifikatänderung vorgenommen wurde. Sobald die Änderung abgeschlossen ist, senden wir Ihnen eine Mitteilung,und danach können Sie das Baltimore-Zertifikat auf sichere Weise löschen. 

## <a name="why-was-baltimorecybertrustroot-certificate-not-replaced-to-digicertglobalrootg2-during-this-change-on-february-15-2021"></a>Warum wurde das BaltimoreCyberTrustRoot-Zertifikat während dieser Änderung am 15. Februar 2021 nicht durch DigiCertGlobalRootG2 ersetzt?

Wir haben die Bereitschaft der Kunden für diese Änderung bewertet und erkannt, dass viele Kunden noch zusätzliche Vorlaufzeit zur Abwicklung dieser Änderung benötigen. Um den Kunden eine längere Vorlaufzeit zur Vorbereitung zu bieten, haben wir uns entschieden, die Zertifikatänderung in DigiCertGlobalRootG2 um mindestens ein Jahr zu verschieben, um Kunden und Endbenutzern genügend Vorlaufzeit zu geben. 

Unsere Empfehlungen für Benutzer lauten: Führen Sie die oben genannten Schritte zum Erstellen eines kombinierten Zertifikats und Herstellen einer Verbindung mit dem Server aus, aber entfernen Sie das BaltimoreCyberTrustRoot-Zertifikat erst dann, wenn wir eine entsprechende Mitteilung zum Entfernen senden. 

## <a name="what-if-we-removed-the-baltimorecybertrustroot-certificate"></a>Was geschieht, wenn das BaltimoreCyberTrustRoot-Zertifikat entfernt wurde?

Beim Herstellen einer Verbindung mit dem Azure Database for PostgreSQL-Server treten Konnektivitätsfehler auf. Sie müssen SSL erneut mit [BaltimoreCyberTrustRoot](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem)-Zertifikat konfigurieren, um Konnektivität zu gewährleisten.


## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

###    <a name="1-if-i-am-not-using-ssltls-do-i-still-need-to-update-the-root-ca"></a>1. Muss ich die Stammzertifizierungsstelle auch aktualisieren, wenn ich SSL/TLS nicht verwende?
Wenn Sie SSL/TLS nicht verwenden, sind keine Schritte erforderlich. 

### <a name="2-if-i-am-using-ssltls-do-i-need-to-restart-my-database-server-to-update-the-root-ca"></a>2. Muss ich bei Verwendung von SSL/TLS meinen Datenbankserver neu starten, um die Stammzertifizierungsstelle zu aktualisieren?
Nein, der Datenbankserver muss nicht neu gestartet werden, damit das neue Zertifikat verwendet werden kann. Es handelt sich um eine clientseitige Änderung, und die eingehenden Clientverbindungen müssen das neue Zertifikat verwenden, um sicherzustellen, dass sie eine Verbindung mit dem Datenbankserver herstellen können.

### <a name="3-how-do-i-know-if-im-using-ssltls-with-root-certificate-verification"></a>3. Wie kann ich feststellen, ob ich SSL/TLS mit Stammzertifikatüberprüfung verwende?

Durch Überprüfen der Verbindungszeichenfolge können Sie ermitteln, ob Ihre Verbindungen das Stammzertifikat überprüfen.
-    Wenn Ihre Verbindungszeichenfolge `sslmode=verify-ca` oder `sslmode=verify-full` enthält, müssen Sie das Zertifikat aktualisieren.
-    Wenn Ihre Verbindungszeichenfolge `sslmode=disable`, `sslmode=allow`, `sslmode=prefer` oder `sslmode=require` enthält, müssen Sie keine Zertifikate aktualisieren. 
-    Wenn in Ihrer Verbindungszeichenfolge „sslmode“ nicht angegeben ist, müssen Sie keine Zertifikat aktualisieren.

Wenn Sie einen Client verwenden, der die Verbindungszeichenfolge abstrahiert, lesen Sie die Dokumentation des Clients, um zu ermitteln, ob Zertifikate überprüft werden. Informationen zur PostgreSQL-sslmode-Überprüfung finden Sie in den [Beschreibungen des SSL-Modus](https://www.postgresql.org/docs/11/libpq-ssl.html#ssl-mode-descriptions) in der PostgreSQL-Dokumentation.

### <a name="4-what-is-the-impact-if-using-app-service-with-azure-database-for-postgresql"></a>4. Welche Auswirkungen hat es, wenn App Service mit Azure Database for PostgreSQL verwendet wird?
Für Azure App Service-Instanzen, die eine Verbindung mit Azure Database for PostgreSQL herstellen, gibt es zwei mögliche Szenarien, je nachdem, wie Sie SSL mit Ihrer Anwendung verwenden.
*   Das neue Zertifikat wurde App Service auf Plattformebene hinzugefügt. Wenn Sie die SSL-Zertifikate verwenden, die in der App Service-Plattform in Ihrer Anwendung enthalten sind, ist keine Aktion erforderlich.
*   Wenn Sie den Pfad zur SSL-Zertifikatsdatei explizit in Ihren Code einschließen, müssen Sie das neue Zertifikat herunterladen und den Code so aktualisieren, dass das neue Zertifikat verwendet wird. Ein gutes Beispiel für dieses Szenario: Wenn Sie benutzerdefinierte Container in App Service so verwenden, wie in der [App Service-Dokumentation](../app-service/tutorial-multi-container-app.md#configure-database-variables-in-wordpress) zu lesen ist.

### <a name="5-what-is-the-impact-if-using-azure-kubernetes-services-aks-with-azure-database-for-postgresql"></a>5. Welche Auswirkungen hat es, wenn Azure Kubernetes Service (AKS) mit Azure Database for PostgreSQL verwendet wird?
Wenn Sie versuchen, mithilfe von Azure Kubernetes Service (AKS) eine Verbindung mit Azure Database for PostgreSQL herzustellen, ähnelt dies dem Zugriff über eine dedizierte Kundenhostumgebung. Die entsprechenden Schritte finden Sie [hier](../aks/ingress-own-tls.md).

### <a name="6-what-is-the-impact-if-using-azure-data-factory-to-connect-to-azure-database-for-postgresql"></a>6. Welche Auswirkungen hat es, wenn Azure Data Factory zum Herstellen einer Verbindung mit Azure Database for PostgreSQL verwendet wird?
Ein Connector, der Azure Integration Runtime verwendet, nutzt Zertifikate im Windows-Zertifikatspeicher in der von Azure gehosteten Umgebung. Diese Zertifikate sind bereits mit den neu angewendeten Zertifikaten kompatibel, daher ist keine Aktion erforderlich.

Bei einem Connector, der die selbstgehostete Integration Runtime verwendet, für die Sie den Pfad zur SSL-Zertifikatsdatei explizit in die Verbindungszeichenfolge einschließen, müssen Sie das [neue Zertifikat](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem) herunterladen und die Verbindungszeichenfolge aktualisieren, damit es verwendet werden kann.

### <a name="7-do-i-need-to-plan-a-database-server-maintenance-downtime-for-this-change"></a>7. Muss ich für diese Änderung eine Wartungsdowntime für den Datenbankserver planen?
Nein. Da die Änderung hier nur auf der Clientseite erfolgt, um eine Verbindung mit dem Datenbankserver herzustellen, ist für diese Änderung keine Wartungsdowntime beim Datenbankserver erforderlich.

### <a name="8-if-i-create-a-new-server-after-february-15-2021-02152021-will-i-be-impacted"></a>8. Bin ich betroffen, wenn ich nach dem 15. Februar 2021 (15.02.2021) einen neuen Server erstelle?
Bei Servern, die nach dem 15. Februar 2021 (15.02.2021) erstellt werden, verwenden Sie weiterhin [BaltimoreCyberTrustRoot](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) für Ihre Anwendungen, um eine SSL-Verbindung herzustellen.

### <a name="9-how-often-does-microsoft-update-their-certificates-or-what-is-the-expiry-policy"></a>9. Wie oft aktualisiert Microsoft seine Zertifikate, bzw. welche Ablaufrichtlinie gilt?
Diese von Azure Database for PostgreSQL verwendeten Zertifikate werden von vertrauenswürdigen Zertifizierungsstellen (ZS) bereitgestellt. Die Unterstützung dieser Zertifikate ist also an die Unterstützung dieser Zertifikate durch die ZS gebunden. Das [BaltimoreCyberTrustRoot](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem)-Zertifikat läuft im Jahr 2025 ab, sodass Microsoft vor dem Ablauf eine Zertifikatänderung durchführen muss. Sollten in diesen vordefinierten Zertifikaten unvorhergesehene Fehler auftreten, muss Microsoft die Zertifikatrotation frühestens wie die Änderung am 15. Februar 2021 durchführen, um zu gewährleisten, dass der Dienst jederzeit sicher und konform ist.

### <a name="10-if-i-am-using-read-replicas-do-i-need-to-perform-this-update-only-on-the-primary-server-or-the-read-replicas"></a>10. Muss ich bei Verwendung von Lesereplikaten dieses Update nur auf dem primären Server oder für die gelesenen Replikate ausführen?
Da dieses Update eine clientseitige Änderung ist, müssen Sie die Änderungen auch für diese Clients anwenden, wenn der Client Daten vom Replikatserver gelesen hat. 

### <a name="11-do-we-have-server-side-query-to-verify-if-ssl-is-being-used"></a>11. Gibt es eine serverseitige Abfrage, um zu überprüfen, ob SSL verwendet wird?
Um zu überprüfen, ob Sie eine SSL-Verbindung zum Herstellen einer Verbindung mit dem Server verwenden, lesen Sie die Informationen unter [Überprüfen der SSL-Verbindung](concepts-ssl-connection-security.md#applications-that-require-certificate-verification-for-tls-connectivity).

### <a name="12-is-there-an-action-needed-if-i-already-have-the-digicertglobalrootg2-in-my-certificate-file"></a>12. Ist eine Aktion erforderlich, wenn sich DigiCertGlobalRootG2 bereits in meiner Zertifikatsdatei befindet?
Nein Wenn in Ihrer Zertifikatsdatei **DigiCertGlobalRootG2** bereits enthalten ist, ist keine Aktion erforderlich.

### <a name="13-what-if-you-are-using-docker-image-of-pgbouncer-sidecar-provided-by-microsoft"></a>13. Was ist, wenn Sie ein Docker-Image des von Microsoft bereitgestellten PgBouncer Sidecar verwenden?
Ein neues Docker-Image, das sowohl [**Baltimore**](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) als auch [**DigiCert**](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem) unterstützt, wird [hier](https://hub.docker.com/_/microsoft-azure-oss-db-tools-pgbouncer-sidecar) veröffentlicht (neuestes Tag). Sie können dieses neue Image abrufen, um eine Unterbrechung der Konnektivität ab dem 15. Februar 2021 zu vermeiden. 

### <a name="14-what-if-i-have-further-questions"></a>14. Wie gehe ich vor, wenn ich weitere Fragen habe?
Bei Fragen können Sie Antworten von Communityexperten auf [Microsoft Q&A](mailto:AzureDatabaseforPostgreSQL@service.microsoft.com) erhalten. Wenn Sie über einen Supportplan verfügen und technische Hilfe benötigen, [kontaktieren Sie uns](mailto:AzureDatabaseforPostgreSQL@service.microsoft.com).
