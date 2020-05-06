---
title: 'Tutorial: Aktualisieren der Frequenz der automatischen Zertifikatrotation in Key Vault | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie die Frequenz der automatischen Zertifikatrotation in Azure Key Vault über das Azure-Portal aktualisieren.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/16/2020
ms.author: sebansal
ms.openlocfilehash: 2e6c250a0bcb9d73e7c572dfe8138c31269993e8
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82107557"
---
# <a name="tutorial-configuring-certificates-auto-rotation-in-key-vault"></a>Tutorial: Konfigurieren der automatischen Zertifikatrotation in Key Vault

Mit Azure Key Vault können Sie mühelos digitale Zertifikate bereitstellen und verwalten. Das gilt sowohl für öffentliche und private SSL-/TLS-Zertifikate, die von einer Zertifizierungsstelle signiert wurden, als auch für selbst signierte Zertifikate. Darüber hinaus kann Key Vault Zertifikate über Partnerschaften mit Zertifizierungsstellen anfordern und verlängern und bietet somit eine zuverlässige Lösung für die Verwaltung des Zertifikatlebenszyklus. In diesem Tutorial werden Zertifikatattribute wie die Gültigkeitsdauer, die Frequenz der automatischen Rotation und die Zertifizierungsstelle aktualisiert. Weitere Informationen zu Key Vault finden Sie in der [Übersicht](../general/overview.md).

Das Tutorial veranschaulicht folgende Vorgehensweisen:

> [!div class="checklist"]
> * Verwalten eines Zertifikats über das Azure-Portal
> * Hinzufügen eines Zertifizierungsstellenanbieter-Kontos
> * Aktualisieren der Gültigkeitsdauer eines Zertifikats
> * Aktualisieren der Frequenz der automatischen Zertifikatrotation
> * Aktualisieren von Zertifikatattributen mithilfe von Azure PowerShell


Machen Sie sich zunächst mit den [grundlegenden Konzepten von Key Vault](../general/basic-concepts.md) vertraut. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="create-a-vault"></a>Erstellen eines Tresors

Erstellen Sie eine Key Vault-Instanz zum Ausführen von Vorgängen, oder wählen Sie eine bereits vorhandene Key Vault-Instanz aus. (Die Schritte zum Erstellen einer Key Vault-Instanz finden Sie [hier](../quick-create-portal.md).) In diesem Beispiel heißt der Tresor **Example-Vault**. 

![Ausgabe nach Erstellung der Key Vault-Instanz](../media/certificates/tutorial-import-cert/vault-properties.png)

## <a name="create-a-certificate-in-key-vault"></a>Erstellen eines Zertifikats in Key Vault

Erstellen Sie ein Zertifikat im Tresor, oder importieren Sie eines. (Die Schritte zum Erstellen eines Zertifikats im Schlüsseltresor finden Sie [hier](../quick-create-portal.md).) In diesem Beispiel wird das Zertifikat **ExampleCertificate** verwendet.

> [!NOTE]
> In Azure Key Vault können die Lebenszyklusattribute eines Zertifikats sowohl im Rahmen der Zertifikaterstellung als auch nach der Erstellung aktualisiert werden. 
## <a name="updating-certificates-life-cycle-attributes"></a>Aktualisieren der Lebenszyklusattribute eines Zertifikats

Bei einem in der Key Vault-Instanz erstellten Zertifikat kann es sich um Folgendes handeln: 
- Ein selbstsigniertes Zertifikat.
- Ein Zertifikat, das mit einer Zertifizierungsstelle (Certificate Authority, CA) erstellt wurde, die über eine Key Vault-Partnerschaft verfügt.
- Ein Zertifikat, das mit einer Zertifizierungsstelle erstellt wurde, die über keine Key Vault-Partnerschaft verfügt.

Folgende Zertifizierungsstellen sind derzeit Key Vault-Partneranbieter:
- DigiCert: Key Vault bietet OV-TLS-/SSL-Zertifikate mit DigiCert.
- GlobalSign: Key Vault bietet OV-TSL-/SSL-Zertifikate mit GlobalSign.

Zertifikate werden von Azure Key Vault durch Partnerschaften mit Zertifizierungsstellen automatisch rotiert. Dank dieser Partnerschaften kann Key Vault automatisch Zertifikate anfordern und verlängern. **Die automatische Rotation ist daher nicht für Zertifikate verfügbar, die mit Zertifizierungsstellen erstellt wurden, die über keine Key Vault-Partnerschaft verfügen.** 

> [!NOTE]
> Ein Kontoadministrator für einen Zertifizierungsstellenanbieter erstellt Anmeldeinformationen, die von Key Vault verwendet werden können, um TSL-/SSL-Zertifikate über Key Vault zu erstellen, zu verlängern und zu verwenden.
![Zertifizierungsstelle](../media/certificates/tutorial-rotate-cert/cert-authority-create.png)
> 


### <a name="updating-certificates-life-cycle-attributes-at-the-time-of-certificate-creation"></a>Aktualisieren der Lebenszyklusattribute eines Zertifikats im Rahmen der Zertifikaterstellung

