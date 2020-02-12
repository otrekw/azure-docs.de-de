---
title: Erstellen eines Azure Bastion-Hosts | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie einen Azure Bastion-Host erstellen.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: cherylmc
ms.openlocfilehash: f907dcb4427fd07a2c212e5de91ccce5e8198960
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/04/2020
ms.locfileid: "76990418"
---
# <a name="create-an-azure-bastion-host"></a>Erstellen eines Azure Bastion-Hosts

In diesem Artikel wird gezeigt, wie Sie mit dem Azure-Portal einen Azure Bastion-Host erstellen. Wenn Sie den Azure Bastion-Dienst in Ihrem virtuellen Netzwerk bereitgestellt haben, steht die nahtlose RDP-/SSH-Funktion für alle virtuellen Computer im selben virtuellen Netzwerk zur Verfügung. Diese Bereitstellung von Azure Bastion erfolgt pro virtuellem Netzwerk und nicht pro Abonnement/Konto oder virtuellem Computer.

Sie können eine neue Bastionhostressource im Portal erstellen, indem Sie alle Einstellungen manuell angeben oder die Einstellungen entsprechend einer vorhandenen VM verwenden. Optional können Sie [Azure PowerShell](bastion-create-host-powershell.md) verwenden, um einen Azure Bastion-Host zu erstellen.

## <a name="before-you-begin"></a>Voraussetzungen

Bastion ist in den folgenden öffentlichen Azure-Regionen verfügbar:

[!INCLUDE [available regions](../../includes/bastion-regions-include.md)]

## <a name="createhost"></a>Erstellen eines Bastionhosts – Angeben von Einstellungen

In diesem Abschnitt erfahren Sie, wie Sie eine neue Azure Bastion-Ressource über das Azure-Portal erstellen.

1. Wählen Sie im Menü des [Azure-Portals](https://portal.azure.com) oder auf der **Startseite** die Option **Ressource erstellen** aus.

1. Geben Sie auf der Seite **Neu** im Feld *Marketplace durchsuchen* den Suchbegriff **Bastion** ein, und drücken Sie anschließend die **EINGABETASTE**, um zu den Suchergebnissen zu gelangen.

1. Klicken Sie in den Ergebnissen auf **Bastion**. Vergewissern Sie sich, dass der Herausgeber *Microsoft* und die Kategorie *Netzwerk* lautet.

1. Klicken Sie auf der Seite **Bastion** auf **Erstellen**, um die Seite **Bastionhost erstellen** zu öffnen.

1. Konfigurieren Sie auf der Seite **Create a bastion** (Erstellen einer Bastion-Ressource) eine neue Bastion-Ressource. Geben Sie die Konfigurationseinstellungen für Ihre Bastion-Ressource an.

    ![Erstellen einer Bastion-Ressource](./media/bastion-create-host-portal/settings.png)

    * **Abonnement**: Das Azure-Abonnement, das Sie zum Erstellen einer neuen Bastion-Ressource verwenden möchten.
    * **Ressourcengruppe**: Die Azure-Ressourcengruppe, in der die neue Bastion-Ressource erstellt wird. Sollte noch keine Ressourcengruppe vorhanden sein, können Sie eine neue Ressourcengruppe erstellen.
    * **Name**: Der Name der neuen Bastion-Ressource.
    * **Region**: Die öffentliche Azure-Region, in der die Ressource erstellt wird.
    * **Virtuelles Netzwerk:** Das virtuelle Netzwerk, in dem die Bastion-Ressource erstellt wird. Im Rahmen dieses Prozesses können Sie über das Portal ein neues virtuelles Netzwerk erstellen oder eines der vorhandenen virtuellen Netzwerke verwenden. Achten Sie bei Verwendung eines bereits vorhandenen virtuellen Netzwerks darauf, dass der freie Adressraum die Anforderungen des Bastion-Subnetzes erfüllt.
    * **Subnetz**: Das Subnetz in Ihrem virtuellen Netzwerk, in dem die neue Bastion-Hostressource bereitgestellt wird. Sie müssen ein Subnetz mit dem Namen **AzureBastionSubnet** erstellen. Dadurch weiß Azure, in welchem Subnetz die Bastion-Ressourcen bereitgestellt werden sollen. Dieses Subnetz unterscheidet sich von einem Gatewaysubnetz. Verwenden Sie mindestens ein Subnetz mit „/27“ oder ein größeres Subnetz („/27“, „/26“ usw.).
    
       Erstellen Sie **AzureBastionSubnet** ohne Routentabellen oder Delegierungen. Weitere Informationen zum Verwenden von Netzwerksicherheitsgruppen in **AzureBastionSubnet** finden Sie im Artikel [Arbeiten mit Netzwerksicherheitsgruppen](bastion-nsg.md).
    * **Öffentliche IP-Adresse:** Die öffentliche IP-Adresse der Bastion-Ressource für den RDP-/SSH-Zugriff (über den Port 443). Erstellen Sie eine neue öffentliche IP-Adresse, oder verwenden Sie eine bereits vorhandene. Die öffentliche IP-Adresse muss sich in der gleichen Region befinden wie die Bastion-Ressource, die Sie erstellen.
    * **Öffentliche IP-Adresse**: Der Name der öffentlichen IP-Adressressource.
    * **SKU der öffentlichen IP-Adresse**: Diese Einstellung ist standardmäßig bereits auf **Standard** festgelegt. Azure Bastion verwendet/unterstützt nur die SKU „Standard“ für die öffentliche IP-Adresse.
    * **Zuweisung**: Diese Einstellung ist standardmäßig bereits auf **Statisch** festgelegt.

1. Klicken Sie nach Angabe der Einstellungen auf **Überprüfen + erstellen**. Daraufhin werden die Werte überprüft. Nach erfolgreicher Überprüfung können Sie mit der Erstellung beginnen.
1. Klicken Sie auf der Seite **Bastion-Instanz erstellen** auf **Erstellen**.
1. Daraufhin wird eine Meldung mit dem Hinweis angezeigt, dass Ihre Bereitstellung ausgeführt wird. Der Status der Ressourcenerstellung wird auf dieser Seite angezeigt. Die Erstellung und Bereitstellung der Bastion-Ressource dauert etwa fünf Minuten.

## <a name="createvmset"></a>Erstellen eines Bastionhosts – Verwenden von VM-Einstellungen

Wenn Sie über das Portal einen Bastionhost unter Verwendung eines bereits vorhandenen virtuellen Computers erstellen, werden verschiedene Einstellungen automatisch auf Standardwerte für Ihren virtuellen Computer und/oder Ihr virtuelles Netzwerk festgelegt.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com). Navigieren Sie zu Ihrem virtuellen Computer, und klicken Sie dann auf **Verbinden**.

   ![Verbindungsherstellung mit virtuellem Computer](./media/bastion-create-host-portal/vmsettings.png)
