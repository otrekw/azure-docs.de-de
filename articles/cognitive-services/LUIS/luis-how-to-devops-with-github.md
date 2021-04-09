---
title: 'Anwenden von DevOps mit GitHub: LUIS'
titleSuffix: Azure Cognitive Services
description: Wenden Sie DevOps mithilfe von GitHub auf Language Understanding (LUIS) an.
services: cognitive-services
author: andycw
manager: cmayo
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 06/5/2020
ms.author: anwigley
ms.openlocfilehash: 448b3d93ed58e4cfc73da576f0c5871600400ac6
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "98019838"
---
# <a name="apply-devops-to-luis-app-development-using-github-actions"></a>Anwenden von DevOps auf die Entwicklung von LUIS-Apps mithilfe von GitHub Actions

Navigieren Sie zum [LUIS DevOps-Vorlagenrepository](https://github.com/Azure-Samples/LUIS-DevOps-Template), wenn Sie eine vollständige Projektmappe suchen, um DevOps und bewährte Methoden der Softwareentwicklung für LUIS zu implementieren. Sie können dieses Vorlagenrepository verwenden, um ein eigenes Repository mit integrierter Unterstützung für CI/CD-Workflows und Praktiken zu erstellen, die [Quellcodeverwaltung](luis-concept-devops-sourcecontrol.md), [automatisierte Builds](luis-concept-devops-automation.md), [Tests](luis-concept-devops-testing.md) und [Releaseverwaltung](luis-concept-devops-automation.md#release-management) mit LUIS für Ihr eigenes Projekt ermöglichen.

## <a name="the-luis-devops-template-repo"></a>Das LUIS DevOps-Vorlagenrepository

Das [LUIS DevOps-Vorlagenrepository](https://github.com/Azure-Samples/LUIS-DevOps-Template) führt Sie durch die folgenden Schritte:

* **Klonen des Vorlagenrepositorys**: Kopieren Sie die Vorlage in Ihr eigenes GitHub-Repository.
* **Konfigurieren von LUIS-Ressourcen**: Erstellen Sie die [LUIS-Erstellungs- und Vorhersageressourcen in Azure](./luis-how-to-azure-subscription.md), die von den Continuous Integration-Workflows verwendet werden.
* **Konfigurieren der CI/CD-Workflows**: Konfigurieren Sie Parameter für die CI/CD-Workflows, und speichern Sie sie in [GitHub-Geheimnissen](https://help.github.com/actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets).
* **Abschreiten der [„inneren Entwicklungsschleife“](/dotnet/architecture/containerized-lifecycle/design-develop-containerized-apps/docker-apps-inner-loop-workflow)** : Der Entwickler nimmt Aktualisierungen an einer LUIS-Beispiel-App vor, während er in einem Entwicklungsbranch arbeitet, testet die Aktualisierungen und löst dann einen Pull Request aus, um die Änderungen vorzuschlagen und um Genehmigung aus dem Review zu bitten.
* **Ausführen von CI/CD-Workflows**: Ausführen von [Continuous Integration-Workflows zum Erstellen und Testen einer LUIS-App](luis-concept-devops-automation.md) mithilfe von GitHub Actions.
* **Ausführen von automatisierten Tests**: Ausführen von [automatisiertem Testen in Batches für eine LUIS-App](luis-concept-devops-testing.md), um die Qualität der App zu bewerten.
* **Bereitstellen der LUIS-App**: Ausführen eines [CD-Auftrags (Continuous Delivery)](luis-concept-devops-automation.md#continuous-delivery-cd) zum Veröffentlichen der LUIS-App.
* **Verwenden des Repositorys für Ihr eigenes Projekt**: Erläutert, wie Sie das Repository für Ihre eigene LUIS-Anwendung nutzen können.

## <a name="next-steps"></a>Nächste Schritte

* Verwenden des [LUIS DevOps-Vorlagenrepositorys](https://github.com/Azure-Samples/LUIS-DevOps-Template) zum Anwenden von DevOps auf Ihr eigenes Projekt.
* [Quellcodeverwaltungs- und Branchstrategien für LUIS](luis-concept-devops-sourcecontrol.md)
* [Testen für LUIS DevOps](luis-concept-devops-testing.md)
* [Automatisierungsworkflows für LUIS DevOps](luis-concept-devops-automation.md)
