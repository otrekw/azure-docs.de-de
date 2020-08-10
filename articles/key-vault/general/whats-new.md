---
title: Neuerungen für Azure Key Vault | Microsoft-Dokumentation
description: Aktuelle Updates für Azure Key Vault
services: key-vault
author: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 07/27/2020
ms.author: mbaldwin
ms.openlocfilehash: 6df9d5de88a3317c509279a7667c1a60eaf26a79
ms.sourcegitcommit: 97a0d868b9d36072ec5e872b3c77fa33b9ce7194
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/04/2020
ms.locfileid: "87563640"
---
# <a name="whats-new-for-azure-key-vault"></a>Neuerungen für Azure Key Vault

> [!WARNING]
> **Juli 2020:** Es gibt zwei Updates für Key Vault, die sich möglicherweise auf Implementierungen des Diensts auswirken: [Standardmäßig aktiviertes vorläufiges Löschen](#soft-delete-on-by-default) und [TLS-Zertifikatänderungen für Azure](#azure-tls-certificate-changes). Weitere Informationen siehe unten.

Dies sind die Neuerungen bei Azure Key Vault. Neue Funktionen und Verbesserungen werden außerdem im [Key Vault-Kanal von Azure Updates](https://azure.microsoft.com/updates/?category=security&query=Key%20vault) angekündigt.

## <a name="soft-delete-on-by-default"></a>Standardmäßig aktiviertes vorläufiges Löschen

Ende 2020 **wird vorläufiges Löschen standardmäßig für alle Schlüsseltresore aktiviert**. Dies gilt sowohl für neue als auch für bereits vorhandene Schlüsseltresore. Ausführliche Informationen zu dieser Änderung, bei der es sich unter Umständen um einen Breaking Change handelt, sowie Schritte, mit denen Sie nach betroffenen Schlüsseltresoren suchen und sie vorab aktualisieren können, finden Sie im Artikel [Vorläufiges Löschen wird für alle Schlüsseltresore aktiviert](soft-delete-change.md). 

## <a name="azure-tls-certificate-changes"></a>TLS-Zertifikatänderungen für Azure  

Microsoft aktualisiert Azure-Dienste für die Verwendung von TLS-Zertifikaten aus einer anderen Gruppe von Stammzertifizierungsstellen (Certificate Authorities, CAs). Diese Änderung wird vorgenommen, da die aktuellen Zertifizierungsstellenzertifikate eine der grundlegenden Anforderungen des Zertifizierungsstellen-/Browserforums nicht erfüllen.

### <a name="when-will-this-change-happen"></a>Wann wird diese Änderung durchgeführt?

- Für Azure AD-Dienste (Azure Active Directory) wurde diese Umstellung am 7. Juli 2020 initiiert.
- Alle neu erstellten TLS/SSL-Azure-Endpunkte enthalten aktualisierte Zertifikate, die mit den neuen Stammzertifizierungsstellen verkettet sind. 
- Bereits vorhandene Azure-Endpunkte werden zwischen dem 13. August 2020 und dem 26. Oktober 2020 nach und nach umgestellt.

> [!IMPORTANT]
> Kunden müssen ihre Anwendungen nach dieser Änderung ggf. aktualisieren, um Konnektivitätsfehler beim Herstellen einer Verbindung mit Azure-Diensten zu vermeiden. 

### <a name="what-is-changing"></a>Was ändert sich?

Heutzutage sind die meisten der von Azure-Diensten verwendeten TLS-Zertifikate mit der folgenden Stammzertifizierungsstelle verkettet:

| Allgemeiner Name der Zertifizierungsstelle | Fingerabdruck (SHA-1) |
|--|--|
| [Baltimore CyberTrust Root](https://cacerts.digicert.com/BaltimoreCyberTrustRoot.crt) | d4de20d05e66fc53fe1a50882c78db2852cae474 |

Von Azure-Diensten verwendete TLS-Zertifikate werden mit einer der folgenden Stammzertifizierungsstellen verkettet:

| Allgemeiner Name der Zertifizierungsstelle | Fingerabdruck (SHA-1) |
|--|--|
| [DigiCert Global Root G2](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt) | df3c24f9bfd666761b268073fe06d1cc8d4f82a4 |
| [DigiCert Global Root CA](https://cacerts.digicert.com/DigiCertGlobalRootCA.crt) | a8985d3a65e5e5c4b2d7d66d40c6dd2fb19c5436 |
| [Baltimore CyberTrust Root](https://cacerts.digicert.com/BaltimoreCyberTrustRoot.crt) | d4de20d05e66fc53fe1a50882c78db2852cae474 |
| [D-TRUST Root Class 3 CA 2 2009](https://www.d-trust.net/cgi-bin/D-TRUST_Root_Class_3_CA_2_2009.crt) | 58e8abb0361533fb80f79b1b6d29d3ff8d5f00f0 |
| [Microsoft RSA Root Certificate Authority 2017](https://www.microsoft.com/pkiops/certs/Microsoft%20RSA%20Root%20Certificate%20Authority%202017.crt) | 73a5e64a3bff8316ff0edccc618a906e4eae4d74 | 
| [Microsoft EV ECC Root Certificate Authority 2017](https://www.microsoft.com/pkiops/certs/Microsoft%20EV%20ECC%20Root%20Certificate%20Authority%202017.crt) | 6b1937abfd64e1e40daf2262a27857c015d6228d |

### <a name="when-can-i-retire-the-old-intermediate-thumbprint"></a>Wann kann ich den alten Zwischenfingerabdruck entfernen?

Die aktuellen Zertifizierungsstellenzertifikate bis zum 15. Februar 2021 *nicht* widerrufen. Nach diesem Datum können Sie die alten Fingerabdrücke aus Ihrem Code entfernen.

Sollte sich dieses Datum ändern, wird das neue Widerrufsdatum bekanntgegeben.

### <a name="will-this-affect-me"></a>Bin ich davon betroffen?

Wir gehen davon aus, dass **die meisten Azure-Kunden nicht betroffen sein werden**.  Ihre Anwendung kann jedoch betroffen sein, wenn darin explizit eine Liste zulässiger Zertifizierungsstellen angegeben wird. Dies wird als Anheften von Zertifikaten bezeichnet.

Im Anschluss finden Sie verschiedene Methoden, mit denen Sie ermitteln können, ob Ihre Anwendung betroffen ist:

- Suchen Sie in Ihrem Quellcode nach dem Fingerabdruck, nach dem allgemeinen Namen und nach anderen Zertifikateigenschaften der Microsoft IT TLS-Zertifizierungsstellen, die [hier](https://www.microsoft.com/pki/mscorp/cps/default.htm) angegeben sind. Wird ein entsprechender Wert gefunden, ist Ihre Anwendung betroffen. Aktualisieren Sie in diesem Fall den Quellcode mit den neuen Zertifizierungsstellen, um das Problem zu beheben. Es empfiehlt sich grundsätzlich, sicherzustellen, dass Zertifizierungsstellen kurzfristig hinzugefügt oder bearbeitet werden können. Zertifizierungsstellenzertifikate müssen aufgrund von Branchenbestimmungen innerhalb von sieben Tagen ersetzt werden. Kunden, die das Anheften von Zertifikaten nutzen, müssen daher schnell reagieren.

- Wenn Sie über eine Anwendung mit Azure-API- oder Azure-Dienstintegration verfügen und nicht sicher sind, ob sie das Anheften von Zertifikaten nutzt, wenden Sie sich den Hersteller der Anwendung.

- Für unterschiedliche Betriebssysteme und Sprach-Runtimes, die mit Azure-Diensten kommunizieren, sind möglicherweise zusätzliche Schritte erforderlich, um eine ordnungsgemäße Zertifikatkette mit den neuen Stammzertifizierungsstellen einzurichten: 
    - **Linux:** Bei vielen Distributionen müssen die oben aufgeführten Zertifizierungsstellen zu „/etc/ssl/certs“ hinzugefügt werden. Spezifische Anweisungen finden Sie in der Dokumentation der Distribution.
    - **Java:** Stellen Sie sicher, dass der Java-Schlüsselspeicher die oben aufgeführten Zertifizierungsstellen enthält.
    - **Windows in nicht verbundenen Umgebungen:** Für Systeme, die in nicht verbundenen Umgebungen betrieben werden, müssen die oben aufgeführten Stammzertifizierungsstellen dem Speicher für vertrauenswürdige Stammzertifizierungsstellen und die Zwischenzertifizierungsstellen dem Speicher für Zwischenzertifizierungsstellen hinzugefügt werden.
    - **Android:** Überprüfen Sie die Dokumentation für Ihr Gerät und für Ihre Android-Version.
    - **Andere Hardwaregeräte (insbesondere IoT):** Wenden Sie sich an den Hersteller des Geräts. 

- Wenn Sie über eine Umgebung mit Firewallregeln verfügen, die dazu führen, dass ausgehende Aufrufe nur für bestimmte CRL-Downloadorte (Certificate Revocation List, Zertifikatsperrliste) und/oder OCSP-Überprüfungsorte (Online Certificate Status-Protokoll) zugelassen werden. In diesem Fall müssen die folgenden CRL- und OCSP-URLs zugelassen werden:

    - http://crl3&#46;digicert&#46;com
    - http://crl4&#46;digicert&#46;com
    - http://ocsp&#46;digicert&#46;com
    - http://www&#46;d-trust&#46;net
    - http://root-c3-ca2-2009&#46;ocsp&#46;d-trust&#46;net
    - http://crl&#46;microsoft&#46;com
    - http://oneocsp&#46;microsoft&#46;com
    - http://ocsp&#46;msocsp&#46;com

## <a name="june-2020"></a>Juni 2020

Azure Monitor für Key Vault ist jetzt als Vorschauversion erhältlich.  Azure Monitor ermöglicht eine umfassende Überwachung Ihrer Schlüsseltresore. Zu diesem Zweck wird eine einheitliche Darstellung der Anforderungen, Leistung, Fehler und Wartezeit im Zusammenhang mit Key Vault bereitgestellt. Weitere Informationen finden Sie unter [Azure Monitor für Key Vault (Vorschau).](../../azure-monitor/insights/key-vault-insights-overview.md).

## <a name="may-2020"></a>Mai 2020

Key Vault-BYOK („Bring-Your-Own-Key“) ist jetzt allgemein verfügbar. Mehr dazu finden Sie in der [Azure Key Vault-BYOK-Spezifikation](../keys/byok-specification.md), und erfahren Sie, wie Sie [HSM-geschützte Schlüssel in Key Vault (BYOK) importieren](../keys/hsm-protected-keys-byok.md).

## <a name="march-2020"></a>März 2020

Private Endpunkte sind jetzt als Vorschauversion erhältlich. Mit dem Azure Private Link-Dienst können Sie über einen privaten Endpunkt in Ihrem virtuellen Netzwerk auf Azure Key Vault sowie auf in Azure gehostete Kunden-/Partnerdienste zugreifen.  Weitere Informationen zum [Integrieren von Key Vault in Azure Private Link](private-link-service.md).

## <a name="2019"></a>2019

- Veröffentlichung der nächsten Generation von Azure Key Vault SDKs. Beispiele zur Verwendung finden Sie in den Schnellstartanleitungen zu Azure Key Vault-Geheimnissen für [Python](../secrets/quick-create-python.md), [.NET](../secrets/quick-create-net.md), [Java](../secrets/quick-create-java.md) und [Node.js](../secrets/quick-create-node.md)
- Neue Azure-Richtlinien zum Verwalten von Key Vault-Zertifikaten. Mehr dazu erfahren Sie unter [Integrierte Azure Policy-Definitionen für Key Vault](../policy-samples.md).
- Die Azure Key Vault-Erweiterung für virtuelle Computer ist jetzt allgemein verfügbar.  Informationen finden Sie unter [Key Vault-VM-Erweiterung für Linux](../../virtual-machines/extensions/key-vault-linux.md) und [Key Vault-VM-Erweiterung für Windows](../../virtual-machines/extensions/key-vault-windows.md).
- Die ereignisgesteuerte Verwaltung von Geheimnissen für Azure Key Vault ist jetzt im Azure Event Grid verfügbar. Weitere Informationen finden Sie im [Event Grid-Schema für Ereignisse in Azure Key Vault](../../event-grid/event-schema-key-vault.md], und erfahren Sie mehr über das [Empfangen von und Antworten auf Schlüsseltresor-Benachrichtigungen mit Azure Event Grid](event-grid-tutorial.md).

## <a name="2018"></a>2018

In diesem Jahr veröffentlichte neue Features und Integrationen:

- Integration in Azure Functions. Ein Beispielszenario, in dem [Azure Functions](../../azure-functions/index.yml) für Schlüsseltresorvorgänge genutzt wird, finden Sie unter [Automate the rotation of a secret](../secrets/tutorial-rotation.md) (Automatisieren der Rotation von Geheimnissen). 
- [Integration in Azure Databricks](/azure/databricks/scenarios/store-secrets-azure-key-vault). Hiermit unterstützt Azure Databricks jetzt zwei Typen von Geheimnisbereichen: Von Azure Key Vault unterstützte und von Databricks unterstützte. Weitere Informationen finden Sie unter [Erstellen eines von Azure Key Vault unterstützten Geheimnisbereichs](/azure/databricks/security/secrets/secret-scopes#--create-an-azure-key-vault-backed-secret-scope)
- [VNET-Dienstendpunkte für Azure Key Vault](overview-vnet-service-endpoints.md).

## <a name="2016"></a>2016

In diesem Jahr veröffentlichte neue Features:

- Schlüssel für verwaltete Speicherkonten. Das hinzugefügte Feature „Speicherkontoschlüssel“ erleichtert die Integration in Azure Storage. Weitere Informationen finden Sie im Übersichtsthema [Azure Key Vault-Speicherkontoschlüssel](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-storage-keys).
- Vorläufiges Löschen. Das Feature „Vorläufiges Löschen“ verbessert den Datenschutz Ihrer Schlüsseltresore und der Schlüsseltresorobjekte. Weitere Informationen finden Sie im Übersichtsthema [Übersicht über die Azure Key Vault-Funktion für vorläufiges Löschen](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete).

## <a name="2015"></a>2015

In diesem Jahr veröffentlichte neue Features:
- Zertifikatverwaltung. Als Funktion der GA-Version 2015-06-01 am 26. September 2016 hinzugefügt.

Die allgemein verfügbare Version 2015-06-01 wurde am 24. Juni 2015 angekündigt. Die folgenden Änderungen wurden in dieser Version vorgenommen: 
- Löschen eines Schlüssels: Feld „use“ entfernt.
- Abrufen von Informationen über einen Schlüssel: Feld „use“ entfernt.
- Importieren eines Schlüssels in einen Tresor: Feld „use“ entfernt.
- Wiederherstellen eines Schlüssels: Feld „use“ entfernt.     
- Für RSA-Algorithmen „RSA_OAEP“ in „RSA-OAEP“ geändert. Siehe [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](about-keys-secrets-certificates.md).    
 
Die zweite Vorschauversion (Version 2015-02-01-preview) wurde am 20. April 2015 angekündigt. Weitere Informationen finden Sie im Blogbeitrag zum [REST-API-Update](https://docs.microsoft.com/archive/blogs/kv/rest-api-update). Die folgenden Aufgaben wurden aktualisiert:
 
- Auflisten der Schlüssel in einem Tresor: Unterstützung für Paginierung wurde zum Vorgang hinzugefügt.
- Auflisten der Versionen eines Schlüssels: Vorgang zum Auflisten der Versionen eines Schlüssels wurde hinzugefügt.  
- Auflisten der Geheimnisse in einem Tresor: Unterstützung für Paginierung wurde hinzugefügt.
- Auflisten der Versionen eines Geheimnisses: Vorgang zum Auflisten der Versionen eines Geheimnisses wurde hinzugefügt.  
- Alle Vorgänge: Attributen wurden Zeitstempel der Erstellung/Aktualisierung hinzugefügt.  
- Erstellen eines Geheimnisses:Geheimnissen wurde ein Inhaltstyp hinzugefügt.
- Erstellen eines Schlüssels: Tags wurden als optionale Informationen hinzugefügt.
- Erstellen eines Geheimnisses: Tags wurden als optionale Informationen hinzugefügt.
- Aktualisieren eines Schlüssels: Tags wurden als optionale Informationen hinzugefügt.
- Aktualisieren eines Geheimnisses: Tags wurden als optionale Informationen hinzugefügt.
- Maximale Größe für Geheimnisse wurde von 10.000 in 25.000 Bytes geändert. Siehe [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](about-keys-secrets-certificates.md).    

## <a name="2014"></a>2014
 
Die erste Vorschauversion (Version 2014-12-08-preview) wurde am 8. Januar 2015 angekündigt.  

## <a name="next-steps"></a>Nächste Schritte

Sollten Sie weitere Fragen haben, wenden Sie sich an den [Support](https://azure.microsoft.com/support/options/).  
