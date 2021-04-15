---
title: include file
description: include file
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/26/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: aa9a715fdafc143a116458691965087b016dec1f
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "83343353"
---
Gehen Sie folgendermaßen vor, um die zurzeit von einer Funktions-App verwendete Runtimeversion anzuzeigen und zu aktualisieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer Funktions-App.

1. Wählen Sie unter **Einstellungen** die Option **Konfiguration** aus. Suchen Sie auf der Registerkarte **Runtimeeinstellungen der Funktion** nach der **Runtimeversion**. Notieren Sie sich die jeweilige Runtimeversion. Im nachstehenden Beispiel ist die Version auf `~3` festgelegt.

    :::image type="content" source="./media/functions-view-update-version-portal/functions-view-runtime-version.png" alt-text="Anzeigen der Runtimeversion." border="true":::

1. Um Ihre Funktions-App an die Runtime der Version 1.x anzuheften, wählen Sie **~1** unter **Laufzeitversion** aus. Dieser Schalter ist deaktiviert, wenn Ihre App Funktionen enthält.

1. Wenn Sie die Runtimeversion geändert haben, wechseln Sie zurück zur Registerkarte **Übersicht**, und wählen Sie **Neu starten** aus, um die App neu zu starten.  Die Funktions-App wird neu gestartet und mit der Runtime der Version 1.x ausgeführt. Außerdem werden die Vorlagen für Version 1.x verwendet, wenn Sie Funktionen erstellen.

    :::image type="content" source="./media/functions-view-update-version-portal/functions-restart-function-app.png" alt-text="Neustarten der Funktions-App." border="true":::
