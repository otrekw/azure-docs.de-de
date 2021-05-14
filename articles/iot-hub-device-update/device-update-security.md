---
title: Sicherheit für Device Update for IoT Hub | Microsoft-Dokumentation
description: Erfahren Sie, wie Device Update for IoT Hub sicherstellt, dass Geräte auf sichere Weise aktualisiert werden.
author: lichris
ms.author: lichris
ms.date: 4/15/2021
ms.topic: conceptual
ms.service: iot-hub
ms.openlocfilehash: b10049e03e26cfe8da2bd57cc9f69dd933af706b
ms.sourcegitcommit: 590f14d35e831a2dbb803fc12ebbd3ed2046abff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2021
ms.locfileid: "107567297"
---
# <a name="device-update-security-model"></a>Device Update-Sicherheitsmodell

Device Update for IoT Hub bietet eine sichere Methode zum Bereitstellen von Updates für Gerätefirmware, Images und Anwendungen auf Ihren IoT-Geräten. Der Workflow bietet einen sicheren End-to-End-Kanal mit einem vollständigen Überwachungsmodell, das von einem Gerät verwendet werden kann, um nachzuweisen, dass ein Update vertrauenswürdig, unverändert und beabsichtigt ist.

Jeder Schritt im Device Update-Workflow wird durch verschiedene Sicherheitsfeatures und -prozesse geschützt, um sicherzustellen, dass jeder Schritt in der Pipeline eine abgesicherte Übergabe an den nächsten Schritt vornimmt. Mit dem Device Update-Client werden alle nicht legitimen Updateanforderungen identifiziert und ordnungsgemäß verwaltet. Der Client überprüft auch jeden Download, um sicherzustellen, dass der Inhalt vertrauenswürdig, unverändert und beabsichtigt ist.

## <a name="for-solution-operators"></a>Für Lösungsoperatoren

Da Lösungsoperatoren Updates in Ihre Device Update-Instanz importieren, lädt der Dienst die Updatebinärdateien hoch und überprüft diese, um sicherzustellen, dass sie nicht von einem böswilligen Benutzer geändert oder ausgetauscht wurden. Nach der Überprüfung generiert der Device Update-Dienst ein internes [Updatemanifest](./update-manifest.md) mit Dateihashes aus dem Importmanifest und anderen Metadaten. Dieses Updatemanifest wird dann vom Device Update-Dienst signiert.

Nachdem sie im Dienst erfasst und in Azure gespeichert wurden, werden die Binärdateien für Updates und die zugehörigen Kundenmetadaten vom Azure-Speicherdienst automatisch im Ruhezustand verschlüsselt. Der Device Update-Dienst bietet keine automatische zusätzliche Verschlüsselung, gibt Entwicklern jedoch die Möglichkeit, Inhalte selbst zu verschlüsseln, bevor der Inhalt den Device Update-Dienst erreicht.

Wenn der Lösungsoperator die Aktualisierung eines Geräts anfordert, wird eine signierte Nachricht über den geschützten IoT Hub-Kanal an das Gerät gesendet. Die Signatur der Anforderung wird vom Device Update-Agent des Geräts als authentisch bestätigt. 

Jeder sich ergebende binäre Download wird durch Validierung der Signatur des Updatemanifests gesichert. Das Updatemanifest enthält die Binärdateihashes. Sobald das Manifest vertrauenswürdig ist, vertraut der Device Update-Agent den Hashwerten und gleicht sie mit den Binärdateien ab. Nachdem die Updatebinärdatei heruntergeladen und überprüft wurde, wird sie auf sichere Weise an das Installationsprogramm auf dem Gerät übergeben.

## <a name="for-device-builders"></a>Für Gerätegeneratoren

Um sicherzustellen, dass der Device Update-Dienst auf einfache Geräte mit geringer Leistung herunterskaliert werden kann, verwendet das Sicherheitsmodell asymmetrische Rohschlüssel und Rohsignaturen. Diese verwenden JSON-basierte Formate wie JSON Web Token und JSON Web Keys.

### <a name="securing-update-content-via-the-update-manifest"></a>Sichern des Updateinhalts über das Updatemanifest

Das Updatemanifest wird mithilfe von zwei Signaturen überprüft. Die Signaturen werden mithilfe einer Struktur erstellt, die aus *Signaturschlüsseln* und *Stammschlüsseln* besteht.

