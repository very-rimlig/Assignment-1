# Klassföräldrar till 2 B

## Översikt
Den här lilla webbappen visar en klocka med hälsning och en funktion som slumpmässigt plockar namn ur en lista och loggar tidigare val i en historiklista. Det här görs bland annat med DOM-manipulation och eventhantering.


---

## Filstruktur (exempel)
- "index.html" = huvudsidan (och den enda html-sidan) med strukturen och plats för dynamiskt innehåll.
- "style.css" = all styling för layout, kort och knappar.
- "app.js" = JavaScript-filen som skapar funktionaliteten och interaktiviteten för  klocka, hälsning, slumparen av namn + slump-historiken.
- "readme.md" = denna fil, som förklarar vad sidan gör, codeflow,  hur den är uppbyggd
+i de olika filerna finns även utförliga kommentarer

-----------------------------------------------------------------------------------------------------

## Roll för varje fil
**index.html**  
Innehåller statiska HTML-element och element med unika id:n som JavaScript använder: time, greeting, current-name, new-name, clear-history, name-history. Skriptet laddas sist i <body> för att säkerställa att DOM-trädet redan är byggt.

**style.css**  
Styr typografi, layout, kortstilar, knappstilar och responsivitet.

**app.js**  
Innehåller all dom-manipulerande logik:
  - Uppdaterar klockan varje sekund och väljer lämplig hälsning.
  - Slumpar namn från arrayen "names".
  - Skapar <li>/list-element för historiken och lägger in en Ta bort-knapp för varje post.
  - Rensar hela historiken vid knapptryck.
  - Stoppar intervallet vid unloading.

----------------------------------------------------------------------------------------------------------------

## Hur filerna är kopplade
1. "index.html" länkar till "style.css" i <head> och laddar innehållet i filen app.js sist i <body>.
2. När skriptet körs hämtar den DOM-element med document.getElementById.... och sparar referenser i variabler (timeEl, greetingEl, currentNameEl, etc.).
3. JavaScript registrerar event-lyssnare för knapparna och startar setInterval för klockuppdatering.

-----------------------------------------------------------------------------------------------------------------

## Vad händer när användaren laddar sidan/kodflöde:
1. Browsern hämtar index.html och style.css och sidan renderas initialt med grundlayout.

2. När parsern/tolkningen når <script src="js/app.js"></script>` (sista i body) så laddas och körs skriptet direkt eftersom DOM redan är byggd, istället för att scriptet ska köras innan DOM är byggt.
Det gör att document.getElementById hittar element direkt utan att behöva vänta.

3. Skriptet hämtar viktiga element via getElementById och kör updateClockAndGreeting() en gång omedelbart så att användaren inte väntar 1 sekund.

4. setInterval(updateClockAndGreeting, 1000) startar — klockan och hälsningen uppdateras varje sekund.

5. Event-lyssnare:
   * new-name - anropar showNewName() som:
     - plockar ett slumpmässigt namn från names-arrayen/listan,
     - uppdaterar current-name,
     - skapar ett nytt <li> med en inre "Ta bort"-knapp och sätter det överst i name-history.
   - "Ta bort"-knappen (per li) tar bort just den posten/namnet från historiken (name-history).
   - clear-history tömmer hela name-history genom att ta bort alla poster/namn (child-noder).
6. beforeunload lyssnaren rensar clockInterval (stoppar setInterval) när användaren lämnar sidan.

----------------------------------------------------------------------------------------------------------------


## Så kör du appen:
1. Öppna filen "index.html" i en webbläsare (eller på annat sätt).
2. Kolla så att:
   - Klockan i time visar aktuell tid och uppdateras varje sekund.
   - Hälsningen i greeting ändras beroende på tid på dygnet.
   - Klicka på "Slumpa en förälder!", då ska current-name uppdateras och en rad läggs in i name-history.
   - Klicka Ta bort-knappen på ett slumpat namn = endast den raden tas bort.
   - Klicka Rensa alla namn = hela listan töms.


