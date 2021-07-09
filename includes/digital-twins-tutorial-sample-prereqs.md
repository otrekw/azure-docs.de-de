---
author: baanders
description: 'Include-Datei für Azure Digital Twins-Tuturials: Voraussetzung für das Beispielprojekt'
ms.service: digital-twins
ms.topic: include
ms.date: 1/20/2021
ms.author: baanders
ms.openlocfilehash: 3a3e32a0ef85ba870cfe7af9639794632549f614
ms.sourcegitcommit: 7f59e3b79a12395d37d569c250285a15df7a1077
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2021
ms.locfileid: "111350596"
---
## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes: 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

### <a name="get-required-resources"></a>Abrufen der erforderlichen Ressourcen

Für dieses Tutorial müssen Sie auf Ihrem Entwicklungscomputer **mindestens Version 16.5** von [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) installieren. Wenn Sie bereits eine ältere Version installiert haben, können Sie die App *Visual Studio-Installer* auf Ihrem Computer öffnen und die Anweisungen zur Aktualisierung Ihrer Installation befolgen.

>[!NOTE]
> Stellen Sie sicher, dass die Installation von Visual Studio 2019 die [Workload „Azure-Entwicklung“](/dotnet/azure/configure-visual-studio) umfasst. Diese Workload ermöglicht einer Anwendung die Veröffentlichung von Azure-Funktionen und die Ausführung anderer Azure-Entwicklungsaufgaben.

Grundlage des Tutorials ist ein [in C# geschriebenes Beispielprojekt End-to-End-Beispielprojekt für Azure Digital Twins](/samples/azure-samples/digital-twins-samples/digital-twins-samples). **Laden Sie das Beispielprojekt auf Ihren Computer herunter**, indem Sie zum Beispiellink navigieren und unter dem Titel die Schaltfläche *Code durchsuchen* auswählen. Dadurch gelangen Sie zum GitHub-Repository für die Beispiele, die Sie als ZIP-Datei herunterladen können. Wählen Sie hierzu die Schaltfläche *Code* und anschließend *ZIP herunterladen* aus.

:::image type="content" source="../articles/digital-twins/media/includes/download-repo-zip.png" alt-text="Screenshot des Repositorys „digital-twins-samples“ auf GitHub, in dem die Schritte zum Herunterladen als ZIP-Datei hervorgehoben sind." lightbox="../articles/digital-twins/media/includes/download-repo-zip.png":::

Dadurch wird ein ZIP-Ordner (**digital-twins-samples-master.zip**) auf Ihren Computer heruntergeladen. Entpacken Sie den Ordner, und extrahieren Sie die Dateien.

### <a name="prepare-an-azure-digital-twins-instance"></a>Vorbereiten einer Azure Digital Twins-Instanz

[!INCLUDE [Azure Digital Twins: instance prereq](digital-twins-prereq-instance.md)]
