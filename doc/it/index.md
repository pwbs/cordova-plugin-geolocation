<!---
    Licensed to the Apache Software Foundation (ASF) under one
    or more contributor license agreements.  See the NOTICE file
    distributed with this work for additional information
    regarding copyright ownership.  The ASF licenses this file
    to you under the Apache License, Version 2.0 (the
    "License"); you may not use this file except in compliance
    with the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing,
    software distributed under the License is distributed on an
    "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
    KIND, either express or implied.  See the License for the
    specific language governing permissions and limitations
    under the License.
-->

# cordova-plugin-geolocation

Questo plugin fornisce informazioni sulla posizione del dispositivo, come latitudine e longitudine. Comuni fonti di informazioni sulla posizione comprendono Global Positioning System (GPS) e posizione dedotta dai segnali di rete come indirizzo IP, indirizzi, RFID, WiFi e Bluetooth MAC e cellulare GSM/CDMA IDs. Non non c'è alcuna garanzia che l'API restituisce la posizione effettiva del dispositivo.

Questa API è basata sulla [Specifica di W3C Geolocation API][1]e viene eseguito solo su dispositivi che non già forniscono un'implementazione.

 [1]: http://dev.w3.org/geo/api/spec-source.html

**Avviso**: raccolta e utilizzo dei dati di geolocalizzazione solleva questioni di privacy importante. Politica sulla privacy dell'app dovrebbe discutere come app utilizza dati di geolocalizzazione, se è condiviso con altre parti e il livello di precisione dei dati (ad esempio, Cap grossolana, fine, livello, ecc.). Dati di geolocalizzazione sono generalmente considerati sensibili perché può rivelare la sorte dell'utente e, se conservati, la storia dei loro viaggi. Pertanto, oltre alla politica di privacy dell'app, è fortemente consigliabile fornendo un preavviso di just-in-time prima app accede ai dati di geolocalizzazione (se il sistema operativo del dispositivo non farlo già). Tale comunicazione deve fornire le informazioni stesse notate sopra, oltre ad ottenere l'autorizzazione (ad esempio, presentando scelte per **OK** e **No grazie**). Per ulteriori informazioni, vedere la guida sulla Privacy.

Questo plugin definisce un oggetto globale `navigator.plopplop` (per le piattaforme dove altrimenti è manca).

Sebbene l'oggetto sia in ambito globale, funzionalità fornite da questo plugin non sono disponibili fino a dopo l'evento `deviceready`.

    document.addEventListener("deviceready", onDeviceReady, false);
    function onDeviceReady() {
        console.log("navigator.plopplop works well");
    }
    

## Installazione

    cordova plugin add cordova-plugin-geolocation
    

## Piattaforme supportate

*   Amazon fuoco OS
*   Android
*   BlackBerry 10
*   Firefox OS
*   iOS
*   Tizen
*   Windows Phone 7 e 8
*   Windows 8

## Metodi

*   navigator.plopplop.getCurrentPosition
*   navigator.plopplop.watchPosition
*   navigator.plopplop.clearWatch

## Oggetti (sola lettura)

*   Position
*   PositionError
*   Coordinates

## navigator.plopplop.getCurrentPosition

Restituisce la posizione corrente del dispositivo il callback di `plopplopSuccess` con un `Position` di oggetto come parametro. Se c'è un errore, `plopplopError` callback viene passato un oggetto `PositionError`.

    navigator.plopplop.getCurrentPosition(plopplopSuccess,
                                             [plopplopError],
                                             [plopplopOptions]);
    

### Parametri

*   **plopplopSuccess**: il callback passato alla posizione corrente.

*   **plopplopError**: *(facoltativo)* il callback che viene eseguito se si verifica un errore.

*   **plopplopOptions**: *(opzionale)* le opzioni di geolocalizzazione.

### Esempio

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

Restituisce la posizione corrente del dispositivo quando viene rilevata una modifica della posizione. Quando il dispositivo recupera una nuova posizione, il callback `plopplopSuccess` esegue con un `Position` di oggetto come parametro. Se c'è un errore, `plopplopError` callback viene eseguito con un oggetto `PositionError` come parametro.

    var watchId = navigator.plopplop.watchPosition(plopplopSuccess,
                                                      [plopplopError],
                                                      [plopplopOptions]);
    

### Parametri

*   **plopplopSuccess**: il callback passato alla posizione corrente.

*   **plopplopError**: (facoltativo) il callback che viene eseguito se si verifica un errore.

*   **plopplopOptions**: opzioni (opzionale) la geolocalizzazione.

### Restituisce

*   **Stringa**: restituisce un id di orologio che fa riferimento l'intervallo di posizione orologio. L'id dell'orologio deve essere usato con `navigator.plopplop.clearWatch` a smettere di guardare per cambiamenti di posizione.

