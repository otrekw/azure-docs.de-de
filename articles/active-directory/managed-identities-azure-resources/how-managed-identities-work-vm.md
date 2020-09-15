---
title: Verwendung verwalteter Identitäten für Azure-Ressourcen zusammen mit virtuellen Azure-Computern
description: Hier finden Sie eine Beschreibung dazu, wie verwaltete Identitäten für Azure-Ressourcen zusammen mit virtuellen Azure-Computern verwendet werden.
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: ''
ms.assetid: 0232041d-b8f5-4bd2-8d11-27999ad69370
ms.service: active-directory
ms.subservice: msi
ms.devlang: ''
ms.topic: conceptual
ms.custom: mvc
ms.date: 06/11/2020
ms.author: barclayn
ms.collection: M365-identity-device-management
ms.openlocfilehash: b93f45b05e6d7773afc2f750fd1a9a034c01ca1e
ms.sourcegitcommit: 3fb5e772f8f4068cc6d91d9cde253065a7f265d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89178670"
---
# <a name="how-managed-identities-for-azure-resources-work-with-azure-virtual-machines"></a>Verwendung verwalteter Identitäten für Azure-Ressourcen zusammen mit virtuellen Azure-Computern

Verwaltete Identitäten für Azure-Ressourcen stellen für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein.

In diesem Artikel erfahren Sie, wie verwaltete Identitäten mit virtuellen Azure-Computern (Virtual Machines, VMs) verwendet werden.


## <a name="how-it-works"></a>Funktionsweise

Intern sind verwaltete Identitäten eine Sonderform von Dienstprinzipalen, die nur mit Azure-Ressourcen verwendet werden können. Wenn die verwaltete Identität gelöscht wird, wird automatisch auch der entsprechende Dienstprinzipal entfernt.
Außerdem gilt: Wenn eine benutzerseitig oder systemseitig zugewiesenen Identität erstellt wird, gibt der Ressourcenanbieter der verwalteten Identität (Managed Identity Resource Provider, MSRP) intern ein Zertifikat für diese Identität aus. 

Ihr Code kann eine verwaltete Identität zum Anfordern von Zugriffstokens für Dienste verwenden, die die Azure AD-Authentifizierung unterstützen. Azure übernimmt die Weitergabe der von der Dienstinstanz verwendeten Anmeldeinformationen. 

Das folgende Diagramm zeigt, wie verwaltete Dienstidentitäten mit virtuellen Azure-Computern funktionieren:

![Verwaltete Dienstidentitäten und Azure-VMs](media/how-managed-identities-work-vm/data-flow.png)

|  Eigenschaft    | Systemseitig zugewiesene verwaltete Identität | Benutzerseitig zugewiesene verwaltete Identität |
|------|----------------------------------|--------------------------------|
| Erstellung |  Als Teil einer Azure-Ressource (etwa eines virtuellen Azure-Computers oder einer Azure App Service-Instanz). | Als eigenständige Azure-Ressource. |
| Lebenszyklus | Gemeinsamer Lebenszyklus mit der Azure-Ressource, für die die verwaltete Identität erstellt wurde. <br/> Wenn die übergeordnete Ressource gelöscht wird, wird auch die verwaltete Identität gelöscht. | Unabhängiger Lebenszyklus. <br/> Muss explizit gelöscht werden. |
| Gemeinsame Nutzung durch mehrere Azure-Ressourcen | Keine gemeinsame Nutzung möglich. <br/> Kann nur einer einzelnen Azure-Ressource zugeordnet werden. | Gemeinsame Nutzung möglich. <br/> Die gleiche benutzerseitig zugewiesene verwaltete Identität kann mehreren Azure-Ressourcen zugeordnet werden. |
| Gängige Anwendungsfälle | Workloads innerhalb einer einzelnen Azure-Ressource. <br/> Workloads, für die unabhängige Identitäten erforderlich sind. <br/> Beispielsweise eine Anwendung, die auf einem einzelnen virtuellen Computer ausgeführt wird. | Workloads, die auf mehrere Ressourcen ausgeführt werden und sich eine einzelne Identität teilen können. <br/> Workloads, die im Rahmen eines Bereitstellungsablaufs vorab für eine sichere Ressource autorisiert werden müssen. <br/> Workloads, bei denen Ressourcen häufig recycelt werden, die Berechtigungen aber konsistent bleiben sollen. <br/> Beispielsweise eine Workload, bei der mehrere virtuelle Computer auf die gleiche Ressource zugreifen müssen. |

## <a name="system-assigned-managed-identity"></a>Systemseitig zugewiesene verwaltete Identität

1. Azure Resource Manager empfängt eine Anforderung zur Aktivierung der vom System zugewiesenen verwalteten Identität auf einer VM.

2. Azure Resource Manager erstellt einen Dienstprinzipal in Azure AD für die Identität des virtuellen Computers. Der Dienstprinzipal wird in dem Azure AD-Mandanten erstellt, der von diesem Abonnement als vertrauenswürdig eingestuft wird.

3. Azure Resource Manager aktualisiert den Identitätsendpunkt von Azure Instance Metadata Service mit der Client-ID und dem Zertifikat des Dienstprinzipals, um die Identität auf der VM zu konfigurieren.

