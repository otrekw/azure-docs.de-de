---
title: Sicherheitswarnungen für Umgebungen in Azure DevTest Labs
description: In diesem Artikel erfahren Sie, wie Sie Sicherheitswarnungen für eine Umgebung in DevTest Labs anzeigen und eine entsprechende Aktion durchführen.
services: devtest-lab,lab-services
documentationcenter: na
author: spelluru
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2020
ms.author: spelluru
ms.openlocfilehash: 4ca17bece33107de756eb221e14eaab851660a99
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/04/2020
ms.locfileid: "76992021"
---
# <a name="security-alerts-for-environments-in-azure-devtest-labs"></a>Sicherheitswarnungen für Umgebungen in Azure DevTest Labs
In diesem Artikel erfahren Sie, wie Sie Sicherheitswarnungen für Umgebungen in Azure DevTest Labs anzeigen. 

## <a name="prerequisites"></a>Voraussetzungen
Derzeit können Sie Sicherheitswarnungen nur für Umgebungen anzeigen, die in Ihrem Lab bereitgestellt wurden. Um dieses Feature zu testen oder zu verwenden, stellen Sie eine Umgebung in Ihrem Lab bereit. 

## <a name="view-security-alerts-for-an-environment"></a>Anzeigen von Sicherheitswarnungen für eine Umgebung

1. Wählen Sie auf der Homepage Ihres Labs im linken Menü **Sicherheitswarnungen** aus. Die Anzahl der Sicherheitswarnungen (hoch, mittel und niedrig) sollte angezeigt werden.

    ![Sicherheitswarnungen – Übersicht](./media/environment-security-alerts/security-alerts-overview-page.png)
2. Klicken Sie mit der rechten Maustaste auf die drei Punkte (...) in der letzten Spalte, und wählen Sie **Sicherheitswarnungen anzeigen** aus. 

    ![Anzeigen von Sicherheitswarnungen](./media/environment-security-alerts/view-security-alerts-menu.png)
3. Es werden weitere Informationen zu den Warnungen sowie Ratgeberempfehlungen angezeigt. 

    ![Anzeigen von Sicherheitswarnungen](./media/environment-security-alerts/advisor-recommendations.png)


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Umgebungen finden Sie in den folgenden Artikeln:

- [Erstellen von Umgebungen mit mehreren virtuellen Computern und PaaS-Ressourcen mit Azure Resource Manager-Vorlagen](devtest-lab-create-environment-from-arm.md)
- [Konfigurieren und Verwenden von öffentlichen Umgebungen](devtest-lab-configure-use-public-environments.md)
