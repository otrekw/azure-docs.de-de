---
title: 'Übersicht: Azure DNS-Aliaseinträge – Azure DNS'
description: In diesem Artikel erfahren Sie mehr über die Unterstützung für Aliaseinträge in Microsoft Azure DNS.
services: dns
author: rohinkoul
ms.service: dns
ms.topic: article
ms.date: 04/23/2021
ms.author: rohink
ms.openlocfilehash: 962f1f98ad610c953368b351c1859e8e738340f9
ms.sourcegitcommit: ad921e1cde8fb973f39c31d0b3f7f3c77495600f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2021
ms.locfileid: "107949378"
---
# <a name="azure-dns-alias-records-overview"></a>Azure DNS-Aliaseinträge – Übersicht

Azure DNS-Aliaseinträge sind Qualifizierungen für einen DNS-Ressourceneintragssatz. Sie können aus Ihrer DNS-Zone heraus auf andere Azure-Ressourcen verweisen. So können Sie beispielsweise einen Aliaseintragssatz erstellen, der anstatt auf einen A-Datensatz auf eine öffentliche Azure-IP-Adresse verweist. Ihr Aliaseintragssatz verweist dynamisch auf eine Dienstinstanz einer öffentlichen Azure-IP-Adresse. Dadurch aktualisiert sich der Aliaseintragssatz während der DNS-Auflösung nahtlos selbst.

Aliaseintragssätze werden für die folgenden Eintragstypen in einer Azure DNS-Zone unterstützt: 

- Ein
- AAAA
- CNAME

