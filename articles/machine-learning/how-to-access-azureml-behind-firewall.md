---
title: Verwenden einer Firewall
titleSuffix: Azure Machine Learning
description: Steuern Sie den Zugriff auf Azure Machine Learning-Arbeitsbereiche mit Azure Firewall. Erfahren Sie mehr über die Hosts, die Sie durch die Firewall zulassen müssen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 07/17/2020
ms.custom: how-to, devx-track-python
ms.openlocfilehash: 5ba1804630649044d29dd2919ef8375265a69a08
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2020
ms.locfileid: "93320877"
---
# <a name="use-workspace-behind-a-firewall-for-azure-machine-learning"></a>Verwenden des Arbeitsbereichs hinter einer Firewall für Azure Machine Learning

In diesem Artikel erfahren Sie, wie Sie Azure Firewall konfigurieren, um den Zugriff auf Ihren Azure Machine Learning-Arbeitsbereich und das öffentliche Internet zu steuern.   Weitere Informationen zum Schützen von Azure Machine Learning finden Sie unter [Unternehmenssicherheit für Azure Machine Learning](concept-enterprise-security.md).

Die Informationen in diesem Dokument basieren zwar auf [Azure Firewall](../firewall/tutorial-firewall-deploy-portal.md), können aber auch auf andere Firewallprodukte angewendet werden. Wenn Sie Fragen dazu haben, wie Sie die Kommunikation über die Firewall zulassen, lesen Sie die Dokumentation für die von Ihnen verwendete Firewall.

## <a name="application-rules"></a>Anwendungsregeln

Erstellen Sie in Ihrer Firewall eine _Anwendungsregel_ , die Datenverkehr zu und von den Adressen in diesem Artikel zulässt.

