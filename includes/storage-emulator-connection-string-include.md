---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 12/28/2020
ms.author: tamram
ms.openlocfilehash: 85cfe3b062d7d9ef3a7bdcf29ef7d2125f8f3ae4
ms.sourcegitcommit: 31d242b611a2887e0af1fc501a7d808c933a6bf6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/29/2020
ms.locfileid: "97812759"
---
Der Emulator unterstützt nur ein einziges festgelegtes Konto und einen bekannten Authentifizierungsschlüssel für die Authentifizierung mit einem gemeinsam verwendeten Schlüssel. Dieses Konto und dieser Schlüssel sind die einzigen gemeinsam verwendeten Anmeldeinformationen, die für den Emulator verwendet werden dürfen. Sie lauten wie folgt:

```
Account name: devstoreaccount1
Account key: Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

> [!NOTE]
> Der vom Emulator unterstützte Authentifizierungsschlüssel ist lediglich für das Testen der Funktionalität des Clientauthentifizierungscodes vorgesehen. Er erfüllt keinerlei Sicherheitszwecke. Sie können das in der Produktion verwendete Speicherkonto und den zugehörigen Schlüssel nicht mit dem Emulator verwenden. Es ist nicht ratsam, das Entwicklungskonto mit Produktionsdaten zu verwenden.
>
> Der Emulator unterstützt nur Verbindungen über HTTP. Das empfohlene Protokoll für den Zugriff auf Ressourcen in einem in der Produktionsumgebung verwendeten Azure-Speicherkonto ist aber HTTPS.
>

#### <a name="connect-to-the-emulator-account-using-the-shortcut"></a>Herstellen einer Verbindung mit dem Emulatorkonto über die Verknüpfung

Am einfachsten können Sie über Ihre Anwendung eine Verbindung mit dem Emulator herstellen, indem Sie eine Verbindungszeichenfolge in der Konfigurationsdatei Ihrer Anwendung konfigurieren, die auf die Verknüpfung `UseDevelopmentStorage=true` verweist. Die Verknüpfung entspricht der vollständigen Verbindungszeichenfolge für den Emulator, die den Kontonamen, den Kontoschlüssel und die Emulatorendpunkte für die einzelnen Azure Storage-Dienste angibt:

```
DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;
AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;
BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;
QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1;
TableEndpoint=http://127.0.0.1:10001/devstoreaccount1;
```

Der folgende .NET-Codeausschnitt zeigt, wie Sie die Verknüpfung aus einer Methode verwenden können, die eine Verbindungszeichenfolge annimmt. Der Konstruktor [BlobContainerClient(String, String)](/dotnet/api/azure.storage.blobs.blobcontainerclient.-ctor#Azure_Storage_Blobs_BlobContainerClient__ctor_System_String_System_String_) nimmt zum Beispiel eine Verbindungszeichenfolge an.

```csharp
BlobContainerClient blobContainerClient = new BlobContainerClient("UseDevelopmentStorage=true", "sample-container");
blobContainerClient.CreateIfNotExists();
```

Stellen Sie sicher, dass der Emulator ausgeführt wird, bevor Sie den Code im Codeausschnitt aufrufen.
