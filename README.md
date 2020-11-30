# Keksdosenalarmanlage
## ~avatar avatar @unplugged
![keksAlarm](https://github.com/r00b1nh00d/einfacheAlarmanlage/blob/master/Keksdose.gif?raw=true) <br>
Du hast schon Weihnachtsplätzchen gebacken aber Angst, dass sie dir jemand wegessen könnte?
Sichere sie jetzt mit deiner Calliope Alarmanlage!!! 

## ~ @unplugged
Du kannst die LED's am @boardname@ nutzen um ungefähr zu messen wie stark das Licht auf den @boardname@ scheint. <br>
Wenn du jetzt diese Lichtstärke misst während dein @boardname@ in einer verschlossen Box mit den Weihnachtsplätzchen liegt, <br>
 kann er das Einfallen von Licht beim öffnen der Box feststellen und darauf einen Signalton abgeben, sobald die Box geöffnet wird.



## Schritt 1 Lichtstärke messen mit dem Calliope mini
Im ersten Schritt geht es darum die Lichtstärke zu messen und ein Gefühl für die Werte bei Lichteinfall und im dunkeln zu bekommen. <br>
Nutze dazu die ``||basic:dauerhaft||`` - Schleife. In diese kommt eine ``||logic: wenn- dann||`` - Bedingung aus dem Bereich ``||logic:Logik||``. <br>
In die Bedingung (also die kleine Raute im Block ) kommt ein größer-kleiner Vergleich. Diesen findest du ebenfalls im Bereich ``||logic:Logik||``. <br>
In diesen Vergleich kommt noch der Block zum messen der ``||inputs:Lichtstärke||``  diesen findest du im Bereich ``||inputs:Eingabe||`` <br>
Als Anweisung also wenn die Bedingung erfüllt ist, soll ein Ton mittels c abgespielt werden. Diesen findest du im Bereich ``||music: Musik||`` <br>
Setze vor dem Test noch eine Zahl wie z.B. 50 in den Vergleich dieser Wert könnte schon passen. Jeztzt kannst du das Programm schonmal auf deinen Calliope laden und testen was passiert wenn du die Hand über die LED-Anzeige hälst. <br>
Dies kannst du auch mehrmals mit verschiedenen Zahlen versuchen und schauen welche am besten passt. 

```blocks
basic.forever(function () {
    if (input.lightLevel() > 50) {
        music.playTone(262, music.beat(BeatFraction.Whole))
    }
})
```



## Schritt 2 Die Alarmanlage Scharfschalten und wieder abschalten
Wenn du dir selbst ein Plätzchen aus der Box heraus holst möchtest du die Alarkanlage vielleicht auch mal abschalten, damit nicht ständig dein Signalton abgespielt wird. <br>
Aber auch sollte der Signalton nach kurzem öffnen von fremden Personen weiter abgespielt werden, sollten diese die Box vor schreck direkt wieder zu machen. <br>
Dies kannst du über eine Variable nennen wir sie ``||variables: ScharfGeschaltet||`` steuern. Erstelle dazu diese Vatiable und `` ||basic: beim Start||`` ``||variables: setze ScharfGeschaltet auf ||`` ``||logic: falsch||``. <br>
Um diese Variable zu ändern nehmen wir aus dem Bereich ``||input: Eingabe||`` den Block ``||input : Wenn knopf A+B gedrückt||``. <br>
In diesen Block soll eine ``||logic: wenn- dann- ansonsten||`` - Bedingung um zu entscheiden ob die Varibale gerade auf  ``||logic: wahr||`` oder ``||logic: falsch||`` gesetzt ist. <br>
So soll, wenn die Variable ``||variables: ScharfGeschaltet||`` auf ``||logic: wahr||`` ist auf ``||logic: falsch||`` ``||variables: gesetzt werden||``. <br>
Ansonsten soll die auf ``||logic: wahr||`` gesetzt werden. <br>
Um die Alarmanlage jetzt nach einer Lichterkennung weiter den Ton abspielen zu lassen auch wenn die Box wieder geschlossen wird, nehme aus dem Bereich ``||loops: Schleifen||`` die ``||loops: Während - Schleife||``. <br>
Baue sie so in dein Programm ein, dass sie den Block ``||music: spiele Note||`` umschließt. Für einen besseres Alarmsignal kanst auch den Ton Mittleres F abwechselnd mit dem mittleren C in dieser Schleife abspielen lassen.
``|| basic: beim Start||`` sollte die Variable ``||variables: ScharfGeschaltet||`` auf ``||logic: falsch||`` ``||variables: gesetzt werden||``. <br>


```blocks
let ScharfGeschaltet = false

input.onButtonPressed(Button.AB, function () {
  
    if (ScharfGeschaltet) {
        ScharfGeschaltet = false
    } else {
        ScharfGeschaltet = true
    }
})

basic.forever(function () {
 
    if (ScharfGeschaltet) {

        if (input.lightLevel() > 50) {
            while (ScharfGeschaltet) {
               
                music.playTone(262, music.beat(BeatFraction.Whole))
                music.playTone(349, music.beat(BeatFraction.Whole))
            }
        }
    }
})

``` 

## Schritt 3 Einschaltverzögerung
Dir ist jetzt vielleicht aufgefallen, dass die Alarmanlage sofort anspringt, wenn der Schwellwert überschritten wurde. Das macht es ziemlich schwer die Calliope Alarmanlage in die Box zu legen, ohne dass diese schon auslöst. 
Dazu bauen wir in den ``||basic: dauerhaft||`` Block noch vor der Abfrage ob die Alarmanlage scharf geschaltet ist eine weiter Abfrage ein welche einen ``||variables: timer||`` Status checken soll.
Ist der ``||variables: timer||`` gleich 1 soll eine Sekunde gewartet werden und der Timer auch gleich wieder auf 0 gesetzt werden. <br>
Den Timer lassen wir ebenfalls mit einem Knopfdruck von A und B auf 1 setzen. 
 
```blocks
input.onButtonPressed(Button.AB, function () {
    if (ScharfGeschaltet) {
        ScharfGeschaltet = false
    } else {
        ScharfGeschaltet = true
    }
    timer = 1
})
let timer = 0
let ScharfGeschaltet = false
ScharfGeschaltet = false
basic.forever(function () {
    if (timer == 1) {
        basic.pause(1000)
    }
    if (ScharfGeschaltet) {
        basic.setLedColor(0x00ff00)
        if (input.lightLevel() > 50) {
            while (ScharfGeschaltet) {
                basic.setLedColor(0xff0000)
                music.playTone(262, music.beat(BeatFraction.Whole))
                music.playTone(349, music.beat(BeatFraction.Whole))
            }
        }
    }
})

```



## Als Tutorial verwenden

Dieses Repository kann als **Tutorial** für MakeCode verwenden.

öffne dazu den Link: [https://makecode.calliope.cc/#tutorial:https://github.com/r00b1nh00d/einfacheAlarmanlage]

#### Metadaten (verwendet für Suche, Rendering)

* for PXT/calliopemini
<script src="https://makecode.com/gh-pages-embed.js"></script><script>makeCodeRender("{{ site.makecode.home_url }}", "{{ site.github.owner_name }}/{{ site.github.repository_name }}");</script>