4. Wenn der virtuelle Computer über eine Identität verfügt, verwenden wir die Dienstprinzipalinformationen, um ihm Zugriff auf Azure-Ressourcen zu gewähren. Verwenden Sie zum Aufrufen von Azure Resource Manager die rollenbasierte Zugriffssteuerung (RBAC) in Azure, um dem VM-Dienstprinzipal die entsprechende Rolle zuzuweisen. Gewähren Sie Ihrem Code zum Aufrufen von Key Vault Zugriff auf das spezifische Geheimnis oder den spezifischen Schlüssel in Key Vault.

5. Der auf dem virtuellen Computer ausgeführte Code kann ein Token vom Azure IMDS-Endpunkt (Instance Metadata Service) anfordern, auf das nur von dem virtuellen Computer aus zugegriffen werden kann: `http://169.254.169.254/metadata/identity/oauth2/token`
    - Der Ressourcenparameter gibt den Dienst an, an den das Token gesendet wird. Verwenden Sie `resource=https://management.azure.com/` für die Authentifizierung bei Azure Resource Manager.
    - Der API-Versionsparameter gibt die IMDS-Version an. Verwenden Sie mindestens „api-version=2018-02-01“.

6. Mit einem an Azure AD gerichteten Aufruf wird ein Zugriffstoken angefordert (siehe Schritt 5). Dabei werden die Client-ID und das Zertifikat verwendet, die in Schritt 3 konfiguriert wurden. Azure AD gibt ein JWT-Zugriffstoken (JSON Web Token) zurück.

7. Der Code sendet das Zugriffstoken in einem Aufruf eines Diensts, der die Azure AD-Authentifizierung unterstützt.

## <a name="user-assigned-managed-identity"></a>Benutzerseitig zugewiesene verwaltete Identität

1. Azure Resource Manager empfängt eine Anforderung zur Erstellung einer vom Benutzer zugewiesenen verwalteten Identität.

2. Azure Resource Manager erstellt einen Dienstprinzipal in Azure AD für die vom Benutzer zugewiesene verwaltete Identität. Der Dienstprinzipal wird in dem Azure AD-Mandanten erstellt, der von diesem Abonnement als vertrauenswürdig eingestuft wird.

3. Azure Resource Manager empfängt eine Anforderung zum Konfigurieren der vom Benutzer zugewiesenen verwalteten Identität auf einer VM und aktualisiert den Identitätsendpunkt von Azure Instance Metadata Service mit der Client-ID und dem Zertifikat des Dienstprinzipals der vom Benutzer zugewiesenen verwalteten Identität.

4. Nachdem die vom Benutzer zugewiesene verwaltete Identität erstellt wurde, werden die Dienstprinzipalinformationen verwendet, um der Identität Zugriff auf Azure-Ressourcen zu gewähren. Verwenden Sie Azure RBAC zum Aufrufen von Azure Resource Manager, um dem Dienstprinzipal der vom Benutzer zugewiesenen Identität die entsprechende Rolle zuzuweisen. Gewähren Sie Ihrem Code zum Aufrufen von Key Vault Zugriff auf das spezifische Geheimnis oder den spezifischen Schlüssel in Key Vault.

   > [!Note]
   > Sie können diesen Schritt auch vor Schritt 3 ausführen.

5. Der auf dem virtuellen Computer ausgeführte Code kann ein Token vom Azure IMDS-Identitätsendpunkt (Instance Metadata Service) anfordern, auf das nur von dem virtuellen Computer aus zugegriffen werden kann: `http://169.254.169.254/metadata/identity/oauth2/token`
    - Der Ressourcenparameter gibt den Dienst an, an den das Token gesendet wird. Verwenden Sie `resource=https://management.azure.com/` für die Authentifizierung bei Azure Resource Manager.
    - Der Client-ID-Parameter gibt die Identität an, für die das Token angefordert wird. Dieser Wert ist erforderlich, um Mehrdeutigkeiten zu vermeiden, wenn auf einer einzelnen VM mehrere vom Benutzer zugewiesene Identitäten vorhanden sind.
    - Der API-Versionsparameter gibt die Azure Instance Metadata Service-Version an. Verwenden Sie `api-version=2018-02-01` oder höher.

6. Mit einem an Azure AD gerichteten Aufruf wird ein Zugriffstoken angefordert (siehe Schritt 5). Dabei werden die Client-ID und das Zertifikat verwendet, die in Schritt 3 konfiguriert wurden. Azure AD gibt ein JWT-Zugriffstoken (JSON Web Token) zurück.
7. Der Code sendet das Zugriffstoken in einem Aufruf eines Diensts, der die Azure AD-Authentifizierung unterstützt.


## <a name="next-steps"></a>Nächste Schritte

Informationen zu den ersten Schritten mit der Funktion für verwaltete Identitäten für Azure-Ressourcen finden Sie in den folgenden Schnellstartanleitungen:

* [Use a Windows VM system-assigned managed identity to access Resource Manager](tutorial-windows-vm-access-arm.md) (Verwenden der systemseitig zugewiesenen verwalteten Identität einer Windows-VM für den Zugriff auf Resource Manager)
* [Use a Linux VM system-assigned managed identity to access Resource Manager](tutorial-linux-vm-access-arm.md) (Verwenden der systemseitig zugewiesenen verwalteten Identität einer Linux-VM für den Zugriff auf Resource Manager)
