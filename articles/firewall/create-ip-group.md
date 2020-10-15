---
title: Erstellen von IP-Adressgruppen in Azure Firewall
description: IP-Adressgruppen ermöglichen Ihnen das Gruppieren und Verwalten von IP-Adressen für Azure Firewall-Regeln.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: how-to
ms.date: 06/23/2020
ms.author: victorh
ms.openlocfilehash: c3ae62bf5b4f0b4796cac2e7079c8a09116d4895
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85602532"
---
# <a name="create-ip-groups"></a>Erstellen von IP-Adressgruppen

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