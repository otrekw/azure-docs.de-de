---
title: Erstellen von Ressourcenpools mit Azure-Abonnementzuordnungen
titleSuffix: Azure VMware Solution by CloudSimple
description: Beschreibt das Erstellen von Ressourcenpools für die private Cloud mithilfe von Azure-Abonnementzuordnungen.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 474ef03d482288b6bf7b5a8b1c224349a8e2d3a8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "77014962"
---
# <a name="create-resource-pools-for-your-private-cloud-with-azure-subscription-mapping"></a>Erstellen von Ressourcenpools für die private Cloud mithilfe von Azure-Abonnementzuordnungen
Azure-Abonnementzuordnung ermöglicht das Erstellen von Ressourcenpools für die private Cloud aus den verfügbaren vSphere-Ressourcenpools. Im CloudSimple-Portal können Sie das Azure-Abonnement für Ihre privaten Clouds anzeigen und verwalten.

> [!NOTE]
> Durch das Zuordnen eines Ressourcenpools werden auch untergeordnete Ressourcenpools zugeordnet. Ein übergeordneter Ressourcenpool kann nicht zugeordnet werden, wenn untergeordnete Ressourcenpools bereits zugeordnet wurden.

1. [Rufen Sie das CloudSimple-Portal auf](access-cloudsimple-portal.md).
2. Öffnen Sie die Seite **Resources** (Ressourcen), und wählen Sie **Azure subsriptions mapping** (Azure-Abonnementzuordnung) aus.  
3. Klicken Sie auf **Bearbeiten**.  
4. Sie können verfügbare Ressourcenpools zuordnen, indem Sie diese auf der linken Seite auswählen und auf den Pfeil klicken, der nach rechts zeigt. 
5. Sie können Zuordnungen entfernen, indem Sie diese auf der rechten Seite auswählen und auf den Pfeil klicken, der nach links zeigt. 

    ![Azure-Abonnements](media/resources-azure-mapping.png)

6. Klicken Sie auf **OK**.
