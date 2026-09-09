<!--

author:    André Dietrich
email:     LiaScript@web.de
version:   1
language: en
narrator: US English Female
comment:   Natives p5.js Template, das LiaScripts eingebaute Versionierung nutzt
logo:      https://p5js.org/assets/img/p5js.svg
script:    https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.2.0/p5.min.js
persistent: true

@P5.eval: @P5.eval_(@uid)

@P5.eval_
<script>
if (window.p5_instances) {
    for (let key in window.p5_instances) {
        try { window.p5_instances[key].remove(); } catch(e) {}
    }
}
window.p5_instances = {};

let div = document.getElementById('p5-@0');
if (div) div.innerHTML = "";

let sketch = function(p5) {
    for (let key in p5) {
        if (typeof p5[key] === 'function') {
            window[key] = p5[key].bind(p5);
        } else {
            try {
                Object.defineProperty(window, key, {
                    get: () => p5[key],
                    set: (val) => { p5[key] = val; },
                    configurable: true
                });
            } catch(e) {}
        }
    }

    window.print = function() {
        console.log.apply(console, arguments);
    };

    // Hier übergibt LiaScript automatisch die vom Nutzer ausgewählte Version aus der History
    try {
        window.eval(`@'input`);
    } catch (e) {
        console.error(e);
    }

    if (typeof window.setup === 'function') {
        let userSetup = window.setup;
        p5.setup = function() { userSetup(); };
        window.setup = undefined; 
    }
    
    if (typeof window.draw === 'function') {
        let userDraw = window.draw;
        p5.draw = function() { userDraw(); };
        window.draw = undefined; 
    }
};

window.p5_instances['@0'] = new p5(sketch, div);

"LIA: stop"
</script>

<div id="p5-@0" class="persistent"></div>
@end

-->

# Willkommen im Code-Atelier: Eure erste eigene Grafik mit p5.js!

Hallo zusammen! Schön, dass ihr da seid. Für die meisten von euch ist Informatik in diesem Jahr ein brandneues Fach. Vielleicht denkt ihr bei "Programmierung" an komplizierte grüne Textzeilen aus Hollywood-Hacker-Filmen. Keine Sorge: Wir fangen ganz entspannt an. 

Heute werdet ihr zu digitalen Künstlerinnen und Künstlern. Wir nutzen dafür **p5.js** – das ist eine Programmierumgebung, mit der man direkt im Browser zeichnen, animieren und coden kann. Ihr braucht dafür keine Vorkenntnisse, nur ein bisschen Neugier!

Bevor wir loslegen, ein winziger Blick auf den Grundaufbau eines p5.js-Programms. Jedes Skript hat zwei Hauptbereiche:
* `setup()`: Das wird **einmal** ganz zu Beginn ausgeführt (wie das Vorbereiten eures Arbeitsplatzes).
* `draw()`: Das läuft danach in einer Dauerschleife (wie ein Daumenkino, das immer weiterzeichnet).

---

## Modul 1: Die digitale Leinwand & Eure erste Nachricht

### 1. Das Problem aus eurem Alltag
Stellt euch vor, ihr wollt ein neues Poster für euer Zimmer kaufen oder ein Hintergrundbild für euer Smartphone einrichten. Das Erste, was ihr wissen müsst, ist: **Wie groß muss es sein?** Und wenn ihr eurem Smartphone-Betriebssystem sagen wollt: "Hey, mach das Bild bitte komplett rot!", müsst ihr ihm befehlen, welche Farbe es nutzen soll. 

In der Informatik ist das genauso. Der Computer weiß nicht automatisch, wie groß euer "Bildschirm" sein soll und startet mit einer leeren, weißen Fläche.

### 2. Die Lösung in p5.js
Wir müssen dem Computer zuerst sagen, wie groß unsere digitale Leinwand sein soll und welche Farbe der Hintergrund haben hat. Und weil wir am Anfang oft Fehler machen, lernen wir direkt, wie der Computer uns Nachrichten schicken kann.

Dafür nutzen wir diese drei Befehle im `setup()`:

* `createCanvas(width, height)`: Erstellt ein Zeichenfenster mit einer Breite (`width`) und Höhe (`height`) in Pixeln (Bildpunkten).
* `background(gray)` oder `background(r, g, b)`: Färbt den Hintergrund. Ein einzelner Wert (0 bis 255) gibt einen Grauwert an (0 = Schwarz, 255 = Weiß). Drei Werte stehen für **R**ot, **G**rün und **B**lau (RGB-Farbraum).
* `print(value)`: Schreibt einen Text oder Wert in die unsichtbare Entwickler-Konsole. Das ist perfekt, um zu prüfen, ob das Programm überhaupt bis zu dieser Stelle läuft (man nennt das *Debugging*).

