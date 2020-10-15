---
title: Aktualisieren des Kennworts für das AD DS-Speicherkonto
description: In diesem Artikel erfahren Sie, wie Sie das Kennwort eines Active Directory Domain Services-Kontos aktualisieren, das Ihr Speicherkonto darstellt. Dies verhindert ein Bereinigen des Speicherkontos bei Ablauf des Kennworts sowie Authentifizierungsfehler.
author: roygara
ms.service: storage
ms.subservice: files
ms.topic: how-to
ms.date: 06/22/2020
ms.author: rogarana
ms.openlocfilehash: d16f11a85c6b370b0187975cce965bf3e1b5ba3f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85510253"
---
# <a name="update-the-password-of-your-storage-account-identity-in-ad-ds"></a>Aktualisieren des Kennworts für Ihre Speicherkontoidentität in AD DS

Wenn Sie die AD DS-Identität/das AD DS-Konto (Active Directory Domain Services), die bzw. das Ihr Speicherkonto darstellt, in einer Organisationseinheit oder Domäne registriert haben, die einen Ablaufzeitpunkt für Kennwörter erzwingt, müssen Sie das Kennwort vor dem maximalen Kennwortalter ändern. Möglicherweise führt Ihre Organisation automatische Bereinigungsskripts aus, die Konten bei Ablauf des Kennworts löschen. Wenn Sie Ihr Kennwort nicht ändern, bevor es abläuft, könnte Ihr Konto daher gelöscht werden. Dadurch würden Sie den Zugriff auf Ihre Azure-Dateifreigaben verlieren.

Durch Ausführen des Befehls `Update-AzStorageAccountADObjectPassword` aus dem [Modul AzFilesHybrid](https://github.com/Azure-Samples/azure-files-samples/releases) können Sie eine Kennwortrotation auslösen. Dieser Befehl muss in einer lokalen mit AD DS verbundenen Umgebung von einem Hybridbenutzer ausgeführt werden, der über die Besitzerberechtigung für das Speicherkonto und AD DS-Berechtigungen zum Ändern des Kennworts der Identität verfügt, die das Speicherkonto darstellt. Er führt Aktionen aus, die der Speicherkonto-Schlüsselrotation ähneln. Genauer gesagt ruft er den zweiten Kerberos-Schlüssel des Speicherkontos ab und aktualisiert damit das Kennwort für das registrierte Konto in AD DS. Anschließend generiert er den Kerberos-Zielschlüssel des Speicherkontos neu und aktualisiert das Kennwort für das registrierte Konto in AD DS. Sie müssen diesen Befehl in einer lokalen mit AD DS verbundenen Umgebung ausführen.

```PowerShell
# Update the password of the AD DS account registered for the storage account
# You may use either kerb1 or kerb2
Update-AzStorageAccountADObjectPassword `
        -RotateToKerbKey kerb2 `
        -ResourceGroupName "<your-resource-group-name-here>" `
        -StorageAccountName "<your-storage-account-name-here>"
```
