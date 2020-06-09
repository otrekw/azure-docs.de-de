---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/06/2020
ms.author: glenga
ms.openlocfilehash: 0bc5977a581006dc760c0435f9d68371ced7e4cd
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83648780"
---
Azure Storage verschlüsselt alle Daten in einem ruhenden Speicherkonto. Weitere Informationen finden Sie unter [Azure Storage-Verschlüsselung für ruhende Daten](../articles/storage/common/storage-service-encryption.md).

Standardmäßig werden Daten mit von Microsoft verwalteten Schlüsseln verschlüsselt. Für eine zusätzliche Kontrolle über die Verschlüsselungsschlüssel können Sie vom Kunden verwaltete Schlüssel bereitstellen, mit denen Blob- und Dateidaten verschlüsselt werden können. Diese Schlüssel müssen in Azure Key Vault vorhanden sein, damit Functions auf das Speicherkonto zugreifen kann. Weitere Informationen finden Sie unter [Verschlüsselung im Ruhezustand mithilfe von kundenseitig verwalteten Schlüsseln](../articles/azure-functions/configure-encrypt-at-rest-using-cmk.md).  