1. Klicken Sie auf der rechten Seitenleiste auf **Bastion** und anschließend auf **Use Bastion** (Bastion verwenden).

   ![Bastion](./media/bastion-create-host-portal/vmbastion.png)
1. Füllen Sie auf der Seite „Bastion“ die folgenden Einstellungsfelder aus:

   * **Name**: Der Name des zu erstellenden Bastion-Hosts.
   * **Subnetz**: Das Subnetz in Ihrem virtuellen Netzwerk, in dem die Bastion-Ressource bereitgestellt wird. Das Subnetz muss mit dem Namen **AzureBastionSubnet** erstellt werden. Dadurch weiß Azure, in welchem Subnetz die Bastion-Ressource bereitgestellt werden soll. Dieses Subnetz unterscheidet sich von einem Gatewaysubnetz. Verwenden Sie mindestens ein Subnetz mit „/27“ oder ein größeres Subnetz („/27“, „/26“ usw.). Erstellen Sie das Subnetz ohne Netzwerksicherheitsgruppen, Routingtabellen oder Delegierungen. Weitere Informationen zum Verwenden von Netzwerksicherheitsgruppen in **AzureBastionSubnet** finden Sie unter [Arbeiten mit Netzwerksicherheitsgruppen](bastion-nsg.md).
   
     Klicken Sie auf **Subnetzkonfiguration verwalten**, um das **AzureBastionSubnet** zu erstellen.  Klicken Sie auf **Erstellen**, um das Subnetz zu erstellen, und fahren Sie mit den nächsten Einstellungen fort.
   * **Öffentliche IP-Adresse:** Die öffentliche IP-Adresse der Bastion-Ressource für den RDP-/SSH-Zugriff (über den Port 443). Erstellen Sie eine neue öffentliche IP-Adresse, oder verwenden Sie eine bereits vorhandene. Die öffentliche IP-Adresse muss sich in der gleichen Region befinden wie die Bastion-Ressource, die Sie erstellen.
   * **Öffentliche IP-Adresse**: Der Name der öffentlichen IP-Adressressource.
1. Klicken Sie auf dem Überprüfungsbildschirm auf **Erstellen**. Warten Sie etwa fünf Minuten, bis die Erstellung und Bereitstellung der Bastion-Ressource abgeschlossen wurde.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen finden Sie unter [Häufig gestellte Fragen zu Azure Bastion](bastion-faq.md).

* Informationen zur Verwendung von Netzwerksicherheitsgruppen mit dem Azure Bastion-Subnetz finden Sie unter [Arbeiten mit Netzwerksicherheitsgruppen](bastion-nsg.md).