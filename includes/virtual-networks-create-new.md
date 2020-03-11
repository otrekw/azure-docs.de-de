---
title: include file
description: include file
services: virtual-network
author: asudbring
ms.service: virtual-network
ms.topic: include
ms.date: 03/01/2020
ms.author: allensu
ms.custom: include file
ms.openlocfilehash: 81db46b52c9b4fe800f2fbfeadad966995d66e12
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2020
ms.locfileid: "78244973"
---
## <a name="create-the-virtual-network"></a>Erstellen des virtuellen Netzwerks

In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk und ein Subnetz.

1. Wählen Sie links oben auf dem Bildschirm **Ressource erstellen > Netzwerk > Virtuelles Netzwerk** aus, oder suchen Sie über das Suchfeld nach **Virtuelles Netzwerk**.

2. Geben Sie unter **Virtuelles Netzwerk erstellen** auf der Registerkarte **Grundlegende Einstellungen** die folgenden Informationen ein, oder wählen Sie sie aus:

    | **Einstellung**          | **Wert**                                                           |
    |------------------|-----------------------------------------------------------------|
    | **Projektdetails**  |                                                                 |
    | Abonnement     | Auswählen des Azure-Abonnements                                  |
    | Ressourcengruppe   | Wählen Sie **Neu erstellen** aus, geben Sie **\<Ressourcengruppenname>** ein, und wählen Sie anschließend „OK“ aus. Alternativ kann basierend auf den Parametern auch ein bereits vorhandener **\<Ressourcengruppenname>** ausgewählt werden. |
    | **Instanzendetails** |                                                                 |
    | Name             | Geben Sie **\<VNET-Name>** ein.                                    |
    | Region           | Wählen Sie **\<Regionsname>** aus. |

3. Wählen Sie die Registerkarte **IP-Adressen** oder die Schaltfläche **Weiter: IP-Adressen** am unteren Seitenrand aus.

4. Geben Sie auf der Registerkarte **IP-Adressen** die folgenden Informationen ein:

    | Einstellung            | Wert                      |
    |--------------------|----------------------------|
    | IPv4-Adressraum | Geben Sie **\<IPv4-Adressraum>** ein. |

5. Wählen Sie unter **Subnetzname** das Wort **Standard** aus.

6. Geben Sie unter **Subnetz bearbeiten** die folgenden Informationen ein:

    | Einstellung            | Wert                      |
    |--------------------|----------------------------|
    | Subnetzname | Geben Sie **\<Subnetzname>** ein. |
    | Subnetzadressbereich | Geben Sie **\<Subnetzadressbereich>** ein.

7. Wählen Sie **Speichern** aus.

8. Wählen Sie die Registerkarte **Überprüfen + erstellen** oder die Schaltfläche **Überprüfen + erstellen** aus.

9. Klicken Sie auf **Erstellen**.