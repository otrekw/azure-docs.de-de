---
title: Beheben von Problemen mit Zertifikaten bei Azure Stack Edge Pro mit GPU | Microsoft-Dokumentation
description: Beschreibt die Problembehandlung von Zertifikatsfehlern mit Azure Stack Edge Pro-GPU-Geräten.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: troubleshooting
ms.date: 08/28/2020
ms.author: alkohli
ms.openlocfilehash: a0918c6cebd50231a9664811bb467e04d2d2bfd9
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90891327"
---
# <a name="troubleshooting-certificate-errors"></a>Problembehandlung für Zertifikate

Dieser Artikel enthält Informationen zur Problembehandlung allgemeiner Zertifikatsfehler beim Installieren von Zertifikaten auf einem Azure Stack Edge Pro-Gerät.

## <a name="common-certificate-errors"></a>Häufige Zertifikatsfehler

In der folgenden Tabelle sind häufige Zertifikatsfehler und ausführliche Informationen zu diesen Fehlern und möglichen Lösungen aufgeführt:

> [!NOTE]
> Positionsparameter sind als &#8220;{0}, {1}, ... , {n}&#8221; angegeben. Die Positionsparameter akzeptieren Werte, die von den verwendeten Zertifikaten abhängen.

| Fehlercode | BESCHREIBUNG |
|---|---|
| CertificateManagement_UntrustedCertificate | Beim Zertifikat mit dem Antragstellernamen {0} ist die Zertifikatkette unterbrochen. Laden Sie das Signaturkettenzertifikat hoch, bevor Sie dieses Zertifikat hochladen.|
| CertificateManagement_DeviceNotRegistered| Das Gerät ist nicht aktiviert. Sie können erst nach der Aktivierung ein Supportzertifikat hochladen.|
| CertificateManagement_EmptySAN | Das Zertifikat mit dem Antragstellernamen {0} weist keinen alternativen Antragstellernamen auf. Überprüfen Sie die Zertifikateigenschaften, und stellen Sie ein neues Zertifikat bereit.|
| CertificateManagement_ExpiredCertificate | Das Zertifikat mit dem Typ {0} ist abgelaufen oder läuft demnächst ab. Überprüfen Sie das Ablaufdatum des Zertifikats, und stellen Sie bei Bedarf ein neues Zertifikat bereit.|
| CertificateManagement_FormatMismatch | Das Zertifikatformat wird nicht unterstützt. Überprüfen Sie das Format des Zertifikats, und stellen Sie bei Bedarf ein neues Zertifikat bereit.  {0} erwartet, {1} gefunden. |
| CertificateManagement_GenericError | Der Zertifikatverwaltungsvorgang konnte nicht durchgeführt werden. Wiederholen Sie den Vorgang in einigen Minuten. Wenden Sie sich an den Microsoft Support, wenn das Problem weiterhin besteht. |
| CertificateManagement_HttpsBindingFailure | Das Zertifikat mit dem Antragstellernamen {0} konnte keinen sicheren HTTPS-Kanal erstellen. Überprüfen Sie das hochgeladene Zertifikat, und stellen Sie bei Bedarf ein neues Zertifikat bereit. Dieser Fehler tritt beim Geräteknotenzertifikat auf.|
| CertificateManagement_IncorrectKeyCertSignKeyUsage | Im Zertifikat mit dem Antragstellernamen {0} darf als Schlüsselverwendung nicht „Zertifikatsignatur“ angegeben werden. Überprüfen Sie die Schlüsselverwendung des Zertifikats, und stellen Sie bei Bedarf ein neues Zertifikat bereit. Dieser Fehler tritt beim Signaturkettenzertifikat auf. |
| CertificateManagement_IncorrectKeyNumber | Das Zertifikat mit dem Antragstellernamen {0} weist die falsche Schlüsselnummer {1} auf. Überprüfen Sie die Schlüsselnummer des Zertifikats, und stellen Sie bei Bedarf ein neues Zertifikat bereit.|
| CertificateManagement_InvalidP7b | Die hochgeladene Zertifikatdatei ist ungültig.  Überprüfen Sie das Format des Zertifikats, und stellen Sie bei Bedarf ein neues Zertifikat bereit.|
| CertificateManagement_KeyAlgorithmNotRSA | Das Zertifikat verwendet den RSA-Schlüsselalgorithmus nicht. Es werden nur RSA-Zertifikate unterstützt. |
| CertificateManagement_KeySizeNotSufficient | Das Zertifikat mit dem Antragstellernamen {0} weist die unzureichende Schlüsselgröße {1} auf. Die Mindestgröße für Schlüssel beträgt 4096.|
| CertificateManagement_MissingClientOid | Das Zertifikat mit dem Antragstellernamen {0} weist keine OID für die Clientauthentifizierung auf. Überprüfen Sie die Zertifikateigenschaften, und stellen Sie bei Bedarf ein neues Zertifikat bereit.|
| CertificateManagement_MissingDigitalSignatureKeyUsage | Im Zertifikat mit dem Antragstellernamen {0} ist als Schlüsselverwendung nicht „Digitale Signatur“ angegeben. Überprüfen Sie die Zertifikateigenschaften, und stellen Sie bei Bedarf ein neues Zertifikat bereit. |
| CertificateManagement_MissingEntryInSAN | Das Zertifikat mit dem Antragstellernamen {0} weist im alternativen Antragstellernamen keinen Eintrag für den Antragstellernamen auf. Überprüfen Sie die Zertifikateigenschaften, und stellen Sie ein neues Zertifikat bereit. |
| CertificateManagement_MissingKeyCertSignKeyUsage | Im Zertifikat mit dem Antragstellernamen {0} ist als Schlüsselverwendung nicht „Zertifikatsignatur“ angegeben. Überprüfen Sie die Zertifikateigenschaften, und stellen Sie bei Bedarf ein neues Zertifikat bereit.|
| CertificateManagement_MissingKeyEnciphermentKeyUsage | Im Zertifikat mit dem Antragstellernamen {0} ist als Schlüsselverwendung nicht „Schlüsselchiffrierung“ angegeben. Überprüfen Sie die Zertifikateigenschaften, und stellen Sie bei Bedarf ein neues Zertifikat bereit. |
| CertificateManagement_MissingServerOid | Das Zertifikat mit dem Antragstellernamen {0} weist keine OID für die Serverauthentifizierung auf. Überprüfen Sie die Zertifikateigenschaften, und stellen Sie bei Bedarf ein neues Zertifikat bereit.|
| CertificateManagement_NameMismatch | Der Zertifikattyp stimmt nicht überein. Erwarteter Bereich: {0}, gefunden: {1}. Laden Sie ein geeignetes Zertifikat hoch.|
| CertificateManagement_NoPrivateKeyPresent | Beim Zertifikat mit dem Antragstellernamen {0} ist kein privater Schlüssel vorhanden. Laden Sie ein PFX-Zertifikat mit einem privaten Schlüssel hoch.|
| CertificateManagement_NotSelfSignedCertificate | Das Zertifikat mit dem Antragstellernamen {0} ist nicht selbstsigniert. Stammzertifikate müssen selbstsigniert sein. |
| CertificateManagement_NotSupportedOnVirtualAppliance | Dieser Vorgang wird auf dem virtuellen Gerät nicht unterstützt. Dieser Fehler weist darauf hin, dass das Signieren nur bei einem Data Box Gateway erfolgt, das in der Tactical Cloud Appliance ausgeführt wird. Dieser Fehler tritt auf, wenn das Gerät über Windows PowerShell verwaltet wird.|
| CertificateManagement_SelfSignedCertificate | Das Zertifikat mit dem Antragstellernamen {0} ist selbstsigniert. Laden Sie ein Zertifikat hoch, das ordnungsgemäß signiert ist.|
| CertificateManagement_SignatureAlgorithmSha1 | Das Zertifikat mit dem Antragstellernamen {0} weist einen nicht unterstützten Signaturalgorithmus auf. SHA1RSA wird nicht unterstützt. |
| CertificateManagement_SubjectNamesInvalid | Das Zertifikat mit dem Antragstellernamen {0} weist nicht den richtigen Antragstellernamen oder alternativen Antragstellernamen für das Zertifikat {1} auf. Überprüfen Sie das hochgeladene Zertifikat, und stellen Sie bei Bedarf ein neues Zertifikat bereit. Sie sollten überprüfen, ob der DNS-Name den SANS-Namen entspricht.|
| CertificateManagement_UnreadableCertificate | Das Zertifikat mit dem Typ {0} konnte nicht gelesen werden. Dieser Fehler tritt auf, wenn das Zertifikat nicht lesbar oder beschädigt ist. Stellen Sie ein neues Zertifikat bereit.|
| CertificateSubjectNotFound | Das Zertifikat mit dem Antragstellernamen {0} wurde nicht gefunden. Stellen Sie ein neues Zertifikat bereit.|

## <a name="next-steps"></a>Nächste Schritte

[Zertifikatanforderungen](azure-stack-edge-j-series-certificate-requirements.md)