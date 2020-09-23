---
title: Erweiterte Erkennung von mehrstufigen Angriffen in Azure Sentinel
description: Verwenden Sie die Fusion-Technologie in Azure Sentinel, um die „Alarmmüdigkeit“ zu reduzieren und verwertbare Incidents zu erstellen, die auf der erweiterten Erkennung von mehrstufigen Angriffen basieren.
services: sentinel
documentationcenter: na
author: yelevin
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/30/2020
ms.author: yelevin
ms.openlocfilehash: ba872f221f3bde29f0bb48b04dc2259d3ab4938a
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90906292"
---
# <a name="advanced-multistage-attack-detection-in-azure-sentinel"></a>Erweiterte Erkennung von mehrstufigen Angriffen in Azure Sentinel


> [!IMPORTANT]
> Einige Fusion-Features in Azure Sentinel befinden sich zurzeit in der **Public Preview**.
> Diese Features werden ohne Vereinbarung zum Servicelevel bereitgestellt und nicht für Produktionsworkloads empfohlen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Durch die Verwendung der auf maschinellem Lernen basierenden Fusion-Technologie kann Azure Sentinel mehrstufige Angriffe automatisch erkennen. Hierzu werden Kombinationen von anomalen Verhaltensweisen und verdächtigen Aktivitäten kombiniert, die an verschiedenen Stellen der Kill Chain beobachtet werden. Auf der Grundlage dieser Entdeckungen generiert Azure Sentinel Incidents, die auf andere Weise nur schwer abgefangen werden können. Diese Incidents umfassen mindestens zwei Warnungen oder Aktivitäten. Standardmäßig weisen diese Incidents ein geringes Volumen, eine hohe Qualität und einen hohen Schweregrad auf.

Diese Erkennungstechnologie ist für Ihre Umgebung angepasst und bewirkt nicht nur eine Reduzierung der False Positive-Rate, sondern kann Angriffe auch mit eingeschränkten oder fehlenden Informationen erkennen.

## <a name="configuration-for-advanced-multistage-attack-detection"></a>Konfiguration für die erweiterte Erkennung von mehrstufigen Angriffen

Diese Erkennung ist in Azure Sentinel standardmäßig aktiviert. Zum Überprüfen des Status oder zum Deaktivieren der Erkennung, für den Fall, dass Sie eine andere Lösung zum Erkennen von Incidents auf der Grundlage mehrerer Warnungen verwenden, führen Sie die folgenden Anweisungen aus:

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com)an, falls Sie dies noch nicht getan haben.

1. Navigieren Sie zu **Azure Sentinel** > **Konfiguration** > **Analyse**.

1. Wählen Sie **Aktive Regeln** aus, und suchen Sie dann in der Spalte **NAME** die **Erweiterte mehrstufige Angriffserkennung** aus, indem Sie die Liste nach dem Regeltyp **Fusion** filtern. Überprüfen Sie in der Spalte **STATUS**, ob diese Erkennung aktiviert oder deaktiviert ist.

    :::image type="content" source="./media/fusion/selecting-fusion-rule-type.png" alt-text="{alt-text}":::

1. Wählen Sie zum Ändern des Status diesen Eintrag und auf dem Blatt **Advanced Multistage Attack Detection** (Erweiterte Erkennung von mehrstufigen Angriffen) die Option **Bearbeiten** aus.

1. Auf dem Blatt **Assistent zum Erstellen von Regeln** ist die Statusänderung automatisch für Sie ausgewählt. Wählen Sie **Weiter: Überprüfen** und dann **Speichern** aus. 

 Da der Regeltyp **Fusion** nur eine Regel enthält, die nicht verändert werden kann, sind Regelvorlagen für diesen Regeltyp nicht anwendbar.

> [!NOTE]
> Azure Sentinel verwendet zurzeit 30 Tage an Verlaufsdaten, um die Systeme für das maschinelle Lernen zu trainieren. Diese Daten werden immer mit den Schlüsseln von Microsoft verschlüsselt, wenn sie die Pipeline für maschinelles Lernen durchlaufen. Die Trainingsdaten werden jedoch nicht mit [vom Kunden verwalteten Schlüsseln (Customer Managed Keys, CMK)](customer-managed-keys.md) verschlüsselt, wenn Sie CMK in Ihrem Azure Sentinel-Arbeitsbereich aktiviert haben. Zum Deaktivieren von Fusion navigieren Sie zu **Azure Sentinel** \> **Konfiguration** \> **Analyse \> Aktive Regeln \> Erweiterte mehrstufige Angriffserkennung**, und wählen Sie in der Spalte **Status** die Option **Deaktivieren** aus.

## <a name="attack-detection-scenarios"></a>Szenarien für die Angriffserkennung

Im folgenden Abschnitt werden nach Bedrohungsklassifizierung gruppiert die Typen von Korrelationsszenarien aufgelistet, nach denen Azure Sentinel mithilfe von Fusion-Technologie sucht.

Wie bereits erwähnt, korreliert Fusion mehrere Sicherheitswarnungen aus verschiedenen Produkten, um erweiterte mehrstufige Angriffe zu erkennen. Daher werden erfolgreiche Fusions-Ermittlungen als **Fusion-Vorfälle** auf der Seite **Vorfälle** von Azure Sentinel statt als **Benachrichtigungen** in der Tabelle **Sicherheitswarnungen** in **Protokolle** angezeigt.

Zum Ermöglichen dieser von Fusion gestützten Szenarien zur Angriffsermittlung müssen alle aufgelisteten Datenquellen mithilfe der zugeordneten Azure Sentinel-Daten-Connectors erfasst werden.

> [!NOTE]
> Einige dieser Szenarien befinden sich in der **Public Preview**. Diese sind entsprechend gekennzeichnet.

## <a name="compute-resource-abuse"></a>Missbrauch von Computeressourcen

### <a name="multiple-vm-creation-activities-following-suspicious-azure-active-directory-sign-in"></a>Mehrere Aktivitäten zur Erstellung von VMs im Anschluss an verdächtige Anmeldungen bei Azure Active Directory
Dieses Szenario befindet sich zurzeit in der **Public Preview**.

**MITRE ATT&CK-Taktiken:** Erstzugriff, Beeinträchtigung 

**MITRE ATT&CK-Techniken:** Gültiges Konto (T1078), Ressourcen-Hijacking (T1496)

**Datenconnector-Quellen:** Microsoft Cloud App Security, Azure Active Directory Identity Protection

