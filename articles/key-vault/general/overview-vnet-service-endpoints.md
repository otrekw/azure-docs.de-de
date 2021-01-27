---
title: VNET-Dienstendpunkte für Azure Key Vault
description: Hier wird einschließlich Verwendungsszenarios erläutert, wie Ihnen die VNET-Dienstendpunkte für Azure Key Vault das Beschränken des Zugriffs auf angegebene virtuelle Netzwerke ermöglichen.
services: key-vault
author: amitbapat
ms.author: ambapat
manager: rkarlin
ms.date: 01/02/2019
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.openlocfilehash: 4a817f386d68c144968540dd05f3fa6859bb0acc
ms.sourcegitcommit: 75041f1bce98b1d20cd93945a7b3bd875e6999d0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2021
ms.locfileid: "98704168"
---
# <a name="virtual-network-service-endpoints-for-azure-key-vault"></a>VNET-Dienstendpunkte für Azure Key Vault

Die VNET-Dienstendpunkte für Azure Key Vault ermöglichen Ihnen, den Zugriff auf angegebene virtuelle Netzwerke zu beschränken. Die Endpunkte ermöglichen Ihnen außerdem, den Zugriff auf eine Liste von IPv4-Adressbereichen (Internet Protocol, Version 4) zu beschränken. Allen Benutzern, die außerhalb dieser Quellen eine Verbindung mit Ihrem Schlüsseltresor herstellen, wird der Zugriff verweigert.

Es gibt eine wichtige Ausnahme dieser Einschränkung. Wenn ein Benutzer entschieden hat, vertrauenswürdige Microsoft-Dienste zuzulassen, dürfen Verbindungen von diesen Diensten die Firewall passieren. Z. B. umfassen diese Dienste Office 365 Exchange Online, Office 365 SharePoint Online, Azure Compute, Azure Resource Manager und Azure Backup. Solche Benutzer müssen natürlich weiterhin ein gültiges Azure Active Directory-Token vorlegen und über Berechtigungen (als Zugriffsrichtlinien konfiguriert) zum Ausführen des angeforderten Vorgangs verfügen. Weitere Informationen finden Sie unter [VNET-Dienstendpunkte](../../virtual-network/virtual-network-service-endpoints-overview.md).

## <a name="usage-scenarios"></a>Verwendungsszenarios

Sie können [Key Vault-Firewalls und virtuelle Netzwerke](network-security.md) so konfigurieren, dass der Zugriff auf Datenverkehr aus allen Netzwerken (einschließlich Internetdatenverkehr) standardmäßig verweigert wird. Sie können Zugriff auf Datenverkehr aus bestimmten virtuellen Azure-Netzwerken und IP-Adressbereichen des öffentlichen Internets gewähren, sodass Sie eine sichere Netzwerkgrenze für Ihre Anwendungen erstellen können.

