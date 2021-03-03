---
title: Clientfehlercodes für Device Update for IoT Hub | Microsoft-Dokumentation
description: Dieses Dokument enthält eine Tabelle mit Clientfehlercodes für verschiedene Device Update-Komponenten.
author: lichris
ms.author: lichris
ms.date: 2/18/2021
ms.topic: reference
ms.service: iot-hub-device-update
ms.openlocfilehash: 5251d0cb09e40305d1efd89c31d3af0fa36ad385
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101678459"
---
# <a name="device-update-for-iot-hub-error-codes"></a>Fehlercodes für Device Update for IoT Hub

Dieses Dokument enthält eine Tabelle mit Fehlercodes für verschiedene Device Update-Komponenten. Es soll als Referenz für Benutzer dienen, die versuchen möchten, ihre eigenen Fehlercodes zu analysieren, um Probleme zu diagnostizieren und zu beheben.

Es gibt zwei primäre clientseitige Komponenten, die möglicherweise Fehlercodes auslösen: den Device Update-Agent und den Übermittlungsoptimierungs-Agent.

## <a name="device-update-agent"></a>Device Update-Agent

### <a name="resultcode-and-extendedresultcode"></a>ResultCode und ExtendedResultCode

Die PNP-Schnittstelle von Device Update for IoT Hub Core meldet `ResultCode` und `ExtendedResultCode`, die zur Diagnose von Fehlern verwendet werden können. [Erfahren Sie mehr](device-update-plug-and-play.md) über die Device Update Core PNP-Schnittstelle.

#### <a name="resultcode"></a>ResultCode

`ResultCode` ist ein allgemeiner Statuscode und folgt der Konvention für HTTP-Statuscodes.
[Weitere Informationen](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) zu HTTP-Statuscodes.

#### <a name="extendedresultcode"></a>ExtendedResultCode

`ExtendedResultCode` ist ein Integerwert mit codierten Fehlerinformationen.

Wahrscheinlich wird `ExtendedResultCode` als ganze Zahl mit Vorzeichen in der PNP-Schnittstelle angezeigt. Konvertieren Sie die ganze Zahl mit Vorzeichen in ein Hexadezimalformat ohne Vorzeichen, um `ExtendedResultCode` zu decodieren. Nur die ersten 4 Bytes von `ExtendedResultCode` werden verwendet und weisen das Format `F` `FFFFFFF` auf, wobei der erste Teil den **Einrichtungscode** und die restlichen Bits den **Fehlercode** darstellen.

**Einrichtungscodes**

| Einrichtungscode     | BESCHREIBUNG  |
|-------------------|--------------|
| D                 | Vom DO SDK ausgelöster Fehler|
| E                 | Fehlercode: errno |


Beispiel:

`ExtendedResultCode` ist gleich `-536870781`.

Die Hexadezimaldarstellung ohne Vorzeichen von `-536870781` ist `FFFFFFFF E0000083`.

| Ignorieren    | Einrichtungscode  | Fehlercode   |
|-----------|----------------|--------------|
| FFFFFFFF  | E              | 0000083      |

`0x83` in Hexadezimaldarstellung ist `131` in Dezimaldarstellung: errno-Wert für `ENOLCK`.

## <a name="delivery-optimization-agent"></a>Übermittlungsoptimierungs-Agent
In der folgenden Tabelle sind die Fehlercodes aufgeführt, die sich auf die Komponente zur Übermittlungsoptimierung (DO) des Device Update-Clients beziehen. Die DO-Komponente ist für das Herunterladen von Updateinhalten auf das IoT-Gerät verantwortlich.

Der DO-Fehlercode kann durch Untersuchen der Ausnahmen abgerufen werden, die als Reaktion auf einen API-Befehl ausgelöst werden.

