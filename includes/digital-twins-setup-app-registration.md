---
author: baanders
description: Includedatei für den Schritt „Zugriffsberechtigungen“ in der Einrichtung von Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 7/17/2020
ms.author: baanders
ms.openlocfilehash: 45f55e6c18d7cc551a6b96504ad2ce2ec3f84d86
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88009649"
---
Nachdem Sie eine Instanz von Azure Digital Twins eingerichtet haben, ist es üblich, mit dieser Instanz über eine Clientanwendung zu interagieren. Zum Erstellen einer funktionierenden Clientanwendung müssen Sie sicherstellen, dass die Client-App in der Lage ist, sich für Azure Digital Twins zu authentifizieren. Dies erfolgt durch Einrichten einer [Azure Active Directory-](../articles/active-directory/fundamentals/active-directory-whatis.md)App-Registrierung **(Azure AD)** , die Ihre Client-App dann verwendet.

Über diese App-Registrierung konfigurieren Sie die Zugriffsberechtigungen für die [Azure Digital Twins-APIs](../articles/digital-twins/how-to-use-apis-sdks.md). Ihre Client-App wird später für die App-Registrierung authentifiziert. Daraufhin werden ihr die konfigurierten Zugriffsberechtigungen für die APIs zugewiesen.

>[!TIP]
> Als Besitzer/Administrator eines Abonnements ziehen Sie es vielleicht vor, für jede neue Azure Digital Twins-Instanz eine neue App-Registrierung festzulegen *oder* nur ein Mal so vorzugehen, indem Sie eine einzige App-Registrierung festlegen, die von allen Azure Digital Twins-Instanzen im Abonnement gemeinsam genutzt wird.

### <a name="create-the-registration"></a>Erstellen der Registrierung