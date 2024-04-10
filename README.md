# API READ.ME

## Inhoudsopgave <a name="inhouds-opgave"></a>
- [Inhoudsopgave](#inhoudsopgave)
- [Introductie](#introductie)
- [Server Side Applicatie](#server-side-applicatie)

## Installatie <a name="installatie"></a>

Installeer deze repository: 
```
Git clone https://github.com/brentjeh/progressive-web-apps-2223
```

Om Node modules te installeren:
```
npm install
```

Om de applicatie te starten:
```
npm run dev
```

## Introductie
In de aankomende weken ga ik me bezig houden met het bouwen van webapplicaties die niet alleen functioneel zijn, maar ook leuk zijn om te gebruiken. Ik ga aan de slag met het maken van een server-side gerenderde applicatie die echt de aandacht trekt van de gebruikers.

Voor mijn opdracht heb ik besloten om een gallerij van schilderijen te maken aan de hand van de Rijksmuseum API. Hierin wil ik een overzichts- en detailpagina maken. 

## Server Side Applicatie
Eerst ben ik de server side applicatie gaan opzetten. Ik begon met het initialiseren van NPM. 

```js
npm init
```

Dit heeft een package.json bestand voor me aangemaakt.

Wat volgde was een reeds vragen over hoe ik mijn applicatie wilde opzetten. Ik heb de applicatie gewoon een naam en beschrijving gegeven, en heb verder alles leeg gelaten of op een standaard waarde gelaten. Ik open het mapje waarin ik NPM heb geïnitialiseerd in Visual Studio Code en ben daarna Express en EJS gaan installeren aan de hand van de volgende code.

```js
npm install express ejs
```

Dit heeft een package-lock.json bestand en een node_modules mapje voor me aangemaakt. Daarna ben ik nodemon gaan installeren, wat helpt met het automatisch overnieuw opstarten van de Node.js-toepassing wanneer er wijzigingen worden gedetecteerd in mijn bronbestanden. 

```js
npm install nodemon
```

Vervolgens ben ik een 'public' mapje gaan maken, waarin ik de JavaScript, CSS en images plaats. Daarnaast heb ik ook een 'views' mapje aangemaakt, waarin ik de pagina's van de applicatie heb gezet. Hierin staan dus de overzichtspagina en de detailpagina. 
