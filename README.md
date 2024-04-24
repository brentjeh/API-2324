# API READ.ME

## Inhoudsopgave <a name="inhouds-opgave"></a>
- [Inhoudsopgave](#inhoudsopgave)
- [Introductie](#introductie)
- [Week 1](#week-1)
  - [Server Side Applicatie](#server-side-applicatie)
  - [Opzetten EJS sjablonen](#opzetten-ejs-sjablonen)
  - [Fetchen van Data](#fetchen-van-data)
- [Week 2](#week-2)
  - [Weergeven kunstwerken](#weergeven-kunstwerken)
  - [Styling](#styling)
- [Week 3](#week-3)
  - [Web Animations API](#web-animations-api)
- [Week 4](#week-4)
  - [READ.ME bijwerken](#read-me-bijwerken)

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

# Week 1

## Server Side Applicatie
Eerst ben ik de server side applicatie gaan opzetten. Ik begon met het initialiseren van NPM. 

```js
npm init
```

Dit heeft een package.json bestand voor me aangemaakt.

Wat volgde was een reeds vragen over hoe ik mijn applicatie wilde opzetten. Ik heb de applicatie gewoon een naam en beschrijving gegeven, en heb verder alles leeg gelaten of op een standaard waarde gelaten. Ik open het mapje waarin ik NPM heb geïnitialiseerd in Visual Studio Code en ben daarna Express en EJS gaan installeren aan de hand van de volgende code. Ik heb voor Express en EJS gekozen omdat ik hier al eerder bekend was. Express is een eenvoudig te gebruiken framework voor het bouwen van webapplicaties en API's in Node.js. Door Express en EJS te gebruiken, kon ik gebruik maken van de voordelen van Node.js voor server-side ontwikkeling en tegelijkertijd snel webpagina's renderen met dynamische inhoud. 

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

## Fetchen van Data

Zoals ik eerder heb benoemd heb ik het fetchen van de data met Axios gedaan. Ik definieer hier een GET-verzoekshandler voor de hoofdroute ('/') van de applicatie. Als een gebruiker naar de homepagina gaat, wordt deze geactiveerd. Daarna begin ik een try blok, omdat er mogelijk fouten kunnen optreden in de code, en ik deze fouten daarom wil kunnen afhandelen. In dit blok maak ik een HTTP-GET verzoek naar de Rijksmuseum API om kunstwerken op te halen. Await zorgt ervoor dat de code wacht tot het antwoord van de server is ontvangen voordat deze verdergaat. Vervolgens itereer ik door elk schilderij om het te veranderen naar een object met de eigenschappen 'title' (titel van het schilderij), 'maker' (schilder van het schilderij) en 'imageUrl' (het schilderij zelf). Daarna wijs ik alles toe aan de eigenschappen in het nieuwe object. Ik render dan index.ejs met de gegeven data. Als er geen kunstwerken zijn, wordt een lege array doorgegeven om fouten te voorkomen. Ik definieer daarna een 'catch' blok om eventuele fouten af te handelen die zijn opgetreden in het try-blok. 

Dit hele blok code haalt dus een lijst met kunstwerken op van de Rijksmuseum API en rendert vervolgens de 'index' view met de opgehaalde gegevens.

```js
app.get('/', async (req, res) => {
    try {
        const response = await axios.get(`https://www.rijksmuseum.nl/api/nl/collection?key=${rijksmuseumApiKey}&type=schilderij&ps=20`);
        const artworks = response.data.artObjects.map(artwork => ({
            title: artwork.title,
            maker: artwork.principalOrFirstMaker,
            imageUrl: artwork.webImage.url
        }));
        res.render('index', { artworks: artworks || [] });
    } catch (error) {
        console.error('Fout bij het ophalen van kunstwerken:', error.message);
        res.status(500).json({ error: 'Er is een fout opgetreden bij het ophalen van de kunstwerken' });
    }
}); 
```

Ik heb ook een zoek-functie geïmplementeerd. Net zoals bij de hoofdroute, definieer ik hier een GET-verzoekshandler maar dan voor de search route ('/search'). Ik doe een HTTP-GET verzoek naar de Rijksmuseum API om kunstwerken te zoeken op basis van de zoekterm die de gebruiker heeft ingevult.

Dit blok code maakt dus een zoekverzoek naar de Rijksmuseum API op basis van de opgegeven zoekterm en stuurt de gevonden kunstwerken als JSON terug naar de client.

```js
app.get('/search', async (req, res) => {
    try {
        const title = req.query.title; // Haal de zoekterm op uit de query
        const response = await axios.get(`https://www.rijksmuseum.nl/api/nl/collection?key=${rijksmuseumApiKey}&q=${title}&type=schilderij`);
        const artworks = response.data.artObjects;  
        res.json({ artworks });
    } catch (error) {
        console.error('Fout bij het zoeken naar kunstwerken:', error.message);
        res.status(500).json({ error: 'Er is een fout opgetreden bij het zoeken naar de kunstwerken' });
    }
});
```

Als laatst heb ik een route naar de detailpagina gedefinieerd. 

De route reageert op HTTP GET-verzoeken naar het pad '/artwork/:id'. 'id' representeert de ID van het kunstwerk. Daarna wordt er een HTTP GET-verzoek gemaakt naar de Rijksmuseum API, die aan de hand van de id van het schilderij meer informatie over het schilderij ophaalt. Ik vervang 'nl-' met een lege string, omdat voor een of andere reden, er elke keer in de id 'nl-' voor de id geplaatst werd, wat er voor zorgde dat erg een informatie over het schilderij werd opgehaald. Ik ga door elk eigenschap van het kunstwerk om te controleren of er informatie beschikbaar is voor een specifiek eigenschap. Zo nee, geef ik een bericht weer dat aangeeft dat de informatie helaas niet bescikbaar is. Als er wel informatie beschikbaar is, wordt deze dynamisch in het sjabloon gerenderd.

```js
app.get('/artwork/:id', async (req, res) => {
    try {
        let artworkId = req.params.id; 
        artworkId = artworkId.replace(/^nl-/, '');

        const response = await axios.get(`https://www.rijksmuseum.nl/api/nl/collection/${artworkId}?key=${rijksmuseumApiKey}`);
        const artworkData = response.data.artObject;

        const isEmpty = (property) => !property || property.length === 0;

        const addNotAvailableText = (property) => isEmpty(property) ? 'Deze informatie is helaas niet beschikbaar' : property;

        artworkData.principalMakers[0].placeOfBirth = addNotAvailableText(artworkData.principalMakers[0].placeOfBirth);
        artworkData.principalMakers[0].dateOfBirth = addNotAvailableText(artworkData.principalMakers[0].dateOfBirth);
        artworkData.principalMakers[0].placeOfDeath = addNotAvailableText(artworkData.principalMakers[0].placeOfDeath);
        artworkData.principalMakers[0].dateOfDeath = addNotAvailableText(artworkData.principalMakers[0].dateOfDeath);
        artworkData.principalMakers[0].occupation = addNotAvailableText(artworkData.principalMakers[0].occupation);
        artworkData.materials = addNotAvailableText(artworkData.materials);
        artworkData.acquisition.creditLine = addNotAvailableText(artworkData.acquisition.creditLine);
        artworkData.objectCollection = addNotAvailableText(artworkData.objectCollection);

        res.render('artwork', { artwork: artworkData });
    } catch (error) {
        console.error('Fout bij het ophalen van het kunstwerk:', error.message);
        res.status(500).json({ error: 'Er is een fout opgetreden bij het ophalen van het kunstwerk' });
    }
});
```

## Opzetten EJS sjablonen

Na het fetchen van de data ben ik de EJS sjablonen gaan opzetten, zodat ik de data die ik gefetched heb in de HTML kan gaan zetten. 

### index.ejs sjabloon

Dit sjabloon dient als de homepage van mijn applicatie.

Ik heb eerst een unordered list aangemaakt waarin ik een aantal schilderijen die ik gefetched heb ben gaan zetten. De JavaScript lus gaat door elk schilderij in de artworks array. Ik heb hierin een list item neergezet, waarin elk schilderij wordt weergegeven. Ik geef dan dynamisch de titel en een afbeelding van het schilderij weer aan de hand van <%= artwork.title %> en <%= artwork.imageUrl %>. 

```html
<ul id="artworksList" class="artworks-list">
    <% artworks.forEach(artwork => { %>
        <li class="artwork-item">
            <h2><%= artwork.title %></h2>
            <img src="<%= artwork.imageUrl %>" alt="<%= artwork.title %>">
        </li>
    <% }); %>
</ul>
```

### artwork.ejs sjabloon

Dit sjabloon dient als detailpagina voor mijn applicatie.

Dit sjabloon is vrij gelijk aan hoe ik de index.ejs sjabloon heb opgezet. Het enige verschil is dat ik geen lus die door elk schilderij itereerd hoef te ontwerpen, aangezien dit sjabloon alleen als detail dient voor een schilderij. Dus wanneer iemand op een schilderij in de homepagina klikt, wordt hij of zij gebracht naar deze detailpagina, waarin extra informatie over het schilderij en de schilder staat.

```html
<div class="artwork-details">
      <div class="header">
          <img src="<%= artwork.webImage.url %>" alt="<%= artwork.title %>">
          <h1><%= artwork.title %></h1>
      </div>
      <div class="contents">
          <p><span class="maker">Maker</span><span class="maker content naam"> <%= artwork.principalMakers[0].name %></span></p>
          <p class="meer-info"><span class="maker info">Geboorteplek</span><span class="maker content"> <%= artwork.principalMakers[0].placeOfBirth %></span></p>
          <p class="meer-info"><span class="maker info">Geboortedatum</span><span class="maker content"> <%= artwork.principalMakers[0].dateOfBirth %></span></p>
          <p class="meer-info"><span class="maker info">Sterfplek</span><span class="maker content"> <%= artwork.principalMakers[0].placeOfDeath %></span></p>
          <p class="meer-info"><span class="maker info">Sterfdatum</span><span class="maker content"> <%= artwork.principalMakers[0].dateOfDeath %></span></p>
          <p class="meer-info"><span class="maker info">Bezigheid</span><span class="maker content"> <%= artwork.principalMakers[0].occupation %></span></p>
          <p><span class="beschrijving">Beschrijving</span><span class="content"> <%= artwork.description %> </span></p>
          <p><span class="jaar-van-creatie">Jaar van creatie</span><span class="content"> <%= artwork.dating.presentingDate %> </span></p>
          <p><span class="materials">Materialen gebruikt</span><span class="content"> <%= artwork.materials %> </span></p>
          <p><span class="acquisition">Herkomst</span><span class="content"> <%= artwork.acquisition.creditLine %> </span></p>          
          <p><span class="collectie">Collectie</span><span class="content"> <%= artwork.objectCollection %> </span></p>
      </div>
  </div>
```

# Week 2

## Weergeven kunstwerken

Met de volgende code, die in mijn client-side bestand staat, genaamd index.js, weergeef ik elk schilderij, zowel wanneer de gebruiker op de homepagina land (met de functie displayArtworks) als wanneer de gebruiker een schilderij opzoekt (met de functie  searchArtworks). Dit is client-side code, omdat de de code wordt uitgevoerd in de webbrowser van de gebruiker, nadat de webpagina geladen is. Dit wordt direct uitgevoerd op de computer of het apparaat van de gebruiker, in plaats van de server-side code in mijn app.js bestand staat die op de server wordt uitgevoerd voordat de webpagina wordt verzonden naar de client. 

De 'displayArtworks' functie is verantwoordelijk voor het weergeven van een lijst met kunstwerken op de webpagina op basis van de gegevens die worden doorgegeven aan de functie. artworksList haalt het ul element op waarin de schilderijen worden weergegeven en noResultsMessage haalt een bericht op dat wordt weergegeven wanneer de gebruiker naar een schilderij zoekt dat niet in de database staat. artworksList wordt geleegd door de innerHTML leeg te maken, wat er voor zorgt dat eerdere zoekresultaten worden verwijderd voordat er nieuwe resultaten worden toegevoegd. Wanneer de gebruiker iets opzoekt, en het resultaat matched niet met een van de schilderijen, wordt er een lege array teruggestuurd naar de client, waardoor de 'displayArtworks' functie controleert of de array leeg is. De voorwaarde 'if (artworks.length === 0)' zal dan kloppen, en het bericht 'Geen zoekresultaten gevonden' wordt dan weergegeven. Als de gebruiker iets opzoekt, en het resultaat matched wel met een van de schilderijen, wordt het bericht 'Geen zoekresultaten gevonden' verborgen en wordt er voor elk kunstwerk in de array een li element aangemaakt en aan de ul artworksList toegevoegd met daarin informatie over het schilderij.

De 'searchArtworks' functie is verantwoordelijk voor het ophalen van zoekresultaten op basis van een opgegeven zoekopdracht. Binnen deze functie wordt er een HTTP verzoek naar de server gedaan met de zoekopdracht als query. Als de zoekopdracht overeenkomt met een schilderij, wordt de functie 'displayArtworks' aangeroepen met de schilderijen die zijn opgehaald uit de server, die vervolgens de schilderijen weergeeft die overeenkomen met de query. 

```js
function displayArtworks(artworks) {
    const artworksList = document.getElementById('artworksList');
    const noResultsMessage = document.getElementById('noResultsMessage');
    artworksList.innerHTML = '';
    if (artworks.length === 0) {
        noResultsMessage.style.display = 'flex';
    } else {
        noResultsMessage.style.display = 'none';
        artworks.forEach(artwork => {
            const li = document.createElement('li');
            li.innerHTML = `
                <h2>${artwork.title}</h2>
                <p class="maker-p"><span class="maker">Maker</span> ${artwork.principalOrFirstMaker}</p>
                <img src="${artwork.webImage.url}" alt="${artwork.title}">
            `;
            li.addEventListener('click', () => openPopup(artwork));
            artworksList.appendChild(li);
        });
    }
}
```

```js
async function searchArtworks(query) {
    try {
        const response = await fetch(`/search?title=${query}`);
        const data = await response.json();
        displayArtworks(data.artworks);
    } catch (error) {
        console.error('Fout bij het zoeken naar kunstwerken:', error.message);
    }
}

const searchInput = document.querySelector('input[name="title"]');
searchInput.addEventListener('input', (event) => {
    const query = event.target.value.trim(); 
    if (query.length > 0) {
        searchArtworks(query); 
    } else {
        searchArtworks(''); 
    }
});
```

## Styling

Om de app wat leuker en gebruiksvriendelijker te maken heb ik styling toegevoegd aan de hand van CSS.

# Week 3

## Web Animations API

Om mijn applicatie een stap interessanter te maken heb ik aan de hand van de Web Animations API en de ScrollTimeline API een aantal animaties toegevoegd, waaronder een scroll tracker op de detail pagina van een schilderij om weer te geven hoe ver in de pagina de gebruiker zich bevindt.

De eerste animatie, de scroll tracker, heb ik op de volgende manier geimplementeerd.
Ik heb een functie aangemaakt die een animatie aanstuurt op basis van de scrollpositie van de gebruiker. Ik selecteer een div die ik heb aangemaakt in 'artwork.ejs' om deze van onzichtbaar te maken (de gebruiker zit bovenin op de pagina) naar volledig zichtbaar te maken (de gebruiker is volledig door de pagina gescrollt). Dit doe ik aan de hand van de ScrollTimeline API, die het mogelijk maakt om te reageren op het scrollen van de gebruiker en dus animaties hieraan kan verbinden. 'source: document.scrollingElement' geeft aan dat de animatie werkt als je over het hele document scrollt. orientation: 'block' geeft aan dat het scrollen verticaal werkt. scrollOffsets: [CSS.percent(0), CSS.percent(100)] geeft aan dat de animatie helemaal bovenaan de pagina begint, en onderaan de pagina eindigt. Dit zorgt er dus voor dat de 'Scroll Tracker' div de volledige width krijgt zodra de gebruiker tot het einde van de pagina is gescrollt. 

```js
function setupScrollTracking() {
    const scrollTracker = document.querySelector('.scroll-tracker');
    const scrollTrackingTimeline = new ScrollTimeline({
        source: document.scrollingElement,
        orientation: 'block',  
        scrollOffsets: [CSS.percent(0), CSS.percent(100)]
    });

    scrollTracker.animate(
        {
            transform: ['scaleX(0)', 'scaleX(1)']
        },
        {
            duration: 1,
            timeline: scrollTrackingTimeline,
        }
    );
}

document.addEventListener('DOMContentLoaded', setupScrollTracking);
```

# Week 4

## READ.ME bijwerken (#read-me-bijwerken)
