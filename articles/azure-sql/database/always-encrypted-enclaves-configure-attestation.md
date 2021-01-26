---
title: Konfigurieren von Azure Attestation für Ihren logischen Azure SQL-Server
description: Konfigurieren von Azure Attestation für Always Encrypted mit Secure Enclaves in Azure SQL-Datenbank
keywords: Verschlüsseln von Daten, SQL-Verschlüsselung, Datenbankverschlüsselung, vertrauliche Daten, Always Encrypted, Secure Enclaves, SGX, Nachweis
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.devlang: ''
ms.topic: how-to
author: jaszymas
ms.author: jaszymas
ms.reviwer: vanto
ms.date: 01/15/2021
ms.openlocfilehash: e8cb423d4d700c4b6b6caa30a02eac3e7ef10cb6
ms.sourcegitcommit: 25d1d5eb0329c14367621924e1da19af0a99acf1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2021
ms.locfileid: "98253241"
---
# <a name="configure-azure-attestation-for-your-azure-sql-logical-server"></a>Konfigurieren von Azure Attestation für Ihren logischen Azure SQL-Server

[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

> [!NOTE]
> Always Encrypted mit Secure Enclaves für Azure SQL-Datenbank befindet sich derzeit in der **öffentlichen Vorschau**.

[Microsoft Azure Attestation](../../attestation/overview.md) ist eine Lösung für Trusted Execution Environment-Nachweise (TEE), einschließlich Intel Software Guard Extensions-Enclaves (Intel SGX). 

Um Azure Attestation für Nachweise von Intel SGX-Enclaves, die für [Always Encrypted mit Secure Enclaves](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-enclaves) verwendet werden, in Azure SQL-Datenbank zu verwenden, führen Sie folgende Schritte aus:

1. Erstellen Sie einen [Nachweisanbieter](../../attestation/basic-concepts.md#attestation-provider), und konfigurieren Sie ihn mit der empfohlenen Nachweisrichtlinie.

2. Gewähren Sie dem logischen Azure SQL-Server Zugriff auf Ihren Nachweisanbieter.

> [!NOTE]
> Für die Konfiguration von Nachweisen ist der Nachweisadministrator verantwortlich. Weitere Informationen finden Sie unter [Rollen und Verantwortlichkeiten beim Konfigurieren von SGX-Enclaves und von Attestation](always-encrypted-enclaves-plan.md#roles-and-responsibilities-when-configuring-sgx-enclaves-and-attestation).

## <a name="requirements"></a>Anforderungen

Der logische Azure SQL-Server und der Nachweisanbieter müssen demselben Azure Active Directory-Mandanten angehören. Mandantenübergreifende Interaktionen werden nicht unterstützt. 

Dem logischen Azure SQL-Server muss eine Azure AD-Identität zugewiesen sein. Als Nachweisadministrator müssen Sie die Azure AD-Identität des Servers vom Azure SQL-Datenbankadministrator für diesen Server erfragen. Sie verwenden die Identität, um dem Server Zugriff auf den Nachweisanbieter zu gewähren. 

Anweisungen zum Erstellen eines Servers mit einer Identität oder zum Zuweisen einer Identität zu einem vorhandenen Server mithilfe von PowerShell und der Azure CLI finden Sie unter [Zuweisen einer Azure AD-Identität zu einem Server](transparent-data-encryption-byok-configure.md#assign-an-azure-active-directory-azure-ad-identity-to-your-server).

## <a name="create-and-configure-an-attestation-provider"></a>Erstellen und Konfigurieren eines Nachweisanbieters

Bei einem [Nachweisanbieter](../../attestation/basic-concepts.md#attestation-provider) handelt es sich um eine Ressource in Azure Attestation, mit deren Hilfe [Nachweisanforderungen](../../attestation/basic-concepts.md#attestation-request) anhand von [Nachweisrichtlinien](../../attestation/basic-concepts.md#attestation-request) ausgewertet und [Nachweistoken](../../attestation/basic-concepts.md#attestation-token) ausgestellt werden. 

Nachweisrichtlinien werden mithilfe der [Anspruchsregelgrammatik](../../attestation/claim-rule-grammar.md) angegeben.

Microsoft empfiehlt die folgende Richtlinie zum Nachweisen von Intel SGX-Enclaves, die für Always Encrypted in Azure SQL-Datenbank verwendet werden:

```output
version= 1.0;
authorizationrules 
{
       [ type=="x-ms-sgx-is-debuggable", value==false ]
        && [ type=="x-ms-sgx-product-id", value==4639 ]
        && [ type=="x-ms-sgx-svn", value>= 0 ]
        && [ type=="x-ms-sgx-mrsigner", value=="e31c9e505f37a58de09335075fc8591254313eb20bb1a27e5443cc450b6e33e5"] 
    => permit();
};
```

Mit der oben genannten Richtlinie wird Folgendes überprüft:

- Die Enclave in Azure SQL-Datenbank unterstützt kein Debuggen (wodurch der von der Enclave gebotene Schutz verringert würde).
- Die Produkt-ID der Bibliothek innerhalb der Enclave entspricht der Produkt-ID, die Always Encrypted mit Secure Enclaves (4639) zugewiesen ist.
- Die Versions-ID (svn) der Bibliothek ist größer als 0 (null).
- Die Bibliothek in der Enclave wurde mit dem Microsoft-Signaturschlüssel signiert (der Wert des x-ms-sgx-mrsigner-Anspruchs entspricht dem Hash des Signaturschlüssels).

> [!IMPORTANT]
> Ein Nachweisanbieter wird mit der Standardrichtlinie für Intel SGX-Enclaves erstellt, wodurch der innerhalb der Enclave ausgeführte Code nicht überprüft wird. Microsoft empfiehlt dringend, für Always Encrypted mit Secure Enclaves die oben empfohlene Richtlinie festzulegen und nicht die Standardrichtlinie zu verwenden.

Anweisungen zum Erstellen eines Nachweisanbieters und Konfigurieren des Anbieters mit einer Nachweisrichtlinie unter Verwendung folgender Optionen:

- [Schnellstart: Einrichten von Azure Attestation über das Azure-Portal](../../attestation/quickstart-portal.md)
    > [!IMPORTANT]
    > Wenn Sie die Nachweisrichtlinie über das Azure-Portal konfigurieren, legen Sie den Nachweistyp auf `SGX-IntelSDK` fest.
- [Schnellstart: Einrichten von Azure Attestation mithilfe von Azure PowerShell](../../attestation/quickstart-powershell.md)
    > [!IMPORTANT]
    > Wenn Sie die Nachweisrichtlinie mit Azure PowerShell konfigurieren, legen Sie den Parameter `Tee` auf `SgxEnclave` fest.
- [Schnellstart: Einrichten von Azure Attestation mithilfe der Azure CLI](../../attestation/quickstart-azure-cli.md)
    > [!IMPORTANT]
    > Wenn Sie die Nachweisrichtlinie mit der Azure CLI konfigurieren, legen Sie den Parameter `attestation-type` auf `SGX-IntelSDK` fest.

## <a name="determine-the-attestation-url-for-your-attestation-policy"></a>Ermitteln der Nachweis-URL für die Nachweisrichtlinie

Nachdem Sie eine Nachweisrichtlinie konfiguriert haben, müssen Sie die Nachweis-URL, die auf die Richtlinie verweist, für Administratoren von Anwendungen freigeben, die Always Encrypted mit Secure Enclaves in Azure SQL-Datenbank verwenden. Anwendungsadministratoren und/oder Benutzer von Anwendungen müssen ihre Apps mit der Nachweis-URL konfigurieren, damit sie Anweisungen ausführen können, die Secure Enclaves nutzen.

### <a name="use-powershell-to-determine-the-attestation-url"></a>Ermitteln der Nachweis-URL mithilfe von PowerShell

Verwenden Sie das folgende Skript, um die Nachweis-URL zu ermitteln:

```powershell
$attestationProvider = Get-AzAttestation -Name $attestationProviderName -ResourceGroupName $attestationResourceGroupName 
$attestationUrl = $attestationProvider.AttestUri + “/attest/SgxEnclave”
Write-Host "Your attestation URL is: " $attestationUrl 
```

### <a name="use-azure-portal-to-determine-the-attestation-url"></a>Ermitteln der Nachweis-URL über das Azure-Portal

1. Kopieren Sie im Bereich „Übersicht“ Ihres Nachweisanbieters den Wert der Eigenschaft für den Nachweis-URI in die Zwischenablage. Ein Nachweis-URI sollte wie folgt aussehen: `https://MyAttestationProvider.us.attest.azure.net`.

2. Fügen Sie die folgende Zeichenfolge an den Nachweis-URI an: `/attest/SgxEnclave`. 

Die resultierende Nachweis-URL sollte wie folgt aussehen: `https://MyAttestationProvider.us.attest.azure.net/attest/SgxEnclave`

## <a name="grant-your-azure-sql-logical-server-access-to-your-attestation-provider"></a>So gewähren Sie dem logischen Azure SQL-Server Zugriff auf den Nachweisanbieter

Während des Nachweisworkflows ruft der logische Azure SQL-Server, der Ihre Datenbank enthält, den Nachweisanbieter auf, um eine Nachweisanforderung zu übermitteln. Damit der logische Azure SQL-Server Nachweisanforderungen übermitteln kann, muss der Server berechtigt sein, die Aktion `Microsoft.Attestation/attestationProviders/attestation/read` für den Nachweisanbieter auszuführen. Die empfohlene Vorgehensweise, um die Berechtigung zu gewähren, besteht darin, dass der Administrator des Nachweisanbieters die Azure AD-Identität des Servers der Rolle „Nachweisleser“ für den Nachweisanbieter oder seine enthaltende Ressourcengruppe zuweist.

### <a name="use-azure-portal-to-assign-permission"></a>Zuweisen der Berechtigung über das Azure-Portal

Um die Identität eines Azure SQL-Servers der Rolle „Nachweisleser“ für einen Nachweisanbieter zuzuweisen, befolgen Sie die allgemeinen Anweisungen unter [Hinzufügen oder Entfernen von Azure-Rollenzuweisungen über das Azure-Portal](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal). Gehen Sie im Bereich **Rollenzuweisung hinzufügen** wie folgt vor:

1. Wählen Sie in der Dropdownliste **Rolle** die Rolle **Nachweisleser** aus.
1. Geben Sie im Feld **Auswählen** den Namen Ihres Azure SQL-Servers ein, um ihn zu suchen.

Ein Beispiel finden Sie im folgenden Screenshot.

![Zuweisen der Rolle „Nachweisleser“](./media/always-encrypted-enclaves/attestation-provider-role-assigment.png)

> [!NOTE]
> Damit im Bereich **Rollenzuweisung hinzufügen** ein Server angezeigt wird, muss dem Server eine Azure AD-Identität zugewiesen sein. Informationen hierzu finden Sie unter [Anforderungen](#requirements).

### <a name="use-powershell-to-assign-permission"></a>Zuweisen der Berechtigung mithilfe von PowerShell

1. Suchen Sie Ihren logischen Azure SQL-Server.

```powershell
$serverResourceGroupName = "<server resource group name>"
$serverName = "<server name>" 
$server = Get-AzSqlServer -ServerName $serverName -ResourceGroupName
```
 
2. Weisen Sie den Server für die Ressourcengruppe, die den Nachweisanbieter enthält, der Rolle „Nachweisleser“ zu.

```powershell
$attestationResourceGroupName = "<attestation provider resource group name>"
New-AzRoleAssignment -ObjectId $server.Identity.PrincipalId -RoleDefinitionName "Attestation Reader" -ResourceGroupName $attestationResourceGroupName
```

Weitere Informationen finden Sie unter [Hinzufügen oder Entfernen von Azure-Rollenzuweisungen mithilfe der Azure PowerShell](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell#add-a-role-assignment).

## <a name="next-steps"></a>Nächste Schritte

- [Verwalten von Schlüsseln für Always Encrypted mit Secure Enclaves](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-enclaves-manage-keys)

## <a name="see-also"></a>Weitere Informationen

- [Tutorial: Erste Schritte mit Always Encrypted mit Secure Enclaves in Azure SQL-Datenbank](always-encrypted-enclaves-getting-started.md)