**Beschreibung:** Fusion-Vorfälle dieses Typs deuten darauf hin, dass virtuelle Computer in ungewöhnlicher Anzahl in einer einzelnen Sitzung im Anschluss an eine verdächtige Anmeldung bei einem Azure AD-Konto erstellt wurden. Diese Art von Benachrichtigung zeigt mit einem hohen Grad an Vertrauenswürdigkeit an, dass das in der Fusion-Vorfallsbeschreibung genannte Konto beschädigt und zum Erstellen neuer VMs für nicht autorisierte Zwecke verwendet wurde, etwa zum Ausführen von Vorgängen zum Kryptografiemining. Die Permutationen verdächtiger Azure AD-Anmeldebenachrichtigungen im Zusammenhang mit der Benachrichtigung über mehrere Aktivitäten zur VM-Erstellung lauten:

- **Unmöglicher Ortswechsel an einen atypischen Speicherort führt zu mehreren Aktivitäten zur VM-Erstellung**

- **Anmeldeereignis von einem unbekannten Ort führt zu mehreren Aktivitäten zur VM-Erstellung**

- **Anmeldeereignis von einem infizierten Gerät führt zu mehreren Aktivitäten zur VM-Erstellung**

- **Anmeldeereignis von einer anonymen IP-Adresse führt zu mehreren Aktivitäten zur VM-Erstellung**

- **Anmeldeereignis eines Benutzers mit kompromittierten Anmeldeinformationen führt zu mehreren Aktivitäten zur VM-Erstellung**

## <a name="data-exfiltration"></a>Datenexfiltration

### <a name="office-365-mailbox-exfiltration-following-a-suspicious-azure-ad-sign-in"></a>Office 365-Postfachexfiltration im Anschluss an eine verdächtige Azure AD-Anmeldung

**MITRE ATT&CK-Taktiken:** Erstzugriff, Exfiltration, Sammlung

**MITRE ATT&CK-Techniken:** Gültiges Konto (T1078), E-Mail-Sammlung (T1114), Automatisierte Exfiltration (T1020)

**Datenconnector-Quellen:** Microsoft Cloud App Security, Azure Active Directory Identity Protection

**Beschreibung:** Fusion-Vorfälle dieses Typs deuten darauf hin, dass eine verdächtige Posteingangs-Weiterleitungsregel für den Posteingang eines Benutzers im Anschluss an eine verdächtige Anmeldung bei einem Azure AD-Konto festgelegt wurde. Diese Indikation bietet hohe Vertrauenswürdigkeit für die Annahme, dass das Konto des Benutzers (das in der Fusion-Vorfallsbeschreibung angegeben ist) kompromittiert und zur Exfiltration von Daten aus dem Netzwerk Ihrer Organisation verwendet wurde, indem ohne Wissen des wahren Benutzers eine Weiterleitungsregel für ein Postfach aktiviert wurde. Die Permutationen verdächtiger Azure AD-Anmeldebenachrichtigungen im Zusammenhang mit der Benachrichtigung zur Office 365-Postfachexfiltration sind:

- **Unmöglicher Ortswechsel zu einem atypischen Ort, der zu einer Exfiltration des Office 365-Postfachs führt**

- **Anmeldeereignis von einem unbekannten Ort, das zu einer Exfiltration des Office 365-Postfachs führt**

- **Anmeldeereignis mit einem infizierten Gerät, das zu einer Exfiltration des Office 365-Postfachs führt**

- **Anmeldeereignis von einer anonymen IP-Adresse, das zu einer Exfiltration des Office 365-Postfachs führt**

- **Anmeldeereignis eines Benutzers mit kompromittierten Anmeldeinformationen, das zur Exfiltration des Office 365-Postfachs führt**

### <a name="mass-file-download-following-suspicious-azure-ad-sign-in"></a>Massendownload von Dateien im Anschluss an eine verdächtige Azure AD-Anmeldung

**MITRE ATT&CK-Taktiken:** Erstzugriff, Exfiltration

**MITRE ATT&CK-Techniken:** Gültiges Konto (T1078)

**Datenconnector-Quellen:** Microsoft Cloud App Security, Azure Active Directory Identity Protection

**Beschreibung:** Fusion-Vorfälle dieses Typs deuten darauf hin, dass Dateien in anomaler Anzahl von einem Benutzer im Anschluss an eine verdächtige Anmeldung bei einem Azure AD-Konto heruntergeladen wurden. Diese Indikation bietet hohe Vertrauenswürdigkeit für die Annahme, dass das in der Fusion-Vorfallsbeschreibung angegebene Konto kompromittiert und zur Exfiltration von Daten aus dem Netzwerk Ihrer Organisation verwendet wurde. Die Permutationen verdächtiger Azure AD-Anmeldebenachrichtigungen im Zusammenhang mit der Benachrichtigung zum Massendownload von Dateien sind:  

- **Unmöglicher Ortswechsel zu einem atypischen Ort mit Massendownload von Dateien**

- **Anmeldeereignis von einem unbekannten Ort, das zum Massendownload von Dateien führt**

- **Anmeldeereignis mit einem infizierten Gerät, das zum Massendownload von Dateien führt**

- **Anmeldeereignis von einer anonymen IP-Adresse, das zum Massendownload von Dateien führt**

- **Anmeldeereignis eines Benutzers mit kompromittierten Anmeldeinformationen, das zum Massendownload von Dateien führt**

### <a name="mass-file-sharing-following-suspicious-azure-ad-sign-in"></a>Massenfreigabe von Dateien im Anschluss an eine verdächtige Azure AD-Anmeldung

**MITRE ATT&CK-Taktiken:** Erstzugriff, Exfiltration

**MITRE ATT&CK-Techniken:** Gültiges Konto (T1078), Exfiltration über einen Webdienst (T1567)

**Datenconnector-Quellen:** Microsoft Cloud App Security, Azure Active Directory Identity Protection

**Beschreibung:** Fusion-Vorfälle dieses Typs deuten darauf hin, dass eine Anzahl von Dateien oberhalb eines bestimmten Schwellenwerts im Anschluss an eine verdächtige Anmeldung bei einem Azure AD-Konto für andere freigegeben wurden. Diese Indikation bietet hohe Vertrauenswürdigkeit für die Annahme, dass das in der Fusion-Vorfallsbeschreibung angegebene Konto kompromittiert und zur Exfiltration von Daten aus dem Netzwerk Ihrer Organisation verwendet wurde, indem Dateien wie Dokumente, Kalkulationstabellen usw. mit böswilliger Absicht für unberechtigte Benutzer freigegeben wurden. Die Permutationen verdächtiger Azure AD-Anmeldebenachrichtigungen im Zusammenhang mit der Benachrichtigung zur Massenfreigabe von Dateien sind:  

