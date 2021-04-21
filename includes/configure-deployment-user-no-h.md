---
title: include file
description: include file
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 06/14/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 8f1fe6ae38c708c5205f8c1230da05457d6b7010
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107764301"
---
Für die Bereitstellung in einer Azure-Web-App über FTP oder ein lokales Git kann ein *Bereitstellungsbenutzer* verwendet werden. Nach der Konfiguration des Bereitstellungsbenutzers können Sie ihn für alle Azure-Bereitstellungen verwenden. Der Benutzername und das Kennwort für die Bereitstellung auf Kontoebene unterscheiden sich von den Anmeldeinformationen für Ihr Azure-Abonnement. 

Führen Sie zum Konfigurieren des Bereitstellungsbenutzers den Befehl [az webapp deployment user set](/cli/azure/webapp/deployment/user#az_webapp_deployment_user_set) in Azure Cloud Shell aus. Ersetzen Sie \<username> und \<password> durch Ihren Benutzernamen bzw. Ihr Kennwort für die Bereitstellung. 

- Der Benutzername muss in Azure eindeutig sein und darf bei lokalen Git-Pushes nicht das Symbol „\@“ enthalten. 
- Das Kennwort muss mindestens acht Zeichen lang sein und zwei der folgenden drei Elemente enthalten: Buchstaben, Zahlen und Symbole. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

In der JSON-Ausgabe wird das Kennwort als `null` angezeigt. Wenn Sie den Fehler `'Conflict'. Details: 409` erhalten, müssen Sie den Benutzernamen ändern. Wenn Sie den Fehler `'Bad Request'. Details: 400` erhalten, müssen Sie ein sichereres Kennwort verwenden. 

Notieren Sie Ihren Benutzernamen und Ihr Kennwort für die Bereitstellung Ihrer Web-Apps.
