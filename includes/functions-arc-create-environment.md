---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 05/12/2021
ms.author: cephalin
ms.openlocfilehash: 43eda9f0f5cdb108c2949205d72433c9d7176571
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110371861"
---
## <a name="create-an-app-service-kubernetes-environment"></a>Erstellen der App Service Kubernetes-Umgebung

Bevor Sie beginnen, müssen Sie [eine App Service Kubernetes-Umgebung](../articles/app-service/manage-create-arc-environment.md) für einen Azure Arc Kubernetes-Cluster erstellen. 

> [!NOTE] 
> Notieren Sie sich beim Erstellen der Umgebung sowohl den benutzerdefinierten Standortnamen als auch den Namen der Ressourcengruppe, die den benutzerdefinierten Speicherort enthält. Sie können diese Namen verwenden, um die benutzerdefinierte Standort-ID zu finden, die Sie beim Erstellen Ihrer Funktions-App in der Umgebung benötigen. 
>
> Wenn Sie die Umgebung nicht erstellt haben, wenden Sie sich an Ihren Cluster-Administrator.