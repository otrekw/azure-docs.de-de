---
title: Lokalisierungszeichenfolgen-IDs – Azure Active Directory B2C | Microsoft-Dokumentation
description: Geben Sie die IDs für eine Inhaltsdefinition mit der ID „api.signuporsignin“ in einer benutzerdefinierten Richtlinie in Azure Active Directory B2C an.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 07/10/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 611d676f5f588ff32f981692456160e269642a43
ms.sourcegitcommit: 0b8320ae0d3455344ec8855b5c2d0ab3faa974a3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2020
ms.locfileid: "87428130"
---
# <a name="localization-string-ids"></a>Lokalisierungszeichenfolgen-IDs

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Mithilfe des **Localization**-Elements können Sie mehrere Gebietsschemas oder Sprachen in der Richtlinie für die User Journeys unterstützen. Dieser Artikel enthält die Liste mit Lokalisierungs-IDs, die Sie in Ihrer Richtlinie verwenden können. Informationen zur Benutzeroberflächenlokalisierung finden Sie unter [Lokalisierung](localization.md).

## <a name="sign-up-or-sign-in-page-elements"></a>Elemente auf der Anmelde- oder Registrierungsseite

Die folgenden IDs werden für eine Inhaltsdefinition mit der ID `api.signuporsignin` verwendet.

