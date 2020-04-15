---
title: Installieren von Language Packs auf Windows 10-VMs in Windows Virtual Desktop – Azure
description: Hier erfahren Sie, wie Sie Language Packs für Windows 10-VMs (mehrere Sitzungen) in Windows Virtual Desktop installieren.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/03/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: c40df9d821e069e2cd5ff0c42d5841f6b9041c96
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2020
ms.locfileid: "80634131"
---
# <a name="install-language-packs"></a>Installieren von Sprachpaketen

Wenn Sie Windows Virtual Desktop-Bereitstellungen für eine internationale Umgebung einrichten, sollten Sie sicherstellen, dass Ihre Bereitstellung mehrere Sprachen unterstützt. Sie können Language Packs auf dem Image eines virtuellen Windows 10 Enterprise, Multisession-Computers installieren, um so viele Sprache zu unterstützen, wie in der Organisation benötigt werden. In diesem Artikel erfahren Sie, wie Sie Language Packs installieren und Images erfassen, anhand derer Ihre Benutzer ihre eigenen Anzeigesprachen auswählen können.

Weitere Informationen zum Bereitstellen einer VM in Azure erfahren Sie unter [Erstellen eines virtuellen Windows-Computers in einer Verfügbarkeitszone mit dem Azure-Portal](../virtual-machines/windows/create-portal-availability-zone.md).

>[!NOTE]
>Dieser Artikel bezieht sich auf virtuelle Windows 10 Enterprise-VMs (mehrere Sitzungen).

## <a name="install-a-language-pack"></a>Installieren eines Language Packs

Zum Erstellen eines VM-Images mit Language Packs müssen Sie zunächst Language Packs auf einem Computer installieren und ein Image davon erfassen.

So installieren Sie Language Packs:

1. Melden Sie sich als Administrator an.
2. Stellen Sie sicher, dass Sie alle aktuellen Windows-und Windows Store-Updates installiert haben.
3. Wechseln Sie zu **Einstellungen** > **Zeit und Sprache** > **Region**.
4. Wählen Sie unter **Land oder Region**im Dropdownmenü das gewünschte Land oder die gewünschte Region aus.
    In diesem Beispiel wählen wir **Frankreich**aus, wie im folgenden Screenshot zu sehen:

    ![Screenshot der Seite „Region“. Die derzeit ausgewählte Region ist Frankreich.](media/region-page-france.png)

5. Wählen Sie anschließend **Sprache**und dann **Sprache hinzufügen**aus. Wählen Sie die zu installierende Sprache in der Liste aus, und wählen Sie **Weiter** aus.
6. Das Fenster **Sprachfeatures installieren** wird geöffnet. Aktivieren Sie das Kontrollkästchen **Language Pack installieren und als Windows-Anzeigesprache festlegen**.
7. Wählen Sie **Installieren** aus.
8. Wenn Sie mehrere Sprachen gleichzeitig hinzufügen möchten, wählen Sie **Sprache hinzufügen** aus, und führen Sie den Vorgang zum Hinzufügen einer Sprache in den Schritten 5 und 6 erneut aus. Wiederholen Sie den Vorgang für jede Sprache, die Sie hinzufügen möchten. Sie können jedoch jeweils nur eine Sprache als Anzeigesprache festlegen.

    Dies wird im Folgenden kurz veranschaulicht. In den folgenden Abbildungen wird gezeigt, wie Sie die Sprachpakete Französisch und Niederländisch installieren und dann Französisch als Anzeigesprache festlegen.

    ![Screenshot der Seite „Sprache“ zu Beginn des Vorgangs. Die ausgewählte Windows-Anzeigesprache ist Englisch.](media/language-page-default.png)

    ![Screenshot des Fensters für die Sprachauswahl. Der Benutzer hat „French“ (Französisch) in die Suchleiste eingegeben, um die Language Packs für Französisch zu suchen.](media/select-language-french.png)

    ![Screenshot der Seite „Sprachfeatures installieren“. Französisch ist als bevorzugte Sprache ausgewählt. Die Optionen „Anzeigesprache festlegen“, „Sprachpaket installieren“, „Spracherkennung“ und „Handschrift“ sind ausgewählt.](media/install-language-features.png)

    Nachdem Sie die Language Packs installiert haben, sollten die Namen der Language Packs in der Liste der Sprachen angezeigt werden.

    ![Screenshot der Seite „Sprache“ mit den installierten neuen Language Packs. Die Sprachpakete Französisch und Niederländisch werden unter „Bevorzugte Sprachen“ aufgeführt.](media/language-page-complete.png)

9. Ein Fenster wird geöffnet, in dem Sie aufgefordert werden, sich von ihrer Sitzung abzumelden. Gehen Sie wie folgt vor: Melden Sie sich ab, und melden Sie sich erneut an. Ihre Anzeigesprache sollte nun der ausgewählten Sprache entsprechen.

10.  Wechseln Sie zu **Systemsteuerung** > **Zeit und Region** > **Region**.

11.  Wenn das Fenster **Region** geöffnet wird, wählen Sie die Registerkarte **Verwaltung** aus, und wählen Sie dann **Einstellungen kopieren**aus.

12.  Aktivieren Sie die Kontrollkästchen **Willkommensseite und Systemkonten** und **Neue Benutzerkonten**.

13.  Klicken Sie auf **OK**.

14.  Ein Fenster wird geöffnet, in dem Sie angewiesen werden, die Sitzung neu zu starten. Klicken Sie auf **Jetzt neu starten**.

15.  Nachdem Sie sich erneut angemeldet haben, wechseln Sie wieder zu **Systemsteuerung** > **Zeit und Region** > **Region**.

16.  Wählen Sie die Registerkarte **Verwaltung** aus.

