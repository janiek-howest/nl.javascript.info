# Commentaar

Zoals we weten van het hoofdstuk <info:structure>, commentaar kan geschreven woreden op een enkele lijn: startend met `//` en over verschillende lijnen: `/* ... */`.

We gebruiken dit voornamelijk om te beschrijven hoe en waarom de code werkt.

Op het eerste gezicht, lijkt commentaar vanzelfsprekend, maar beginnende ontwikkelaars gebruiken ze dikwijls verkeerd.

## Slechte commentaar

Beginners gebruiken commentaar om te verklaren "wat er juist gebeurd in de code". Zoals dit:

```js
// This code will do this thing (...) and that thing (...)
// ...and who knows what else...
very;
complex;
code;
```

Maar in degelijke code, moet het aantal "verklarende" commentaar zo weinig mogelijk zijn. Code moet zonder commentaar zo gemakkelijk mogelijk verstaanbaar zijn.

Er is een zeer goede regel daarover: "Als de code zo slecht is dat het commentaar nodig heeft, is het beter om de code te herschrijven".

### Recept: In functies wegwerken

Soms komt het ten goede om code te vervangen door een functie, zoals hier:

```js
function showPrimes(n) {
  nextPrime:
  for (let i = 2; i < n; i++) {

*!*
    // check if i is a prime number
    for (let j = 2; j < i; j++) {
      if (i % j == 0) continue nextPrime;
    }
*/!*

    alert(i);
  }
}
```

Dit kan beter, door dit weg te werken in een functie `isPrime`:


```js
function showPrimes(n) {

  for (let i = 2; i < n; i++) {
    *!*if (!isPrime(i)) continue;*/!*

    alert(i);  
  }
}

function isPrime(n) {
  for (let i = 2; i < n; i++) {
    if (n % i == 0) return false;
  }

  return true;
}
```

Nu verstaan we de code veel beter. De commentaar wordt hierdoor weggewerkt door de functie. De code wordt hierdoor *zelf-beschrijvend*.

### Recept: Toevoegende functies

En als we lange "code" hebben zoals hieronder:

```js
// here we add whiskey
for(let i = 0; i < 10; i++) {
  let drop = getWhiskey();
  smell(drop);
  add(drop, glass);
}

// here we add juice
for(let t = 0; t < 3; t++) {
  let tomato = getTomato();
  examine(tomato);
  let juice = press(tomato);
  add(juice, glass);
}

// ...
```

Dan kunnen we dit beter herschrijven (refactoren) in functions zoals:

```js
addWhiskey(glass);
addJuice(glass);

function addWhiskey(container) {
  for(let i = 0; i < 10; i++) {
    let drop = getWhiskey();
    //...
  }
}

function addJuice(container) {
  for(let t = 0; t < 3; t++) {
    let tomato = getTomato();
    //...
  }
}
```

Nogmaals, de functies zelf vertellen al wat er gebeurt. Commentaar wordt daardoor overbodig. De structuur van de code wordt beter wanneer deze wordt gesplitst. Het is duidelijk wat elke functie doet, welke parameters er nodig zijn en wat het teruggeeft.

In de praktijk, kunnen we "verklarende" commentaar niet altijd vermijden. Er bestaan complexe algoritmes. En er zijn slimme "truckjes" om bepaalde zaken te optimaliseren. Maar algemeen moeten we proberen om onze code zo simpel mogelijk te houden en zelf-beschrijvend.

## Goede commentaar

Dus, beschrijvende commentaar is over het algemeeen slecht. Maar welke commentaar is dan wel goed?

Beschrijf de architectuur: Voorzie een overzicht van componenten, hoe deze met elkaar in interactie gaan , hoe de flow in elkaar zit in verschillende situaties... In het kort -- Geef een vogelperspectief van de code. Er bestaat een speciale taal [UML](http://wikipedia.org/wiki/Unified_Modeling_Language) om high-level architecturale diagrammen te maken om code uit te leggen. Zeker de moeite waard om dit te bestuderen.

Documenteer functie parameters en hun gebruik: Er bestaat een speciale syntax [JSDoc](http://en.wikipedia.org/wiki/JSDoc) om een functie te documenteren: gebruik, parameters, teruggegeven waarde.

Bijvoorbeeld:
```js
/**
 * Returns x raised to the n-th power.
 *
 * @param {number} x The number to raise.
 * @param {number} n The power, must be a natural number.
 * @return {number} x raised to the n-th power.
 */
function pow(x, n) {
  ...
}
```

Deze commentaar laat ons toe te begrijpen wat het doel is van de functie en het te gebruiken op de jusite manier zonder naar de code te gaan kijken.

Langs de andere kant, kunnen veel editoren zoals [WebStorm](https://www.jetbrains.com/webstorm/) dit begrijpen en gebruiken om autocomplete te voorzien en automatische code checks toe te passen.

Ook zijn er tools zoals [JSDoc 3](https://github.com/jsdoc3/jsdoc) die HTML-documentatie genereren van de commentaar. Je kan er meer over lezen op <http://usejsdoc.org/>.

Waarom is dit op deze manier opgelost?
: Wat geschreven is, is belangrijk. Maar wat *niet* geschreven is, kan zelf belangrijker zijn om te verstaan wat er aan het gebeuren is. Waarom is de taak op deze manier opgelost? De code geeft geen antwoord.

    Als er meerdere manieren zijn om een opdracht op te lossen, waarom gebruik je deze? Zeker wanneer het niet de meest voordehand liggende is.

    Zonder zo'n commentaar worden volgende situaties mogelijk:
    1. Jij (of je collega) openen de code geschreven een tijdje terug, en zien dat het "niet optimaal" geschreven is.
    2. Jij denkt: "Hoe dom was ik toen, en hoeveel wijzer ben ik nu", en je herschrijft de code met de "meer voor de hand liggende" manier.
    3. ...De neiging om te herschrijven was goed. Maar in een proces zie je dat de "meer voor de hand liggende" oplossing eigenlijk tekort schiet. Je begint je lichtjes te herinneren waarom, omdat je het vroeger al geprobeerd had. Je keert terug naar de eerste oplossing, maar je hebt tijd verloren.

    Commentaar die de oplossing beschrijven zijn zeer belangrijk. Ze helpen om de ontwikkeling in de juist richting verder te zetten.

Zijn er speciale features aan de code? Wanneer gebruik je ze?
: Als de code contra-intuitive zaken bevat, is het zeker interessant om deze te be-commentariëren.

## Samenvatting

Een belangrijk kenmerk van een goede ontwikkelaar is zijn commentaar: de aan- en afwezigheid er van.

Goeie commentaar staan ons toe om code goed te onderhouden, later er op terug te komen en het meer efficiënt te gebruiken.

**Becommentarieer:**

- Algemene architectuur, high-level overzicht.
- Gebruik van functies.
- Belangrijke oplossingen, zeker wanneer deze niet voor de hand liggend zijn.

**Vermijd commentaar:**

- Die zegt "hoe de code werkt" en "wat het doet".
- Doe dit enkel als het onmogelijk is de code eenvoudig en zelf-beschrijvend te maken, dat ze wel nodig zijn.

Commentaar wordt ook gebruikt voor auto-documenteer tools zoals JSDoc3: Deze lezen de code en genereren HTML-documentatie (of documentatie in een ander formaat).
