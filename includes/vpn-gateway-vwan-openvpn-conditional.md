---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/14/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 410570302eec418f1e4bcb75d6413936a96b5171
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "77471521"
---
Der bedingte Zugriff ermöglicht eine differenzierte Zugriffssteuerung auf Anwendungsbasis. Für die Nutzung des bedingten Zugriffs sollten Sie mindestens eine Azure AD Premium 1-Lizenz auf die Benutzer anwenden, die den Regeln des bedingten Zugriffs unterliegen.

1. Navigieren Sie zur Seite **Unternehmensanwendungen – Alle Anwendungen**, und klicken Sie auf **Azure-VPN**.

   - Klicken Sie auf **Bedingter Zugriff**.
   - Klicken Sie auf **Neue Richtlinie**, um den Bereich **Neu** zu öffnen.
2. Navigieren Sie im Bereich **Neu** zu **Zuweisungen > Benutzer und Gruppen**. Gehen Sie auf der Registerkarte **Benutzer und Gruppen** > **Einschließen** wie folgt vor:

   - Wählen Sie **Benutzer und Gruppen auswählen**.
   - Aktivieren Sie **Benutzer und Gruppen**.
   - Klicken Sie auf **Auswählen**, um eine Gruppe oder eine Gruppe von Benutzern auszuwählen, für die MFA gelten soll.
   - Klicken Sie auf **Fertig**.

   ![Zuweisungen](./media/vpn-gateway-vwan-openvpn-azure-ad-mfa/mfa-ca-assignments.png)
3. Navigieren Sie im Bereich **Neu** zum Bereich **Zugriffssteuerungen > Gewähren**:

   - Klicken Sie auf **Zugriff gewähren**.
   - Aktivieren Sie **Mehrstufige Authentifizierung erforderlich**.
   - Aktivieren Sie **Alle ausgewählten Kontrollen anfordern**.
   - Klicken Sie auf **Auswählen**.
   
   ![Zugriff gewähren – MFA](./media/vpn-gateway-vwan-openvpn-azure-ad-mfa/mfa-ca-grant-mfa.png)
4. Gehen Sie im Abschnitt **Richtlinie aktivieren** wie folgt vor:

   - Wählen Sie **Ein** aus.
   - Klicken Sie auf **Erstellen**.

   ![Richtlinie aktivieren](./media/vpn-gateway-vwan-openvpn-azure-ad-mfa/mfa-ca-enable-policy.png)