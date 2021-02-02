---
title: 'Windows Virtual Desktop-Hostpool im Azure-Portal: Azure'
description: Hier erfahren Sie, wie Sie einen Windows Virtual Desktop-Hostpool im Azure-Portal erstellen.
author: Heidilohr
ms.topic: tutorial
ms.date: 09/14/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 268f1c4bff84a2c9ef67c3f6b5749bc8d0b90e85
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2021
ms.locfileid: "98735180"
---
# <a name="tutorial-create-a-host-pool-with-the-azure-portal"></a>Tutorial: Erstellen eines Hostpools mit dem Azure-Portal

>[!IMPORTANT]
>Dieser Inhalt gilt für Windows Virtual Desktop mit Windows Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie Windows Virtual Desktop (klassisch) ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/create-host-pools-azure-marketplace-2019.md). Objekte, die Sie mit Windows Virtual Desktop (klassisch) erstellen, können nicht mit dem Azure-Portal verwaltet werden.

Ein Hostpool ist eine Sammlung identischer VMs innerhalb von Windows Virtual Desktop-Umgebungen. Jeder Hostpool kann eine App-Gruppe enthalten, mit der Benutzer genau wie auf einem physischen Desktop interagieren können.

In diesem Artikel wird der Einrichtungsprozess zum Erstellen eines Hostpools für eine Windows Virtual Desktop-Umgebung über das Azure-Portal detailliert beschrieben. Bei dieser Methode verwenden Sie eine browserbasierte Benutzeroberfläche, um einen Hostpool in Windows Virtual Desktop zu erstellen, eine Ressourcengruppe mit VMs unter einem Azure-Abonnement zu erstellen und diese VMs dann in die Azure Active Directory-Domäne (AD) einzubinden und bei Windows Virtual Desktop zu registrieren.

## <a name="prerequisites"></a>Voraussetzungen

Zum Erstellen eines Hostpools müssen Sie die folgenden Parameter eingeben:

- Name des VM-Images
- Konfiguration des virtuellen Computers
- Domänen- und Netzwerkeigenschaften
- Eigenschaften des Windows Virtual Desktop-Hostpools

Außerdem müssen Sie Folgendes wissen:

- Wo befindet sich die Quelle des Images, das Sie verwenden möchten? Stammt es aus dem Azure-Katalog, oder handelt es sich um ein benutzerdefiniertes Image?
- Wo befinden sich Ihre Anmeldeinformationen für den Domänenbeitritt?

Stellen Sie außerdem sicher, dass Sie den Ressourcenanbieter „Microsoft.DesktopVirtualization“ registriert haben. Gehen Sie wie folgt vor, falls Sie dies noch nicht getan haben: Navigieren Sie zu **Abonnements**, und wählen Sie den Namen Ihres Abonnements und dann **Ressourcenanbieter** aus. Suchen Sie nach DesktopVirtualization, und wählen Sie „Microsoft.DesktopVirtualization“ und dann „Registrieren“ aus.

Beim Erstellen eines Windows Virtual Desktop-Hostpools mit der Azure Resource Manager-Vorlage können Sie einen virtuellen Computer über den Azure-Katalog, ein verwaltetes Image oder ein nicht verwaltetes Image erstellen. Weitere Informationen zur Erstellung von VM-Images finden Sie unter [Vorbereiten einer Windows-VHD oder -VHDX zum Hochladen in Azure](../virtual-machines/windows/prepare-for-upload-vhd-image.md) bzw. unter [Erstellen eines verwalteten Images eines generalisierten virtuellen Computers in Azure](../virtual-machines/windows/capture-image-resource.md).

Wenn Sie noch kein Azure-Abonnement haben, müssen Sie [ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), bevor Sie diese Anweisungen ausführen.

## <a name="begin-the-host-pool-setup-process"></a>Erste Schritte des Einrichtungsprozesses für den Hostpool

Zum Erstellen des neuen Hostpools führen Sie zunächst die folgenden Schritte aus:

1. Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com/) beim Azure-Portal an.
   
   >[!NOTE]
   > Wenn Sie sich beim US Gov-Portal anmelden, wechseln Sie stattdessen zu [https://portal.azure.us/](https://portal.azure.us/).

2. Geben Sie **Windows Virtual Desktop** in die Suchleiste ein, und wählen Sie dann unter „Dienste“ den Eintrag **Windows Virtual Desktop** aus.

3. Wählen Sie auf der Übersichtsseite für **Windows Virtual Desktop** die Option **Hostpool erstellen** aus.

4. Wählen Sie auf der Registerkarte **Grundlagen** unter „Projektdetails“ das richtige Abonnement aus.

5. Klicken Sie entweder auf **Neu erstellen**, um eine neue Ressourcengruppe zu erstellen, oder wählen Sie im Dropdownmenü eine vorhandene Ressourcengruppe aus.

6. Geben Sie einen eindeutigen Namen für Ihren Hostpool ein.

7. Wählen Sie im Feld „Standort“ im Dropdownmenü die Region aus, in der Sie den Hostpool erstellen möchten.

   Die Metadaten für diesen Hostpool und die zugehörigen Objekte werden in der Azure-Geografie gespeichert, die den von Ihnen ausgewählten Regionen zugeordnet ist. Stellen Sie sicher, dass Sie die Regionen innerhalb der Geografie auswählen, in der die Dienstmetadaten gespeichert werden sollen.

     > [!div class="mx-imgBorder"]
     > ![Screenshot des Azure-Portals, der das Feld „Standort“ mit ausgewählter Region „USA, Osten“ zeigt. Neben dem Feld wird „Metadaten werden in ‚USA, Osten‘ gespeichert“ angezeigt.](media/portal-location-field.png)

8. Wählen Sie unter „Host pool type“ (Hostpooltyp) die Option **Persönlich** oder **In Pool** für Ihren Hostpool aus.

    - Wählen Sie bei Auswahl von **Persönlich** im Feld „Zuweisungstyp“ die Option **Automatisch** oder **Direkt** aus.

      > [!div class="mx-imgBorder"]
      > ![Screenshot des Dropdownmenüs „Zuweisungstyp“. Der Benutzer hat „Automatisch“ ausgewählt.](media/assignment-type-field.png)

9.  Geben Sie bei Auswahl von **In Pool** die folgenden Informationen ein:

     - Geben Sie für **Maximale Anzahl von Sitzungen** die Höchstanzahl von Benutzern ein, für die ein Lastenausgleich auf einem einzelnen Sitzungshost durchgeführt werden soll.
     - Wählen Sie für **Lastenausgleichsalgorithmus** je nach Nutzungsmuster „Breitenorientierter Lastenausgleich“ oder „Tiefenorientierter Lastenausgleich“ aus.

       > [!div class="mx-imgBorder"]
       > ![Screenshot des Dropdownmenüs „Zuweisungstyp“ mit ausgewählter Option „In Pool“. Der Benutzer zeigt mit dem Cursor auf die Option „Breitenorientierter Lastenausgleich“ im Dropdownmenü „Lastenausgleichsalgorithmus“.](media/pooled-assignment-type.png)

10. Klicken Sie auf **Weiter: Virtuelle Computer >** .

11. Wenn Sie bereits virtuelle Computer erstellt haben und diese mit dem neuen Hostpool verwenden möchten, wählen Sie **Nein** und dann **Weiter: Arbeitsbereich >** aus, und wechseln Sie zum Abschnitt [Arbeitsbereichsinformationen](#workspace-information). Möchten Sie neue VMs erstellen und beim neuen Hostpool registrieren, wählen Sie **Ja** aus.

Damit ist der erste Teil abgeschlossen. Im nächsten Teil des Einrichtungsprozesses erstellen Sie die VM.

## <a name="virtual-machine-details"></a>Details zum virtuellen Computer

Nachdem Sie den ersten Teil abgeschlossen haben, müssen Sie nun Ihre VM einrichten.

So richten Sie Ihre VM im Rahmen des Einrichtungsprozesses für den Hostpool ein:

1. Wählen Sie unter **Ressourcengruppe** die Ressourcengruppe aus, in der Sie die virtuellen Computer erstellen möchten. Dabei kann es sich um eine andere Ressourcengruppe als die für den Hostpool handeln.

2. Wählen Sie den **VM-Standort** aus, in dem Sie die virtuellen Computer erstellen möchten. Sie können die für den Hostpool ausgewählte Region oder eine andere Region verwenden.

3. Wählen Sie als Nächstes die **VM-Größe** aus. Sie können entweder die Standardgröße übernehmen oder **Größe ändern** auswählen, um die Größe zu ändern. Wenn Sie auf **Größe ändern** klicken, wählen Sie im angezeigten Fenster die passende VM-Größe für Ihre Workload aus.

4. Geben Sie unter **Anzahl von VMs** die Anzahl von virtuellen Computern an, die Sie für Ihren Hostpool erstellen möchten.

    >[!NOTE]
    >Sie können während der Einrichtung Ihres Hostpools bis zu 400 VMs erstellen, und bei jedem VM-Einrichtungsprozess werden vier Objekte in Ihrer Ressourcengruppe erstellt. Ihr Abonnementkontingent wird bei der Erstellung nicht überprüft. Achten Sie daher darauf, dass die von Ihnen angegebene Anzahl virtueller Computer den Azure-VM- und API-Grenzwerten für Ihre Ressourcengruppe und Ihr Abonnement entspricht. Sie können nach der Erstellung des Hostpools weitere VMs hinzufügen.

5. Geben Sie anschließend ein **Namenspräfix** für die Benennung der VMs an, die beim Einrichtungsprozess erstellt werden. Das Suffix ist `-` mit Zahlen ab 0.

6. Wählen Sie nun das Image aus, das zum Erstellen der VM verwendet werden muss. Sie können **Katalog** oder **Speicherblob** auswählen.

    - Wählen Sie bei Auswahl von **Katalog** im Dropdownmenü eines der empfohlenen Images aus:

      - Windows 10 Enterprise (mehrere Sitzungen), Version 1909
      - Windows 10 Enterprise (mehrere Sitzungen), Version 1909 + Microsoft 365 Apps
      - Windows Server 2019 Datacenter
      - Windows 10 Enterprise (mehrere Sitzungen), Version 2004
      - Windows 10 Enterprise (mehrere Sitzungen), Version 2004 + Microsoft 365 Apps

     Falls das gewünschte Image nicht angezeigt wird, wählen Sie **Alle Images und Datenträger durchsuchen** aus. Sie können dann ein anderes Image in Ihrem Katalog oder ein von Microsoft und anderen Herausgebern bereitgestelltes Image auswählen. Wählen Sie unbedingt eines der [unterstützten Betriebssystemimages](overview.md#supported-virtual-machine-os-images) aus.

     > [!div class="mx-imgBorder"]
     > ![Screenshot des Marketplace mit einer Liste von Images von Microsoft.](media/marketplace-images.png)

     Sie können auch zu **Meine Elemente** wechseln und ein benutzerdefiniertes Image auswählen, das Sie bereits hochgeladen haben.

     > [!div class="mx-imgBorder"]
     > ![Screenshot der Registerkarte „Meine Elemente“.](media/my-items.png)

    - Bei Auswahl von **Speicherblob** können Sie Ihren eigenen Imagebuild über Hyper-V oder auf einer Azure-VM nutzen. Sie müssen lediglich im Speicherblob den Speicherort des Images als URI eingeben.

7. Wählen Sie den gewünschten Typ für die Betriebssystemdatenträger Ihrer VMs aus: „SSD Standard“, „SSD Premium“ oder „HDD Standard“.

8. Wählen Sie unter „Netzwerk und Sicherheit“ das **virtuelle Netzwerk** und das **Subnetz** aus, in dem Sie die von Ihnen erstellten virtuellen Computer platzieren möchten. Stellen Sie sicher, dass das virtuelle Netzwerk eine Verbindung mit dem Domänencontroller herstellen kann, da Sie die VMs im virtuellen Netzwerk der Domäne hinzufügen müssen. Die DNS-Server des virtuellen Netzwerks, das Sie ausgewählt haben, sollten für die Verwendung der IP-Adresse des Domänencontrollers konfiguriert werden.

9. Wählen Sie als Nächstes aus, ob die VMs eine öffentliche IP-Adresse haben sollen. Sie sollten **Nein** auswählen, weil eine private IP-Adresse sicherer ist.

10. Wählen Sie den gewünschten Sicherheitsgruppentyp aus: **Basic**, **Erweitert** oder **Keine**.

    Bei Auswahl von **Basic** müssen Sie auswählen, ob ein Eingangsport geöffnet sein soll. Wählen Sie bei Auswahl von **Ja** in der Liste der Standardports einen Port aus, um eingehende Verbindungen zuzulassen.

    >[!NOTE]
    >Aus Sicherheitsgründen empfehlen wir, öffentliche Eingangsports nicht zu öffnen.

    > [!div class="mx-imgBorder"]
    > ![Screenshot der Seite „Sicherheitsgruppe“ mit einer Liste verfügbarer Ports in einem Dropdownmenü.](media/available-ports.png)

    Wählen Sie bei Auswahl von **Erweitert** eine vorhandene Netzwerksicherheitsgruppe aus, die Sie bereits konfiguriert haben.

11. Wählen Sie anschließend aus, ob die VMs einer bestimmten Domäne und Organisationseinheit beitreten sollen. Geben Sie bei Auswahl von **Ja** die jeweilige Domäne an. Sie können optional eine spezifische Organisationseinheit hinzufügen, in der die virtuellen Computer enthalten sein sollen. Wenn Sie **Nein** auswählen, werden die virtuellen Computer mit der Domäne verknüpft, die dem Suffix des **UPN für AD-Domänenbeitritt** entspricht.

  - Stellen Sie beim Angeben einer Organisationseinheit sicher, dass Sie den vollständigen Pfad (Distinguished Name) ohne Anführungszeichen verwenden.

12. Geben Sie unter „Administratorkonto“ die Anmeldeinformationen für den Active Directory-Domänenadministrator des virtuellen Netzwerks an, das Sie ausgewählt haben. Für dieses Konto kann die mehrstufige Authentifizierung(MFA) nicht aktiviert werden. Bei der Einbindung in eine Azure Active Directory Domain Services-Domäne (Azure AD DS) muss das Konto Teil der Gruppe „Azure AD DC-Administratoren“ sein, und das Kontokennwort muss in Azure AD DS gültig sein.

13. Klicken Sie auf **Weiter: Arbeitsbereich >** .

Damit sind Sie bereit für die nächste Phase der Einrichtung Ihres Hostpools: Registrieren Ihrer App-Gruppe in einem Arbeitsbereich.

## <a name="workspace-information"></a>Informationen zum Arbeitsbereich

Beim Einrichtungsprozess für den Hostpool wird standardmäßig eine Desktopanwendungsgruppe erstellt. Damit der Hostpool wie beabsichtigt funktioniert, müssen Sie diese App-Gruppe für Benutzer oder Benutzergruppen veröffentlichen und die App-Gruppe in einem Arbeitsbereich registrieren.

So registrieren Sie die Desktop-App-Gruppe in einem Arbeitsbereich:

1. Wählen Sie **Ja** aus.

   Wenn Sie **Nein** auswählen, können Sie die App-Gruppe zu einem späteren Zeitpunkt registrieren. Wir empfehlen jedoch, die Registrierung im Arbeitsbereich so schnell wie möglich zu erledigen, damit Ihr Hostpool korrekt funktioniert.

2. Wählen Sie aus, ob Sie einen neuen Arbeitsbereich erstellen oder einen vorhandenen Arbeitsbereich verwenden möchten. Die App-Gruppe kann nur in Arbeitsbereichen registriert werden, die am gleichen Standort wie der Hostpool erstellt werden.

3. Optional können Sie **Weiter: Tags >** auswählen.

    Hier können Sie Tags hinzufügen, damit Sie die Objekte mit Metadaten gruppieren und dadurch Ihren Administratoren die Arbeit erleichtern können.

4. Wählen Sie abschließend **Überprüfen + erstellen** aus.

     >[!NOTE]
     >Beim Überprüfungsprozess „Überprüfen + erstellen“ wird nicht überprüft, ob Ihr Kennwort den Sicherheitsstandards entspricht oder Ihre Architektur korrekt ist. Sie müssen daher selbst überprüfen, ob in dieser Hinsicht Probleme vorliegen.

5. Überprüfen Sie die Informationen zu Ihrer Bereitstellung, um sicherzustellen, dass alles richtig ist. Wählen Sie **Erstellen**, wenn Sie fertig sind. Dadurch wird der Bereitstellungsprozess gestartet, bei dem die folgenden Objekte erstellt werden:

     - Ihr neuer Hostpool.
     - Eine Desktop-App-Gruppe.
     - Ein Arbeitsbereich (sofern Sie keinen vorhandenen Arbeitsbereich verwenden).
     - Wenn Sie sich für die Registrierung der Desktop-App-Gruppe entschieden haben, wird die Registrierung durchgeführt.
     - VMs (sofern Sie sich für deren Erstellung entschieden haben), die der Domäne hinzugefügt und beim neuen Hostpool registriert werden.
     - Ein Downloadlink für eine Azure-Ressourcenverwaltungsvorlage, die auf Ihrer Konfiguration basiert.

Danach sind Sie fertig!

## <a name="run-the-azure-resource-manager-template-to-provision-a-new-host-pool"></a>Ausführen der Azure Resource Manager-Vorlage zum Bereitstellen eines neuen Hostpools

Wenn Sie lieber einen automatisierten Prozess verwenden möchten, [laden Sie unsere Azure Resource Manager-Vorlage herunter](https://github.com/Azure/RDS-Templates/tree/master/ARM-wvd-templates), um stattdessen Ihren neuen Hostpool bereitzustellen.

>[!NOTE]
>Wenn Sie einen automatisierten Prozess zum Erstellen Ihrer Umgebung nutzen, benötigen Sie die aktuelle Version der JSON-Konfigurationsdatei. Die JSON-Datei finden Sie [hier](https://wvdportalstorageblob.blob.core.windows.net/galleryartifacts?restype=container&comp=list).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihren Hostpool erstellt haben, können Sie ihn nun mit RemoteApp-Programmen auffüllen. Weitere Informationen zum Verwalten von Apps in Windows Virtual Desktop finden Sie in unserem nächsten Tutorial:

> [!div class="nextstepaction"]
> [Manage app groups for Windows Virtual Desktop Preview](./manage-app-groups.md) (Verwalten von App-Gruppen für Windows Virtual Desktop (Vorschauversion))
