---
title: Bereitstellen der Azure Sentinel-Lösung für SAP-Umgebungen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Azure Sentinel-Lösung für SAP-Umgebungen bereitstellen.
author: batamig
ms.author: bagold
ms.service: azure-sentinel
ms.topic: tutorial
ms.custom: mvc
ms.date: 05/13/2021
ms.subservice: azure-sentinel
ms.openlocfilehash: cf7a9fb700bba135663e0684d8ba25c7ebcf92f0
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110466528"
---
# <a name="tutorial-deploy-the-azure-sentinel-solution-for-sap-public-preview"></a>Tutorial: Bereitstellen der Azure Sentinel-Lösung für SAP (öffentliche Vorschau)

In diesem Tutorial werden Sie Schritt für Schritt durch den Prozess zur Bereitstellung der Azure Sentinel-Lösung für SAP geführt.

> [!IMPORTANT]
> Die SAP-Lösung von Azure Sentinel befindet sich derzeit in der Vorschauphase. In den [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) finden Sie weitere rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden oder anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.
>

## <a name="overview"></a>Überblick

[Azure Sentinel-Lösungen](sentinel-solutions.md) umfassen gebündelte Sicherheitsinhalte wie Bedrohungserkennungen, Arbeitsmappen und Watchlists. Durch Lösungen können Sie Azure Sentinel-Sicherheitsinhalte für einen bestimmten Datenconnector mithilfe eines einzelnen Prozesses integrieren.

Der SAP-Datenconnector von Azure Sentinel ermöglicht es Ihnen, SAP-Systeme auf komplexe Bedrohungen innerhalb der Geschäfts- und Anwendungsebene zu überwachen.

Der SAP-Datenconnector streamt eine Vielzahl von 14 Anwendungsprotokollen aus der gesamten SAP-Systemlandschaft und sammelt Protokolle sowohl von Advanced Business Application Programming (ABAP) über NetWeaver RFC-Aufrufe als auch Dateispeicherdaten über die OSSAP-Kontrollschnittstelle. Der SAP-Datenconnector bietet Azure Sentinel die Möglichkeit, die zugrunde liegende SAP-Infrastruktur zu überwachen.

Um SAP-Protokolle in Azure Sentinel zu erfassen, muss der SAP-Datenconnector von Azure Sentinel in Ihrer SAP-Umgebung installiert sein.
Es wird empfohlen, einen Docker-Container auf einer Azure-VM für die Bereitstellung zu verwenden, wie es in diesem Tutorial beschrieben ist.

Nach der Bereitstellung des SAP-Datenconnectors stellen Sie die Sicherheitsinhalte der SAP-Lösung bereit, um reibungslos Einblicke in die SAP-Umgebung Ihrer Organisation zu erhalten und alle zugehörigen Funktionen für Sicherheitsvorgänge zu verbessern.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Vorbereiten Ihres SAP-Systems für die Bereitstellung des SAP-Datenconnectors
> * Verwenden eines Docker-Containers und einer Azure-VM zum Bereitstellen des SAP-Datenconnectors
> * Bereitstellen der Sicherheitsinhalte der SAP-Lösung in Azure Sentinel

## <a name="prerequisites"></a>Voraussetzungen

Um den SAP-Datenconnector von Azure Sentinel und die Sicherheitsinhalte wie in diesem Tutorial beschrieben bereitzustellen, müssen die folgenden Voraussetzungen erfüllt sein:

