---
title: Bereitstellungsphase des Team Data Science-Prozesslebenszyklus
description: Die Ziele, Aufgaben und Projektleistungen für die Bereitstellungsphase Ihrer Data Science-Projekte
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: dd28d1a111c6655588b2e1254e2e503ad3f3ad28
ms.sourcegitcommit: a434cfeee5f4ed01d6df897d01e569e213ad1e6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111813536"
---
# <a name="deployment-stage-of-the-team-data-science-process-lifecycle"></a>Bereitstellungsphase des Team Data Science-Prozesslebenszyklus

In diesem Artikel werden die Ziele, Aufgaben und Projektleistungen im Zusammenhang mit der Bereitstellung des Team Data Science-Prozesses (TDSP) behandelt. Dieser Prozess bietet einen empfohlenen Lebenszyklus, mit dem Sie Ihre Data Science-Projekte strukturieren können. Der Lebenszyklus beschreibt die wichtigsten Phasen, die Projekte typischerweise, oft iterativ, durchlaufen:

   1. **Geschäftliche Aspekte**
   2. **Datenerfassung und -auswertung**
   3. **Modellierung**
   4. **Bereitstellung** (dieser Artikel)
   5. **Kundenakzeptanz**

Eine Übersicht und visuelle Darstellung des TDSP-Lebenszyklus finden Sie unter [Team Data Science-Prozesslebenszyklus](./lifecycle.md).

## <a name="goal"></a>Zielsetzung
Stellen Sie Modelle mit einer Datenpipeline in einer Produktionsumgebung oder einer produktionsähnlichen Umgebung bereit, um die endgültige Benutzerakzeptanz zu testen. 

## <a name="how-to-do-it"></a>Vorgehensweise
In dieser Phase geht es um die folgende Hauptaufgabe:

**Operationalisieren des Modells**: Stellen Sie das Modell und die Pipeline in einer Produktionsumgebung oder produktionsähnlichen Umgebung zur Nutzung der Anwendung bereit.

### <a name="operationalize-a-model"></a>Operationalisieren eines Modells
Nachdem wir eine Gruppe von Modellen mit adäquater Leistung haben, können diese für die Nutzung durch andere Anwendungen operationalisiert werden. Abhängig von den Geschäftsanforderungen erfolgen Vorhersagen entweder in Echtzeit oder auf Batchbasis. Um Modelle bereitzustellen, machen Sie diese mit einer offenen API-Schnittstelle verfügbar. Die Schnittstelle ermöglicht die einfache Verwendung des Modells durch unterschiedliche Anwendungen wie:

   * Onlinewebsites
   * Kalkulationstabellen 
   * Dashboards
   * Branchenanwendungen 
   * Back-End-Anwendungen 

Beispiele für die Operationalisierung von Modellen mit einem Azure Machine Learning-Webdienst finden Sie unter [Bereitstellen eines Azure Machine Learning-Webdiensts](../classic/deploy-a-machine-learning-web-service.md). Es gilt als bewährte Methode, in das bereitgestellte Produktionsmodell und die bereitgestellte Datenpipeline Telemetriedaten und Überwachung zu integrieren. Diese Vorgehensweise erleichtert nachfolgende Systemstatus-Berichtserstellung und Problembehandlung.  

## <a name="artifacts"></a>Artifacts

* Ein Statusdashboard mit Systemintegrität und wichtigen Metriken
* Ein finaler Modellierungsbericht mit Bereitstellungsdetails
* Ein Dokument mit der endgültigen Lösungsarchitektur


## <a name="next-steps"></a>Nächste Schritte

Hier finden Sie Links zu jedem Schritt im Lebenszyklus des TDSP:

   1. [Geschäftliche Aspekte](lifecycle-business-understanding.md)
   2. [Datenerfassung und -auswertung](lifecycle-data.md)
   3. [Modellierung](lifecycle-modeling.md)
   4. [Bereitstellung](lifecycle-deployment.md)
   5. [Kundenakzeptanz](lifecycle-acceptance.md)

Wir bieten vollständige exemplarische Vorgehensweisen, in denen sämtliche Schritte im Prozess für bestimmte Szenarien gezeigt werden. Der Artikel mit [exemplarischen Vorgehensweisen](walkthroughs.md) enthält eine Liste der Szenarien mit Links und Kurzbeschreibungen. Die exemplarischen Vorgehensweisen zeigen auch, wie Cloud- und lokale Tools sowie Dienste in einem Workflow oder einer Pipeline zum Erstellen einer intelligenten Anwendung kombiniert werden. 

Beispiele für die Ausführung der Schritte in TDSPs mit Azure Machine Learning Studio finden Sie unter [Verwenden des TDSP mit Azure Machine Learning](./index.yml).