---
title: Entwicklerhandbuch zu Azure-Schlüsseltresor
description: Mit Azure-Schlüsseltresor können Entwickler kryptografische Schlüssel in der Microsoft Azure-Umgebung verwalten.
services: key-vault
author: msmbaldwin
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.date: 10/05/2020
ms.author: mbaldwin
ms.openlocfilehash: a04435b1e2feb537231bb80d2777b9ea2599c241
ms.sourcegitcommit: 5abc3919a6b99547f8077ce86a168524b2aca350
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2020
ms.locfileid: "91812402"
---
# <a name="azure-key-vault-developers-guide"></a>Entwicklerhandbuch zu Azure-Schlüsseltresor

Mithilfe des Schlüsseltresors können Sie in Ihren Anwendungen sicher auf vertrauliche Daten zugreifen:

- Schlüssel, Geheimnisse und Zertifikate werden geschützt, ohne dass Sie den Code selbst schreiben müssen, und können auf einfache Weise in Ihren Anwendungen verwendet werden.
- Ihre Kunden können die Verantwortung für ihre eigenen Schlüssel, Geheimnisse und Zertifikate übernehmen und diese selbst verwalten, sodass Sie sich auf die Bereitstellung der Hauptfunktionen der Software konzentrieren können. Auf diese Weise müssen Ihre Anwendungen weder die Verantwortung noch die Haftung für die Mandantenschlüssel, Geheimnisse und Zertifikate Ihrer Kunden übernehmen.
- Ihre Anwendung kann Signatur- und Verschlüsselungsschlüssel verwenden, wobei die Schlüssel außerhalb der Anwendung verwaltet werden. Weitere Informationen zu Schlüsseln finden Sie unter [Informationen zu Schlüsseln](../keys/about-keys.md).
- Sie können Anmeldeinformationen wie Kennwörter, Zugriffsschlüssel und SAS-Token verwalten, die Sie in Key Vault als Geheimnisse speichern. Informationen dazu finden Sie unter [Informationen zu Schlüsseln](../secrets/about-secrets.md).
- Verwalten Sie Zertifikate. Weitere Informationen finden Sie unter [Informationen zu Zertifikaten](../certificates/about-certificates.md).

Allgemeine Informationen zu Azure Key Vault finden Sie unter [Was ist der Schlüsseltresor?](overview.md).

## <a name="public-previews"></a>Öffentliche Vorschauversionen

Wir veröffentlichen in regelmäßigen Abständen eine öffentliche Vorschau des neuen Key Vault-Features. Testen Sie die Funktionen der öffentlichen Vorschau, und teilen Sie uns Ihre Meinung über die folgende E-Mail-Adresse für Feedback mit: azurekeyvault@microsoft.com.

## <a name="creating-and-managing-key-vaults"></a>Erstellen und Verwalten von Schlüsseltresoren

Key Vault-Verwaltung erfolgt ähnlich wie bei anderen Azure-Diensten über den Azure Resource Manager-Dienst. Azure Resource Manager ist der Bereitstellungs- und Verwaltungsdienst für Azure. Er bietet eine Verwaltungsebene, die das Erstellen, Aktualisieren und Löschen von Ressourcen in Ihrem Azure-Konto ermöglicht. Weitere Informationen finden Sie unter [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/management/overview).