> **Achtung, Geometrie-Falle!** Im Mathe-Unterricht ist der Nullpunkt (0,0) unten links und die Y-Achse geht nach oben. Beim Programmieren ist der Nullpunkt **oben links**! Die X-Achse geht nach rechts, aber die Y-Achse geht **nach unten**!

``` js
function setup() {
  createCanvas(400, 300); // 400 Pixel breit, 300 Pixel hoch
  background(255, 0, 0);  // Ein knalliges Rot!
  print("Die Leinwand steht!"); // Geheime Nachricht an uns
}
```
@P5.eval

### 3. Schneller Check: Habt ihr es drauf?

Welcher Befehl erstellt eine quadratische Leinwand, die 500 Pixel breit und hoch ist?

[( )] `createCanvas(500);`
[( )] `createCanvas(280, 500);`
[(X)] `createCanvas(500, 500);`
[[?]] Es gibt keine Kurzform, wenn die Leinwand quadratisch sein soll.
******
**Bemerkung** Eine kleine Bermerkung am Rande wenn das Quiz vorbei ist.
******


Wo befindet sich der Koordinatenpunkt `(0, 300)` bei einer Leinwand, die `createCanvas(400, 300)` groß ist?

[( )] Oben links
[( )] Unten links
[(X)] Unten links an der Ecke (ganz unten auf der Y-Achse)
[( )] Oben rechts

### 4. Mini-Aufgabe (Direkt ausprobieren)
Hier ist ein fehlerhafter Code. Passt ihn so an, dass:
1. Die Leinwand **600 Pixel breit** und **400 Pixel hoch** wird.
2. Der Hintergrund komplett **Schwarz** wird.
3. In der Konsole euer Vorname ausgegeben wird.

``` js
function setup() {
  // ÄNDERE MICH!
  createCanvas(200, 200);
  background(255);
  print("Hallo");
}
```
@P5.eval

