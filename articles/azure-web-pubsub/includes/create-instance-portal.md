---
title: Erstellen einer Azure Web PubSub-Instanz mit dem Portal
description: Includedatei zum Erstellen einer Azure Web PubSub-Instanz mit dem Portal
author: yjin81
ms.author: yajin1
ms.service: azure-web-pubsub
ms.topic: include
ms.date: 03/11/2021
ms.openlocfilehash: e147d6f954009f8ed4267f8b947438e2e60da0b4
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110486669"
---
## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com/](https://portal.azure.com/) mit Ihrem Azure-Konto beim Azure-Portal an.

## <a name="create-an-azure-web-pubsub-service-instance"></a>Erstellen einer Azure Web PubSub-Dienstinstanz

Ihre Anwendung stellt eine Verbindung mit einer Instanz des Web PubSub-Diensts in Azure her.

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche „Neu“. Geben Sie im Bildschirm „Neu“ den Begriff *Web PubSub* in das Suchfeld ein, und drücken Sie die EINGABETASTE. (Sie können Azure Web PubSub auch in der Kategorie `Web` durchsuchen.)

:::image type="content" source="../media/create-instance-portal/search-web-pubsub-in-portal.png" alt-text="Screenshot: Durchsuchen von Azure Web PubSub im Portal.":::

2. Wählen Sie in den Suchergebnissen **Web PubSub** und dann **Erstellen** aus.

3. Geben Sie folgende Einstellungen ein.

    | Einstellung      | Vorgeschlagener Wert  | BESCHREIBUNG                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Ressourcenname** | Global eindeutiger Name | Der Name, der Ihre neue Web PubSub-Dienstinstanz identifiziert. Gültige Zeichen sind `a-z`, `0-9` und `-`.  | 
    | **Abonnement** | Ihr Abonnement | Das Abonnement, unter dem diese neue Web PubSub-Dienstinstanz erstellt wird | 
    | **[Ressourcengruppe](../../azure-resource-manager/management/overview.md)** |  myResourceGroup | Name für die neue Ressourcengruppe, in der Ihre Web PubSub-Dienstinstanz erstellt werden soll | 
    | **Location** | USA (Westen) | Wählen Sie eine [Region](https://azure.microsoft.com/regions/) in Ihrer Nähe aus. |
    | **Preisstufe** | Kostenlos | Probieren Sie Azure Web PubSub kostenlos aus. |
    | **Einheitenanzahl** |  Nicht verfügbar | Die Einheitenanzahl gibt an, wie viele Verbindungen Ihre Instanz des Web PubSub-Diensts akzeptieren kann. Dies kann nur im Tarif „Standard“ konfiguriert werden. |

4. Wählen Sie **Erstellen** aus, um mit der Bereitstellung der Instanz des Web PubSub-Diensts zu beginnen.
