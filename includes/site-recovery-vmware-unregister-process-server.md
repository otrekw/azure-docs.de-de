---
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: include
ms.date: 04/28/2019
ms.author: ramamill
ms.openlocfilehash: bff9457220efbb6a6b318550a5b2dd9af4ae230f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91361387"
---
Führen Sie die Schritte für Ihre spezifische Situation aus.

### <a name="unregister-a-connected-process-server"></a>Aufheben der Registrierung eines verbundenen Prozessservers

1. Richten Sie eine Remoteverbindung mit dem Prozessserver als Administrator ein.
2. Öffnen Sie die **Systemsteuerung**, und wählen Sie **Programme > Programm deinstallieren** aus.
3. Deinstallieren Sie das Programm **Microsoft Azure Site Recovery Mobility Service/Masterzielserver**.
4. Deinstallieren Sie das Programm **Microsoft Azure Site Recovery-Konfiguration/Prozessserver**.
5. Nachdem Sie die Programme in den Schritten 3 und 4 deinstalliert haben, deinstallieren Sie **Microsoft Azure Site Recovery-Konfiguration/Prozessserverabhängigkeiten**.

### <a name="unregister-a-disconnected-process-server"></a>Aufheben der Registrierung eines getrennten Prozessservers

Führen Sie diese Schritte nur aus, wenn es keine Möglichkeit gibt, den Computer, auf dem der Prozessserver installiert ist, erneut zu aktivieren.

1. Melden Sie sich beim Konfigurationsserver als Administrator an.
2. Öffnen Sie eine Administratoreingabeaufforderung, und navigieren Sie zu `%ProgramData%\ASR\home\svsystems\bin`.
3. Führen Sie den folgenden Befehl aus, um eine Liste von einem oder mehreren Prozessservern abzurufen.

    ```
    perl Unregister-ASRComponent.pl -IPAddress <IP_of_Process_Server> -Component PS
    ```
    - S. No: die Seriennummer des Prozessservers.
    - IP/Name: Die IP-Adresse und der Name des Computers, auf dem der Prozessserver ausgeführt wird.
    - Heartbeat: Der letzte Heartbeat vom Computer des Prozessservers.
    ![Screenshot: Informationen im Klartext zu Ihren Prozessservern sowie der Aufforderung, einen der oben genannten Server auszuwählen, um seine Registrierung aufzuheben(media/site-recovery-vmware-unregister-process-server/Unregister-cmd.PNG)

4. Geben Sie die Seriennummer des Prozessservers an, dessen Registrierung Sie aufheben möchten.
5. Durch Aufheben der Registrierung eines Prozessservers werden alle zugehörigen Details aus dem System entfernt, und es wird folgende Meldung angezeigt: **Servername> (Server-IP-Adresse) wurde erfolgreich deinstalliert**

