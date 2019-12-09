---
title: Verwaltungskonzepte für Azure AD Domain Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine verwaltete Azure Active Directory Domain Services-Domäne und das Verhalten von Benutzerkonten und Kennwörtern verwalten.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 10/08/2019
ms.author: iainfou
ms.openlocfilehash: f239bab48e732755361fe734fdc24b37d3823c63
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2019
ms.locfileid: "74481012"
---
# <a name="management-concepts-for-user-accounts-passwords-and-administration-in-azure-active-directory-domain-services"></a>Verwaltungskonzepte für Benutzerkonten, Kennwörter und die Verwaltung in Azure Active Directory Domain Services

Beim Erstellen und Ausführen einer verwalteten Azure AD DS-Domäne (Active Directory Domain Services) gibt es einige Unterschiede im Verhalten im Vergleich zu einer herkömmlichen lokalen AD DS-Umgebung. Sie verwenden dieselben Verwaltungstools in Azure AD DS wie eine selbstverwaltete Domäne, aber Sie können nicht direkt auf die Domänencontroller (DC) zugreifen. Es gibt auch einige Unterschiede im Verhalten von Kennwortrichtlinien und Kennworthashes, die von der Quelle der Erstellung des Benutzerkontos abhängig sind.

In diesem konzeptionellen Artikel erfahren Sie, wie Sie eine verwaltete Azure AD DS-Domäne und das unterschiedliche Verhalten von Benutzerkonten abhängig von der Art ihrer Erstellung verwalten.

## <a name="domain-management"></a>Domänenverwaltung

In Azure AD DS sind die Domänencontroller (DCs), die alle Ressourcen wie Benutzer und Gruppen, Anmeldeinformationen und Richtlinien enthalten, Teil des verwalteten Diensts. Aus Redundanzgründen werden zwei DCs als Teil einer verwalteten Azure AD DS-Domäne erstellt. Sie können sich nicht bei diesen DCs anmelden, um Verwaltungsaufgaben auszuführen. Stattdessen erstellen Sie eine Verwaltungs-VM, die der verwalteten Domäne von Azure AD DS hinzugefügt wird, und installieren dann die regulären AD DS Verwaltungstools. Sie können z.B. das Active Directory-Verwaltungscenter oder MMC-Snap-Ins (Microsoft Management Console) wie etwa DNS oder Gruppenrichtlinienobjekte verwenden.

## <a name="user-account-creation"></a>Erstellung von Benutzerkonten

Benutzerkonten können in Azure AD DS auf verschiedene Weise erstellt werden. Die meisten Benutzerkonten werden aus Azure AD synchronisiert. Dies kann auch Benutzerkonten beinhalten, die aus einer lokalen AD DS-Umgebung synchronisiert werden. Sie können Konten auch direkt in Azure AD DS manuell erstellen. Einige Features wie die anfängliche Kennwortsynchronisierung oder die Kennwortrichtlinie verhalten sich abhängig davon, wie und wo Benutzerkonten erstellt werden, unterschiedlich.

* Das Benutzerkonto kann aus Azure AD synchronisiert werden. Dazu gehören reine Cloudbenutzerkonten, die direkt in Azure AD erstellt wurden, sowie hybride Benutzerkonten, die aus einer lokalen AD DS-Umgebung mit Azure AD Connect synchronisiert werden.
    * Der Großteil der Benutzerkonten in Azure AD DS wird durch den Synchronisierungsprozess aus Azure AD erstellt.
* Das Benutzerkonto kann manuell in einer verwalteten Azure AD DS-Domäne erstellt werden und ist dann in Azure AD nicht vorhanden.
    * Wenn Sie Dienstkonten für Anwendungen erstellen müssen, die nur in Azure AD DS ausgeführt werden, können Sie diese manuell in der verwalteten Domäne erstellen. Da die Synchronisierung aus Azure AD unidirektional ist, werden Benutzerkonten, die in Azure AD DS erstellt werden, nicht mit Azure AD synchronisiert.

## <a name="password-policy"></a>Kennwortrichtlinie

