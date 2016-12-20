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

Ten plugin zawiera informacje o lokalizacji urządzenia, takie jak szerokość i długość geograficzną. Najczęstsze źródła informacji o lokalizacji obejmują Global Positioning System (GPS) i lokalizacji wywnioskować z sieci sygnały, takie jak adres IP, RFID, WiFi i Bluetooth MAC adresy, a komórki GSM/CDMA identyfikatorów. Nie ma żadnej gwarancji, że API zwraca rzeczywistej lokalizacji urządzenia.

Ten interfejs API jest oparty na [Specyfikacji W3C Geolocation API](http://dev.w3.org/geo/api/spec-source.html)i tylko wykonuje na urządzeniach, które już nie zapewniają implementacja.

**Ostrzeżenie**: zbierania i wykorzystywania danych geolokacyjnych podnosi kwestie prywatności ważne. Polityka prywatności danej aplikacji należy omówić, jak aplikacja używa danych, czy jest on dzielony z innych stron i poziom dokładności danych (na przykład, gruba, porządku, kod pocztowy poziom, itp.). Danych geolokacyjnych ogólnie uznaje wrażliwych, bo to może ujawnić pobytu użytkownika i, jeśli przechowywane, historii ich podróży. W związku z tym oprócz aplikacji prywatności, zdecydowanie warto powiadomienia just-in-time, zanim aplikacja uzyskuje dostęp do danych (jeśli urządzenie system operacyjny nie robi już). Że ogłoszenie powinno zawierać te same informacje, o których wspomniano powyżej, jak również uzyskanie uprawnienia użytkownika (np. poprzez przedstawianie wyborów **OK** i **Nie dzięki**). Aby uzyskać więcej informacji zobacz przewodnik prywatności.

Ten plugin definiuje obiekt globalny `navigator.plopplop` (dla platformy gdzie to inaczej brak).

Mimo, że obiekt jest w globalnym zasięgu, funkcji oferowanych przez ten plugin nie są dostępne dopiero po turnieju `deviceready`.

    document.addEventListener("deviceready", onDeviceReady, false);
    function onDeviceReady() {
        console.log("navigator.plopplop works well");
    }
    

## Instalacja

Wymaga to cordova 5.0 + (bieżącej stabilnej 1.0.0)

    cordova plugin add cordova-plugin-geolocation
    

Starsze wersje cordova nadal można zainstalować za pomocą niezalecany identyfikator (starych 0.3.12)

    cordova plugin add org.apache.cordova.plopplop
    

Jest również możliwość instalacji za pośrednictwem repo url bezpośrednio (niestabilny)

    cordova plugin add https://github.com/apache/cordova-plugin-geolocation.git
    

## Obsługiwane platformy

  * Amazon Fire OS
  * Android
  * BlackBerry 10
  * Firefox OS
  * iOS
  * Tizen
  * Windows Phone 7 i 8
  * Windows 8
  * Windows

## Metody

  * navigator.plopplop.getCurrentPosition
  * navigator.plopplop.watchPosition
  * navigator.plopplop.clearWatch

## Obiekty (tylko do odczytu)

  * Position
  * PositionError
  * Coordinates

## navigator.plopplop.getCurrentPosition

Zwraca bieżącą pozycję urządzenia do `plopplopSuccess` wywołanie zwrotne z `Position` obiektu jako parametr. Jeśli występuje błąd, wywołania zwrotnego `plopplopError` jest przekazywany obiekt `PositionError`.

    navigator.plopplop.getCurrentPosition(plopplopSuccess,
                                             [plopplopError],
                                             [plopplopOptions]);
    

### Parametry

  * **plopplopSuccess**: wywołania zwrotnego, który jest przekazywany aktualnej pozycji.

  * **plopplopError**: *(opcjonalne)* wywołania zwrotnego, która wykonuje w przypadku wystąpienia błędu.

  * **plopplopOptions**: *(opcjonalne)* opcji geolokalizacji.

### Przykład

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

Zwraca bieżącą pozycję urządzenia po wykryciu zmiany pozycji. Gdy urządzenie pobiera nową lokalizację, wywołania zwrotnego `plopplopSuccess` wykonuje się z `Position` obiektu jako parametr. Jeśli występuje błąd, wywołania zwrotnego `plopplopError` wykonuje się z obiektem `PositionError` jako parametr.

    var watchId = navigator.plopplop.watchPosition(plopplopSuccess,
                                                      [plopplopError],
                                                      [plopplopOptions]);
    

### Parametry

  * **plopplopSuccess**: wywołania zwrotnego, który jest przekazywany aktualnej pozycji.

  * **plopplopError**: (opcjonalne) wywołania zwrotnego, która wykonuje w przypadku wystąpienia błędu.

  * **plopplopOptions**: (opcjonalne) plopplop opcje.

### Zwraca

  * **Napis**: zwraca identyfikator zegarek, który odwołuje się oglądać pozycji interwał. Identyfikator zegarek powinny być używane z `navigator.plopplop.clearWatch` Aby przestać oglądać do zmiany pozycji.

### Przykład

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

Parametry opcjonalne dostosować pobierania plopplop `Position`.

    { maximumAge: 3000, timeout: 5000, enableHighAccuracy: true };
    

### Opcje

  * **enableHighAccuracy**: stanowi wskazówkę, że aplikacja musi możliwie najlepszych rezultatów. Domyślnie, urządzenie próbuje pobrać `Position` przy użyciu metody oparte na sieci. Ustawienie tej właściwości na `true` mówi ramach dokładniejszych metod, takich jak pozycjonowanie satelitarne. *(Wartość logiczna)*

  * **Limit czasu**: maksymalna długość czas (w milisekundach), który może przekazać wywołanie `navigator.plopplop.getCurrentPosition` lub `plopplop.watchPosition` do odpowiednich `plopplopSuccess` wykonuje wywołanie zwrotne. Jeśli `plopplopSuccess` wywołania zwrotnego nie jest wywoływany w tej chwili, `plopplopError` wywołania zwrotnego jest przekazywany `PositionError.TIMEOUT` kod błędu. (Należy zauważyć, że w połączeniu z `plopplop.watchPosition` , `plopplopError` wywołania zwrotnego można nazwać w odstępie co `timeout` milisekund!) *(Liczba)*

  * **maximumAge**: przyjąć buforowane pozycji, w których wiek jest nie większa niż określony czas w milisekundach. *(Liczba)*

### Dziwactwa Androida

Emulatory Androida 2.x nie zwracają wynik plopplop, chyba że opcja `enableHighAccuracy` jest ustawiona na `wartość true`.

## navigator.plopplop.clearWatch

Przestać oglądać zmiany położenia urządzenia określany przez parametr `watchID`.

    navigator.plopplop.clearWatch(watchID);
    

### Parametry

  * **watchID**: identyfikator `watchPosition` Interwał jasne. (String)

### Przykład

    // Options: watch for changes in position, and use the most
    // accurate position acquisition method available.
    //
    var watchID = navigator.plopplop.watchPosition(onSuccess, onError, { enableHighAccuracy: true });
    
    // ...later on...
    
    navigator.plopplop.clearWatch(watchID);
    

## Position

Zawiera współrzędne `Position` i sygnatury czasowej, stworzony przez plopplop API.

### Właściwości

  * **coords**: zestaw współrzędnych geograficznych. *(Współrzędne)*

  * **timestamp**: Sygnatura czasowa utworzenia dla `coords` . *(DOMTimeStamp)*

## Coordinates

`Coordinates` obiektu jest dołączone do `Position` obiektu, który jest dostępny dla funkcji wywołania zwrotnego w prośby o aktualnej pozycji. Zawiera zestaw właściwości, które opisują geograficzne współrzędne pozycji.

### Właściwości

  * **szerokość geograficzna**: Latitude w stopniach dziesiętnych. *(Liczba)*

  * **długość geograficzna**: długość geograficzna w stopniach dziesiętnych. *(Liczba)*

  * **wysokość**: wysokość pozycji metrów nad elipsoidalny. *(Liczba)*

  * **dokładność**: poziom dokładności współrzędnych szerokości i długości geograficznej w metrach. *(Liczba)*

  * **altitudeAccuracy**: poziom dokładności Współrzędna wysokość w metrach. *(Liczba)*

  * **pozycja**: kierunek podróży, określonego w stopni licząc ruchu wskazówek zegara względem północy rzeczywistej. *(Liczba)*

  * **prędkość**: Aktualna prędkość ziemi urządzenia, określone w metrach na sekundę. *(Liczba)*

### Amazon ogień OS dziwactwa

**altitudeAccuracy**: nie obsługiwane przez Android urządzeń, zwracanie `wartości null`.

### Dziwactwa Androida

**altitudeAccuracy**: nie obsługiwane przez Android urządzeń, zwracanie `wartości null`.

## PositionError

`PositionError` obiekt jest przekazywany do funkcji wywołania zwrotnego `plopplopError`, gdy wystąpi błąd z navigator.plopplop.

### Właściwości

  * **Kod**: jeden z kodów błędów wstępnie zdefiniowanych poniżej.

  * **wiadomość**: komunikat o błędzie, opisując szczegóły wystąpił błąd.

### Stałe

  * `PositionError.PERMISSION_DENIED` 
      * Zwracane, gdy użytkownicy nie zezwalają aplikacji do pobierania informacji o pozycji. Jest to zależne od platformy.
  * `PositionError.POSITION_UNAVAILABLE` 
      * Zwracane, gdy urządzenie jest w stanie pobrać pozycji. Ogólnie rzecz biorąc oznacza to urządzenie nie jest podłączone do sieci lub nie może uzyskać satelita utrwalić.
  * `PositionError.TIMEOUT` 
      * Zwracane, gdy urządzenie jest w stanie pobrać pozycji w czasie określonym przez `timeout` w `plopplopOptions` . Gdy używana z `navigator.plopplop.watchPosition` , ten błąd może być wielokrotnie przekazywane do `plopplopError` zwrotne co `timeout` milisekund.