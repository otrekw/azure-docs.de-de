---
title: Konfigurieren von Windows Push Notification Service in Azure Notification Hubs | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Windows Push Notification Service-Einstellungen für einen Azure Notification Hub konfigurieren.
services: notification-hubs
author: sethmanheim
manager: femila
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
ms.date: 08/04/2020
ms.author: sethm
ms.reviewer: thsomasu
ms.lastreviewed: 03/25/2019
ms.openlocfilehash: 62d73a23fff3bd15d873a579d451b2f91509b7fe
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87758729"
---
# <a name="configure-windows-push-notification-service-settings-in-the-azure-portal"></a>Konfigurieren der Einstellungen des Windows-Pushbenachrichtigungsdiensts im Azure-Portal

In diesem Artikel wird gezeigt, wie Sie die Einstellungen des Windows-Pushbenachrichtigungsdiensts für einen Azure Notification Hub über das Azure-Portal konfigurieren.  

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie noch keinen Notification Hub erstellt haben, erstellen Sie ihn jetzt. Weitere Informationen finden Sie unter [Erstellen einer Azure Notification Hub-Instanz über das Azure-Portal](create-notification-hub-portal.md).

## <a name="configure-windows-push-notification-service-wns"></a>Konfigurieren von Windows Push Notification Service (WNS)

In der folgenden Vorgehensweise werden die Schritte zum Konfigurieren der Einstellungen des Windows-Pushbenachrichtigungsdiensts für einen Benachrichtigungshub beschrieben:

1. Wählen Sie im Azure-Portal auf der Seite **Notification Hub** im Menü auf der linken Seite die Option **Windows (WNS)** aus.
2. Geben Sie Werte für **Paket-SID** und **Sicherheitsschlüssel** ein.
3. Wählen Sie **Speichern** aus.

   ![Screenshot, der die Felder „Paket-SID“ und „Sicherheitsschlüssel“ zeigt](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

## <a name="next-steps"></a>Nächste Schritte

Ein Tutorial mit Schrittanleitungen zum Senden von Pushbenachrichtigungen an UWP-Anwendungen (Universelle Windows-Plattform) mithilfe von Azure Notification Hubs und dem Windows-Pushbenachrichtigungsdienst finden Sie unter [Senden von Benachrichtigungen an UWP-Apps mithilfe von Azure Notification Hubs](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).