Azure AD DS enthält eine Standardkennwortrichtlinie, die Einstellungen für Vorgänge wie Kontosperrung, maximales Kennwortalter und Kennwortkomplexität definiert. Einstellungen wie die Kontosperrungsrichtlinie gelten für alle Benutzer in Azure AD DS, und zwar unabhängig davon, wie der Benutzer erstellt wurde (im vorherigen Abschnitt beschrieben). Einige Einstellungen (z.B. die minimale Kennwortlänge und die Kennwortkomplexität) gelten nur für Benutzer, die direkt in Azure AD DS erstellt werden.

Sie können eigene benutzerdefinierte Kennwortrichtlinien erstellen, um die Standardrichtlinie in Azure AD DS außer Kraft zu setzen. Diese benutzerdefinierten Richtlinien können dann nach Bedarf auf bestimmte Gruppen von Benutzern angewendet werden.

Weitere Informationen zu den Unterschieden bei der Anwendung von Kennwortrichtlinien in Abhängigkeit von der Quelle der Benutzererstellung finden Sie unter [Kennwort- und Kontosperrungsrichtlinien für verwaltete Domänen ][password-policy].

## <a name="password-hashes"></a>Kennworthashes

Um Benutzer in der verwalteten Domäne authentifizieren zu können, benötigt Azure AD DS Kennworthashes in einem Format, das für die Authentifizierung über NT LAN Manager (NTLM) und Kerberos geeignet ist. Azure AD generiert oder speichert erst dann Kennworthashes in dem für die NTLM- oder Kerberos-Authentifizierung erforderlichen Format, wenn Sie Azure AD DS für Ihren Mandanten aktivieren. Aus Sicherheitsgründen speichert Azure AD Kennwörter nicht als Klartext. Daher kann Azure AD diese NTLM- oder Kerberos-Kennworthashes nicht automatisch auf der Grundlage bereits vorhandener Anmeldeinformationen von Benutzern generieren.

Bei reinen Cloudbenutzerkonten müssen Benutzer ihre Kennwörter ändern, bevor sie Azure AD DS verwenden können. Diese Kennwortänderung führt dazu, dass die Kennworthashes für die Kerberos- und NTLM-Authentifizierung in Azure AD generiert und gespeichert werden.

Für Benutzer, die mit Azure AD Connect aus einer lokalen AD DS-Umgebung synchronisiert werden, [aktivieren Sie die Synchronisierung von Kennworthashes][hybrid-phs].

> [!IMPORTANT]
> Azure AD Connect synchronisiert Legacykennworthashes nur, wenn Sie Azure AD DS für Ihren Azure AD-Mandanten aktivieren. Legacykennworthashes werden nicht verwendet, wenn Sie Azure AD Connect nur zum Synchronisieren einer lokalen AD DS-Umgebung mit Azure AD verwenden.
>
> Wenn Ihre Legacyanwendungen keine NTLM-Authentifizierung oder einfachen LDAP-Bindungen verwenden, empfiehlt es sich, die NTLM-Kennworthashsynchronisierung für Azure AD DS zu deaktivieren. Weitere Informationen finden Sie unter [Deaktivieren von schwachen Verschlüsselungssammlungen und der Synchronisierung von NTLM-Anmeldeinformationshashes][secure-domain].

Nach entsprechender Konfiguration werden die verwendbaren Kennworthashes in der verwalteten Azure AD DS-Domäne gespeichert. Wenn Sie die verwaltete Azure AD DS-Domäne löschen, werden alle zu diesem Zeitpunkt gespeicherten Kennworthashes ebenfalls gelöscht. Synchronisierte Anmeldeinformationen in Azure AD können nicht wiederverwendet werden, wenn Sie später eine verwaltete Azure AD DS-Domäne erstellen. Sie müssen die Kennworthashsynchronisierung erneut konfigurieren, um die Kennworthashes wieder zu speichern. VMs oder Benutzer, die zuvor in eine Domäne eingebunden wurden, können sich nicht sofort authentifizieren, weil Azure AD die Kennworthashes in der neuen verwalteten Azure AD DS-Domäne generieren und speichern muss. Weitere Informationen finden Sie unter [Prozess der Kennworthashsynchronisierung für Azure AD DS und Azure AD Connect][azure-ad-password-sync].