- **Unmöglicher Ortswechsel zu einem atypischen Ort mit Massenfreigabe von Dateien**

- **Anmeldeereignis von einem unbekannten Ort, das zur Massenfreigabe von Dateien führt**

- **Anmeldeereignis mit einem infizierten Gerät, das zur Massenfreigabe von Dateien führt**

- **Anmeldeereignis von einer anonymen IP-Adresse, das zur Massenfreigabe von Dateien führt**

- **Anmeldeereignis eines Benutzers mit kompromittierten Anmeldeinformationen, das zur Massenfreigabe von Dateien führt**

### <a name="suspicious-inbox-manipulation-rules-set-following-suspicious-azure-ad-sign-in"></a>Verdächtige Regeln zur Posteingangsänderung im Anschluss an verdächtige Azure AD-Anmeldung festgelegt
Dieses Szenario ist zwei Klassifikationen in dieser Liste zuzuordnen: **Datenexfiltration** und **Lateral Movement**. Aus Gründen der Klarheit wird es in beiden Abschnitten aufgeführt.

Dieses Szenario befindet sich zurzeit in der **Public Preview**.

**MITRE ATT&CK-Taktiken:** Erstzugriff, Lateral Movement, Exfiltration

**MITRE ATT&CK-Techniken:** Gültiges Konto (T1078), internes Spear-Phishing (T1534)

**Datenconnector-Quellen:** Microsoft Cloud App Security, Azure Active Directory Identity Protection

**Beschreibung:** Fusion-Vorfälle dieses Typs deuten darauf hin, dass anomale Posteingangs-Weiterleitungsregeln für den Posteingang eines Benutzers im Anschluss an eine verdächtige Anmeldung bei einem Azure AD-Konto festgelegt wurden. Dies gibt einen sehr vertrauenswürdigen Hinweis darauf, dass das in der Fusion-Vorfallsbeschreibung genannte Konto kompromittiert und zu böswilligen Zwecken zur Manipulation der Regeln des Benutzerposteingangs verwendet wurde. Dies kann ein möglicher Versuch eines Angreifers sein, Daten aus dem Netzwerk der Organisation zu exfiltrieren. Alternativ könnte der Angreifer versuchen, Phishing-E-Mails aus dem Innern der Organisation zu erstellen (durch Umgehen der Mechanismen zur Erkennung von Phishing, die auf E-Mails aus externen Quellen gerichtet sind), um ihm durch Erwerb des Zugriffs auf weitere Benutzerkonten und/oder bevorrechtigte Konten eine Seitwärtsbewegung zu ermöglichen. Die Permutationen verdächtiger Azure AD-Anmeldebenachrichtigungen im Zusammenhang mit der Benachrichtigung über verdächtige Regeln zur Postfachänderung lauten:  

- **Unmöglicher Ortswechsel zu einem atypischen Ort, der zu einer verdächtigen Regel zur Posteingangs-Änderung führt**

- **Anmeldeereignis von einem unbekannten Ort, das zu verdächtiger Regel zur Posteingangsänderung führt**

- **Anmeldeereignis von einem infizierten Gerät, das zu verdächtiger Regel zur Posteingangsänderung führt**

- **Anmeldeereignis von einer anonymen IP-Adresse, das zu verdächtiger Regel zur Posteingangsänderung führt**

- **Anmeldeereignis eines Benutzers mit kompromittierten Anmeldeinformationen, das zu verdächtiger Regel zur Posteingangsänderung führt**

### <a name="multiple-power-bi-report-sharing-activities-following-suspicious-azure-ad-sign-in"></a>Mehrere Aktivitäten zur Freigabe von Power BI-Berichten nach verdächtiger Azure AD Anmeldung 
Dieses Szenario befindet sich zurzeit in der **Public Preview**.

**MITRE ATT&CK-Taktiken:** Erstzugriff, Exfiltration 

**MITRE ATT&CK-Techniken:** Gültiges Konto (T1078), Exfiltration über einen Webdienst (T1567)

**Datenconnector-Quellen:** Microsoft Cloud App Security, Azure Active Directory Identity Protection

**Beschreibung:** Fusion-Vorfälle dieses Typs deuten darauf hin, dass Power BI-Berichte in ungewöhnlicher Anzahl in einer einzelnen Sitzung im Anschluss an eine verdächtige Anmeldung bei einem Azure AD-Konto freigegeben wurden. Diese Indikation bietet hohe Vertrauenswürdigkeit für die Annahme, dass das in der Fusion-Vorfallsbeschreibung angegebene Konto kompromittiert und zur Exfiltration von Daten aus dem Netzwerk Ihrer Organisation verwendet wurde, indem Power BI-Berichte in böswilliger Absicht für unberechtigte Benutzer freigegeben wurden. Die Permutationen verdächtiger Azure AD-Anmeldebenachrichtigungen im Zusammenhang mit der Benachrichtigung über mehrere Aktivitäten zur Freigabe von Power BI-Berichten lauten:  

- **Unmöglicher Ortswechsel an einen atypischen Speicherort führt zu mehreren Aktivitäten zur Freigabe von Power BI-Berichten**

- **Anmeldeereignis von einem unbekannten Ort führt zu mehreren Aktivitäten zur Freigabe von Power BI-Berichten**

- **Anmeldeereignis von einem infizierten Gerät führt zu mehreren Aktivitäten zur Freigabe von Power BI-Berichten**

- **Anmeldeereignis von einer anonymen IP-Adresse führt zu mehreren Aktivitäten zur Freigabe von Power BI-Berichten**

- **Anmeldeereignis eines Benutzers mit kompromittierten Anmeldeinformationen führt zu mehreren Aktivitäten zur Freigabe von Power BI-Berichten**

### <a name="suspicious-power-bi-report-sharing-following-suspicious-azure-ad-sign-in"></a>Verdächtige Freigabe von Power BI-Berichten im Anschluss an verdächtige Azure AD-Anmeldung
Dieses Szenario befindet sich zurzeit in der **Public Preview**.