| Fehlercode  | Zeichenfolgenfehler                       | type                 | BESCHREIBUNG |
|-------------|------------------------------------|----------------------|-------------|
| 0x80D01001L | DO_E_NO_SERVICE                    | –                  | Die Übermittlungsoptimierung konnte den Dienst nicht bereitstellen. |
| 0x80D02002L | DO_E_DOWNLOAD_NO_PROGRESS          | Auftrag herunterladen         | Beim Herunterladen einer Datei ist innerhalb des definierten Zeitraums kein Fortschritt aufgetreten. |
| 0x80D02003L | DO_E_JOB_NOT_FOUND                 | Auftrag herunterladen         | Auftrag wurde nicht gefunden. |
| 0x80D02005L | DO_E_NO_DOWNLOADS                  | Auftrag herunterladen         | Zurzeit sind keine Downloads vorhanden. |
| 0x80D0200CL | DO_E_JOB_TOO_OLD                   | Auftrag herunterladen         | Der Auftrag wurde nicht abgeschlossen oder vor dem Erreichen des maximalen Altersschwellenwerts abgebrochen. |
| 0x80D02011L | DO_E_UNKNOWN_PROPERTY_ID           | Auftrag herunterladen         | SetProperty() oder GetProperty() wurde mit einer unbekannten Eigenschaften-ID aufgerufen. |
| 0x80D02012L | DO_E_READ_ONLY_PROPERTY            | Auftrag herunterladen         | SetProperty() kann nicht für eine schreibgeschützte Eigenschaft aufgerufen werden. |
| 0x80D02013L | DO_E_INVALID_STATE                 | Auftrag herunterladen         | Die angeforderte Aktion ist im aktuellen Auftragszustand unzulässig. Der Auftrag wurde möglicherweise abgebrochen, oder die Übertragung wurde abgeschlossen. Er befindet sich jetzt in einem schreibgeschützten Zustand. |
| 0x80D02018L | DO_E_FILE_DOWNLOADSINK_UNSPECIFIED | Auftrag herunterladen         | Ein Download kann nicht gestartet werden, da keine Downloadsenke (lokale Datei oder Streamschnittstelle) angegeben wurde. |
| 0x80D02200L | DO_E_DOWNLOAD_NO_URI               | IDODownload-Schnittstelle| Der Download wurde ohne Angabe eines URI gestartet. |
| 0x80D03805L | DO_E_BLOCKED_BY_NO_NETWORK         | Vorübergehende Schwierigkeiten | Der Download wurde aufgrund des Verlusts der Netzwerkkonnektivität angehalten. |
| 0x80D05001L | DO_E_HTTP_BLOCKSIZE_MISMATCH       | HTTP                 | Der HTTP-Server hat eine Antwort mit einer Datengröße zurückgegeben, die nicht der Anforderung entsprach. |
| 0x80D05002L | DO_E_HTTP_CERT_VALIDATION          | HTTP                 | Fehler beim Überprüfen des HTTP-Serverzertifikats. |
| 0x80D05010L | DO_E_INVALID_RANGE                 | HTTP                 | Der angegebene Bytebereich ist ungültig. |
| 0x80D05011L | DO_E_INSUFFICIENT_RANGE_SUPPORT    | HTTP                 | Der Server unterstützt das erforderliche HTTP-Protokoll nicht. Übermittlungsoptimierung (DO) erfordert, dass der Server den Protokollheader „Range“ unterstützt. |
| 0x80D05012L | DO_E_OVERLAPPING_RANGES            | HTTP                 | Die Liste der Bytebereiche enthält einige überlappende Bereiche, die nicht unterstützt werden. |
## <a name="device-update-content-service"></a>Device Update-Inhaltsdienst
In der folgenden Tabelle werden Fehlercodes aufgeführt, die sich auf die Inhaltsdienstkomponente des Device Update-Diensts beziehen. Die Inhaltsdienstkomponente ist für die Verarbeitung des Imports von Updateinhalten zuständig.

