---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/17/2020
ms.author: tamram
ms.openlocfilehash: 85e7cb86217340e77a6f597a357c3de1f91fb8d0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87070556"
---
Azurite unterstützt ein einziges festgelegtes Konto und einen bekannten Authentifizierungsschlüssel für die Authentifizierung mit gemeinsam verwendetem Schlüssel. Dieses Konto und dieser Schlüssel sind die einzigen gemeinsam verwendeten Anmeldeinformationen, die bei Azurite verwendet werden dürfen. Sie lauten wie folgt:

```
Account name: devstoreaccount1
Account key: Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

> [!NOTE]
> Der von Azurite unterstützte Authentifizierungsschlüssel ist lediglich für das Testen der Funktionalität Ihres Clientauthentifizierungscodes vorgesehen. Er erfüllt keinerlei Sicherheitszwecke. Sie können das in der Produktion verwendete Speicherkonto und den zugehörigen Schlüssel bei Azurite nicht verwenden. Es ist nicht ratsam, das Entwicklungskonto mit Produktionsdaten zu verwenden.
> 
> Azurite unterstützt nur die Verbindung über HTTP. Das empfohlene Protokoll für den Zugriff auf Ressourcen in einem in der Produktionsumgebung verwendeten Azure-Speicherkonto ist aber HTTPS.
> 

#### <a name="connect-to-the-emulator-account-using-a-shortcut"></a>Herstellen einer Verbindung mit dem Emulatorkonto über eine Verknüpfung
Am einfachsten können Sie über Ihre Anwendung eine Verbindung mit Azurite herstellen, indem Sie in der Konfigurationsdatei der Anwendung eine Verbindungszeichenfolge konfigurieren, die auf die Verknüpfung `UseDevelopmentStorage=true` verweist. Eine Verbindungszeichenfolge, die auf Azurite in einer *app.config*-Datei verweist, kann beispielsweise so aussehen: 

```xml
<appSettings>
  <add key="StorageConnectionString" value="UseDevelopmentStorage=true" />
</appSettings>
```

#### <a name="connect-to-the-emulator-account-using-the-well-known-account-name-and-key"></a>Herstellen einer Verbindung mit dem Emulatorkonto mithilfe des bekannten Kontonamens und Schlüssels
Um eine Verbindungszeichenfolge zu erstellen, die auf den Emulator-Kontonamen und -schlüssel verweist, müssen Sie die Endpunkte für jeden Dienst, den Sie über den Emulator verwenden möchten, in der Verbindungszeichenfolge angeben. Dies ist erforderlich, damit die Verbindungszeichenfolge auf die Emulator-Endpunkte verweist. Diese unterscheiden sich von den Endpunkten eines Produktions-Speicherkontos. Beispielsweise wird der Wert der Verbindungszeichenfolge wie folgt aussehen:

```
DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;
AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;
BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;
QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1;
```

Dieser Wert stimmt mit der oben dargestellten Verknüpfung `UseDevelopmentStorage=true` überein.