**MITRE ATT&CK-Taktiken:** Erstzugriff, Exfiltration 

**MITRE ATT&CK-Techniken:** Gültiges Konto (T1078), Exfiltration über einen Webdienst (T1567)

**Datenconnector-Quellen:** Microsoft Cloud App Security, Azure Active Directory Identity Protection

**Beschreibung:** Fusion-Vorfälle dieses Typs deuten darauf hin, dass eine verdächtige Aktivität zur Freigabe von Power BI-Berichten im Anschluss an eine verdächtige Anmeldung bei einem Azure AD-Konto auftrat. Die Freigabeaktivität wurde als verdächtig identifiziert, weil der Power BI-Bericht vertrauliche Informationen enthielt, wie mithilfe der natürlichsprachlichen Verarbeitung festgestellt wurde, und weil er mit einer externen E-Mail-Adresse geteilt, im Web veröffentlicht oder als Momentaufnahme an eine extern abonnierte E-Mail-Adresse übermittelt wurde. Diese Benachrichtigung deutet mit hoher Vertrauenswürdigkeit darauf hin, dass das in der Fusion-Vorfallsbeschreibung angegebene Konto kompromittiert und zur Exfiltration vertraulicher Daten aus Ihrer Organisation verwendet wurde, indem Power BI-Berichte in böswilliger Absicht für unberechtigte Benutzer freigegeben wurden. Die Permutationen verdächtiger Azure AD-Anmeldebenachrichtigungen im Zusammenhang mit der Benachrichtigung über die verdächtige Freigabe von Power BI-Berichten lauten:  

- **Unmöglicher Ortswechsel an einen atypischen Speicherort führt zu verdächtiger Freigabe von Power BI-Berichten**

- **Anmeldeereignis von einem unbekannten Ort führt zu verdächtiger Freigabe von Power BI-Berichten**

- **Anmeldeereignis von einem infizierten Gerät führt zu verdächtiger Freigabe von Power BI-Berichten**

- **Anmeldeereignis von einer anonymen IP-Adresse führt zu verdächtiger Freigabe von Power BI-Berichten**

- **Anmeldeereignis eines Benutzers mit kompromittierten Anmeldeinformationen führt zu verdächtiger Freigabe von Power BI-Berichten**

## <a name="data-destruction"></a>Datenvernichtung

### <a name="mass-file-deletion-following-suspicious-azure-ad-sign-in"></a>Massenlöschung von Dateien im Anschluss an eine verdächtige Azure AD-Anmeldung

**MITRE ATT&CK-Taktiken:** Erstzugriff, Beeinträchtigung

**MITRE ATT&CK-Techniken:** Gültiges Konto (T1078), Datenvernichtung (T1485)

**Datenconnector-Quellen:** Microsoft Cloud App Security, Azure Active Directory Identity Protection

**Beschreibung:** Fusion-Vorfälle dieses Typs deuten darauf hin, dass einzigartige Dateien in anomaler Anzahl von einem Benutzer im Anschluss an eine verdächtige Anmeldung bei einem Azure AD-Konto gelöscht wurden. Dies gibt einen Hinweis darauf, dass das in der Fusion-Vorfallsbeschreibung genannte Konto möglicherweise kompromittiert und zu böswilligen Zwecken zur Vernichtung von Daten verwendet wurde. Die Permutationen verdächtiger Azure AD-Anmeldebenachrichtigungen im Zusammenhang mit der Benachrichtigung zur Massenlöschung von Dateien sind:  

- **Unmöglicher Ortswechsel zu einem atypischen Ort, der zur Massenlöschung von Dateien führt**

- **Anmeldeereignis von einem unbekannten Ort, das zur Massenlöschung von Dateien führt**

- **Anmeldeereignis mit einem infizierten Gerät, das zur Massenlöschung von Dateien führt**

- **Anmeldeereignis von einer anonymen IP-Adresse, das zur Massenlöschung von Dateien führt**

- **Anmeldeereignis eines Benutzers mit kompromittierten Anmeldeinformationen, das zur Massenlöschung von Dateien führt**

### <a name="suspicious-email-deletion-activity-following-suspicious-azure-ad-sign-in"></a>Verdächtige Aktivität zur E-Mail-Löschung im Anschluss an eine verdächtige Azure AD-Anmeldung
Dieses Szenario befindet sich zurzeit in der **Public Preview**.

**MITRE ATT&CK-Taktiken:** Erstzugriff, Beeinträchtigung 

**MITRE ATT&CK-Techniken:** Gültiges Konto (T1078), Datenvernichtung (T1485)

**Datenconnector-Quellen:** Microsoft Cloud App Security, Azure Active Directory Identity Protection

**Beschreibung:** Fusion-Vorfälle dieses Typs deuten darauf hin, dass E-Mails in ungewöhnlicher Anzahl in einer einzelnen Sitzung im Anschluss an eine verdächtige Anmeldung bei einem Azure AD-Konto gelöscht wurden. Dies gibt einen Hinweis darauf, dass das in der Fusion-Vorfallsbeschreibung genannte Konto möglicherweise kompromittiert und zu böswilligen Zwecken zur Vernichtung von Daten verwendet wurde, etwa um die Organisation zu schädigen oder E-Mail-Aktivitäten im Zusammenhang mit Spam zu verbergen. Die Permutationen verdächtiger Azure AD-Anmeldebenachrichtigungen im Zusammenhang mit der Benachrichtigung über verdächtige Aktivitäten zur E-Mail-Löschung lauten:   

- **Unmöglicher Ortswechsel an einen atypischen Speicherort führt zu verdächtiger Aktivität zur E-Mail-Löschung**

- **Anmeldeereignis von einem unbekannten Ort führt zu verdächtiger Aktivität zur E-Mail-Löschung**

- **Anmeldeereignis von einem infizierten Gerät führt zu verdächtiger Aktivität zur E-Mail-Löschung**

- **Anmeldeereignis von einer anonymen IP-Adresse führt zu verdächtiger Aktivität zur E-Mail-Löschung**

- **Anmeldeereignis eines Benutzers mit kompromittierten Anmeldeinformationen führt zu verdächtiger Aktivität zur E-Mail-Löschung**

## <a name="denial-of-service"></a>Denial of Service

### <a name="multiple-vm-delete-activities-following-suspicious-azure-ad-sign-in"></a>Mehrere Aktivitäten zur Löschung von VMs nach verdächtiger Azure AD Anmeldung
Dieses Szenario befindet sich zurzeit in der **Public Preview**.

