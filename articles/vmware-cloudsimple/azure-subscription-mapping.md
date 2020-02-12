---
title: Erstellen von Ressourcenpools mit Azure-Abonnementzuordnungen
description: Beschreibt das Erstellen von Ressourcenpools für die private AVS-Cloud mithilfe von Azure-Abonnementzuordnungen
titleSuffix: Azure VMware Solution by CloudSimple
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: ab7549650e4e20d27d3ad11a96d77ba943797f88
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77014962"
---
# <a name="create-resource-pools-for-your-avs-private-cloud-with-azure-subscription-mapping"></a>Erstellen von Ressourcenpools für die private AVS-Cloud mithilfe von Azure-Abonnementzuordnungen
Durch die Azure-Abonnementzuordnung können Sie Ressourcenpools für die private AVS-Cloud aus den verfügbaren vSphere-Ressourcenpools erstellen. Im AVS-Portal können Sie das Azure-Abonnement für Ihre privaten AVS-Clouds anzeigen und verwalten.

> [!NOTE]
> Durch das Zuordnen eines Ressourcenpools werden auch untergeordnete Ressourcenpools zugeordnet. Ein übergeordneter Ressourcenpool kann nicht zugeordnet werden, wenn untergeordnete Ressourcenpools bereits zugeordnet wurden.

1. [Greifen Sie auf das AVS-Portal zu](access-cloudsimple-portal.md).
2. Öffnen Sie die Seite **Resources** (Ressourcen), und wählen Sie **Azure subsriptions mapping** (Azure-Abonnementzuordnung) aus.  
3. Klicken Sie auf **Bearbeiten**.  
4. Sie können verfügbare Ressourcenpools zuordnen, indem Sie diese auf der linken Seite auswählen und auf den Pfeil klicken, der nach rechts zeigt. 
5. Sie können Zuordnungen entfernen, indem Sie diese auf der rechten Seite auswählen und auf den Pfeil klicken, der nach links zeigt. 

    ![Azure-Abonnements](media/resources-azure-mapping.png)

6. Klicken Sie auf **OK**.
