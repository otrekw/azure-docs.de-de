---
title: 'Mit PowerShell: Aktivieren von TDE mit Bring Your Own Key (BYOK)'
titleSuffix: Azure SQL Managed Instance
description: Erfahren Sie, wie Azure SQL Managed Instance mithilfe von PowerShell für die Verwendung von Transparent Data Encryption (TDE) zur Verschlüsselung ruhender Daten in einem BYOK-Szenario (Bring Your Own Key) konfiguriert wird.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: security
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: vanto, carlrab
ms.date: 08/25/2020
ms.openlocfilehash: c812dfa0be3e5ced40b7f347639aeb2540b75351
ms.sourcegitcommit: 927dd0e3d44d48b413b446384214f4661f33db04
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2020
ms.locfileid: "88871529"
---
# <a name="transparent-data-encryption-in-sql-managed-instance-using-your-own-key-from-azure-key-vault"></a>Transparent Data Encryption in SQL Managed Instance mithilfe eines eigenen Schlüssels aus dem Azure Key Vault

[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqlmi.md)]

In diesem PowerShell-Skriptbeispiel wird Transparent Data Encryption (TDE) mit einem kundenseitig verwalteten Schlüssel für eine verwaltete Azure SQL-Datenbank-Instanz unter Verwendung eines Schlüssels aus Azure Key Vault konfiguriert. Dieses Szenario wird häufig als „Bring Your Own Key“ (BYOK) für TDE bezeichnet. Mehr dazu erfahren Sie unter [Azure SQL Transparent Data Encryption mithilfe eines kundenseitig verwalteten Schlüssels](../../database/transparent-data-encryption-byok-overview.md).

## <a name="prerequisites"></a>Voraussetzungen

- Eine verwaltete Instanz. Siehe dazu [Verwenden von PowerShell zum Erstellen einer verwalteten Instanz](create-configure-managed-instance-powershell.md).

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Sowohl die lokale Verwendung von PowerShell als auch die Verwendung von Azure Cloud Shell erfordert Azure PowerShell 2.3.2 oder eine höhere Version. Wenn Sie ein Upgrade durchführen müssen, lesen Sie [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps), oder führen Sie das folgende Beispielskript zum Installieren des Moduls für den aktuellen Benutzer aus:

`Install-Module -Name Az -AllowClobber -Scope CurrentUser`

Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="sample-scripts"></a>Beispielskripts 

[!code-powershell-interactive[main](../../../../powershell_scripts/sql-database/transparent-data-encryption/setup-tde-byok-sqlmi.ps1 "Set up BYOK TDE for SQL Managed Instance")]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/).

Weitere PowerShell-Skriptbeispiele für SQL Managed Instance finden Sie unter [Azure PowerShell-Beispiele für Azure SQL-Datenbank und verwaltete Azure SQL-Instanzen](../../database/powershell-script-content-guide.md).
