---
title: 'Web-App-Tutorial: Einrichten von Azure API for FHIR'
description: Dieses Tutorial führt Sie Schritte für Schritt durch ein Beispiel für die Bereitstellung einer einfachen Webanwendung. In diesem ersten Tutorial werden die Voraussetzungen und die Bereitstellung von Azure API for FHIR beschrieben.
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: tutorial
ms.reviewer: matjazl
ms.author: cavoeg
author: caitlinv39
ms.date: 01/03/2020
ms.custom: devx-track-javascript
ms.openlocfilehash: 4b39c32da04efa3782cb2166c8e1137029b21258
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87852955"
---
# <a name="deploy-javascript-app-to-read-data-from-fhir-service"></a>Bereitstellen einer JavaScript-App zum Lesen von Daten aus dem FHIR-Dienst
In diesem Tutorial stellen Sie eine kleine JavaScript-App bereit, die Daten aus einem FHIR-Dienst liest. Die folgenden Schritte werden in diesem Tutorial beschrieben:
1. Bereitstellen eines FHIR-Servers
1. Registrieren einer öffentlichen Clientanwendung
1. Testen des Zugriffs auf die Anwendung
1. Erstellen einer Webanwendung, die diese FHIR-Daten liest

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit dieser Tutorialreihe beginnen, benötigen Sie Folgendes:
1. Azure-Abonnement
1. Azure Active Directory-Mandant
1. [Postman](https://www.getpostman.com/) ist installiert

> [!NOTE]
> Bei diesem Tutorial befinden sich der FHIR-Dienst, die Azure AD-Anwendung und die Azure AD-Benutzer alle in demselben Azure AD-Mandanten. Wenn das nicht der Fall ist, können Sie die Schritte in diesem Tutorial dennoch ausführen, müssen aber möglicherweise auf einige der im Text genannten Dokumente zurückgreifen, um zusätzliche Schritte auszuführen.

## <a name="deploy-azure-api-for-fhir"></a>Bereitstellen von Azure API for FHIR
Der erste Schritt im Tutorial ist das ordnungsgemäße Einrichten von Azure API for FHIR.

1. Stellen Sie [Azure API for FHIR](fhir-paas-portal-quickstart.md) bereit.
1. Sobald Sie Azure API for FHIR bereitgestellt haben, konfigurieren Sie die [CORS](configure-cross-origin-resource-sharing.md)-Einstellungen. Dazu rufen Sie Azure API for FHIR aus und wählen CORS aus. 
    1. Legen Sie **Ursprünge** auf * fest.
    1. Legen Sie **Header** auf * fest.
    1. Wählen Sie unter **Methoden** die Option **Alle auswählen** aus.
    1. Legen Sie **Max. Alter** auf **600** fest.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie Azure API for FHIR bereitgestellt haben, können Sie eine öffentliche Clientanwendung registrieren.

>[!div class="nextstepaction"]
>[Registrieren einer öffentlichen Clientanwendung](tutorial-web-app-public-app-reg.md)
