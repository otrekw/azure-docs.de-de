---
title: Standardmäßige Größe des TEMP-Ordners zu klein für eine Rolle | Microsoft-Dokumentation
description: Eine Clouddienstrolle verfügt über begrenzten Speicherplatz für den Ordner TEMP. Dieser Artikel enthält einige Vorschläge dazu, wie Sie vermeiden, dass kein Speicherplatz mehr vorhanden ist.
services: cloud-services
documentationcenter: ''
author: simonxjx
manager: dcscontentpm
editor: ''
tags: top-support-issue
ms.assetid: 9f2af8dd-2012-4b36-9dd5-19bf6a67e47d
ms.service: cloud-services
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 06/15/2018
ms.author: v-six
ms.openlocfilehash: d18e174eac30bbcf87010531821a4017efb088ba
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2020
ms.locfileid: "92075109"
---
# <a name="default-temp-folder-size-is-too-small-on-a-cloud-service-webworker-role"></a>Standardmäßige Größe des TEMP-Ordners ist für eine Clouddienst-Web-/Workerrolle zu gering
Das standardmäßige temporäre Verzeichnis einer Clouddienst-Web- oder -Workerrolle hat eine maximale Größe von 100 MB. Es kann vorkommen, dass dieser Platz auch belegt wird. In diesem Artikel wird beschrieben, wie Sie verhindern, dass der Speicherplatz des temporären Verzeichnisses erschöpft ist.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-do-i-run-out-of-space"></a>Wie kann es passieren, dass kein Speicherplatz mehr übrig ist?
Die standardmäßigen Windows-Umgebungsvariablen TEMP und TMP stehen für Code zu Verfügung, der in Ihrer Anwendung ausgeführt wird. Sowohl TEMP als auch TMP zeigen auf ein einzelnes Verzeichnis mit einer maximalen Größe von 100 MB. Alle Daten, die in diesem Verzeichnis gespeichert werden, werden nicht über den Lebenszyklus des Clouddiensts beibehalten. Wenn die Rolleninstanzen in einem Clouddienst recycelt werden, wird das Verzeichnis bereinigt.

## <a name="suggestion-to-fix-the-problem"></a>Vorschläge zum Beheben des Problems
Implementieren Sie eine der folgenden Alternativen:

* Konfigurieren Sie eine lokale Speicherressource, und greifen Sie direkt darauf zu, anstatt TEMP oder TMP zu verwenden. Rufen Sie die [RoleEnvironment.GetLocalResource](/previous-versions/azure/reference/ee772845(v=azure.100)) -Methode auf, um aus Code, der in Ihrer Anwendung ausgeführt wird, auf eine lokale Speicherressource zuzugreifen.
* Konfigurieren Sie eine lokale Speicherressource, und lassen Sie die Verzeichnisse TEMP und TMP auf den Pfad der lokalen Speicherressource verweisen. Führen Sie diese Änderung in der [RoleEntryPoint.OnStart](/previous-versions/azure/reference/ee772851(v=azure.100)) -Methode durch.

Im folgenden Codebeispiel wird veranschaulicht, wie Sie die Zielverzeichnisse für TEMP und TMP über die OnStart-Methode ändern:

```csharp
using System;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override bool OnStart()
        {
            // The local resource declaration must have been added to the
            // service definition file for the role named WorkerRole1:
            //
            // <LocalResources>
            //    <LocalStorage name="CustomTempLocalStore"
            //                  cleanOnRoleRecycle="false"
            //                  sizeInMB="1024" />
            // </LocalResources>

            string customTempLocalResourcePath =
            RoleEnvironment.GetLocalResource("CustomTempLocalStore").RootPath;
            Environment.SetEnvironmentVariable("TMP", customTempLocalResourcePath);
            Environment.SetEnvironmentVariable("TEMP", customTempLocalResourcePath);

            // The rest of your startup code goes here…

            return base.OnStart();
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie einen Blog, der beschreibt, [wie Sie die Größe des temporären Ordners für die Azure-Webrolle ASP.NET erhöhen](/archive/blogs/kwill/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder).

Sehen Sie sich weitere [Artikel zur Problembehandlung](/visualstudio/azure/vs-azure-tools-debugging-cloud-services-overview) für Clouddienste an.

Erfahren Sie in der [Blogreihe von Kevin Williamson](/archive/blogs/kwill/windows-azure-paas-compute-diagnostics-data), wie Sie Probleme bei Clouddienstrollen mithilfe der Computerdiagnosedaten von Azure-PaaS beheben.