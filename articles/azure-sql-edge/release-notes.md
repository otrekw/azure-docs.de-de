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
ms.date: 06/05/2020
ms.openlocfilehash: d5d60db2cc0c35b908454be3f00716db6d2da945
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85361641"
---
# <a name="azure-sql-edge-release-notes"></a>Azure SQL Edge: Versionshinweise 

In diesem Artikel erhalten Sie Informationen zu Neuerungen und Änderungen für neue Azure SQL Edge-Builds.

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
