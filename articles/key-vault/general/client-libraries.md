---
title: Clientbibliotheken für Azure Key Vault | Microsoft-Dokumentation
description: Clientbibliotheken für Azure Key Vault
services: key-vault
author: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 08/14/2020
ms.author: mbaldwin
ms.openlocfilehash: 34321ac9baf3d3cb43dace33b574cb582c2d72a0
ms.sourcegitcommit: 3246e278d094f0ae435c2393ebf278914ec7b97b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89378949"
---
# <a name="client-libraries-for-azure-key-vault"></a>Clientbibliotheken für Azure Key Vault

Die Clientbibliotheken für Azure Key Vault ermöglichen den programmgesteuerten Zugriff auf Key Vault-Funktionen mit verschiedenen Sprachen, z. B. .NET, Python, Java und JavaScript.

## <a name="client-libraries-per-language-and-object"></a>Clientbibliotheken pro Sprache und Objekt

Jedes SDK verfügt über separate Clientbibliotheken für Geheimnisse, Schlüssel und Zertifikate (siehe Tabelle unten).

| Sprache | Geheimnisse | Schlüssel | Zertifikate |
|--|--|--|--|
| .NET | - [API-Referenz](/dotnet/api/azure.security.keyvault.secrets?view=azure-dotnet)<br>- [NuGet-Paket](https://www.nuget.org/packages/Azure.Security.KeyVault.Secrets/)<br>- [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/keyvault/Azure.Security.KeyVault.Secrets)<br>- [Schnellstart](../secrets/quick-create-net.md) | - [API-Referenz](/dotnet/api/azure.security.keyvault.keys?view=azure-dotnet)<br>- [NuGet-Paket](https://www.nuget.org/packages/Azure.Security.KeyVault.Keys/)<br>- [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/keyvault/Azure.Security.KeyVault.Keys) | - [API-Referenz](/dotnet/api/azure.security.keyvault.certificates?view=azure-dotnet)<br>- [NuGet-Paket](https://www.nuget.org/packages/Azure.Security.KeyVault.Certificates/)<br>- [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/keyvault/Azure.Security.KeyVault.Certificates) |
| Python| - [API-Referenz](/python/api/overview/azure/keyvault-secrets-readme?view=azure-python)<br>- [PyPI-Paket](https://pypi.org/project/azure-keyvault-secrets/)<br>- [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/keyvault/azure-keyvault-secrets)<br>- [Schnellstart](../secrets/quick-create-python.md) |- [API-Referenz](/python/api/overview/azure/keyvault-keys-readme?view=azure-python)<br>- [PyPI-Paket](https://pypi.org/project/azure-keyvault-keys/)<br>- [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/keyvault/azure-keyvault-keys)<br>- [Schnellstart](../keys/quick-create-python.md) | - [API-Referenz](/python/api/overview/azure/keyvault-certificates-readme?view=azure-python)<br>- [PyPI-Paket](https://pypi.org/project/azure-keyvault-certificates/)<br>- [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/keyvault/azure-keyvault-certificates)<br>- [Schnellstart](../certificates/quick-create-python.md) |
| Java | - [API-Referenz](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-security-keyvault-secrets/4.2.0/index.html)<br>- [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/keyvault/azure-security-keyvault-secrets)<br>- [Schnellstart](../secrets/quick-create-java.md) |- [API-Referenz](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-security-keyvault-keys/4.2.0/index.html)<br>- [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/keyvault/azure-security-keyvault-keys) | - [API-Referenz](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-security-keyvault-certificates/4.1.0/index.html)<br>- [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/keyvault/azure-security-keyvault-certificates) |
| Node.js | - [API-Referenz](/javascript/api/@azure/keyvault-secrets/?view=azure-node-latest)<br>- [npm-Paket](https://www.npmjs.com/package/@azure/keyvault-secrest)<br>- [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/keyvault/keyvault-secrets)<br>- [Schnellstart](../secrets/quick-create-node.md) |- [API-Referenz](/javascript/api/@azure/keyvault-keys/?view=azure-node-latest)<br>- [npm-Paket](https://www.npmjs.com/package/@azure/keyvault-keys)<br>- [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/keyvault/keyvault-keys)| - [API-Referenz](/javascript/api/@azure/keyvault-certificates/?view=azure-node-latest)<br>- [npm-Paket](https://www.npmjs.com/package/@azure/keyvault-certificates)<br>- [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/keyvault/keyvault-certificates) |

## <a name="next-steps"></a>Nächste Schritte

- [Entwicklerhandbuch zu Azure-Schlüsseltresor](developers-guide.md)
- Weitere Informationen zum [Authentifizieren bei Key Vault](authentication.md)