**MITRE ATT&CK-Taktiken:** Erstzugriff, Beeinträchtigung

**MITRE ATT&CK-Techniken:** Gültiges Konto (T1078), Denial-of-Service am Endpunkt (T1499)

**Datenconnector-Quellen:** Microsoft Cloud App Security, Azure Active Directory Identity Protection

**Beschreibung:** Fusion-Vorfälle dieses Typs deuten darauf hin, dass virtuelle Computer in ungewöhnlicher Anzahl in einer einzelnen Sitzung im Anschluss an eine verdächtige Anmeldung bei einem Azure AD-Konto gelöscht wurden. Diese Indikation bietet hohe Vertrauenswürdigkeit für die Annahme, dass das in der Fusion-Vorfallsbeschreibung genannte Konto kompromittiert und zum Versuch der Betriebsunterbrechung oder Vernichtung der Cloudumgebung der Organisation verwendet wurde. Die Permutationen verdächtiger Azure AD-Anmeldebenachrichtigungen im Zusammenhang mit der Benachrichtigung über mehrere Aktivitäten zur VM-Löschung lauten:  

- **Unmöglicher Ortswechsel an einen atypischen Speicherort führt zu mehreren Aktivitäten zur VM-Löschung**

- **Anmeldeereignis von einem unbekannten Ort führt zu mehreren Aktivitäten zur VM-Löschung**

- **Anmeldeereignis von einem infizierten Gerät führt zu mehreren Aktivitäten zur VM-Löschung**

- **Anmeldeereignis von einer anonymen IP-Adresse führt zu mehreren Aktivitäten zur VM-Löschung**

- **Anmeldeereignis eines Benutzers mit kompromittierten Anmeldeinformationen führt zu mehreren Aktivitäten zur VM-Löschung**

## <a name="lateral-movement"></a>Seitwärtsbewegung

### <a name="office-365-impersonation-following-suspicious-azure-ad-sign-in"></a>Office 365-Identitätswechsel im Anschluss an eine verdächtige Azure AD-Anmeldung

**MITRE ATT&CK-Taktiken:** Erstzugriff, Lateral Movement

**MITRE ATT&CK-Techniken:** Gültiges Konto (T1078), internes Spear-Phishing (T1534)

**Datenconnector-Quellen:** Microsoft Cloud App Security, Azure Active Directory Identity Protection

**Beschreibung:** Fusion-Vorfälle dieses Typs deuten darauf hin, dass Identitätswechselaktionen in anomaler Anzahl im Anschluss an eine verdächtige Anmeldung bei einem Azure AD-Konto aufgetreten sind. Einige Softwareanwendungen bieten Optionen, mit denen Benutzer die Identität anderer Benutzer annehmen können. Beispielsweise geben E-Mail-Dienste ihren Benutzern die Möglichkeit, andere Benutzer zum Senden von E-Mails in ihrem Auftrag zu ermächtigen. Diese Benachrichtigung gibt einen ziemlich vertrauenswürdigen Hinweis darauf, dass das in der Fusion-Vorfallsbeschreibung genannte Konto kompromittiert und dazu verwendet wurde, Identitätswechselaktivitäten zu böswilligen Zwecken durchzuführen, etwa um Phishing-E-Mails zur Verteilung von Malware oder zur Seitwärtsbewegung zu senden. Die Permutationen verdächtiger Azure AD-Anmeldebenachrichtigungen im Zusammenhang mit der Benachrichtigung zum Office 365-Identitätswechsel sind:  

- **Unmöglicher Ortswechsel zu einem atypischen Ort, der zu einem Office 365-Identitätswechsel führt**

- **Anmeldeereignis von einem unbekannten Ort, das zu einem Office 365-Identitätswechsel führt**

- **Anmeldeereignis mit einem infizierten Gerät, das zu einem Office 365-Identitätswechsel führt**

- **Anmeldeereignis von einer anonymen IP-Adresse, das zu einem Office 365-Identitätswechsel führt**

- **Anmeldeereignis eines Benutzers mit kompromittierten Anmeldeinformationen, das zu einem Office 365-Identitätswechsel führt**
 
### <a name="suspicious-inbox-manipulation-rules-set-following-suspicious-azure-ad-sign-in"></a>Verdächtige Regeln zur Posteingangsänderung im Anschluss an verdächtige Azure AD-Anmeldung festgelegt
Dieses Szenario ist zwei Klassifikationen in dieser Liste zuzuordnen: **Seitwärtsbewegung** und **Datenexfiltration**. Aus Gründen der Klarheit wird es in beiden Abschnitten aufgeführt.

Dieses Szenario befindet sich zurzeit in der **Public Preview**.

**MITRE ATT&CK-Taktiken:** Erstzugriff, Lateral Movement, Exfiltration

**MITRE ATT&CK-Techniken:** Gültiges Konto (T1078), internes Spear-Phishing (T1534), Automatisierte Exfiltration (T1020)

**Datenconnector-Quellen:** Microsoft Cloud App Security, Azure Active Directory Identity Protection

**Beschreibung:** Fusion-Vorfälle dieses Typs deuten darauf hin, dass anomale Posteingangs-Weiterleitungsregeln für den Posteingang eines Benutzers im Anschluss an eine verdächtige Anmeldung bei einem Azure AD-Konto festgelegt wurden. Dies gibt einen sehr vertrauenswürdigen Hinweis darauf, dass das in der Fusion-Vorfallsbeschreibung genannte Konto kompromittiert und zu böswilligen Zwecken zur Manipulation der Regeln des Benutzerposteingangs verwendet wurde. Dies kann ein möglicher Versuch eines Angreifers sein, Daten aus dem Netzwerk der Organisation zu exfiltrieren. Alternativ könnte der Angreifer versuchen, Phishing-E-Mails aus dem Innern der Organisation zu erstellen (durch Umgehen der Mechanismen zur Erkennung von Phishing, die auf E-Mails aus externen Quellen gerichtet sind), um ihm durch Erwerb des Zugriffs auf weitere Benutzerkonten und/oder bevorrechtigte Konten eine Seitwärtsbewegung zu ermöglichen. Die Permutationen verdächtiger Azure AD-Anmeldebenachrichtigungen im Zusammenhang mit der Benachrichtigung über verdächtige Regeln zur Postfachänderung lauten:

