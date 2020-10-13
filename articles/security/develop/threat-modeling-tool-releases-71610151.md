---
title: Microsoft Threat Modeling Tool, Release 16.10.2019 – Azure
description: Dokumentation der Versionshinweise für das Threat Modeling Tool-Release 7.1.61015.1
author: jegeib
ms.author: jegeib
ms.service: security
ms.topic: article
ms.date: 10/16/2019
ms.openlocfilehash: e45d1347993611130d1bb5036ad4638210c15517
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91317938"
---
# <a name="threat-modeling-tool-update-release-71610151---10162019"></a>Threat Modeling Tool, Updaterelease 7.1.61015.1: 16.10.2019

Version 7.1.61015.1 für das Microsoft Threat Modeling Tool (TMT) wurde am 16. Oktober 2019 veröffentlicht und umfasst die folgenden Änderungen:

- Verbesserungen der Barrierefreiheit
- Behebung von Programmfehlern
- Neue Schablonen für Azure Logic Apps und Azure Data Explorer

## <a name="notable-bug-fixes"></a>Wichtige Programmfehlerbehebungen

### <a name="improved-backward-compatibility-with-files-created-in-threat-modeling-tool-2016"></a>Verbesserte Abwärtskompatibilität für Dateien, die in „Threat Modeling Tool 2016“ erstellt wurden

Einige Fehler beim Öffnen oder Anzeigen von Bedrohungsmodelldateien, die in „Threat Modeling Tool 2016“ erstellt wurden, wurden behoben.

## <a name="feature-enhancements"></a>Featureverbesserungen

### <a name="new-stencils-for-azure-logic-apps-and-azure-data-explorer"></a>Neue Schablonen für Azure Logic Apps und Azure Data Explorer

Die Azure-Schablone wurde mit neuen Schablonen für Azure Logic Apps und Azure Data Explorer und den entsprechenden Bedrohungen und Entschärfungen ergänzt.

![Azure Logic Apps und Azure Data Explorer-Schablonen](./media/threat-modeling-tool-releases-71610151/tmt-logic-apps.png)

## <a name="known-issues"></a>Bekannte Probleme

### <a name="errors-related-to-priority-values-outside-of-the-expected-ranges"></a>Fehler im Zusammenhang mit Prioritätswerten außerhalb der erwarteten Bereiche

Einige Kunden haben berichtet, dass beim Öffnen von Dateien, die in „Threat Modeling Tool 2016“ oder mit benutzerdefinierten Vorlagen erstellt wurden, folgende Fehlermeldung angezeigt wird:

```output
System.InvalidOperationException: Invalid Priority value. Accepted values are [0..4] and 'High', 'Medium', 'Low' at ThreatModeling.Model.Threat.get_Priority()

System.ArgumentOutOfRangeException: Accepted values are 'High', 'Medium', and 'Low' Parameter name: value Actual value was 5.6. at ThreatModeling.Model.Threat.set_Priority(String value)
```

Dieses Issue wird untersucht.

## <a name="system-requirements"></a>Systemanforderungen

- Unterstützte Betriebssysteme
  - [Windows 10 Anniversary Update](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97) oder höher
- Erforderliche .NET-Version
  - [.NET 4.7.1](https://go.microsoft.com/fwlink/?LinkId=863262) oder höher
- Zusätzliche Anforderungen
  - Eine Internetverbindung ist erforderlich, um Updates für das Tool sowie Vorlagen zu erhalten.

## <a name="documentation-and-feedback"></a>Dokumentation und Feedback

- Dokumentation für das Threat Modeling Tool befindet sich auf [docs.microsoft.com](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool) und umfasst Informationen [zur Verwendung des Tools](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool-getting-started).

## <a name="next-steps"></a>Nächste Schritte

Laden Sie die aktuelle Version des [Microsoft Threat Modeling Tools](https://aka.ms/threatmodelingtool) herunter.
