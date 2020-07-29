---
title: Verwenden der Azure Digital Twins-Befehlszeilenschnittstelle
titleSuffix: Azure Digital Twins
description: Informieren Sie sich über die ersten Schritte mit der Azure Digital Twins-Befehlszeilenschnittstelle und deren Verwendung.
author: baanders
ms.author: baanders
ms.date: 05/25/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 5272babf794529e5e9bd87a3c4a96e6df5758fb8
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86537461"
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
* [*Verwenden der Azure Digital Twins-APIs und SDKs*](how-to-use-apis-sdks.md)