- **Unmöglicher Ortswechsel zu einem atypischen Ort, der zu einer verdächtigen Regel zur Posteingangs-Änderung führt**

- **Anmeldeereignis von einem unbekannten Ort, das zu verdächtiger Regel zur Posteingangsänderung führt**

- **Anmeldeereignis von einem infizierten Gerät, das zu verdächtiger Regel zur Posteingangsänderung führt**

- **Anmeldeereignis von einer anonymen IP-Adresse, das zu verdächtiger Regel zur Posteingangsänderung führt**

- **Anmeldeereignis eines Benutzers mit kompromittierten Anmeldeinformationen, das zu verdächtiger Regel zur Posteingangsänderung führt**

## <a name="malicious-administrative-activity"></a>Böswillige administrative Aktivität

### <a name="suspicious-cloud-app-administrative-activity-following-suspicious-azure-ad-sign-in"></a>Verdächtige administrative Aktivität in der Cloud-App im Anschluss an eine verdächtige Azure AD-Anmeldung

**MITRE ATT&CK-Taktiken:** Erstzugriff, Persistenz, Umgehung von Verteidigungsmaßnahmen, Lateral Movement, Sammlung, Exfiltration und Beeinträchtigung

**MITRE ATT&CK-Techniken:** –

**Datenconnector-Quellen:** Microsoft Cloud App Security, Azure Active Directory Identity Protection

**Beschreibung:** Fusion-Vorfälle dieses Typs deuten darauf hin, dass administrative Aktivitäten in anomaler Anzahl in einer einzelnen Sitzung im Anschluss an eine verdächtige Azure AD-Anmeldung des gleichen Kontos durchgeführt wurden. Dies gibt einen Hinweis darauf, dass das in der Fusion-Vorfallsbeschreibung genannte Konto möglicherweise kompromittiert und in böswilliger Absicht zum Ausführen einer beliebigen Anzahl nicht autorisierter administrativer Aktionen verwendet wurde. Dies ist zugleich ein Hinweis darauf, dass möglicherweise ein Konto mit Administratorberechtigungen kompromittiert wurde. Die Permutationen verdächtiger Azure AD-Anmeldebenachrichtigungen im Zusammenhang mit der Benachrichtigung über verdächtige administrative Aktivitäten in der Cloud-App lauten:  

- **Unmöglicher Ortswechsel zu einem typischen Ort, der zu verdächtiger Verwaltungsaktivität für die Cloud-App führt**

- **Anmeldeereignis von einem unbekannten Ort, das zu verdächtiger Verwaltungsaktivität für die Cloud-App führt**

- **Anmeldeereignis mit einem infizierten Gerät, das zu verdächtiger Verwaltungsaktivität für die Cloud-App führt**

- **Anmeldeereignis von einer anonymen IP-Adresse, das zu verdächtiger Verwaltungsaktivität für die Cloud-App führt**

- **Anmeldeereignis eines Benutzers mit kompromittierten Anmeldeinformationen, das zu verdächtiger Verwaltungsaktivität für die Cloud-App führt**

## <a name="malicious-execution-with-legitimate-process"></a>Böswillige Ausführung mit legitimem Prozess

### <a name="powershell-made-a-suspicious-network-connection-followed-by-anomalous-traffic-flagged-by-palo-alto-networks-firewall"></a>PowerShell hat eine verdächtige Netzwerkverbindung hergestellt, auf die anomaler, durch die Palo Alto Networks-Firewall gekennzeichneter Datenverkehr folgte.
Dieses Szenario befindet sich zurzeit in der **Public Preview**.

**MITRE ATT&CK-Taktiken:** Ausführung

**MITRE ATT&CK-Techniken:** Befehls- und Skript-Interpreter (T1059)

**Datenconnector-Quellen:** Microsoft Defender for Endpoint (früher Microsoft Defender Advanced Threat Protection oder MDATP), Palo Alto Networks 

