---
author: craigshoemaker
ms.service: static-web-apps
ms.topic: include
ms.date: 03/25/2021
ms.author: cshoe
ms.openlocfilehash: 806a30214e0307b8fa101c0de51ed2f16622d433
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105543555"
---
| Eigenschaft | BESCHREIBUNG | Beispiel | Erforderlich |
|---|---|---|---|
| `app_location` | Speicherort Ihres Anwendungscodes. | Geben Sie `/` ein, wenn sich Ihr Quellcode für die Anwendung im Stammverzeichnis des Repositorys befindet, oder `/app`, wenn Ihr Anwendungscode in einem Verzeichnis mit dem Namen `app` enthalten ist. | Ja |
| `api_location` | Speicherort Ihres Azure Functions-Codes. | Geben Sie `/api` ein, wenn sich Ihr App-Code in einem Ordner mit dem Namen `api` befindet. Wenn im Ordner keine Azure Functions-App erkannt wird, tritt für den Buildvorgang kein Fehler auf. Im Workflow wird angenommen, dass Sie keine API benötigen. | Nein |
| `output_location` | Speicherort des Verzeichnisses für die Buildausgabe relativ zu `app_location`. | Wenn sich Ihr Quellcode für die Anwendung unter `/app` befindet und vom Buildskript Dateien im Ordner `/app/build` ausgegeben werden, legen Sie `build` als Wert für `output_location` fest. | Nein |