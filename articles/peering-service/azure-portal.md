---
title: Registrieren von Azure Peering Service – Azure-Portal
description: Hier erfahren Sie, wie Sie Azure Peering Service über das Azure-Portal registrieren.
services: peering-service
author: derekolo
ms.service: peering-service
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: Infrastructure-services
ms.date: 05/18/2020
ms.author: derekol
ms.openlocfilehash: 62210105ba4ba84cff4b246c5b7cf8cc794fb5e7
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91534946"
---
# <a name="register-peering-service-by-using-the-azure-portal"></a>Registrieren von Peering Service über das Azure-Portal

Azure Peering Service ist ein Netzwerkdienst zur Verbesserung der Kundenkonnektivität mit Microsoft Cloud Services wie Microsoft 365, Dynamics 365, SaaS-Diensten (Software-as-a-Service), Azure und anderen Microsoft-Diensten, auf die über das öffentliche Internet zugegriffen werden kann.

In diesem Artikel erfahren Sie, wie Sie eine Peering Service-Verbindung über das Azure-Portal registrieren.

Sollten Sie über kein Azure-Abonnement verfügen, [erstellen Sie ein Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

> 

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Folgendes:

### <a name="azure-account"></a>Azure-Konto

Sie müssen über ein gültiges und aktives Microsoft Azure-Konto verfügen. Dieses Konto wird zum Einrichten der Peering Service-Verbindung benötigt. Peering Service ist eine Ressource innerhalb von Azure-Abonnements. 

### <a name="connectivity-provider"></a>Konnektivitätsanbieter

Sie können mit einem Internetdienst- oder Internet Exchange-Anbieter zusammenarbeiten, um Peering Service zu erhalten und eine Verbindung zwischen Ihrem Netzwerk und dem Microsoft-Netzwerk herzustellen.

Achten Sie darauf, dass es sich bei den [Konnektivitätsanbietern](location-partners.md) um Microsoft-Partner handelt.



## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Navigieren Sie in einem Browser zum Azure-Portal, und melden Sie sich mit Ihrem Azure-Konto an.

## <a name="register-a-peering-service-connection"></a>Registrieren einer Peering Service-Verbindung

1. Zum Registrieren einer Peering Service-Verbindung wählen Sie **Ressource erstellen** > **Peering Service** aus.

    ![Registrieren von Peering Service](./media/peering-service-portal/peering-servicecreate.png)
1. Geben Sie auf der Registerkarte **Grundlagen** auf der Seite **Erstellen einer Peering Service-Verbindung** die folgenden Details ein.

 
1. Wählen Sie das Abonnement und die Ressourcengruppe aus, das bzw. die dem Abonnement zugeordnet ist.

   ![Registerkarte „Grundlagen“ für die Peeringregistrierung](./media/peering-service-portal/peering-servicebasics.png)

1. Geben Sie im Feld **Name** den Namen ein, auf den die Peering Service-Instanz registriert werden soll.
 
1. Wählen Sie anschließend unten auf der Seite die Schaltfläche **Next:Configuration** aus. Die Seite **Konfiguration** wird angezeigt.

## <a name="configure-the-peering-service-connection"></a>Konfigurieren der Peering Service-Verbindung

1. Wählen Sie auf der Seite **Konfiguration** den Speicherort aus, an dem der Peering Service aktiviert werden muss. Wählen Sie dazu in der Dropdownliste **Peering Service-Speicherort** denselben Speicherort aus.

1. Wählen Sie den Dienstanbieter aus, von dem der Peering Service abgerufen werden muss. Wählen Sie dazu in der Dropdownliste **Peering Service-Anbieter** einen Anbieternamen aus.
 
1. Wählen Sie im unteren Bereich des Abschnitts **Präfixe** die Option **Neues Präfix erstellen** aus. Dann werden Textfelder angezeigt. Geben Sie anschließend den Namen der Präfixressource und die Präfixe ein, die dem Dienstanbieter zugeordnet werden.

1. Wählen Sie **Präfixschlüssel** aus, und fügen Sie den Präfixschlüssel hinzu, der Ihnen von Ihrem Anbieter (ISP oder IXP) zugeteilt wurde. Mit diesem Schlüssel kann MS das Präfix und den Anbieter überprüfen, die Ihr IP-Präfix zugeordnet haben.
   > ![Screenshot, der die Registerkarte „Konfiguration“ der Seite „Peering Service-Verbindung erstellen“ zeigt, auf der Sie den Präfixschlüssel eingeben können.](./media/peering-service-portal/peering-serviceconfiguration.png)

1. Wählen Sie unten links auf der Seite die Schaltfläche **Überprüfen + erstellen** aus. Die Seite **Überprüfen + erstellen** wird angezeigt, und Azure überprüft Ihre Konfiguration.
    

1. Wenn die Meldung **Überprüfung erfolgreich** wie hier angezeigt wird, wählen Sie **Erstellen** aus.

   > ![Screenshot, der die Registerkarte „Überprüfen und erstellen“ der Seite „Peering Service-Verbindung erstellen“ zeigt.](./media/peering-service-portal/peering-service-prefix.png)


1. Nachdem Sie eine Peering Service Verbindung registriert haben, wird für die darin enthaltenen Präfixe eine zusätzliche Prüfung durchgeführt. Sie können den Prüfungsstatus unter dem Abschnitt **Präfixe** des Ressourcennamens überprüfen. Wenn die Überprüfung fehlschlägt, wird eine der folgenden Fehlermeldungen angezeigt:

   - Invalid Peering Service prefix, the prefix should be valid format, only Ipv4 prefix is supported. (Ungültiges Peering Service-Präfix; das Präfix sollte ein gültiges Format haben; unterstützt wird nur das Präfix „Ipv4“.)
   - Prefix was not received from Peering Service provider. (Es wurde kein Präfix vom Peering Service-Anbieter empfangen.)
   - „Prefix announcement does not have a valid BGP community, contact Peering Service provider.“ (Präfixankündigung enthält keine gültige BGP-Community; kontaktieren Sie den Peering Service-Anbieter.)
   - „Backup route not found, contact Peering Service provider.“ (Sicherungsroute nicht gefunden; kontaktieren Sie den Peering Service-Anbieter.)
   - „Prefix received with longer AS path, contact Peering Service provider.“ (Präfix mit längerem AS-Pfad empfangen; kontaktieren Sie den Peering Service-Anbieter.)
   - „Prefix received with private AS in the path, contact Peering Service provider.“ (Präfix mit privatem AS im Pfad empfangen; kontaktieren Sie den Peering Service-Anbieter.)

### <a name="add-or-remove-a-prefix"></a>Hinzufügen oder Entfernen eines Präfixes

Wählen Sie auf der Seite **Präfixe** die Option **Präfixe hinzufügen** aus, um Präfixe hinzuzufügen.

Wählen Sie neben dem aufgeführten Präfix die Auslassungspunkte (...) und dann die Option **Löschen** aus.

### <a name="delete-a-peering-service-connection"></a>Löschen einer Peering Service-Verbindung

Aktivieren Sie auf der Seite **Alle Ressourcen** das Kontrollkästchen für den Peering Service, und wählen Sie oben auf der Seite die Option **Löschen** aus.

> [!NOTE]
> Ein vorhandenes Präfix kann nicht geändert werden.
>

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur Peering Service-Verbindung finden Sie unter [Peering Service-Verbindung](connection.md).
- Informationen zur Peering Service-Verbindungstelemetrie finden Sie unter [Peering Service-Verbindungstelemetrie](connection-telemetry.md).
- Informationen zum Messen der Telemetrie finden Sie unter [Messen der Verbindungstelemetrie](measure-connection-telemetry.md).
- Informationen zum Registrieren der Verbindung mithilfe von Azure PowerShell finden Sie unter [Registrieren einer Peering Service-Verbindung – Azure PowerShell](powershell.md).
- Informationen zum Registrieren der Verbindung über die Azure CLI finden Sie unter [Registrieren einer Peering Service-Verbindung – Azure CLI](cli.md).
