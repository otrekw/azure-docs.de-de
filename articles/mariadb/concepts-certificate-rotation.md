---
title: Zertifikatrotation für Azure Database for MariaDB
description: Hier erfahren Sie mehr über die bevorstehenden Änderungen an Stammzertifikaten im Zusammenhang mit Azure Database for MariaDB.
author: kummanish
ms.author: manishku
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/02/2020
ms.openlocfilehash: f35a43e9cbffb2613f7a98e02b03840c774e5999
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/16/2020
ms.locfileid: "90708154"
---
# <a name="understanding-the-changes-in-the-root-ca-change-for-azure-database-for-mariadb"></a>Grundlegendes zu den Änderungen im Zusammenhang mit der Stammzertifizierungsstelle für Azure Database for MariaDB

Azure Database for MariaDB ändert das Stammzertifikat für die Clientanwendung oder den Clienttreiber mit SSL-Aktivierung, die bzw. den Sie zum [Herstellen einer Verbindung mit dem Datenbankserver](concepts-connectivity-architecture.md) verwenden. Für das derzeit verfügbare Stammzertifikat wird im Rahmen der standardmäßigen Wartung und bewährten Sicherheitsmethoden der Ablauf auf den 26. Oktober 2020 festgelegt. In diesem Artikel erhalten Sie ausführlichere Informationen zu den bevorstehenden Änderungen, den betroffenen Ressourcen sowie den Schritten, die erforderlich sind, um sicherzustellen, dass die Konnektivität Ihrer Anwendung mit dem Datenbankserver bestehen bleibt.

## <a name="what-update-is-going-to-happen"></a>Welches Update wird durchgeführt?

