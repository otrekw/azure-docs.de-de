---
title: Microsoft Threat Modeling Tool, Release vom 29.07.2020 – Azure
description: Dokumentation der Versionshinweise für das Threat Modeling Tool
author: jegeib
ms.author: jegeib
ms.service: security
ms.topic: article
ms.date: 07/29/2020
ms.openlocfilehash: 47ffe753d661bb8c9a181148ea235b2a0eb54400
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87839825"
---
# <a name="threat-modeling-tool-update-release-73007291---07292020"></a>Threat Modeling Tool, Updaterelease 7.3.00729.1 – 29.07.2020

Version 7.3.00729.1 des Microsoft Threat Modeling Tools (TMT) wurde am 29. Juli 2020 veröffentlicht und umfasst die folgenden Änderungen:

- Behebung von Programmfehlern
 
## <a name="known-issues"></a>Bekannte Probleme

### <a name="errors-related-to-tmt7application-file-deserialization"></a>Fehler im Zusammenhang mit der Deserialisierung von TMT7.application-Dateien

#### <a name="issue"></a>Problem

Einige Kunden haben berichtet, dass beim Herunterladen des Threat Modeling Tools folgende Fehlermeldung angezeigt wird:

```
The threat model file '$PATH\TMT7.application' could not be deserialized. File is not an actual threat model or the threat model may be corrupted.
```

Dieser Fehler tritt auf, weil einige Browser die ClickOnce-Installation nicht nativ unterstützen. In diesen Fällen wird die ClickOnce-Anwendungsdatei auf die Festplatte des Benutzers heruntergeladen.

#### <a name="workaround"></a>Problemumgehung

Dieser Fehler tritt weiterhin auf, wenn das Threat Modeling Tool durch Doppelklicken auf die Datei „TMT7.application“ gestartet wird. Nachdem der Fehler umgangen wurde, funktioniert das Tool jedoch normal. Anstatt das Threat Modeling Tool durch Doppelklicken auf die Datei „TMT7.application“ zu starten, sollten Benutzer Tastenkombinationen verwenden, die während der Installation im Windows-Menü erstellt wurden.

## <a name="system-requirements"></a>Systemanforderungen

- Unterstützte Betriebssysteme
  - [Windows 10 Anniversary Update](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97) oder höher
- Erforderliche .NET-Version
  - [.NET 4.7.1](https://go.microsoft.com/fwlink/?LinkId=863262)
- Zusätzliche Anforderungen
  - Eine Internetverbindung ist erforderlich, um Updates für das Tool sowie Vorlagen zu erhalten.

## <a name="documentation-and-feedback"></a>Dokumentation und Feedback

- Dokumentation für das Threat Modeling Tool befindet sich auf [docs.microsoft.com](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool) und umfasst Informationen [zur Verwendung des Tools](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool-getting-started).

## <a name="next-steps"></a>Nächste Schritte

Laden Sie die aktuelle Version des [Microsoft Threat Modeling Tools](https://aka.ms/threatmodelingtool) herunter.
