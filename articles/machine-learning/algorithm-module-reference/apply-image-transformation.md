---
title: Anwenden einer Bildtransformation
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie das Modul „Anwenden einer Bildtransformation“ verwenden, um eine Bildtransformation auf ein Bildverzeichnis anzuwenden.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 05/26/2020
ms.openlocfilehash: a64d5cebfd8e70e2f54a66193a7041c47887c54a
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "90898909"
---
# <a name="apply-image-transformation"></a>Anwenden einer Bildtransformation 

In diesem Artikel wird die Verwendung des Moduls „Anwenden einer Bildtransformation“ im Azure Machine Learning-Designer zum Ändern eines Eingabebildverzeichnisses auf der Grundlage einer zuvor festgelegten Bildtransformation beschrieben.  

Sie müssen eine Verbindung mit einem Modul [Initialisieren einer Bildtransformation](init-image-transformation.md) herstellen, um die Transformation festzulegen, dann können Sie eine solche Transformation auf das Eingabe-Bildverzeichnis des Moduls „Anwenden einer Bildtransformation“ anwenden.

## <a name="how-to-use-apply-image-transformation"></a>Verwenden von „Anwenden einer Bildtransformation“  

1. Fügen Sie Ihrer Pipeline das Modul **Anwenden einer Bildtransformation** hinzu. Sie finden dieses Modul in der Kategorie *Maschinelles Sehen/Transformation von Bilddaten*. 

2. Verbinden Sie die Ausgabe von **Initialisieren einer Bildtransformation** mit der Eingabe von **Anwenden einer Bildtransformation** auf der linken Seite.

     > [!NOTE]
     > Von diesem Modul wird nur eine vom Modul [Initialisieren einer Bildtransformation](init-image-transformation.md) generierte Bildtransformation akzeptiert. Stellen Sie für andere Transformationsarten eine Verbindung mit [Anwenden der Transformation](apply-transformation.md) her, andernfalls wird der Fehler ‚InvalidTransformationDirectoryError‘ ausgelöst.


3. Stellen Sie eine Verbindung mit dem Bildverzeichnis her, das Sie transformieren möchten.

4. Geben Sie für **Modus** an, für welchen Zweck Sie die Eingabetransformation verwenden möchten: ‚Zum Training‘ oder ‚Zum Rückschließen‘. 

   Wenn Sie **Zum Training** angeben, werden alle Transformationen angewendet, die Sie in „Initialisieren einer Bildtransformation“ angeben.

   Wenn Sie **Zum Rückschließen** angeben, werden Transformationen wie das zufällige Erstellen neuer Stichproben vor dem Anwenden ausgeschlossen. Das hat den Grund, dass Transformationsvorgänge zum zufälligen Erstellen neuer Stichproben wie ‚Random horizontal flip‘ zur Vermehrung der Daten beim Training verwendet werden, was beim Rückschließen nicht zur Anwendung kommen sollte, da Rückschluss-Stichproben zum Erreichen einer genauen Vorhersage und Auswertung fixiert sein müssen.

   > [!NOTE]
   > Diese Transformationen werden im Modus **Zum Rückschließen** ausgeschlossen: Random resized crop, Random crop, Random horizontal flip, Random vertical flip, Random rotation, Random affine, Random grayscale, Random perspective, Random erasing.

5. Um eine Bildtransformation auf ein neues Bildverzeichnis anzuwenden, reichen Sie die Pipeline ein.  

### <a name="module-parameters"></a>Modulparameter

| Name | Range | type | Standard                   | BESCHREIBUNG                              |
| ---- | ----- | ---- | ------------------------- | ---------------------------------------- |
| Mode | Any   | Mode | (Muss vom Benutzer angegeben werden) | Der Zweck, für den Sie die Eingabetransformation verwenden. Sie sollten zufällige (Random) Transformationsvorgänge beim Rückschließen ausschließen, sie aber fürs Training beibehalten |

### <a name="expected-inputs"></a>Erwartete Eingaben  

| Name                       | type                    | BESCHREIBUNG                       |
| -------------------------- | ----------------------- | --------------------------------- |
| Eingabe-Bildtransformation | TransformationDirectory | Eingabe-Bildtransformation        |
| Eingabe-Bildverzeichnis      | ImageDirectory          | Zu transformierendes Bildverzeichnis |

### <a name="outputs"></a>Ausgaben  

| Name                   | type           | BESCHREIBUNG            |
| ---------------------- | -------------- | ---------------------- |
| Ausgabe-Bildverzeichnis | ImageDirectory | Ausgabe-Bildverzeichnis |

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 
