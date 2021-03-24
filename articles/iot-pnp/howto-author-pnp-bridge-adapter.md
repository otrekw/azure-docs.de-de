---
title: Erstellen eines Adapters für die IoT Plug & Play-Bridge | Microsoft-Dokumentation
description: Identifizieren Sie die IoT Plug & Play-Bridge-Adapterkomponenten. Erfahren Sie, wie Sie die Bridge durch Schreiben Ihres eigenen Adapters erweitern.
author: usivagna
ms.author: ugans
ms.date: 1/20/2021
ms.topic: how-to
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 9a7028dfaeb94e87366de7acfa8cebc4c2f4c767
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98746810"
---
# <a name="extend-the-iot-plug-and-play-bridge"></a>Erweitern der IoT Plug & Play-Bridge
Mit der [IoT Plug & Play-Bridge](concepts-iot-pnp-bridge.md#iot-plug-and-play-bridge-architecture) können Sie die vorhandenen Geräte, die an ein Gateway angeschlossen sind, an Ihre IoT-Hub-Instanz anschließen. Mit der Bridge ordnen Sie angeschlossenen Geräten IoT Plug & Play-Schnittstellen zu. Mit einer IoT Plug & Play-Schnittstelle werden die Telemetriedaten, die ein Gerät sendet, die Eigenschaften, die zwischen dem Gerät und der Cloud synchronisiert werden, und die Befehle, auf die das Gerät reagiert, definiert. Die Open-Source-Bridge-Anwendung kann auf Windows- oder Linux-Gateways installiert und konfiguriert werden. Außerdem kann die Bridge als Azure IoT Edge-Runtimemodul ausgeführt werden.

In diesem Artikel wird Folgendes ausführlich erläutert:

- Erweitern der IoT Plug & Play-Bridge mit einem Adapter.
- Implementieren der gängigen Rückrufe für einen Bridge-Adapter.

Ein einfaches Beispiel für die Verwendung der Bridge finden Sie unter [Herstellen einer Verbindung eines IoT Plug & Play-Bridge-Beispiels unter Linux oder Windows mit IoT-Hub](howto-use-iot-pnp-bridge.md).

Bei dem Leitfaden und den Beispielen in diesem Artikel werden grundlegende Kenntnisse in [Azure Digital Twins](../digital-twins/overview.md) und [IoT Plug & Play](overview-iot-plug-and-play.md) vorausgesetzt. Außerdem wird in diesem Artikel vorausgesetzt, dass Sie mit dem [Erstellen und Bereitstellen der IoT Plug & Play Bridge](howto-build-deploy-extend-pnp-bridge.md) vertraut sind.

## <a name="design-guide-to-extend-the-iot-plug-and-play-bridge-with-an-adapter"></a>Entwurfshandbuch zum Erweitern der IoT Plug & Play-Bridge mit einem Adapter

Zur Erweiterung der Bridge-Funktionen können Sie eigene Bridge-Adapter erstellen.

Adapter werden von der Bridge für Folgendes verwendet:

- Herstellen einer Verbindung zwischen einem Gerät und der Cloud
- Aktivieren des Datenflusses zwischen einem Gerät und der Cloud
- Aktivieren der Geräteverwaltung über die Cloud

Für alle Bridge-Adapter müssen folgende Aktionen durchgeführt werden:

- Erstellen einer Schnittstelle für digitale Zwillinge
- Verwenden der Schnittstelle zum Binden von geräteseitigen Funktionen an cloudbasierte Funktionen wie Telemetriedaten, Eigenschaften und Befehle
- Herstellen einer Steuerungs- und Datenverbindung mit der Gerätehardware oder -firmware.

Jeder Bridge-Adapter interagiert mit einem bestimmten Gerätetyp, je nachdem, wie der Adapter die Verbindung herstellt und mit dem Gerät interagiert. Auch wenn für die Kommunikation mit einem Gerät ein Handshakeprotokoll verwendet wird, gibt es für einen Bridge-Adapter mehrere Möglichkeiten, die vom Gerät stammenden Daten zu interpretieren. In diesem Szenario werden vom Bridge-Adapter Informationen für den Adapter in der Konfigurationsdatei verwendet, um die *Schnittstellenkonfiguration* zu bestimmen, die vom Adapter zum Analysieren der Daten verwendet werden soll.

Für die Interaktion mit dem Gerät werden vom Bridge-Adapter neben einem vom Gerät unterstützten Kommunikationsprotokoll auch APIs verwendet, die entweder vom zugrunde liegenden Betriebssystem oder vom Gerätehersteller bereitgestellt werden.

Für die Interaktion mit der Cloud werden vom Bridge-Adapter APIs verwendet, die vom C SDK des Azure IoT-Geräts bereitgestellt werden, um Telemetriedaten zu senden, Schnittstellen für digitale Zwillinge zu erstellen, Eigenschaftsaktualisierungen zu senden und Rückruffunktionen für Eigenschaftsaktualisierungen und Befehle zu erstellen.

### <a name="create-a-bridge-adapter"></a>Erstellen eines Bridge-Adapters

Von der Bridge wird erwartet, dass die in der [_PNP_ADAPTER](https://github.com/Azure/iot-plug-and-play-bridge/blob/9964f7f9f77ecbf4db3b60960b69af57fd83a871/pnpbridge/src/pnpbridge/inc/pnpadapter_api.h#L296)-Schnittstelle definierten APIs vom Bridge-Adapter implementiert werden:

```c
typedef struct _PNP_ADAPTER {
  // Identity of the IoT Plug and Play adapter that is retrieved from the config
  const char* identity;

  PNPBRIDGE_ADAPTER_CREATE createAdapter;
  PNPBRIDGE_COMPONENT_CREATE createPnpComponent;
  PNPBRIDGE_COMPONENT_START startPnpComponent;
  PNPBRIDGE_COMPONENT_STOP stopPnpComponent;
  PNPBRIDGE_COMPONENT_DESTROY destroyPnpComponent;
  PNPBRIDGE_ADAPTER_DESTOY destroyAdapter;
} PNP_ADAPTER, * PPNP_ADAPTER;
```

Bei dieser Schnittstelle gilt Folgendes:

- Mit `PNPBRIDGE_ADAPTER_CREATE` werden der Adapter erstellt und die Verwaltungsressourcen der Schnittstelle eingerichtet. Ein Adapter kann bei der Adaptererstellung auch auf globale Adapterparameter zurückgreifen. Diese Funktion wird für einen einzelnen Adapter einmal aufgerufen.
- Mit `PNPBRIDGE_COMPONENT_CREATE` werden die Clientschnittstellen des digitalen Zwillings erstellt und die Rückruffunktionen gebunden. Der Kommunikationskanal zum Gerät wird vom Gerät initiiert. Die Ressourcen können vom Adapter eingerichtet werden, um den Telemetriedatenfluss zu aktivieren. Mit dem Melden von Telemetriedaten wird jedoch erst begonnen, nachdem `PNPBRIDGE_COMPONENT_START` aufgerufen wurde. Diese Funktion wird für jede Schnittstellenkomponente in der Konfigurationsdatei einmal aufgerufen.
- `PNPBRIDGE_COMPONENT_START` wird aufgerufen, damit vom Bridge-Adapter mit dem Weiterleiten von Telemetriedaten vom Gerät an den Client des digitalen Zwillings begonnen wird. Diese Funktion wird für jede Schnittstellenkomponente in der Konfigurationsdatei einmal aufgerufen.
- Mit `PNPBRIDGE_COMPONENT_STOP` wird der Telemetriedatenfluss beendet.
- Mit `PNPBRIDGE_COMPONENT_DESTROY` wird der Client des digitalen Zwillings zusammen mit den entsprechenden Schnittstellenressourcen zerstört. Diese Funktion wird für jede Schnittstellenkomponente in der Konfigurationsdatei einmal aufgerufen, wenn die Bridge abgebrochen wird oder ein schwerwiegender Fehler auftritt.
- Mit `PNPBRIDGE_ADAPTER_DESTROY` werden die Bridge-Adapterressourcen bereinigt.

### <a name="bridge-core-interaction-with-bridge-adapters"></a>Bridge-Kerninteraktion mit Bridge-Adaptern

In der folgenden Liste wird beschrieben, was geschieht, wenn die Bridge gestartet wird:

1. Wenn die Bridge gestartet wird, durchsucht der Bridge-Adapter-Manager alle in der Konfigurationsdatei definierten Schnittstellenkomponenten und ruft `PNPBRIDGE_ADAPTER_CREATE` auf dem entsprechenden Adapter auf. Vom Adapter können globale Parameter für die Adapterkonfiguration verwendet werden, um Ressourcen so einzurichten, dass die verschiedenen *Schnittstellenkonfigurationen* unterstützt werden.
1. Für jedes Gerät in der Konfigurationsdatei wird vom Bridge-Manager durch Aufrufen von `PNPBRIDGE_COMPONENT_CREATE` auf dem entsprechenden Bridge-Adapter eine Schnittstellenerstellung initiiert.
1. Vom Adapter werden optionale Adapterkonfigurationseinstellungen für die Schnittstellenkomponente empfangen und mit diesen Informationen eine Verbindung mit dem Gerät eingerichtet.
1. Vom Adapter werden die Clientschnittstellen des digitalen Zwillings erstellt und die Rückruffunktionen für Eigenschaftenaktualisierungen und Befehle gebunden. Durch das Einrichten von Geräteverbindungen sollte die Rückgabe der Rückrufe nach dem Erstellen der Schnittstelle des digitalen Zwillings nicht blockiert werden. Die aktive Geräteverbindung ist unabhängig vom aktiven Schnittstellenclient, der von der Bridge erstellt wird. Wenn eine Verbindung nicht hergestellt werden kann, geht der Adapter davon aus, dass das Gerät inaktiv ist. Der Bridge-Adapter kann erneut versuchen, diese Verbindung herzustellen.
1. Nachdem alle in der Konfigurationsdatei angegebenen Schnittstellenkomponenten vom Bridge-Adapter-Manager erstellt wurden, werden alle Schnittstellen bei Azure IoT Hub registriert. Bei der Registrierung handelt es sich um einen blockierenden, asynchronen Aufruf. Nach Abschluss des Aufrufs wird im Bridge-Adapter ein Rückruf ausgelöst, durch den dann die Verarbeitung von Eigenschafts- und Befehlsrückrufen aus der Cloud begonnen werden kann.
1. Anschließend wird vom Bridge-Adapter-Manager auf jeder Komponente `PNPBRIDGE_INTERFACE_START` aufgerufen, und vom Bridge-Adapter wird mit dem Melden von Telemetriedaten an den Client des digitalen Zwillings begonnen.

### <a name="design-guidelines"></a>Entwurfsrichtlinien

Beachten Sie beim Entwickeln eines neuen Bridge-Adapters die folgenden Richtlinien:

- Legen Sie fest, welche Gerätefunktionen unterstützt werden und wie die Schnittstellendefinition der Komponenten, die diesen Adapter nutzen, aussieht.
- Legen Sie fest, welche Schnittstelle und globalen Parameter für Ihren Adapter in der Konfigurationsdatei definiert werden müssen.
- Entscheiden Sie, welche Low-Level-Gerätekommunikation zur Unterstützung der Komponenteneigenschaften und Befehle erforderlich ist.
- Legen Sie fest, wie der Adapter die Rohdaten vom Gerät analysieren und in die Telemetriedatentypen konvertieren soll, die in der IoT Plug & Play-Schnittstellendefinition angegeben sind.
- Implementieren Sie die weiter oben beschriebene Bridge-Adapterschnittstelle.
- Fügen Sie dem Adaptermanifest den neuen Adapter hinzu, und erstellen Sie die Bridge.

### <a name="enable-a-new-bridge-adapter"></a>Aktivieren des neuen Bridge-Adapters

Adapter werden in der Bridge durch Hinzufügen eines Verweises in [adapter_manifest.c](https://github.com/Azure/iot-plug-and-play-bridge/blob/master/pnpbridge/src/adapters/src/shared/adapter_manifest.c) aktiviert:

```c
  extern PNP_ADAPTER MyPnpAdapter;
  PPNP_ADAPTER PNP_ADAPTER_MANIFEST[] = {
    .
    .
    &MyPnpAdapter
  }
```

> [!IMPORTANT]
> Bridge-Adapterrückrufe werden nacheinander aufgerufen. Rückrufe dürfen von einem Adapter nicht blockiert werden, da dadurch verhindert wird, dass der Bridge-Kern Fortschritte macht.

### <a name="sample-camera-adapter"></a>Beispiel für einen Kameraadapter

In der [Infodatei zum Kameraadapter](https://github.com/Azure/iot-plug-and-play-bridge/blob/master/pnpbridge/src/adapters/src/Camera/readme.md) wird ein Beispiel für einen Kameraadapter beschrieben, den Sie aktivieren können.

## <a name="code-examples-for-common-adapter-scenarioscallbacks"></a>Codebeispiele für gängige Adapterszenarios/Rückrufe

Der folgende Abschnitt enthält ausführliche Informationen dazu, wie ein Adapter für die Bridge Rückrufe für eine Reihe allgemeiner Szenarien und Nutzungen implementiert. Die folgenden Rückrufe werden in diesem Abschnitt behandelt:
- [Empfangen einer Eigenschaftsaktualisierung (Cloud an Gerät)](#receive-property-update-cloud-to-device)
- [Berichten einer Eigenschaftsaktualisierung (Gerät an Cloud)](#report-a-property-update-device-to-cloud)
- [Senden von Telemetriedaten (Gerät an Cloud)](#send-telemetry-device-to-cloud)
- [Empfangen eines Befehlsaktualisierungs-Rückrufs aus der Cloud und dessen geräteseitige Verarbeitung (Cloud an Gerät)](#receive-command-update-callback-from-the-cloud-and-process-it-on-the-device-side-cloud-to-device)
- [Reagieren auf das Befehlsupdate auf der Geräteseite (Gerät an Cloud)](#respond-to-command-update-on-the-device-side-device-to-cloud)

Die folgenden Beispiele basieren auf dem [Umgebungssensor-Beispieladapter](https://github.com/Azure/iot-plug-and-play-bridge/tree/master/pnpbridge/src/adapters/samples/environmental_sensor).

### <a name="receive-property-update-cloud-to-device"></a>Empfangen einer Eigenschaftsaktualisierung (Cloud an Gerät)
Der erste Schritt ist, eine Rückruffunktion zu registrieren:

```c
PnpComponentHandleSetPropertyUpdateCallback(BridgeComponentHandle, EnvironmentSensor_ProcessPropertyUpdate);
```
Im nächsten Schritt wird die Rückruffunktion implementiert, um die Eigenschaftsaktualisierung auf dem Gerät zu lesen:

```c
void EnvironmentSensor_ProcessPropertyUpdate(
    PNPBRIDGE_COMPONENT_HANDLE PnpComponentHandle,
    const char* PropertyName,
    JSON_Value* PropertyValue,
    int version,
    void* userContextCallback
)
{
  // User context for the callback is set to the IoT Hub client handle, and therefore can be type-cast to the client handle type
    SampleEnvironmentalSensor_ProcessPropertyUpdate(userContextCallback, PropertyName, PropertyValue, version, PnpComponentHandle);
}

// SampleEnvironmentalSensor_ProcessPropertyUpdate receives updated properties from the server.  This implementation
// acts as a simple dispatcher to the functions to perform the actual processing.
void SampleEnvironmentalSensor_ProcessPropertyUpdate(
    void * ClientHandle,
    const char* PropertyName,
    JSON_Value* PropertyValue,
    int version,
    PNPBRIDGE_COMPONENT_HANDLE PnpComponentHandle)
{
  if (strcmp(PropertyName, sampleEnvironmentalSensorPropertyBrightness) == 0)
    {
        SampleEnvironmentalSensor_BrightnessCallback(ClientHandle, PropertyName, PropertyValue, version, PnpComponentHandle);
    }
    else
    {
        // If the property is not implemented by this interface, presently we only record a log message but do not have a mechanism to report back to the service
        LogError("Environmental Sensor Adapter:: Property name <%s> is not associated with this interface", PropertyName);
    }
}

// Process a property update for bright level.
static void SampleEnvironmentalSensor_BrightnessCallback(
    void * ClientHandle,
    const char* PropertyName,
    JSON_Value* PropertyValue,
    int version,
    PNPBRIDGE_COMPONENT_HANDLE PnpComponentHandle)
{
    IOTHUB_CLIENT_RESULT iothubClientResult;
    STRING_HANDLE jsonToSend = NULL;
    char targetBrightnessString[32];

    LogInfo("Environmental Sensor Adapter:: Brightness property invoked...");

    PENVIRONMENT_SENSOR EnvironmentalSensor = PnpComponentHandleGetContext(PnpComponentHandle);

    if (json_value_get_type(PropertyValue) != JSONNumber)
    {
        LogError("JSON field %s is not a number", PropertyName);
    }
    else if(EnvironmentalSensor == NULL || EnvironmentalSensor->SensorState == NULL)
    {
        LogError("Environmental sensor device context not initialized correctly.");
    }
    else if (SampleEnvironmentalSensor_ValidateBrightness(json_value_get_number(PropertyValue)))
    {
        EnvironmentalSensor->SensorState->brightness = (int) json_value_get_number(PropertyValue);
        if (snprintf(targetBrightnessString, sizeof(targetBrightnessString), 
            g_environmentalSensorBrightnessResponseFormat, EnvironmentalSensor->SensorState->brightness) < 0)
        {
            LogError("Unable to create target brightness string for reporting result");
        }
        else if ((jsonToSend = PnP_CreateReportedPropertyWithStatus(EnvironmentalSensor->SensorState->componentName,
                    PropertyName, targetBrightnessString, PNP_STATUS_SUCCESS, g_environmentalSensorPropertyResponseDescription,
                    version)) == NULL)
        {
            LogError("Unable to build reported property response");
        }
        else
        {
            const char* jsonToSendStr = STRING_c_str(jsonToSend);
            size_t jsonToSendStrLen = strlen(jsonToSendStr);

            if ((iothubClientResult = SampleEnvironmentalSensor_RouteReportedState(ClientHandle, PnpComponentHandle, (const unsigned char*)jsonToSendStr, jsonToSendStrLen,
                                        SampleEnvironmentalSensor_PropertyCallback,
                                        (void*) &EnvironmentalSensor->SensorState->brightness)) != IOTHUB_CLIENT_OK)
            {
                LogError("Environmental Sensor Adapter:: SampleEnvironmentalSensor_RouteReportedState for brightness failed, error=%d", iothubClientResult);
            }
            else
            {
                LogInfo("Environmental Sensor Adapter:: Successfully queued Property update for Brightness for component=%s", EnvironmentalSensor->SensorState->componentName);
            }

            STRING_delete(jsonToSend);
        }
    }
}

```

### <a name="report-a-property-update-device-to-cloud"></a>Berichten einer Eigenschaftsaktualisierung (Gerät an Cloud)
Zu einem beliebigen Zeitpunkt nach dem Erstellen der Komponente kann Ihr Gerät Eigenschaften mit Status an die Cloud melden: 
```c
// Environmental sensor's read-only property, device state indiciating whether its online or not
//
static const char sampleDeviceStateProperty[] = "state";
static const unsigned char sampleDeviceStateData[] = "true";
static const int sampleDeviceStateDataLen = sizeof(sampleDeviceStateData) - 1;

// Sends a reported property for device state of this simulated device.
IOTHUB_CLIENT_RESULT SampleEnvironmentalSensor_ReportDeviceStateAsync(
    PNPBRIDGE_COMPONENT_HANDLE PnpComponentHandle,
    const char * ComponentName)
{

    IOTHUB_CLIENT_RESULT iothubClientResult = IOTHUB_CLIENT_OK;
    STRING_HANDLE jsonToSend = NULL;

    if ((jsonToSend = PnP_CreateReportedProperty(ComponentName, sampleDeviceStateProperty, (const char*) sampleDeviceStateData)) == NULL)
    {
        LogError("Unable to build reported property response for propertyName=%s, propertyValue=%s", sampleDeviceStateProperty, sampleDeviceStateData);
    }
    else
    {
        const char* jsonToSendStr = STRING_c_str(jsonToSend);
        size_t jsonToSendStrLen = strlen(jsonToSendStr);

        if ((iothubClientResult = SampleEnvironmentalSensor_RouteReportedState(NULL, PnpComponentHandle, (const unsigned char*)jsonToSendStr, jsonToSendStrLen,
            SampleEnvironmentalSensor_PropertyCallback, (void*)sampleDeviceStateProperty)) != IOTHUB_CLIENT_OK)
        {
            LogError("Environmental Sensor Adapter:: Unable to send reported state for property=%s, error=%d",
                                sampleDeviceStateProperty, iothubClientResult);
        }
        else
        {
            LogInfo("Environmental Sensor Adapter:: Sending device information property to IoTHub. propertyName=%s, propertyValue=%s",
                        sampleDeviceStateProperty, sampleDeviceStateData);
        }

        STRING_delete(jsonToSend);
    }

    return iothubClientResult;
}


// Routes the reported property for device or module client. This function can be called either by passing a valid client handle or by passing
// a NULL client handle after components have been started such that the client handle can be extracted from the PnpComponentHandle
IOTHUB_CLIENT_RESULT SampleEnvironmentalSensor_RouteReportedState(
    void * ClientHandle,
    PNPBRIDGE_COMPONENT_HANDLE PnpComponentHandle,
    const unsigned char * ReportedState,
    size_t Size,
    IOTHUB_CLIENT_REPORTED_STATE_CALLBACK ReportedStateCallback,
    void * UserContextCallback)
{
    IOTHUB_CLIENT_RESULT iothubClientResult = IOTHUB_CLIENT_OK;

    PNP_BRIDGE_CLIENT_HANDLE clientHandle = (ClientHandle != NULL) ?
            (PNP_BRIDGE_CLIENT_HANDLE) ClientHandle : PnpComponentHandleGetClientHandle(PnpComponentHandle);

    if ((iothubClientResult = PnpBridgeClient_SendReportedState(clientHandle, ReportedState, Size,
            ReportedStateCallback, UserContextCallback)) != IOTHUB_CLIENT_OK)
    {
        LogError("IoTHub client call to _SendReportedState failed with error code %d", iothubClientResult);
        goto exit;
    }
    else
    {
        LogInfo("IoTHub client call to _SendReportedState succeeded");
    }

exit:
    return iothubClientResult;
}

```

### <a name="send-telemetry-device-to-cloud"></a>Senden von Telemetriedaten (Gerät an Cloud)
```c
//
// SampleEnvironmentalSensor_SendTelemetryMessagesAsync is periodically invoked by the caller to
// send telemetry containing the current temperature and humidity (in both cases random numbers
// so this sample will work on platforms without these sensors).
//
IOTHUB_CLIENT_RESULT SampleEnvironmentalSensor_SendTelemetryMessagesAsync(
    PNPBRIDGE_COMPONENT_HANDLE PnpComponentHandle)
{
    IOTHUB_CLIENT_RESULT result = IOTHUB_CLIENT_OK;
    IOTHUB_MESSAGE_HANDLE messageHandle = NULL;
    PENVIRONMENT_SENSOR device = PnpComponentHandleGetContext(PnpComponentHandle);

    float currentTemperature = 20.0f + ((float)rand() / RAND_MAX) * 15.0f;
    float currentHumidity = 60.0f + ((float)rand() / RAND_MAX) * 20.0f;

    char currentMessage[128];
    sprintf(currentMessage, "{\"%s\":%.3f, \"%s\":%.3f}", SampleEnvironmentalSensor_TemperatureTelemetry, 
            currentTemperature, SampleEnvironmentalSensor_HumidityTelemetry, currentHumidity);


    if ((messageHandle = PnP_CreateTelemetryMessageHandle(device->SensorState->componentName, currentMessage)) == NULL)
    {
        LogError("Environmental Sensor Adapter:: PnP_CreateTelemetryMessageHandle failed.");
    }
    else if ((result = SampleEnvironmentalSensor_RouteSendEventAsync(PnpComponentHandle, messageHandle,
            SampleEnvironmentalSensor_TelemetryCallback, device)) != IOTHUB_CLIENT_OK)
    {
        LogError("Environmental Sensor Adapter:: SampleEnvironmentalSensor_RouteSendEventAsync failed, error=%d", result);
    }

    IoTHubMessage_Destroy(messageHandle);

    return result;
}

// Routes the sending asynchronous events for device or module client
IOTHUB_CLIENT_RESULT SampleEnvironmentalSensor_RouteSendEventAsync(
        PNPBRIDGE_COMPONENT_HANDLE PnpComponentHandle,
        IOTHUB_MESSAGE_HANDLE EventMessageHandle,
        IOTHUB_CLIENT_EVENT_CONFIRMATION_CALLBACK EventConfirmationCallback,
        void * UserContextCallback)
{
    IOTHUB_CLIENT_RESULT iothubClientResult = IOTHUB_CLIENT_OK;
    PNP_BRIDGE_CLIENT_HANDLE clientHandle = PnpComponentHandleGetClientHandle(PnpComponentHandle);
    if ((iothubClientResult = PnpBridgeClient_SendEventAsync(clientHandle, EventMessageHandle,
            EventConfirmationCallback, UserContextCallback)) != IOTHUB_CLIENT_OK)
    {
        LogError("IoTHub client call to _SendEventAsync failed with error code %d", iothubClientResult);
        goto exit;
    }
    else
    {
        LogInfo("IoTHub client call to _SendEventAsync succeeded");
    }

exit:
    return iothubClientResult;
}

```
### <a name="receive-command-update-callback-from-the-cloud-and-process-it-on-the-device-side-cloud-to-device"></a>Empfangen eines Befehlsaktualisierungs-Rückrufs aus der Cloud und dessen geräteseitige Verarbeitung (Cloud an Gerät)
```c
// SampleEnvironmentalSensor_ProcessCommandUpdate receives commands from the server.  This implementation acts as a simple dispatcher
// to the functions to perform the actual processing.
int SampleEnvironmentalSensor_ProcessCommandUpdate(
    PENVIRONMENT_SENSOR EnvironmentalSensor,
    const char* CommandName,
    JSON_Value* CommandValue,
    unsigned char** CommandResponse,
    size_t* CommandResponseSize)
{
    if (strcmp(CommandName, sampleEnvironmentalSensorCommandBlink) == 0)
    {
        return SampleEnvironmentalSensor_BlinkCallback(EnvironmentalSensor, CommandValue, CommandResponse, CommandResponseSize);
    }
    else if (strcmp(CommandName, sampleEnvironmentalSensorCommandTurnOn) == 0)
    {
        return SampleEnvironmentalSensor_TurnOnLightCallback(EnvironmentalSensor, CommandValue, CommandResponse, CommandResponseSize);
    }
    else if (strcmp(CommandName, sampleEnvironmentalSensorCommandTurnOff) == 0)
    {
        return SampleEnvironmentalSensor_TurnOffLightCallback(EnvironmentalSensor, CommandValue, CommandResponse, CommandResponseSize);
    }
    else
    {
        // If the command is not implemented by this interface, by convention we return a 404 error to server.
        LogError("Environmental Sensor Adapter:: Command name <%s> is not associated with this interface", CommandName);
        return SampleEnvironmentalSensor_SetCommandResponse(CommandResponse, CommandResponseSize, sampleEnviromentalSensor_NotImplemented);
    }
}

// Implement the callback to process the command "blink". Information pertaining to the request is
// specified in the CommandValue parameter, and the callback fills out data it wishes to
// return to the caller on the service in CommandResponse.

static int SampleEnvironmentalSensor_BlinkCallback(
    PENVIRONMENT_SENSOR EnvironmentalSensor,
    JSON_Value* CommandValue,
    unsigned char** CommandResponse,
    size_t* CommandResponseSize)
{
    int result = PNP_STATUS_SUCCESS;
    int BlinkInterval = 0;

    LogInfo("Environmental Sensor Adapter:: Blink command invoked. It has been invoked %d times previously", EnvironmentalSensor->SensorState->numTimesBlinkCommandCalled);

    if (json_value_get_type(CommandValue) != JSONNumber)
    {
        LogError("Cannot retrieve blink interval for blink command");
        result = PNP_STATUS_BAD_FORMAT;
    }
    else
    {
        BlinkInterval = (int)json_value_get_number(CommandValue);
        LogInfo("Environmental Sensor Adapter:: Blinking with interval=%d second(s)", BlinkInterval);
        EnvironmentalSensor->SensorState->numTimesBlinkCommandCalled++;
        EnvironmentalSensor->SensorState->blinkInterval = BlinkInterval;

        result = SampleEnvironmentalSensor_SetCommandResponse(CommandResponse, CommandResponseSize, sampleEnviromentalSensor_BlinkResponse);
    }

    return result;
}

```
### <a name="respond-to-command-update-on-the-device-side-device-to-cloud"></a>Reagieren auf das Befehlsupdate auf der Geräteseite (Gerät an Cloud)

```c
    static int SampleEnvironmentalSensor_BlinkCallback(
        PENVIRONMENT_SENSOR EnvironmentalSensor,
        JSON_Value* CommandValue,
        unsigned char** CommandResponse,
        size_t* CommandResponseSize)
    {
        int result = PNP_STATUS_SUCCESS;
        int BlinkInterval = 0;
    
        LogInfo("Environmental Sensor Adapter:: Blink command invoked. It has been invoked %d times previously", EnvironmentalSensor->SensorState->numTimesBlinkCommandCalled);
    
        if (json_value_get_type(CommandValue) != JSONNumber)
        {
            LogError("Cannot retrieve blink interval for blink command");
            result = PNP_STATUS_BAD_FORMAT;
        }
        else
        {
            BlinkInterval = (int)json_value_get_number(CommandValue);
            LogInfo("Environmental Sensor Adapter:: Blinking with interval=%d second(s)", BlinkInterval);
            EnvironmentalSensor->SensorState->numTimesBlinkCommandCalled++;
            EnvironmentalSensor->SensorState->blinkInterval = BlinkInterval;
    
            result = SampleEnvironmentalSensor_SetCommandResponse(CommandResponse, CommandResponseSize, sampleEnviromentalSensor_BlinkResponse);
        }
    
        return result;
    }
    
    // SampleEnvironmentalSensor_SetCommandResponse is a helper that fills out a command response
    static int SampleEnvironmentalSensor_SetCommandResponse(
        unsigned char** CommandResponse,
        size_t* CommandResponseSize,
        const unsigned char* ResponseData)
    {
        int result = PNP_STATUS_SUCCESS;
        if (ResponseData == NULL)
        {
            LogError("Environmental Sensor Adapter:: Response Data is empty");
            *CommandResponseSize = 0;
            return PNP_STATUS_INTERNAL_ERROR;
        }
    
        *CommandResponseSize = strlen((char*)ResponseData);
        memset(CommandResponse, 0, sizeof(*CommandResponse));
    
        // Allocate a copy of the response data to return to the invoker. Caller will free this.
        if (mallocAndStrcpy_s((char**)CommandResponse, (char*)ResponseData) != 0)
        {
            LogError("Environmental Sensor Adapter:: Unable to allocate response data");
            result = PNP_STATUS_INTERNAL_ERROR;
        }
    
        return result;
}
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur IoT Plug & Play-Bridge finden Sie im GitHub-Repository zur [IoT Plug & Play-Bridge](https://github.com/Azure/iot-plug-and-play-bridge).