In einigen Fällen verwenden Anwendungen eine lokale Zertifikatdatei, die auf der Grundlage der Zertifikatdatei einer vertrauenswürdigen Zertifizierungsstelle (Certificate Authority, CA) generiert wurde, um eine sichere Verbindung herzustellen. Derzeit können Kunden nur das vordefinierte Zertifikat zum Herstellen einer Verbindung mit einem Azure Database for MariaDB-Server verwenden. Dieses Zertifikat finden Sie [hier](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem). Im [ZA-/Browserforum](https://cabforum.org/) wurden jedoch kürzlich Berichte veröffentlicht, in denen mehrere von Zertifizierungsstellenanbietern ausgestellte Zertifikate aufgeführt sind, die nicht konform sind.

Gemäß den Konformitätsanforderungen der Branche haben Zertifizierungsstellenanbieter damit begonnen, Zertifizierungsstellenzertifikate für nicht konforme Zertifizierungsstellen zu sperren, sodass Server Zertifikate verwenden müssen, die von konformen Zertifizierungsstellen ausgestellt und durch Zertifizierungsstellenzertifikate von diesen konformen Zertifizierungsstellen signiert wurden. Da Azure Database for MariaDB derzeit eines dieser nicht konformen Zertifikate verwendet, die von Clientanwendungen zum Überprüfen der SSL-Verbindungen genutzt werden, müssen wir sicherstellen, dass entsprechende Maßnahmen ergriffen werden (siehe unten), um die potenziellen Auswirkungen auf Ihre MariaDB-Server zu minimieren.


Das neue Zertifikat wird ab dem 26. Oktober 2020 verwendet. Wenn Sie beim Herstellen einer Verbindung von einem MySQL-Client entweder die Zertifizierungsstellenüberprüfung oder die vollständige Überprüfung des Serverzertifikats („sslmode=verify-ca“ bzw. „sslmode=verify-full“) verwenden, müssen Sie die Anwendungskonfiguration vor dem 26. Oktober 2020 aktualisieren.

## <a name="how-do-i-know-if-my-database-is-going-to-be-affected"></a>Wie erkenne ich, ob meine Datenbank betroffen ist?

Alle Anwendungen, die SSL/TLS verwenden und das Stammzertifikat überprüfen, müssen das Stammzertifikat aktualisieren. Durch Überprüfen der Verbindungszeichenfolge können Sie ermitteln, ob Ihre Verbindungen das Stammzertifikat überprüfen.
-   Wenn Ihre Verbindungszeichenfolge `sslmode=verify-ca` oder `sslmode=verify-full` enthält, müssen Sie das Zertifikat aktualisieren.
-   Wenn Ihre Verbindungszeichenfolge `sslmode=disable`, `sslmode=allow`, `sslmode=prefer` oder `sslmode=require` enthält, müssen Sie keine Zertifikate aktualisieren. 
-   Wenn in Ihrer Verbindungszeichenfolge „sslmode“ nicht angegeben ist, müssen Sie keine Zertifikat aktualisieren.

Wenn Sie einen Client verwenden, der die Verbindungszeichenfolge abstrahiert, lesen Sie die Dokumentation des Clients, um zu ermitteln, ob Zertifikate überprüft werden.
Informationen zum Azure Database for MariaDB-Parameter „sslmode“ finden Sie in der [Beschreibung der SSL-Modus](concepts-ssl-connection-security.md#default-settings).

Informationen dazu, wie Sie verhindern, dass die Verfügbarkeit Ihrer Anwendung aufgrund unerwartet widerrufener Zertifikate unterbrochen wird, oder wie Sie ein widerrufenes Zertifikat aktualisieren, finden Sie im Abschnitt [**Was muss ich tun, um die Konnektivität aufrechtzuerhalten?** ](concepts-certificate-rotation.md#what-do-i-need-to-do-to-maintain-connectivity).

## <a name="what-do-i-need-to-do-to-maintain-connectivity"></a>Was muss ich tun, um die Konnektivität aufrechtzuerhalten?

Führen Sie die unten angegebenen Schritte aus, um zu verhindern, dass die Verfügbarkeit Ihrer Anwendung aufgrund unerwartet widerrufener Zertifikate unterbrochen wird, oder um ein Zertifikat zu aktualisieren. Die Idee dahinter ist, eine neue *PEM*-Datei zu erstellen, die das aktuelle Zertifikat und das neue Zertifikat kombiniert. Während der SSL-Zertifikatüberprüfung wird einer der zulässigen Werte verwendet. Sehen Sie sich die folgenden Schritte an:

*   Laden Sie die Stammzertifizierungsstellen **BaltimoreCyberTrustRoot** & **DigiCertGlobalRootG2** über die folgenden Links herunter:
    *   https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem
    *   https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem

*   Generieren Sie einen kombinierten Zertifizierungsstellen-Zertifikatspeicher, in dem sowohl **BaltimoreCyberTrustRoot** als auch **DigiCertGlobalRootG2** enthalten sind.
    *   Führen Sie für Java-Benutzer (MariaDB-Connector/J) Folgendes aus:

          ```azurecli-interactive
          keytool -importcert -alias MariaDBServerCACert  -file D:\BaltimoreCyberTrustRoot.crt.pem  -keystore truststore -storepass password -noprompt
          ```

          ```azurecli-interactive
          keytool -importcert -alias MariaDBServerCACert2  -file D:\DigiCertGlobalRootG2.crt.pem -keystore truststore -storepass password  -noprompt
          ```

          Ersetzen Sie dann die ursprüngliche Keystore-Datei durch die neu generierte Datei:
        *   System.setProperty("javax.net.ssl.trustStore","Pfad_zur_Truststore-Datei"); 
        *   System.setProperty("javax.net.ssl.trustStorePassword","password");
    *   Stellen Sie bei .NET-Benutzern (MariaDB-Connector/NET, MariaDBConnector) sicher, dass **BaltimoreCyberTrustRoot** und **DigiCertGlobalRootG2** im Windows-Zertifikatspeicher unter vertrauenswürdigen Stammzertifizierungsstellen enthalten sind. Ist eines der Zertifikate nicht vorhanden, importieren Sie das fehlende Zertifikat.

        ![Azure Database for MariaDB: .NET-Zertifikat](media/overview/netconnecter-cert.png)

    *   Stellen Sie bei .NET-Benutzern unter Linux, die SSL_CERT_DIR verwenden, sicher, dass **BaltimoreCyberTrustRoot** und **DigiCertGlobalRootG2** in dem durch SSL_CERT_DIR angegebenen Verzeichnis enthalten sind. Ist eines der Zertifikate nicht vorhanden, erstellen Sie die fehlende Zertifikatsdatei.

    *   Bei anderen Benutzern (MariaDB-Client/MariaDB Workbench/C/C++/Go/Python/Ruby/PHP/NodeJS/Perl/Swift) können Sie zwei ZS-Zertifikatsdateien im folgenden Format zusammenführen:</b>

        </br>-----BEGIN CERTIFICATE-----
 </br>(Root CA1: BaltimoreCyberTrustRoot.crt.pem)
 </br>-----END CERTIFICATE-----
 </br>-----BEGIN CERTIFICATE-----
 </br>(Root CA2: DigiCertGlobalRootG2.crt.pem)
 </br>-----END CERTIFICATE-----

*   Ersetzen Sie die ursprüngliche PEM-Datei der Stammzertifizierungsstelle durch die kombinierte Datei der Stammzertifizierungsstelle, und starten Sie die Anwendung bzw. den Client neu.
*   Nachdem das neue Zertifikat auf der Serverseite bereitgestellt wurde, können Sie die PEM-Datei der Zertifizierungsstelle in Zukunft in „DigiCertGlobalRootG2.crt.pem“ ändern.

## <a name="what-can-be-the-impact-of-not-updating-the-certificate"></a>Welche Auswirkungen kann es haben, wenn das Zertifikat nicht aktualisiert wird?
Wenn Sie das ausgestellte Azure Database for MariaDB-Zertifikat wie hier dokumentiert verwenden, wird die Verfügbarkeit Ihrer Anwendung möglicherweise unterbrochen, da die Datenbank nicht erreichbar ist. Abhängig von Ihrer Anwendung erhalten Sie möglicherweise eine Vielzahl von Fehlermeldungen, einschließlich der folgenden:
*   Ungültiges Zertifikat/Widerrufenes Zertifikat
*   Timeout bei Verbindung

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="1-if-i-am-not-using-ssltls-do-i-still-need-to-update-the-root-ca"></a>1. Muss ich die Stammzertifizierungsstelle auch aktualisieren, wenn ich SSL/TLS nicht verwende?
Wenn Sie SSL/TLS nicht verwenden, sind keine Schritte erforderlich. 

### <a name="2-if-i-am-using-ssltls-do-i-need-to-restart-my-database-server-to-update-the-root-ca"></a>2. Muss ich bei Verwendung von SSL/TLS meinen Datenbankserver neu starten, um die Stammzertifizierungsstelle zu aktualisieren?
Nein, der Datenbankserver muss nicht neu gestartet werden, damit das neue Zertifikat verwendet werden kann. Die Zertifikataktualisierung ist eine clientseitige Änderung, und die eingehenden Clientverbindungen müssen das neue Zertifikat verwenden, um sicherzustellen, dass sie eine Verbindung mit dem Datenbankserver herstellen können.

### <a name="3-what-will-happen-if-i-do-not-update-the-root-certificate-before-october-26-2020-10262020"></a>3. Was geschieht, wenn ich das Stammzertifikat nicht vor dem 26. Oktober 2020 aktualisiere?
Wenn Sie das Stammzertifikat nicht vor dem 26. Oktober 2020 aktualisieren, können Ihre Anwendung, die über SSL/TLS eine Verbindung herstellen und die Überprüfung des Stammzertifikats durchführen, nicht mit dem MariaDB-Datenbankserver kommunizieren. Außerdem treten Verbindungsprobleme im Zusammenhang mit dem MariaDB-Datenbankserver auf.

### <a name="4-what-is-the-impact-if-using-app-service-with-azure-database-for-mariadb"></a>4. Welche Auswirkungen hat es, wenn App Service mit Azure Database for MariaDB verwendet wird?
Für Azure App Service-Instanzen, die eine Verbindung mit Azure Database for MariaDB herstellen, gibt es zwei mögliche Szenarien, abhängig davon, wie Sie SSL mit Ihrer Anwendung verwenden.
*   Dieses neue Zertifikat wurde App Service auf Plattformebene hinzugefügt. Wenn Sie die SSL-Zertifikate verwenden, die in der App Service-Plattform in Ihrer Anwendung enthalten sind, ist keine Aktion erforderlich.
*   Wenn Sie den Pfad zur SSL-Zertifikatsdatei explizit in Ihren Code einschließen, müssen Sie das neue Zertifikat herunterladen und den Code so aktualisieren, dass das neue Zertifikat verwendet wird.

### <a name="5-what-is-the-impact-if-using-azure-kubernetes-services-aks-with-azure-database-for-mariadb"></a>5. Welche Auswirkungen hat es, wenn Azure Kubernetes Service (AKS) mit Azure Database for MariaDB verwendet wird?
Wenn Sie versuchen, mithilfe von Azure Kubernetes Service (AKS) eine Verbindung mit Azure Database for MariaDB herzustellen, ähnelt dies dem Zugriff über eine dedizierte Kundenhostumgebung. Die entsprechenden Schritte finden Sie [hier](../aks/ingress-own-tls.md).

### <a name="6-what-is-the-impact-if-using-azure-data-factory-to-connect-to-azure-database-for-mariadb"></a>6. Welche Auswirkungen hat es, wenn Azure Data Factory zum Herstellen einer Verbindung mit Azure Database for MariaDB verwendet wird?
Ein Connector, der Azure Integration Runtime verwendet, nutzt Zertifikate im Windows-Zertifikatspeicher in der von Azure gehosteten Umgebung. Diese Zertifikate sind bereits mit den neu angewendeten Zertifikaten kompatibel, daher ist keine Aktion erforderlich.

Bei einem Connector, der die selbstgehostete Integration Runtime verwendet, für die Sie den Pfad zur SSL-Zertifikatsdatei explizit in die Verbindungszeichenfolge einschließen, müssen Sie das [neue Zertifikat](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem) herunterladen und die Verbindungszeichenfolge aktualisieren, damit es verwendet werden kann.

### <a name="7-do-i-need-to-plan-a-database-server-maintenance-downtime-for-this-change"></a>7. Muss ich für diese Änderung eine Wartungsdowntime für den Datenbankserver planen?
Nein. Da die Änderung hier nur auf der Clientseite erfolgt, um eine Verbindung mit dem Datenbankserver herzustellen, ist für diese Änderung keine Wartungsdowntime beim Datenbankserver erforderlich.

### <a name="8--what-if-i-cannot-get-a-scheduled-downtime-for-this-change-before-october-26-2020-10262020"></a>8.  Was geschieht, wenn vor dem 26. Oktober 2020 (26.10.2020) keine geplante Downtime für diese Änderung möglich ist?
Da die Clients, die zum Herstellen einer Verbindung mit dem Server verwendet werden, die Zertifikatinformationen gemäß [diesem Abschnitt](./concepts-certificate-rotation.md#what-do-i-need-to-do-to-maintain-connectivity) aktualisieren müssen, ist in diesem Fall keine Downtime beim Server erforderlich.

### <a name="9-if-i-create-a-new-server-after-october-26-2020-will-i-be-impacted"></a>9. Bin ich betroffen, wenn ich nach dem 26. Oktober 2020 einen neuen Server erstelle?
Für Server, die nach dem 26. Oktober 2020 erstellt werden, können Sie das neu ausgestellte Zertifikat für Ihre Anwendungen verwenden, um eine Verbindung mithilfe von SSL herzustellen.

### <a name="10-how-often-does-microsoft-update-their-certificates-or-what-is-the-expiry-policy"></a>10. Wie oft aktualisiert Microsoft seine Zertifikate, bzw. welche Ablaufrichtlinie gilt?
Diese von Azure Database for MariaDB verwendeten Zertifikate werden von vertrauenswürdigen Zertifizierungsstellen (ZS) bereitgestellt. Die Unterstützung dieser Zertifikate in Azure Database for MariaDB ist also an die Unterstützung dieser Zertifikate durch die ZS gebunden. Wie in diesem Fall kann es jedoch zu unvorhergesehenen Fehlern in diesen vordefinierten Zertifikaten kommen, die schnellstmöglich behoben werden müssen.

### <a name="11-if-i-am-using-read-replicas-do-i-need-to-perform-this-update-only-on-master-server-or-the-read-replicas"></a>11. Muss ich bei Verwendung von Lesereplikaten dieses Update nur auf dem Masterserver oder für die gelesenen Replikate ausführen?
Da dieses Update eine clientseitige Änderung ist, müssen Sie die Änderungen auch für diese Clients anwenden, wenn der Client Daten vom Replikatserver gelesen hat.

### <a name="12-do-we-have-server-side-query-to-verify-if-ssl-is-being-used"></a>12. Gibt es eine serverseitige Abfrage, um zu überprüfen, ob SSL verwendet wird?
Um zu überprüfen, ob Sie eine SSL-Verbindung zum Herstellen einer Verbindung mit dem Server verwenden, lesen Sie die Informationen unter [Überprüfen der SSL-Verbindung](howto-configure-ssl.md#verify-the-ssl-connection).

### <a name="13-what-if-i-have-further-questions"></a>13. Wie gehe ich vor, wenn ich weitere Fragen habe?
Bei Fragen können Sie Antworten von Communityexperten auf [Microsoft Q&A](mailto:AzureDatabaseformariadb@service.microsoft.com) erhalten. Wenn Sie über einen Supportplan verfügen und technische Hilfe benötigen, [kontaktieren Sie uns](mailto:AzureDatabaseformariadb@service.microsoft.com).
