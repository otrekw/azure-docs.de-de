---
title: Exportieren von Kostendaten mit einem SAS-Schlüssel für das Azure Storage-Konto
description: Dieser Artikel hilft Partnern, einen SAS-Schlüssel zu erstellen und Cost Management-Exporte zu konfigurieren.
author: bandersmsft
ms.author: banders
ms.date: 03/08/2021
ms.topic: how-to
ms.service: cost-management-billing
ms.subservice: cost-management
ms.reviewer: adwise
ms.openlocfilehash: 16302a6bcc3bf41432500d2fdaa4ec27c28dd5b3
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2021
ms.locfileid: "102509620"
---
# <a name="export-cost-data-with-an-azure-storage-account-sas-key"></a>Exportieren von Kostendaten mit einem SAS-Schlüssel für das Azure Storage-Konto

Die folgenden Informationen gelten nur für Microsoft-Partner.

Häufig verfügen Partner über keine eigenen Azure-Abonnements in dem Mandanten, der mit ihrer Microsoft Partner-Vereinbarung verknüpft ist. Partner mit einer Microsoft Partner-Vereinbarung, die globale Administratoren ihres Abrechnungskontos sind, können mit einem SAS-Schlüssel (Shared Access Signature) Kostendaten in ein Speicherkonto in einem anderen Mandanten exportieren und kopieren. Anders ausgedrückt: Ein Speicherkonto mit einem SAS-Schlüssel ermöglicht es dem Partner, über ein Speicherkonto, das nicht in der Microsoft Partner-Vereinbarung enthalten ist, exportierte Informationen zu erhalten. Dieser Artikel hilft Partnern, einen SAS-Schlüssel zu erstellen und Cost Management-Exporte zu konfigurieren.

## <a name="requirements"></a>Anforderungen

- Sie müssen ein Partner mit einer Microsoft Partner-Vereinbarung sein und über Kunden im Azure-Plan verfügen.
- Sie müssen globaler Administrator für das Abrechnungskonto der Partnerorganisation sein.
- Sie müssen über Zugriffsberechtigungen zum Konfigurieren eines Speicherkontos verfügen, das sich in einem anderen Mandanten Ihrer Partnerorganisation befindet. Sie sind für die Verwaltung der Berechtigungen und des Datenzugriffs verantwortlich, wenn Sie Daten in Ihr Speicherkonto exportieren.

## <a name="configure-azure-storage-with-a-sas-key"></a>Konfigurieren von Azure Storage mit einem SAS-Schlüssel

Rufen Sie ein Speicherkonto-SAS-Token ab, oder erstellen Sie eines im Azure-Portal. Führen Sie zum Erstellen eines SAS-Tokens im Azure-Portal die folgenden Schritte aus. Weitere Informationen über SAS-Schlüssel finden Sie unter [Gewähren von eingeschränktem Zugriff auf Daten mithilfe von SAS (Shared Access Signature)](../../storage/common/storage-sas-overview.md).

1. Navigieren Sie im Azure-Portal zu dem Speicherkonto.
    - Wenn Ihr Konto Zugriff auf mehrere Mandanten hat, wechseln Sie für den Zugriff auf das Speicherkonto das Verzeichnis. Wählen Sie in der oberen rechten Ecke des Azure-Portals Ihr Konto aus, und klicken Sie dann auf **Verzeichnis wechseln**.
    - Möglicherweise müssen Sie sich mit dem entsprechenden Mandantenkonto beim Azure-Portal anmelden, um auf das Speicherkonto zugreifen zu können.
1. Wählen Sie im linken Bereich **Shared Access Signature (SAS)** aus.  
    :::image type="content" source="./media/export-cost-data-storage-account-sas-key/storage-shared-access-signature.png" alt-text="Screenshot: Konfigurierte Azure Storage-SAS" lightbox="./media/export-cost-data-storage-account-sas-key/storage-shared-access-signature.png" :::
1. Konfigurieren Sie das Token mit den Einstellungen, die in der vorherigen Abbildung angegeben sind.
    1. Wählen Sie für _Zulässige Dienste_ die Option **Blob** aus.
    1. Aktivieren Sie für _Zugelassene Ressourcentypen_ die Optionen **Dienst**, **Container** und **Objekt**.
    1. Aktivieren Sie unter **Zugelassene Berechtigungen** die Optionen **Lesen**, **Schreiben**, **Löschen**, **Auflisten**, **Hinzufügen** und _Erstellen_.
    1. Wählen Sie das Ablaufdatum aus. Stellen Sie sicher, dass Sie das SAS-Exporttoken aktualisieren, bevor es abläuft. Je länger der von Ihnen konfigurierte Zeitraum bis zum Ablauf ist, desto länger wird der Export ausgeführt, bevor ein neues SAS-Token benötigt wird.
