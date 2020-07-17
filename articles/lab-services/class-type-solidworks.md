---
title: Einrichten eines SolidWorks-Labs für Engineering mit Azure Lab Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe von SolidWorks ein Lab für Engineeringkurse einrichten.
author: nicolela
ms.topic: article
ms.date: 06/26/2020
ms.author: nicolela
ms.openlocfilehash: fa1b93bd71c1319bf8705c8c84cdb3e6f9da19e2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85443807"
---
# <a name="set-up-a-lab-for-engineering-classes-using-solidworks"></a>Einrichten eines Labs für Engineeringkurse mithilfe von SolidWorks

[SolidWorks](https://www.solidworks.com/) stellt eine 3D-CAD-Umgebung (Computer-Aided Design) bereit, um solide Objekte zu modellieren, und wird in verschiedenen Engineeringbereichen verwendet.  Mit SolidWorks können Engineers ihre Entwürfe problemlos erstellen, visualisieren, simulieren und dokumentieren.

Eine von Universitäten häufig verwendete Lizenzierungsoption ist die Netzwerklizenzierung von SolidWorks.   Mit dieser Option verwenden Benutzer einen Pool von Lizenzen gemeinsam, die von einem Lizenzierungsserver verwaltet werden.  Diese Art von Lizenz wird manchmal als „übergreifende“ Lizenz bezeichnet, da Sie nur über genügend Lizenzen für die Anzahl gleichzeitiger Benutzer verfügen müssen.  Wenn ein Benutzer seine Arbeit mit SolidWorks beendet hat, geht die Lizenz wieder in den zentral verwalteten Lizenzpoolein, sodass sie von einem anderen Benutzer wiederverwendet werden kann.

In diesem Artikel erfahren Sie, wie Sie einen Kurs einrichten, in dem SolidWorks 2019 und Netzwerklizenzierung verwendet wird.

## <a name="license-server"></a>Lizenzserver

SolidWorks-Netzwerklizenzierung erfordert, dass Sie den Lizenz-Manager von SolidNetWork installiert und auf dem Lizenzserver aktiviert haben.  Dieser Lizenzserver befindet sich in der Regel in Ihrem lokalen Netzwerk oder in einem privaten Netzwerk in Azure.  Weitere Informationen zum Einrichten des Lizenz-Managers von SolidNetWork auf Ihrem Server finden Sie unter [Installieren und Aktivieren eines Lizenz-Managers](https://help.solidworks.com/2019/English/Installation/install_guide/t_installing_snl_lic_mgr.htm) im SolidWorks-Installationsleitfaden.  Notieren Sie sich beim Einrichten die **Portnummer** und die [**Seriennummer**](https://help.solidworks.com/2019/english/installation/install_guide/r_hid_state_serial_number.htm), die verwendet werden, da beide Angaben in späteren Schritten benötigt werden.

Nachdem der Lizenzserver eingerichtet wurde, müssen Sie ein Peering des [virtuellen Netzwerks (VNET)](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-connect-peer-virtual-network) mit Ihrem [Lab-Konto](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-lab-account) ausführen.  Das Netzwerkpeering muss vor dem Erstellen des Labs erfolgen, damit die virtuellen Lab-Computer auf den Lizenzserver zugreifen können und umgekehrt.

> [!NOTE]
> Vergewissern Sie sich, dass die entsprechenden Ports Ihrer Firewalls geöffnet sind, um die Kommunikation zwischen den virtuellen Lab-Computern und dem Lizenzserver zu erlauben.  Informationen hierzu finden Sie beispielsweise in den Anleitungen unter [Ändern der Lizenz-Manager-Computerports für die Windows-Firewall](http://help.solidworks.com/2019/english/installation/install_guide/t_mod_ports_on_lic_mgr_for_firewall.htm), die zeigen, wie der Firewall des Lizenzservers Eingangs- und Ausgangsregeln hinzugefügt werden.  Möglicherweise müssen Sie auch Ports für die virtuellen Lab-Computer öffnen.  Führen Sie die Schritte im Artikel [Firewalleinstellungen für Labs](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-firewall-settings) aus, um weitere Informationen zu diesem Thema zu erhalten, einschließlich Informationen dazu, wie Sie die öffentliche IP-Adresse des Labs abrufen.

## <a name="lab-configuration"></a>Labkonfiguration

Zum Einrichten dieses Labs benötigen Sie zunächst ein Azure-Abonnement und ein Lab-Konto. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen. Sobald Sie ein Azure-Abonnement erhalten, können Sie ein neues Lab-Konto in Azure Lab Services erstellen. Weitere Informationen zum Erstellen eines neuen Lab-Kontos finden Sie im Tutorial zum [Einrichten eines Lab-Kontos](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-lab-account). Sie können auch ein vorhandenes Lab-Konto verwenden.

### <a name="lab-account-settings"></a>Lab-Kontoeinstellungen

Aktivieren Sie die Einstellungen für das Lab-Konto, die in der nachfolgenden Tabelle beschrieben werden. Weitere Informationen zum Aktivieren von Marketplace-Images finden Sie im Artikel zum [Angeben von für Lab-Ersteller verfügbaren Marketplace-Images](https://docs.microsoft.com/azure/lab-services/classroom-labs/specify-marketplace-images).

| Lab-Kontoeinstellungen | Instructions |
| ------------------- | ------------ |
|Marketplace-Image| Aktivieren Sie das Windows 10 Pro-Image zur Verwendung in Ihrem Lab-Konto.|

> [!NOTE]
> Zusätzlich zu Windows 10 unterstützt SolidWorks auch andere Versionen von Windows.  Weitere Informationen hierzu finden Sie unter [Systemanforderungen für SolidWorks](https://www.solidworks.com/sw/support/SystemRequirements.html).

### <a name="lab-settings"></a>Lab-Einstellungen

Verwenden Sie beim Einrichten eines Classroom-Labs die Einstellungen in der unten stehenden Tabelle. Weitere Informationen zum Erstellen eines Classroom-Labs finden Sie im Tutorial zum Einrichten eines Classroom-Labs.

| Lab-Einstellungen | Wert/Anweisungen |
| ------------ | ------------------ |
|VM-Größe| **Kleine GPU (Visualisierung)** .  Diese VM eignet sich am besten für Remotevisualisierung, Streaming, Gaming und Codierung mit Frameworks wie OpenGL und DirectX.|  
|VM-Image| Windows 10 Pro|

> [!NOTE]
> Die VM-Größe **Kleine GPU (Visualisierung)** ist so konfiguriert, dass eine leistungsstarke Grafikdarstellung ermöglicht wird.  Weitere Informationen zu dieser VM-Größe finden Sie im Artikel zum [Einrichten eines Labs mit GPUs](./how-to-setup-lab-gpu.md).

> [!WARNING]
> Vergessen Sie nicht, **vor** dem Erstellen des Labs ein [Peering für das virtuelle Netzwerk](https://www.mathworks.com/support/requirements/matlab-system-requirements.html) für das Lab-Konto und das virtuelle Netzwerk für den Lizenzserver durchzuführen.

## <a name="template-virtual-machine-configuration"></a>Vorlage für die Konfiguration des virtuellen Computers

Die Schritte in diesem Abschnitt veranschaulichen, wie Sie die Vorlagen-VM einrichten, indem Sie die SolidWorks-Installationsdateien herunterladen und die Clientsoftware installieren:

1. Starten Sie die Vorlagen-VM, und stellen Sie über RDP eine Verbindung mit dem Computer her.

1. Laden Sie die Installationsdateien für die SolidWorks-Clientsoftware herunter. Für den Download stehen zwei Optionen zur Verfügung:
   - Download aus dem [SolidWorks-Kundenportal](https://login.solidworks.com/nidp/idff/sso?id=cpenglish&sid=1&option=credential&sid=1&target=https%3A%2F%2Fcustomerportal.solidworks.com%2F).
   - Download aus einem Verzeichnis auf einem Server.  Wenn Sie diese Option verwendet haben, müssen Sie sicherstellen, dass die Vorlagen-VM auf den Server zugreifen kann.  Beispielsweise befindet sich dieser Server möglicherweise im gleichen virtuellen Netzwerk, für das Peering mit Ihrem Lab-Konto ausgeführt wurde.
  
    Weitere Informationen finden Sie unter [Installation auf einzelnen Computern in SolidWorks](http://help.solidworks.com/2019/english/Installation/install_guide/c_installing_on_individual_computers.htm?id=fc149e8a968a422a89e2a943265758d3#Pg0) im SolidWorks-Installationsleitfaden.

1. Nachdem Sie die Installationsdateien heruntergeladen haben, installieren Sie die Clientsoftware mit dem SolidWorks-Installations-Manager. Weitere Informationen finden Sie unter [Installieren eines Lizenzclients](http://help.solidworks.com/2019/english/installation/install_guide/t_installing_snl_license_client.htm) im SolidWorks-Installationsleitfaden.

    > [!NOTE]
    > Im Dialogfeld **Server hinzufügen** werden Sie aufgefordert, die **Portnummer** anzugeben, die für Ihren Lizenzserver verwendet wird, sowie die IP-Adresse des Lizenzservers.

## <a name="cost"></a>Kosten

Betrachten wir eine mögliche Kostenschätzung für diesen Kurs. Diese Schätzung umfasst nicht die Kosten für das Ausführen des Lizenzservers. Wir setzen einen Kurs mit 25 Kursteilnehmern an. Es ist eine Kursdauer von 20 Stunden geplant. Zudem erhält jeder Kursteilnehmer ein Kontingent von 10 Stunden für Hausaufgaben und Aufgaben außerhalb der regulären Kurszeiten. Als VM-Größe wurde **Kleine GPU (Visualisierung)** ausgewählt, was 160 Lab-Einheiten entspricht.

25 Kursteilnehmer \* (20 geplante Stunden + 10 Kontingentstunden) \* 160 Lab-Einheiten * 0,01 USD pro Stunde = 1.200,00 USD

>[!IMPORTANT]
> Diese Kostenschätzung dient ausschließlich zu Beispielzwecken.  Weitere Informationen zu den aktuellen Preisen finden Sie unter [Azure Lab Services – Preise](https://azure.microsoft.com/pricing/details/lab-services/).  

## <a name="next-steps"></a>Nächste Schritte

Die nächsten Einrichtungsschritte sind für alle Labs gleich.

- [Erstellen und Verwalten einer Vorlage](how-to-create-manage-template.md)
- [Hinzufügen von Benutzern](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Kontingent festlegen](how-to-configure-student-usage.md#set-quotas-for-users)
- [Zeitplan festlegen](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab)
- [Registrierungslinks per E-Mail an Kursteilnehmer senden](how-to-configure-student-usage.md#send-invitations-to-users)
