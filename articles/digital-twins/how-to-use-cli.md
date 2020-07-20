---
title: Verwenden der Azure Digital Twins-Befehlszeilenschnittstelle
titleSuffix: Azure Digital Twins
description: Informieren Sie sich über die ersten Schritte mit der Azure Digital Twins-Befehlszeilenschnittstelle und deren Verwendung.
author: baanders
ms.author: baanders
ms.date: 05/25/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 53b20ded8e4b4a003beff1ef8489ecd9ff3451ac
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84725295"
---
# <a name="use-the-azure-digital-twins-cli"></a>Verwenden der Azure Digital Twins-Befehlszeilenschnittstelle

Zusätzlich zur Verwaltung Ihrer Azure Digital Twins-Instanz im Azure-Portal verfügt Azure Digital Twins über eine **Befehlszeilenschnittstelle (Command-Line Interface, CLI)** , über die Sie die meisten wichtigen Aktionen mit dem Dienst ausführen können, einschließlich:
* Verwalten einer Azure Digital Twins-Instanz
* Verwalten von Modellen
* Verwalten von digitalen Zwillingen
* Verwalten von Zwillingsbeziehungen
* Konfigurieren von Endpunkten
* Verwalten von [Routen](concepts-route-events.md)
* Konfigurieren der [Sicherheit](concepts-security.md) über die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)

Die Azure Digital Twins-Befehle sind Teil der [Azure IoT-Erweiterung für Azure CLI](https://github.com/Azure/azure-iot-cli-extension). Sie können die Referenzdokumentation für diese Befehle als Teil des `az iot`-Befehlssatzes anzeigen: [az dt](https://docs.microsoft.com/cli/azure/ext/azure-iot/dt?view=azure-cli-latest).

## <a name="deploy-and-validate"></a>Bereitstellen und validieren

Zusätzlich zur allgemeinen Verwaltung Ihrer Instanz ist die CLI auch ein nützliches Tool für die Bereitstellung und Validierung.
* Mithilfe der Befehle auf Steuerungsebene kann die Bereitstellung einer neuen Instanz wiederholbar oder automatisiert werden.
* Mit den Befehlen auf Datenebene können Sie schnell Werte in Ihrer Instanz und den erwartungsgemäßen Abschluss von Vorgängen überprüfen.

## <a name="next-steps"></a>Nächste Schritte

Um eine Alternative zu CLI-Befehlen kennenzulernen, informieren Sie sich über die Verwaltung einer Azure Digital Twins-Instanz mit APIs und SDKs:
* [Gewusst wie: Verwenden der APIs und SDKs für Azure Digital Twins](how-to-use-apis-sdks.md)