> [!NOTE]
> Key Vault-Firewalls und VNET-Regeln gelten nur für die [Datenebene](secure-your-key-vault.md#data-plane-access-control) von Key Vault. Vorgänge auf Key Vault-Steuerungsebene (z. B. Vorgänge zum Erstellen, Löschen und Ändern, das Festlegen von Zugriffsrichtlinien, Festlegen von Firewalls und VNET-Regeln) sind von Firewalls und VNET-Regeln nicht betroffen.

Hier finden Sie einige Beispiele dafür, wie Sie Dienstendpunkte verwenden können:

* Sie verwenden Key Vault zum Speichern von Verschlüsselungsschlüsseln, Anwendungsgeheimnissen und Zertifikaten, und Sie möchten den Zugriff auf Ihren Schlüsseltresor aus dem öffentlichen Internet blockieren.
* Sie möchten den Zugriff auf Ihren Schlüsseltresor sperren, sodass nur Ihre Anwendung oder einige festgelegte Hosts eine Verbindung mit dem Schlüsselspeicher herstellen können.
* Sie führen eine Anwendung in Ihrem virtuellen Azure-Netzwerk aus, und dieses virtuelle Netzwerk ist für den gesamten eingehenden und ausgehenden Datenverkehr gesperrt. Ihre Anwendung muss dennoch weiterhin eine Verbindung mit dem Schlüsseltresor herstellen, um Geheimnisse abzurufen oder Kryptografieschlüssel zu verwenden.

## <a name="trusted-services"></a>Vertrauenswürdige Dienste

Es folgt eine Liste der vertrauenswürdigen Dienste, denen Zugriff auf einen Schlüsseltresor gewährt wird, wenn die Option **Vertrauenswürdige Dienste zulassen** aktiviert ist.

|Vertrauenswürdiger Dienst|Unterstützte Verwendungsszenarien|
| --- | --- |
|Azure Virtual Machines-Bereitstellung (Dienst)|[Bereitstellen von Zertifikaten auf virtuellen Computern über eine vom Kunden verwaltete Key Vault-Instanz](/archive/blogs/kv/updated-deploy-certificates-to-vms-from-customer-managed-key-vault).|
|Azure Resource Manager-Vorlagenbereitstellung (Dienst)|[Übergeben sicherer Werte während der Bereitstellung](../../azure-resource-manager/templates/key-vault-parameter.md).|
|Azure Application Gateway V2-SKU|[TLS-Terminierung mit Key Vault-Zertifikaten](../../application-gateway/key-vault-certs.md)|
|Azure Disk Encryption-Volumeverschlüsselung (Dienst)|Zulassen des Zugriffs auf den BitLocker-Schlüssel (virtueller Windows-Computer) oder die DM-Passphrase (virtueller Linux-Computer) und den Schlüssel für die Schlüsselverschlüsselung bei der Bereitstellung virtueller Computer. Dies aktiviert die [Azure Disk Encryption](../../security/fundamentals/encryption-overview.md).|
|Azure Backup|Zulassen der Sicherung und Wiederherstellung von relevanten Schlüsseln und Geheimnissen während der Azure Virtual Machines-Sicherung mithilfe von [Azure Backup](../../backup/backup-overview.md).|
|Exchange Online und SharePoint Online|Zulassen des Zugriffs auf den Kundenschlüssel für die Azure Storage Service-Dienstverschlüsselung mit [Kundenschlüssel](/microsoft-365/compliance/customer-key-overview).|
|Azure Information Protection|Zulassen des Zugriffs auf den Mandantenschlüssel für [Azure Information Protection](/azure/information-protection/what-is-information-protection)|
|Azure App Service|[Bereitstellen eines Azure-Web-App-Zertifikats über Key Vault](https://azure.github.io/AppService/2016/05/24/Deploying-Azure-Web-App-Certificate-through-Key-Vault.html).|
|Azure SQL-Datenbank|[Transparent Data Encryption mit BYOK-Unterstützung (Bring Your Own Key) für Azure SQL-Datenbank und Azure Synapse Analytics](../../azure-sql/database/transparent-data-encryption-byok-overview.md?view=sql-server-2017&preserve-view=true&viewFallbackFrom=azuresqldb-current).|
|Azure Storage|[Storage Service Encryption mit von Kunden verwalteten Schlüsseln in Azure Key Vault](../../storage/common/customer-managed-keys-configure-key-vault.md).|
|Azure Data Lake Store|[Datenverschlüsselung in Azure Data Lake Store](../../data-lake-store/data-lake-store-encryption.md) mit von Kunden verwalteten Schlüsseln.|
|Azure Databricks|[Ein schneller, einfacher und kollaborativer Analysedienst auf Basis von Apache Spark](/azure/databricks/scenarios/what-is-azure-databricks)|
|Azure API Management|[Verwenden der verwalteten Dienstidentität für den Zugriff auf andere Ressourcen](../../api-management/api-management-howto-use-managed-service-identity.md#use-ssl-tls-certificate-from-azure-key-vault)|
|Azure Data Factory|[Abrufen von Anmeldeinformationen für den Datenspeicher in Key Vault aus Data Factory](https://go.microsoft.com/fwlink/?linkid=2109491)|
|Azure Event Hubs|[Zulassen des Zugriffs auf einen Schlüsseltresor für Szenarien mit kundenseitig verwalteten Schlüsseln](../../event-hubs/configure-customer-managed-key.md)|
|Azure-Servicebus|[Zulassen des Zugriffs auf einen Schlüsseltresor für Szenarien mit kundenseitig verwalteten Schlüsseln](../../service-bus-messaging/configure-customer-managed-key.md)|
|Azure Import/Export| [Verwenden kundenseitig verwalteter Schlüssel in Azure Key Vault für den Import/Export-Dienst](../../import-export/storage-import-export-encryption-key-portal.md)
|Azure Container Registry|[Registrierungsverschlüsselung mithilfe kundenseitig verwalteter Schlüssel](../../container-registry/container-registry-customer-managed-keys.md)

> [!NOTE]
> Sie müssen die relevanten Key Vault-Zugriffsrichtlinien so einrichten, dass die entsprechenden Dienste Zugriff auf den Schlüsseltresor erhalten.

## <a name="next-steps"></a>Nächste Schritte

- Schrittanleitungen finden Sie unter [Konfigurieren von Azure Key Vault-Firewalls und virtuellen Netzwerken](network-security.md).
- Weitere Informationen finden Sie in der [Azure Key Vault-Sicherheitsübersicht](security-overview.md).
