---
title: Importieren und Exportieren einer Domänenzonendatei – Azure CLI
titleSuffix: Azure DNS
description: Informationen zum Importieren und Exportieren einer DNS-Zonendatei (Domain Name System) in Azure DNS mithilfe einer Azure CLI
services: dns
author: rohinkoul
ms.service: dns
ms.date: 04/29/2021
ms.author: rohink
ms.topic: how-to
ms.openlocfilehash: 949e497057646507fcaa04907d032beb5b8ef47c
ms.sourcegitcommit: 52491b361b1cd51c4785c91e6f4acb2f3c76f0d5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108316891"
---
# <a name="import-and-export-a-dns-zone-file-using-the-azure-cli"></a>Importieren und Exportieren einer DNS-Zonendatei mit der Azure-Befehlszeilenschnittstelle

In diesem Artikel erfahren Sie, wie Sie eine DNS-Zonendatei mithilfe von Azure CLI in Azure DNS importieren und exportieren können.

## <a name="introduction-to-dns-zone-migration"></a>Einführung in die DNS-Zonenmigration

Eine DNS-Zonendatei ist eine Textdatei, die Informationen zu jedem Domain Name System-Eintrag (DNS-Eintrag) in der Zone enthält. Sie hat ein Standardformat, weshalb sie sich für das Übertragen von DNS-Einträgen zwischen DNS-Systemen eignet. Die Verwendung einer Zonendatei ist eine schnelle und praktische Möglichkeit zum Importieren von DNS-Zonen in Azure DNS. Sie können auch eine Zonendatei aus Azure DNS exportieren, um sie zusammen mit anderen DNS-Systemen zu verwenden.

Für Azure DNS wird das Importieren und Exportieren von Zonendateien mit der Azure-Befehlszeilenschnittstelle (CLI) unterstützt. Der Import von Zonendateien mithilfe von Azure PowerShell oder über das Azure-Portal wird zurzeit **nicht** unterstützt.

