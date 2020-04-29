---
title: Webdiensteingabe/-ausgabe
description: Erfahren Sie mehr über die verfügbaren Module im Azure Machine Learning-Designer (Vorschauversion).
titleSuffix: Azure Machine Learning
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 04/13/2020
ms.openlocfilehash: be940e3ba693270707e22ffc7b9377dbea1df5f8
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81462022"
---
# <a name="web-service-inputoutput"></a>Webdiensteingabe/-ausgabe

In diesem Artikel werden die Module **Web Service Input** und **Web Service Output** im Azure Machine Learning-Designer (Vorschauversion) beschrieben.

Das Modul **Web Service Input** kann nur eine Verbindung mit einem Eingabeport vom Typ **DataFrameDirectory** herstellen. Das Modul **Web Service Output** kann entsprechend nur mit einem Ausgabeport vom Typ **DataFrameDirectory** verbunden werden. Die beiden Module sind in der Modulstruktur in der Kategorie **Web Service** zu finden. 

Mit dem Modul **Web Service Input** wird angegeben, wo Benutzerdaten in die Pipeline gelangen, und mit dem Modul **Web Service Output** wird angegeben, wo die Benutzerdaten in einer Echtzeit-Rückschlusspipeline zurückgegeben werden.

## <a name="how-to-use-web-service-inputoutput"></a>Verwenden von Web Service Input/Output

- Wenn Sie aus Ihrer Trainingspipeline eine Echtzeit-Rückschlusspipeline erstellen, werden automatisch **Web Service Input**- und **Web Service Output**-Module hinzugefügt, um anzuzeigen, wo Benutzerdaten in die Pipeline gelangen und wo Daten zurückgegeben werden. 

    Erfahren Sie mehr über das [Erstellen einer Echtzeit-Rückschlusspipeline](https://docs.microsoft.com/azure/machine-learning/tutorial-designer-automobile-price-deploy#create-a-real-time-inference-pipeline).

    > [!NOTE]
    > Das automatische Generieren einer Echtzeit-Rückschlusspipeline ist ein regelbasierter Prozess nach bestem Wissen, bei dem keine Richtigkeit garantiert werden kann. Sie können manuell **Web Service Input-/Output**-Module hinzufügen oder entfernen, um Ihre Anforderungen zu erfüllen. Stellen Sie sicher, dass in der Echtzeit-Rückschlusspipeline mindestens ein **Web Service Input**- und ein **Web Service Output**-Modul vorhanden ist. Wenn Sie über mehrere **Web Service Input**- oder **Web Service Output**-Module verfügen, stellen Sie sicher, dass diese eindeutige Namen aufweisen. Sie können den Namen im rechten Bereich des Moduls eingeben.

- Sie können eine Echtzeit-Rückschlusspipeline auch manuell erstellen, indem Sie in Ihrer noch nicht übermittelten Pipeline **Web Service Input**- und **Web Service Output**-Module hinzufügen.

    > [!NOTE]
    >  Der Typ der Pipeline wird beim ersten Übermitteln festgelegt. Stellen Sie also sicher, dass Sie die **Web Service Input**- und **Web Service Output**-Module vor dem ersten Übermitteln hinzufügen, wenn Sie eine Echtzeit-Rückschlusspipeline erstellen möchten.

   Im nachstehenden Beispiel wird gezeigt, wie Sie aus dem Modul **Execute Python Script** manuell eine Echtzeit-Rückschlusspipeline erstellen. 

   ![Beispiel](media/module/web-service-input-output-example.png)
   
   Nachdem Sie die Pipeline übermittelt haben und die Ausführung erfolgreich abgeschlossen wurde, können Sie den Echtzeitendpunkt bereitstellen.
   
   > [!NOTE]
   >  Im obigen Beispiel stellt die **manuelle Dateneingabe** das Datenschema für die Webdiensteingabe bereit und ist für die Bereitstellung des Echtzeitendpunkts erforderlich. Im Allgemeinen sollten Sie immer ein Modul oder Dataset mit dem Port verbinden, mit dem **Web Service Input** verbunden ist, um das Datenschema bereitzustellen.
   
## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über das [Bereitstellen des Echtzeitendpunkts](https://docs.microsoft.com/azure/machine-learning/tutorial-designer-automobile-price-deploy#deploy-the-real-time-endpoint).
Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an.