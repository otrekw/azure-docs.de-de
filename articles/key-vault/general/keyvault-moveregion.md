---
title: 'Azure Key Vault: Verschieben eines Tresors in eine andere Region | Microsoft-Dokumentation'
description: Leitfaden zum Verschieben eines Schlüsseltresors in eine andere Region.
services: key-vault
author: ShaneBala-keyvault
manager: ravijan
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 04/24/2020
ms.author: sudbalas
Customer intent: As a key vault administrator, I want to move my vault to another region.
ms.openlocfilehash: e65a723ac9daafdc09896a50e197034104408df2
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82254071"
---
# <a name="moving-an-azure-key-vault-across-regions"></a>Regionsübergreifendes Verschieben eines Azure Key Vault

## <a name="overview"></a>Übersicht

Key Vault unterstützt keinen Vorgang zum Verschieben von Ressourcen, der das Verschieben eines Schlüsseltresors in eine andere Region zulässt. In diesem Artikel werden Problemumgehungen behandelt, wenn die Geschäftsanforderung besteht, einen Schlüsseltresor in eine andere Region zu verschieben. Jede Option weist Einschränkungen auf, und es ist wichtig, die Auswirkungen dieser Problemumgehungen zu verstehen, bevor Sie sie in einer Produktionsumgebung ausführen.

Wenn Sie einen Schlüsseltresor in eine andere Region verschieben müssen, besteht die Lösung darin, einen neuen Schlüsseltresor in der gewünschten Region zu erstellen und manuell jeden geheimen Schlüssel aus dem vorhandenen Schlüsseltresor in den neuen Schlüsseltresor zu kopieren. Dieser Vorgang kann auf eine der beiden unten aufgeführten Arten durchgeführt werden.

## <a name="design-considerations"></a>Entwurfsaspekte

* Key Vault-Namen sind global eindeutig. Der gleiche Tresorname kann nicht wiederverwendet werden.

* Sie müssen Zugriffsrichtlinien und Netzwerkkonfigurationseinstellungen im neuen Schlüsseltresor konfigurieren.

* Sie müssen vorläufiges Löschen und Bereinigungsschutz im neuen Schlüsseltresor erneut konfigurieren.

* Der Sicherungs- und Wiederherstellungsvorgang behält keine Einstellungen für automatische Rotation bei. Diese Einstellungen müssen möglicherweise erneut konfiguriert werden.

## <a name="option-1---use-the-key-vault-backup-and-restore-commands"></a>Option 1: Verwenden der Key Vault-Befehle zum Sichern und Wiederherstellen

Mit dem Sicherungsbefehl können Sie die einzelnen Geheimnisse, Schlüssel und Zertifikate in Ihrem Tresor sichern. Ihre Geheimnisse werden als verschlüsseltes Blob heruntergeladen. Anschließend können Sie das Blob in Ihrem neuen Schlüsseltresor wiederherstellen. Die Befehle werden im Link unten dokumentiert.

[Azure Key Vault-Befehle](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-6.13.0#key_vault)

### <a name="limitations"></a>Einschränkungen

* Es ist nicht möglich, einen Schlüsseltresor in einer geografischen Region zu sichern und ihn in einer anderen geografischen Region wiederherzustellen. Erfahren Sie mehr über geografische Azure-Regionen. [Link](https://azure.microsoft.com/global-infrastructure/geographies/)

* Der Sicherungsbefehl sichert alle Versionen der einzelnen Geheimnisse. Wenn Sie ein Geheimnis mit einer großen Anzahl von Vorgängerversionen (mehr als 10) verwenden, besteht die Möglichkeit, dass die Anforderung die maximal zulässige Anforderungsgröße überschreitet und der Vorgang fehlschlagen kann.

## <a name="option-2---manually-download-and-upload-secrets"></a>Option 2: Manuelles Herunterladen und Hochladen von Geheimnissen

Bestimmte Geheimnistypen können manuell heruntergeladen werden. Sie können Zertifikate beispielsweise als PFX-Datei herunterladen. Mit dieser Option werden die geografischen Einschränkungen für einige Geheimnistypen wie Zertifikate beseitigt. Sie können die PFX-Dateien in beliebige Schlüsseltresore in einer beliebigen Region hochladen. Ihr Geheimnis wird in einem Format heruntergeladen, das nicht durch ein Kennwort geschützt ist. Sie sind dafür verantwortlich, Ihre Geheimnisse zu sichern, sobald sie Key Vault verlassen, während der Verschiebevorgang durchgeführt wird.
