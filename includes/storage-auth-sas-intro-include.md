---
title: include file
description: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 12/20/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: aec1faa4de1149f08fb6fbc1cc5bf3aa2ab6becd
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "75371774"
---
Eine Shared Access Signature (SAS) ermöglicht Ihnen, eingeschränkten Zugriff auf Container und Blobs in Ihrem Speicherkonto zu gewähren. Wenn Sie eine SAS erstellen, geben Sie ihre Einschränkungen an, einschließlich der Azure Storage-Ressourcen, auf die Clients zugreifen dürfen, welche Berechtigungen sie für diese Ressourcen haben und wie lange die SAS gültig ist.

Jede SAS wird mit einem Schlüssel signiert. Zum Signieren einer SAS stehen zwei Möglichkeiten zur Verfügung:

- Mit einem Schlüssel, der mit Azure Active Directory-Anmeldeinformationen (Azure AD) erstellt wurde. Eine mit Azure AD-Anmeldeinformationen signierte SAS ist eine SAS für die *Benutzerdelegierung*.
- Mit dem Speicherkontoschlüssel. Sowohl eine *Dienst-SAS* als auch eine *Konto-SAS* wird mit dem Speicherkontoschlüssel signiert.

Eine SAS für die Benutzerdelegierung bietet überragende Sicherheit für eine SAS, die mit dem Speicherkontoschlüssel signiert wird. Microsoft empfiehlt, nach Möglichkeit eine SAS für die Benutzerdelegierung zu verwenden. Weitere Informationen finden Sie unter [Gewähren von eingeschränktem Zugriff auf Daten mithilfe von SAS (Shared Access Signature)](../articles/storage/common/storage-sas-overview.md).
