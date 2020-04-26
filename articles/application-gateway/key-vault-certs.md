---
title: TLS-Terminierung mit Azure Key Vault-Zertifikaten
description: Erfahren Sie, wie Sie Azure Application Gateway mit Key Vault für Serverzertifikate integrieren können, die einem HTTPS-fähigen Listener zugeordnet sind.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 4/25/2019
ms.author: victorh
ms.openlocfilehash: 934cf854b0c526ed994c7dc91763f65de64fd14b
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2020
ms.locfileid: "81617505"
---
# <a name="tls-termination-with-key-vault-certificates"></a>TLS-Terminierung mit Key Vault-Zertifikaten

[Azure Key Vault](../key-vault/general/overview.md) ist ein als Plattform verwalteter Geheimnisspeicher, mit dem Sie Geheimnisse, Schlüssel und TLS/SSL-Zertifikate schützen können. Azure Application Gateway unterstützt die Integration mit Key Vault für Serverzertifikate, die HTTPS-fähigen Listenern zugeordnet sind. Diese Unterstützung ist auf die v2-SKU von Application Gateway beschränkt.

Die Key Vault-Integration bietet zwei Modelle für die TLS-Terminierung:

- Sie können explizit TLS/SSL-Zertifikate angeben, die an den Listener angefügt sind. Dies ist das traditionelle Modell für die Übergabe von TLS/SSL-Zertifikaten an Application Gateway für die TLS-Terminierung.
- Sie können bei der Erstellung des HTTPS-fähigen Listeners optional einen Verweis auf ein vorhandenes Key Vault-Zertifikat oder -Geheimnis angeben.

Die Application Gateway-Integration mit Key Vault bietet viele Vorteile. Zu diesen gehören:

- Höhere Sicherheit, da TLS/SSL-Zertifikate nicht direkt vom Anwendungsentwicklungsteam gehandhabt werden. Die Integration ermöglicht einem getrennten Sicherheitsteam Folgendes:
  * Einrichten von Anwendungsgateways
  * Steuern von Application Gateway-Lebenszyklen
  * Erteilen von Berichtigungen für ausgewählte Gateways, damit diese auf Zertifikate zugreifen können, die in Ihrem Schlüsseltresor gespeichert sind
- Unterstützung des Imports vorhandener Zertifikate in Ihren Schlüsseltresor Verwenden von Key Vault-APIs zum Erstellen und Verwalten neuer Zertifikate bei den vertrauenswürdigen Key Vault-Partnern
- Unterstützung für die automatische Verlängerung von Zertifikaten, die in Ihrem Schlüsseltresor gespeichert sind

Application Gateway unterstützt derzeit nur per Software überprüfte Zertifikate. Durch das Hardwaresicherheitsmodul (HSM) überprüfte Zertifikate werden nicht unterstützt. Nachdem Application Gateway für die Verwendung von Key Vault-Zertifikaten konfiguriert wurde, rufen die Instanzen die Zertifikate von Key Vault ab und installieren sie lokal für die TLS-Terminierung. Die Instanzen rufen außerdem Key Vault in 24-Stunden-Intervallen ab, um ggf. eine erneuerte Version des Zertifikats zu erhalten. Wenn ein aktualisiertes Zertifikat gefunden wird, wird das derzeit dem HTTPS-Listener zugeordnete TLS/SSL-Zertifikat automatisch rotiert.

> [!NOTE]
> Das Azure-Portal unterstützt nur KeyVault-Zertifikate und keine Geheimnisse. Application Gateway unterstützt weiterhin die Referenzierung von Geheimnissen aus KeyVault, aber nur über Nicht-Portalressourcen wie PowerShell, CLI, API, ARM-Vorlagen usw. 

## <a name="how-integration-works"></a>Funktionsweise der Integration

Die Application Gateway-Integration in Key Vault erfolgt über eine Konfiguration in drei Schritten:

1. **Erstellen einer benutzerseitig zugewiesenen verwalteten Identität**

   Sie erstellen eine benutzerseitig zugewiesene verwaltete Identität oder verwenden eine bereits vorhandenen, über die Application Gateway in Ihrem Namen Zertifikate von Key Vault abruft. Weitere Informationen finden Sie im unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../active-directory/managed-identities-azure-resources/overview.md). In diesem Schritt wird eine neue Identität im Azure Active Directory-Mandanten erstellt. Die Identität wird vom Abonnement, das zum Erstellen der Identität verwendet wird, als vertrauenswürdig eingestuft.

1. **Konfigurieren Ihres Schlüsseltresors**

   Sie importieren dann entweder ein vorhandenes Zertifikat oder erstellen ein neues in Ihrem Schlüsseltresor. Das Zertifikat wird von Anwendungen verwendet, die über das Anwendungsgateway ausgeführt werden. In diesem Schritt kann auch ein Key Vault-Geheimnis verwendet werden, das als base-64-codierte PFX-Datei ohne Kennwort gespeichert ist. Die Verwendung eines Zertifikattyps wird empfohlen, da für Zertifikattypobjekte im Schlüsseltresor Funktionen für die automatische Verlängerung zur Verfügung stehen. Nach dem Erstellen eines Zertifikats oder Geheimnisses definieren Sie Zugriffsrichtlinien im Schlüsseltresor, durch die der Identität der *get*-Zugriff zum Erhalten des Geheimnisses gewährt werden kann.
   
   > [!NOTE]
   > Wenn Sie das Application Gateway über eine ARM-Vorlage bereitstellen, entweder mithilfe der Azure CLI oder PowerShell, oder über eine im Azure-Portal bereitgestellte Azure-Anwendung, muss das SSL-Zertifikat, das im Schlüsseltresor als base-64-codierte PFX-Datei gespeichert ist, **kennwortlos** sein. Außerdem müssen Sie die unter [Verwenden von Azure Key Vault zum Übergeben eines sicheren Parameterwerts während der Bereitstellung](../azure-resource-manager/templates/key-vault-parameter.md) aufgeführten Schritte durchführen. Es ist besonders wichtig, `enabledForTemplateDeployment` auf `true` festzulegen.

1. **Konfigurieren der Application Gateway-Instanz**

   Nachdem Sie die beiden vorherigen Schritte abgeschlossen haben, können Sie ein vorhandenes Anwendungsgateway einrichten oder ändern, um die vom Benutzer zugewiesene verwaltete Identität zu verwenden. Sie können auch das TLS/SSL-Zertifikat des HTTP-Listeners so konfigurieren, dass es auf den vollständigen URI des Key Vault-Zertifikats oder der Geheimnis-ID zeigt.

   ![Key Vault-Zertifikate](media/key-vault-certs/ag-kv.png)

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren von TLS-Terminierung mit Key Vault-Zertifikaten mithilfe von Azure PowerShell](configure-keyvault-ps.md)
