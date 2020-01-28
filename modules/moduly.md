# Podstawowe elementy biblioteki React

## Podstawy biblioteki React, definicja elementu, komponentu. 

### Jak działa aplikacja korzystająca z React

1. Przeglądarka w odpowiedzi na zapytanie HTTP otrzymuje plik `HTML` ze skryptem `javascipt` zawierającym aplikację zbudowaną z elementów biblioteki `React`
2. Skrypt `javascript` zawiera metodę renderującą pod wskazanym adresem główny komponent aplikacji najczęściej nazywany `App` 

Przykład:

Skrypt zalinkowany w pliku HTML oraz docelowy element w którym wyrenderuje się aplikacja

```html
<div id="root"></div>
<script src="js/bundle.js"></script>
```

Tak wygląda `index.js` odpowiedzialny za włożenie głownego komponentu aplikacji `<App/>` do elementu `div#root`

```javascript
import React from "react";
import ReactDOM from "react-dom";

import App from "./App";

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

W aplikacjach popularne jest używanie narzędzi automatyzujących przygotowanie kodu aplikacji pod silniki starszych przeglądarek jak i podziału aplikacji na mniejsze paczki tak aby czas ładowania aplikacji był lepszy. Najczęściej korzysta się z [`webpack`](https://webpack.js.org/).

### Czym jest element?

Element to najmniejsza część składowa aplikacji. Za pomocą elementów komponenty komponują swój wygląd.

```javascript
const element = <h1>Witaj, świecie</h1>;
```

### Czym jest komponent?

Komponent to funkcja lub klasa, która musi spełnić następujące warunki:
1. Nazwa musi zaczynać się dużą literą
2. Zwraca `element` lub `null`  

Przykłady:

Komponent funkcyjny ( popularniejsze od klasowych ):
```javascript
function Welcome(props) {
  return <h1>Witaj, {props.name}</h1>
}
```

Komponent klasowy

```jsx
class Welcome extends React.Component {
    render(){
      return <h1>Witaj, {props.name}</h1>
    }
}
```

## Elementy języka JSX. 

### Podstawowy zapis

Zapewne zauważyłeś, że stosujemy zapis `<App/>` zamiast `"<App/>"`. Jest to język JSX stworzony przez facebook. Umożliwia w czytelny sposób tworzyć strukturę naszej aplikacji. Każdy element zostanie zamieniony do funkcji.

Przykład:
```javascript
//zapis JSX
const Button = () => <button>Click</button>

//zapis z użyciem funkcji
const Button = () => React.createElement('button', null, 'Click');
```

Każdy element `JSX` musi być odpowiednio zamknięty nawet jeżeli tworzy element `html` który nie wymaga zamknięcia.

Przykład:

Javascript
```javascript
const Br = () => <br/>
```

HTML
```html
<br>
```

### Wyrażenia

#### Czym jest wyrażenie?

Wyrażenie to dowolny prawidłowy fragment kodu zamieniony do jednej wartości

Przykład:
```javascript
1 + 1 // zostanie zamienione do wartości 2
```

### Jak osadzić wyrażenie w `JSX` ?

Przyjmijmy, że chcemy aby tekst w guziku pochodził ze zmiennej `text`

```javascript
const text = "Click":

const Button = () => <button>{text}</button>
```

Wyrażenia zamykamy w klamry `{  }`


## Właściwości komponentu, walidacja właściwości, własny typ właściwości, `children`.

### Właściwości komponentu

#### Czym są właściwości?

Po angielskim `props` od słowa `properties`.

To informacje przekazane komponentowi dziecku od komponentu rodzica. Ważne jest, że dziecko nie może ich zmieniać. Posiadają status `readonly`.

#### Przykład:
Komponent `App` jako rodzic przekazuje wartość `Click` swojemu komponentowi dziecku `Button` jako właściwość. 

```javascript
//każda para klucz=wartosc z App zostaje przekazana do zmiennej props
const Button = (props) => <button>{props.text}</button>

const App = () => <Button text="Click" />
```

Rodzic może przekazać dowolną wartość dziecku (funkcja, tablica, obiekt). Takie słowa jak `class` lub `for` są zarezerwowane w języku `javscript` dlatego należy używać `className` chcąc przekazać klasę `CSS` komponentowi.

### Walidacja właściwości

[Dokumentacja React](https://pl.reactjs.org/docs/typechecking-with-proptypes.html)

Aby walidować właściwości należy doinstalować pakiet `prop-types`, który stał się niezależną paczką od wersji `React 15.5`. Aby to zrobić wystarczy w terminalu otwartym w lokalizacji projektu wywołać:

```
npm i prop-types;
```

#### Przykład: 
Zadbajmy aby właściwość guzika spełniała dwa warunki:
1. Jest napisem 
2. Jest wymagana

```javascript
const Button = (props) => <button>{props.text}</button>

Button.propTypes = {
    text: PropTypes.string.isRequired
}
```

### Własny typ właściwości

Możemy na wiele sposobów stworzyć własny typ właściwości. Skutecznym podejściem jest określenie tablicy z wartościami jakie może przyjąć właściwość


#### Przykład
Tekst guzika może przyjąć tylką jedną z dwóch wartości `Delete` lub `Add`

```javascript
const Button = (props) => <button>{props.text}</button>

Button.propTypes = {
    text: PropTypes.oneOf(['Delete', 'Add'])
}
```

### Children

Właściwości posiadają specjalny klucz `children` który zawiera wszystko co znajduje się pomiędzy znacznikiem otwierającym a zamykającym w miejscu użycia komponentu.

#### Przykład

Posiadamy komponent `Form`, który powinien "owinąć" pola formularza.

```javascript
const Form = (props) => <form method="post">{props.children}</form>


const App = () => {
    return (
       <Form>
         <Input type="number" />
         <Input type="submit" value="submit" />
       </Form>
    )
}
```

## Utworzenie szkieletu projektu i omówienie wygenerowanej struktury.

# Stan komponentów

Cześć. W tej części poznasz podstawy przechowywania i zarządzania informacjami w komponencie.

## Definicja stanu komponentów

Stan komponentu to specjalnie przygotowany interface umożliwiający przechowywanie danych wewnątrz kompomentu. Komponent ma możliwość zmiany tych danych przez specjalne setery. Przerenderowanie komponentu nie powoduje utraty danych. Każde wywołanie metody zmienijącej stan komponentu powoduje przerenderowanie  czyli odzwierciedlenie stanu komponentu w interface użytkownika.

## Inicjalizacja i zmiana stanu 

Od wersji React 16.08 dostępne są tak zwane hooki czyli funkcje umożliwiające przechowywanie i zmianę stanu w komponetnach funkcyjnych. 

#### Przykład: Liczenie kliknięć guzika

Każde kliknięcie spowoduje zwiększenie `counter` o `1`.
Zauważ, że nie użyliśmy `counter++` ponieważ zmienna jest tylko do odczytu.
Dodatkowo każde użycie `setCounter` spowoduje wywołanie funkcji `Button` i zwrócenie guzika z nową wartością zmiennej `counter` 

```javascript
const Button = () => {
    const [counter, setCounter] = React.useState(0);

    return <button onClick={() => setCounter( counter + 1 )}>Clicks {counter}counter}</button>
}
```

## Zdarzenia syntetyczne i powiązanie zdarzeń ze zmiennymi stanu

## Event pooling.
