---
title: Zertifizierung von Gerätebundles und indirekt verbundenen Geräten
titleSuffix: Azure Certified
description: Erfahren Sie, wie Sie ein indirekt verbundenes Gerät zur Zertifizierung einreichen.
author: cbroad
ms.author: cbroad
ms.date: 02/23/2021
ms.topic: how-to
ms.service: certification
ms.openlocfilehash: 3a4fd2838c0ddf6d7d03d68f105fc59471b77dea
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107304476"
---
# <a name="device-bundles-and-indirectly-connected-devices"></a>Gerätebundles und indirekt verbundene Geräte

Zur Unterstützung von Geräten, die mit Azure über ein Gerät, SaaS- oder PaaS-Angebote interagieren, bieten unser Einreichungsportal (https://certify.azure.com/) und der Gerätekatalog (https://devicecatalog.azure.com) Konzepte von Bündelung und Abhängigkeiten, um diese Gerätekombinationen zu bewerben und ihnen den Zugang zu unserem Azure Certified Device-Geräteprogramm zu ermöglichen.

Abhängig von der angebotenen Produktlinie und den angebotenen Diensten erfordert Ihre Situation möglicherweise eine Kombination der folgenden Schritte:


![Erstellen von Projektabhängigkeiten](./media/indirect-connected-device/picture-1.png )
## <a name="sensors-and-indirect-devices"></a>Sensoren und indirekte Geräte
Viele Sensoren erfordern ein Gerät, um eine Verbindung mit Azure herzustellen. Außerdem verfügen Sie möglicherweise über mehrere kompatible Geräte, die mit dem Sensorgerät funktionieren. **Um diese Szenarios zu berücksichtigen, müssen Sie zunächst die Geräte zertifizieren, bevor Sie den Sensor zertifizieren, der Informationen über sie übergibt.**

Beispielmatrix für Einreichungskombinationen ![Einreichungsbeispiel](./media/indirect-connected-device/picture-2.png )

So zertifizieren Sie Ihren Sensor, für den ein separates Gerät erforderlich ist:
1.  Zunächst [zertifizieren Sie das Gerät](https://certify.azure.com) und veröffentlichen es im Azure Certified Device-Katalog.
    - Wenn Sie (wie im obigen Beispiel) über mehrere, kompatible Passthrough-Geräte verfügen, reichen Sie diese separat zur Zertifizierung ein, und veröffentlichen Sie sie auch im Katalog.
2.  Reichen Sie den über das Gerät verbundenen Sensor zur Zertifizierung ein.
    * Legen Sie auf der Registerkarte „Abhängigkeiten“ im Abschnitt „Gerätedetails“ die folgenden Werte fest.
        * Abhängigkeitstyp = „Hardwaregateway“
        * Abhängigkeits-URL = „URL-Link zum Gerät im Gerätekatalog“
        * Beim Test verwendet = „Ja“
        * Fügen Sie ggf. an Kunden gerichteten Anmerkungen hinzu, die einem Benutzer zur Verfügung stehen sollten, der die Produktbeschreibung im Gerätekatalog sieht. (Beispiel: „Serie 100-Geräte sind erforderlich, damit Sensoren eine Verbindung mit Azure herstellen können“)

3.  Wenn Sie über weitere Geräte verfügen, die Sie für dieses Gerät als optional hinzufügen möchten, können Sie „+ Zusätzliche Abhängigkeit hinzufügen“ auswählen. Befolgen Sie dann dieselbe Anleitung, und beachten Sie, dass es nicht beim Testen verwendet wurde. Stellen Sie in den an Kunden gerichteten Anmerkungen sicher, dass Ihre Kunden wissen, dass andere Geräte, die diesem Sensor zugeordnet sind, verfügbar sind (als Alternative zu dem Gerät, das während des Tests verwendet wurde).

![Alternativer Text](./media/indirect-connected-device/picture-3.png "Typ der Hardwareabhängigkeit")

## <a name="paas-and-saas-offerings"></a>PaaS- und SaaS-Angebote
Möglicherweise umfasst Ihr Produktportfolio Geräte, die Sie zertifizieren, aber Ihr Gerät erfordert auch andere Dienste Ihres Unternehmens oder anderer Drittanbieter. Um diese Abhängigkeit hinzuzufügen, führen Sie die folgenden Schritte aus:
1. Starten Sie den Einreichungsprozess für Ihr Gerät.
2. Legen Sie in der Registerkarte „Abhängigkeiten“ die folgenden Werte fest:
    - Abhängigkeitstyp = „Softwaredienst“
    - Dienstname = „[Ihr Produktname]“
    - Abhängigkeits-URL = „URL-Link zu einer Produktseite, die den Dienst beschreibt“
    - Fügen Sie ggf. an Kunden gerichteten Anmerkungen hinzu, die einem Benutzer zur Verfügung stehen sollten, der die Produktbeschreibung im Azure Certified Device-Katalog sieht.
3. Wenn Sie andere Software-, Dienst- oder Hardwareabhängigkeiten als für dieses Gerät optional hinzufügen möchten, können Sie „+ Zusätzliche Abhängigkeit hinzufügen“ auswählen und dieselbe Anleitung befolgen.

![Typ der Softwareabhängigkeit](./media/indirect-connected-device/picture-4.png )

## <a name="bundled-products"></a>Gebündelte Produkte
Auflistungen gebündelter Produkte sind einfach die erfolgreiche Zertifizierung eines Geräts mit anderen Komponenten, die als Teil des Bundles in einer Produktauflistung verkauft werden. Sie haben die Möglichkeit, ein Gerät einzureichen, das zusätzliche Komponenten wie einen Temperatursensor und einen Kamerasensor (1) enthält, oder Sie können einen Touchsensor einreichen, der ein Passthrough-Gerät (2) enthält. Über das Feature „Komponente“ haben Sie die Möglichkeit, Ihrer Auflistung mehrere Komponenten hinzuzufügen.

Wenn Sie dies beabsichtigen, formatieren Sie das Produktauflistungsbild so, dass zu erkennen ist, dass dieses Produkt mit anderen Komponenten geliefert wird.  Wenn zusätzliche Dienste zur Zertifizierung Ihres Bundles erforderlich sind, müssen Sie diese außerdem über die Diensteabhängigkeit identifizieren.
Beispielmatrix für gebündelte Produkte

![Beispiel für Bundleeinreichung](./media/indirect-connected-device/picture-5.png )

Eine ausführlichere Beschreibung zur Verwendung der Komponentenfunktionalität im Azure Certified Device-Portal finden Sie in der [Hilfedokumentation](./how-to-using-the-components-feature.md). 

Wenn ein Produkt aus einem Passthrough-Gerät mit einem separaten Sensor besteht, erstellen Sie eine Komponente, die das Passthrough-Gerät widerspiegelt, und eine weitere Komponente, die den Sensor widerspiegelt. Auf der Registerkarte „Produktdetails“ des Abschnitts „Gerätedetails“ können dem Projekt Komponenten hinzugefügt werden:

![Hinzufügen von Komponenten](./media/indirect-connected-device/picture-6.png )

Legen Sie für das Passthrough-Gerät „Sofort einsatzfähiges Produkt“ als „Komponententyp“ fest, und füllen Sie die anderen für Ihr Produkt relevanten Felder aus. Beispiel:

![Komponentendetails](./media/indirect-connected-device/picture-7.png )

Fügen Sie für den Sensor eine zweite Komponente hinzu, und legen Sie „Peripheriegerät“ als „Komponententyp“ und „Eigenständig“als „Anschlussmethode“ fest. Beispiel:

![Details der zweiten Komponente](./media/indirect-connected-device/picture-8.png )

Bearbeiten Sie nach dem Erstellen der Sensorkomponente die Details, navigieren Sie zur Registerkarte „Sensoren“, und fügen Sie dann die Sensordetails hinzu. Beispiel:

![Sensordetails](./media/indirect-connected-device/picture-9.png )

Tragen Sie Ihre Projektdetails ein, und reichen Sie Ihr Gerät wie gewohnt zur Zertifizierung ein.

