---
title: 'Schnellstart: Konfigurieren einer Firewall für einen Azure Analysis Services-Server | Microsoft-Dokumentation'
description: In diesem Schnellstart erfahren Sie, wie Sie eine Firewall für einen Azure Analysis Services-Server über das Azure-Portal konfigurieren.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: quickstart
ms.date: 05/19/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 48618815519fad31bff5d6a8d2d2edc82535f437
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83697904"
---
# <a name="quickstart-configure-server-firewall---portal"></a>Schnellstart: Konfigurieren der Serverfirewall – Portal

In diesem Schnellstart wird beschrieben, wie Sie eine Firewall für Ihren Azure Analysis Services-Server konfigurieren. Das Aktivieren einer Firewall und das Konfigurieren von IP-Adressbereichen nur für jene Computer, die auf Ihren Server zugreifen, sind ein wichtiger Bestandteil der Sicherung Ihres Servers und Ihrer Daten.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Analysis Services-Server in Ihrem Abonnement. Weitere Informationen finden Sie unter [Schnellstart: Erstellen eines Analysis Services-Servers im Azure-Portal](analysis-services-create-server.md) oder unter [Schnellstart: Erstellen eines Servers mit PowerShell](analysis-services-create-powershell.md).
- Mindestens ein IP-Adressbereich für Clientcomputer (falls erforderlich).
- Einige Szenarien, in denen Power BI Premium eine Verbindung mit Azure Analysis Services herstellt, einschließlich Datenimport (Aktualisierung) und paginierte Berichte, werden derzeit nicht unterstützt. Dies gilt auch, wenn „Zugriff über Power BI zulassen“ aktiviert ist. Das gängigere Szenario mit der Nutzung von Live Connect über Power BI Premium wird unterstützt. Alle Power BI Pro-Szenarien werden unterstützt.


## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an. 

[Anmelden beim Portal](https://portal.azure.com)

## <a name="configure-a-firewall"></a>Konfigurieren einer Firewall

1. Klicken Sie auf Ihren Server, um die Seite „Übersicht“ zu öffnen. 
2. Klicken Sie unter **EINSTELLUNGEN** > **Firewall** > **Firewall aktivieren** auf **Ein**.
3. Klicken Sie in **Zugriff über Power BI zulassen** auf **Ein**, um DirectQuery-Zugriff über Power BI zuzulassen.  
4. (Optional) Geben Sie einen oder mehrere IP-Adressbereiche an. Geben Sie einen Namen sowie eine Start- und End-IP-Adresse für jeden Bereich ein. Der Name einer Firewallregel ist auf 128 Zeichen begrenzt und darf nur aus Großbuchstaben, Kleinbuchstaben, Zahlen, Unterstriche und Bindestriche bestehen. Leer- und andere Sonderzeichen sind nicht zulässig.
5. Klicken Sie auf **Speichern**.

     ![Firewalleinstellungen](./media/analysis-services-qs-firewall/aas-qs-firewall.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie IP-Adressbereiche, oder deaktivieren Sie die Firewall, wenn diese nicht mehr benötigt werden.

## <a name="next-steps"></a>Nächste Schritte
In diesem Schnellstart haben Sie erfahren, wie eine Firewall für Ihren Server konfiguriert wird. Nachdem Sie nun über einen Server verfügen und ihn mit einer Firewall gesichert haben, können Sie ihm ein einfaches Beispieldatenmodell aus dem Portal hinzufügen. Ein Beispielmodell ist hilfreich, um sich mit dem Konfigurieren von Modelldatenbankrollen und dem Testen von Clientverbindungen vertraut zu machen. Fahren Sie mit dem Tutorial zum Hinzufügen eines Beispielmodells fort, um mehr zu erfahren.

> [!div class="nextstepaction"]
> [Tutorial: Hinzufügen eines Beispielmodells zu Ihrem Server](analysis-services-create-sample-model.md)
