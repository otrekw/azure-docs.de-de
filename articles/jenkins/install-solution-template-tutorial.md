---
title: 'Tutorial: Erstellen eines Jenkins-Servers in Azure'
description: In diesem Tutorial wird Jenkins auf der Grundlage der Jenkins-Lösungsvorlage auf einem virtuellen Azure-Linux-Computer installiert und eine Java-Beispielanwendung erstellt.
keywords: Jenkins, Azure, DevOps, Portal, virtueller Computer, Lösungsvorlage
ms.topic: tutorial
ms.date: 03/03/2020
ms.openlocfilehash: 3fa8dcbcc0edcc987fa9c4e76fbdb9d7a2f3396c
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2020
ms.locfileid: "78274529"
---
# <a name="tutorial-create-a-jenkins-server-on-an-azure-linux-vm"></a>Tutorial: Erstellen eines Jenkins-Servers auf einem virtuellen Azure-Linux-Computer 

In diesem Tutorial erfahren Sie, wie Sie [Jenkins](https://jenkins.io) auf einem virtuellen Computer unter Ubuntu Linux mit Tools und Plug-Ins installieren, die für die Arbeit mit Azure konfiguriert sind. Dadurch erhalten Sie einen in Azure ausgeführten Jenkins-Server, der eine Java-Beispielanwendung aus [GitHub](https://github.com) erstellt.

> [!div class="checklist"]
> * Installieren und Konfigurieren eines Jenkins-Servers in Azure
> * Zugreifen auf die Jenkins-Konsole über einen SSH-Tunnel
> * Erstellen eines Freestyle-Projekts
> * Kompilieren des Codes und Erstellen des Pakets für die Beispiel-App

[!INCLUDE [jenkins-install-solution-template-include](../../includes/jenkins-install-solution-template-steps.md)]