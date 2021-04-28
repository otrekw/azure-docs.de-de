---
title: include file
description: file
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: include
author: mingshen-ms
ms.author: krsh
ms.date: 03/25/2021
ms.openlocfilehash: 8898a762e8a1e7a2d5c104f99d12032c676a5ca4
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105629824"
---
## <a name="generalize-the-image"></a>Generalisieren des Images

Alle Images im Azure Marketplace müssen allgemein wiederverwendbar sein. Um dies zu erreichen, muss die Betriebssystem-VHD generalisiert werden. Dies ist ein Vorgang, bei dem alle instanzspezifischen Bezeichner und Softwaretreiber von einer VM entfernt werden.

### <a name="for-windows"></a>Für Windows

Windows-Betriebssystemdatenträger werden mit dem [Sysprep](/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview)-Tool generalisiert. Wenn Sie das Betriebssystem später aktualisieren oder neu konfigurieren, müssen Sie Sysprep erneut ausführen.

> [!WARNING]
> Fahren Sie die VM nach der Ausführung von Sysprep bis zur Bereitstellung herunter, da u. U. automatische Updates ausgeführt werden. Dadurch verhindern Sie, dass durch nachfolgende Updates instanzspezifische Änderungen am Betriebssystem oder den installierten Diensten vorgenommen werden. Weitere Informationen zum Ausführen von Sysprep finden Sie unter [Schritte zum Generalisieren einer VHD](../../virtual-machines/windows/capture-image-resource.md#generalize-the-windows-vm-using-sysprep).

### <a name="for-linux"></a>Für Linux

Mit dem folgenden Prozess wird eine Linux-VM generalisiert und als separate VM erneut bereitgestellt. Ausführliche Informationen finden Sie unter [Vorgehensweise zum Erstellen eines Images von einem virtuellen Computer oder einer VHD](../../virtual-machines/linux/capture-image.md). Für Sie sind in diesem Fall nur die Informationen bis zum Abschnitt „Bereitstellen eines virtuellen Computers anhand des erfassten Images“ relevant.

1. Entfernen Sie den Azure Linux-Agent.
    1. Stellen Sie mit einem SSH-Client eine Verbindung mit Ihrer Linux-VM her.
    2. Geben Sie im SSH-Fenster den Befehl `sudo waagent –deprovision+user` ein.
    3. Geben Sie Y ein, um fortzufahren (Sie können dem vorherigen Befehl den Parameter -force hinzufügen, um diesen Bestätigungsschritt zu umgehen).
    4. Geben Sie nach der Ausführung des Befehls **Exit** ein, um den SSH-Client zu schließen.
2. Beenden Sie die VM.
    1. Wählen Sie im Azure-Portal Ihre Ressourcengruppe (RG) aus, und heben Sie die Zuordnung der VM auf.
    2. Ihre VM ist jetzt generalisiert, und Sie können mit diesem VM-Datenträger eine neue VM erstellen.

### <a name="capture-image"></a>Erfassen des Images

Sobald Ihre VM bereit ist, können Sie sie in Azure Shared Image Gallery erfassen. Führen Sie hierzu die folgenden Schritte aus:

1. Navigieren Sie im [Azure-Portal](https://ms.portal.azure.com/) zur Seite Ihres virtuellen Computers.
2. Wählen Sie die Option **Erfassen** aus.
3. Klicken Sie unter **Image in Shared Image Gallery freigeben** auf **Ja, als Imageversion für einen Katalog freigeben**.
4. Wählen Sie unter **Betriebssystemstatus** die Option „Generalized“ (Generalisiert) aus.
5. Wählen Sie ein Image Gallery-Ziel aus, oder klicken Sie auf **Neu erstellen**.
6. Wählen Sie eine Zielimagedefinition aus, oder klicken Sie auf **Neu erstellen**.
7. Geben Sie eine **Versionsnummer** für das Image ein.
8. Wählen Sie **Bewerten + erstellen** aus, um Ihre Auswahl zu überprüfen.
9. Klicken Sie auf **Erstellen**, sobald die Überprüfung bestanden wurde.

Zum Veröffentlichen muss sich das Azure-Abonnement, das Shared Image Gallery enthält, unter demselben Mandanten wie das Herausgeberkonto befinden. Außerdem muss das Herausgeberkonto über Besitzerzugriff auf Shared Image Gallery verfügen. 

So gewähren Sie den Zugriff:

1. Rufen Sie Shared Image Gallery auf.
2. Wählen Sie im linken Bereich **Zugriffssteuerung (IAM)** aus.
3. Klicken Sie auf **Hinzufügen**, und wählen Sie dann **Rollenzuweisung hinzufügen** aus.
4. Wählen Sie eine **Rolle** oder einen **Besitzer** aus.
5. Wählen Sie unter **Assign access to** (Zugriff zuweisen zu) die Option **User, group, or service principal** (Benutzer, Gruppe oder Dienstprinzipal) aus.
6. Wählen Sie die Azure-E-Mail-Adresse der Person aus, die das Image veröffentlichen wird.
7. Wählen Sie **Speichern** aus.

:::image type="content" source="../media/create-vm/add-role-assignment.png" alt-text="Screenshot: Fenster „Rollenzuweisung hinzufügen“":::

> [!NOTE]
> Sie müssen keinen URI (Uniform Resource Identifier) generieren, da Sie nun ein Shared Image Gallery-Image in Partner Center veröffentlichen können. Wenn Sie dennoch Informationen zu den Schritten zum Erstellen eines SAS-URIs benötigen, finden Sie diese unter [Generieren eines SAS-URIs für ein VM-Image](../azure-vm-get-sas-uri.md).