**Beschreibung:** Fusion-Vorfälle dieses Typs zeigen an, dass eine ausgehende Verbindungsanforderung mithilfe eines PowerShell-Befehls durchgeführt wurde. Im Anschluss daran wurde von der Palo Alto Networks-Firewall eine ungewöhnliche eingehende Aktivität erkannt. Dies bietet einen Hinweis darauf, dass ein Angreifer wahrscheinlich Zugriff auf Ihr Netzwerk erlangt hat und versucht, böswillige Aktionen auszuführen. Verbindungsversuche von PowerShell, die diesem Muster folgen, können einen Hinweis auf Command-and-Control-Aktivitäten von Malware, Anforderungen für den Download zusätzlicher Malware oder einen Angreifer darstellen, der interaktiven Remotezugriff einrichtet. Wie bei allen Living-off-the-Land-Angriffen könnte diese Aktivität auch eine legitime Verwendung von PowerShell darstellen. Die Ausführung eines PowerShell-Befehls, auf die verdächtige eingehende Firewall-Aktivitäten folgen, steigert aber die Glaubwürdigkeit der Annahme, dass PowerShell in böswilliger Weise verwendet wird und der Vorfall genauer untersucht werden sollte. In Palo Alto-Protokollen sucht Azure Sentinel hauptsächlich nach [Bedrohungen](https://docs.paloaltonetworks.com/pan-os/8-1/pan-os-admin/monitoring/view-and-manage-logs/log-types-and-severity-levels/threat-logs), und der Datenverkehr wird als verdächtig eingestuft, wenn Bedrohungen durchgelassen werden (verdächtige Daten, Dateien, Überflutungen, Pakete, Scans, Spyware, URLs, Viren, Sicherheitsrisiken, Wildfireviren, Wildfires). Konsultieren Sie außerdem das Palo Alto-Bedrohungsprotokoll, das dem in der Fusion-Vorfallsbeschreibung aufgelisteten [Bedrohungs-/Inhaltstyp](https://docs.paloaltonetworks.com/pan-os/8-1/pan-os-admin/monitoring/use-syslog-for-monitoring/syslog-field-descriptions/threat-log-fields.html) entspricht, um weitere Details zur Benachrichtigung zu erhalten.

### <a name="suspicious-remote-wmi-execution-followed-by-anomalous-traffic-flagged-by-palo-alto-networks-firewall"></a>Verdächtige WMI-Remoteausführung, auf die anomaler, durch die Palo Alto Networks-Firewall gekennzeichneter Datenverkehr folgte.
Dieses Szenario befindet sich zurzeit in der **Public Preview**.

**MITRE ATT&CK-Taktiken:** Ausführung, Ermittlung

**MITRE ATT&CK-Techniken:** Windows-Verwaltungsinstrumentation (T1047)

**Datenconnector-Quellen:** Microsoft Defender für den Endpunkt (vormals MDATP), Palo Alto Networks 

**Beschreibung:** Fusion-Vorfälle dieses Typs deuten darauf hin, dass WMI-Befehle (Windows Management Interface) remote auf einem System ausgeführt wurden. Im Anschluss daran wurden von der Palo Alto Networks-Firewall verdächtige eingehende Aktivitäten erkannt. Dies gibt einen Hinweis darauf, dass ein Angreifer möglicherweise Zugriff auf Ihr Netzwerk erhalten hat und Seitwärtsbewegung, das Heraufstufen von Berechtigungen und/oder das Ausführen bösartiger Nutzlasten versucht. Wie bei allen Living-off-the-Land-Angriffen könnte diese Aktivität auch eine legitime Verwendung von WMI darstellen. Die Remoteausführung eines WMI-Befehls, auf die verdächtige eingehende Firewall-Aktivitäten folgen, steigert aber die Glaubwürdigkeit der Annahme, dass WMI in böswilliger Weise verwendet wird und der Vorfall genauer untersucht werden sollte. In Palo Alto-Protokollen sucht Azure Sentinel hauptsächlich nach [Bedrohungen](https://docs.paloaltonetworks.com/pan-os/8-1/pan-os-admin/monitoring/view-and-manage-logs/log-types-and-severity-levels/threat-logs), und der Datenverkehr wird als verdächtig eingestuft, wenn Bedrohungen durchgelassen werden (verdächtige Daten, Dateien, Überflutungen, Pakete, Scans, Spyware, URLs, Viren, Sicherheitsrisiken, Wildfireviren, Wildfires). Konsultieren Sie außerdem das Palo Alto-Bedrohungsprotokoll, das dem in der Fusion-Vorfallsbeschreibung aufgelisteten [Bedrohungs-/Inhaltstyp](https://docs.paloaltonetworks.com/pan-os/8-1/pan-os-admin/monitoring/use-syslog-for-monitoring/syslog-field-descriptions/threat-log-fields.html) entspricht, um weitere Details zur Benachrichtigung zu erhalten.

## <a name="malware-c2-or-download"></a>Malware C2 oder Download

### <a name="network-request-to-tor-anonymization-service-followed-by-anomalous-traffic-flagged-by-palo-alto-networks-firewall"></a>Netzwerkanforderung an den TOR-Anonymisierungsdienst gefolgt von anomalem, durch die Palo Alto Networks-Firewall gekennzeichnetem Datenverkehr
Dieses Szenario befindet sich zurzeit in der **Public Preview**.

**MITRE ATT&CK-Taktiken:** Befehl und Steuerung

**MITRE ATT&CK-Techniken:** Verschlüsselter Kanal (T1573), Proxy (T1090)

**Datenconnector-Quellen:** Microsoft Defender für den Endpunkt (vormals MDATP), Palo Alto Networks 

**Beschreibung:** Fusion-Vorfälle dieses Typs zeigen an, dass eine ausgehende Verbindungsanforderung an den TOR-Anonymisierungsdienst durchgeführt wurde. Im Anschluss daran wurde dann von der Palo Alto Networks-Firewall eine ungewöhnliche eingehende Aktivität erkannt. Dies bietet einen Hinweis darauf, dass ein Angreifer wahrscheinlich Zugriff auf Ihr Netzwerk erlangt hat und versucht, seine Aktionen und Absichten zu verbergen. Verbindungsversuche mit dem TOR-Netzwerk, die diesem Muster folgen, können einen Hinweis auf Command-and-Control-Aktivitäten von Malware, Anforderungen für den Download zusätzlicher Malware oder einen Angreifer darstellen, der interaktiven Remotezugriff einrichtet. In Palo Alto-Protokollen sucht Azure Sentinel hauptsächlich nach [Bedrohungen](https://docs.paloaltonetworks.com/pan-os/8-1/pan-os-admin/monitoring/view-and-manage-logs/log-types-and-severity-levels/threat-logs), und der Datenverkehr wird als verdächtig eingestuft, wenn Bedrohungen durchgelassen werden (verdächtige Daten, Dateien, Überflutungen, Pakete, Scans, Spyware, URLs, Viren, Sicherheitsrisiken, Wildfireviren, Wildfires). Konsultieren Sie außerdem das Palo Alto-Bedrohungsprotokoll, das dem in der Fusion-Vorfallsbeschreibung aufgelisteten [Bedrohungs-/Inhaltstyp](https://docs.paloaltonetworks.com/pan-os/8-1/pan-os-admin/monitoring/use-syslog-for-monitoring/syslog-field-descriptions/threat-log-fields.html) entspricht, um weitere Details zur Benachrichtigung zu erhalten.

### <a name="outbound-connection-to-ip-with-a-history-of-unauthorized-access-attempts-followed-by-anomalous-traffic-flagged-by-palo-alto-networks-firewall"></a>Ausgehende Verbindung mit IP mit einem Verlauf nicht autorisierter Zugriffsversuche, gefolgt von anomalem, durch die Palo Alto Networks-Firewall gekennzeichnetem Datenverkehr
Dieses Szenario befindet sich zurzeit in der **Public Preview**.

**MITRE ATT&CK-Taktiken:** Befehl und Steuerung

**MITRE ATT&CK-Techniken:** Nicht verfügbar

**Datenconnector-Quellen:** Microsoft Defender für den Endpunkt (vormals MDATP), Palo Alto Networks 

**Beschreibung:** Fusion-Vorfälle dieses Typs zeigen an, dass eine ausgehende Verbindung mit einer IP-Adresse mit einem Backlog unberechtigter Zugriffsversuche hergestellt wurde. Im Anschluss daran wurde von der Palo Alto Networks-Firewall eine ungewöhnliche Aktivität erkannt. Dies bietet einen Hinweis darauf, dass ein Angreifer wahrscheinlich Zugriff auf Ihr Netzwerk erlangt hat. Verbindungsversuche, die diesem Muster folgen, können einen Hinweis auf Command-and-Control-Aktivitäten von Malware, Anforderungen für den Download zusätzlicher Malware oder einen Angreifer darstellen, der interaktiven Remotezugriff einrichtet. In Palo Alto-Protokollen sucht Azure Sentinel hauptsächlich nach [Bedrohungen](https://docs.paloaltonetworks.com/pan-os/8-1/pan-os-admin/monitoring/view-and-manage-logs/log-types-and-severity-levels/threat-logs), und der Datenverkehr wird als verdächtig eingestuft, wenn Bedrohungen durchgelassen werden (verdächtige Daten, Dateien, Überflutungen, Pakete, Scans, Spyware, URLs, Viren, Sicherheitsrisiken, Wildfireviren, Wildfires). Konsultieren Sie außerdem das Palo Alto-Bedrohungsprotokoll, das dem in der Fusion-Vorfallsbeschreibung aufgelisteten [Bedrohungs-/Inhaltstyp](https://docs.paloaltonetworks.com/pan-os/8-1/pan-os-admin/monitoring/use-syslog-for-monitoring/syslog-field-descriptions/threat-log-fields.html) entspricht, um weitere Details zur Benachrichtigung zu erhalten.

## <a name="ransomware"></a>Ransomware

### <a name="ransomware-execution-following-suspicious-azure-ad-sign-in"></a>Ausführung von Ransomware im Anschluss an eine verdächtige Azure AD-Anmeldung

**MITRE ATT&CK-Taktiken:** Erstzugriff, Beeinträchtigung

**MITRE ATT&CK-Techniken:** Gültiges Konto (T1078), Verschlüsselung von Daten zur Beeinträchtigung (T1486)

**Datenconnector-Quellen:** Microsoft Cloud App Security, Azure Active Directory Identity Protection

**Beschreibung:** Fusion-Vorfälle dieses Typs deuten darauf hin, dass anomales Benutzerverhalten, das auf einen Ransomware-Angriff hinweist, im Anschluss an eine verdächtige Anmeldung bei einem Azure AD-Konto entdeckt wurden. Dieser Hinweis weist mit hoher Vertrauenswürdigkeit darauf hin, dass das in der Fusion-Vorfallsbeschreibung genannte Konto kompromittiert und dazu verwendet wurde, Daten zum Zweck der Erpressung des Datenbesitzers oder zur Hinderung des Datenbesitzers am Zugriff auf seine Daten zu verschlüsseln. Die Permutationen verdächtiger Azure AD-Anmeldebenachrichtigungen im Zusammenhang mit der Benachrichtigung zur Ausführung von Ransomware sind:  

- **Unmöglicher Ortswechsel zu einem typischen Ort, der zum Vorhandensein von Ransomware in der Cloud-App führt**

- **Anmeldeereignis von einem unbekannten Ort, das zum Vorhandensein von Ransomware in der Cloud-App führt**

- **Anmeldeereignis mit einem infizierten Gerät, das zum Vorhandensein von Ransomware in der Cloud-App führt**

- **Anmeldeereignis von einer anonymen IP-Adresse, das zum Vorhandensein von Ransomware in der Cloud-App führt**

- **Anmeldeereignis eines Benutzers mit kompromittierten Anmeldeinformationen, das zum Vorhandensein von Ransomware in der Cloud-App führt**

## <a name="remote-exploitation"></a>Remoteausnutzung

### <a name="suspected-use-of-attack-framework-followed-by-anomalous-traffic-flagged-by-palo-alto-networks-firewall"></a>Verdacht der Nutzung eines Angriffs-Frameworks, auf den anomaler, durch die Palo Alto Networks-Firewall gekennzeichneter Datenverkehr folgt.
Dieses Szenario befindet sich zurzeit in der **Public Preview**.

**MITRE ATT&CK-Taktiken:** Erstzugriff, Ausführung, Lateral Movement, Rechteausweitung

**MITRE ATT&CK-Techniken:** Ausnutzung von öffentlichen Anwendungen (T1190), Ausnutzung zur Clientausführung (T1203), Ausnutzung von Remotediensten (T1210), Ausnutzung zur Rechteausweitung (T1068)

**Datenconnector-Quellen:** Microsoft Defender für den Endpunkt (vormals MDATP), Palo Alto Networks 

**Beschreibung:** Fusion-Vorfälle dieses Typs deuten darauf hin, dass die nicht standardmäßige Verwendung von Protokollen, die der Verwendung von Angriffs-Frameworks wie z. B. Metasploit ähnelt, erkannt wurde. Im Anschluss daran wurden von der Palo Alto Networks-Firewall verdächtige eingehende Aktivitäten erkannt. Dies kann ein früher Hinweis darauf, sein, dass ein Angreifer einen Dienst ausgenutzt hat, um Zugriff auf Ihre Netzwerkressourcen zu erlangen, oder den Zugriff bereits erlangt hat und versucht, verfügbare Systeme/Dienste weiter für Seitwärtsbewegung und/oder Rechteausweitung auszunutzen. In Palo Alto-Protokollen sucht Azure Sentinel hauptsächlich nach [Bedrohungen](https://docs.paloaltonetworks.com/pan-os/8-1/pan-os-admin/monitoring/view-and-manage-logs/log-types-and-severity-levels/threat-logs), und der Datenverkehr wird als verdächtig eingestuft, wenn Bedrohungen durchgelassen werden (verdächtige Daten, Dateien, Überflutungen, Pakete, Scans, Spyware, URLs, Viren, Sicherheitsrisiken, Wildfireviren, Wildfires). Konsultieren Sie außerdem das Palo Alto-Bedrohungsprotokoll, das dem in der Fusion-Vorfallsbeschreibung aufgelisteten [Bedrohungs-/Inhaltstyp](https://docs.paloaltonetworks.com/pan-os/8-1/pan-os-admin/monitoring/use-syslog-for-monitoring/syslog-field-descriptions/threat-log-fields.html) entspricht, um weitere Details zur Benachrichtigung zu erhalten.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun mehr über die erweiterte Erkennung von mehrstufigen Angriffen erfahren haben, ist für Sie ggf. die folgende Schnellstartanleitung interessant. Darin wird veranschaulicht, wie Sie Einblicke in Ihre Daten und in potenzielle Bedrohungen erhalten: [Erste Schritte mit Azure Sentinel](quickstart-get-visibility.md).

Arbeiten Sie das folgende Tutorial durch, wenn Sie bereit zum Untersuchen der Incidents sind, die für Sie erstellt wurden: [Untersuchen von Vorfällen mit Azure Sentinel](tutorial-investigate-cases.md).

