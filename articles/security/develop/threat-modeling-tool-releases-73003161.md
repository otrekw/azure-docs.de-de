---
title: Microsoft Threat Modeling Tool, Release 22.03.2020 – Azure
description: Dokumentation der Versionshinweise für das Threat Modeling Tool-Release 7.3.00316.1.
author: jegeib
ms.author: jegeib
ms.service: security
ms.topic: article
ms.date: 03/22/2020
ms.openlocfilehash: 39963e9dadca0ee0be7bb8ad0ebf824c287bfee0
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91317904"
---
# <a name="threat-modeling-tool-update-release-73003161---03222020"></a>Threat Modeling Tool, Updaterelease 7.3.00316.1 (22.03.2020)

Die Version 7.3.00316.1 für das Microsoft Threat Modeling Tool (TMT) wurde am 22. März 2020 veröffentlicht und umfasst die folgenden Änderungen:

- Verbesserungen der Barrierefreiheit
- Behebung von Programmfehlern
- Neues DiagramReader-Feature

## <a name="notable-bug-fixes"></a>Wichtige Programmfehlerbehebungen

### <a name="exporting-the-threat-list-to-csv"></a>Exportieren der Bedrohungsliste in CSV

Die Funktion für das Exportieren in eine CSV-Datei wählte inkonsistent Felder aus der Bedrohungsliste aus, die exportiert werden sollten. Nun werden alle Felder aus der Bedrohungsliste in die CSV-Datei exportiert. 

### <a name="ux-bugs"></a>UX-Fehler

- Die Hilfemenüs im primären Workflow (Erstellen/Öffnen/Analysieren) und der Vorlagen-Editor verfügen nun über konsistente Menüoptionen.
- Die Suchleiste im Bereich für Schablonen verfügt jetzt über einen Standardcursor, und es wurden entsprechende Bezeichnungen hinzugefügt.

## <a name="new-features"></a>Neue Funktionen

### <a name="diagramreader-feature-has-been-added"></a>DiagramReader-Feature hinzugefügt

Im Hauptmenü wurde für geöffnete Modelle ein neues DiagramReader-Feature hinzugefügt. Mit diesem Feature wird die grafische Darstellung des Modells in eine textbasierte Darstellung konvertiert. 

## <a name="system-requirements"></a>Systemanforderungen

- Unterstützte Betriebssysteme:
  - [Windows 10 Anniversary Update](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97) oder höher
- Erforderliche .NET-Version:
  - [.NET 4.7.1](https://go.microsoft.com/fwlink/?LinkId=863262)
- Zusätzliche Anforderungen:
  - Es ist eine Internetverbindung erforderlich, um Updates für das Tool sowie Vorlagen zu erhalten.

## <a name="documentation-and-feedback"></a>Dokumentation und Feedback

- Dokumentation für das Threat Modeling Tool befindet sich auf [docs.microsoft.com](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool) und umfasst Informationen [zur Verwendung des Tools](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool-getting-started).

## <a name="next-steps"></a>Nächste Schritte

Laden Sie die aktuelle Version des [Microsoft Threat Modeling Tools](https://aka.ms/threatmodelingtool) herunter.
