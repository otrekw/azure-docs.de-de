---
title: include file
description: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/26/2019
ms.author: tamram
ms.custom: include
ms.openlocfilehash: 694501fdaaaa92e898f4973838d86343e29144e3
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2019
ms.locfileid: "74895295"
---
Azure Storage verschlüsselt alle Daten in einem ruhenden Speicherkonto. Standardmäßig werden Daten mit von Microsoft verwalteten Schlüsseln verschlüsselt. Für eine zusätzliche Kontrolle über die Verschlüsselungsschlüssel können Sie vom Kunden verwaltete Schlüssel bereitstellen, mit denen Blob- und Dateidaten verschlüsselt werden können.

Vom Kunden verwaltete Schlüssel müssen in Azure Key Vault gespeichert werden. Sie können entweder Ihre eigenen Schlüssel erstellen und in einem Schlüsseltresor speichern oder mit den Azure Key Vault-APIs Schlüssel generieren. Das Speicherkonto und der Schlüsseltresor müssen sich in derselben Region befinden, dürfen aber zu verschiedenen Abonnements gehören. Weitere Informationen zur Azure Storage-Verschlüsselung und -Schlüsselverwaltung finden Sie unter [Azure Storage-Verschlüsselung für ruhende Daten](../articles/storage/common/storage-service-encryption.md). Weitere Informationen zum Azure-Schlüsseltresor finden Sie unter [Was ist der Azure-Schlüsseltresor?](../articles/key-vault/key-vault-overview.md)
