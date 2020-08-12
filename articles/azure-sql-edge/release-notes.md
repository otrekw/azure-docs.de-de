---
title: Versionshinweise für Azure SQL Edge
description: Versionshinweise mit Informationen zu Neuerungen oder Änderungen in den Azure SQL Edge-Images
keywords: Versionshinweise für SQL Edge
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
ms.subservice: ''
author: VasiyaKrishnan
ms.author: vakrishn
ms.reviewer: sstein
ms.date: 07/27/2020
ms.openlocfilehash: 74e9772ada010d79e81ef36cae89ba586db73077
ms.sourcegitcommit: 1b2d1755b2bf85f97b27e8fbec2ffc2fcd345120
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/04/2020
ms.locfileid: "87553409"
---
# <a name="azure-sql-edge-release-notes"></a>Azure SQL Edge: Versionshinweise 

In diesem Artikel erhalten Sie Informationen zu Neuerungen und Änderungen für neue Azure SQL Edge-Builds.

## <a name="ctp-22"></a>CTP 2.2
### <a name="sql-engine-build-number---15020001546"></a>SQL-Engine-Buildnummer: 15.0.2000.1546
### <a name="fixes"></a>Fehlerbehebungen
1. Unterstützung für Container ohne Root-Berechtigung 
2. Unterstützung für Verbrauchs- und Diagnosedatensammlung 
3. T-SQL-Streamingupdates
   - Unterstützung für Unicode-Zeichen für Streamobjektnamen
   - Verbesserungen der Prozessbereinigung
   - Verbesserungen bei Protokollierung und Diagnose
4. Leistungsverbesserung bei der Datenerfassung

## <a name="ctp-21"></a>CTP 2.1 
### <a name="sql-engine-build-number---15020001545"></a>SQL-Engine-Buildnummer: 15.0.2000.1545
### <a name="fixes"></a>Fehlerbehebungen
1. Vorhersage (PREDICT) mit ONNX-Modellen korrigiert, um das CPUID-Problem in ARM zu behandeln 
2. Behandlung des Fehlerpfads beim Starten des TQL-Streamings verbessert 
3. Falscher Wert der Wasserzeichenverzögerung in Auftragsmetriken korrigiert, wenn keine Daten vorhanden sind 
4. Problem mit dem Ausgabeadapter behoben, wenn der Adapter über ein variables Schema zwischen Batches verfügt  

## <a name="ctp-20"></a>CTP 2.0 
### <a name="sql-engine-build-number---15020001401"></a>SQL-Engine-Buildnummer: 15.0.2000.1401
### <a name="whats-new"></a>Neuigkeiten
1.  Produktname wurde in „Azure SQL Edge“ geändert
1.  Funktion „Date_bucket“

    i.  Unterstützung der Typen „Date“, „Time“ und „DateTime“
3.  Vorhersage (PREDICT) mit ONNX
    
    i.  Parameter „RUNTIME“ für ONNX erforderlich 
    
4.  Unterstützung von TSQL-Streaming (eingeschränkte Vorschau) 
 
### <a name="known-issues"></a>Bekannte Probleme

1. <b>Problem:</b> Mögliche Fehler beim Anwenden von DACPAC beim Start aufgrund eines Zeitsteuerungsproblems.

    <b>Problemumgehung:</b> Wenn Sie SQL Server oder den Container neu starten, wird erneut versucht, DACPAC anzuwenden, und das Problem sollte behoben werden.
### <a name="request-support"></a>Anfordern von Support
1. Support kann auf der [Supportseite](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) angefordert werden.

4. Folgende Felder müssen ausgewählt sein: 
    * Problemtyp: Technisch 
    * Dienst: IoT Edge
    * Problemtyp: Mein Problem bezieht sich auf ein IoT Edge-Modul
    * Problemuntertyp: Azure SQL Edge

   ![Beispiel für in Supportticket](media/get-support/support-ticket.png)

## <a name="ctp-15"></a>CTP 1.5
### <a name="sql-engine-build-number---15020001331"></a>SQL-Engine-Buildnummer: 15.0.2000.1331
### <a name="whats-new"></a>Neuigkeiten
1. Funktion „Date_bucket“
    
    i. Unterstützung des Typs „DateTimeOffset“
2. Vorhersage (PREDICT) mit ONNX-Modellen

    i. nvarchar-Unterstützung
 
## <a name="ctp-14"></a>CTP 1.4
### <a name="sql-engine-build-number---15020001247"></a>SQL-Engine-Buildnummer: 15.0.2000.1247
### <a name="whats-new"></a>Neuigkeiten
1.  Vorhersage (PREDICT) mit ONNX-Modellen
 
    i.  varchar-Unterstützung
    
    ii. Migration zur ONNX-Runtimeversion 1.0 
2.  Featureunterstützung: Folgende Features sind aktiviert:

    i.   CDC-Unterstützung

    ii.  Verlaufstabelle mit Komprimierung

    iii. Höherer Skalierungsfaktor für Protokoll-Read-Ahead

    iv.  ES-Filterweitergabe für Batchmodus

    v.   Read-Ahead-Optimierungen
 
## <a name="ctp-13"></a>CTP 1.3
### <a name="sql-engine-build-number---15020001147"></a>SQL-Engine-Buildnummer: 15.0.2000.1147
### <a name="whats-new"></a>Neuigkeiten
1. Azure IoT-Portalbereitstellung 

    i.   Unterstützung der Bereitstellung von AMD64- und ARM-Images

    ii.  Unterstützung der Streamingauftragserstellung

    iii. DACPAC-Bereitstellung
2. Vorhersage (PREDICT) mit ONNX-Modellen

    i. Unterstützung numerischer Typen
3. Featureunterstützung: Folgende Features sind aktiviert:

    i.  Weitergabe von Aggregat an Columnstore-Scan

    ii. Karussellscans
4. Verringerung des Speicher- und Arbeitsspeicherbedarfs
