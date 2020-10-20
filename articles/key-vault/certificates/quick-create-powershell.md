---
title: 'Schnellstart: Festlegen und Anzeigen von Azure Key Vault-Zertifikaten – Azure PowerShell'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe von Azure PowerShell ein Zertifikat in Azure Key Vault festlegen und daraus abrufen.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: quickstart
ms.custom: mvc, seo-javascript-september2019, seo-javascript-october2019
ms.date: 09/03/2019
ms.author: mbaldwin
ms.openlocfilehash: 7ff618024c269a010eddf08128e827ade7921c94
ms.sourcegitcommit: 541bb46e38ce21829a056da880c1619954678586
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2020
ms.locfileid: "91940598"
---
# <a name="quickstart-set-and-retrieve-a-certificate-from-azure-key-vault-using-azure-powershell"></a>Schnellstart: Festlegen und Abrufen eines Zertifikats aus Azure Key Vault mit Azure PowerShell

In dieser Schnellstartanleitung erstellen Sie mit Azure PowerShell einen Schlüsseltresor in Azure Key Vault. Azure Key Vault ist ein Clouddienst, der als sicherer Geheimnisspeicher fungiert. Dadurch können Schlüssel, Kennwörter, Zertifikate und andere Geheimnisse sicher gespeichert werden. Weitere Informationen zu Key Vault finden Sie in der [Übersicht](../general/overview.md). Mit Azure PowerShell können Azure-Ressourcen mithilfe von Befehlen oder Skripts erstellt und verwaltet werden. Nachdem Sie diese Vorgänge ausgeführt haben, speichern Sie ein Zertifikat.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie PowerShell lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial mindestens die Version 1.0.0 des Azure PowerShell-Moduls verwenden. Geben Sie `$PSVersionTable.PSVersion` ein, um die Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Login-AzAccount` ausführen, um eine Verbindung mit Azure herzustellen.

```azurepowershell-interactive
Login-AzAccount
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) eine Azure-Ressourcengruppe. Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. 

```azurepowershell-interactive
New-AzResourceGroup -Name ContosoResourceGroup -Location EastUS
```

## <a name="create-a-key-vault"></a>Erstellen eines Schlüsseltresors

Als Nächstes erstellen Sie eine Key Vault-Instanz. Für diesen Schritt benötigen Sie einige Informationen:

In dieser Schnellstartanleitung lautet der Name für die Key Vault-Instanz zwar „Contoso KeyVault2“, Sie müssen jedoch einen eindeutigen Namen verwenden.

- **Tresorname:** Contoso-Vault2
- **Ressourcengruppenname:** ContosoResourceGroup
- **Standort:** USA, Osten

```azurepowershell-interactive
New-AzKeyVault -Name 'Contoso-Vault2' -ResourceGroupName 'ContosoResourceGroup' -Location 'East US'
```

Die Ausgabe dieses Cmdlets zeigt Eigenschaften des neu erstellten Schlüsseltresors. Beachten Sie die beiden folgenden Eigenschaften:

* **Tresorname**: In diesem Beispiel wird **Contoso-Vault2** verwendet. Sie verwenden diesen Namen für andere Schlüsseltresor-Cmdlets.
* **Tresor-URI**: In diesem Beispiel https://Contoso-Vault2.vault.azure.net/. Anwendungen, die Ihren Tresor über die zugehörige REST-API nutzen, müssen diesen URI verwenden.

Nach der Tresorerstellung ist Ihr Azure-Konto das einzige Konto, das zum Ausführen von Vorgängen für diesen neuen Tresor berechtigt ist.

## <a name="add-a-certificate-to-key-vault"></a>Hinzufügen eines Zertifikats zu Key Vault

Um dem Tresor ein Zertifikat hinzuzufügen, sind nur einige zusätzliche Schritte erforderlich. Dieses Zertifikat kann dann von einer Anwendung verwendet werden. 

Geben Sie die folgenden Befehle ein, um das selbstsignierte Zertifikat **ExampleCertificate** mit einer Richtlinie zu erstellen:

```azurepowershell-interactive
$Policy = New-AzKeyVaultCertificatePolicy -SecretContentType "application/x-pkcs12" -SubjectName "CN=contoso.com" -IssuerName "Self" -ValidityInMonths 6 -ReuseKeyOnRenewal
Add-AzKeyVaultCertificate -VaultName "Contoso-Vault2" -Name "ExampleCertificate" -CertificatePolicy $Policy
```

Nun können Sie mit dem dazugehörigen URI auf dieses Zertifikat verweisen, das Sie der Azure Key Vault-Instanz hinzugefügt haben. Verwenden Sie **„https://Contoso-Vault2.vault.azure.net/certificates/ExampleCertificate“** , um die aktuelle Version abzurufen. 

So zeigen Sie ein zuvor gespeichertes Zertifikat an:

```azurepowershell-interactive
Get-AzKeyVaultCertificate -VaultName "Contoso-Vault2" -Name "ExampleCertificate"
```

Sie haben nun eine Key Vault-Instanz erstellt sowie ein Zertifikat gespeichert und abgerufen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Andere Schnellstartanleitungen und Tutorials in dieser Sammlung bauen auf dieser Schnellstartanleitung auf. Falls Sie mit weiteren Schnellstartanleitungen und Tutorials fortfahren möchten, sollten Sie die Ressourcen nicht bereinigen.
Wenn die Ressourcengruppe und alle zugehörigen Ressourcen nicht mehr benötigt werden, können Sie sie mit dem Befehl [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) entfernen. Die Ressourcen können wie folgt gelöscht werden:

```azurepowershell-interactive
Remove-AzResourceGroup -Name ContosoResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine Key Vault-Instanz erstellt und ein Zertifikat darin gespeichert. Weitere Informationen zu Key Vault und zur Integration in Ihre Anwendungen finden Sie in den folgenden Artikeln:

- [Was ist der Azure-Schlüsseltresor?](../general/overview.md)
- Sehen Sie sich die Referenz zu den [Azure PowerShell-Cmdlets für Key Vault](/powershell/module/az.keyvault/) an.
- [Bewährte Methoden zum Verwenden von Key Vault](../general/best-practices.md)
