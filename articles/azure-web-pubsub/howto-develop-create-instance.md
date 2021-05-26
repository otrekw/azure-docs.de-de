---
title: Erstellen der Azure Web PubSub-Instanz
description: Eine Übersicht über die Optionen zum Erstellen einer Azure Web PubSub-Instanz und die Vorgehensweise
author: yjin81
ms.author: yajin1
ms.service: azure-web-pubsub
ms.topic: how-to
ms.date: 03/17/2021
ms.openlocfilehash: 0ed91129f2093018ff34a2b3eef505eded9483cf
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110060704"
---
# <a name="how-to-create-azure-web-pubsub-instance"></a>Erstellen einer Azure Web PubSub-Instanz

Um eine Anwendung mit dem Azure Web PubSub-Dienst zu erstellen, müssen Sie die Web PubSub-Instanz erstellen und dann Ihre Clients und Server verbinden. In dieser Anleitung werden die Optionen zum Erstellen einer Azure Web PubSub-Instanz erläutert.

## <a name="create-azure-web-pubsub-instance-with-azure-portal"></a>Erstellen einer Azure Web PubSub-Instanz mit Azure-Portal 

Das [Azure-Portal](https://docs.microsoft.com/azure/azure-portal/) ist eine webbasierte, zentrale Konsole, die eine Alternative zu Befehlszeilentools darstellt. Sie können Ihr Azure-Abonnement mit dem Azure-Portal verwalten. Erstellen, Verwalten und Überwachen aller Komponenten – von einfachen Web-Apps bis hin zu komplexen Cloudbereitstellungen. Sie können auch eine Azure Web PubSub-Dienstinstanz mit Azure-Portal erstellen.

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche „Neu“. Geben Sie im Bildschirm „Neu“ den Begriff *Web PubSub* in das Suchfeld ein, und drücken Sie die EINGABETASTE.

1. Wählen Sie in den Suchergebnissen **Web PubSub** und dann **Erstellen** aus.

1. Geben Sie folgende Einstellungen ein.

    | Einstellung      | BESCHREIBUNG                                        |
    | ------------ | -------------------------------------------------- |
    | **Ressourcenname** | Der global eindeutige Name, welcher Ihre neue Web PubSub-Dienstinstanz identifiziert. Gültige Zeichen sind `a-z`, `0-9` und `-`.  | 
    | **Abonnement** | Das Azure-Web PubSubAbonnement, unter dem diese neue Web PubSub-Dienstinstanz erstellt wird. | 
    | **[Ressourcengruppe](../azure-resource-manager/management/overview.md)** |  Name für die neue oder vorhandene Ressourcengruppe, in der Ihre Web PubSub-Dienstinstanz erstellt werden soll. | 
    | **Location** | Wählen Sie eine [Region](https://azure.microsoft.com/regions/) in Ihrer Nähe aus. |
    | **Preisstufe** | Erfahren Sie mehr über die Tarife die [Preisebenen des Azure Web PubSub-Dienstes](https://azure.microsoft.com/pricing/details/web-pubsub/). |
    | **Einheitenanzahl** |  Die Einheitenanzahl gibt an, wie viele Verbindungen Ihre Instanz des Web PubSub-Dienstes akzeptieren kann. Jede Einheit unterstützt höchstens 1.000 gleichzeitige Verbindungen. Dies kann nur im Tarif „Standard“ konfiguriert werden. |

1. Wählen Sie **Erstellen** aus, um mit der Bereitstellung der Instanz des Web PubSub-Dienstes zu beginnen.

## <a name="create-azure-web-pubsub-instance-with-azure-cli"></a>Erstellen einer Azure Web PubSub-Instanz mit Azure CLI

Die [Azure-Befehlszeilenschnittstelle (Azure CLI)](https://docs.microsoft.com/cli/azure) setzt sich aus Befehlen zum Erstellen und Verwalten von Azure-Ressourcen zusammen. Die Azure CLI ist in allen Azure-Diensten verfügbar und soll die schnelle Verwendung von Azure ermöglichen. Der Fokus liegt dabei auf der Automatisierung. Sie können auch eine Azure Web PubSub-Dienstinstanz mit Azure CLI nach GA erstellen.


