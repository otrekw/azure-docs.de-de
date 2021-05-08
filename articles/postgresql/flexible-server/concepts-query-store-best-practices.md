---
title: Bewährte Methoden für den Abfragespeicher in Azure Database for PostgreSQL – Flex Server
description: Dieser Artikel beschreibt bewährte Methoden für den Abfragespeicher in Azure Database for PostgreSQL – Flex Server.
author: ssen-msft
ms.author: ssen
ms.service: postgresql
ms.topic: conceptual
ms.date: 04/01/2021
ms.openlocfilehash: 9ee2dc4b3e8ea6a9f892adbc378e8e13b8bd8160
ms.sourcegitcommit: 49b2069d9bcee4ee7dd77b9f1791588fe2a23937
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2021
ms.locfileid: "107558691"
---
# <a name="best-practices-for-query-store---flexible-server"></a>Bewährte Methoden für den Abfragespeicher – Flexible Server

**Gilt für:** Azure Database for PostgreSQL – Flex Server-Versionen 11, 12

Dieser Artikel beschreibt bewährte Methoden für die Verwendung des Abfragespeichers in Azure Database for PostgreSQL.

## <a name="set-the-optimal-query-capture-mode"></a>Festlegen des optimalen Abfrageerfassungsmodus
Lassen Sie den Abfragespeicher die Daten erfassen, die für Sie wichtig sind. 

|**pg_qs.query_capture_mode** | **Szenario**|
|---|---|
|_Alle_  |Analysieren Sie Ihre Workload sorgfältig im Hinblick auf alle Abfragen und deren Ausführungshäufigkeit und andere Statistiken. Identifizieren Sie neue Abfragen in Ihrer Workload. Erkennen Sie, ob Ad-hoc-Abfragen verwendet werden, um Möglichkeiten für Benutzer oder eine automatische Parametrisierung zu identifizieren. Bei _All_ sind die Kosten für den Ressourcenverbrauch höher. |
|_Top_  |Konzentrieren Sie sich auf die wichtigsten Abfragen – die von Clients ausgestellt wurden.
|_None_ |Wird „None“ festgelegt, erfasst der Abfragespeicher keine neuen Abfragen. Sie haben bereits einen Abfragesatz und ein Zeitfenster erfasst, das Sie untersuchen möchten, und Sie möchten die nun Ablenkungen beseitigen, die ggf. durch andere Abfragen entstehen. _None_ ist für Testzwecke sowie für Benchmarkingumgebungen geeignet. _None_ sollte mit Vorsicht verwendet werden, da Sie unter Umständen keine Möglichkeit haben, wichtige neue Abfragen nachzuverfolgen und zu optimieren. |


> [!NOTE] 
> **pg_qs.query_capture_mode** ersetzt **pgms_wait_sampling.query_capture_mode**. Wenn für pg_qs.query_capture_mode _None_ festgelegt ist, hat die Einstellung pgms_wait_sampling.query_capture_mode keine Auswirkungen. 


## <a name="keep-the-data-you-need"></a>Beibehalten der erforderlichen Daten
Der Parameter **pg_qs.retention_period_in_days** gibt den Datenaufbewahrungszeitraum für den Abfragespeicher in Tagen an. Ältere Abfrage- und Statistikdaten werden gelöscht. Standardmäßig ist der Abfragespeicher so konfiguriert, dass die Daten sieben Tage lang aufbewahrt werden. Speichern Sie keine historischen Daten, wenn Sie nicht beabsichtigen, diese zu verwenden. Legen Sie hierfür ggf. einen höheren Wert fest, wenn Sie die Daten länger aufbewahren möchten.


## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie Sie Parameter mithilfe des [Azure-Portals](howto-configure-server-parameters-using-portal.md) oder der [Azure-Befehlszeilenschnittstelle](howto-configure-server-parameters-using-cli.md) abrufen oder festlegen.
