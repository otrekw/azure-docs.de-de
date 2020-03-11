---
title: Bereitstellen des Formularerkennungstools für die Bezeichnung von Beispielen
titleSuffix: Azure Cognitive Services
description: Erfahren Sie mehr über die verschiedenen Möglichkeiten, wie Sie das Formularerkennungstool für die Bezeichnung von Beispielen für überwachte Lernen bereitstellen können.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 02/28/2020
ms.author: pafarley
ms.openlocfilehash: fa419d7dd9668ac2ce8f2b0eb904117c7e22692d
ms.sourcegitcommit: 1fa2bf6d3d91d9eaff4d083015e2175984c686da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2020
ms.locfileid: "78207832"
---
# <a name="deploy-the-sample-labeling-tool"></a>Bereitstellen des Samplebezeichnungstools

Das Formularerkennungstool für die Bezeichnung von Beispielen ist eine Anwendung, die in einem Docker-Container ausgeführt wird. Es stellt eine hilfreiche Benutzeroberfläche bereit, mit der Sie Formulardokumente für das überwachte Lernen manuell bezeichnen können. Im Schnellstart [Trainieren mit Bezeichnungen](./quickstarts/label-tool.md) wird gezeigt, wie Sie das Tool auf einem lokalen Computer ausführen. Dies ist das häufigste Szenario. 

In dieser Anleitung werden alternative Möglichkeiten erläutert, wie Sie das Tool zur Beispielbezeichnung bereitstellen und ausführen können. 

## <a name="deploy-with-azure-container-instances"></a>Bereitstellen mit Azure Container Instances

Sie können das Bezeichnungstool in einem Docker-Web-App-Container ausführen. [Erstellen Sie zunächst eine neue Web-App-Ressource](https://ms.portal.azure.com/#create/Microsoft.WebSite) im Azure-Portal. Füllen Sie das Formular mit den Details Ihres Abonnements und der Ressourcengruppe aus. Geben Sie die folgenden Informationen in die erforderlichen Felder ein:
* **Veröffentlichen**: Docker-Container
* **Betriebssystem:** Linux

Füllen Sie auf der nächsten Seite die folgenden Felder für die Einrichtung des Docker-Containers aus:

* **Optionen:** Einzelner Container
* **Imagequelle:** Azure Container Registry
* **Zugriffstyp:** öffentlich
* **Image und Tag:** mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool:latest

Die folgenden Schritte sind optional. Nachdem die Bereitstellung Ihrer App abgeschlossen wurde, können Sie sie ausführen und online auf das Bezeichnungstool zugreifen.

### <a name="connect-to-azure-ad-for-authorization"></a>Herstellen einer Verbindung mit Azure AD zur Autorisierung

Es wird empfohlen, eine Verbindung zwischen Ihrer Web-App und Azure Active Directory (AAD) herzustellen, sodass sich nur Personen mit Ihren Anmeldeinformationen anmelden und die App verwenden können. Befolgen Sie die Anweisungen in [Konfigurieren Ihrer App Service-App](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-aad), um eine Verbindung mit AAD herzustellen.

## <a name="next-steps"></a>Nächste Schritte

Wechseln Sie zurück zum Schnellstart [Trainieren mit Bezeichnungen](./quickstarts/label-tool.md), um zu erfahren, wie Sie das Tool verwenden, um Trainingsdaten manuell zu bezeichnen und das überwachte Lernen auszuführen.
