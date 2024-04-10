# API READ.ME

## Inhoudsopgave <a name="inhouds-opgave"></a>
- [Inhoudsopgave](#inhoudsopgave)
- [Introductie](#introductie)
- [Server Side Applicatie]

## Installatie

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

Vervolgens ben ik een 'public' mapje gaan maken, waarin ik de JavaScript, CSS en images plaats. Daarnaast heb ik ook een 'views' mapje aangemaakt, waarin ik de pagina's van de applicatie heb gezet. Hierin staan dus de overzichtspagina en de detailpagina. Ook heb ik een 'app.js' bestand gemaakt, wat dient om de app te laten werken. Om nodemon te laten werken, heb ik in het package.json bestand een nieuwe regel code toegevoegd.

```json
"start": "nodemon app.js"
```

Vervolgens ben ik in dit 'app.js' bestand Express en Axios (een veelgebruikte HTTP-clientbibliotheek voor het maken van HTTP-verzoeken vanuit Node.js- en browseromgevingen) gaan importeren en heb ik een port opgezet waarnaar Express naar moet luisteren voor inkomende verzoeken. 

```js
const express = require('express')
const app = express()
const axios = require('axios')
const port = 3000

app.listen(port, () => console.info(`Luisterende op port ${port}`))
```

Daaronder ben ik een aantal statische bestanden gaan toevoegen. Dit zijn mijn JavaScript bestand, CSS bestand en de foto's die in het project wil toevoegen. 

```js
app.use(express.static('public'))
app.use('/css', express.static(__dirname + 'public/css'))
app.use('/js', express.static(__dirname + 'public/js'))
app.use('/img', express.static(__dirname + 'public/img'))
```

Vervolgens ben ik voor een manier gaan zoeken hoe ik de bestanden in de 'views' map ging weergeven. Dit heb ik gedaan aan de hand van de volgende code:

```js
app.set('views', './views')
app.set('view engine', 'ejs')

app.get('', (req, res) => {
    res.render('index')
})

app.get('/about', (req, res) => {
    res.render('about')
})
```