> [!TIP]
> Legen Sie beim Hinzufügen der Netzwerkregel das __Protokoll__ auf „Beliebig“ und die Ports auf `*` fest.
>
> Weitere Informationen zum Konfigurieren von Azure Firewall finden Sie unter [Bereitstellen und Konfigurieren von Azure Firewall](../firewall/tutorial-firewall-deploy-portal.md#configure-an-application-rule).

## <a name="routes"></a>Routen

Wenn Sie die ausgehende Route für das Subnetz konfigurieren, das Azure Machine Learning-Ressourcen enthält, verwenden Sie die Anleitung im Abschnitt [Erzwungenes Tunneln](how-to-secure-training-vnet.md#forced-tunneling) zur Sicherung der Trainingsumgebung.

## <a name="microsoft-hosts"></a>Microsoft-Hosts

Wenn sie nicht ordnungsgemäß konfiguriert ist, kann die Firewall Probleme bei der Verwendung Ihres Arbeitsbereichs verursachen. Es gibt eine Vielzahl von Hostnamen, die vom Azure Machine Learning-Arbeitsbereich verwendet werden.

Die Hosts in diesem Abschnitt befinden sich im Besitz von Microsoft und stellen Dienste bereit, die für die ordnungsgemäße Funktionsweise Ihres Arbeitsbereichs erforderlich sind.

| **Hostname** | **Zweck** |
| ---- | ---- |
| **login.microsoftonline.com** | Authentifizierung |
| **management.azure.com** | Wird zum Abrufen der Arbeitsbereichsinformationen verwendet. |
| **\*.batchai.core.windows.net** | Trainingscluster |
| **ml.azure.com** | Azure Machine Learning Studio |
| **default.exp-tas.com** | Von Azure Machine Learning Studio verwendet |
| **\*.azureml.ms** | Von Azure Machine Learning-APIs verwendet |
| **\*.experiments.azureml.net** | Von Experimenten verwendet, die in Azure Machine Learning ausgeführt werden |
| **\*.modelmanagement.azureml.net** | Zum Registrieren und Bereitstellen von Modellen verwendet|
| **mlworkspace.azure.ai** | Vom Azure-Portal beim Anzeigen eines Arbeitsbereichs verwendet |
| **\*.aether.ms** | Beim Ausführen von Azure Machine Learning-Pipelines verwendet |
| **\*.instances.azureml.net** | Azure Machine Learning-Computeinstanzen |
| **\*.instances.azureml.ms** | Azure Machine Learning-Computeinstanzen, wenn für den Arbeitsbereich Private Link aktiviert ist |
| **windows.net** | Azure Blob Storage |
| **vault.azure.net** | Azure-Schlüsseltresor |
| **azurecr.io** | Azure Container Registry |
| **mcr.microsoft.com** | Microsoft Container Registry für Basis-Docker-Images |
| **your-acr-server-name.azurecr.io** | Nur erforderlich, wenn sich Ihre Azure Container Registry-Instanz hinter dem virtuellen Netzwerk befindet – in dieser Konfiguration wird ein privater Link von der Microsoft-Umgebung zur ACR-Instanz in Ihrem Abonnement erstellt. Verwenden Sie den ACR-Servernamen für Ihren Azure Machine Learning-Arbeitsbereich. |
| **\*.notebooks.azure.net** | Wird von den Notebooks in Azure Machine Learning Studio benötigt |
| **\*.file.core.windows.net** | Wird vom Datei-Explorer in Azure Machine Learning Studio benötigt. |
| **\*.dfs.core.windows.net** | Wird vom Datei-Explorer in Azure Machine Learning Studio benötigt. |
| **graph.windows.net** | Für Notebooks erforderlich |

> [!TIP]
> Wenn Sie die Verbundidentität verwenden möchten, befolgen Sie die Anleitungen im Artikel [Bewährte Methoden zum Sichern von Active Directory-Verbunddiensten](/windows-server/identity/ad-fs/deployment/best-practices-securing-ad-fs).

## <a name="python-hosts"></a>Python-Hosts

Die Hosts in diesem Abschnitt werden zum Installieren von Python-Paketen verwendet. Sie sind für Entwicklung, Training und Bereitstellung erforderlich. 

| **Hostname** | **Zweck** |
| ---- | ---- |
| **anaconda.com**</br>**\*.anaconda.com** | Wird verwendet, um Standardpakete zu installieren |
| **\*.anaconda.org** | Wird verwendet, um Repositorydaten abzurufen |
| **pypi.org** | Wird zum Auflisten von Abhängigkeiten vom Standardindex verwendet, sofern vorhanden, und der Index wird nicht durch Benutzereinstellungen überschrieben. Wenn der Index überschrieben wird, müssen Sie auch **\*.pythonhosted.org** zulassen. |

## <a name="r-hosts"></a>R-Hosts

Die Hosts in diesem Abschnitt werden zum Installieren von R-Paketen verwendet. Sie sind für Entwicklung, Training und Bereitstellung erforderlich.

> [!IMPORTANT]
> Intern verwendet das R SDK für Azure Machine Learning Python-Pakete. Daher müssen Sie auch Python-Hosts durch die Firewall zulassen.

| **Hostname** | **Zweck** |
| ---- | ---- |
| **cloud.r-project.org** | Beim Installieren von CRAN-Paketen verwendet |

## <a name="azure-government-region"></a>Azure Government-Region

Erforderliche URLs für die Azure Government-Regionen.

| **Hostname** | **Zweck** |
| ---- | ---- |
| **usgovarizona.api.ml.azure.us** | Die Region „US-Arizona“ |
| **usgovvirginia.api.ml.azure.us** | Die Region „US-Virginia“ |

## <a name="next-steps"></a>Nächste Schritte

* [Tutorial: Bereitstellen und Konfigurieren von Azure Firewall über das Azure-Portal](../firewall/tutorial-firewall-deploy-portal.md)
* [Verwenden einer Machine Learning Compute-Instanz](how-to-network-security-overview.md)