1. Wählen Sie auf den Key Vault-Eigenschaftenseiten die Option **Zertifikate** aus.
2. Klicken Sie auf **Generieren/Importieren**.
3. Aktualisieren Sie im Bildschirm **Zertifikat erstellen** die folgenden Werte:
    

    - **Gültigkeitsdauer**: Geben Sie den Wert in Monaten ein. Die Erstellung kurzlebiger Zertifikate ist eine empfohlene Sicherheitsmethode. Der Standardwert für die Gültigkeitsdauer beträgt 12 Monate.
    - **Lebensdauer-Aktionstyp**: Wählen Sie die Aktion für die automatische Verlängerung des Zertifikats bzw. für die Benachrichtigung aus. Aktualisieren Sie je nach getroffener Auswahl den Wert für „Prozentsatz der Lebensdauer“ oder für „Anzahl von Tagen vor Ablauf“. Die automatische Verlängerung eines Zertifikats ist standardmäßig auf 80 Prozent der Lebensdauer festgelegt.<br> Wählen Sie im Dropdownmenü eine der folgenden Optionen aus:

    |  Bei Erreichen des vorgegebenen Prozentsatzes der Lebensdauer automatisch erneuern| Bei Erreichen des vorgegebenen Prozentsatzes der Lebensdauer eine E-Mail an alle Kontakte senden |
    |-----------|------|
    |Wenn Sie diese Option auswählen, wird die automatische Rotation aktiviert. | Wenn Sie diese Option auswählen, werden nur die Kontakte benachrichtigt. Die automatische Rotation wird dadurch NICHT aktiviert.|
        


4. Klicken Sie auf **Erstellen**.

![Zertifikatlebenszyklus](../media/certificates/tutorial-rotate-cert/create-cert-lifecycle.png)

### <a name="updating-life-cycle-attributes-of-stored-certificate"></a>Aktualisieren der Lebenszyklusattribute eines gespeicherten Zertifikats

1. Wählen Sie die Key Vault-Instanz aus.
2. Wählen Sie auf den Key Vault-Eigenschaftenseiten die Option **Zertifikate** aus.
3. Wählen Sie das Zertifikat aus, das Sie aktualisieren möchten. In diesem Beispiel wird das Zertifikat **ExampleCertificate** verwendet.
4. Wählen Sie auf der oberen Menüleiste die Option **Ausstellungsrichtlinie** aus.

![Zertifikateigenschaften](../media/certificates/tutorial-rotate-cert/cert-issuance-policy.png)
5. Aktualisieren Sie im Bildschirm **Ausstellungsrichtlinie** die folgenden Werte:
- **Gültigkeitsdauer**: Aktualisieren Sie den Wert (in Monaten).
- **Lebensdauer-Aktionstyp**: Wählen Sie die Aktion für die automatische Verlängerung des Zertifikats bzw. für die Benachrichtigung aus. Aktualisieren Sie je nach getroffener Auswahl den Wert für „Prozentsatz der Lebensdauer“ oder für „Anzahl von Tagen vor Ablauf“. 

![Zertifikateigenschaften](../media/certificates/tutorial-rotate-cert/cert-policy-change.png)
6. Klicken Sie auf **Speichern**.

> [!IMPORTANT]
> Wenn Sie den Lebensdauer-Aktionstyp eines Zertifikats ändern, werden Änderungen für die vorhandenen Zertifikate umgehend erfasst.


### <a name="updating-certificates-attributes-using-powershell"></a>Aktualisieren von Zertifikatattributen mithilfe von PowerShell

```azurepowershell


Set-AzureKeyVaultCertificatePolicy -VaultName $vaultName 
                                   -Name $certificateName 
                                   -RenewAtNumberOfDaysBeforeExpiry [276 or appropriate calculated value]
```

> [!TIP]
> Wenn Sie die Verlängerungsrichtlinie für eine Liste von Zertifikaten ändern möchten, verwenden Sie „File.csv“ mit „<Tresorname>,<Zertifikatname>“ als Eingabe. <br/>
>  Tresor1,Zertifikat1 <br/>
>  Tresor2,Zertifikat2
>
>  ```azurepowershell
>  $file = Import-CSV C:\Users\myfolder\ReadCSVUsingPowershell\File.csv 
> foreach($line in $file)
> {
> Set-AzureKeyVaultCertificatePolicy -VaultName $vaultName -Name $certificateName -RenewAtNumberOfDaysBeforeExpiry [276 or appropriate calculated value]
> }
>  ```
> 
Weitere Informationen zu Parametern finden Sie [hier](https://docs.microsoft.com/cli/azure/keyvault/certificate?view=azure-cli-latest#az-keyvault-certificate-set-attributes).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Andere Schnellstartanleitungen und Tutorials für Key Vault bauen auf dieser Schnellstartanleitung auf. Falls Sie mit weiteren Schnellstartanleitungen und Tutorials fortfahren möchten, sollten Sie die Ressourcen nicht bereinigen.
Wenn Sie die Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe. Dadurch werden die Key Vault-Instanz und die dazugehörigen Ressourcen gelöscht. So löschen Sie die Ressourcengruppe über das Portal:

1. Geben Sie den Namen Ihrer Ressourcengruppe in das Suchfeld am oberen Rand des Portals ein. Klicken Sie in den Suchergebnissen auf die Ressourcengruppe aus dieser Schnellstartanleitung.
2. Wählen Sie die Option **Ressourcengruppe löschen**.
3. Geben Sie im Feld **GEBEN SIE DEN RESSOURCENGRUPPENNAMEN EIN:** den Namen der Ressourcengruppe ein, und klicken Sie anschließend auf **Löschen**.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie den Lebenszyklus eines Zertifikats aktualisiert. Weitere Informationen zu Key Vault und zur Integration in Ihre Anwendungen finden Sie in den folgenden Artikeln:

Informieren Sie sich ausführlicher über die [Verwaltung der Zertifikaterstellung in Azure Key Vault](https://docs.microsoft.com/azure/key-vault/certificates/create-certificate-scenarios).
- Lesen Sie die [Übersicht über Key Vault](../general/overview.md).