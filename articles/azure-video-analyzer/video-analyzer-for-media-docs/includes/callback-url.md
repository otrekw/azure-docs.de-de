---
author: Juliako
ms.topic: include
ms.date: 11/13/2020
ms.author: juliako
ms.openlocfilehash: db2f6fe6175b8ff4d6371cb7074fb8c83af51c14
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110385626"
---
Eine URL, die zum Benachrichtigen des Kunden über die folgenden Ereignisse (mithilfe einer POST-Anforderung) verwendet wird:

- Änderung des Indizierungszustands: 
    - Eigenschaften:    
    
        |Name|BESCHREIBUNG|
        |---|---|
        |id|Video-ID|
        |state|Videozustand|  
    - Beispiel: https:\//test.com/notifyme?projectName=MeinProjekt&id=1234abcd&state=Processed
- Im Video identifizierte Person:
  - Eigenschaften
    
      |Name|BESCHREIBUNG|
      |---|---|
      |id| Video-ID|
      |faceId|Die Gesichts-ID, die im Videoindex angezeigt wird|
      |knownPersonId|Die Personen-ID, die innerhalb eines Gesichtsmodells eindeutig ist|
      |personName|Der Name der Person|
        
    - Beispiel: https:\//test.com/notifyme?projectName=MeinProjekt&id=1234abcd&faceid=12&knownPersonId=CCA84350-89B7-4262-861C-3CAC796542A5&personName=Inigo_Montoya 
