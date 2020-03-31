---
title: Erstellen von IP-Adressgruppen in Azure Firewall
description: IP-Adressgruppen ermöglichen Ihnen das Gruppieren und Verwalten von IP-Adressen für Azure Firewall-Regeln.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 02/18/2020
ms.author: victorh
ms.openlocfilehash: 7e8b2350b9e85d07ce1c399ce4536703ec998cbc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "77444855"
---
# <a name="create-ip-groups-preview"></a>Erstellen von IP-Adressgruppen (Vorschau)

> [!IMPORTANT]
> Diese öffentliche Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und sollte nicht für Produktionsworkloads verwendet werden. Unter Umständen werden bestimmte Features nicht unterstützt, verfügen über eingeschränkte Funktionen und sind nicht an allen Azure-Standorten verfügbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

IP-Adressgruppen ermöglichen Ihnen das Gruppieren und Verwalten von IP-Adressen für Azure Firewall-Regeln. Sie können eine einzelne IP-Adresse, mehrere IP-Adressen oder aber einen oder mehrere IP-Adressbereiche enthalten.

## <a name="create-an-ip-group"></a>Erstellen einer IP-Adressgruppe

1. Wählen Sie auf der Startseite des Azure-Portals **Ressource erstellen** aus.
2. Geben Sie im Textfeld für Suche **IP-Adressgruppen** ein, und wählen Sie **IP-Adressgruppen** aus.
3. Klicken Sie auf **Erstellen**.
4. Wählen Sie Ihr Abonnement aus.
5. Wählen Sie eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue.
6. Geben Sie einen eindeutigen Namen für Ihre IP-Adressgruppe ein, und wählen Sie die gewünschte Region aus.

6. Klicken Sie auf **Weiter: IP-Adressen**.
7. Geben Sie eine IP-Adresse, mehrere IP-Adressen oder IP-Adressbereiche ein.

   Es gibt zwei Möglichkeiten zum Eingeben von IP-Adressen:
   - Sie können sie manuell eingeben.
   - Sie können sie aus einer Datei importieren.

   Zum Importieren aus einer Datei wählen Sie **Aus Datei importieren** aus. Sie können entweder Ihre Datei in das Feld ziehen oder **Nach Dateien suchen**auswählen. Falls erforderlich, können Sie Ihre hochgeladenen IP-Adressen überprüfen und bearbeiten.

   Wenn Sie eine IP-Adresse eingeben, wird sie vom Portal im Hinblick auf Überlappung, Duplikate und Formatierungsprobleme überprüft.

5. Nach Abschluss dieses Vorgangs wählen Sie **Bewerten + erstellen** aus.
6. Klicken Sie auf **Erstellen**.


## <a name="next-steps"></a>Nächste Schritte

- [Weitere Informationen zu IP-Adressgruppen](ip-groups.md)