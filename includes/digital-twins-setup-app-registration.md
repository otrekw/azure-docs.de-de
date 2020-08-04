---
author: baanders
description: Includedatei für den Schritt „Zugriffsberechtigungen“ in der Einrichtung von Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 7/17/2020
ms.author: baanders
ms.openlocfilehash: 18aa039977beeff3f6b16249822fbd98370ca404
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87408160"
---
Nachdem Sie eine Instanz von Azure Digital Twins eingerichtet haben, ist es üblich, mit dieser Instanz über eine Clientanwendung zu interagieren. Zum Erstellen einer funktionierenden Clientanwendung müssen Sie sicherstellen, dass die Client-App in der Lage ist, sich für Azure Digital Twins zu authentifizieren. Dies erfolgt durch Einrichten einer [Azure Active Directory-](../articles/active-directory/fundamentals/active-directory-whatis.md)App-Registrierung **(Azure AD)** , die Ihre Client-App dann verwendet.

Über diese App-Registrierung konfigurieren Sie die Zugriffsberechtigungen für die [Azure Digital Twins-APIs](../articles/digital-twins/how-to-use-apis-sdks.md). Ihre Client-App wird später für die App-Registrierung authentifiziert. Daraufhin werden ihr die konfigurierten Zugriffsberechtigungen für die APIs zugewiesen.

>[!TIP]
> Als Besitzer eines Abonnements ziehen Sie es vielleicht vor, für jede neue Azure Digital Twins-Instanz eine neue App-Registrierung festzulegen, *oder* nur ein Mal so vorzugehen, indem Sie eine einzige App-Registrierung festlegen, die von allen Azure Digital Twins-Instanzen im Abonnement gemeinsam genutzt wird.

### <a name="create-the-registration"></a>Erstellen der Registrierung