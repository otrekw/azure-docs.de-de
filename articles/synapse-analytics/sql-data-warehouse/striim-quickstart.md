---
title: Striim-Schnellstart
description: Schneller Einstieg in Striim und Azure Synapse Analytics.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 10/12/2018
ms.author: martinle
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: d6b5fa1a42201479c5d426a5bbb9188d0cb8e03d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "89485973"
---
# <a name="striim-azure-synapse-analytics-marketplace-offering-install-guide"></a>Installationsanleitung zum Striim-Marketplace-Angebot für Azure Synapse Analytics

Für diesen Schnellstart wird vorausgesetzt, dass Sie bereits über eine vorhandene Instanz von Azure Synapse Analytics verfügen.

Suchen Sie im Azure Marketplace nach Striim, und wählen Sie die Option „Striim for Data Integration to Azure Synapse Analytics (Staged)“ aus. 

![Installieren von Striim][install]

Konfigurieren Sie den virtuellen Striim-Computer mit den angegebenen Eigenschaften, und notieren Sie sich dabei den Striim-Clusternamen, das Kennwort und das Administratorkennwort.

![Konfigurieren von Striim][configure]

Klicken Sie nach der Bereitstellung auf „\<VM Name>-masternode“ im Azure-Portal, klicken Sie auf „Verbinden“, und kopieren Sie die Anmeldung mit einem lokalen VM-Konto. 

![Verbinden von Striim mit Azure Synapse Analytics][connect]

Laden Sie die Datei „sqljdbc42.jar“ von <https://www.microsoft.com/en-us/download/details.aspx?id=54671> auf Ihren lokalen Computer herunter. 

Öffnen Sie ein Befehlszeilenfenster, und wechseln Sie zu dem Verzeichnis, in das Sie die JDBC-JAR-Datei heruntergeladen haben. Laden Sie die JAR-Datei mit SCP auf Ihren virtuellen Striim-Computer, wobei Sie die Adresse und das Kennwort vom Azure-Portal abrufen.

![Kopieren der JAR-Datei auf den virtuellen Computer][copy-jar]

Öffnen Sie ein weiteres Befehlszeilenfenster, oder verwenden Sie ein SSH-Hilfsprogramm, um eine SSH-Verbindung mit dem Striim-Cluster herzustellen.

![Herstellen einer SSH-Verbindung mit dem Cluster][ssh]

Führen Sie die folgenden Befehle aus, um die JDBC-JAR-Datei in das Verzeichnis „lib“ von Striim zu verschieben und den Server zu starten und zu stoppen.

   1. sudo su
   2. cd /tmp
   3. mv sqljdbc42.jar /opt/striim/lib
   4. systemctl stop striim-node
   5. systemctl stop striim-dbms
   6. systemctl start striim-dbms
   7. systemctl start striim-node

![Starten des Striim-Clusters][start-striim]

Öffnen Sie nun Ihren bevorzugten Browser, und navigieren Sie zu „\<DNS Name>:9080“.

![Navigieren zum Anmeldebildschirm][navigate]

Melden Sie sich mit dem Benutzernamen und Kennwort an, die Sie im Azure-Portal eingerichtet haben, und wählen Sie den bevorzugte Assistenten für die ersten Schritte aus, oder wechseln Sie zur Seite „Apps“, um mit der Verwendung der Drag & Drop-Benutzeroberfläche zu beginnen.

![Anmelden mit Serveranmeldeinformationen][login]



[install]: ./media/striim-quickstart/install.png
[configure]: ./media/striim-quickstart/configure.png
[connect]:./media/striim-quickstart/connect.png
[copy-jar]:./media/striim-quickstart/copy-jar.png
[ssh]:./media/striim-quickstart/ssh.png
[start-striim]:./media/striim-quickstart/start-striim.png
[navigate]:./media/striim-quickstart/navigate.png
[login]:./media/striim-quickstart/login.png
