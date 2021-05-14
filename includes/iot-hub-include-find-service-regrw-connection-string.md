---
title: include file
description: include file
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 04/03/2020
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: e2df24a85b39d1a66a162a826dbc3985a3908d53
ms.sourcegitcommit: 1fbd591a67e6422edb6de8fc901ac7063172f49e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2021
ms.locfileid: "109508393"
---
<!-- This tells how to create a custom shared access policy that has service connect and registry RW permissions for your IoT hub and get the connection string for it-->

Wenn Sie eine SAS-Richtlinie, die die Berechtigungen **Dienstverbindung** und **Schreibvorgänge in Registrierung** gewährt, und eine Verbindungszeichenfolge für diese Richtlinie abrufen möchten, führen Sie die folgenden Schritte aus:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Ressourcengruppen** aus. Wählen Sie die Ressourcengruppe aus, in der sich der Hub befindet, und wählen Sie dann in der Liste der Ressourcen Ihren Hub aus.

1. Wählen Sie im linken Bereich Ihres Hubs **SAS-Richtlinien** aus.

1. Wählen Sie im Menü über der Richtlinienliste die Option **Hinzufügen** aus.

1. Geben Sie unter **Richtlinie für den gemeinsamen Zugriff hinzufügen** einen aussagekräftigen Namen für Ihre Richtlinie ein (z. B. *serviceAndRegistryReadWrite*). Wählen Sie unter **Berechtigungen** die Berechtigungen **Schreibvorgänge in Registrierung** und **Dienstverbindung** und anschließend **Erstellen** aus. (Die Berechtigung **Lesevorgänge in Registrierung** wird automatisch einbezogen, wenn Sie **Schreibvorgänge in Registrierung** auswählen.)

    ![Hinzufügen einer neuen SAS-Richtlinie](./media/iot-hub-include-find-service-regrw-connection-string/iot-hub-add-svc-regrw-policy.png)

1. Wählen Sie Ihre neue Richtlinie aus der Liste der Richtlinien aus.

1. Wählen Sie unter **Schlüssel für gemeinsamen Zugriff** das Kopiersymbol für **Verbindungszeichenfolge – Primärschlüssel** aus, und speichern Sie den Wert.

    ![Abrufen der Verbindungszeichenfolge.](./media/iot-hub-include-find-service-regrw-connection-string/iot-hub-get-connection-string.png)

Weitere Informationen zu SAS-Richtlinien und Berechtigungen für IoT-Hubs finden Sie unter [Access Control und Berechtigungen](../articles/iot-hub/iot-hub-dev-guide-sas.md#access-control-and-permissions).
