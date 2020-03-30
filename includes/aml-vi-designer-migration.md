---
title: include file
description: include file
services: machine-learning
ms.service: machine-learning
ms.custom: include file
ms.topic: include
author: xiaoharper
ms.author: zhanxia
ms.date: 10/18/2019
ms.openlocfilehash: e3cb977871af2e6cd7a59dd48505090dd29e8a76
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "75535714"
---
1. Melden Sie sich bei [Azure Machine Learning Studio](https://ml.azure.com) an.

1. Führen Sie ein Upgrade des Arbeitsbereichs auf die Enterprise Edition durch.

    Nach dem Upgrade werden alle Experimente der grafischen Benutzeroberfläche im Designer in Pipelineentwürfe konvertiert.
    
    > [!NOTE]
    > Sie müssen kein Upgrade auf die Enterprise Edition durchführen, um Webdienste der grafischen Benutzeroberfläche in Echtzeitendpunkte zu konvertieren.
    
1. Wechseln Sie zum Designerabschnitt des Arbeitsbereichs, um die Liste der Pipelineentwürfe anzuzeigen. 
    
    Konvertierte Webdienste finden Sie durch Navigation zu **Endpunkte** > **Echtzeitendpunkte**.

1. Wählen Sie einen Pipelineentwurf aus, um ihn zu öffnen.

    Wenn während der Konvertierung ein Fehler aufgetreten ist, wird eine Fehlermeldung mit Anweisungen zum Beheben des Problems angezeigt. 

### <a name="known-issues"></a>Bekannte Probleme

 Nachstehend sind bekannte Migrationsprobleme aufgelistet, die manuell behoben werden müssen:

- Module **Daten importieren** oder **Daten exportieren**
        
    Wenn Sie über ein Modul **Daten importieren** oder **Daten exportieren** im Experiment verfügen, müssen Sie die Datenquelle aktualisieren, um Datenspeicher zu verwenden. Informationen zum Erstellen eines Datenspeichers finden Sie unter [Informationen zum Zugriff auf Daten in Azure-Speicherdiensten](../articles/machine-learning/how-to-access-data.md). Die Informationen für Ihr Cloudspeicherkonto wurden der Einfachheit halber in den Kommentaren des Moduls **Daten importieren** oder **Daten exportieren** hinzugefügt. 
      