---
title: Verwenden einer Firewall
titleSuffix: Azure Machine Learning
description: Steuern Sie den Zugriff auf Azure Machine Learning-Arbeitsbereiche mit Azure Firewall. Erfahren Sie mehr über die Hosts, die Sie über die Firewall zulassen müssen, damit Azure Machine Learning ordnungsgemäß funktioniert.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 04/27/2020
ms.openlocfilehash: 40c25dda3fefa9c54df832e16149a68a4aa5a33b
ms.sourcegitcommit: 999ccaf74347605e32505cbcfd6121163560a4ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2020
ms.locfileid: "82981964"
---
# <a name="use-workspace-behind-azure-firewall-for-azure-machine-learning"></a>Verwenden des Arbeitsbereichs hinter Azure Firewall für Azure Machine Learning

In diesem Artikel erfahren Sie, wie Sie Azure Firewall für die Verwendung mit einem Azure Machine Learning-Arbeitsbereich konfigurieren.

Azure Firewall kann verwendet werden, um den Zugriff auf Ihren Azure Machine Learning-Arbeitsbereich und das öffentliche Internet zu steuern. Wenn sie nicht ordnungsgemäß konfiguriert ist, kann die Firewall Probleme bei der Verwendung Ihres Arbeitsbereichs verursachen.

## <a name="network-rules"></a>Netzwerkregeln

Erstellen Sie in Ihrer Firewall eine Netzwerkregel, die Datenverkehr zu und von den Adressen in diesem Artikel zulässt.

> [!TIP]
> Legen Sie beim Hinzufügen der Netzwerkregel das __Protokoll__ auf „Beliebig“ und die Ports auf `*` fest.
>
> Weitere Informationen zum Konfigurieren von Azure Firewall finden Sie unter [Bereitstellen und Konfigurieren von Azure Firewall](../firewall/tutorial-firewall-deploy-portal.md#configure-a-network-rule).

## <a name="microsoft-hosts"></a>Microsoft-Hosts

Die Hosts in diesem Abschnitt befinden sich im Besitz von Microsoft und stellen Dienste bereit, die für die ordnungsgemäße Funktionsweise Ihres Arbeitsbereichs erforderlich sind.

| **Hostname** | **Zweck** |
| ---- | ---- |
| **\*.batchai.core.windows.net** | Trainingscluster |
| **ml.azure.com** | Azure Machine Learning Studio |
| **\*.azureml.ms** | Von Azure Machine Learning-APIs verwendet |
| **\*.experiments.azureml.net** | Von Experimenten verwendet, die in Azure Machine Learning ausgeführt werden|
| **\*.modelmanagement.azureml.net** | Zum Registrieren und Bereitstellen von Modellen verwendet|
| **mlworkspace.azure.ai** | Vom Azure-Portal beim Anzeigen eines Arbeitsbereichs verwendet |
| **\*.aether.ms** | Beim Ausführen von Azure Machine Learning-Pipelines verwendet |
| **\*.instances.azureml.net** | Azure Machine Learning-Computeinstanzen |
| **windows.net** | Azure Blob Storage |
| **vault.azure.net** | Azure-Schlüsseltresor |
| **microsoft.com** | Basis-Docker-Images |
| **azurecr.io** | Azure Container Registry |

## <a name="python-hosts"></a>Python-Hosts

Die Hosts in diesem Abschnitt werden zum Installieren von Python-Paketen verwendet. Sie sind für Entwicklung, Training und Bereitstellung erforderlich. 

| **Hostname** | **Zweck** |
| ---- | ---- |
| **anaconda.com** | Beim Installieren von conda-Paketen verwendet |
| **pypi.org** | Beim Installieren von pip-Paketen verwendet |

## <a name="r-hosts"></a>R-Hosts

Die Hosts in diesem Abschnitt werden zum Installieren von R-Paketen verwendet. Sie sind für Entwicklung, Training und Bereitstellung erforderlich.

> [!IMPORTANT]
> Intern verwendet das R SDK für Azure Machine Learning Python-Pakete. Daher müssen Sie auch Python-Hosts durch die Firewall zulassen.

| **Hostname** | **Zweck** |
| ---- | ---- |
| **cloud.r-project.org** | Beim Installieren von CRAN-Paketen verwendet |

Nächste Schritte

* [[Bereitstellen und Konfigurieren von Azure Firewall](../firewall/tutorial-firewall-deploy-portal.md)]
* [Verwenden einer Machine Learning Compute-Instanz](how-to-enable-virtual-network.md)