17.  Wählen Sie **Gebietsschema ändern** aus.

18. Wählen Sie im Dropdownmenü unter **Aktuelles Gebietsschema** das gewünschte Gebietsschema aus. Wählen Sie anschließend **OK** aus.

19. Wählen Sie **Jetzt neu starten** aus, um die Sitzung erneut zu starten.

Herzlichen Glückwunsch, Sie haben Ihre Language Packs installiert!

Bevor Sie fortfahren, vergewissern Sie sich, dass in Ihrem System die neuesten Versionen von Windows und Windows Store installiert sind.

## <a name="sysprep"></a>Sysprep

Nun müssen Sie Sysprep für Ihren Computer ausführen, um ihn für den Image-Erfassungsvorgang vorzubereiten.

So führen Sie Sysprep für Ihren Computer aus:

1. Öffnen Sie PowerShell als Administrator.
2. Führen Sie das folgende Cmdlet aus, um zum richtigen Verzeichnis zu wechseln:
   
    ```powershell
    cd Windows\System32\Sysprep
    ```

3. Führen Sie anschließend das folgende Cmdlet aus:
    
    ```powershell
    .\sysprep.exe
    ```

4. Das Fenster des Systemvorbereitungstools wird geöffnet. Aktivieren Sie das Kontrollkästchen **Verallgemeinern**, wechseln Sie zu **Optionen für Herunterfahren**, und wählen Sie im Dropdownmenü die Option **Herunterfahren** aus.

>[!NOTE]
>Es dauert einige Minuten, bis der Sysprep-Prozess abgeschlossen ist. Beim Herunterfahren der VM wird die Remotesitzung getrennt.

### <a name="resolve-sysprep-errors"></a>Beheben von Sysprep-Fehlern

Wenn während des Sysprep-Prozesses ein Fehler auftritt, gehen Sie wie folgt vor:

1. Öffnen Sie **Laufwerk C**, und wechseln Sie zu **Windows** > **System32 Sysprep** > **Panther**. Öffnen Sie anschließend die Datei **setuperr**.

   In der Fehlerdatei werden Sie angewiesen, ein bestimmtes Language Pack zu deinstallieren, wie in der folgenden Abbildung veranschaulicht. Kopieren Sie den Namen des Language Packs für den nächsten Schritt.

   ![Screenshot der Datei „setuperr“. Der Text mit dem Namen des Language Packs ist dunkelblau hervorgehoben.](media/setuperr-package-name.png)

2. Öffnen Sie ein neues PowerShell-Fenster, und führen Sie das folgende Cmdlet mit dem in Schritt 2 kopierten Language Pack-Namen aus, um das Language Pack zu entfernen:

   ```powershell
   Remove-AppxPackage <package name>
   ```

3. Vergewissern Sie sich, dass Sie das Language Pack entfernt haben, indem Sie das Cmdlet `Remove-AppxPackage` noch einmal ausführen. Wenn das Paket erfolgreich entfernt wurde, sollte eine Meldung angezeigt werden, die besagt, dass das zu entfernende Paket nicht vorhanden ist.

4. Führen Sie das Cmdlet `sysprep.exe` noch einmal aus.

## <a name="capture-the-image"></a>Erfassen des Images

Nachdem das System nun vorbereitet ist, können Sie ein Image erfassen, damit andere Benutzer auf Ihrem System basierende VMs verwenden können, ohne dass sie Konfigurationsprozess wiederholen müssen.

So nehmen Sie ein Image auf:

1. Wechseln Sie zum Azure-Portal, und wählen Sie den Namen des Computers aus, den Sie in [Installieren eines Language Packs](#install-a-language-pack) und [Sysprep](#sysprep) konfiguriert haben.

2. Wählen Sie die Option **Erfassen** aus.

3. Geben Sie im Feld **Name** einen Namen für das Image ein, und weisen Sie es mithilfe des Dropdownmenüs **Ressourcengruppe** der Ressourcengruppe zu, wie in der folgenden Abbildung dargestellt.

   ![Screenshot des Fensters „Image erstellen“. Der Benutzer hat diesem Testimage den Namen „vmwvd-image-fr“ zugewiesen, und es wurde der Ressourcengruppe „testwvdimagerg“ zugewiesen.](media/create-image.png)

4. Klicken Sie auf **Erstellen**.

5. Warten Sie einige Minuten, bis die Erfassung abgeschlossen ist. Wenn das Image erstellt wurde, wird im Benachrichtigungscenter eine Meldung angezeigt, in der Sie informiert werden, dass das Image erfasst wurde.

Sie können jetzt einen virtuellen Computer mit Ihrem neuen Image bereitstellen. Beim Bereitstellen der VM müssen Sie die Anweisungen unter [Erstellen eines virtuellen Windows-Computers in einer Verfügbarkeitszone mit dem Azure-Portal](../virtual-machines/windows/create-portal-availability-zone.md) befolgen.

### <a name="how-to-change-display-language-for-standard-users"></a>Ändern der Anzeigesprache für Standardbenutzer

Standardbenutzer können die Anzeigesprache auf Ihren VMs ändern.

So ändern Sie die Anzeigesprache:

1. Wechseln Sie zu **Spracheinstellungen**. Wenn Sie nicht wissen, wo sich diese Einstellungen befinden, können Sie im Startmenü im Suchfeld den Begriff **Sprache** eingeben.

2. Wählen Sie im Dropdownmenü „Windows-Anzeigesprache“ die Sprache aus, die Sie als Anzeigesprache festlegen möchten.

3. Melden Sie sich von Ihrer Sitzung ab, und melden Sie sich anschließend wieder an. Die Anzeigesprache sollte nun der Sprache entsprechen, die Sie in Schritt 2 ausgewählt haben.
