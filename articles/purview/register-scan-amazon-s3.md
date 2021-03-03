---
title: Überprüfen von Amazon S3-Buckets
description: In dieser Schrittanleitung wird beschrieben, wie Sie Amazon S3-Buckets überprüfen.
author: batamig
ms.author: bagol
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 01/19/2021
ms.custom: references_regions
ms.openlocfilehash: 7d3fd0b1ffb87a84772000702b958c52ed1cc47c
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101678775"
---
# <a name="azure-purview-connector-for-amazon-s3"></a>Azure Purview-Connector für Amazon S3

In dieser Schrittanleitung erfahren Sie, wie Sie Azure Purview verwenden, um Ihre unstrukturierten Daten zu überprüfen, die derzeit in Amazon S3-Standardbuckets gespeichert sind, und ermitteln, welche Typen vertraulicher Informationen in Ihren Daten vorhanden sind. In diesem Leitfaden wird außerdem beschrieben, wie Sie die Amazon S3-Buckets identifizieren, in denen die Daten derzeit gespeichert sind, um Information Protection und Datencompliance zu vereinfachen.

Verwenden Sie für diesen Dienst Purview, um ein Microsoft-Konto mit sicherem Zugriff auf AWS bereitzustellen, wo die Purview-Überprüfung ausgeführt wird. Die Purview-Überprüfung verwendet diesen Zugriff auf Ihre Amazon S3-Buckets, um Ihre Daten zu lesen, und meldet dann die Überprüfungsergebnisse, einschließlich nur der Metadaten und der Klassifizierung, zurück an Azure. Verwenden Sie die Purview-Klassifizierungs- und -Beschriftungsberichte, um die Ergebnisse Ihrer Datenüberprüfungen zu analysieren und zu prüfen.

In dieser Schrittanleitung erfahren Sie, wie Sie Amazon S3-Buckets als Purview-Ressourcen hinzufügen und eine Überprüfung für Ihre Amazon S3-Daten erstellen.

## <a name="purview-scope-for-amazon-s3"></a>Purview-Umfang für Amazon S3

Der folgende Umfang ist spezifisch für die Registrierung und Überprüfung von Amazon S3-Buckets als Purview-Datenquellen.

