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
ms.openlocfilehash: f846d75833b3a796e24fd23c5f841ea24a8d1876
ms.sourcegitcommit: 57eb9acf6507d746289efa317a1a5210bd32ca2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2019
ms.locfileid: "74665904"
---
Azure Storage verschlüsselt alle Daten in einem ruhenden Speicherkonto. Standardmäßig werden Daten mit von Microsoft verwalteten Schlüsseln verschlüsselt. Sie können vom Kunden verwaltete Schlüssel auf Ebene des Speicherkontos angeben, um zusätzlich Kontrollen über die Verschlüsselungsschlüssel zu erhalten.

Vom Kunden verwaltete Schlüssel müssen in Azure Key Vault gespeichert werden. Sie können entweder Ihre eigenen Schlüssel erstellen und in einem Schlüsseltresor speichern oder mit den Azure Key Vault-APIs Schlüssel generieren. Das Speicherkonto und der Schlüsseltresor müssen sich in derselben Region befinden, dürfen aber zu verschiedenen Abonnements gehören. Weitere Informationen zur Azure Storage-Verschlüsselung und -Schlüsselverwaltung finden Sie unter [Azure Storage-Verschlüsselung für ruhende Daten](../articles/storage/common/storage-service-encryption.md). Weitere Informationen zum Azure-Schlüsseltresor finden Sie unter [Was ist der Azure-Schlüsseltresor?](../articles/key-vault/key-vault-overview.md)