1. Wählen Sie für **Zugelassene Protokolle** die Option _Nur HTTPS_ aus.
1. Wählen Sie für **Bevorzugte Routingebene** die Option _Basic_ aus.
1. Wählen Sie für **Signaturschlüssel** die Option _key1_ aus. Wenn der Schlüssel zum Signieren des SAS-Tokens rotiert oder aktualisiert wird, müssen Sie ein neues SAS-Token für den Export generieren.
1. Klicken Sie auf **SAS und Verbindungszeichenfolge generieren**.
    Der angezeigte Wert für **SAS-Token** ist das Token, das Sie beim Konfigurieren von Exporten benötigen.

## <a name="create-a-new-export-with-a-sas-token"></a>Erstellen eines neuen Exports mit einem SAS-Token

Navigieren Sie im Abrechnungskontobereich zu **Exporte**, und erstellen Sie mit den folgenden Schritten einen neuen Export.

1. Klicken Sie auf **Erstellen**.
1. Konfigurieren Sie die Exportdetails wie bei einem normalen Export. Sie können den Export so konfigurieren, dass ein vorhandenes Verzeichnis oder ein vorhandener Container verwendet wird, oder Sie können ein neues Verzeichnis oder einen neuen Container angeben, und die Exporte werden für Sie erstellt.
1. Wenn Sie Speicher konfigurieren, wählen Sie **Use a SAS Token**  (SAS-Token verwenden) aus.  
    :::image type="content" source="./media/export-cost-data-storage-account-sas-key/new-export.png" alt-text="Screenshot: „Neuer Export“, wo ein SAS-Token ausgewählt wird" lightbox="./media/export-cost-data-storage-account-sas-key/new-export.png" :::
1. Geben Sie den Namen des Speicherkontos ein, und fügen Sie das SAS-Token ein.
1. Geben Sie einen vorhandenen Container oder ein vorhandenes Verzeichnis an, oder geben Sie einen neuen Container oder ein neues Verzeichnis an, der bzw. das erstellt werden soll.
1. Klicken Sie auf **Erstellen**.

Exporte auf Grundlage eines SAS-Tokens werden nur ausgeführt, solange das Token gültig ist. Wenn Sie das aktuelle Token nicht zurücksetzen, bevor es abläuft, kann der Export nicht mehr ausgeführt werden. Da das Token Zugriff auf Ihr Speicherkonto bietet, schützen Sie das Token so sorgfältig wie alle anderen vertraulichen Informationen. Sie sind für die Verwaltung der Berechtigungen und des Datenzugriffs verantwortlich, wenn Sie Daten in Ihr Speicherkonto exportieren.

## <a name="troubleshoot-exports-using-sas-tokens"></a>Problembehandlung bei Exporten mithilfe von SAS-Token

Im Folgenden werden häufige Probleme beschrieben, die bei der Konfiguration oder Verwendung von Exporten auf Grundlage von SAS-Token auftreten können.

- Im Azure-Portal wird keine Option für SAS-Schlüssel angezeigt.
  - Stellen Sie sicher, dass Sie ein Partner mit einer Microsoft Partner-Vereinbarung sind und über globale Administratorberechtigungen für das Abrechnungskonto verfügen. Dies sind die einzigen Personen, die den Export mit einem SAS-Schlüssel durchführen können.

- Wenn Sie versuchen, den Export zu konfigurieren, erhalten Sie die folgende Fehlermeldung:

    **Please ensure the SAS token is valid for blob service, is valid for container and object resource types, and has permissions: add create read write delete. (Storage service error code: AuthorizationResourceTypeMismatch)** (Stellen Sie sicher, dass das SAS-Token für den Blob-Dienst sowie für Container- und Objektressourcentypen gültig ist und über Erstellungs-, Lese-, Schreib- und Löschberechtigungen verfügt. (Speicherdienst-Fehlercode: AuthorizationResourceTypeMismatch))

    - Stellen Sie sicher, dass Sie den SAS-Schlüssel in Azure Storage ordnungsgemäß konfigurieren und erstellen.

- Nachdem Sie einen Export erstellt haben, wird nicht der vollständige SAS-Schlüssel angezeigt.
  - Dass der Schlüssel nicht angezeigt wird, entspricht dem erwarteten Verhalten. Nachdem der SAS-Export konfiguriert wurde, wird der Schlüssel aus Sicherheitsgründen ausgeblendet.

- Sie können über den Mandanten, in dem der Export konfiguriert wurde, nicht auf das Speicherkonto zugreifen.
  - Dies entspricht dem erwarteten Verhalten. Wenn sich das Speicherkonto in einem anderen Mandanten befindet, müssen Sie zunächst im Azure-Portal zu diesem Mandanten navigieren, um das Speicherkonto zu suchen.

- Der Export schlägt aufgrund eines Fehlers beim SAS-Token fehl.
  - Der Export kann nur ausgeführt werden, solange das SAS-Token gültig ist. Erstellen Sie einen neuen Schlüssel, und führen Sie den Export aus.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Exportieren von Cost Management-Daten finden Sie unter [Erstellen und Exportieren von Daten](tutorial-export-acm-data.md).
- Informationen zum Exportieren großer Mengen von Nutzungsdaten finden Sie unter [Regelmäßiges Abrufen großer Datasets mithilfe von Exporten](ingest-azure-usage-at-scale.md).
