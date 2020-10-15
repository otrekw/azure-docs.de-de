---
title: 'Web Service Input/Output: Modulreferenz'
description: Erfahren Sie mehr über die verfügbaren Module im Azure Machine Learning-Designer.
titleSuffix: Azure Machine Learning
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 04/13/2020
ms.openlocfilehash: a62f8aee0bd0a0d2b7009a48e9d5f00ea3c5155f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90883224"
---
# <a name="web-service-input-and-web-service-output-modules"></a>Web Service Input- und Web Service Output-Modul (Webdiensteingabe und Webdienstausgabe)

In diesem Artikel werden die Module Web Service Input und Web Service Output im Azure Machine Learning-Designer beschrieben.

Das Modul Web Service Input kann nur eine Verbindung mit einem Eingabeport des Typs **DataFrameDirectory** herstellen. Das Modul Web Service Output kann nur eine Verbindung von einem Ausgabeport des Typs **DataFrameDirectory** herstellen. Sie finden die beiden Module in der Modulstruktur in der Kategorie **Web Service**. 

Das Modul Web Service Input gibt an, wo Benutzerdaten in die Pipeline gelangen. Das Modul Web Service Output gibt an, wo Benutzerdaten in einer Echtzeit-Rückschlusspipeline zurückgegeben werden.

## <a name="how-to-use-web-service-input-and-output"></a>Verwenden von Web Service Input und Output

Wenn Sie aus Ihrer Trainingspipeline [eine Echtzeit-Rückschlusspipeline erstellen](https://docs.microsoft.com/azure/machine-learning/tutorial-designer-automobile-price-deploy#create-a-real-time-inference-pipeline), werden automatisch Web Service Input- und Web Service Output-Module hinzugefügt, um anzuzeigen, wo Benutzerdaten in die Pipeline gelangen und wo Daten zurückgegeben werden. 

> [!NOTE]
> Das automatische Generieren einer Echtzeit-Rückschlusspipeline ist ein regelbasierter Prozess nach bestem Wissen. Dabei gibt es keine Garantie für die Richtigkeit. 

Sie können manuell Web Service Input- und Web Service Output-Module hinzufügen oder entfernen, um Ihre Anforderungen zu erfüllen. Stellen Sie sicher, dass Ihre Echtzeit-Rückschlusspipeline mindestens über ein Web Service Input- und ein Web Service Output-Modul verfügt. Wenn Sie über mehrere Web Service Input- oder Web Service Output-Module verfügen, stellen Sie sicher, dass diese eindeutige Namen aufweisen. Sie können den Namen im rechten Bereich des Moduls eingeben.

Sie können eine Echtzeit-Rückschlusspipeline auch manuell erstellen, indem Sie in Ihrer noch nicht übermittelten Pipeline Web Service Input- und Web Service Output-Module hinzufügen.

> [!NOTE]
> Der Typ der Pipeline wird beim ersten Übermitteln festgelegt. Stellen Sie also sicher, dass Sie die Web Service Input- und Web Service Output-Module vor dem ersten Übermitteln hinzufügen.

Im nachstehenden Beispiel wird gezeigt, wie Sie aus dem Modul Execute Python Script manuell eine Echtzeit-Rückschlusspipeline erstellen. 

![Beispiel](media/module/web-service-input-output-example.png)
   
Nachdem Sie die Pipeline übermittelt haben und die Ausführung erfolgreich abgeschlossen wurde, können Sie den Echtzeitendpunkt bereitstellen.
   
> [!NOTE]
>  Im obigen Beispiel stellt die **manuelle Dateneingabe** das Datenschema für die Webdiensteingabe bereit und ist für die Bereitstellung des Echtzeitendpunkts erforderlich. Im Allgemeinen sollten Sie immer ein Modul oder Dataset mit dem Port verbinden, mit dem **Web Service Input** verbunden ist, um das Datenschema bereitzustellen.
   
## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über das [Bereitstellen des Echtzeitendpunkts](https://docs.microsoft.com/azure/machine-learning/tutorial-designer-automobile-price-deploy#deploy-the-real-time-endpoint).

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an.