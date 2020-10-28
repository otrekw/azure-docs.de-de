---
title: Bring Your Own Key (vom Kunden verwaltete Schlüssel) in Media Services
description: 'Sie können in Media Services einen vom Kunden verwalteten Schlüssel (also: „Bring Your Own Key“) verwenden.'
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: conceptual
ms.date: 10/14/2020
ms.openlocfilehash: a89ff56eb9e0f0a29b5b1fed7543c5f718425e51
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2020
ms.locfileid: "92325925"
---
# <a name="bring-your-own-key-customer-managed-keys-with-media-services"></a>Bring Your Own Key (vom Kunden verwaltete Schlüssel) in Media Services

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

„Bring Your Own Key“ (BYOK) ist eine im gesamten Bereich von Azure unterstützte Initiative, die Kunden dabei unterstützt, ihre Workloads in die Cloud auszulagern. Vom Kunden verwaltete Schlüssel ermöglichen es Kunden, die Konformitätsbestimmungen ihrer Branche einzuhalten, und verbessern die Mandantenisolation eines Diensts. Die Kontrolle der Verschlüsselungsschlüssel in die Hand der Kunden zu legen, ist eine Möglichkeit, unnötigen Zugriff zu minimieren und die Kontrolle und Vertrauenswürdigkeit von Microsoft-Diensten zu stärken.

## <a name="keys-and-key-management"></a>Schlüssel und Schlüsselverwaltung

Sie können eigene Schlüssel in Media Services verwenden, wenn Sie die Media Services-API 2020-05-01 einsetzen. Für alle Konten wird ein Standard-Kontoschlüssel erstellt, der mit einem Systemschlüssel verschlüsselt wird, der im Besitz von Media Services ist. Wenn Sie Ihren eigenen Schlüssel verwenden, wird der Kontoschlüssel mit Ihrem Schlüssel verschlüsselt. Inhaltsschlüssel werden mit dem Kontoschlüssel verschlüsselt. JobInputHttp-URLs und symmetrische Schlüssel zur Tokenüberprüfung werden ebenfalls verschlüsselt.

:::image type="content" source="./media/customer-managed-key/customer-managed-key.svg" alt-text="Ein vom System verwalteter Schlüssel wird durch einen vom Kunden verwalteten Schlüssel ersetzt":::

Media Services verwendet die verwaltete Identität des Media Services-Kontos, um Ihren Schlüssel aus einem Schlüsseltresor zu lesen, der sich in Ihrem Besitz befindet. Für Media Services ist es erforderlich, dass sich der Schlüsseltresor im gleichen Bereich wie das Konto befindet und dass vorläufiges Löschen und Schutz vor Bereinigung aktiviert sind.

Bei Ihrem Schlüssel kann es sich um einen 2048-, 3072- oder 4096-RSA-Schlüssel handeln, und sowohl HSM- als auch Softwareschlüssel werden unterstützt.

> [!NOTE]
> EC-Schlüssel werden nicht unterstützt.

Sie können wahlweise einen Schlüsselnamen und eine Schlüsselversion oder nur einen Schlüsselnamen angeben. Wenn Sie nur einen Schlüsselnamen verwendet, verwendet Media Services die aktuellste Schlüsselversion. Neue Versionen von Kundenschlüsseln werden automatisch erkannt, und der Kontoschlüssel wird erneut verschlüsselt.

> [!WARNING]
> Media Services überwacht den Zugriff auf den Kundenschlüssel. Wenn der Zugriff auf den Kundenschlüssel nicht möglich ist (beispielsweise, weil der Schlüssel gelöscht wurde, der Schlüsseltresor gelöscht wurde oder die Zugriffsberechtigung entzogen wurde), überführt Media Services das Konto in den Status „Customer Key Inaccessible“ (Kein Zugriff auf den Kundenschlüssel – effektiv ist das Konto damit deaktiviert). Allerdings kann das Konto in diesem Zustand gelöscht werden. Für das Konto werden lediglich die Vorgänge GET, LIST und DELETE unterstützt; alle anderen Anforderungen (Codierung, Streamen usw.) schlagen fehl, bis der Zugriff auf den Kontoschlüssel wiederhergestellt ist.

## <a name="tutorial"></a>Lernprogramm
Im Tutorial [Verwenden von vom Kunden verwalteten Schlüsseln oder BYOK (Bring Your Own Key) in Media Services](tutorial-byok.md) erhalten Sie weitere Informationen zum Einrichten und Verwenden von vom Kunden verwalteten Schlüsseln mit Postman und der Azure REST-API.

## <a name="next-steps"></a>Nächste Schritte

* [Tutorial: Verwenden von vom Kunden verwalteten Schlüsseln oder BYOK (Bring Your Own Key) in Media Services](tutorial-byok.md)