| Fehlercode                    | Zeichenfolgenfehler                                                               | Nächste Schritte                         |
|-------------------------------|----------------------------------------------------------------------------|------------------------------------|
| UpdateAlreadyExists         | Ein Update mit der gleichen Identität ist bereits vorhanden.                              | Stellen Sie sicher, dass Sie ein Update importieren, das nicht bereits in diese Instanz von Device Update for IoT Hub importiert wurde. |
| DuplicateContentImport      | Identischer Inhalt wurde gleichzeitig mehrmals importiert.                  | Siehe UpdateAlreadyExists. |
| CannotProcessImportManifest | Fehler beim Verarbeiten des Importmanifests.                                          | Informationen zur ordnungsgemäßen Formatierung des Importmanifests finden Sie unter [Importkonzepte](./import-concepts.md) und in der Dokumentation zum [Importieren des Updates](./import-update.md). |
| CannotDownload              | Das Importmanifest kann nicht heruntergeladen werden.                                           | Stellen Sie sicher, dass die URL für die Importmanifestdatei noch gültig ist. |
| CannotParse                 | Das Importmanifest kann nicht analysiert werden.                                              | Überprüfen Sie das Importmanifest anhand des Schemas auf Genauigkeit, das in der Dokumentation zum [Importieren des Updates](./import-update.md) definiert ist. |
| UnsupportedVersion          | Die Schemaversion des Importmanifests wird nicht unterstützt.                           | Stellen Sie sicher, dass das Importmanifest das neueste Schema verwendet, das in der Dokumentation zum [Importieren des Updates](./import-update.md) definiert ist. |
| UpdateLimitExceeded         | Fehler beim Importieren des Updates aufgrund eines überschrittenen Grenzwerts.                              | Sie haben eine Beschränkung für die Anzahl der unterschiedlichen Anbieter, Namen oder Versionen erreicht, die in Ihrer Instanz von Device Update for IoT Hub zulässig sind. Löschen Sie einige Updates aus der Instanz, und versuchen Sie es noch mal. |
| UpdateProvider              | Ein neuer Updateanbieter kann nicht importiert werden.                                       | Sie haben eine Beschränkung für die Anzahl der unterschiedlichen __Anbieter__ erreicht, die in Ihrer Instanz von Device Update for IoT Hub zulässig sind. Löschen Sie einige Updates aus der Instanz, und versuchen Sie es noch mal. |
| UpdateName                  | Für den angegebenen Anbieter kann kein neuer Updatename importiert werden.                | Sie haben eine Beschränkung für die Anzahl der unterschiedlichen __Namen__ erreicht, die unter einem Anbieter in Ihrer Instanz von Device Update for IoT Hub zulässig sind. Löschen Sie einige Updates aus der Instanz, und versuchen Sie es noch mal. |
| UpdateVersion               | Für den angegebenen Anbieter und Namen kann keine neue Updateversion importiert werden.    | Sie haben eine Beschränkung für die Anzahl der unterschiedlichen __Versionen__ erreicht, die unter einem Anbieter und Namen in Ihrer Instanz von Device Update for IoT Hub zulässig sind. Löschen Sie einige Updates mit diesem Namen aus Ihrer Instanz, und versuchen Sie es noch mal. |
| UpdateProviderCompatibility | Ein zusätzlicher Updateanbieter mit der angegebenen Kompatibilität kann nicht importiert werden. | Beachten Sie beim Definieren von Kompatibilitätseigenschaften für Gerätehersteller und Gerätemodell in einem Importmanifest, dass Device Update for IoT Hub eine einzige Kombination aus Anbieter und Name für einen bestimmten Hersteller und ein Modell unterstützt. Wenn Sie also versuchen, die gleichen Hersteller-/Modellkompatibilitätseigenschaften mit mehr als einer Anbieter/Name-Kombination zu verwenden, werden diese Fehler angezeigt. Um dies zu beheben, stellen Sie sicher, dass alle Updates für ein bestimmtes Gerät (wie durch Hersteller/Modell definiert) denselben Anbieter und Namen verwenden. Es ist zwar nicht erforderlich, aber der Einfachheit halber sollten Sie erwägen, den Anbieter mit dem Hersteller und den Namen mit dem Modell gleichzusetzen. |
| UpdateNameCompatibility     | Ein zusätzlicher Updatename mit der angegebenen Kompatibilität kann nicht importiert werden.     | Siehe UpdateProviderCompatibility.ContentLimitNamespaceCompatibility. |
| UpdateVersionCompatibility  | Eine zusätzliche Updateversion mit der angegebenen Kompatibilität kann nicht importiert werden.  | Siehe UpdateProviderCompatibility.ContentLimitNamespaceCompatibility. |
| CannotProcessUpdateFile     | Fehler beim Verarbeiten der Quelldatei.                                              |                                    |
| ContentFileCannotDownload   | Die Quelldatei kann nicht heruntergeladen werden.                                               | Stellen Sie sicher, dass die URL für die Updatedatei(en) noch gültig ist. |

**[Nächster Schritt: Beheben von Problemen mit Device Update](.\troubleshoot-device-update.md)**