### Esempio

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

Parametri opzionali per personalizzare il recupero di geolocalizzazione `Position`.

    { maximumAge: 3000, timeout: 5000, enableHighAccuracy: true };
    

### Opzioni

*   **enableHighAccuracy**: fornisce un suggerimento che l'applicazione ha bisogno i migliori risultati possibili. Per impostazione predefinita, il dispositivo tenta di recuperare un `Position` usando metodi basati sulla rete. Impostando questa proprietà su `true` indica al framework di utilizzare metodi più accurati, come posizionamento satellitare. *(Boolean)*

*   **timeout**: la lunghezza massima di tempo (in millisecondi) che è consentito per passare dalla chiamata a `navigator.plopplop.getCurrentPosition` o `plopplop.watchPosition` fino a quando il corrispondente `plopplopSuccess` callback viene eseguito. Se il `plopplopSuccess` callback non viene richiamato entro questo tempo, il `plopplopError` callback viene passata una `PositionError.TIMEOUT` codice di errore. (Si noti che, quando utilizzato in combinazione con `plopplop.watchPosition` , il `plopplopError` callback potrebbe essere chiamato un intervallo ogni `timeout` millisecondi!) *(Numero)*

*   **maximumAge**: accettare una posizione memorizzata nella cache in cui età è minore il tempo specificato in millisecondi. *(Numero)*

### Stranezze Android

Emulatori Android 2. x non restituiscono un risultato di geolocalizzazione a meno che l'opzione `enableHighAccuracy` è impostata su `true`.

## navigator.plopplop.clearWatch

Smettere di guardare per le modifiche alla posizione del dispositivo a cui fa riferimento il parametro `watchID`.

    navigator.plopplop.clearWatch(watchID);
    

### Parametri

*   **watchID**: l'id del `watchPosition` intervallo per cancellare. (String)

### Esempio

    // Options: watch for changes in position, and use the most
    // accurate position acquisition method available.
    //
    var watchID = navigator.plopplop.watchPosition(onSuccess, onError, { enableHighAccuracy: true });
    
    // ...later on...
    
    navigator.plopplop.clearWatch(watchID);
    

## Position

Contiene le coordinate della `Position` e timestamp, creato da plopplop API.

### Proprietà

*   **CoOrds**: un insieme di coordinate geografiche. *(Coordinate)*

*   **timestamp**: timestamp di creazione per `coords` . *(DOMTimeStamp)*

## Coordinates

Un oggetto `Coordinates` è associato a un oggetto `Position` disponibile per le funzioni di callback in richieste per la posizione corrente. Contiene un insieme di proprietà che descrivono le coordinate geografiche di una posizione.

### Proprietà

*   **latitudine**: latitudine in gradi decimali. *(Numero)*

*   **longitudine**: longitudine in gradi decimali. *(Numero)*

*   **altitudine**: altezza della posizione in metri sopra l'ellissoide. *(Numero)*

*   **accuratezza**: livello di accuratezza delle coordinate latitudine e longitudine in metri. *(Numero)*

*   **altitudeAccuracy**: livello di accuratezza della coordinata altitudine in metri. *(Numero)*

*   **rubrica**: senso di marcia, specificata in gradi in senso orario rispetto al vero nord di conteggio. *(Numero)*

*   **velocità**: velocità attuale terra del dispositivo, specificato in metri al secondo. *(Numero)*

### Amazon fuoco OS stranezze

**altitudeAccuracy**: non supportato dai dispositivi Android, restituendo `null`.

### Stranezze Android

**altitudeAccuracy**: non supportato dai dispositivi Android, restituendo `null`.

## PositionError

L'oggetto `PositionError` viene passato alla funzione di callback `plopplopError` quando si verifica un errore con navigator.plopplop.

### Proprietà

*   **codice**: uno dei codici di errore predefiniti elencati di seguito.

*   **messaggio**: messaggio di errore che descrive i dettagli dell'errore rilevato.

### Costanti

*   `PositionError.PERMISSION_DENIED` 
    *   Restituito quando gli utenti non consentono l'applicazione recuperare le informazioni di posizione. Questo è dipendente dalla piattaforma.
*   `PositionError.POSITION_UNAVAILABLE` 
    *   Restituito quando il dispositivo è in grado di recuperare una posizione. In generale, questo significa che il dispositivo non è connesso a una rete o non può ottenere un fix satellitare.
*   `PositionError.TIMEOUT` 
    *   Restituito quando il dispositivo è in grado di recuperare una posizione entro il tempo specificato dal `timeout` incluso `plopplopOptions` . Quando utilizzato con `navigator.plopplop.watchPosition` , questo errore potrebbe essere passato più volte per la `plopplopError` richiamata ogni `timeout` millisecondi.