Der Device Update-Agent verfügt über eingebettete öffentliche Schlüssel, die für alle mit Device Update kompatiblen Geräte verwendet werden. Dies sind die *Stammschlüssel*. Die entsprechenden privaten Schlüssel werden von Microsoft gesteuert.

Microsoft generiert auch ein öffentliches/privates Schlüsselpaar, das nicht im Device Update-Agent enthalten ist oder auf dem Gerät gespeichert wird. Dies ist der *Signaturschlüssel*.

Wenn ein Update in Device Update for IoT Hub importiert und das Updatemanifest vom Dienst generiert wird, signiert der Dienst das Manifest mit dem Signaturschlüssel und schließt den Signaturschlüssel selbst ein, der von einem Stammschlüssel signiert wird. Wenn das Updatemanifest an das Gerät gesendet wird, empfängt der Device Update-Agent die folgenden Signaturdaten:

1. Den Signaturwert selbst.
2. Den Algorithmus, der zum Erstellen von Nr. 1 verwendet wird.
3. Die Informationen zum öffentlichen Schlüssel des Signaturschlüssels, der zum Generieren von Nr. 1 verwendet wird.
4. Die Signatur des öffentlichen Signaturschlüssels in Nr. 3.
5. Die ID des öffentlichen Schlüssels des Stammschlüssels, der zum Generieren von Nr. 3 verwendet wird.
6. Den Algorithmus, der zum Generieren von Nr. 4 verwendet wird.

Der Device Update-Agent verwendet die oben definierten Informationen, um zu bestätigen, dass die Signatur des öffentlichen Signaturschlüssels durch den Stammschlüssel signiert ist. Der Device Update-Agent überprüft dann, ob die Signatur des Updatemanifests mit dem Signaturschlüssel signiert ist. Wenn alle Signaturen richtig sind, wird das Updatemanifest vom Device Update-Agent als vertrauenswürdig eingestuft. Da das Updatemanifest die Dateihashes enthält, die den Updatedateien selbst entsprechen, können die Updatedateien auch als vertrauenswürdig eingestuft werden, wenn die Hashwerte übereinstimmen.

Durch die Verwendung von Stamm- und Signaturschlüsseln kann Microsoft den Signaturschlüssel regelmäßig rotiert: eine bewährte Sicherheitsmaßnahme.

### <a name="json-web-signature-jws"></a>JSON Web Signature (JWS)

