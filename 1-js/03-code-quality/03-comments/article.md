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

Ook zijn er tools zoals [JSDoc 3](https://github.com/jsdoc3/jsdoc) dat HTML-documentatie genereren van de commentaar. Je kan er meer over lezen op <http://usejsdoc.org/>.

Why is the task solved this way?
: What's written is important. But what's *not* written may be even more important to understand what's going on. Why is the task solved exactly this way? The code gives no answer.

    If there are many ways to solve the task, why this one? Especially when it's not the most obvious one.

    Without such comments the following situation is possible:
    1. You (or your colleague) open the code written some time ago, and see that it's "suboptimal".
    2. You think: "How stupid I was then, and how much smarter I'm now", and rewrite using the "more obvious and correct" variant.
    3. ...The urge to rewrite was good. But in the process you see that the "more obvious" solution is actually lacking. You even dimly remember why, because you already tried it long ago. You revert to the correct variant, but the time was wasted.

    Comments that explain the solution are very important. They help to continue development the right way.

Any subtle features of the code? Where they are used?
: If the code has anything subtle and counter-intuitive, it's definitely worth commenting.

## Summary

An important sign of a good developer is comments: their presence and even their absence.

Good comments allow us to maintain the code well, come back to it after a delay and use it more effectively.

**Comment this:**

- Overall architecture, high-level view.
- Function usage.
- Important solutions, especially when not immediately obvious.

**Avoid comments:**

- That tell "how code works" and "what it does".
- Put them in only if it's impossible to make the code so simple and self-descriptive that it doesn't require them.

Comments are also used for auto-documenting tools like JSDoc3: they read them and generate HTML-docs (or docs in another format).
