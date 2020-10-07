---
title: Docker-Pullbefehl für den „Textanalyse für Gesundheit“-Container
titleSuffix: Azure Cognitive Services
description: Docker-Pullbefehl für „Textanalyse für Gesundheit“-Container
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 07/07/2020
ms.author: aahi
ms.openlocfilehash: 519625f6468372ec7ace523dae7648212f4f3203
ms.sourcegitcommit: ef69245ca06aa16775d4232b790b142b53a0c248
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2020
ms.locfileid: "91779850"
---
Füllen Sie das [Formular zum Anfordern von Cognitive Services-Containern](https://aka.ms/csgate) aus, und übermitteln Sie es, um Zugriff auf den Container anzufordern.
Im Formular müssen Sie Informationen über Sie selbst, Ihr Unternehmen und das Benutzerszenario eintragen, für das Sie den Container verwenden möchten. Nach der Übermittlung des Formulars überprüft das Azure Cognitive Services-Team das Formular, um sicherzustellen, dass Sie die Kriterien für den Zugriff auf die private Containerregistrierung erfüllen.

> [!IMPORTANT]
> * Im Formular müssen Sie eine E-Mail-Adresse angeben, die einer Azure-Abonnement-ID zugeordnet ist.
> * Die Azure-Ressource, die Sie zum Ausführen des Containers verwenden, muss mit der genehmigten Azure-Abonnement-ID erstellt worden sein. 
> * Überprüfen Sie Ihre E-Mail-Adresse (Posteingang und Junk-Ordner) auf Updates zum Status Ihrer Anwendung von Microsoft.

Stellen Sie mit dem Docker-Anmeldebefehl mit Anmeldeinformationen, die in ihrer Onboarding-E-Mail bereitgestellt werden, eine Verbindung mit unserer privaten Containerregistrierung für Cognitive Services-Container her.


```Docker
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Laden Sie mit dem Befehl [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) dieses Containerimage aus unserer privaten Containerregistrierung herunter.

```
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-healthcare:latest
```