Der Zugriff auf die Verwaltungsschicht wird durch die [rollenbasierte Zugriffssteuerung von Azure (Azure RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview) gesteuert. In Key Vault können Sie mit der Verwaltungsschicht, auch als Verwaltungs- oder Steuerungsebene bezeichnet, Schlüsseltresore und zugehörige Attribute erstellen und verwalten, einschließlich Zugriffsrichtlinien, aber keine Schlüssel, Geheimnisse und Zertifikate, die auf der Datenebene verwaltet werden. Sie können eine vordefinierte `Key Vault Contributor`-Rolle verwenden, um Verwaltungszugriff auf Key Vault zu gewähren.     

**APIs und SDKs für die Key Vault-Verwaltung:**

| Azure CLI | PowerShell | REST-API | Ressourcen-Manager | .NET | Python | Java | JavaScript |  
|--|--|--|--|--|--|--|--|
|[Verweis](/cli/azure/keyvault)<br>[Schnellstart](quick-create-cli.md)|[Verweis](/powershell/module/az.keyvault)<br>[Schnellstart](quick-create-powershell.md)|[Referenz](/rest/api/keyvault/)|[Referenz](/azure/templates/microsoft.keyvault/vaults)|[Referenz](/dotnet/api/microsoft.azure.management.keyvault)|[Referenz](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault)|[Referenz](/java/api/com.microsoft.azure.management.keyvault)|[Referenz](/javascript/api/@azure/arm-keyvault)|

Weitere Informationen zu Installationspaketen und Quellcode finden Sie unter [Clientbibliotheken](client-libraries.md).

Weitere Informationen zur Key Vault-Verwaltungsebene finden Sie unter [Key Vault-Verwaltungsebene](https://docs.microsoft.com/azure/key-vault/general/secure-your-key-vault#management-plane-and-azure-rbac).

## <a name="authenticate-to-key-vault-in-code"></a>Authentifizieren bei Key Vault mit Code

Key Vault verwendet Azure AD-Authentifizierung, die den Azure AD-Sicherheitsprinzipal erfordert, um Zugriff zu gewähren. Ein Azure AD-Sicherheitsprinzipal kann ein Benutzer, ein Anwendungsdienstprinzipal, eine [verwaltete Identität für Azure-Ressourcen](../../active-directory/managed-identities-azure-resources/overview.md) oder eine Gruppe eines beliebigen Typs von Sicherheitsprinzipalen sein.

### <a name="authentication-best-practices"></a>Bewährte Methoden für die Authentifizierung
Es wird empfohlen, die verwaltete Identität für in Azure bereitgestellte Anwendungen zu verwenden. Wenn Sie Azure-Dienste verwenden, die keine verwaltete Identität unterstützen, oder wenn Anwendungen lokal bereitgestellt werden, ist ein [Dienstprinzipal mit einem Zertifikat](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal) eine mögliche Alternative. In diesem Szenario sollte das Zertifikat in Key Vault gespeichert und häufig rotiert werden. Ein Dienstprinzipal mit Geheimnis kann für Entwicklungs- und Testumgebungen verwendet werden. Lokal oder in Cloud Shell wird die Verwendung eines Benutzerprinzipals empfohlen.

Die oben beschriebenen Authentifizierungsszenarien werden von der Azure Identity-Clientbibliothek unterstützt und in Key Vault SDKs integriert. Die Azure Identity-Bibliothek kann in verschiedenen Umgebungen und auf verschiedenen Plattformen verwendet werden, ohne den Code zu ändern. Azure Identity ruft auch automatisch das Authentifizierungstoken von bei Azure angemeldeten Benutzern mit der Azure CLI, Visual Studio, Visual Studio Code und anderen Anwendungen ab. 

Weitere Informationen finden Sie unter 

| .NET | Python | Java | JavaScript |
|--|--|--|--|
|[Azure Identity SDK .NET](https://docs.microsoft.com/dotnet/api/overview/azure/identity-readme)|[Azure Identity SDK Python](https://docs.microsoft.com/python/api/overview/azure/identity-readme)|[Azure Identity SDK Java](https://docs.microsoft.com/java/api/overview/azure/identity-readme)|[Azure Identity SDK JavaScript](https://docs.microsoft.com/javascript/api/overview/azure/identity-readme)|     

Authentifizieren bei Key Vault in Anwendungen:
- [Authentifizieren bei Key Vault in einer in der VM gehosteten Anwendung in .NET](https://docs.microsoft.com/azure/key-vault/general/tutorial-net-virtual-machine)
- [Authentifizieren bei Key Vault in einer in der VM gehosteten Anwendung in Python](https://docs.microsoft.com/azure/key-vault/general/tutorial-python-virtual-machine)
- [Authentifizieren bei Key Vault mit App Service](https://docs.microsoft.com/azure/key-vault/general/tutorial-net-create-vault-azure-web-app)

## <a name="manage-keys-certificates-and-secrets"></a>Verwalten von Schlüsseln, Zertifikaten und Geheimnissen

Der Zugriff auf Schlüssel, Geheimnisse und Zertifikate wird durch die Datenebene gesteuert. Die Zugriffssteuerung der Datenebene kann mithilfe von Zugriffsrichtlinien für lokale Tresore oder RBAC (Vorschau) erfolgen.

**Schlüssel-APIs und SDKs**


| Azure CLI | PowerShell | REST-API | Ressourcen-Manager | .NET | Python | Java | JavaScript |  
|--|--|--|--|--|--|--|--|
|[Verweis](/cli/azure/keyvault/key)<br>[Schnellstart](../keys/quick-create-cli.md)|[Verweis](/powershell/module/az.keyvault/)<br>[Schnellstart](../keys/quick-create-powershell.md)|[Verweis](/rest/api/keyvault/#key-operations)|–|[Referenz](/dotnet/api/azure.security.keyvault.keys)|[Referenz](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault)<br>[Schnellstart](../keys/quick-create-python.md)|[Referenz](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-security-keyvault-keys/4.2.0/index.html)|[Referenz](/javascript/api/@azure/keyvault-keys/)|

**Zertifikat-APIs and SDKs**


| Azure CLI | PowerShell | REST-API | Ressourcen-Manager | .NET | Python | Java | JavaScript |  
|--|--|--|--|--|--|--|--|
|[Verweis](/cli/azure/keyvault/certificate)<br>[Schnellstart](../certificates/quick-create-cli.md)|[Verweis](/powershell/module/az.keyvault)<br>[Schnellstart](../certificates/quick-create-powershell.md)|[Verweis](/rest/api/keyvault/#certificate-operations)|–|[Referenz](/dotnet/api/azure.security.keyvault.certificates)|[Referenz](/python/api/overview/azure/keyvault-certificates-readme)<br>[Schnellstart](../certificates/quick-create-python.md)|[Referenz](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-security-keyvault-certificates/4.1.0/index.html)|[Referenz](/javascript/api/@azure/keyvault-certificates/)|

**Geheimnis-APIs und SDKs**


| Azure CLI | PowerShell | REST-API | Ressourcen-Manager | .NET | Python | Java | JavaScript |  
|--|--|--|--|--|--|--|--|
|[Verweis](/cli/azure/keyvault/secret)<br>[Schnellstart](../secrets/quick-create-cli.md)|[Verweis](/powershell/module/az.keyvault/)<br>[Schnellstart](../secrets/quick-create-powershell.md)|[Referenz](/rest/api/keyvault/#secret-operations)|[Referenz](/azure/templates/microsoft.keyvault/vaults/secrets)<br>[Schnellstart](../secrets/quick-create-template.md)|[Verweis](/dotnet/api/azure.security.keyvault.secrets)<br>[Schnellstart](../secrets/quick-create-net.md)|[Verweis](/python/api/overview/azure/keyvault-secrets-readme)<br>[Schnellstart](../secrets/quick-create-python.md)|[Verweis](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-security-keyvault-secrets/4.2.0/index.html)<br>[Schnellstart](../secrets/quick-create-java.md)|[Verweis](/javascript/api/@azure/keyvault-secrets/)<br>[Schnellstart](../secrets/quick-create-node.md)|

Weitere Informationen zu Installationspaketen und Quellcode finden Sie unter [Clientbibliotheken](client-libraries.md).

Weitere Informationen zur Sicherheit der Key Vault-Datenebene finden Sie unter [Key Vault-Datenebene und Zugriffsrichtlinien](https://docs.microsoft.com/azure/key-vault/general/secure-your-key-vault#data-plane-and-access-policies) und [Key Vault-Datenebene und RBAC (Vorschau)](https://docs.microsoft.com/azure/key-vault/general/secure-your-key-vault#data-plane-and-azure-rbac-preview).

### <a name="code-examples"></a>Codebeispiele

Vollständige Beispiele für die Verwendung von Key Vault mit Ihren Anwendungen finden Sie hier:

- [Azure Key Vault-Codebeispiele](https://azure.microsoft.com/resources/samples/?service=key-vault) – Codebeispiele für Azure Key Vault. 

## <a name="how-tos"></a>Vorgehensweisen

Die folgenden Artikel und Szenarien bieten aufgabenspezifische Anleitungen für die Arbeit mit Azure Key Vault:

- [Zugreifen auf Schlüsseltresore hinter der Firewall:](access-behind-firewall.md) Für den Zugriff auf einen Schlüsseltresor muss Ihre Schlüsseltresor-Clientanwendung auf mehrere Endpunkte für unterschiedliche Funktionen zugreifen können.
- Bereitstellen von Zertifikaten für VMs aus Key Vault – [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/key-vault-windows), [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/key-vault-linux): Eine in einer VM in Azure ausgeführte Cloudanwendung benötigt ein Zertifikat. Wie wird dieses Zertifikat derzeit für eine solche VM bereitgestellt?
- [Bereitstellen eines Azure-Web-App-Zertifikats über Key Vault](https://docs.microsoft.com/azure/app-service/configure-ssl-certificate#import-a-certificate-from-key-vault)
- Weisen Sie ein Zugriffsrichtlinie zu ([CLI](assign-access-policy-cli.md) | [PowerShell](assign-access-policy-powershell.md) | [Portal](assign-access-policy-portal.md)). 
- [Verwenden des vorläufigen Löschens in Key Vault mit der CLI](soft-delete-cli.md) enthält Informationen zur Verwendung und zum Lebenszyklus eines Schlüsseltresors und verschiedener Schlüsseltresorobjekte, für den bzw. die vorläufiges Löschen aktiviert ist.
- [Übergeben sicherer Werte während der Bereitstellung:](../../azure-resource-manager/templates/key-vault-parameter.md) Wenn Sie während der Bereitstellung einen sicheren Wert (z.B. ein Kennwort) als Parameter übergeben müssen, können Sie diesen Wert als Geheimnis in Azure Key Vault speichern und in anderen Resource Manager-Vorlagen auf ihn verweisen.

## <a name="integrated-with-key-vault"></a>Key Vault-Integration

Diese Artikel enthalten Informationen zu anderen Szenarien und Diensten, in denen Key Vault oder eine Key Vault-Integration zum Einsatz kommt.

- Die [Verschlüsselung ruhender Daten](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest) ermöglicht die Codierung (Verschlüsselung) von Daten, sobald diese gespeichert werden. Datenverschlüsselungsschlüssel werden häufig mit einem Schlüssel für die Schlüsselverschlüsselung in Azure Key Vault verschlüsselt, um den Zugriff weiter einzuschränken.
- [Azure Information Protection](/azure/information-protection/plan-implement-tenant-key) ermöglicht Ihnen, Ihren eigenen Mandantenschlüssel zu verwalten. Anstatt z.B. Ihren Mandantenschlüssel von Microsoft verwalten zu lassen (Standard), können Sie Ihren eigenen Mandantenschlüssel verwalten, um bestimmte Vorschriften zu erfüllen, die in Ihrer Organisation gelten. Das Verwalten Ihres eigenen Mandantenschlüssels wird auch als „Bring Your Own Key“ (BYOK) bezeichnet.
- Mit dem [Azure Private Link-Dienst](private-link-service.md) können Sie über einen privaten Endpunkt in Ihrem virtuellen Netzwerk auf Azure-Dienste wie Azure Key Vault, Azure Storage und Azure Cosmos DB sowie auf in Azure gehostete Kunden-/Partnerdienste zugreifen.
- Durch die Integration von Key Vault in [Event Grid](https://docs.microsoft.com/azure/event-grid/event-schema-key-vault) können Benutzer benachrichtigt werden, wenn sich der Status eines im Schlüsseltresor gespeicherten Geheimnisses geändert hat. Sie können eine neue Version von Geheimnissen an Anwendungen verteilen oder Geheimnisse rotieren, die bald ablaufen, um Ausfälle zu verhindern.
- Sie können Ihre [Azure Devops](https://docs.microsoft.com/azure/devops/pipelines/release/azure-key-vault)-Geheimnisse vor unerwünschtem Zugriff in Key Vault schützen.
- [Verwenden des in Key Vault in DataBricks gespeicherten Geheimnisses, um eine Verbindung mit Azure Storage herzustellen](https://docs.microsoft.com/azure/key-vault/general/integrate-databricks-blob-storage)
- Konfigurieren und Ausführen des Azure Key Vault-Anbieters für den [Secrets Store CSI-Treiber](https://docs.microsoft.com/azure/key-vault/general/key-vault-integrate-kubernetes) unter Kubernetes

## <a name="key-vault-overviews-and-concepts"></a>Key Vault-Übersichten und -Konzepte

- [Übersicht über die Azure Key Vault-Funktion für vorläufiges Löschen](soft-delete-overview.md) beschreibt ein Feature, dass die Wiederherstellung gelöschter Objekte unabhängig davon ermöglicht, ob die Löschung versehentlich oder absichtlich erfolgte.
- [Anleitung zur Drosselung von Azure Key Vault](overview-throttling.md) informiert Sie über die grundlegenden Konzepte der Drosselung und bietet einen Ansatz für Ihre App.
- [Sicherheitsumgebungen und geografische Grenzen von Azure Key Vault](overview-security-worlds.md) beschreibt die Beziehungen zwischen Regionen und Sicherheitsbereichen.

## <a name="social"></a>Soziale Netzwerke

- [Key Vault Blog](https://aka.ms/kvblog)
- [Key Vault Forum](https://aka.ms/kvforum)