`updateManifestSignature` wird verwendet, um sicherzustellen, dass die in `updateManifest` enthaltenen Informationen nicht manipuliert wurden. `updateManifestSignature` wird mithilfe einer JSON Web Signature mit JSON Web Keys generiert und ermöglicht damit Quellüberprüfung. Die Signatur ist eine Base64Url-codierte Zeichenfolge mit drei Abschnitten, die durch „.“ getrennt sind.  Informationen zum Analysieren und Überprüfen von JSON-Schlüsseln und -Token finden Sie unter den [Hilfsmethoden „jws_util.h“](https://github.com/Azure/iot-hub-device-update/tree/main/src/utils/jws_utils).

Die JSON WebSignature ist ein weit verbreiteter [vorgeschlagener IETF-Standard](https://tools.ietf.org/html/rfc7515) zum Signieren von Inhalten mithilfe von JSON-basierten Datenstrukturen. Dies ist eine Möglichkeit, die Integrität von Daten sicherzustellen, indem die Signatur der Daten überprüft wird. Weitere Informationen finden Sie in [RFC 7515](https://www.rfc-editor.org/info/rfc7515) zu JSON Web Signature (JWS).

### <a name="json-web-token"></a>JSON Web Token

JSON Web Token sind eine offene [Standardmethode](https://tools.ietf.org/html/rfc7519) der Branche zur sicheren Darstellung von Ansprüchen zwischen zwei Parteien.

### <a name="root-keys"></a>Stammschlüssel

Jedes Device Update-Gerät enthält einen Satz von Stammschlüsseln. Diese Schlüssel sind bilden den Vertrauensstamm für alle Signaturen von Device Update. Jede Signatur muss mit einem dieser Stammschlüssel verkettet werden, damit sie als legitim eingestuft wird.

Der Satz von Stammschlüsseln ändert sich im Laufe der Zeit, da Signaturschlüssel aus Sicherheitsgründen regelmäßig rotiert werden müssen. Folglich muss sich die Software des Device Update-Agents mit dem aktuellen Satz von Stammschlüsseln aktualisieren. 

### <a name="signatures"></a>Signaturen

Alle Signaturen erfolgen durch einen Signaturschlüssel (öffentlichen Schlüssel), der von einem der Stammschlüssel signiert wird. Die Signatur identifiziert den Stammschlüssel, der zum Signieren des Signaturschlüssels verwendet wurde. 

Ein Device Update-Agent muss Signaturen validieren, indem er zuerst überprüft, ob die Signatur des Signaturschlüssels (öffentlichen Schlüssels) ordnungsgemäß, gültig und von einem der genehmigten Stammschlüssel signiert ist. Nachdem der Signaturschlüssel erfolgreich überprüft wurde, kann die Signatur selbst mithilfe des jetzt vertrauenswürdigen öffentlichen Signaturschlüssels überprüft werden.

Signaturschlüssel werden in einem viel kürzeren Intervall als Stammschlüssel rotiert, sodass Nachrichten zu erwarten sind, die von verschiedenen unterschiedlichen Signaturschlüsseln signiert wurden. 

Die Sperrung eines Signaturschlüssels wird vom Device Update-Dienst verwaltet, sodass Benutzer nicht versuchen sollten, Signaturschlüssel zwischenzuspeichern. Verwenden Sie immer den Signaturschlüssel, der eine Signatur begleitet.

### <a name="receiving-updates"></a>Empfangen von Updates

Von einem Device Update-Agent empfangene Updateanforderungen enthalten ein signiertes Updatemanifest (UM-Dokument). Der Agent muss überprüfen, ob die Signatur des UM-Dokuments ordnungsgemäß und intakt ist. Dies erfolgt durch Überprüfung, ob der Signaturschlüssel der UM-Signatur durch einen ordnungsgemäßen Stammschlüssel signiert wurde. Anschließend überprüft der Agent die UM-Signatur anhand des Signaturschlüssels.

Nachdem die UM-Signatur überprüft wurde, kann sie vom Device Update-Agent als „Source of Truth“ und damit als vertrauenswürdig eingestuft werden. Alle weiteren Sicherheitsvertrauensstellungen werden von dieser Quelle abgeleitet. 

Das UM-Dokument enthält URLs und Dateihashes von Inhalten, die heruntergeladen und installiert werden sollen. Nachdem der Agent eine Updatebinärdatei heruntergeladen hat, muss er das Update anhand des im UM-Dokument gefundenen Dateihash überprüfen. Dadurch wird ein transitives Vertrauensstellungsmodell für die Downloadvalidierung bereitstellt. Es wird nicht nur sichergestellt, dass der Inhalt intakt (nicht geändert) ist, sondern auch bestätigt, dass der Download tatsächlich der beabsichtigte Download ist. 

### <a name="securing-the-device"></a>Sichern des Geräts

Es ist wichtig, sicherzustellen, dass die auf Device Update bezogenen Sicherheitsressourcen auf Ihrem Gerät ordnungsgemäß gesichert und geschützt werden. Ressourcen wie Stammschlüssel müssen vor Änderungen geschützt werden. Hierfür gibt es verschiedene Möglichkeiten, z. B. das Verwenden von Sicherheitsgeräten (TPM, SGX, HSM, andere Sicherheitsgeräte) oder sogar das Hardcodieren im Device Update-Agent. Letzteres erfordert, dass der Code des Device Update-Agents digital signiert wird und die Unterstützung der Codeintegrität des Systems zum Schutz vor böswilliger Änderung des Agent-Codes aktiviert ist.

Zusätzliche Sicherheitsmaßnahmen können gerechtfertigt sein, z. B. um sicherzustellen, dass die Übergabe von Komponente zu Komponente auf sichere Weise erfolgt. Beispielsweise das Registrieren eines bestimmten isolierten Kontos, um die verschiedenen Komponenten auszuführen. Und das Beschränken der netzwerkbasierten Kommunikation (z. B. Aufrufe der REST-API) ausschließlich auf localhost.

**[Nächster Schritt: Weitere Informationen dazu, wie Device Update Azure RBAC verwendet](.\device-update-control-access.md)**