### 5. Challenge für Profis / Hausaufgabe
Öffnet den offiziellen [p5.js Web Editor](https://editor.p5js.org/). 
Erstellt eine Leinwand im typischen Kino-Format (16:9), zum Beispiel `1600` mal `900` Pixel. Färbt den Hintergrund in einem sanften Grau. Nutzt den `print()`-Befehl, um das aktuelle Jahr in der Konsole auszugeben. 
*Tipp: Schaut genau hin, wo unten im Editor das Feld "Console" ist!*

---

## Modul 2: Die Grundformen (Formen zeichnen)

### 1. Das Problem aus eurem Alltag
Ihr wollt ein einfaches Emoji zeichnen, ein Haus auf einem Schild skizzieren oder ein Interface für ein Game (wie eine Lebensanzeige) bauen. Alles um uns herum besteht aus geometrischen Grundformen: Kreisen, Quadraten und Linien. Der Computer braucht präzise Anweisungen, wo diese Formen starten und wie groß sie sein sollen.

### 2. Die Lösung in p5.js
Wir wechseln nun in den `draw()`-Bereich, da wir hier unsere Formen platzieren. Wir lernen drei wichtige Werkzeuge kennen:

* `circle(x, y, diameter)`: Zeichnet einen Kreis. `x` und `y` bestimmen den Mittelpunkt, `diameter` ist der Durchmesser.
* `rect(x, y, width, height)`: Zeichnet ein Rechteck. **Wichtig:** Standardmäßig sind `x` und `y` die **obere linke Ecke** des Rechtecks!
* `line(x1, y1, x2, y2)`: Zieht eine gerade Linie vom Startpunkt `(x1, y1)` zum Endpunkt `(x2, y2)`.

``` js
function setup() {
  createCanvas(400, 400);
}

function draw() {
  background(220); // Grauer Hintergrund
  
  circle(200, 200, 100);    // Kreis genau in der Mitte
  rect(50, 50, 100, 75);    // Ein Rechteck oben links
  line(0, 0, 400, 400);     // Eine Diagonale von ganz oben links nach ganz unten rechts
}
```
@P5.eval

### 3. Schneller Check: Habt ihr es drauf?

Ein Rechteck wird mit `rect(10, 20, 50, 100);` gezeichnet. Welche Aussage stimmt?

[(X)] Die obere linke Ecke des Rechtecks liegt bei X=10 und Y=20.
[( )] Das Rechteck ist 100 Pixel breit.
[( )] Das Rechteck ist ein perfektes Quadrat.

Wenn du eine Linie zeichnen willst, die genau waagerecht (horizontal) von links nach rechts verläuft, was muss erfüllt sein?

[( )] Die X-Werte müssen gleich sein (`x1 == x2`).
[(X)] Die Y-Werte müssen gleich sein (`y1 == y2`).
[( )] Alle vier Werte müssen unterschiedlich sein.

### 4. Mini-Aufgabe (Direkt ausprobieren)
In dem folgenden Code versucht jemand, ein einfaches "Haus" (ein Viereck mit einem Strich als Dach) zu bauen, aber die Maße stimmen hinten und vorne nicht. Verschiebe die Koordinaten der Linie so, dass sie exakt als Flachdach auf dem Rechteck liegt!

``` js
function setup() {
  createCanvas(400, 400);
}

function draw() {
  background(240);
  
  // Das Haus-Fundament
  rect(100, 200, 200, 150);
  
  // Das Dach (Aktuell völlig falsch platziert!)
  // Setze die Koordinaten so, dass die Linie von der linken oberen Ecke des Rechtecks zur rechten oberen Ecke geht!
  line(0, 0, 100, 100); 
}
```
@P5.eval

### 5. Challenge für Profis / Hausaufgabe
Programmiere im p5.js-Editor das Gesicht eines Roboters. Er soll einen großen, quadratischen Kopf haben, zwei kreisrunde Augen und einen geraden Strich als Mund. 
*Zusatzaufgabe für Fleißige: Gib ihm mit zwei Linien noch ein paar coole Antennen auf dem Kopf!*

---

## Modul 3: Farbe & Style (fill, stroke, strokeWeight)

### 1. Das Problem aus eurem Alltag
Ein Spiel ohne Skins, Texturen oder Farben wäre sterbenslangweilig. Wenn ihr in Fortnite einen Skin wählt oder bei Instagram einen Filter nutzt, ändert ihr Farben und Linienstärken. Bisher sind all unsere Formen in p5.js weiß mit einer dünnen, schwarzen Kontur. Das ändern wir jetzt!

### 2. Die Lösung in p5.js
Stellt euch vor, ihr habt vor euch drei Stifte liegen: Einen dicken Edding für den Rand, eine Pinselfarbe zum Ausfüllen und einen Buntstift für die Kontur. Bevor ihr eine Form zeichnet, müsst ihr dem Computer sagen, welchen Stift er "in die Hand nehmen" soll.

* `fill(r, g, b)`: Setzt die Füllfarbe für alle Formen, die **danach** gezeichnet werden.
* `stroke(r, g, b)`: Setzt die Farbe für die Umrandung (Kontur).
* `strokeWeight(weight)`: Bestimmt, wie dick die Umrandung in Pixeln sein soll.

> **Wichtig:** Der Befehl muss **vor** der Form stehen! Wenn ihr erst `circle()` schreibt und danach `fill()`, wird der Kreis weiß bleiben und erst die nächste Form bunt.

``` js
function draw() {
  background(255);

  stroke(0, 0, 255);      // Blaue Kontur
  strokeWeight(5);        // Ziemlich dicke Linie
  fill(255, 255, 0);      // Gelbe Füllung
  
  circle(200, 200, 80);   // Dieser Kreis wird gelb mit blauem Rand!
}
```
@P5.eval

### 3. Schneller Check: Habt ihr es drauf?

Wie mischt man in der Informatik (RGB) die Farbe **Grün**?

[( )] `fill(255, 0, 0);`
[(X)] `fill(0, 255, 0);`
[( )] `fill(0, 0, 255);`
[( )] `fill(255, 255, 255);`

Was passiert, wenn du folgenden Code ausgeführt hast?
`circle(100, 100, 50);`
`fill(255, 0, 0);`

[(X)] Der Kreis bleibt weiß, weil die Farbe zu spät ausgewählt wurde.
[( )] Der Kreis wird sofort rot.
[( )] Das Programm stürzt ab.

### 4. Mini-Aufgabe (Direkt ausprobieren)
Wir wollen eine Ampel bauen, die aktuell "Rot" anzeigt. Verändere den folgenden Code so, dass das obere Licht rot leuchtet und das untere Licht dunkelgrau (z.B. Grauwert 50) bleibt. Vergiss nicht, die Konturen der Ampellichter komplett schwarz und etwas dicker zu machen!

``` js
function setup() {
  createCanvas(200, 400);
}

function draw() {
  background(200);
  
  // Das Ampolgehäuse
  fill(30);
  rect(50, 50, 100, 250);
  
  // OBERES LICHT (Soll ROT werden!)
  circle(100, 110, 60);
  
  // UNTERES LICHT (Soll DUNKELGRAU werden!)
  circle(100, 240, 60);
}
```
@P5.eval

### 5. Challenge für Profis / Hausaufgabe
Baut im p5.js-Editor eine Schweizer Flagge (oder eine andere Flagge eurer Wahl). Die Schweizer Flagge hat einen roten Hintergrund und ein weißes Kreuz in der Mitte. Das Kreuz könnt ihr aus zwei dicken, weißen Linien (`stroke` und `strokeWeight`) oder zwei Rechtecken bauen. Achtet penibel darauf, welcher Befehl vor welchem stehen muss!

---

## Modul 4: Der Profi-Ausrichtungstrick (rectMode)

### 1. Das Problem aus eurem Alltag
Stellt euch vor, ihr wollt ein Bild an der Wand aufhängen und euer Kumpel sagt: "Häng es genau in die Mitte der Wand!". Wenn ihr jetzt von der oberen linken Ecke des Bildes aus messen müsst, müsst ihr kompliziert rechnen (Wandmitte minus halbe Bildbreite...). Es wäre viel einfacher, wenn ihr einen Nagel genau in die Mitte des Bildes schlagen und es dort aufhängen könntet.

Bei Kreisen macht p5.js das automatisch (der Punkt X/Y ist das Zentrum). Bei Rechtecken fängt p5.js aber immer oben links an zu zeichnen. Das kann nerven, wenn man ein Rechteck exakt mittig platzieren will.

### 2. Die Lösung in p5.js
Mit dem Befehl `rectMode(MODE)` können wir das Verhalten von p5.js komplett umstellen!

* `rectMode(CORNER)`: Das ist der Standard. `rect(200, 200, 50, 50)` startet oben links bei (200,200).
* `rectMode(CENTER)`: Schaltet um auf den "Zentrums-Modus". `rect(200, 200, 50, 50)` platziert nun den **Mittelpunkt** des Quadrats exakt auf (200,200).

``` js
function draw() {
  background(220);
  
  rectMode(CENTER); // Ab jetzt wird aus der Mitte heraus gezeichnet!
  
  fill(255, 0, 0);
  rect(200, 200, 100, 100); // Quadrat genau im Zentrum der Leinwand
  
  fill(255);
  circle(200, 200, 50);     // Kreis genau im Zentrum des Quadrats – ohne Rechnen!
}
```
@P5.eval

### 3. Schneller Check: Habt ihr es drauf?

Wenn `rectMode(CENTER);` aktiv ist, was bewirkt der Befehl `rect(0, 0, 100, 100);`?

[( )] Das Quadrat wird unsichtbar.
[(X)] Das Quadrat wird so gezeichnet, dass sein Mittelpunkt genau in der oberen linken Ecke (0,0) der Leinwand liegt. Man sieht also nur ein Viertel des Quadrats.
[( )] Es verhält sich genau wie im Modul 2.

### 4. Mini-Aufgabe (Direkt ausprobieren)
Hier wird versucht, eine Zielscheibe (Bullseye) zu bauen. Ein Quadrat und ein Kreis sollen exakt übereinanderliegen. Da `rectMode(CENTER)` fehlt, verrutscht das Quadrat völlig. Füge den richtigen Befehl an der passenden Stelle ein, um das Quadrat zu zentrieren!

``` js
function setup() {
  createCanvas(400, 400);
}

function draw() {
  background(240);
  
  // FÜGE HIER DEN ERFORDERLICHEN BEFEHL EIN!
  
  // Das äußere Quadrat
  fill(0);
  rect(200, 200, 150, 150);
  
  // Der innere Kreis
  fill(255, 0, 0);
  circle(200, 200, 100);
}
```
@P5.eval

### 5. Challenge für Profis / Hausaufgabe (Das Meisterstück)
Loggt euch in den p5.js-Editor ein. Eure Aufgabe ist es, das Logo einer bekannten Smartphone-App oder ein cooles Icon zu designen (z. B. das Instagram-Logo oder ein einfaches Gamepad-Icon). 

Nutzt dafür zwingend:
* `rectMode(CENTER)` für perfekt geschachtelte Quadrate.
* Mindestens einen `circle()`.
* Unterschiedliche Farben (`fill`) und dicke Ränder (`strokeWeight`).
* Schickt euren Code-Link bis zur nächsten Stunde im Schulportal ein!

---
**Geschafft!** Ihr habt die absoluten Fundamente der Computergrafik gelernt. Beim nächsten Mal bringen wir Bewegung in die Sache und lassen die Formen über den Bildschirm fliegen!