| id | Standardwert |
| -- | ------------- |
| **local_intro_email** | Melden Sie sich mit Ihrem vorhandenen Konto an. |
| **logonIdentifier_email** | E-Mail-Adresse |
| **requiredField_email** | Geben Sie Ihre E-Mail-Adresse ein. |
| **invalid_email** | Geben Sie eine gültige E-Mail-Adresse ein. |
| **email_pattern** | ^[a-zA-Z0-9.!#$%&'' *+/=?^_\`{\|}~-]+@[a-zA-Z0-9-]+(?:\\.[a-zA-Z0-9-]+)* $ |
| **local_intro_username** | Melden Sie sich mit Ihrem Benutzernamen an. |
| **logonIdentifier_username** | Username |
| **requiredField_username** | Geben Sie Ihren Benutzernamen ein. |
| **password** | Kennwort |
| **requiredField_password** | Geben Sie Ihr Kennwort ein. |
| **invalid_password** | Das eingegebene Kennwort entspricht nicht dem erwarteten Format. |
| **forgotpassword_link** | Forgot your password? (Kennwort vergessen?) |
| **createaccount_intro** | Sie haben kein Konto? |
| **createaccount_link** | Jetzt anmelden |
| **divider_title** | oder |
| **cancel_message** | Der Benutzer hat sein Kennwort vergessen. |
| **button_signin** | Anmelden |
| **social_intro** | Melden Sie sich mit Ihrem Konto für ein soziales Netzwerk an. |
  **remember_me** |Angemeldet bleiben|
| **unknown_error** | Wir können Sie nicht anmelden. Versuchen Sie es später noch mal. |

Die folgenden Beispiele zeigen die Verwendung einiger Benutzeroberflächenelemente auf der Anmelde- oder Registrierungsseite:

![UX-Elemente auf der Anmelde- oder Registrierungsseite](./media/localization-string-ids/localization-susi.png)

Die ID der Identitätsanbieter wird im **ClaimsExchange**-Element der User Journey konfiguriert. Um den Titel des Identitätsanbieters zu lokalisieren, wird **ElementType** auf `ClaimsProvider` festgelegt, während für **StringId** die ID von `ClaimsExchange` festgelegt wird.

```xml
<OrchestrationStep Order="2" Type="ClaimsExchange">
  <Preconditions>
    <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
      <Value>objectId</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="FacebookExchange" TechnicalProfileReferenceId="Facebook-OAUTH" />
    <ClaimsExchange Id="MicrosoftExchange" TechnicalProfileReferenceId="MSA-OIDC" />
    <ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
    <ClaimsExchange Id="SignUpWithLogonEmailExchange" TechnicalProfileReferenceId="LocalAccount" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Im folgenden Beispiel wird der Facebook-Identitätsanbieter in Arabisch lokalisiert:

```xml
<LocalizedString ElementType="ClaimsProvider" StringId="FacebookExchange">فيس بوك</LocalizedString>
```

### <a name="sign-up-or-sign-in-error-messages"></a>Fehlermeldungen beim Registrieren oder Anmelden

| id | Standardwert |
| -- | ------------- |
| **UserMessageIfInvalidPassword** | Ihr Kennwort ist falsch. |
| **UserMessageIfClaimsPrincipalDoesNotExist** | Wir können Ihr Konto nicht finden. |
| **UserMessageIfOldPasswordUsed** | Sie verwenden offenbar ein altes Kennwort. |
| **DefaultMessage** | Ungültiger Benutzername oder ungültiges Kennwort. |
| **UserMessageIfUserAccountDisabled** | Ihr Konto wurde gesperrt. Wenden Sie sich an einen Supportmitarbeiter, um die Sperre aufheben zu lassen, und wiederholen Sie den Vorgang. |
| **UserMessageIfUserAccountLocked** | Ihr Konto wurde vorübergehend gesperrt, um eine unbefugte Nutzung zu verhindern. Versuchen Sie es später noch einmal. |
| **AADRequestsThrottled** | Derzeit sind zu viele Anforderungen vorhanden. Warten Sie einige Zeit, und versuchen Sie es erneut. |

## <a name="sign-up-and-self-asserted-pages-user-interface-elements"></a>Benutzeroberflächenelemente für Anmeldeseiten und Seiten zur Identitätsüberprüfung

Folgende IDs dienen der Inhaltsdefinition mit der ID `api.localaccountsignup` oder einer Inhaltsdefinition, die mit `api.selfasserted` beginnt, wie etwa `api.selfasserted.profileupdate` und `api.localaccountpasswordreset`.

| id | Standardwert |
| -- | ------------- |
| **ver_sent** | Prüfcode wurde gesendet an: |
| **ver_but_default** | Standard |
| **cancel_message** | Der Benutzer hat die Eingabe von Informationen zur Identitätsüberprüfung abgebrochen. |
| **preloader_alt** | Bitte warten |
| **ver_but_send** | Überprüfungscode senden |
| **alert_yes** | Ja |
| **error_fieldIncorrect** | Mindestens ein Feld wurde falsch ausgefüllt. Überprüfen Sie Ihre Eingaben, und versuchen Sie es erneut. |
| **year** | Jahr |
| **verifying_blurb** | Warten Sie, während wir Ihre Informationen verarbeiten. |
| **button_cancel** | Abbrechen |
| **ver_fail_no_retry** | Es wurden zu viele ungültige Versuche durchgeführt. Versuchen Sie es später noch mal. |
| **month** | Month (Monat) |
| **ver_success_msg** | Die E-Mail-Adresse wurde verifiziert. Sie können den Vorgang jetzt fortsetzen. |
| **months** | Januar, Februar, März, April, Mai, Juni, Juli, August, September, Oktober, November, Dezember |
| **ver_fail_server** | Beim Überprüfen Ihrer E-Mail-Adresse sind Probleme aufgetreten. Geben Sie eine gültige E-Mail-Adresse ein, und versuchen Sie es erneut. |
| **error_requiredFieldMissing** | Ein erforderliches Feld fehlt. Füllen Sie alle erforderlichen Felder aus, und versuchen Sie es erneut. |
| **initial_intro** | Geben Sie die folgenden Details an. |
| **ver_but_resend** | Neuen Code senden |
| **button_continue** | Erstellen |
| **error_passwordEntryMismatch** | Die Kennwörter stimmen nicht überein. Geben Sie in beiden Feldern das gleiche Kennwort ein, und versuchen Sie es erneut. |
| **ver_incorrect_format** | Falsches Format. |
| **ver_but_edit** | E-Mail-Adresse ändern |
| **ver_but_verify** | Code überprüfen |
| **alert_no** | Nein |
| **ver_info_msg** | Der Prüfcode wurde an Ihr Postfach gesendet. Kopieren Sie den Code in das nachstehende Eingabefeld. |
| **day** | Day (Tag) |
| **ver_fail_throttled** | Es wurden zu viele Anforderungen zur Überprüfung dieser E-Mail-Adresse gesendet. Warten Sie einige Minuten, und versuchen Sie es erneut. |
| **helplink_text** | Was ist das? |
| **ver_fail_retry** | Dieser Code ist falsch. Wiederholen Sie den Vorgang. |
| **alert_title** | Eingabe Ihrer Details abbrechen |
| **required_field** | Diese Informationen sind erforderlich. |
| **alert_message** | Möchten Sie die Eingabe der Detailinformationen abbrechen? |
| **ver_intro_msg** | Es ist eine Überprüfung erforderlich. Klicken Sie auf die Schaltfläche „Senden“. |
| **ver_input** | Prüfcode |

### <a name="sign-up-and-self-asserted-pages-error-messages"></a>Fehlermeldungen für Anmeldeseiten und Seiten zur Identitätsüberprüfung

| id | Standardwert |
| -- | ------------- |
| **UserMessageIfClaimsPrincipalAlreadyExists** | Es ist bereits ein Benutzer mit der angegebenen ID vorhanden. Wählen Sie eine andere ID. |
| **UserMessageIfClaimNotVerified** | Anspruch nicht überprüft: {0} |
| **UserMessageIfIncorrectPattern** | Ungültiges Muster für {0}. |
| **UserMessageIfMissingRequiredElement** | Erforderliches Element fehlt: {0} |
| **UserMessageIfValidationError** | Fehler bei der Überprüfung durch {0}. |
| **UserMessageIfInvalidInput** | {0} weist eine ungültige Eingabe auf. |
| **ServiceThrottled** | Derzeit sind zu viele Anforderungen vorhanden. Warten Sie einige Zeit, und versuchen Sie es erneut. |

Die folgenden Beispiele zeigen die Verwendung einiger Benutzeroberflächenelemente auf der Registrierungsseite:

![Registrierungsseite mit bezeichneten Namen der Benutzeroberflächenelemente](./media/localization-string-ids/localization-sign-up.png)

Die folgenden Beispiele zeigen die Verwendung einiger Benutzeroberflächenelemente auf der Registrierungsseite, nachdem der Benutzer auf die Schaltfläche „Überprüfungscode senden“ geklickt hat:

![UX-Elemente zur E-Mail-Überprüfung auf der Registrierungsseite](./media/localization-string-ids/localization-email-verification.png)


## <a name="phone-factor-authentication-page-user-interface-elements"></a>Benutzeroberflächenelemente auf der Seite für die Phone Factor Authentication

Die folgenden IDs werden für eine Inhaltsdefinition mit der ID `api.phonefactor` verwendet.

| id | Standardwert |
| -- | ------------- |
| **button_verify** | Rückruf |
| **country_code_label** | Landesvorwahl |
| **cancel_message** | Der Benutzer hat die mehrstufige Authentifizierung abgebrochen. |
| **text_button_send_second_code** | Neuen Code senden |
| **code_pattern** | \\d{6} |
| **intro_mixed** | Bei uns ist die folgende Nummer für Sie hinterlegt. Wir können Ihnen einen Code per SMS senden oder Sie anrufen, um Sie zu authentifizieren. |
| **intro_mixed_p** | Bei uns sind die folgenden Nummern für Sie hinterlegt. Wählen Sie eine Nummer aus, an die wir per Telefon oder SMS einen Code senden können, um Sie zu authentifizieren. |
| **button_verify_code** | Code überprüfen |
| **requiredField_code** | Geben Sie den Prüfcode ein, den Sie erhalten haben. |
| **invalid_code** | Geben Sie den 6-stelligen Code ein, den Sie erhalten haben. |
| **button_cancel** | Abbrechen |
| **local_number_input_placeholder_text** | Telefonnummer |
| **button_retry** | Wiederholen |
| **alternative_text** | Mein Telefon ist nicht verfügbar. |
| **intro_phone_p** | Bei uns sind die folgenden Nummern für Sie hinterlegt. Wählen Sie eine Nummer aus, die wir anrufen können, um Sie zu authentifizieren. |
| **intro_phone** | Bei uns ist die folgende Nummer für Sie hinterlegt. Wir rufen Sie an, um Sie zu authentifizieren. |
| **enter_code_text_intro** | Geben Sie unten Ihren Prüfcode ein, oder  |
| **intro_entry_phone** | Geben Sie unten eine Nummer ein, die wir anrufen können, um Sie zu authentifizieren. |
| **intro_entry_sms** | Geben Sie unten eine Nummer ein, an die wir per SMS einen Code senden können, um Sie zu authentifizieren. |
| **button_send_code** | Code senden |
| **invalid_number** | Geben Sie eine gültige Telefonnummer ein. |
| **intro_sms** | Bei uns ist die folgende Nummer für Sie hinterlegt. Wir senden einen Code per SMS, um Sie zu authentifizieren. |
| **intro_entry_mixed** | Geben Sie unten eine Nummer ein, damit wir Ihnen einen Code per SMS senden oder Sie anrufen können, um Sie zu authentifizieren. |
| **number_pattern** | ^\\+(?:[0-9][\\x20-]?){6,14}[0-9]$ |
| **intro_sms_p** |Bei uns sind die folgenden Nummern für Sie hinterlegt. Wählen Sie eine Nummer aus, an die wir per SMS einen Code senden können, um Sie zu authentifizieren. |
| **requiredField_countryCode** | Wählen Sie Ihre Landeskennzahl aus. |
| **requiredField_number** | Geben Sie Ihre Telefonnummer ein. |
| **country_code_input_placeholder_text** |Land oder Region |
| **number_label** | Rufnummer |
| **error_tryagain** | Die angegebene Telefonnummer ist besetzt oder nicht erreichbar. Überprüfen Sie die Nummer, und versuchen Sie es erneut. |
| **error_incorrect_code** | Der eingegebene Prüfcode stimmt nicht mit unseren Daten überein. Versuchen Sie es erneut, oder fordern Sie einen neuen Code an. |
| **countryList** | Informationen finden Sie in der [Länderliste](#countries-list). |
| **error_448** | Die angegebene Telefonnummer ist nicht erreichbar. |
| **error_449** | Der Benutzer hat die zulässige Anzahl von Wiederholungsversuchen überschritten. |
| **verification_code_input_placeholder_text** | Prüfcode |

Die folgenden Beispiele zeigen die Verwendung einiger Benutzeroberflächenelemente auf der MFA-Registrierungsseite:

![UX-Elemente zur E-Mail-Überprüfung auf der Registrierungsseite](./media/localization-string-ids/localization-mfa1.png)

Die folgenden Beispiele zeigen die Verwendung einiger Benutzeroberflächenelemente auf der MFA-Überprüfungsseite:

![UX-Elemente zur E-Mail-Überprüfung auf der Registrierungsseite](./media/localization-string-ids/localization-mfa2.png)

## <a name="verification-display-control-user-interface-elements"></a>Anzeigesteuerelemente zur Überprüfung von Benutzeroberflächenelementen

Im Folgenden finden Sie die IDs für ein [Anzeigesteuerelement zur Überprüfung](display-control-verification.md)

| id | Standardwert |
| -- | ------------- |
|verification_control_but_change_claims |Change |
|verification_control_fail_send_code |Der Code konnte nicht gesendet werden, versuchen Sie es später erneut. |
|verification_control_fail_verify_code |Der Code konnte nicht überprüft werden, versuchen Sie es später erneut. |
|verification_control_but_send_code |Code senden |
|verification_control_but_send_new_code |Neuen Code senden |
|verification_control_but_verify_code |Code überprüfen |
|verification_control_code_sent| Prüfcode wurde gesendet. Kopieren Sie den Code in das nachstehende Eingabefeld. |

### <a name="example"></a>Beispiel

```xml
<LocalizedResources Id="api.localaccountsignup.en">
  <LocalizedStrings>
    <LocalizedString ElementType="UxElement" StringId="verification_control_but_change_claims">Change</LocalizedString>
    <LocalizedString ElementType="UxElement" StringId="verification_control_fail_send_code">Failed to send the code, please try again later.</LocalizedString>
    <LocalizedString ElementType="UxElement" StringId="verification_control_fail_verify_code">Failed to verify the code, please try again later.</LocalizedString>
    <LocalizedString ElementType="UxElement" StringId="verification_control_but_send_code">Send Code</LocalizedString>
    <LocalizedString ElementType="UxElement" StringId="verification_control_but_send_new_code">Send New Code</LocalizedString>
    <LocalizedString ElementType="UxElement" StringId="verification_control_but_verify_code">Verify Code</LocalizedString>
    <LocalizedString ElementType="UxElement" StringId="verification_control_code_sent">Verification code has been sent. Please copy it to the input box below.</LocalizedString>
  </LocalizedStrings>
</LocalizedResources>
```

## <a name="restful-service-error-messages"></a>Fehlermeldungen des RESTful-Diensts

Im Folgenden finden Sie die IDs für Fehlermeldungen für das [technische Profil zum RESTful-Dienst](restful-technical-profile.md):

| id | Standardwert |
| -- | ------------- |
|DefaultUserMessageIfRequestFailed | Fehler beim Herstellen einer Verbindung mit dem Endpunkt des RESTful-Diensts. URL des RESTful-Diensts: {0} |
|UserMessageIfCircuitOpen | {0} URL des RESTful-Diensts: {1} |
|UserMessageIfDnsResolutionFailed | Der Hostname des Endpunkts des RESTful-Diensts konnte nicht aufgelöst werden. URL des RESTful-Diensts: {0} |
|UserMessageIfRequestTimeout | Fehler beim Herstellen einer Verbindung mit dem Endpunkt des RESTful-Diensts innerhalb des Zeitlimits von {0} Sekunden. URL des RESTful-Diensts: {1} |


### <a name="example"></a>Beispiel

```xml
<LocalizedResources Id="api.localaccountsignup.en">
  <LocalizedStrings>
    <LocalizedString ElementType="ErrorMessage" StringId="DefaultUserMessageIfRequestFailed">Failed to establish connection to restful service end point.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfCircuitOpen">Unable to connect to the restful service end point.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfDnsResolutionFailed">Failed to resolve the hostname of the restful service endpoint.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfRequestTimeout">Failed to establish connection to restful service end point within timeout limit.</LocalizedString>
  </LocalizedStrings>
</LocalizedResources>
```

## <a name="azure-mfa-error-messages"></a>Azure MFA-Fehlermeldungen

Im Folgenden finden Sie die IDs für Fehlermeldungen für ein [technisches Profil mit Azure MFA](multi-factor-auth-technical-profile.md):

| id | Standardwert |
| -- | ------------- |
|UserMessageIfCouldntSendSms | SMS kann nicht an das Telefon gesendet werden, versuchen Sie es mit einer anderen Telefonnummer. |
|UserMessageIfInvalidFormat | Das Format Ihrer Telefonnummer ist ungültig. Korrigieren Sie sie, und versuchen Sie es erneut.|
|UserMessageIfMaxAllowedCodeRetryReached | Falscher Code wurde zu oft eingegeben, versuchen Sie es später erneut.|
|UserMessageIfServerError | MFA-Dienst kann nicht verwendet werden, versuchen Sie es später erneut.|
|UserMessageIfThrottled | Ihre Anforderung wurde gedrosselt, versuchen Sie es später erneut.|
|UserMessageIfWrongCodeEntered|Falscher Code wurde eingegeben, versuchen Sie es erneut.|

### <a name="example"></a>Beispiel

```xml
<LocalizedResources Id="api.localaccountsignup.en">
  <LocalizedStrings>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfCouldntSendSms">Cannot Send SMS to the phone, please try another phone number.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfInvalidFormat">Your phone number is not in a valid format, please correct it and try again.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfMaxAllowedCodeRetryReached">Wrong code entered too many times, please try again later.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfServerError">Cannot use MFA service, please try again later.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfThrottled">Your request has been throttled, please try again later.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfWrongCodeEntered">Wrong code entered, please try again.</LocalizedString>
  </LocalizedStrings>
</LocalizedResources>
```

## <a name="azure-ad-sspr"></a>Azure AD-Self-Service-Kennwortzurücksetzung

Im Folgenden finden Sie die IDs für Fehlermeldungen für ein [technisches Profil zu Azure AD-SSPR](aad-sspr-technical-profile.md):

| id | Standardwert |
| -- | ------------- |
|UserMessageIfChallengeExpired | Der Code ist abgelaufen.|
|UserMessageIfInternalError | Interner Fehler beim E-Mail-Dienst. Versuchen Sie es später noch einmal.|
|UserMessageIfThrottled | Sie haben zu viele Anforderungen gesendet. Versuchen Sie es später noch einmal.|
|UserMessageIfVerificationFailedNoRetry | Sie haben die maximale Anzahl von Überprüfungsversuchen überschritten.|
|UserMessageIfVerificationFailedRetryAllowed | Fehler bei der Überprüfung. Versuchen Sie es noch einmal.|


### <a name="example"></a>Beispiel

```XML
<LocalizedResources Id="api.localaccountsignup.en">
  <LocalizedStrings>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfInternalError">We are having trouble verifying your email address. Please try again later.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfThrottled">There have been too many requests to verify this email address. Please wait a while, then try again.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfChallengeExpired">That code is expired. Please request a new code.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfVerificationFailedNoRetry">You've made too many incorrect attempts. Please try again later.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfVerificationFailedRetryAllowed">That code is incorrect. Please try again.</LocalizedString>
  </LocalizedStrings>
</LocalizedResources>
```

## <a name="one-time-password-error-messages"></a>Fehlermeldungen für Einmalkennwort

Im Folgenden finden Sie die IDs für Fehlermeldungen für ein [technisches Profil mit Einmalkennwort](one-time-password-technical-profile.md).

| id | Standardwert |
| -- | ------------- |
|UserMessageIfMaxRetryAttempted |Die für das Einmalkennwort bereitgestellte Überprüfung hat die maximale Anzahl von Versuchen überschritten |
|UserMessageIfSessionDoesNotExist |Überprüfungssitzung für Einmalkennwort ist abgelaufen |
|UserMessageIfSessionConflict |Konflikt bei der Überprüfungssitzung für Einmalkennwort |
|UserMessageIfInvalidCode |Das Einmalkennwort für die Überprüfung ist falsch |
|UserMessageIfVerificationFailedRetryAllowed |Dieser Code ist falsch. Wiederholen Sie den Vorgang. | 

### <a name="example"></a>Beispiel

```xml
<LocalizedResources Id="api.localaccountsignup.en">
  <LocalizedStrings>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfSessionDoesNotExist">You have exceed the maximum time allowed.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfMaxRetryAttempted">You have exceed the number of retries allowed.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfInvalidCode">You have entered the wrong code.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfSessionConflict">Cannot verify the code, please try again later.</LocalizedString>
   <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfVerificationFailedRetryAllowed">That code is incorrect. Please try again.</LocalizedString>
  </LocalizedStrings>
</LocalizedResources>
```

## <a name="claims-transformations-error-messages"></a>Fehlermeldungen zu Transformationen von Ansprüchen

Im Folgenden finden Sie die IDs für Fehlermeldungen zu Transformationen von Ansprüchen:

| id | Transformation von Ansprüchen | Standardwert |
| -- | ------------- |------------- |
|UserMessageIfClaimsTransformationBooleanValueIsNotEqual |[AssertBooleanClaimIsEqualToValue](boolean-transformations.md#assertbooleanclaimisequaltovalue) | Fehler bei booleschem Anspruchswertvergleich für Anspruchstyp „inputClaim“.| 
|DateTimeGreaterThan |[AssertDateTimeIsGreaterThan](date-transformations.md#assertdatetimeisgreaterthan) | Fehler bei Anspruchswertvergleich: Der angegebene linke Operand ist größer als der rechte Operand.|
|UserMessageIfClaimsTransformationStringsAreNotEqual |[AssertStringClaimsAreEqual](string-transformations.md#assertstringclaimsareequal) | Fehler bei Anspruchswertvergleich mithilfe von StringComparison „OrdinalIgnoreCase“.|

### <a name="example"></a>Beispiel

```xml
<LocalizedResources Id="api.localaccountsignup.en">
  <LocalizedStrings>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfClaimsTransformationBooleanValueIsNotEqual">Your email address hasn't been verified.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="DateTimeGreaterThan">Expiration date must be greater that the current date.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfClaimsTransformationStringsAreNotEqual">The email entry fields do not match. Please enter the same email address in both fields and try again.</LocalizedString>
  </LocalizedStrings>
</LocalizedResources>
```

## <a name="countries-list"></a>Länderliste

Nachfolgend sind die von der mehrstufigen Authentifizierung verwendeten countryList-Werte aufgeführt:

```JSON
{"DEFAULT":"Country/Region","AF":"Afghanistan","AX":"Åland Islands","AL":"Albania","DZ":"Algeria","AS":"American Samoa","AD":"Andorra","AO":"Angola","AI":"Anguilla","AQ":"Antarctica","AG":"Antigua and Barbuda","AR":"Argentina","AM":"Armenia","AW":"Aruba","AU":"Australia","AT":"Austria","AZ":"Azerbaijan","BS":"Bahamas","BH":"Bahrain","BD":"Bangladesh","BB":"Barbados","BY":"Belarus","BE":"Belgium","BZ":"Belize","BJ":"Benin","BM":"Bermuda","BT":"Bhutan","BO":"Bolivia","BQ":"Bonaire","BA":"Bosnia and Herzegovina","BW":"Botswana","BV":"Bouvet Island","BR":"Brazil","IO":"British Indian Ocean Territory","VG":"British Virgin Islands","BN":"Brunei","BG":"Bulgaria","BF":"Burkina Faso","BI":"Burundi","CV":"Cabo Verde","KH":"Cambodia","CM":"Cameroon","CA":"Canada","KY":"Cayman Islands","CF":"Central African Republic","TD":"Chad","CL":"Chile","CN":"China","CX":"Christmas Island","CC":"Cocos (Keeling) Islands","CO":"Colombia","KM":"Comoros","CG":"Congo","CD":"Congo (DRC)","CK":"Cook Islands","CR":"Costa Rica","CI":"Côte d'Ivoire","HR":"Croatia","CU":"Cuba","CW":"Curaçao","CY":"Cyprus","CZ":"Czech Republic","DK":"Denmark","DJ":"Djibouti","DM":"Dominica","DO":"Dominican Republic","EC":"Ecuador","EG":"Egypt","SV":"El Salvador","GQ":"Equatorial Guinea","ER":"Eritrea","EE":"Estonia","ET":"Ethiopia","FK":"Falkland Islands","FO":"Faroe Islands","FJ":"Fiji","FI":"Finland","FR":"France","GF":"French Guiana","PF":"French Polynesia","TF":"French Southern Territories","GA":"Gabon","GM":"Gambia","GE":"Georgia","DE":"Germany","GH":"Ghana","GI":"Gibraltar","GR":"Greece","GL":"Greenland","GD":"Grenada","GP":"Guadeloupe","GU":"Guam","GT":"Guatemala","GG":"Guernsey","GN":"Guinea","GW":"Guinea-Bissau","GY":"Guyana","HT":"Haiti","HM":"Heard Island and McDonald Islands","HN":"Honduras","HK":"Hong Kong SAR","HU":"Hungary","IS":"Iceland","IN":"India","ID":"Indonesia","IR":"Iran","IQ":"Iraq","IE":"Ireland","IM":"Isle of Man","IL":"Israel","IT":"Italy","JM":"Jamaica","JP":"Japan","JE":"Jersey","JO":"Jordan","KZ":"Kazakhstan","KE":"Kenya","KI":"Kiribati","KR":"Korea","KW":"Kuwait","KG":"Kyrgyzstan","LA":"Laos","LV":"Latvia","LB":"Lebanon","LS":"Lesotho","LR":"Liberia","LY":"Libya","LI":"Liechtenstein","LT":"Lithuania","LU":"Luxembourg","MO":"Macao SAR","MK":"North Macedonia","MG":"Madagascar","MW":"Malawi","MY":"Malaysia","MV":"Maldives","ML":"Mali","MT":"Malta","MH":"Marshall Islands","MQ":"Martinique","MR":"Mauritania","MU":"Mauritius","YT":"Mayotte","MX":"Mexico","FM":"Micronesia","MD":"Moldova","MC":"Monaco","MN":"Mongolia","ME":"Montenegro","MS":"Montserrat","MA":"Morocco","MZ":"Mozambique","MM":"Myanmar","NA":"Namibia","NR":"Nauru","NP":"Nepal","NL":"Netherlands","NC":"New Caledonia","NZ":"New Zealand","NI":"Nicaragua","NE":"Niger","NG":"Nigeria","NU":"Niue","NF":"Norfolk Island","KP":"North Korea","MP":"Northern Mariana Islands","NO":"Norway","OM":"Oman","PK":"Pakistan","PW":"Palau","PS":"Palestinian Authority","PA":"Panama","PG":"Papua New Guinea","PY":"Paraguay","PE":"Peru","PH":"Philippines","PN":"Pitcairn Islands","PL":"Poland","PT":"Portugal","PR":"Puerto Rico","QA":"Qatar","RE":"Réunion","RO":"Romania","RU":"Russia","RW":"Rwanda","BL":"Saint Barthélemy","KN":"Saint Kitts and Nevis","LC":"Saint Lucia","MF":"Saint Martin","PM":"Saint Pierre and Miquelon","VC":"Saint Vincent and the Grenadines","WS":"Samoa","SM":"San Marino","ST":"São Tomé and Príncipe","SA":"Saudi Arabia","SN":"Senegal","RS":"Serbia","SC":"Seychelles","SL":"Sierra Leone","SG":"Singapore","SX":"Sint Maarten","SK":"Slovakia","SI":"Slovenia","SB":"Solomon Islands","SO":"Somalia","ZA":"South Africa","GS":"South Georgia and South Sandwich Islands","SS":"South Sudan","ES":"Spain","LK":"Sri Lanka","SH":"St Helena, Ascension, Tristan da Cunha","SD":"Sudan","SR":"Suriname","SJ":"Svalbard","SZ":"Swaziland","SE":"Sweden","CH":"Switzerland","SY":"Syria","TW":"Taiwan","TJ":"Tajikistan","TZ":"Tanzania","TH":"Thailand","TL":"Timor-Leste","TG":"Togo","TK":"Tokelau","TO":"Tonga","TT":"Trinidad and Tobago","TN":"Tunisia","TR":"Turkey","TM":"Turkmenistan","TC":"Turks and Caicos Islands","TV":"Tuvalu","UM":"U.S. Outlying Islands","VI":"U.S. Virgin Islands","UG":"Uganda","UA":"Ukraine","AE":"United Arab Emirates","GB":"United Kingdom","US":"United States","UY":"Uruguay","UZ":"Uzbekistan","VU":"Vanuatu","VA":"Vatican City","VE":"Venezuela","VN":"Vietnam","WF":"Wallis and Futuna","YE":"Yemen","ZM":"Zambia","ZW":"Zimbabwe"}
```

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Artikel enthalten Lokalisierungsbeispiele:

- [Sprachanpassung mit benutzerdefinierter Richtlinie in Azure Active Directory B2C](custom-policy-localization.md)
- [Sprachanpassung mit Benutzerflows in Azure Active Directory B2C](user-flow-language-customization.md)