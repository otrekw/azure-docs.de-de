---
title: Beispiele für Azure Resource Manager-Vorlagen
description: Hier finden Sie Azure Resource Manager-Vorlagenbeispiele für einige gängige App Service-Szenarien. Hier erfahren Sie, wie Sie Ihre Bereitstellungs- oder Verwaltungsaufgaben für App Service automatisieren.
author: tfitzmac
tags: azure-service-management
ms.topic: sample
ms.date: 08/26/2020
ms.author: tomfitz
ms.custom: mvc, fasttrack-edit
ms.openlocfilehash: 5129fccceb633991767cdd3b52bbb5b6af067270
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "88891044"
---
# <a name="azure-resource-manager-templates-for-app-service"></a>Azure Resource Manager-Vorlagen für App Service

Die folgende Tabelle enthält Links zu Azure Resource Manager-Vorlagen für Azure App Service. Empfehlungen zur Vermeidung allgemeiner Fehler beim Erstellen von App-Vorlagen finden Sie unter [Anleitung zum Bereitstellen von Apps mit Azure Resource Manager-Vorlagen](deploy-resource-manager-template.md).

Weitere Informationen zur JSON-Syntax und zu den Eigenschaften für App Services-Ressourcen finden Sie unter [Microsoft.Web resource types](/azure/templates/microsoft.web/allversions) (Microsoft.Web-Ressourcentypen).

| Bereitstellen einer App | BESCHREIBUNG |
|-|-|
| [App Service-Plan und einfache Linux-App](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-basic-linux) | Stellt eine für Linux konfigurierte App Service-App bereit. |
| [App Service-Plan und einfache Windows-App](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-basic-windows) | Stellt eine für Windows konfigurierte App Service-App bereit. |
| [Mit einem GitHub-Repository verknüpfte App](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-github-deploy)| Stellt eine App Service-App bereit, die Code aus GitHub bezieht. |
| [App mit benutzerdefinierten Bereitstellungsslots](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-custom-deployment-slots)| Stellt eine App Service-App mit benutzerdefinierten Bereitstellungsslots/-umgebungen bereit. |
| [App mit privatem Endpunkt](https://github.com/Azure/azure-quickstart-templates/tree/master/101-private-endpoint-webapp)| Hier wird eine App Service-App mit einem privaten Endpunkt bereitgestellt. |
|**Konfigurieren einer App**| **Beschreibung** |
| [App-Zertifikat aus Key Vault](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-certificate-from-key-vault)| Stellt ein App Service-App-Zertifikat auf der Grundlage eines Azure Key Vault-Geheimnisses bereit und verwendet es für die TLS-/SSL-Bindung. |
| [App mit benutzerdefinierter Domäne und SSL](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-custom-domain-and-ssl)| Stellt eine App Service-App mit einem benutzerdefinierten Hostnamen bereit und ruft aus Key Vault ein App-Zertifikat für die TLS-/SSL-Bindung ab. |
| [App mit Golang-Erweiterung](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-with-golang)| Stellt eine App Service-App mit der Golang-Websiteerweiterung bereit. Dies ermöglicht die Ausführung von mit Golang entwickelten Webanwendungen in Azure. |
| [App mit Java 8 und Tomcat 8](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-java-tomcat)| Stellt eine App Service-App mit aktiviertem Java 8 und Tomcat 8 bereit. Dies ermöglicht die Ausführung von Java-Anwendungen in Azure. |
| [App mit regionaler VNET-Integration](https://github.com/Azure/azure-quickstart-templates/tree/master/101-app-service-regional-vnet-integration)| Stellt eine App Service-App mit aktivierter regionaler VNET-Integration bereit. |
|**Schützen einer App**| **Beschreibung** |
| [In Azure Application Gateway integrierte App](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-with-app-gateway-v2)| Stellt eine App Service-App und Application Gateway bereit und isoliert den Datenverkehr mithilfe eines Dienstendpunkts und mithilfe von Zugriffsbeschränkungen. |
|**Linux-App mit verbundenen Ressourcen**| **Beschreibung** |
| [App unter Linux mit MySQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-linux-managed-mysql) | Stellt eine App Service-App unter Linux mit Azure Database for MySQL bereit. |
| [App unter Linux mit PostgreSQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-linux-managed-postgresql) | Stellt eine App Service-App unter Linux mit Azure Database for PostgreSQL bereit. |
|**App mit verbundenen Ressourcen**| **Beschreibung** |
| [App mit MySQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-managed-mysql)| Stellt eine App Service-App unter Windows mit Azure Database for MySQL bereit. |
| [App mit PostgreSQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-managed-postgresql)| Stellt eine App Service-App unter Windows mit Azure Database for PostgreSQL bereit. |
| [App mit Datenbank in Azure SQL-Datenbank](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-sql-database)| Stellt eine App Service-App und eine Datenbank in Azure SQL-Datenbank auf der Dienstebene „Basic“ bereit. |
| [App mit Blob Storage-Verbindung](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-blob-connection)| Stellt eine App Service-App mit einer Azure Blob Storage-Verbindungszeichenfolge bereit. Dies ermöglicht die Verwendung von Blob Storage über die App. |
| [App mit Azure Cache for Redis](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-with-redis-cache)| Stellt eine App Service-App mit einer Azure Cache for Redis-Instanz bereit. |
| [Mit einer Back-End-Web-App verbundene App](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-privateendpoint-vnet-injection)| Stellt zwei Web-Apps (Front-End und Back-End) sicher miteinander verbunden mit VNET-Injektion und privatem Endpunkt bereit. |
|**App Service-Umgebung**| **Beschreibung** |
| [Erstellen einer App Service-Umgebung v2](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-asev2-create) | Erstellt eine App Service-Umgebung v2 in Ihrem virtuellen Netzwerk. |
| [Erstellen einer App Service-Umgebung v2 mit einer ILB-Adresse](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-asev2-ilb-create/) | Erstellt eine App Service-Umgebung v2 in Ihrem virtuellen Netzwerk mit einer privaten Adresse des internen Lastenausgleichs. |
| [Konfigurieren des SSL-Standardzertifikats für eine ILB-App Service-Umgebung oder eine ILB-App Service-Umgebung v2](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-ase-ilb-configure-default-ssl) | Konfiguriert das TLS-/SSL-Standardzertifikat für eine ILB-App Service-Umgebung oder eine ILB-App Service-Umgebung v2. |
| | |
