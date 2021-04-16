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
ms.date: 03/25/2021
ms.author: aahi
ms.openlocfilehash: 8595af7f46b63f9991d6a02279ccad76afb38311
ms.sourcegitcommit: 99fc6ced979d780f773d73ec01bf651d18e89b93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106089707"
---
Füllen Sie das [Formular zum Anfordern von Cognitive Services](https://aka.ms/csgate) aus, und übermitteln Sie es, um Zugriff auf Textanalyse for Health (Public Preview) anzufordern.  Diese Anwendung gilt sowohl für den Container als auch für die öffentliche Vorschau der gehosteten Web-API.
Im Formular müssen Sie Informationen über Sie selbst, Ihr Unternehmen und das Benutzerszenario eintragen, für das Sie den Container verwenden möchten. Nach der Übermittlung des Formulars überprüft das Azure Cognitive Services-Team das Formular, um sicherzustellen, dass Sie die Kriterien für den Zugriff auf die private Containerregistrierung erfüllen.

> [!IMPORTANT]
> * Im Formular müssen Sie eine E-Mail-Adresse angeben, die einer Azure-Abonnement-ID zugeordnet ist.
> * Die Textanalyse-Ressource (Abrechnungsendpunkt und apikey), die Sie zum Ausführen des Containers verwenden, muss mit der genehmigten Azure-Abonnement-ID erstellt worden sein. 
> * Überprüfen Sie Ihre E-Mail-Adresse (Posteingang und Junk-Ordner) auf Updates zum Status Ihrer Anwendung von Microsoft.
> * Diese Container-URL wurde geändert, siehe folgende Beispiele. Der Container kann ab dem 26. April 2021 nicht mehr heruntergeladen werden `containerpreview.azurecr.io`.


Verwenden Sie nach der Genehmigung den [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) Befehl, um dieses Containerimage aus der öffentlichen Microsoft-Containerregistrierung herunterzuladen.

```
docker pull mcr.microsoft.com/azure-cognitive-services/textanalytics/healthcare:latest
```
