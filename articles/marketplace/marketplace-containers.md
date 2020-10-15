---
title: Leitfaden für die Veröffentlichung von Containerangeboten im Azure Marketplace
description: In diesem Artikel werden die Anforderungen für die Veröffentlichung von Containerangeboten im Azure Marketplace beschrieben.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: keferna
ms.author: keferna
ms.date: 09/04/2020
ms.openlocfilehash: c52fabcfc2ff22df2de6dd93f2543d625310baef
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89484340"
---
# <a name="publishing-guide-for-container-offers"></a>Leitfaden für die Veröffentlichung von Containerangeboten

Containerangebote erleichtern Ihnen die Veröffentlichung Ihres Containerimages im Azure Marketplace. Informieren Sie sich in diesem Handbuch über die Anforderungen für dieses Angebot. 

Containerangebote sind Transaktionsangebote, die über den Azure Marketplace bereitgestellt und abgerechnet werden. Den Benutzern wird die Listenoption „Jetzt kaufen“ angezeigt.

Verwenden Sie den Angebotstyp *Container*, wenn Ihre Lösung ein Docker-Container-Image ist, das als Kubernetes-basierte Azure Container Service-Instanz eingerichtet wird. 

> [!NOTE]
> Beispiele für Kubernetes-basierte Azure Container Service-Instanzen sind Azure Kubernetes Service oder Azure Container Instances, die Wahl der Azure-Kunden für eine Kubernetes-basierte Containerruntime.  

Microsoft unterstützt derzeit kostenlose und Bring-Your-Own-License-Lizenzierungsmodelle (BYOL).

## <a name="container-offer-requirements"></a>Anforderungen für Containerangebote

| Anforderung | Details |  
|:--- |:--- |  
| Abrechnung und Messung | Es wird entweder das kostenlose oder BYOL-Abrechnungsmodell unterstützt.<br><br> |  
| Auf Grundlage einer Dockerfile erstelltes Image | Containerimages müssen auf der Docker-Imagespezifikation basieren und auf der Grundlage einer Dockerfile-Datei erstellt werden.<br> <br>Weitere Informationen zum Entwickeln von Docker-Images finden Sie im Abschnitt zur Verwendung in der [Dockerfile-Referenz](https://docs.docker.com/engine/reference/builder/#usage).<br><br> |  
| Hosten in einem Azure Container Registry-Repository | Containerimages müssen in einem Azure Container Registry-Repository gehostet werden.<br> <br>Weitere Informationen zum Arbeiten mit Azure Container Registry finden Sie unter [Schnellstart: Erstellen einer privaten Containerregistrierung über das Azure-Portal](../container-registry/container-registry-get-started-portal.md).<br><br> |  
| Imagemarkierung | Containerimages müssen mindestens ein Tag enthalten (maximale Taganzahl: 16).<br><br>Weitere Informationen zum Markieren eines Images finden Sie auf der Seite `docker tag` auf der Website zur [Docker-Dokumentation](https://docs.docker.com/engine/reference/commandline/tag).<br><br> |  

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich (falls noch nicht geschehen), wie Sie [Ihr Cloudgeschäft mit dem Azure Marketplace ausweiten](https://azuremarketplace.microsoft.com/sell).

So registrieren Sie sich in Partner Center und beginnen mit der Arbeit

- [Melden Sie sich bei Partner Center an](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership), um Ihr Angebot zu erstellen oder abzuschließen.
- Weitere Informationen finden Sie unter [Erstellen eines Azure-Containerangebots](./partner-center-portal/create-azure-container-offer.md).
