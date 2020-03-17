---
title: Aktivieren von privatem Websitezugriff für Azure Functions
description: Hier erfahren Sie, wie Sie den privaten Websitezugriff von virtuellen Azure-Netzwerken für Azure Functions einrichten.
author: mcollier
ms.author: mcollier
ms.service: azure-functions
ms.topic: tutorial
ms.date: 02/15/2020
ms.openlocfilehash: ada08de182791c6ecb2b83ef3b924bf40975e1ee
ms.sourcegitcommit: f5e4d0466b417fa511b942fd3bd206aeae0055bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78852015"
---
# <a name="tutorial-establish-azure-functions-private-site-access"></a>Tutorial: Einrichten von privatem Websitezugriff für Azure Functions

In diesem Tutorial erfahren Sie, wie Sie den [privaten Websitezugriff](./functions-networking-options.md#private-site-access) mit Azure Functions aktivieren. Mit dem privaten Websitezugriff können Sie sicherstellen, dass Ihr Funktionscode nur von einem bestimmten virtuellen Netzwerk aus ausgelöst wird.

Der private Websitezugriff ist hilfreich in Szenarien, in denen der Zugriff auf die Funktions-App auf ein bestimmtes virtuelles Netzwerk beschränkt werden muss. Dies kann beispielsweise der Fall sein, wenn die Funktions-App nur für Mitarbeiter einer bestimmten Organisation oder nur für Dienste innerhalb des angegebenen virtuellen Netzwerks vorgesehen ist (etwa für eine andere Azure-Funktion, einen virtuellen Azure-Computer oder einen AKS-Cluster).

Wenn eine Funktions-App auf Azure-Ressourcen innerhalb des virtuellen Netzwerks zugreifen oder über [Dienstendpunkte](../virtual-network/virtual-network-service-endpoints-overview.md) verbunden werden muss, ist eine [VNET-Integration](./functions-create-vnet.md) erforderlich.

In diesem Tutorial erfahren Sie, wie Sie den privaten Websitezugriff für Ihre Funktions-App konfigurieren:

> [!div class="checklist"]
> * Erstellen eines virtuellen Computers
> * Erstellen eines Azure Bastion-Diensts
> * Erstellen einer Azure Functions-App
> * Konfigurieren eines VNET-Dienstendpunkts
> * Erstellen und Bereitstellen einer Azure-Funktion
> * Aufrufen der Funktion außerhalb und innerhalb des virtuellen Netzwerks

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="topology"></a>Topologie

Das folgende Diagramm zeigt die Architektur der zu erstellenden Lösung:

![Allgemeines Architekturdiagramm der Lösung für privaten Websitezugriff](./media/functions-create-private-site-access/topology.png)

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial ist es wichtig, dass Sie die Funktionsweise und Verwendung von IP-Adressen und Subnetzen verstehen. Beginnen Sie mit [diesem Artikel zu den Grundlagen von Adressen und Subnetzen](https://support.microsoft.com/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics). Viele weitere Artikel und Videos finden Sie online.

## <a name="sign-in-to-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers

Im ersten Schritt dieses Tutorials wird ein neuer virtueller Computer in einem virtuellen Netzwerk erstellt.  Der virtuelle Computer wird für den Zugriff auf Ihre Funktion verwendet, nachdem Sie den Zugriff so eingeschränkt haben, dass nur innerhalb des virtuellen Netzwerks darauf zugegriffen werden kann.

1. Wählen Sie die Schaltfläche **Ressource erstellen** aus.

2. Geben Sie `Windows Server` in das Suchfeld ein, und wählen Sie in den Suchergebnissen die Option **Windows Server** aus.

3. Wählen Sie in der Liste mit den Windows Server-Optionen **Windows Server 2019 Datacenter** und anschließend die Schaltfläche **Erstellen** aus.

4. Verwenden Sie auf der Registerkarte **Grundlagen** die VM-Einstellungen, die Sie in der Tabelle unterhalb der Abbildung finden:

    >[!div class="mx-imgBorder"]
    >![Registerkarte „Grundeinstellungen“ für einen neuen virtuellen Windows-Computer](./media/functions-create-private-site-access/create-vm-3.png)

    | Einstellung      | Vorgeschlagener Wert  | Beschreibung      |
    | ------------ | ---------------- | ---------------- |
    | **Abonnement** | Ihr Abonnement | Das Abonnement, in dem Ihre Ressourcen erstellt werden. |
    | [**Ressourcengruppe**](../azure-resource-manager/management/overview.md) | myResourceGroup | Wählen Sie die Ressourcengruppe aus, die die Ressourcen für dieses Tutorial enthalten soll.  Die Verwendung der gleichen Ressourcengruppe vereinfacht die Bereinigung der Ressourcen am Ende des Tutorials. |
    | **Name des virtuellen Computers** | myVM | Der VM-Name muss in der Ressourcengruppe eindeutig sein. |
    | [**Region**](https://azure.microsoft.com/regions/) | (US) „USA, Norden-Mitte“ | Wählen Sie eine Region in Ihrer Nähe oder in der Nähe der Funktionen aus, auf die zugegriffen werden soll. |
    | **Öffentliche Eingangsports** | Keine | Wählen Sie **Keine** aus, um sicherzustellen, dass keine eingehende Internetkonnektivität für den virtuellen Computer vorhanden ist. Der Remotezugriff auf den virtuellen Computer wird über den Azure Bastion-Dienst konfiguriert. |

5. Wählen Sie die Registerkarte **Netzwerk** und anschließend **Neu erstellen** aus, um ein neues virtuelles Netzwerk zu konfigurieren.

    >[!div class="mx-imgBorder"]
    >![Erstellen eines neuen virtuellen Netzwerks für den neuen virtuellen Computer](./media/functions-create-private-site-access/create-vm-networking.png)

6. Verwenden Sie unter **Virtuelles Netzwerk erstellen** die Einstellungen in der Tabelle unterhalb der Abbildung:

    >[!div class="mx-imgBorder"]
    >![Erstellen eines neuen virtuellen Netzwerks für den neuen virtuellen Computer](./media/functions-create-private-site-access/create-vm-vnet-1.png)

    | Einstellung      | Vorgeschlagener Wert  | Beschreibung      |
    | ------------ | ---------------- | ---------------- |
    | **Name** | myResourceGroup-vnet | Sie können den für Ihr virtuelles Netzwerk generierten Standardnamen verwenden. |
    | **Adressbereich** | 10.10.0.0/16 | Verwenden Sie einen einzelnen Adressbereich für das virtuelle Netzwerk. |
    | **Subnetzname** | Lernprogramm | Der Name des Subnetzes. |
    | **Adressbereich** (Subnetz) | 10.10.1.0/24 | Die Subnetzgröße definiert, wie viele Schnittstellen zum Subnetz hinzugefügt werden können. Dieses Subnetz wird von dem virtuellen Computer verwendet. Ein `/24`-Subnetz bietet 254 Hostadressen. |

7. Klicken Sie auf **OK**, um das virtuelle Netzwerk zu erstellen.
8. Vergewissern Sie sich auf der Registerkarte **Netzwerk**, dass für **Öffentliche IP** die Option **Keine** ausgewählt ist.
9. Wählen Sie die Registerkarte **Verwaltung** und anschließend unter **Diagnosespeicherkonto** die Option **Neu erstellen** aus, um ein neues Speicherkonto zu erstellen.
10. Behalten Sie für **Identität**, **Automatisch herunterfahren** und **Sicherung** die Standardwerte bei.
11. Klicken Sie auf **Überprüfen + erstellen**. Wenn die Überprüfung abgeschlossen ist, wählen Sie **Erstellen** aus. Der Vorgang der VM-Erstellung dauert einige Minuten.

## <a name="configure-azure-bastion"></a>Konfigurieren von Azure Bastion

[Azure Bastion](https://azure.microsoft.com/services/azure-bastion/) ist ein vollständig verwalteter Azure-Dienst für sicheren RDP- und SSH-Zugriff auf virtuelle Computer direkt über das Azure-Portal. Bei Verwendung des Azure Bastion-Diensts müssen keine Netzwerkeinstellungen für den RDP-Zugriff konfiguriert werden.

1. Wählen Sie im Portal im oberen Bereich der Ressourcengruppenansicht die Option **Hinzufügen** aus.
2. Geben Sie „Bastion“ in das Suchfeld ein.  Wählen Sie „Bastion“ aus.
3. Wählen Sie **Erstellen** aus, um mit der Erstellung einer neuen Azure Bastion-Ressource zu beginnen. Im Abschnitt **Virtuelles Netzwerk** wird eine Fehlermeldung angezeigt, da noch kein Subnetz vom Typ `AzureBastionSubnet` vorhanden ist. Das Subnetz wird in den folgenden Schritten erstellt. Verwenden Sie die Einstellungen aus der Tabelle unterhalb der Abbildung:

    >[!div class="mx-imgBorder"]
    >![Beginn der Azure Bastion-Erstellung](./media/functions-create-private-site-access/create-bastion-basics-1.png)

    | Einstellung      | Vorgeschlagener Wert  | Beschreibung      |
    | ------------ | ---------------- | ---------------- |
    | **Name** | myBastion | Der Name der neuen Bastion-Ressource. |
    | **Region** | USA Nord Mitte | Wählen Sie eine [Region](https://azure.microsoft.com/regions/) in Ihrer Nähe oder in der Nähe von anderen Diensten aus, auf die Ihre Funktionen zugreifen. |
    | **Virtuelles Netzwerk** | myResourceGroup-vnet | Das virtuelle Netzwerk, in dem die Bastion-Ressource erstellt wird. |
    | **Subnetz** | AzureBastionSubnet | Das Subnetz in Ihrem virtuellen Netzwerk, in dem die neue Bastion-Hostressource bereitgestellt wird. Erstellen Sie ein Subnetz mit dem Namen `AzureBastionSubnet`. Dadurch weiß Azure, in welchem Subnetz die Bastion-Ressourcen bereitgestellt werden sollen. Verwenden Sie ein Subnetz mit `/27` oder mehr (`/27`, `/26` usw.). |

    > [!NOTE]
    > Eine ausführliche Schritt-für-Schritt-Anleitung zum Erstellen einer Azure Bastion-Ressource finden Sie im Tutorial [Erstellen eines Azure Bastion-Hosts](../bastion/bastion-create-host-portal.md).

4. Erstellen Sie ein Subnetz, in dem Azure den Azure Bastion-Host bereitstellen kann. Wenn Sie **Subnetzkonfiguration verwalten** auswählen, wird ein neuer Bereich geöffnet, in dem Sie ein neues Subnetz definieren können.  Wählen Sie **+ Subnetz** aus, um ein neues Subnetz zu erstellen.
5. Das Subnetz muss den Namen `AzureBastionSubnet` haben, und das Subnetzpräfix muss mindestens `/27` sein.  Klicken Sie auf **OK**, um das Subnetz zu erstellen.

    >[!div class="mx-imgBorder"]
    >![Erstellen eines Subnetzes für einen Azure Bastion-Host](./media/functions-create-private-site-access/create-bastion-subnet-2.png)

6. Wählen Sie auf der Seite **Bastion-Instanz erstellen** in der Liste mit den verfügbaren Subnetzen das neu erstellte Subnetz `AzureBastionSubnet` aus.

    >[!div class="mx-imgBorder"]
    >![Erstellen eines Azure Bastion-Hosts mit einem bestimmten Subnetz](./media/functions-create-private-site-access/create-bastion-basics-2.png)

7. Wählen Sie **Überprüfen + erstellen** aus. Wählen Sie nach Abschluss der Überprüfung **Erstellen** aus. Die Erstellung der Azure Bastion-Ressource dauert einige Minuten.

## <a name="create-an-azure-functions-app"></a>Erstellen einer Azure Functions-App

Im nächsten Schritt wird in Azure eine Funktions-App mit [Verbrauchsplan](functions-scale.md#consumption-plan) erstellt. Später in diesem Tutorial wird Ihr Funktionscode für diese Ressource bereitgestellt.

1. Wählen Sie im Portal im oberen Bereich der Ressourcengruppenansicht die Option **Hinzufügen** aus.
2. Wählen Sie **Compute > Funktions-App** aus.
3. Verwenden Sie im Abschnitt **Grundeinstellungen** die Funktions-App-Einstellungen aus der folgenden Tabelle:

    | Einstellung      | Vorgeschlagener Wert  | Beschreibung      |
    | ------------ | ---------------- | ---------------- |
    | **Ressourcengruppe** | myResourceGroup | Wählen Sie die Ressourcengruppe aus, die die Ressourcen für dieses Tutorial enthalten soll.  Die Verwendung der gleichen Ressourcengruppe für die Funktions-App und den virtuellen Computer vereinfacht die Bereinigung der Ressourcen am Ende des Tutorials. |
    | **Name der Funktions-App** | Global eindeutiger Name | Der Name, der Ihre neue Funktionen-App bezeichnet Gültige Zeichen sind a–z (unabhängig von der Groß-/Kleinschreibung), 0–9 und -. |
    | **Veröffentlichen** | Code | Option zum Veröffentlichen von Codedateien oder eines Docker-Containers. |
    | **Laufzeitstapel** | Bevorzugte Sprache | Wählen Sie eine Runtime aus, die Ihre bevorzugte Programmiersprache für Funktionen unterstützt. |
    | **Region** | USA Nord Mitte | Wählen Sie eine [Region](https://azure.microsoft.com/regions/) in Ihrer Nähe oder in der Nähe von anderen Diensten aus, auf die Ihre Funktionen zugreifen. |

    Wählen Sie unten auf der Seite die Schaltfläche **Next: Hosting >** aus.
4. Wählen Sie im Abschnitt **Hosting** die passenden Werte für **Speicherkonto**, **Betriebssystem** und **Tarif** aus. Orientieren Sie sich dabei an den Angaben in der folgenden Tabelle:

    | Einstellung      | Vorgeschlagener Wert  | Beschreibung      |
    | ------------ | ---------------- | ---------------- |
    | **Speicherkonto** | Global eindeutiger Name | Erstellen Sie ein Speicherkonto, das von Ihrer Funktions-App verwendet wird. Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten. Sie können auch ein vorhandenes Konto verwenden, das die [Anforderungen an das Speicherkonto](./functions-scale.md#storage-account-requirements) erfüllen muss. |
    | **Betriebssystem** | Bevorzugtes Betriebssystem | Ein Betriebssystem ist für Sie basierend auf Ihrer Runtimestapelauswahl vorab ausgewählt, aber Sie können die Einstellung ggf. ändern. |
    | **Planen** | Nutzung | Der [Hostingplan](./functions-scale.md) bestimmt die Skalierung der Funktions-App und die Ressourcenverfügbarkeit für die jeweilige Instanz. |
5. Wählen Sie **Überprüfen + erstellen** aus, um die App-Konfigurationsauswahl zu überprüfen.
6. Klicken Sie auf **Erstellen**, um die Funktionen-App bereitzustellen.

## <a name="configure-access-restrictions"></a>Konfigurieren der Zugriffseinschränkungen

Im nächsten Schritt werden [Zugriffseinschränkungen](../app-service/app-service-ip-restrictions.md) konfiguriert, um sicherzustellen, dass die Funktion nur von Ressourcen im virtuellen Netzwerk aufgerufen werden kann.

Zur Ermöglichung von [privatem Websitezugriff](functions-networking-options.md#private-site-access) wird ein Azure Virtual Network-[Dienstendpunkt](../virtual-network/virtual-network-service-endpoints-overview.md) zwischen der Funktions-App und dem angegebenen virtuellen Netzwerk erstellt. Zugriffseinschränkungen werden mithilfe von Dienstendpunkten implementiert. Dienstendpunkte sorgen dafür, dass nur von Datenverkehr aus dem angegebenen virtuellen Netzwerk auf die angegebene Ressource zugegriffen werden kann. In diesem Fall ist die Azure-Funktion die angegebene Ressource.

1. Navigieren Sie in der Funktions-App zur Registerkarte **Plattformfeatures**. Wählen Sie unter der Überschrift des Abschnitts *Netzwerk* den Link **Netzwerk** aus, um den Abschnitt „Netzwerkfeaturestatus“ zu öffnen.
2. Die Seite **Netzwerkfeaturestatus** ist der Ausgangspunkt für die Konfiguration von Azure Front Door, Azure CDN und Zugriffseinschränkungen. Wählen Sie **Zugriffseinschränkungen konfigurieren** aus, um den privaten Websitezugriff zu konfigurieren.
3. Auf der Seite **Zugriffseinschränkungen** ist zunächst nur die Standardeinschränkung vorhanden. Die Standardeinschränkung bewirkt keinerlei Zugriffseinschränkungen für die Funktions-App.  Wählen Sie **Regel hinzufügen** aus, um eine Einschränkungskonfiguration für den privaten Websitezugriff zu erstellen.
4. Wählen Sie im Bereich **Zugriffseinschränkung hinzufügen** im Dropdownfeld **Typ** die Option **Virtuelles Netzwerk** und anschließend das zuvor erstellte virtuelle Netzwerk und das Subnetz aus.
5. Auf der Seite **Zugriffseinschränkungen** wird nun angezeigt, dass eine neue Einschränkung vorhanden ist. Es kann einige Sekunden dauern, bis der Wert von **Endpunktstatus** von `Disabled` über `Provisioning` zu `Enabled` wechselt.

    >[!IMPORTANT]
    > Jede Funktions-App verfügt über eine [Kudu-Website](../app-service/app-service-ip-restrictions.md#scm-site) mit erweiterten Tools zur Verwaltung von Funktions-App-Bereitstellungen. Auf diese Website wird über eine URL wie die folgende zugegriffen: `<FUNCTION_APP_NAME>.scm.azurewebsites.net`. Da für diese Bereitstellungswebsite keine Zugriffseinschränkungen aktiviert wurden, können Sie Ihren Projektcode weiterhin über eine lokale Entwicklerarbeitsstation oder über einen lokalen Builddienst bereitstellen, ohne einen Agent im virtuellen Netzwerk bereitstellen zu müssen.

## <a name="access-the-functions-app"></a>Zugreifen auf die Funktions-App

1. Kehren Sie zu der zuvor erstellten Funktions-App zurück.  Kopieren Sie im Abschnitt **Übersicht** die URL.

    >[!div class="mx-imgBorder"]
    >![Abrufen der URL der Funktions-App](./media/functions-create-private-site-access/access-function-overview.png)

2. Wenn Sie nun auf Ihrem Computer außerhalb des virtuellen Netzwerks versuchen, auf die Funktions-App zuzugreifen, wird eine HTTP 403-Seite mit dem Hinweis angezeigt, dass die App beendet wurde.  Die App wurde jedoch nicht beendet. Bei der Antwort handelt es sich vielmehr um den HTTP-Status 403 (unzulässige IP-Adresse).
3. Als Nächstes greifen Sie auf Ihre Funktion über den zuvor erstellten virtuellen Computer zu, der mit Ihrem virtuellen Netzwerk verbunden ist. Um von dem virtuellen Computer aus auf die Website zugreifen zu können, müssen Sie über den Azure Bastion-Dienst eine Verbindung mit dem virtuellen Computer herstellen.  Wählen Sie zuerst **Verbinden** und dann **Bastion** aus.
4. Geben Sie den erforderlichen Benutzernamen und das entsprechende Kennwort an, um sich bei dem virtuellen Computer anzumelden.  Wählen Sie **Verbinden**. Daraufhin wird ein neues Browserfenster angezeigt, in dem Sie mit dem virtuellen Computer interagieren können.
5. Da dieser virtuelle Computer über das virtuelle Netzwerk auf die Funktion zugreift, kann über den Webbrowser des virtuellen Computers auf die Website zugegriffen werden.  Wichtig: Auf die Funktions-App kann zwar nur innerhalb des angegebenen virtuellen Netzwerks zugegriffen werden, es ist jedoch weiterhin ein öffentlicher DNS-Eintrag vorhanden. Wie oben gezeigt, führt der Versuch, auf die Website zuzugreifen, zu einer HTTP 403-Antwort.

## <a name="create-a-function"></a>Erstellen einer Funktion

Im nächsten Schritt dieses Tutorials wird eine Azure-Funktion mit HTTP-Trigger erstellt. Wenn diese Funktion über eine HTTP GET- oder POST-Anforderung aufgerufen wird, sollte „Hello {name}“ zurückgegeben werden.  

1. Verwenden Sie eine der folgenden Schnellstartanleitungen, um Ihre Azure Functions-App zu erstellen und bereitzustellen:

    * [Visual Studio Code](./functions-create-first-function-vs-code.md)
    * [Visual Studio](./functions-create-your-first-function-visual-studio.md)
    * [Befehlszeile](./functions-create-first-azure-function-azure-cli.md)
    * [Maven (Java)](./functions-create-first-java-maven.md)

2. Wählen Sie beim Veröffentlichen Ihres Azure Functions-Projekts die Funktions-App-Ressource aus, die Sie weiter oben in diesem Tutorial erstellt haben.
3. Vergewissern Sie sich, dass die Funktion bereitgestellt wurde.

    >[!div class="mx-imgBorder"]
    >![Bereitgestellte Funktion in der Liste der Funktionen](./media/functions-create-private-site-access/verify-deployed-function.png)

## <a name="invoke-the-function-directly"></a>Direktes Aufrufen der Funktion

1. Um den Zugriff auf die Funktion zu testen, müssen Sie die URL der Funktion kopieren. Wählen Sie die bereitgestellte Funktion und anschließend **</> Funktions-URL abrufen** aus. Klicken Sie auf die Schaltfläche **Kopieren**, um die URL in die Zwischenablage zu kopieren.

    >[!div class="mx-imgBorder"]
    >![Kopieren Sie die Funktions-URL](./media/functions-create-private-site-access/get-function-url.png)

    > [!NOTE]
    > Wenn die Funktion ausgeführt wird, wird im Portal ein Laufzeitfehler mit dem Hinweis angezeigt, dass die Funktionslaufzeit nicht gestartet werden kann. Die Funktions-App wird jedoch trotzdem ausgeführt. Der Fehler ist auf die neuen Zugriffseinschränkungen zurückzuführen, die dafür sorgen, dass das Portal keine Abfragen zur Überprüfung der Laufzeit ausführen kann.

2. Fügen Sie die URL in den Webbrowser ein. Wenn Sie nun auf einem Computer außerhalb Ihres virtuellen Netzwerks versuchen, auf die Funktions-App zuzugreifen, wird eine HTTP 403-Antwort mit dem Hinweis angezeigt, dass die App beendet wurde.

## <a name="invoke-the-function-from-the-virtual-network"></a>Aufrufen der Funktion innerhalb des virtuellen Netzwerks

Wenn Sie auf dem konfigurierten virtuellen Computer innerhalb des virtuellen Netzwerks über einen Webbrowser (unter Verwendung des Azure Bastion-Diensts) auf die Funktion zugreifen, ist der Zugriff erfolgreich.

>[!div class="mx-imgBorder"]
>![Zugreifen auf die Azure-Funktion über Azure Bastion](./media/functions-create-private-site-access/access-function-via-bastion-final.png)

[!INCLUDE [clean-up-section-portal](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Nächste Schritte


> [!div class="nextstepaction"]
> [Weitere Informationen zu den Netzwerkoptionen in Functions](./functions-networking-options.md)