Die Azure-CLI ist ein plattformübergreifendes Befehlszeilentool zum Verwalten von Azure-Diensten. Sie steht für Windows, Mac und Linux auf der [Azure-Downloadseite](https://azure.microsoft.com/downloads/) zur Verfügung.

## <a name="obtain-your-existing-dns-zone-file"></a>Abrufen Ihrer vorhandenen DNS-Zonendatei

Bevor Sie eine DNS-Zonendatei in Azure DNS importieren, müssen Sie eine Kopie der Zonendatei abrufen. Der Speicherort dieser Datei hängt davon ab, wo die DNS-Zone derzeit gehostet wird.

* Wenn Ihre DNS-Zone zurzeit von einem Partnerdienst gehostet wird, können Sie die DNS-Zonendatei von dort herunterladen. Zu den Partnerdiensten gehören die Domänenregistrierungsstelle, der dedizierte DNS-Hostinganbieter oder ein alternativer Cloudanbieter.
* Falls Ihre DNS-Zone unter einem Windows-DNS gehostet wird, lautet der Standardordner für die Zonendateien **%systemroot%\system32\dns**. Der vollständige Pfad zu jeder Zonendatei wird auch in der DNS-Konsole auf der Registerkarte **Allgemein** angezeigt.
* Wenn Ihre DNS-Zone mithilfe von BIND gehostet wird, wird der Speicherort der Zonendatei für jede Zone in der BIND-Konfigurationsdatei **named.conf** angegeben.

## <a name="import-a-dns-zone-file-into-azure-dns"></a>Importieren einer DNS-Zonendatei in Azure DNS

Beim Importieren einer Zonendatei wird in Azure DNS eine neue Zone erstellt, falls noch keine vorhanden ist. Wenn die Zone aber vorhanden ist, werden die Ressourceneintragssätze in der Zonendatei mit den vorhandenen Sätzen zusammengeführt.

### <a name="merge-behavior"></a>Zusammenführungsverhalten

* Die neuen Ressourceneintragssätze werden standardmäßig mit den vorhandenen Sätzen zusammengeführt. Identische Einträge innerhalb eines zusammengeführten Ressourceneintragssatzes werden nicht dupliziert.
* Wenn Ressourceneintragssätze zusammengeführt werden, wird die Gültigkeitsdauer (Time to live, TTL) von bereits vorhandenen Sätzen verwendet.
* SOA-Parameter (Start of Authority, Autoritätsursprung) werden (mit Ausnahme von `host`) immer aus der importierten Zonendatei übernommen. Auch im Namenserver-Ressourceneintragssatz an der Zonenspitze wird immer die Gültigkeitsdauer aus der importierten Zonendatei verwendet.
* Ein importierter CNAME-Eintrag ersetzt keinen vorhandenen gleichnamigen CNAME-Eintrag.  
* Wenn es zu einem Konflikt zwischen einem CNAME-Eintrag und einem anderen gleichnamigen Eintrag eines anderen Typs kommt, wird der vorhandene Eintrag verwendet.

### <a name="additional-information-about-importing"></a>Weitere Informationen zum Importieren

Die folgenden Hinweise enthalten weitere technische Details zum Zonenimportprozess.

* Die Direktive `$TTL` ist optional und wird unterstützt. Wird die Direktive `$TTL` nicht angegeben, werden Einträge ohne explizite Gültigkeitsdauer mit dem Standardwert der Gültigkeitsdauer von 3.600 Sekunden importiert. Wenn bei zwei Einträgen in demselben Ressourceneintragssatz eine unterschiedliche Gültigkeitsdauer angegeben ist, wird der niedrigere Wert verwendet.
* Die Direktive `$ORIGIN` ist optional und wird unterstützt. Wenn `$ORIGIN` nicht festgelegt wird, ist der verwendete Standardwert der Zonenname entsprechend der Angabe in der Befehlszeile (einschließlich des abschließenden „.“).
* Die Direktiven `$INCLUDE` und `$GENERATE` werden nicht unterstützt.
* Die folgenden Eintragstypen werden unterstützt: A, AAAA, CAA, CNAME, MX, NS, SOA, SRV und TXT.
* Der SOA-Eintrag wird von Azure DNS automatisch erstellt, wenn eine Zone erstellt wird. Wenn Sie eine Zonendatei importieren, werden alle SOA-Parameter aus der Zonendatei entnommen, *mit Ausnahme* des Parameters `host`. Dieser Parameter verwendet den von Azure DNS bereitgestellten Wert, weil er auf den primären Namenserver verweisen muss, der von Azure DNS bereitgestellt wird.
* Der Namenserver-Ressourceneintragssatz an der Zonenspitze wird beim Erstellen der Zone auch automatisch von Azure DNS erstellt. Nur die Gültigkeitsdauer dieses Ressourceneintragssatzes wird importiert. Diese Einträge enthalten die von Azure DNS bereitgestellten Namen der Namenserver. Die Eintragsdaten werden von den Werten in der importierten Zonendatei nicht überschrieben.
* Während der öffentlichen Vorschauphase unterstützt Azure DNS nur TXT-Einträge mit einer einzelnen Zeichenfolge. TXT-Einträge müssen mit mehreren Zeichenfolgen verkettet und auf 255 Zeichen verkürzt werden.

### <a name="cli-format-and-values"></a>Format und Werte der Befehlszeilenschnittstelle

Das Format des Befehls der Azure-Befehlszeilenschnittstelle zum Importieren einer DNS-Zone lautet:

```azurecli-interactive-interactive
az network dns zone import -g <resource group> -n <zone name> -f <zone file name>
```

Werte:

* `<resource group>` ist der Name der Ressourcengruppe für die Zone in Azure DNS.
* `<zone name>` ist der Name der Zone.
* `<zone file name>` ist der Pfad/Name der zu importierenden Zonendatei.

Wenn eine Zone mit diesem Namen in der Ressourcengruppe nicht bereits vorhanden ist, wird eine für Sie erstellt. Bei einer vorhandenen Zone werden die importierten Ressourceneintragssätze mit vorhandenen Sätzen zusammengeführt. 

### <a name="import-a-zone-file"></a>Importieren einer Zonendatei

So importieren Sie eine Zonendatei für die Zone **contoso.com**

1. Falls noch keine vorhanden ist, erstellen Sie eine Ressourcengruppe.

    ```azurecli-interactive
    az group create --resource-group myresourcegroup -l westeurope
    ```

1. Zum Importieren der Zone **contoso.com** aus der Datei **contoso.com.txt** in eine neue DNS-Zone in der Ressourcengruppe **myresourcegroup** führen Sie den Befehl `az network dns zone import` aus.

    Mit diesem Befehl wird die Zonendatei geladen und analysiert. Der Befehl führt im Azure DNS-Dienst eine Reihe von Vorgängen aus, um die Zone und alle Ressourceneintragssätze darin zu erstellen. Der Befehl meldet im Konsolenfenster der Status sowie Fehler und Warnungen. Da Ressourceneintragssätze reihenweise erstellt werden, kann der Import einer großen Zonendatei einige Minuten dauern.

    ```azurecli-interactive
    az network dns zone import -g myresourcegroup -n contoso.com -f contoso.com.txt
    ```

### <a name="verify-the-zone"></a>Überprüfen der Zone

Nachdem Sie die Datei importiert haben, können Sie die DNS-Zone mithilfe einer der folgenden Methoden überprüfen:

* Zum Auflisten der Einträge verwenden Sie den folgenden Azure CLI-Befehl:

    ```azurecli-interactive
    az network dns record-set list -g myresourcegroup -z contoso.com
    ```

* Sie können die Einträge auch mit dem Azure CLI-Befehl `az network dns record-set ns list` auflisten.
* Mit `nslookup` können Sie die Namensauflösung für die Einträge überprüfen. Wenn die Zone noch nicht delegiert wurde, müssen Sie die richtigen Azure DNS-Namenserver explizit angeben. Im folgenden Beispiel wird gezeigt, wie Sie die Namen der Namenserver abrufen, die der Zone zugewiesen sind.

    ```azurecli-interactive
    az network dns record-set ns list -g myresourcegroup -z contoso.com  --output json 
    ```

    ```json
    [
      {
       .......
       "name": "@",
        "nsRecords": [
          {
            "additionalProperties": {},
            "nsdname": "ns1-03.azure-dns.com."
          },
          {
            "additionalProperties": {},
            "nsdname": "ns2-03.azure-dns.net."
          },
          {
            "additionalProperties": {},
            "nsdname": "ns3-03.azure-dns.org."
          },
          {
            "additionalProperties": {},
            "nsdname": "ns4-03.azure-dns.info."
          }
        ],
        "resourceGroup": "myresourcegroup",
        "ttl": 86400,
        "type": "Microsoft.Network/dnszones/NS"
      }
    ]
    ```

    Verwenden Sie die Windows-Eingabeaufforderung zum Abfragen des Datensatzes "www" mit dem Befehl `nslookup`.

    ```cmd
    nslookup www.contoso.com ns1-03.azure-dns.com

        Server: ns1-01.azure-dns.com
        Address:  40.90.4.1

        Name:www.contoso.com
        Addresses:  134.170.185.46
        134.170.188.221
    ```

### <a name="update-dns-delegation"></a>Aktualisieren der DNS-Delegierung

Nachdem Sie überprüft haben, ob die Zone richtig importiert wurde, müssen Sie die DNS-Delegierung so aktualisieren, dass sie auf die Azure DNS-Namenserver verweist. Weitere Informationen finden Sie unter [Aktualisieren der DNS-Delegierung](dns-domain-delegation.md).

## <a name="export-a-dns-zone-file-from-azure-dns"></a>Exportieren einer DNS-Zonendatei aus Azure DNS

Zum Importieren einer DNS-Zone verwenden Sie den folgenden Azure CLI-Befehl:

```azurecli-interactive
az network dns zone export -g <resource group> -n <zone name> -f <zone file name>
```

Werte:

* `<resource group>` ist der Name der Ressourcengruppe für die Zone in Azure DNS.
* `<zone name>` ist der Name der Zone.
* `<zone file name>` ist der Pfad/Name der zu exportierenden Zonendatei.

Sie müssen sich – wie beim Zonenimport – zunächst anmelden, Ihr Abonnement auswählen und die Azure-Befehlszeilenschnittstelle so konfigurieren, dass der Ressourcen-Manager-Modus verwendet wird.

### <a name="to-export-a-zone-file"></a>So exportieren Sie eine Zonendatei

Um die vorhandene Azure-DNS-Zone **contoso.com** in die Ressourcengruppe **myresourcegroup** in der Datei **contoso.com.txt** (im aktuellen Ordner) zu exportieren, führen Sie `azure network dns zone export` aus. Mit diesem Befehl wird der Azure DNS-Dienst aufgefordert, die Ressourceneintragssätze in der Zone aufzuzählen und die Ergebnisse in eine mit BIND kompatible Zonendatei zu exportieren.

```azurecli-interactive
az network dns zone export -g myresourcegroup -n contoso.com -f contoso.com.txt
```

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie [Ressourceneintragssätze und Einträge in Ihrer DNS-Zone verwalten](./dns-getstarted-cli.md).

* Erfahren Sie, wie Sie [Ihre Domäne an Azure DNS delegieren](dns-domain-delegation.md).
