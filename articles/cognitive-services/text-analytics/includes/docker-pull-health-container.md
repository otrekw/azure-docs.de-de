---
title: Docker-Pullbefehl für den „Textanalyse für Gesundheit“-Container
titleSuffix: Azure Cognitive Services
description: Docker-Pullbefehl für „Textanalyse für Gesundheit“-Container
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 07/07/2020
ms.author: aahi
ms.openlocfilehash: b9452baf0301233a1cd6e05621a89d013d32c9e0
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90906025"
---
Füllen Sie das [Formular zum Anfordern von Cognitive Services-Containern](https://aka.ms/cognitivegate) aus, und übermitteln Sie es, um Zugriff auf den Container anzufordern.
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
