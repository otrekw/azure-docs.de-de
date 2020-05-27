---
author: aahill
ms.author: aahi
ms.service: cognitive-services
ms.topic: include
ms.date: 05/18/2020
ms.openlocfilehash: 66bd78c94e6c54d26959778cc059730c13d02629
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83698565"
---
Im Formular müssen Sie Informationen über Sie selbst, Ihr Unternehmen und das Benutzerszenario eintragen, für das Sie den Container verwenden möchten. Nach der Übermittlung des Formulars wird es vom Azure Cognitive Services-Team überprüft, um sicherzustellen, dass Sie die Kriterien für den Zugriff auf die private Containerregistrierung erfüllen.

> [!IMPORTANT]
> Sie müssen im Formular eine E-Mail-Adresse verwenden, die entweder einem Microsoft-Konto (MSA) oder einem Azure Active Directory-Konto (Azure AD) zugeordnet ist.

Wenn Ihre Anforderung genehmigt wurde, erhalten Sie eine E-Mail mit Anweisungen zum Abrufen Ihrer Anmeldeinformationen und zum Zugreifen auf die private Containerregistrierung.

## <a name="log-in-to-the-private-container-registry"></a>Anmelden bei der privaten Containerregistrierung

Es gibt verschiedene Möglichkeiten, sich bei der privaten Containerregistrierung für Cognitive Services-Container zu authentifizieren. Es wird empfohlen, die Befehlszeilenmethode mithilfe der [Docker CLI](https://docs.docker.com/engine/reference/commandline/cli/) zu verwenden.

Verwenden Sie den Befehl [docker login](https://docs.docker.com/engine/reference/commandline/login/) wie im folgenden Beispiel gezeigt, um sich bei der privaten Containerregistrierung für Cognitive Services-Container (`containerpreview.azurecr.io`) anzumelden. Ersetzen Sie *\<username\>* durch den Benutzernamen und *\<password\>* durch das Kennwort. Diese Anmeldeinformationen wurden Ihnen vom Azure Cognitive Services-Team bereitgestellt.

```
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Wenn Sie Ihre Anmeldeinformationen in einer Textdatei gesichert haben, können Sie den Inhalt dieser Textdatei mit dem Befehl `docker login` verketten. Verwenden Sie den Befehl `cat` wie im folgenden Beispiel. Ersetzen Sie *\<passwordFile\>* durch den Pfad und Namen der Textdatei, die das Kennwort enthält. Ersetzen Sie den *\<Benutzernamen\>* durch den in Ihren Anmeldeinformationen angegebenen Benutzernamen.

```
cat <passwordFile> | docker login containerpreview.azurecr.io -u <username> --password-stdin
```