> [!IMPORTANT]
> Azure AD Connect sollte nur für die Synchronisierung mit lokalen AD DS-Umgebungen installiert und konfiguriert werden. Die Installation von Azure AD Connect in einer verwalteten Azure AD DS-Domäne zur erneuten Synchronisierung von Objekten mit Azure AD wird nicht unterstützt.

## <a name="forests-and-trusts"></a>Gesamtstrukturen und Vertrauensstellungen

Eine *Gesamtstruktur* ist ein logisches Konstrukt, das von Active Directory Domain Services (AD DS) zum Gruppieren von *Domänen* verwendet wird. Die Domänen speichern dann Objekte für Benutzer oder Gruppen und stellen Authentifizierungsdienste bereit.

In Azure AD DS enthält die Gesamtstruktur nur eine einzelne Domäne. Lokale AD DS-Gesamtstrukturen enthalten häufig mehrere Domänen. In großen Unternehmen sind, insbesondere nach Fusionen und Übernahmen, unter Umständen mehrere lokale Gesamtstrukturen vorhanden, die jeweils mehrere Domänen enthalten.

Eine verwaltete Azure AD DS-Domäne wird standardmäßig als *Benutzergesamtstruktur* erstellt. Mit dieser Art von Gesamtstruktur werden alle Objekte aus Azure AD synchronisiert – einschließlich aller in einer lokalen AD DS-Umgebung erstellten Benutzerkonten. Benutzerkonten können sich direkt bei der verwalteten Azure AD DS-Domäne authentifizieren, um sich beispielsweise bei einem in eine Domäne eingebundenen virtuellen Computer anzumelden. Eine Benutzergesamtstruktur ist geeignet, wenn die Kennworthashes synchronisiert werden können und Benutzer keine exklusiven Anmeldemethoden wie etwa die Smartcard-Authentifizierung verwenden.

In einer *Ressourcengesamtstruktur* für Azure AD DS authentifizieren sich Benutzer über eine unidirektionale Gesamtstruktur-*Vertrauensstellung* von der lokalen AD DS-Instanz aus. Hierbei werden die Benutzerobjekte und Kennworthashes nicht mit Azure AD DS synchronisiert. Die Benutzerobjekte und Anmeldeinformationen sind nur in der lokalen AD DS-Instanz vorhanden. Mit diesem Ansatz können Unternehmen Ressourcen und Anwendungsplattformen in Azure hosten, die eine klassische Authentifizierung (beispielsweise LDAPS, Kerberos oder NTLM) benötigen, und gleichzeitig sämtliche Authentifizierungsprobleme oder -bedenken beseitigen. Azure AD DS-Ressourcengesamtstrukturen befinden sich derzeit in der Vorschauphase.

Weitere Informationen zu Gesamtstrukturtypen in Azure AD DS finden Sie unter [Was sind Ressourcengesamtstrukturen?][concepts-forest] sowie unter [Wie funktionieren Gesamtstruktur-Vertrauensstellungen in Azure AD DS?][concepts-trust].

## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie als Einstieg [eine verwaltete Azure AD DS-Domäne][create-instance].

<!-- INTERNAL LINKS -->
[password-policy]: password-policy.md
[hybrid-phs]: tutorial-configure-password-hash-sync.md#enable-synchronization-of-password-hashes
[secure-domain]: secure-your-domain.md
[azure-ad-password-sync]: ../active-directory/hybrid/how-to-connect-password-hash-synchronization.md#password-hash-sync-process-for-azure-ad-domain-services
[create-instance]: tutorial-create-instance.md
[tutorial-create-instance-advanced]: tutorial-create-instance-advanced.md
[concepts-forest]: concepts-resource-forest.md
[concepts-trust]: concepts-forest-trust.md
