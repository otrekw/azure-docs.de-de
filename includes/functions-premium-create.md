---
title: include file
description: include file
services: functions
author: jeffhollan
ms.service: azure-functions
ms.topic: include
ms.date: 04/24/2020
ms.author: jehollan, glenga
ms.custom: include file
ms.openlocfilehash: c53486bf3368039f172c7a13420e2291dd9c9892
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2020
ms.locfileid: "83122659"
---
1. Wählen Sie im Menü des Azure-Portals oder auf der **Startseite** die Option **Ressource erstellen** aus.

1. Wählen Sie auf der Seite **Neu** die Option **Compute** > **Funktions-App** aus.

1. Verwenden Sie auf der Seite **Grundlagen** die Funktions-App-Einstellungen, die in der folgenden Tabelle angegeben sind:

    | Einstellung      | Vorgeschlagener Wert  | BESCHREIBUNG |
    | ------------ | ---------------- | ----------- |
    | **Abonnement** | Ihr Abonnement | Das Abonnement, unter dem diese neue Funktions-App erstellt wird. |
    | **[Ressourcengruppe](../articles/azure-resource-manager/management/overview.md)** |  *myResourceGroup* | Der Name der neuen Ressourcengruppe, in der die Funktionen-App erstellt wird |
    | **Name der Funktions-App** | Global eindeutiger Name | Der Name, der Ihre neue Funktionen-App bezeichnet Gültige Zeichen sind `a-z` (Groß-/Kleinschreibung nicht beachtet), `0-9` und `-`.  |
    |**Veröffentlichen**| Code | Option zum Veröffentlichen von Codedateien oder eines Docker-Containers. |
    | **Laufzeitstapel** | Bevorzugte Sprache | Wählen Sie eine Runtime aus, die Ihre bevorzugte Programmiersprache für Funktionen unterstützt. Wählen Sie **.NET** für C#- und F#-Funktionen aus. |
    |**Region**| Bevorzugte Region | Wählen Sie eine [Region](https://azure.microsoft.com/regions/) in Ihrer Nähe oder in der Nähe von anderen Diensten aus, auf die Ihre Funktionen zugreifen. |

    ![Seite „Grundlagen“](./media/functions-premium-create/function-app-create-basics.png)

1. Klicken Sie auf **Weiter: Hosting** aus. Geben Sie auf der Seite **Hosting** die folgenden Einstellungen ein:

    | Einstellung      | Vorgeschlagener Wert  | BESCHREIBUNG |
    | ------------ | ---------------- | ----------- |
    | **[Speicherkonto](../articles/storage/common/storage-account-create.md)** |  Global eindeutiger Name |  Erstellen Sie ein Speicherkonto, das von Ihrer Funktions-App verwendet wird. Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten. Sie können auch ein vorhandenes Konto verwenden, das die [Anforderungen an das Speicherkonto](../articles/azure-functions/functions-scale.md#storage-account-requirements) erfüllen muss. |
    |**Betriebssystem**| Bevorzugtes Betriebssystem | Ein Betriebssystem ist für Sie basierend auf Ihrer Runtimestapelauswahl vorab ausgewählt, aber Sie können die Einstellung ggf. ändern. |
    | **[Plan](../articles/azure-functions/functions-scale.md)** | Premium | Der Hostingplan, der definiert, wie Ihre Ressourcen der Funktionen-App zugewiesen werden Wählen Sie **Premium** und dann die Standardeinstellungen für **Windows-Plan** und **SKU und Größe** aus. |

    ![Seite „Hosting“](./media/functions-premium-create/function-app-premium-create-hosting.png)

1. Klicken Sie auf **Weiter: Überwachung** aus. Geben Sie auf der Seite **Überwachung** die folgenden Einstellungen ein:

    | Einstellung      | Vorgeschlagener Wert  | BESCHREIBUNG |
    | ------------ | ---------------- | ----------- |
    | **[Application Insights](../articles/azure-functions/functions-monitoring.md)** | Standard | Erstellt eine Application Insights-Ressource mit dem gleichen *App-Namen* in der nächstgelegenen unterstützten Region. Durch die Erweiterung dieser Einstellung können Sie den **neuen Ressourcennamen** ändern oder einen anderen **Standort** in einer [Azure-Region](https://azure.microsoft.com/global-infrastructure/geographies/) zum Speichern Ihrer Daten auswählen. |

    ![Seite „Überwachung“](./media/functions-create-function-app-portal/function-app-create-monitoring.png)

1. Wählen Sie **Bewerten + erstellen** aus, um die App-Konfigurationsauswahl zu überprüfen.

1. Überprüfen Sie auf der Seite **Bewerten + erstellen** Ihre Einstellungen, und wählen Sie dann **Erstellen** aus, um die Funktions-App bereitzustellen.

1. Wählen Sie oben rechts im Portal das **Benachrichtigungssymbol** aus, und achten Sie auf die Meldung **Bereitstellung erfolgreich**.

1. Wählen Sie **Zu Ressource wechseln**, um Ihre neue Funktionen-App anzuzeigen. Sie können auch die Option **An Dashboard anheften** auswählen. Wenn Sie die Funktions-App anheften, können Sie einfacher über das Dashboard auf sie zugreifen.

    ![Bereitstellungsbenachrichtigung](./media/functions-premium-create/function-app-create-notification2.png)