---
title: 'Schnellstart: Hinzufügen von Azure-Ressourcen zu Ihrer IoT-Lösung'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie Ihre End-to-End-IoT-Lösung mit Azure Defender für IoT konfigurieren.
ms.topic: quickstart
ms.date: 01/25/2021
ms.openlocfilehash: 6a90e8c3007f7b3448fd3f1b6e4fa46ba861081b
ms.sourcegitcommit: 77d7639e83c6d8eb6c2ce805b6130ff9c73e5d29
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2021
ms.locfileid: "106384579"
---
# <a name="quickstart-configure-your-azure-defender-for-iot-solution"></a>Schnellstart: Konfigurieren Ihrer Lösung mit Azure Defender für IoT

In diesem Artikel wird die Erstkonfiguration Ihrer IoT-Sicherheitslösung mit Defender für IoT erläutert.

## <a name="prerequisites"></a>Voraussetzungen

- Keine

## <a name="what-is-defender-for-iot"></a>Was ist Defender für IoT?

Defender für IoT bietet umfassende End-to-End-Sicherheit für Azure-basierte IoT-Lösungen.

Mit Defender für IoT können Sie Ihre gesamte IoT-Lösung über ein zentrales Dashboard überwachen, auf dem alle Ihre IoT-Geräte, IoT-Plattformen und Back-End-Ressourcen in Azure angezeigt werden.

Nach der Aktivierung in Ihrer IoT Hub-Instanz erkennt Defender für IoT automatisch andere Azure-Dienste, die ebenfalls mit Ihrer IoT Hub-Instanz verbunden sind und mit Ihrer IoT-Lösung zusammenhängen.

Zusätzlich zur automatischen Beziehungserkennung können Sie auch weitere Azure-Ressourcengruppen als Teil Ihrer IoT-Lösung kennzeichnen.

Dadurch können Sie vollständige Abonnements, Ressourcengruppen oder auch einzelne Ressourcen hinzufügen.

Nachdem Sie alle Ressourcenbeziehungen definiert haben, werden von Defender für IoT unter Verwendung von Defender Sicherheitsempfehlungen und Warnungen für diese Ressourcen bereitgestellt.

## <a name="add-azure-resources-to-your-iot-solution"></a>Hinzufügen von Azure-Ressourcen zu Ihrer IoT-Lösung

So fügen Sie Ihrer IoT-Lösung eine neue Ressource hinzu:

1. Öffnen Sie im Azure-Portal Ihre Instanz von **IoT Hub**.

1. Wählen Sie unter **Sicherheit** die Option **Übersicht**, anschließend **Einstellungen** und dann **Überwachte Ressourcen** aus.

1. Wählen Sie **Bearbeiten** und dann die überwachten Ressourcen aus, die zu ihrer IoT-Lösung gehören.

1. Wählen Sie **Hinzufügen**.

Glückwunsch! Sie haben Ihrer IoT-Lösung eine neue Ressourcengruppe hinzugefügt.

Defender für IoT überwacht nun die neu hinzugefügten Ressourcengruppen und zeigt relevante Sicherheitsempfehlungen und Warnungen im Rahmen Ihrer IoT-Lösung an.

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit dem nächsten Artikel fort, um sich über das Erstellen von Defender-IoT-Micro-Agents zu informieren:

> [!div class="nextstepaction"]
> [Erstellen von Defender-IoT-Micro-Agents](quickstart-create-security-twin.md)
