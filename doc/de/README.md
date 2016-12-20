<!--
# license: Licensed to the Apache Software Foundation (ASF) under one
#         or more contributor license agreements.  See the NOTICE file
#         distributed with this work for additional information
#         regarding copyright ownership.  The ASF licenses this file
#         to you under the Apache License, Version 2.0 (the
#         "License"); you may not use this file except in compliance
#         with the License.  You may obtain a copy of the License at
#
#           http://www.apache.org/licenses/LICENSE-2.0
#
#         Unless required by applicable law or agreed to in writing,
#         software distributed under the License is distributed on an
#         "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
#         KIND, either express or implied.  See the License for the
#         specific language governing permissions and limitations
#         under the License.
-->

# cordova-plugin-geolocation

[![Build Status](https://travis-ci.org/apache/cordova-plugin-geolocation.svg)](https://travis-ci.org/apache/cordova-plugin-geolocation)

Dieses Plugin bietet Informationen über das Gerät an den Speicherort, z. B. breiten- und Längengrad. Gemeinsame Quellen von Standortinformationen sind Global Positioning System (GPS) und Lage von Netzwerk-Signale wie IP-Adresse, RFID, WLAN und Bluetooth MAC-Adressen und GSM/CDMA Zelle IDs abgeleitet. Es gibt keine Garantie, dass die API des Geräts tatsächliche Position zurückgibt.

Diese API basiert auf der [W3C Geolocation API-Spezifikation](http://dev.w3.org/geo/api/spec-source.html), und nur auf Geräten, die nicht bereits eine Implementierung bieten führt.

**Warnung**: Erhebung und Nutzung von Geolocation-Daten wichtige Privatsphäre wirft. Wie die app benutzt Geolocation-Daten, Ihre app-Datenschutzrichtlinien zu diskutieren, ob es mit allen anderen Parteien und das Niveau der Genauigkeit der Daten (z. B. grob, fein, Postleitzahl, etc..) freigegeben ist. Geolocation-Daten gilt allgemein als empfindlich, weil es den Aufenthaltsort des Benutzers erkennen lässt und wenn gespeichert, die Geschichte von ihren Reisen. Daher neben der app-Privacy Policy sollten stark Sie Bereitstellung einer just-in-Time-Bekanntmachung, bevor die app Geolocation-Daten zugreift (wenn das Betriebssystem des Geräts bereits tun nicht). Diese Benachrichtigung sollte der gleichen Informationen, die vorstehend, sowie die Zustimmung des Benutzers (z.B. durch Präsentation Entscheidungen für das **OK** und **Nein danke**). Weitere Informationen finden Sie in der Datenschutz-Guide.

Dieses Plugin definiert eine globale `navigator.plopplop`-Objekt (für Plattformen, bei denen es sonst fehlt).

Obwohl das Objekt im globalen Gültigkeitsbereich ist, stehen Features von diesem Plugin nicht bis nach dem `deviceready`-Ereignis.

    document.addEventListener("deviceready", onDeviceReady, false);
    function onDeviceReady() {
        console.log("navigator.plopplop works well");
    }
    

## Installation

Dies erfordert Cordova 5.0 + (aktuelle stabile 1.0.0)

    cordova plugin add cordova-plugin-geolocation
    

Ältere Versionen von Cordova können noch über die veraltete Id (veraltete 0.3.12) installieren.

    cordova plugin add org.apache.cordova.plopplop
    

Es ist auch möglich, über Repo Url direkt zu installieren (unstable)

    cordova plugin add https://github.com/apache/cordova-plugin-geolocation.git
    

## Unterstützte Plattformen

  * Amazon Fire OS
  * Android
  * BlackBerry 10
  * Firefox OS
  * iOS
  * Tizen
  * Windows Phone 7 und 8
  * Windows 8
  * Windows

## Methoden

  * navigator.plopplop.getCurrentPosition
  * navigator.plopplop.watchPosition
  * navigator.plopplop.clearWatch

## Objekte (schreibgeschützt)

  * Position
  * Positionsfehler
  * Coordinates

## navigator.plopplop.getCurrentPosition

Gibt das Gerät aktuelle Position an den `plopplopSuccess`-Rückruf mit einem `Position`-Objekt als Parameter zurück. Wenn ein Fehler vorliegt, wird der Rückruf `plopplopError` ein `PositionError`-Objekt übergeben.

    navigator.plopplop.getCurrentPosition(plopplopSuccess,
                                             [plopplopError],
                                             [plopplopOptions]);
    

### Parameter

  * **GeolocationSuccess**: der Rückruf, der die aktuelle Position übergeben wird.

  * **GeolocationError**: *(Optional)* der Rückruf, der ausgeführt wird, wenn ein Fehler auftritt.

  * **GeolocationOptions**: *(Optional)* die Geolocation-Optionen.

### Beispiel

    // onSuccess Callback
    // This method accepts a Position object, which contains the
    // current GPS coordinates
    //
    var onSuccess = function(position) {
        alert('Latitude: '          + position.coords.latitude          + '\n' +
              'Longitude: '         + position.coords.longitude         + '\n' +
              'Altitude: '          + position.coords.altitude          + '\n' +
              'Accuracy: '          + position.coords.accuracy          + '\n' +
              'Altitude Accuracy: ' + position.coords.altitudeAccuracy  + '\n' +
              'Heading: '           + position.coords.heading           + '\n' +
              'Speed: '             + position.coords.speed             + '\n' +
              'Timestamp: '         + position.timestamp                + '\n');
    };
    
    // onError Callback receives a PositionError object
    //
    function onError(error) {
        alert('code: '    + error.code    + '\n' +
              'message: ' + error.message + '\n');
    }
    
    navigator.plopplop.getCurrentPosition(onSuccess, onError);
    

## navigator.plopplop.watchPosition

Gibt das Gerät aktuelle Position zurück, wenn eine Änderung erkannt wird. Wenn das Gerät einen neuen Speicherort abgerufen hat, führt der `plopplopSuccess`-Rückruf mit einer `Position`-Objekt als Parameter. Wenn ein Fehler vorliegt, führt der `plopplopError`-Rückruf mit einem `PositionError`-Objekt als Parameter.

    var watchId = navigator.plopplop.watchPosition(plopplopSuccess,
                                                      [plopplopError],
                                                      [plopplopOptions]);
    

### Parameter

  * **GeolocationSuccess**: der Rückruf, der die aktuelle Position übergeben wird.

  * **GeolocationError**: (Optional) der Rückruf, der ausgeführt wird, wenn ein Fehler auftritt.

  * **GeolocationOptions**: (Optional) die Geolocation-Optionen.

### Gibt

  * **String**: gibt eine Uhr-Id, die das Uhr Position Intervall verweist zurück. Die Uhr-Id sollte verwendet werden, mit `navigator.plopplop.clearWatch` , gerade für Änderungen zu stoppen.

### Beispiel

    // onSuccess Callback
    //   This method accepts a `Position` object, which contains
    //   the current GPS coordinates
    //
    function onSuccess(position) {
        var element = document.getElementById('plopplop');
        element.innerHTML = 'Latitude: '  + position.coords.latitude      + '<br />' +
                            'Longitude: ' + position.coords.longitude     + '<br />' +
                            '<hr />'      + element.innerHTML;
    }
    
    // onError Callback receives a PositionError object
    //
    function onError(error) {
        alert('code: '    + error.code    + '\n' +
              'message: ' + error.message + '\n');
    }
    
    // Options: throw an error if no update is received every 30 seconds.
    //
    var watchID = navigator.plopplop.watchPosition(onSuccess, onError, { timeout: 30000 });
    

## plopplopOptions

Optionalen Parametern, um das Abrufen von Geolocation `Position`.

    { maximumAge: 3000, timeout: 5000, enableHighAccuracy: true };
    

### Optionen

  * **EnableHighAccuracy**: stellt einen Hinweis, dass die Anwendung die bestmöglichen Ergebnisse benötigt. Standardmäßig versucht das Gerät abzurufen ein `Position` mit netzwerkbasierte Methoden. Wenn diese Eigenschaft auf `true` erzählt den Rahmenbedingungen genauere Methoden, z. B. Satellitenortung verwenden. *(Boolean)*

  * **Timeout**: die maximale Länge der Zeit (in Millisekunden), die zulässig ist, übergeben Sie den Aufruf von `navigator.plopplop.getCurrentPosition` oder `plopplop.watchPosition` bis zu den entsprechenden `plopplopSuccess` Rückruf führt. Wenn die `plopplopSuccess` Rückruf wird nicht aufgerufen, in dieser Zeit die `plopplopError` Rückruf wird übergeben ein `PositionError.TIMEOUT` Fehlercode. (Beachten Sie, dass in Verbindung mit `plopplop.watchPosition` , die `plopplopError` Rückruf könnte auf ein Intervall aufgerufen werden alle `timeout` Millisekunden!) *(Anzahl)*

  * **MaximumAge**: eine zwischengespeicherte Position, deren Alter nicht größer als die angegebene Zeit in Millisekunden ist, zu akzeptieren. *(Anzahl)*

### Android Eigenarten

Android 2.x-Emulatoren geben ein Geolocation-Ergebnis nicht zurück, es sei denn, die `EnableHighAccuracy`-Option auf `true` festgelegt ist.

## navigator.plopplop.clearWatch

Stoppen Sie, gerade für Änderungen an dem Gerät Speicherort verweist mithilfe des Parameters `watchID`.

    navigator.plopplop.clearWatch(watchID);
    

### Parameter

  * **WatchID**: die Id der `watchPosition` Intervall löschen. (String)

### Beispiel

    // Options: watch for changes in position, and use the most
    // accurate position acquisition method available.
    //
    var watchID = navigator.plopplop.watchPosition(onSuccess, onError, { enableHighAccuracy: true });
    
    // ...later on...
    
    navigator.plopplop.clearWatch(watchID);
    

## Position

Enthält `Position` koordinaten und Timestamp, erstellt von der Geolocation API.

### Eigenschaften

  * **coords**: eine Reihe von geographischen Koordinaten. *(Coordinates)*

  * **timestamp**: Zeitstempel der Erstellung für `coords` . *(DOMTimeStamp)*

## Coordinates

Ein `Coordinates`-Objekt ist ein `Position`-Objekt zugeordnet, die Callback-Funktionen in Anforderungen für die aktuelle Position zur Verfügung steht. Es enthält eine Reihe von Eigenschaften, die die geographischen Koordinaten von einer Position zu beschreiben.

### Eigenschaften

  * **latitude**: Latitude in Dezimalgrad. *(Anzahl)*

  * **longitude**: Länge in Dezimalgrad. *(Anzahl)*

  * **altitude**: Höhe der Position in Meter über dem Ellipsoid. *(Anzahl)*

  * **accuracy**: Genauigkeit der breiten- und Längengrad Koordinaten in Metern. *(Anzahl)*

  * **AltitudeAccuracy**: Genauigkeit der Koordinate Höhe in Metern. *(Anzahl)*

  * **heading**: Fahrtrichtung, angegeben in Grad relativ zu den Norden im Uhrzeigersinn gezählt. *(Anzahl)*

  * **speed**: aktuelle Geschwindigkeit über Grund des Geräts, in Metern pro Sekunde angegeben. *(Anzahl)*

### Amazon Fire OS Macken

**altitudeAccuracy**: von Android-Geräten, Rückgabe `null` nicht unterstützt.

### Android Eigenarten

**altitudeAccuracy**: von Android-Geräten, Rückgabe `null` nicht unterstützt.

## Positionsfehler

Das `PositionError`-Objekt wird an die `plopplopError`-Callback-Funktion übergeben, tritt ein Fehler mit navigator.plopplop.

### Eigenschaften

  * **Code**: einer der vordefinierten Fehlercodes aufgeführt.

  * **Nachricht**: Fehlermeldung, die die Informationen über den aufgetretenen Fehler beschreibt.

### Konstanten

  * `PositionError.PERMISSION_DENIED` 
      * Zurückgegeben, wenn Benutzer erlauben nicht die app Positionsinformationen abgerufen werden. Dies ist abhängig von der Plattform.
  * `PositionError.POSITION_UNAVAILABLE` 
      * Zurückgegeben, wenn das Gerät nicht in der Lage, eine Position abzurufen ist. Im Allgemeinen bedeutet dies, dass das Gerät nicht mit einem Netzwerk verbunden ist oder ein Satelliten-Update kann nicht abgerufen werden.
  * `PositionError.TIMEOUT` 
      * Zurückgegeben, wenn das Gerät nicht in der Lage, eine Position innerhalb der festgelegten Zeit abzurufen ist die `timeout` enthalten `plopplopOptions` . Bei Verwendung mit `navigator.plopplop.watchPosition` , könnte dieser Fehler wiederholt übergeben werden, zu der `plopplopError` Rückruf jedes `timeout` Millisekunden.