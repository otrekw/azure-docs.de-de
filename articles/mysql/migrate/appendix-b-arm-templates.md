---
title: 'Leitfaden zur Migration einer lokalen MySQL-Instanz zu Azure Database for MySQL – Anhang B: ARM-Vorlagen'
description: Mit dieser Vorlage werden alle Ressourcen mit privaten Endpunkten bereitgestellt.
ms.service: mysql
ms.subservice: migration-guide
ms.topic: how-to
author: arunkumarthiags
ms.author: arthiaga
ms.reviewer: maghan
ms.custom: ''
ms.date: 05/26/2021
ms.openlocfilehash: 07e689b8458650ca39a0aa949504dd762621bac1
ms.sourcegitcommit: 1b698fb8ceb46e75c2ef9ef8fece697852c0356c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2021
ms.locfileid: "110656776"
---
# <a name="mysql-on-premises-to-azure-database-for-mysql-migration-guide-appendix-b-arm-templates"></a>Leitfaden zur Migration einer lokalen MySQL-Instanz zu Azure Database for MySQL – Anhang B: ARM-Vorlagen

### <a name="secure"></a>Sicher

Mit dieser Vorlage werden alle Ressourcen mit privaten Endpunkten bereitgestellt. Dadurch wird jeglicher Zugriff auf die PaaS-Dienste über das Internet entfernt.

[ARM-Vorlage ](https://github.com/Azure/azure-mysql/tree/master/MigrationGuide/arm-templates/ExampleWithMigration)

### <a name="non-secure"></a>Nicht sicher

Mit dieser Vorlage werden Ressourcen mithilfe der Standardbereitstellung bereitgestellt, bei der alle Ressourcen über das Internet verfügbar sind.

[ARM-Vorlage ](https://github.com/Azure/azure-mysql/tree/master/MigrationGuide/arm-templates/ExampleWithMigrationSecure)