> [!NOTE]
> Wenn Sie beabsichtigen, für den Verweis auf ein [Azure Traffic Manager-Profil](../traffic-manager/quickstart-create-traffic-manager-profile.md) einen Aliaseintrag für die A- oder AAAA-Eintragstypen zu verwenden, stellen Sie sicher, dass das Traffic Manager-Profil nur [externe Endpunkte](../traffic-manager/traffic-manager-endpoint-types.md#external-endpoints) aufweist. Sie müssen die IPv4- oder IPv6-Adresse für externe Endpunkte in Traffic Manager angeben. Sie können keine vollständig qualifizierten Domänennamen (FQDNs) in Endpunkten verwenden. Verwenden Sie idealerweise statische IP-Adressen.

## <a name="capabilities"></a>Funktionen

- **Verweisen auf eine öffentliche IP-Ressource aus einem DNS-A/AAAA-Eintragssatz:** Sie können einen A/AAAA-Eintragssatz erstellen und zu einem Aliaseintragssatz machen, um auf eine öffentliche IP-Ressource (Standard oder Basic) zu verweisen. Der DNS-Ressourceneintragssatz verändert sich automatisch, wenn die öffentliche IP-Adresse geändert oder gelöscht wird. Verbleibende DNS-Einträge, die auf falsche IP-Adressen verweisen, werden vermieden.

   > [!NOTE]
   > Es besteht derzeit ein Limit von 20 Alias-Datensätzen pro Ressource.

- **Verweisen Sie auf ein Traffic Manager-Profil von einem DNS A/AAAA/CNAME-Datensatz** - Sie können einen A/AAAA- oder CNAME-Datensatz erstellen und Alias-Datensätze verwenden, um auf ein Traffic Manager-Profil zu verweisen. Dies ist besonders nützlich, wenn Sie Datenverkehr an einem Zonen-Apex weiterleiten müssen, weil herkömmliche CNAME-Einträge für einen Zonen-Apex nicht unterstützt werden. Beispiel: Ihr Traffic Manager-Profil heißt „myprofile.trafficmanager.net“, und die DNS-Zone Ihres Unternehmens heißt „contoso.com“. Sie können einen Aliaseintragssatz vom Typ A/AAAA für „contoso.com“ (den Zonen-Apex) erstellen und an „myprofile.trafficmanager.net“ verweisen.
- **Verweisen Sie auf einen Azure Content Delivery Network-Endpunkt (CDN)** - Dies ist nützlich, wenn Sie statische Websites mit Azure Storage und Azure CDN erstellen.
- **Verweisen Sie auf einen anderen DNS-Datensatz innerhalb desselben Bereichs** - Alias-Datensätze können auf andere Datensätze desselben Typs verweisen. So kann beispielsweise ein DNS-CNAME-Eintragssatz ein Alias für einen anderen CNAME-Eintragssatz sein. Dies ist nützlich, wenn Sie möchten, dass einige Eintragssätze Aliase und andere keine Aliase sind.

## <a name="scenarios"></a>Szenarien

Wie folgt, finden Sie einige gängige Szenarien für die Verwendung von Alias-Datensätzen.

### <a name="prevent-dangling-dns-records"></a>Vermeiden verwaister DNS-Einträge

Ein verbreitetes Problem bei herkömmlichen DNS-Einträgen sind verwaiste Einträge. Dabei handelt es sich zum Beispiel um DNS-Einträge, die bei Änderungen an IP-Adressen nicht aktualisiert wurden. Dieses Problem tritt besonders bei A/AAAA- und CNAME-Eintragstypen auf.

Bei einem herkömmlichen DNS-Bereichsdatensatz wurde der damit verbundene DNS-Datensatz manuell aktualisiert, wenn die Ziel-IP oder der CNAME nicht mehr existiert. In einigen Unternehmen kann eine manuelle Aktualisierung aufgrund von Prozessproblemen oder der Trennung von Rollen und zugehörigen Berechtigungsstufen nicht schnell erfolgen. So kann es beispielsweise sein, dass eine Rolle zum Löschen von CNAMEs oder IP-Adressen autorisiert ist, die zu einer Anwendung gehören. Sie haben jedoch nicht genügend Berechtigung, um den DNS-Eintrag zu aktualisieren, der auf diese Ziele verweist. Eine Verzögerung bei der Aktualisierung des DNS-Eintrags kann für die Benutzer potenziell zu einem Ausfall führen.

Aliaseinträge verhindern verbleibende Verweise durch eine enge Kopplung des Lebenszyklus eines DNS-Eintrags an eine Azure-Ressource. Stellen Sie sich beispielsweise einen DNS-Eintrag vor, der als Aliaseintrag qualifiziert wird, um auf eine öffentliche IP-Adresse oder auf ein Traffic Manager-Profil zu verweisen. Wenn Sie diese zugrunde liegenden Ressourcen löschen, wird der DNS-Aliaseintrag ein leerer Eintragssatz. Er verweist nicht mehr auf die gelöschte Ressource.

### <a name="update-dns-record-set-automatically-when-application-ip-addresses-change"></a>Automatisches Aktualisieren von DNS-Eintragssätzen bei Änderung von Anwendungs-IP-Adressen

Dieses Szenario ähnelt dem vorherigen Szenario. Möglicherweise wird eine Anwendung verschoben, oder der zugrunde liegende virtuelle Computer wird neu gestartet. Ein Aliaseintrag wird automatisch aktualisiert, wenn sich die IP-Adresse für die zugrundeliegende öffentliche IP-Ressource ändert. Anhand dieser Aktualisierung kann das Sicherheitsrisiko vermieden werden, dass die Benutzer auf eine andere Anwendung geleitet werden, der die alte öffentliche IP-Adresse zugewiesen wurde.

### <a name="host-load-balanced-applications-at-the-zone-apex"></a>Hosten von Anwendungen mit Lastenausgleich im Zonen-Apex

Das DNS-Protokoll verhindert die Zuweisung von CNAME-Einträgen am Zonen-Apex. Wenn Ihre Domäne zum Beispiel contoso.com ist. Sie können CNAME-Einträge für somelabel.contoso.com erstellen, Sie können jedoch keinen CNAME für contoso.com selbst erstellen.

Diese Einschränkung stellt Anwendungsbesitzer, die über Anwendungen mit Lastenausgleich hinter [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) verfügen, vor ein Problem. Da die Verwendung eines Traffic Manager-Profils die Erstellung eines CNAME-Eintrags erfordert, ist es nicht möglich, vom Bereichs-Apex aus auf das Traffic Manager-Profil zu verweisen.

Um dieses Problem zu beheben, können Sie Aliaseinträge verwenden. Im Gegensatz zu CNAME-Einträgen werden Aliaseinträge am Zonen-Apex erstellt, und Anwendungsbesitzer können damit Ihren Zonen-Apex-Eintrag an ein Traffic Manager-Profil mit externen Endpunkten verweisen. Anwendungsbesitzer verweisen auf das gleiche Traffic Manager-Profil, das auch für andere Domänen innerhalb ihrer DNS-Zone verwendet wird.

So können beispielsweise „contoso.com“ und „www\.contoso.com“ jeweils auf das gleiche Traffic Manager-Profil verweisen. Weitere Informationen zur Verwendung von Aliaseinträgen mit Azure Traffic Manager-Profilen finden Sie im Abschnitt „Nächste Schritte“.

### <a name="point-zone-apex-to-azure-cdn-endpoints"></a>Verweisen an der Zonenspitze auf Azure CDN-Endpunkte

Genau wie ein Traffic Manager-Profil können Sie auch Aliasdatensätze verwenden, um an der Zonenspitze Ihrer DNS-Zone auf Azure CDN-Endpunkte zu verweisen. Dies ist nützlich, wenn Sie statische Websites mit Azure Storage und Azure CDN erstellen. Anschließend können Sie auf die Website zugreifen, ohne dem DNS-Namen „www“ voranstellen zu müssen.

Wenn Ihre statische Website `www.contoso.com` heißt, können Ihre Benutzer mit `contoso.com` auf Ihre Website zugreifen, ohne dem DNS-Namen „www“ voranstellen zu müssen.

Wie zuvor beschrieben, werden CNAME-Einträge am Zonen-Apex nicht unterstützt. Sie können keinen CNAME-Eintrag verwenden, um contoso.com auf Ihren CDN-Endpunkt zu verweisen. Stattdessen können Sie einen Aliaseintrag verwenden, um direkt an der Zonenspitze auf einen CDN-Endpunkt zu verweisen.

> [!NOTE]
> Das Verweisen einer Zonenspitze auf CDN-Endpunkte für Azure CDN von Akamai wird derzeit nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Aliaseinträgen finden Sie in den folgenden Artikeln:

- [Tutorial: Konfigurieren eines Aliaseintrags, um auf eine öffentliche Azure-IP-Adresse zu verweisen](tutorial-alias-pip.md)
- [Tutorial: Konfigurieren eines Alias-Ressourceneintrags zur Unterstützung von Apex-Domänennamen mit Traffic Manager](tutorial-alias-tm.md)
- [Häufig gestellte Fragen zu DNS](./dns-faq.md#alias-records)