|`Scope`  |BESCHREIBUNG  |
|---------|---------|
|**Datengrenzwerte**     |    Der Purview-Überprüfungsdienst unterstützt derzeit das Überprüfen von Amazon S3-Buckets von bis zu 100 GB Daten pro Mandant.     |
|**Dateitypen**     | Der Purview-Überprüfungsdienst unterstützt derzeit die folgenden Dateitypen: <br><br>.avro, .csv, .doc, .docm, .docx, .dot, .json, .odp, .ods, .odt, .orc, .parquet, .pdf, .pot, .pps, .ppsx, .ppt, .pptm, .pptx, .psv, .ssv, .tsv, .txt, .xlc, .xls, .xlsb, .xlsm, .xlsx, .xlt, .xml        |
|**Regionen**     | Der Purview-Connector für den Amazon S3-Dienst wird zurzeit nur in den Regionen **AWS USA, Osten (Ohio)** und **Europa (Frankfurt)** bereitgestellt. <br><br>Weitere Informationen finden Sie unter [Speicher- und Überprüfungsregionen](#storage-and-scanning-regions).   |
|     |         |

Weitere Informationen finden Sie in den dokumentierten Purview-Grenzwerten unter:

- [Verwalten und Erhöhen der Kontingente für Ressourcen mit Azure Purview](how-to-manage-quotas.md)
- [Unterstützte Datenquellen und Dateitypen in Azure Purview](sources-and-scans.md)
### <a name="storage-and-scanning-regions"></a>Speicher- und Überprüfungsregionen

In der folgenden Tabelle werden die Regionen, in denen Ihre Daten gespeichert sind, der jeweiligen Region zugeordnet, in der sie von Azure Purview überprüft würden.

> [!IMPORTANT]
> Kunden werden alle zugehörigen Datenübertragungsgebühren gemäß der Region Ihres Buckets in Rechnung gestellt.
>

| Speicherregion | Überprüfungsregion |
| ------------------------------- | ------------------------------------- |
| USA, Osten (Ohio)                  | USA, Osten (Ohio)                        |
| USA, Osten (Nord Virginia)           | USA, Osten (Ohio)                        |
| USA, Westen (Nord Kalifornien)         | USA, Osten (Ohio)                        |
| USA, Westen (Oregon)                | USA, Osten (Ohio)                        |
| Afrika (Kapstadt)              | Europa (Frankfurt)                    |
| Asien-Pazifik (Hongkong)        | Europa (Frankfurt)                    |
| Asien-Pazifik (Mumbai)           | Europa (Frankfurt)                    |
| Asien-Pazifik (Osaka lokal)      | Europa (Frankfurt)                    |
| Asien-Pazifik (Seoul)            | Europa (Frankfurt)                    |
| Asien-Pazifik, (Singapur)        | Europa (Frankfurt)                    |
| Asien-Pazifik, (Sydney)           | Europa (Frankfurt)                    |
| Asien-Pazifik, (Tokio)            | Europa (Frankfurt)                    |
| Kanada (Mitte)                | USA, Osten (Ohio)                        |
| China (Beijing)                 | Europa (Frankfurt)                    |
| China (Ningxia)                 | Europa (Frankfurt)                    |
| Europa (Frankfurt)              | Europa (Frankfurt)                    |
| Europa (Irland)                | Europa (Frankfurt)                    |
| Europa (London)                 | Europa (Frankfurt)                    |
| Europa (Mailand)                  | Europa (Frankfurt)                    |
| Europa (Paris)                  | Europa (Frankfurt)                    |
| Europa (Stockholm)              | Europa (Frankfurt)                    |
| Naher Osten (Bahrain)           | Europa (Frankfurt)                    |
| Südamerika (São Paulo)       | USA, Osten (Ohio)                        |
| | |
## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind, bevor Sie Ihre Amazon S3-Buckets als Purview-Datenquellen hinzufügen und Ihre S3-Daten überprüfen.

- Sie müssen ein Azure Purview-Datenquellenadministrator sein.

- Wenn Sie Ihre Buckets als Purview-Ressourcen hinzufügen, benötigen Sie die Werte Ihres [AWS-ARN](#retrieve-your-new-role-arn), den [Bucketnamen](#retrieve-your-amazon-s3-bucket-name) und manchmal Ihre [AWS-Konto-ID](#locate-your-aws-account-id).

### <a name="create-a-purview-account"></a>Erstellen eines Purview-Kontos

- **Wenn Sie bereits über ein Purview-Konto verfügen,** können Sie mit den Konfigurationen fortfahren, die für die AWS S3-Unterstützung erforderlich sind. Beginnen Sie mit dem [Erstellen von Purview-Anmeldeinformationen für Ihre AWS-Bucketüberprüfung](#create-a-purview-credential-for-your-aws-bucket-scan).

- **Wenn Sie ein Purview-Konto erstellen müssen,** befolgen Sie die Anweisungen unter [Erstellen einer Azure Purview-Kontoinstanz](create-catalog-portal.md). Nachdem Sie Ihr Konto erstellt haben, kehren Sie hierhin zurück, um die Konfiguration abzuschließen und mit der Verwendung des Purview-Connectors für Amazon S3 zu beginnen.

### <a name="create-a-purview-credential-for-your-aws-bucket-scan"></a>Erstellen von Purview-Anmeldeinformationen für Ihre AWS-Bucketüberprüfung

In diesem Verfahren wird beschrieben, wie Sie neue Purview-Anmeldeinformationen erstellen, die beim Überprüfen Ihrer AWS-Buckets verwendet werden sollen.

> [!TIP]
> Sie können im Verlauf des Prozesses auch neue Anmeldeinformationen erstellen, während Sie [Ihre Überprüfung konfigurieren](#create-a-scan-for-your-amazon-s3-bucket). Wählen Sie in diesem Fall im Feld **Anmeldeinformationen** die Option **Neu** aus.
>

1. Navigieren Sie in Purview zum **Verwaltungscenter**, und wählen Sie unter **Sicherheit und Zugriff** die Option **Anmeldeinformationen** aus.

1. Wählen Sie **Neu** aus, und verwenden Sie im rechts angezeigten Bereich **Neue Anmeldeinformationen** die folgenden Felder, um Ihre Purview-Anmeldeinformationen zu erstellen:

    |Feld |BESCHREIBUNG  |
    |---------|---------|
    |**Name**     |Geben Sie einen aussagekräftigen Namen für diese Anmeldeinformationen ein, oder verwenden Sie den Standardwert.        |
    |**Beschreibung**     |Geben Sie eine optionale Beschreibung für diese Anmeldeinformationen ein, z. B. `Used to scan the tutorial S3 buckets` (Zum Überprüfen der Tutorial-S3-Buckets).         |
    |**Authentifizierungsmethode**     |Wählen Sie **Rollen-ARN** aus, da Sie für den Zugriff auf Ihren Bucket einen Rollen-ARN verwenden.         |
    |**Microsoft-Konto-ID**     |Klicken Sie, um diesen Wert in die Zwischenablage zu kopieren. Verwenden Sie diesen Wert als **Microsoft-Konto ID**, wenn [Sie Ihren Rollen-ARN in AWS erstellen](#create-a-new-aws-role-for-purview).           |
    |**Externe ID**     |Klicken Sie, um diesen Wert in die Zwischenablage zu kopieren. Verwenden Sie diesen Wert als **Externe ID**, wenn [Sie Ihren Rollen-ARN in AWS erstellen](#create-a-new-aws-role-for-purview).        |
    |**Rollen-ARN**     | Nachdem Sie [Ihre Amazon IAM-Rolle erstellt haben](#create-a-new-aws-role-for-purview), navigieren Sie im IAM-Bereich zu Ihrer Rolle, kopieren Sie den **Rollen-ARN**-Wert, und geben Sie ihn hier ein. Beispiel: `arn:aws:iam::284759281674:role/S3Role`. <br><br>Weitere Informationen finden Sie unter [Abrufen Ihres neuen Rollen-ARN](#retrieve-your-new-role-arn). |
    | | |

    Wählen Sie **Erstellen** aus, wenn Sie fertig sind, um die Erstellung der Anmeldeinformationen abzuschließen.

Weitere Informationen zu Purview-Anmeldeinformationen finden Sie in der [Dokumentation zur öffentlichen Vorschau von Azure Purview](manage-credentials.md).

### <a name="create-a-new-aws-role-for-purview"></a>Erstellen einer neuen AWS-Rolle für Purview

1.  Öffnen Sie Ihre **Amazon Web Services**-Konsole, und wählen Sie unter **Sicherheit, Identität und Compliance** den Eintrag **IAM** aus.

1. Wählen Sie zuerst **Rollen** und dann **Rolle erstellen** aus.

1. Wählen Sie **Ein anderes AWS-Konto** aus, und geben Sie dann die folgenden Werte ein:

    |Feld  |BESCHREIBUNG  |
    |---------|---------|
    |**Konto-ID**     |    Geben Sie Ihre Microsoft-Konto-ID ein. Beispiel: `615019938638`     |
    |**Externe ID**     |   Wählen Sie unter „Optionen“ **Externe ID anfordern...** aus, und geben Sie dann Ihre externe ID in das angegebene Feld ein. <br>Beispiel: `e7e2b8a3-0a9f-414f-a065-afaf4ac6d994`    <br><br>Sie finden diese externe ID, wenn Sie .  |
    | | |

    > [!NOTE]
    > Sie finden den Wert für die **Microsoft-Konto-ID** als auch für die **Externe ID** im Purview-Bereich **Verwaltungscenter** > **Anmeldeinformationen**, in dem Sie [Ihre Purview-Anmeldeinformationen erstellt haben](#create-a-purview-credential-for-your-aws-bucket-scan).
    >

    Beispiel:

    ![Hinzufügen der Microsoft-Konto-ID zu Ihrem AWS-Konto.](./media/register-scan-amazon-s3/aws-create-role-amazon-s3.png)

1. Filtern Sie im Bereich **Rollen > Berechtigungsrichtlinien anfügen** die angezeigten Berechtigungen nach **S3**. Wählen Sie **AmazonS3ReadOnlyAccess** und dann **Weiter: Tags** aus.

    ![Wählen Sie die „ReadOnlyAccess“-Richtlinie für die neue Amazon S3-Überprüfungsrolle aus.](./media/register-scan-amazon-s3/aws-permission-role-amazon-s3.png)

1. Im Bereich **Tags hinzufügen (optional)** können Sie optional auswählen, dass ein aussagekräftiges Tag für diese neue Rolle erstellt werden soll. Nützliche Tags ermöglichen es Ihnen, den Zugriff für jede von Ihnen erstellte Rolle zu organisieren, nachzuverfolgen und zu steuern.

    Geben Sie nach Bedarf einen neuen Schlüssel und Wert für Ihr Tag ein. Wenn Sie fertig sind, oder wenn Sie diesen Schritt überspringen möchten, wählen Sie **Weiter: Überprüfen** aus, um die Rollendetails zu überprüfen und die Rollenerstellung abzuschließen.

    ![Fügen Sie ein aussagekräftiges Tag hinzu, um den Zugriff für Ihre neue Rolle zu organisieren, nachzuverfolgen oder zu steuern.](./media/register-scan-amazon-s3/add-tag-new-role.png)

1. Gehen Sie im Bereich **Überprüfen** wie folgt vor:

    - Geben Sie im Feld **Rollenname** einen aussagekräftigen Namen für Ihre Rolle ein.
    - Geben Sie im Feld **Rollenbeschreibung** eine optionale Beschreibung ein, um den Zweck der Rolle zu bezeichnen.
    - Vergewissern Sie sich im Abschnitt **Richtlinien**, dass die richtige Richtlinie (**AmazonS3ReadOnlyAccess**) an die Rolle angefügt ist.

    Wählen Sie dann **Rolle erstellen** aus, um den Prozess abzuschließen.

    Beispiel:

    ![Überprüfen von Details vor dem Erstellen Ihrer Rolle.](./media/register-scan-amazon-s3/review-role.png)


### <a name="configure-scanning-for-encrypted-amazon-s3-buckets"></a>Konfigurieren der Überprüfung verschlüsselter Amazon S3-Buckets

AWS-Buckets unterstützen mehrere Verschlüsselungstypen. Buckets, die **AWS-KMS**-Verschlüsselung verwenden, ist eine spezielle Konfiguration erforderlich, um die Überprüfung zu ermöglichen.

> [!NOTE]
> Für Buckets, die keine Verschlüsselung bzw. AES-256- oder AWS-KMS S3-Verschlüsselung verwenden, überspringen Sie diesen Abschnitt, und fahren Sie mit dem [Abrufen Ihres Amazon S3-Bucketnamens](#retrieve-your-amazon-s3-bucket-name) fort.
>

**So überprüfen Sie den Verschlüsselungstyp, der in Ihren Amazon S3-Buckets verwendet wird**

1. Navigieren Sie in AWS zu **Speicher** > **S3**, und wählen Sie links im Menü **Buckets** aus.

    ![Wählen Sie die Registerkarte „Amazon S3-Buckets“ aus.](./media/register-scan-amazon-s3/check-encryption-type-buckets.png)

1. Wählen Sie den Bucket aus, den Sie überprüfen möchten. Wählen Sie auf der Seite mit den Details des Buckets die Registerkarte **Eigenschaften** aus, und scrollen Sie nach unten zum Bereich **Standardverschlüsselung**.

    - Wenn der von Ihnen ausgewählte Bucket für eine andere Verschlüsselung als **AWS-KMS** konfiguriert ist, und auch, wenn die Standardverschlüsselung für Ihren Bucket **deaktiviert** ist, überspringen Sie den Rest dieses Verfahrens, und fahren Sie mit dem [Abrufen Ihres Amazon S3-Bucketnamens](#retrieve-your-amazon-s3-bucket-name) fort.

    - Wenn der von Ihnen ausgewählte Bucket für **AWS-KMS**-Verschlüsselung konfiguriert ist, fahren Sie wie unten beschrieben fort, um eine neue Richtlinie hinzuzufügen, die das Überprüfen eines Bucket mit benutzerdefinierter **AWS-KMS**-Verschlüsselung zulässt.

    Beispiel:

    ![Anzeigen eines mit AWS-KMS-Verschlüsselung konfigurierten Amazon S3-Buckets](./media/register-scan-amazon-s3/default-encryption-buckets.png)

**So fügen Sie eine neue Richtlinie hinzu, die das Überprüfen eines Buckets mit benutzerdefinierter AWS-KMS-Verschlüsselung zulässt**

1. Navigieren Sie in AWS zu **Dienste** >  **IAM** >  **Richtlinien**, und wählen Sie **Richtlinie erstellen** aus.

1. Definieren Sie auf der Registerkarte **Richtlinie erstellen** > **Visueller Editor** Ihre Richtlinie mit den folgenden Werten:

    |Feld  |BESCHREIBUNG  |
    |---------|---------|
    |**Service**     |  Geben Sie **KMS** ein, und wählen Sie den Eintrag aus.       |
    |**Aktionen**     | Wählen Sie unter **Zugriffsebene** die Option **Schreiben** aus, um den Abschnitt **Schreiben** zu erweitern.<br>Sobald dieser erweitert ist, wählen Sie nur die Option **Entschlüsseln** aus.        |
    |**Ressourcen**     |Wählen Sie eine bestimmte Ressource oder **Alle Ressourcen** aus.         |
    | | |

    Wenn Sie fertig sind, wählen Sie **Richtlinie überprüfen** aus, um fortzufahren.

    ![Erstellen Sie eine Richtlinie zum Überprüfen eines Buckets mit AWS-KMS-Verschlüsselung.](./media/register-scan-amazon-s3/create-policy-kms.png)

1. Geben Sie auf der Seite **Richtlinie überprüfen** einen aussagekräftigen Namen für die Richtlinie und eine optionale Beschreibung ein, und wählen Sie dann **Richtlinie erstellen** aus.

    Die neu erstellte Richtlinie wird Ihrer Liste mit Richtlinien hinzugefügt.

1. Fügen Sie Ihre neue Richtlinie an die Rolle an, die Sie zum Überprüfen hinzugefügt haben.

    1. Navigieren Sie zurück zur Seite **IAM** > **Rollen**, und wählen Sie die Rolle aus, die Sie [zuvor](#create-a-new-aws-role-for-purview) hinzugefügt hatten.

    1. Wählen Sie auf der Registerkarte **Berechtigungen** die Option **Richtlinien anfügen** aus.

        ![Wählen Sie auf der Registerkarte „Berechtigungen“ Ihrer Rolle „Richtlinien anfügen“ aus.](./media/register-scan-amazon-s3/iam-attach-policies.png)

    1. Suchen Sie auf der Seite **Berechtigungen anfügen** nach der oben erstellten neuen Richtlinie, und wählen Sie diese aus. Wählen Sie **Richtlinie anfügen** aus, um der Rolle Ihre Richtlinie anzufügen.

        Die Seite **Zusammenfassung** wird aktualisiert, und Ihre neue Richtlinie ist Ihrer Rolle angefügt.

        ![Anzeigen einer aktualisierten Zusammenfassungsseite mit der neuen Richtlinie, die an Ihre Rolle angefügt ist.](./media/register-scan-amazon-s3/attach-policy-role.png)

### <a name="retrieve-your-new-role-arn"></a>Abrufen Ihres neuen Rollen-ARN

Sie müssen Ihren AWS-Rollen-ARN aufzeichnen und in Purview kopieren, wenn Sie [eine Überprüfung für Ihren Amazon S3-Bucket erstellen](#create-a-scan-for-your-amazon-s3-bucket).

**So rufen Sie Ihren Rollen-ARN ab**

1. Suchen Sie im AWS-Bereich **Identity & Access Management (IAM)**  > **Rollen** nach der neuen Rolle, die Sie [für Purview erstellt haben](#create-a-purview-credential-for-your-aws-bucket-scan), und wählen Sie sie aus.

1. Wählen Sie auf der Seite **Zusammenfassung** der Rolle rechts neben dem **Rollen-ARN**-Wert die Schaltfläche **In Zwischenablage kopieren** aus.

    ![Kopieren Sie den Rollen-ARN-Wert in die Zwischenablage.](./media/register-scan-amazon-s3/aws-copy-role-purview.png)

1. Fügen Sie diesen Wert an einem sicheren Ort ein, sodass er einsatzbereit ist, wenn Sie [eine Überprüfung für Ihren Amazon S3-Bucket erstellen](#create-a-scan-for-your-amazon-s3-bucket).

### <a name="retrieve-your-amazon-s3-bucket-name"></a>Abrufen Ihres Amazon S3-Bucketnamens

Sie benötigen den Namen Ihres Amazon S3-Buckets, um ihn in Purview zu kopieren, wenn Sie [eine Überprüfung für Ihren Amazon S3-Bucket erstellen](#create-a-scan-for-your-amazon-s3-bucket).

**So rufen Sie Ihren Bucketnamen ab**

1. Navigieren Sie in AWS zu **Speicher** > **S3**, und wählen Sie links im Menü **Buckets** aus.

    ![Zeigen Sie die Registerkarte „Amazon S3-Buckets“ an.](./media/register-scan-amazon-s3/check-encryption-type-buckets.png)

1. Suchen Sie nach Ihrem Bucket, und wählen Sie ihn aus, um die Seite mit den Details des Buckets anzuzeigen, und kopieren Sie dann den Bucketnamen in die Zwischenablage.

    Beispiel:

    ![Abrufen und Kopieren der S3-Bucket-URL.](./media/register-scan-amazon-s3/retrieve-bucket-url-amazon.png)

    Fügen Sie Ihren Bucketnamen in eine sichere Datei ein, und fügen Sie ein `s3://`-Präfix hinzu, um den Wert zu erstellen, den Sie eingeben müssen, wenn Sie Ihren Bucket als Purview-Ressource konfigurieren.

    Beispiel: `s3://purview-tutorial-bucket`

> [!NOTE]
> Nur die Stammebene Ihres Buckets wird als Purview-Datenquelle unterstützt. Die folgende URL, die einen Unterordner enthält, wird beispielsweise *nicht* unterstützt: `s3://purview-tutorial-bucket/view-data`
>

### <a name="locate-your-aws-account-id"></a>Auffinden Ihrer AWS-Konto-ID

Sie benötigen Ihre AWS-Konto-ID, um Ihr AWS-Konto als Purview-Datenquelle zu registrieren, zusammen mit allen zugehörigen Buckets.

Ihre AWS-Konto-ID ist die ID, die Sie zum Anmelden bei der AWS-Konsole verwenden. Sie finden sie auch, sobald Sie beim IAM-Dashboard angemeldet sind, im linken Bereich unter den Navigationsoptionen sowie am oberen Rand als numerischer Teil Ihrer Anmelde-URL:

Beispiel:

![Abrufen Ihrer AWS-Konto-ID.](./media/register-scan-amazon-s3/aws-locate-account-id.png)


## <a name="add-a-single-amazon-s3-bucket-as-a-purview-resource"></a>Hinzufügen eines einzelnen Amazon S3-Buckets als Purview-Ressource

Verwenden Sie dieses Verfahren, wenn Sie nur über einen einzigen S3-Bucket verfügen, den Sie als Datenquelle bei Purview registrieren möchten, oder wenn Sie über mehrere Buckets in Ihrem AWS-Konto verfügen, aber nicht alle bei Purview registrieren möchten.

1. Starten Sie das Purview-Portal mithilfe des dedizierten Purview-Connectors für die Amazon S3-URL. Diese URL wurde Ihnen vom Amazon S3 Purview-Connector-Produktverwaltungsteam bereitgestellt.

    ![Starten Sie das Purview-Portal.](./media/register-scan-amazon-s3/purview-portal-amazon-s3.png)

1. Navigieren Sie zur Azure Purview-Seite **Quellen**, und wählen Sie **Registrieren** ![Symbol „Registrieren“](./media/register-scan-amazon-s3/register-button.png) aus. > **Amazon S3** > **Weiter**.

    ![Fügen Sie einen Amazon AWS-Bucket als Purview-Datenquelle hinzu.](./media/register-scan-amazon-s3/add-s3-datasource-to-purview.png)

    > [!TIP]
    > Wenn Sie mehrere [Sammlungen](manage-data-sources.md#manage-collections) haben und Ihr Amazon S3 zu einer bestimmten Sammlung hinzufügen möchten, wählen Sie oben rechts die **Zuordnungsansicht** aus, und wählen Sie dann **Registrieren** ![Symbol „Registrieren“](./media/register-scan-amazon-s3/register-button.png) aus. Schaltfläche in Ihrer Sammlung.
    >

1. Geben Sie im Bereich **Quellen registrieren (Amazon S3)** , der geöffnet wird, die folgenden Details ein:

    |Feld  |BESCHREIBUNG  |
    |---------|---------|
    |**Name**     |Geben Sie einen aussagekräftigen Namen ein, oder verwenden Sie den bereitgestellten Standardwert.         |
    |**Bucket-URL**     | Geben Sie Ihre AWS-Bucket-URL mit der folgenden Syntax ein: `s3://<bucketName>`     <br><br>**Hinweis**: Stellen Sie sicher, dass Sie nur die Stammebene Ihres Buckets ohne Unterordner verwenden. Weitere Informationen finden Sie unter [Abrufen Ihres Amazon S3-Bucketnamens](#retrieve-your-amazon-s3-bucket-name). |
    |**Auswählen einer Sammlung** |Wenn Sie sich für die Registrierung einer Datenquelle aus einer Sammlung heraus entschieden haben, wird diese Sammlung bereits aufgeführt. <br><br>Wählen Sie nach Bedarf eine andere Sammlung aus, **Keine**, um keine Sammlung zuzuweisen, oder **Neu**, um jetzt eine neue Sammlung zu erstellen. <br><br>Weitere Informationen zu Purview-Sammlungen finden Sie unter [Verwalten von Datenquellen in Azure Purview](manage-data-sources.md#manage-collections).|
    | | |

    Wenn Sie fertig sind, wählen Sie **Fertig stellen** aus, um die Registrierung abzuschließen.

Fahren Sie mit dem [Erstellen einer Überprüfung für Ihren Amazon S3-Bucket](#create-a-scan-for-your-amazon-s3-bucket) fort.

## <a name="add-all-of-your-amazon-s3-buckets-as-purview-resources"></a>Hinzufügen aller Ihrer Amazon S3-Buckets als Purview-Ressourcen

Verwenden Sie dieses Verfahren, wenn Sie über mehrere S3-Buckets in Ihrem Amazon-Konto verfügen und alle als Purview-Datenquellen registrieren möchten.

1. Starten Sie das Purview-Portal mithilfe des dedizierten Purview-Connectors für die Amazon S3-URL. Diese URL wurde Ihnen vom Amazon S3 Purview-Connector-Produktverwaltungsteam bereitgestellt.

    ![Starten des Connectors für das dedizierte Purview-Portal von Amazon S3](./media/register-scan-amazon-s3/purview-portal-amazon-s3.png)

1. Navigieren Sie zur Azure Purview-Seite **Quellen**, und wählen Sie **Registrieren** ![Symbol „Registrieren“](./media/register-scan-amazon-s3/register-button.png) aus. > **Amazon-Konten** > **Weiter**.

    ![Fügen Sie ein Amazon-Konto als Purview-Datenquelle hinzu.](./media/register-scan-amazon-s3/add-s3-account-to-purview.png)

    > [!TIP]
    > Wenn Sie mehrere [Sammlungen](manage-data-sources.md#manage-collections) haben und Ihr Amazon S3 zu einer bestimmten Sammlung hinzufügen möchten, wählen Sie oben rechts die **Zuordnungsansicht** aus, und wählen Sie dann **Registrieren** ![Symbol „Registrieren“](./media/register-scan-amazon-s3/register-button.png) aus. Schaltfläche in Ihrer Sammlung.
    >

1. Geben Sie im Bereich **Quellen registrieren (Amazon S3)** , der geöffnet wird, die folgenden Details ein:

    |Feld  |BESCHREIBUNG  |
    |---------|---------|
    |**Name**     |Geben Sie einen aussagekräftigen Namen ein, oder verwenden Sie den bereitgestellten Standardwert.         |
    |**AWS-Konto-ID**     | Geben Sie Ihre AWS-Konto-ID ein. Weitere Informationen finden Sie unter [Auffinden Ihrer AWS-Konto-ID](#locate-your-aws-account-id).|
    |**Auswählen einer Sammlung** |Wenn Sie sich für die Registrierung einer Datenquelle aus einer Sammlung heraus entschieden haben, wird diese Sammlung bereits aufgeführt. <br><br>Wählen Sie nach Bedarf eine andere Sammlung aus, **Keine**, um keine Sammlung zuzuweisen, oder **Neu**, um jetzt eine neue Sammlung zu erstellen. <br><br>Weitere Informationen zu Purview-Sammlungen finden Sie unter [Verwalten von Datenquellen in Azure Purview](manage-data-sources.md#manage-collections).|
    | | |

    Wenn Sie fertig sind, wählen Sie **Fertig stellen** aus, um die Registrierung abzuschließen.

Fahren Sie mit dem [Erstellen einer Überprüfung für Ihren Amazon S3-Bucket](#create-a-scan-for-your-amazon-s3-bucket) fort.

## <a name="create-a-scan-for-your-amazon-s3-bucket"></a>Erstellen einer Überprüfung für Ihren Amazon S3-Bucket

Nachdem Sie Ihre Buckets als Purview-Datenquellen hinzugefügt haben, können Sie eine Überprüfung so konfigurieren, dass sie in geplanten Intervallen oder sofort ausgeführt wird.

1. Navigieren Sie zum Azure Purview-Bereich **Quellen**, und führen Sie einen der folgenden Schritte aus:

    - Wählen Sie in der **Zuordnungsansicht** die Option **Neue Überprüfung** ![Symbol „Neue Überprüfung“](./media/register-scan-amazon-s3/new-scan-button.png) aus. in Ihrem Feld „Datenquelle“.
    - Zeigen Sie in der **Listenansicht** mit dem Mauszeiger auf die Zeile für Ihre Datenquelle, und wählen Sie **Neue Überprüfung** ![ Symbol „Neue Überprüfung“](./media/register-scan-amazon-s3/new-scan-button.png) aus.

1. Definieren Sie im rechts angezeigten Bereich **Überprüfen** die folgenden Felder, und wählen Sie dann **Weiter** aus:

    |Feld  |BESCHREIBUNG  |
    |---------|---------|
    |**Name**     |  Geben Sie einen aussagekräftigen Namen für Ihre Überprüfung ein, oder verwenden Sie den Standardwert.       |
    |**Type** |Wird nur angezeigt, wenn Sie Ihr AWS-Konto hinzugefügt haben, einschließlich aller Buckets. <br><br>Die aktuellen Optionen umfassen nur **Alle** > **Amazon S3**. Bleiben Sie dran, um über weitere auswählbare Optionen auf dem Laufenden zu bleiben, die mit der sich erweiternden Unterstützungsmatrix von Purview entwickelt werden. |
    |**Credential**     |  Wählen Sie Purview-Anmeldeinformation mit Ihrem Rollen-ARN aus. <br><br>**Tipp**: Wenn Sie zu diesem Zeitpunkt neue Anmeldeinformationen erstellen möchten, wählen Sie **Neu** aus. Weitere Informationen finden Sie unter [Erstellen von Purview-Anmeldeinformationen für Ihre AWS-Bucketüberprüfung](#create-a-purview-credential-for-your-aws-bucket-scan).     |
    |     |         |

    Purview überprüft automatisch, ob der Rollen-ARN gültig ist und ob der Bucket und das Objekt im Bucket zugänglich sind, und fährt dann fort, wenn die Verbindung erfolgreich hergestellt wird.

    > [!TIP]
    > Um andere Werte einzugeben und selbst die Verbindung zu testen, bevor Sie fortfahren, wählen Sie unten rechts **Verbindung testen** aus, bevor Sie **Weiter** auswählen.
    >

1. Wählen Sie im Bereich **Überprüfungsregelsatz auswählen** entweder den Standardregelsatz **AmazonS3** aus, oder wählen Sie **Neuer Überprüfungsregelsatz** aus, um einen neuen benutzerdefinierten Regelsatz zu erstellen. Nachdem Sie Ihren Regelsatz ausgewählt haben, wählen Sie **Weiter** aus.

    Wenn Sie sich dafür entscheiden, einen neuen benutzerdefinierten Überprüfungsregelsatz zu erstellen, verwenden Sie den Assistenten, um die folgenden Einstellungen zu definieren:

    |Bereich  |BESCHREIBUNG  |
    |---------|---------|
    |**Neuer Überprüfungsregelsatz** /<br>**Beschreibung der Überprüfungsregel**    |   Geben Sie einen aussagekräftigen Namen und eine optionale Beschreibung für Ihren Regelsatz ein.      |
    |**Dateitypen auswählen**     | Wählen Sie alle Dateitypen aus, die Sie in die Überprüfung einschließen möchten, und wählen Sie dann **Weiter** aus.<br><br>Um einen neuen Dateityp hinzuzufügen, wählen Sie **Neuer Dateityp** aus, und definieren Sie Folgendes: <br>– Die Dateierweiterung, die Sie hinzufügen möchten. <br>– Eine optionale Beschreibung.  <br>– Ob der Dateiinhalt ein benutzerdefiniertes Trennzeichen besitzt oder ein Systemdateityp ist. Geben Sie dann Ihr benutzerdefiniertes Trennzeichen ein, oder wählen Sie Ihren Systemdateityp aus. <br><br>Wählen Sie **Erstellen** aus, um Ihren benutzerdefinierten Dateityp zu erstellen.     |
    |**Klassifizierungsregeln auswählen**     |   Navigieren Sie zu den Klassifizierungsregeln, die Sie für Ihr Dataset ausführen möchten, und wählen Sie sie aus.      |
    |     |         |

    Wählen Sie **Erstellen** aus, wenn Sie fertig sind, um Ihren Regelsatz zu erstellen.

1. Wählen Sie im Bereich **Überprüfungstrigger festlegen** eine der folgenden Optionen aus, und wählen Sie dann **Weiter** aus:

    - **Serie**, um einen Zeitplan für eine wiederkehrende Überprüfung zu konfigurieren.
    - **Einmalig**, um eine Überprüfung zu konfigurieren, die sofort startet.

1. Überprüfen Sie im Bereich **Überprüfen Ihrer Überprüfung** die Details Ihrer Überprüfung, um zu bestätigen, dass sie korrekt sind, und wählen Sie dann **Speichern** oder **Speichern und ausführen** aus, wenn Sie im vorherigen Bereich **Einmalig** ausgewählt haben.

    > [!NOTE]
    > Sobald die Überprüfung gestartet wurde, kann es bis zu 24 Stunden bis zu deren Abschluss dauern. Sie können 24 Stunden nach dem Start der jeweiligen Überprüfung Ihre **Erkenntnisberichte** überprüfen und den Katalog durchsuchen.
    >

Weitere Informationen finden Sie unter [Erkunden der Ergebnisse der Purview-Überprüfung](#explore-purview-scanning-results).

## <a name="explore-purview-scanning-results"></a>Erkunden der Ergebnisse der Purview-Überprüfung

Wenn eine Purview-Überprüfung Ihrer Amazon S3-Buckets abgeschlossen ist, führen Sie ein Drilldown in den Purview-Bereich **Quellen** aus, um den Überprüfungsverlauf anzuzeigen.

Wählen Sie eine Datenquelle aus, um deren Details anzuzeigen, und wählen Sie dann die Registerkarte **Überprüfungen** aus, um alle zurzeit ausgeführten oder abgeschlossenen Überprüfungen anzuzeigen.
Wenn Sie ein AWS-Konto mit mehreren Buckets hinzugefügt haben, wird der Überprüfungsverlauf für jeden Bucket unter dem Konto angezeigt.

Beispiel:

![Anzeigen der AWS S3-Bucketüberprüfungen unter Ihrer AWS-Kontoquelle.](./media/register-scan-amazon-s3/account-scan-history.png)

Verwenden Sie die anderen Bereiche von Purview, um Details zu den Inhalten in Ihrem Datenbestand zu ermitteln, einschließlich Ihrer Amazon S3-Buckets:

- **Durchsuchen Sie den Purview-Datenkatalog,** und filtern Sie nach einem bestimmten Bucket. Beispiel:

    ![Durchsuchen des Katalogs nach AWS S3-Objekten.](./media/register-scan-amazon-s3/search-catalog-screen-aws.png)

- **Zeigen Sie Erkenntnisberichte an**, um Statistiken für die Klassifizierung, Vertraulichkeitsbezeichnungen, Dateitypen sowie weitere Details zu Ihrem Inhalt anzuzeigen.

    Alle Erkenntnisberichte von Purview enthalten die Amazon S3-Überprüfungsergebnisse, zusammen mit dem Rest der Ergebnisse aus Ihren Azure-Datenquellen. Wenn dies relevant ist, wurde ein zusätzlicher **Amazon S3**-Ressourcentyp zu den Berichtfilteroptionen hinzugefügt.

    Weitere Informationen finden Sie unter [Grundlegendes zu Erkenntnissen in Azure Purview](concept-insights.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Erkenntnisberichten in Azure Purview:

> [!div class="nextstepaction"]
> [Grundlegendes zu Erkenntnissen in Azure Purview](concept-insights.md)