|Bereich  |Beschreibung  |
|---------|---------|
|**Voraussetzungen für Azure**     |  **Zugriff auf Azure Sentinel**. Notieren Sie sich ID und Schlüssel Ihres Azure Sentinel-Arbeitsbereichs, den Sie in diesem Tutorial beim [Bereitstellen Ihres SAP-Datenconnectors](#deploy-your-sap-data-connector) verwenden möchten. <br>Zum Anzeigen dieser Details in Azure Sentinel navigieren Sie zu **Einstellungen** > **Arbeitsbereichseinstellungen** > **Agent-Verwaltung**. <br><br>**Möglichkeit zum Erstellen von Azure-Ressourcen**. Weitere Informationen finden Sie in der [Dokumentation zu Azure Resource Manager](/azure/azure-resource-manager/management/manage-resources-portal). <br><br>**Zugriff auf Azure Key Vault**. In diesem Tutorial werden die empfohlenen Schritte für die Verwendung von Azure Key Vault zum Speichern Ihrer Anmeldeinformationen beschrieben. Weitere Informationen finden Sie in der [Dokumentation zu Azure Key Vault](/azure/key-vault/).       |
|**Systemanforderungen**     |   **Software**. Die Softwarevoraussetzungen werden mit dem Bereitstellungsskript für den SAP-Datenconnector automatisch installiert. Weitere Informationen finden Sie unter [Automatisch installierte Software](#automatically-installed-software). <br><br> **Systemkonnektivität**. Stellen Sie sicher, dass die VM, die als Host für den SAP-Datenconnector dient, über Zugriff auf Folgendes verfügt: <br>– Azure Sentinel <br>– Azure Key Vault <br>– Den Host der SAP-Umgebung über die folgenden TCP-Ports: *32xx*, *5xx13* und *33xx*, wobei *xx* für die SAP-Instanznummer steht <br><br>Stellen Sie sicher, dass Sie auch über ein SAP-Benutzerkonto verfügen, um auf die Downloadseite für SAP-Software zugreifen zu können.<br><br>**Systemarchitektur**. Die SAP-Lösung wird auf einer VM als Docker-Container bereitgestellt, und jeder SAP-Client benötigt eine eigene Containerinstanz. <br>Ihre VM und der Azure Sentinel-Arbeitsbereich können sich in unterschiedlichen Azure-Abonnements und sogar in unterschiedlichen Azure AD-Mandanten befinden.|
|**SAP-Voraussetzungen**     |   **Unterstützte SAP-Versionen**. Es wird empfohlen, die Version [SAP_BASIS 750 SP13](https://support.sap.com/en/my-support/software-downloads/support-package-stacks/product-versions.html#:~:text=SAP%20NetWeaver%20%20%20%20SAP%20Product%20Version,%20%20SAPKB710%3Cxx%3E%20%207%20more%20rows) oder höher zu verwenden. <br>Ausgewählte Schritte in diesem Tutorial bieten alternative Anweisungen, wenn Sie mit SAP-Version [SAP_BASIS 740](https://support.sap.com/en/my-support/software-downloads/support-package-stacks/product-versions.html#:~:text=SAP%20NetWeaver%20%20%20%20SAP%20Product%20Version,%20%20SAPKB710%3Cxx%3E%20%207%20more%20rows) arbeiten.<br><br> **SAP-Systemdetails**. Notieren Sie sich die folgenden SAP-Systemdetails, um sie in diesem Tutorial zu verwenden:<br>    – IP-Adresse des SAP-Systems<br>– SAP-Systemnummer, z. B. `00`<br>    – SAP-System-ID aus dem SAP NetWeaver-System, z. B. `NPL` <br>– SAP-Client-ID, z. B. `001`<br><br>**Zugriff auf SAP NetWeaver-Instanz**. Für den Zugriff auf Ihre SAP-Instanzen muss eine der folgenden Optionen verwendet werden: <br>- [SAP ABAP-Benutzer/-Kennwort](#configure-your-sap-system) <br>– Ein Benutzer mit einem X509-Zertifikat, der SAP CRYPTOLIB PSE verwendet. Für diese Option müssen möglicherweise manuelle Schritte von einem Experten ausgeführt werden.<br><br>**Unterstützung durch Ihr SAP-Team**.  Sie brauchen die Unterstützung Ihres SAP-Teams, um sicherzustellen, dass Ihr SAP-System für die Lösungsbereitstellung [richtig konfiguriert](#configure-your-sap-system) ist.   |
|     |         |


### <a name="automatically-installed-software"></a>Automatisch installierte Software

Mit dem [Bereitstellungsskript für den SAP-Datenconnector](#deploy-your-sap-data-connector) wird die folgende Software mit SUDO-Berechtigungen (Root) auf Ihrer VM installiert:

- [Unzip](https://www.microsoft.com/en-us/p/unzip/9mt44rnlpxxt?activetab=pivot:overviewtab)
- [NetCat](https://sectools.org/tool/netcat/)
- [Python 3.6 oder höher](https://www.python.org/downloads/)
- [Python3-pip](https://pypi.org/project/pip/)
- [Docker](https://www.docker.com/)

## <a name="configure-your-sap-system"></a>Konfigurieren Ihres SAP-Systems

In diesem Verfahren wird beschrieben, wie Sie sicherstellen, dass auf Ihrem SAP-System die richtige erforderliche Software installiert und für die Bereitstellung des SAP-Datenconnectors von Azure Sentinel konfiguriert ist.

> [!IMPORTANT]
> Führen Sie dieses Verfahren gemeinsam mit Ihrem SAP-Team aus, um richtige Konfigurationen zu gewährleisten.
>

**So konfigurieren Sie Ihr SAP-System für den SAP-Datenconnector**:

1. Wenn Sie eine frühere SAP-Version als 750 verwenden, stellen Sie sicher, dass die folgenden SAP-Hinweise in Ihrem System bereitgestellt sind:

    - **SPS12641084**. Für Systeme, auf denen SAP-Versionen vor SAP BASIS 750 SPS13 ausgeführt werden.
    - **2502336**. Für Systeme, auf denen SAP-Versionen vor SAP BASIS 750 SPS1 ausgeführt werden.
    - **2173545**. Für Systeme, auf denen SAP-Versionen vor SAP BASIS 750 ausgeführt werden.

    Greifen Sie auf diese SAP-Hinweise auf der [Launchpad-Website des SAP-Support](https://support.sap.com/en/index.html) mit einem SAP-Benutzerkonto zu.

1. Laden Sie eine der folgenden SAP-Änderungsanforderungen aus dem Azure Sentinel-GitHub-Repository unter https://github.com/Azure/Azure-Sentinel/tree/master/Solutions/SAP/CR: herunter, und installieren Sie sie:

    - **SAP-Versionen 750 oder höher**: Installieren Sie die SAP-Änderungsanforderung *131 (NPLK900131)* .
    - **SAP-Versionen 740**: Installieren Sie die SAP-Änderungsanforderung *132 (NPLK900132)* .

    Verwenden Sie beim Ausführen dieses Schritts den SAP-Transaktionscode **STMS_IMPORT**.

    > [!NOTE]
    > Im SAP-Bereich **Importoptionen** wird möglicherweise die Option **Ungültige Komponentenversion ignorieren** angezeigt. Wenn das der Fall ist, wählen Sie diese Option aus, bevor Sie fortfahren.
    >

1. Erstellen Sie eine neue SAP-Rolle mit dem Namen **/MSFTSEN/SENTINEL_CONNECTOR**, indem Sie die SAP-Änderungsanforderung *14 (NPLK900114)* importieren. Verwenden Sie den SAP-Transaktionscode **STMS_IMPORT**.

    Stellen Sie sicher, dass die Rolle mit den erforderlichen Berechtigungen erstellt wird. Hier ein Beispiel:

    :::image type="content" source="media/sap/required-sap-role-authorizations.png" alt-text="Erforderliche SAP-Rollenberechtigungen für den SAP-Datenconnector von Azure Sentinel":::

    Weitere Informationen finden Sie unter den [Autorisierungen für den ABAP-Benutzer](sap-solution-detailed-requirements.md#required-abap-authorizations).

1. Erstellen Sie einen RFC/NetWeaver-Benutzer ohne Dialog für den SAP-Datenconnector, und fügen Sie die neu erstellte Rolle **/MSFTSEN/SENTINEL_CONNECTOR** an.

    - Überprüfen Sie nach dem Anfügen der Rolle, ob die Rollenberechtigungen an den Benutzer verteilt sind.
    - Für diesen Vorgang müssen Sie einen Benutzernamen und ein Kennwort für den ABAP-Benutzer verwenden. Nachdem der neue Benutzer erstellt wurde und über die erforderlichen Berechtigungen verfügt, stellen Sie sicher, dass Sie das ABAP-Benutzerkennwort ändern.

1. Laden Sie die Version **SAP NetWeaver RFC SDK 7.50 for Linux on x86_64 64 BIT** herunter, und legen Sie sie auf Ihrer VM ab, da sie während des Installationsprozesses benötigt wird.

    Suchen Sie beispielsweise das SDK auf der [Downloadwebsite für SAP-Software](https://launchpad.support.sap.com/#/softwarecenter/template/products/_APP=00200682500000001943&_EVENT=DISPHIER&HEADER=Y&FUNCTIONBAR=N&EVENT=TREE&NE=NAVIGATE&ENR=01200314690100002214&V=MAINT) > **SAP NW RFC SDK** > **SAP NW RFC SDK 7.50** > **nwrfc750X_X-xxxxxxx.zip**. Vergewissern Sie sich, dass Sie die Option **LINUX ON X86_64 65BIT** herunterladen. Kopieren Sie die Datei (z. B. mit SCP) auf Ihre VM.

    Sie benötigen ein SAP-Benutzerkonto, um auf die Downloadseite für SAP-Software zuzugreifen.

1. (Optional) Die SAP-Datei **Auditlog** wird systemweit verwendet und unterstützt mehrere SAP-Clients. Jede Instanz der SAP-Lösung von Azure Sentinel unterstützt jedoch nur einen einzigen SAP-Client.

    Wenn Sie über ein SAP-System mit mehreren Clients verfügen, wird daher empfohlen, die Datei **Auditlog** nur für den Client zu aktivieren, auf dem Sie die SAP-Lösung bereitstellen, um Datenduplizierung zu vermeiden.


## <a name="deploy-a-linux-vm-for-your-sap-data-connector"></a>Bereitstellen einer Linux-VM für Ihren SAP-Datenconnector

In diesem Verfahren wird beschrieben, wie Sie mithilfe der Azure CLI eine Ubuntu Server 18.04 LTS-VM bereitstellen und ihr eine [systemseitig verwalteten Identität](/azure/active-directory/managed-identities-azure-resources/) zuweisen.

> [!TIP]
> Sie können den Datenconnector auch unter RHEL, Version 7.7 und höher, oder SUSE-Versionen 15 und höher bereitstellen. Beachten Sie, dass alle Betriebssystem- und Patchebenen vollständig auf dem neuesten Stand sein müssen.
>

**So stellen Sie Ihre Ubuntu-VM bereit und bereiten sie vor**:

1. Verwenden Sie den folgenden Befehl als Beispiel, und fügen Sie die Werte für Ihre Ressourcengruppe und den VM-Namen ein:

    ```azurecli
    az vm create  --resource-group [resource group name]   --name [VM Name] --image UbuntuLTS  --admin-username AzureUser --data-disk-sizes-gb 10 – --size Standard_DS2_– --generate-ssh-keys  --assign-identity
    ```

1. Installieren Sie Folgendes auf Ihrer neuen VM:

    - [Venv](https://docs.python.org/3.8/library/venv.html) mit Python-Version 3.8 oder höher
    - [Azure CLI](/cli/azure/), Version 2.8.0 oder höher

> [!IMPORTANT]
> Stellen Sie sicher, dass Sie alle bewährten Sicherheitsmethoden für Ihre Organisation anwenden, genau wie bei jeder anderen VM.
>

Weitere Informationen finden Sie unter [Schnellstart: Erstellen einer Linux-VM mit der Azure CLI](/azure/virtual-machines/linux/quick-create-cli).

## <a name="create-key-vault-for-your-sap-credentials"></a>Erstellen eines Schlüsseltresors für Ihre SAP-Anmeldeinformationen

In diesem Tutorial wird eine neu erstellte oder dedizierte [Azure Key Vault](/azure/key-vault/)-Instanz verwendet, um Anmeldeinformationen für Ihren SAP-Datenconnector zu speichern.

**So erstellen oder bestimmen Sie eine Azure Key Vault-Instanz**:

1. Erstellen Sie eine neue Azure Key Vault-Instanz, oder wählen Sie eine vorhandene Instanz aus, um sie für die Bereitstellung Ihres SAP-Datenconnectors festzulegen.

    Wenn Sie beispielsweise eine neue Key Vault-Instanz erstellen möchten, führen Sie die folgenden Befehle aus. Verwenden Sie dabei den Namen Ihrer Key Vault-Ressourcengruppe, und geben Sie Ihren Key Vault-Namen ein:

    ```azurecli
    kvgp=<KVResourceGroup>

    kvname=<keyvaultname>

    #Create Key Vault
    az keyvault create \
      --name $kvname \
      --resource-group $kvgp
    ```

1. Weisen Sie der verwalteten Identität der VM eine Zugriffsrichtlinie zu, einschließlich GET-, LIST- und SET-Berechtigungen.

    Wählen Sie in Azure Key Vault die Optionen **Zugriffsrichtlinien** > **Zugriffsrichtlinie hinzufügen – Geheimnisberechtigungen: Abrufen, Auflisten und Festlegen** > **Prinzipal auswählen**. Geben Sie den [Namen Ihrer VM](#deploy-a-linux-vm-for-your-sap-data-connector) ein, und wählen Sie dann **Hinzufügen** > **Speichern** aus.

    Weitere Informationen finden Sie in der [Key Vault](/azure/key-vault/general/assign-access-policy-portal)-Dokumentation.

1. Führen Sie den folgenden Befehl aus, um die [Prinzipal-ID der VM](#deploy-a-linux-vm-for-your-sap-data-connector) abzurufen, und geben Sie den Namen Ihrer Azure-Ressourcengruppe ein:

    ```azurecli
    az vm show -g [resource group] -n [Virtual Machine] --query identity.principal– --out tsv
    ```

    Ihre Prinzipal-ID wird angezeigt, damit Sie sie im folgenden Schritt verwenden können.

1. Führen Sie den folgenden Befehl aus, um der Key Vault-Instanz die Zugriffsberechtigungen der VM zuzuweisen. Geben Sie dazu den Namen Ihrer Ressourcengruppe und den Wert für die Prinzipal-ID ein, der im vorherigen Schritt zurückgegeben wurde.

    ```azurecli
    az keyvault set-policy  --name $kv  --resource-group [resource group]  --object-id [Principal ID]  --secret-permissions get set
    ```

## <a name="deploy-your-sap-data-connector"></a>Bereitstellen Ihres SAP-Datenconnectors

Das Bereitstellungsskript für den SAP-Datenconnector von Azure Sentinel installiert die [erforderliche Software](#automatically-installed-software) und dann den Connector auf Ihrer [neu erstellten VM](#deploy-a-linux-vm-for-your-sap-data-connector). Dabei werden Anmeldeinformationen in Ihrem [dedizierten Schlüsseltresor](#create-key-vault-for-your-sap-credentials) gespeichert.

Das Bereitstellungsskript für den SAP-Datenconnector ist im Verzeichnis [Azure Sentinel-GitHub-Repository > DataConnectors > SAP](https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/DataConnectors/SAP/) gespeichert.

Zum Ausführen des Bereitstellungsskripts für den SAP-Datenconnector benötigen Sie folgende Details:

- Ihre Azure Sentinel-Arbeitsbereichsdetails, wie sie im Abschnitt [Voraussetzungen](#prerequisites) aufgeführt sind
- Die im Abschnitt [Voraussetzungen](#prerequisites) aufgeführten SAP-Systemdetails
- Zugriff auf einen VM-Benutzer mit SUDO-Berechtigungen
- Den SAP-Benutzer, den Sie in [Konfigurieren Ihres SAP-Systems](#configure-your-sap-system)erstellt haben, mit der Rolle **/MSFTSEN/SENTINEL_CONNECTOR**
- Die Hilfe Ihres SAP-Teams


**So führen Sie das Bereitstellungsskript für die SAP-Lösung aus**:

1. Führen Sie den folgenden Befehl aus, um die SAP-Lösung auf Ihrer VM bereitzustellen:

    ```azurecli
    wget -O sapcon-sentinel-kickstart.sh https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/Solutions/SAP/sapcon-sentinel-kickstart.sh && bash ./sapcon-sentinel-kickstart.sh
    ```

1. Befolgen Sie die Anweisungen auf dem Bildschirm, um Ihre SAP- und Key Vault-Details einzugeben und die Bereitstellung abzuschließen. Nach Abschluss der Bereitstellung wird eine Bestätigungsmeldung angezeigt:

    ```azurecli
    The process has been successfully completed, thank you!
    ```

    Azure Sentinel beginnt mit dem Abrufen von SAP-Protokollen für die konfigurierte Zeitspanne bis 24 Stunden vor der Initialisierung.

1. Es wird empfohlen, die Systemprotokolle zu überprüfen, um sicherzustellen, dass der Datenconnector Daten überträgt. Führen Sie Folgendes aus:

    ```bash
    docker logs -f sapcon-[SID]
    ```

## <a name="deploy-sap-security-content"></a>Bereitstellen von SAP-Sicherheitsinhalten

Stellen Sie die [SAP-Sicherheitsinhalte](sap-solution-security-content.md) aus den Bereichen **Lösungen** und **Watchlists** von Azure Sentinel bereit.

Mit der Lösung **Azure Sentinel – Kontinuierliche Bedrohungsüberwachung für SAP** kann der SAP-Datenconnector im Azure Sentinel-Bereich **Datenconnectors** angezeigt werden. Außerdem werden die Arbeitsmappe **SAP – Systemanwendungen und Produkte** sowie SAP-bezogene Analyseregeln bereitgestellt.

Fügen Sie Ihrem Azure Sentinel-Arbeitsbereich manuell SAP-bezogene Watchlists hinzu.

**So stellen Sie Sicherheitsinhalte der SAP-Lösung bereit**:

1. Wählen Sie im Azure Sentinel-Navigationsmenü die Option **Lösungen (Vorschau)** aus.

    Auf der Seite **Lösungen** wird eine gefilterte, durchsuchbare Liste von Lösungen angezeigt.

1. Wählen Sie **Azure Sentinel – Kontinuierliche Bedrohungsüberwachung für SAP (Vorschau)** aus, um die SAP-Lösungsseite zu öffnen.

    :::image type="content" source="media/sap/sap-solution.png" alt-text="Lösung: Azure Sentinel – Kontinuierliche Bedrohungsüberwachung für SAP (Vorschau)":::

1. Wählen Sie **Erstellen** aus, um den Lösungsbereitstellungs-Assistenten zu starten, und geben Sie die Details zum Azure-Abonnement, der Ressourcengruppe und dem Log Analytics-Arbeitsbereich ein, in dem Sie die Lösung bereitstellen möchten.

1. Wählen Sie **Weiter** aus, um die Registerkarten **Datenconnectors**, **Analysen** und **Arbeitsmappen** zu durchlaufen. Dort erfahren Sie mehr über die Komponenten, die mit dieser Lösung bereitgestellt werden.

    Der Standardname für die Arbeitsmappe lautet **SAP – Systemanwendungen und Produkte – Vorschau**. Ändern Sie ihn bei Bedarf auf der Registerkarte „Arbeitsmappen“.

    Weitere Informationen finden Sie unter [SAP-Lösung von Azure Sentinel: Referenz zu Sicherheitsinhalten (öffentliche Vorschau)](sap-solution-security-content.md).

1. Warten Sie, bis auf der Registerkarte **Überprüfen und erstellen** die Meldung **Überprüfung erfolgreich** angezeigt wird, und wählen Sie dann **Erstellen** aus, um die Lösung bereitzustellen.

    > [!TIP]
    > Sie können auch **Vorlage herunterladen** auswählen, um einen Link zum Bereitstellen der Lösung als Code zu erhalten.

1. Nach Abschluss der Bereitstellung wird oben rechts auf der Seite eine Bestätigungsmeldung angezeigt.

    Zum Anzeigen der neu bereitgestellten Inhalte navigieren Sie zu:

    - **Bedrohungsmanagement** > **Arbeitsmappen**, um die Arbeitsmappe [SAP – Systemanwendungen und Produkte – Vorschau](sap-solution-security-content.md#sap---system-applications-and-products-workbook) anzuzeigen.
    - **Konfiguration** > **Analysen**, um eine Reihe von [SAP-bezogenen Analyseregeln](sap-solution-security-content.md#built-in-analytics-rules) anzuzeigen.

1. Fügen Sie SAP-bezogene Watchlists hinzu, um sie in Ihrer Suche, für Erkennungsregeln, bei der Bedrohungssuche sowie in Playbooks für die Reaktion auf Bedrohungen zu verwenden. Diese Watchlists stellen die Konfiguration für die SAP-Lösung von Azure Sentinel für kontinuierliche Bedrohungsüberwachung bereit.

    1. Laden Sie SAP-Watchlists aus dem Azure Sentinel-GitHub-Repository unter https://github.com/Azure/Azure-Sentinel/tree/master/Solutions/SAP/Analytics/Watchlists herunter.

    1. Fügen Sie im Bereich **Watchlists** von Azure Sentinel die Watchlists zu ihrem Azure Sentinel-Arbeitsbereich hinzu. Verwenden Sie die heruntergeladenen CSV-Dateien als Quellen, und passen Sie sie dann entsprechend Ihrer Umgebung an. 

        [ ![Azure Sentinel hinzugefügte SAP-bezogene Watchlists](media/sap/sap-watchlists.png) ](media/sap/sap-watchlists.png#lightbox)

        Weitere Informationen finden Sie unter [Verwenden von Azure Sentinel-Watchlists](watchlists.md) und [Verfügbare SAP-Watchlists](sap-solution-security-content.md#available-watchlists).

1. Navigieren Sie in Azure Sentinel zum Datenconnector **Azure Sentinel – Kontinuierliche Bedrohungsüberwachung für SAP**, um die Verbindung zu bestätigen:

    [ ![Seite des Datenconnectors „Azure Sentinel – Kontinuierliche Bedrohungsüberwachung für SAP“](media/sap/sap-data-connector.png) ](media/sap/sap-data-connector.png#lightbox)

    SAP ABAP-Protokolle werden auf der Seite **Protokolle** von Azure Sentinel unter **Benutzerdefinierte Protokolle** angezeigt:

    [ ![SAP ABAP-Protokolle unter „Benutzerdefinierte Protokolle“ in Azure Sentinel](media/sap/sap-logs-in-sentinel.png) ](media/sap/sap-logs-in-sentinel.png#lightbox)

    Weitere Informationen finden Sie unter [Referenz zu Protokollen der SAP-Lösung von Azure Sentinel (öffentliche Vorschau)](sap-solution-log-reference.md).

## <a name="sap-solution-deployment-troubleshooting"></a>Problembehandlung bei der Bereitstellung der SAP-Lösung

Nachdem Sie sowohl den SAP-Datenconnector als auch Sicherheitsinhalte bereitgestellt haben, können die folgenden Fehler oder Probleme auftreten:

|Problem  |Problemumgehung  |
|---------|---------|
|Probleme der Netzwerkkonnektivität mit der SAP-Umgebung oder Azure Sentinel     |  Überprüfen Sie bei Bedarf die Netzwerkkonnektivität.       |
|Falsche SAP ABAP-Benutzeranmeldeinformationen     |Überprüfen Sie Ihre Anmeldeinformationen, und korrigieren Sie diese, indem Sie die richtigen Werte für **ABAPUSER** und **ABAPPASS** in Azure Key Vault anwenden.         |
|Fehlende Berechtigungen, z. B. ist die Rolle **/MSFTSEN/SENTINEL_CONNECTOR** dem SAP-Benutzer nicht entsprechend zugewiesen oder inaktiv     |Beheben Sie diesen Fehler, indem Sie die Rolle zuweisen und sicherstellen, dass sie in Ihrem SAP-System aktiv ist.         |
|Fehlende SAP-Änderungsanforderung     | Vergewissern Sie sich, dass Sie die richtige SAP-Änderungsanforderung importiert haben, wie es unter [Konfigurieren Ihres SAP-Systems](#configure-your-sap-system)beschrieben ist.        |
|Falsche ID oder falscher Schlüssel für den Azure Sentinel-Arbeitsbereich im Bereitstellungsskript eingegeben     |  Zur Behebung dieses Fehlers geben Sie die richtigen Anmeldeinformationen in Azure KeyVault ein.       |
|Beschädigte oder fehlende SAP SDK-Datei     | Beheben Sie diesen Fehler, indem Sie das SAP SDK neu installieren und sicherstellen, dass Sie die richtige 64-Bit-Versionen von Linux verwenden.        |
|Fehlende Daten in Ihrer Arbeitsmappe oder Warnungen     |    Vergewissern Sie sich, dass die **Auditlog**-Richtlinie auf SAP-Seite ordnungsgemäß aktiviert ist und keine Fehler in der Protokolldatei enthalten sind. Verwenden Sie die Transaktion **RSAU_CONFIG_LOG** für diesen Schritt.     |
|     |         |

> [!TIP]
> Es wird dringend empfohlen, die Systemprotokolle nach der Installation des Datenconnectors zu überprüfen. Führen Sie Folgendes aus:
>
> ```bash
> docker logs -f sapcon-[SID]
> ```
>
Weitere Informationen finden Sie unter

- [Anzeigen aller Docker-Ausführungsprotokolle](#view-all-docker-execution-logs)
- [Überprüfen und Aktualisieren der Konfiguration des SAP-Datenconnectors](#review-and-update-the-sap-data-connector-configuration)
- [Nützliche Docker-Befehle](#useful-docker-commands)

### <a name="view-all-docker-execution-logs"></a>Anzeigen aller Docker-Ausführungsprotokolle

Führen Sie einen der folgenden Befehle aus, um alle Docker-Ausführungsprotokolle für die Bereitstellung des SAP-Datenconnectors von Azure Sentinel anzuzeigen:

```bash
docker exec -it sapcon-[SID] bash && cd /sapcon-app/sapcon/logs
```

oder

```bash
docker exec –it sapcon-[SID] cat /sapcon-app/sapcon/logs/[FILE_LOGNAME]
```

Es sollte eine Ausgabe angezeigt werden, die Folgendem ähnelt:

```bash
Logs directory:
root@644c46cd82a9:/sapcon-app# ls sapcon/logs/ -l
total 508
-rwxr-xr-x 1 root root      0 Mar 12 09:22 ' __init__.py'
-rw-r--r-- 1 root root    282 Mar 12 16:01  ABAPAppLog.log
-rw-r--r-- 1 root root   1056 Mar 12 16:01  ABAPAuditLog.log
-rw-r--r-- 1 root root    465 Mar 12 16:01  ABAPCRLog.log
-rw-r--r-- 1 root root    515 Mar 12 16:01  ABAPChangeDocsLog.log
-rw-r--r-- 1 root root    282 Mar 12 16:01  ABAPJobLog.log
-rw-r--r-- 1 root root    480 Mar 12 16:01  ABAPSpoolLog.log
-rw-r--r-- 1 root root    525 Mar 12 16:01  ABAPSpoolOutputLog.log
-rw-r--r-- 1 root root      0 Mar 12 15:51  ABAPTableDataLog.log
-rw-r--r-- 1 root root    495 Mar 12 16:01  ABAPWorkflowLog.log
-rw-r--r-- 1 root root 465311 Mar 14 06:54  API.log # view this log to see submits of data into Azure Sentinel
-rw-r--r-- 1 root root      0 Mar 12 15:51  LogsDeltaManager.log
-rw-r--r-- 1 root root      0 Mar 12 15:51  PersistenceManager.log
-rw-r--r-- 1 root root   4830 Mar 12 16:01  RFC.log
-rw-r--r-- 1 root root   5595 Mar 12 16:03  SystemAdmin.log
```

### <a name="review-and-update-the-sap-data-connector-configuration"></a>Überprüfen und Aktualisieren der Konfiguration des SAP-Datenconnectors

Wenn Sie die Konfigurationsdatei des SAP-Datenconnectors überprüfen und manuelle Aktualisierungen vornehmen möchten, führen Sie die folgenden Schritte aus:

1. Öffnen Sie auf Ihrer VM im Basisverzeichnis des Benutzers die Datei **~/sapcon/[SID]/systemconfig.ini**.
1. Aktualisieren Sie die Konfiguration bei Bedarf, und starten Sie den Container dann neu:

    ```bash
    docker restart sapcon-[SID]
    ```

### <a name="useful-docker-commands"></a>Nützliche Docker-Befehle

Bei der Problembehandlung des SAP-Datenconnectors können die folgenden Befehle nützlich sein:

|Funktion  |Befehl  |
|---------|---------|
|**Beenden des Docker-Containers**     |  `docker stop sapcon-[SID]`       |
|**Starten des Docker-Containers**     |`docker start sapcon-[SID]`         |
|**Anzeigen von Docker-Systemprotokollen**     |  `docker logs -f sapcon-[SID]`       |
|**Aufrufen des Docker-Containers**     |   `docker exec -it sapcon-[SID] bash`      |
|     |         |

Weitere Informationen finden Sie in der [Dokumentation zur Docker CLI](https://docs.docker.com/engine/reference/commandline/docker/).

## <a name="update-your-sap-data-connector"></a>Aktualisieren Ihres SAP-Datenconnectors

Wenn Sie über einen Docker-Container verfügen, der bereits mit einer früheren Version des SAP-Datenconnectors ausgeführt wird, führen Sie das Updateskript für den SAP-Datenconnector aus, um die neuesten verfügbaren Features zu erhalten.

1. Vergewissern Sie sich, dass Sie über die neuesten Versionen der relevanten Bereitstellungsskripts aus dem Azure Sentinel-GitHub-Repository verfügen. Führen Sie Folgendes aus:

    ```azurecli
    - wget -O sapcon-sentinel-kickstart.sh https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/Solutions/SAP/sapcon-sentinel-kickstart.sh && bash ./sapcon-sentinel-update.sh
    ```

1. Führen Sie den folgenden Befehl auf Ihrem Computer mit dem SAP-Datenconnector aus:

    ```azurecli
    ./ sapcon-instance-update.sh
    ```

Der Docker-Container des SAP-Datenconnectors auf Ihrem Computer wird aktualisiert.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die SAP-Lösungen von Azure Sentinel:

- [Bereitstellen der SAP-Lösung von Azure Sentinel mithilfe alternativer Bereitstellungen](sap-solution-deploy-alternate.md)
- [Detaillierte SAP-Anforderungen für die SAP-Lösung von Azure Sentinel](sap-solution-detailed-requirements.md)
- [Referenz zu Protokollen der SAP-Lösung von Azure Sentinel](sap-solution-log-reference.md)
- [SAP-Lösung von Azure Sentinel: integrierte Sicherheitsinhalte](sap-solution-security-content.md)

Weitere Informationen finden Sie unter [Azure Sentinel-Lösungen](sentinel-solutions.md).
