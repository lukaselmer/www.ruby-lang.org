---
layout: page
title: "Ruby in 20 Minuten"
lang: de
---

## Einleitung

Dies ist eine kleine Ruby-Anleitung, für die man nicht mehr als 20
Minuten zum Lesen benötigen sollte. Voraussetzung ist, dass Du Ruby
bereits installiert hast. (Falls nicht, solltest Du vor dem Weiterlesen
[Ruby herunterladen](/de/downloads/) und installieren.)

## Interaktives Ruby

Zuerst starten wir die interaktive Ruby-Konsole: IRB.

* Wenn Du **Mac OS X** benutzt, öffne `Terminal`, gib `irb` ein und
  drücke Enter.
* Wenn Du **Linux** benutzt, öffne eine Shell, gib `irb` ein und drücke
  Enter.
* Wenn Du **Windows** benutzt, öffne `fxri` aus dem Ruby-Ordner des
  Start-Menüs heraus.
^

    irb(main):001:0>

So, IRB ist nun gestartet. Wie geht es weiter?

Gib folgendes ein: `"Hallo, Welt!"`

    irb(main):001:0> "Hallo, Welt!"
    => "Hallo, Welt!"

## Ruby hört auf Dich!

Was ist gerade passiert? Haben wir etwa gerade das kürzeste “Hallo,
Welt!”-Programm der Welt geschrieben? Nun, nicht direkt: Mit der zweiten
Zeile teilt uns IRB lediglich das Ergebnis des letzten ausgewerteten
Ausdrucks mit. Wenn wir “Hallo, Welt!” auf den Bildschirm ausgeben
wollen, müssen wir ein bisschen mehr tun:

    irb(main):002:0> puts "Hallo, Welt!" 
    Hallo, Welt!
    => nil

`puts` ist das grundlegende Kommando, um etwas in Ruby auf den
Bildschirm auszugeben. Und was bedeutet `=> nil`? Das ist das Ergebnis
des Ausdrucks. `puts` liefert immer nil zurück (Rubys Wert für “rein gar
nichts”).

## Dein kostenloser Taschenrechner

Wir haben schon genügend Informationen, um IRB als einfachen
Taschenrechner zu benutzen:

    irb(main):003:0> 3+2
    => 5

Drei plus Zwei. Das war nicht schwer. Was ist mit Drei *mal* Zwei? Man
könnte das nun einfach eintippen, kurz genug ist es jedenfalls. Du
kannst aber auch zurückgehen und die letzte Eingabe verändern. Drücke
die **Pfeil-nach-oben**-Taste auf der Tastatur und sieh nach, ob die
Zeile mit `3+2` wieder erscheint. Falls ja, kannst Du mit der
Pfeil-nach-links-Taste den Cursor direkt hinter das `+`-Zeichen setzen,
dort einfach Backspace drücken und es so zu einem `*`-Zeichen (dem
Multiplikations-Operator) umändern.

    irb(main):004:0> 3*2
    => 6

Probieren wir als nächstes Drei zum Quadrat aus:

    irb(main):005:0> 3**2
    => 9

In Ruby sagt man `**`, wenn man “hoch” sagen will. Aber was, wenn man
den umgekehrten Weg gehen und die Quadratwurzel von etwas finden will?

    irb(main):006:0> Math.sqrt(9)
    => 3.0

Moment, wie war das? So wird die Quadratwurzel von Neun berechnet? Damit
liegst Du vollkommen richtig. Aber sehen wir uns das genauer an!
Zuallererst: Was genau ist `Math`?

## Module gruppieren Code nach Themen

`Math` ist ein in Ruby eingebautes Mathematik-Modul. Module spielen zwei
wichtige Rollen in Ruby. Dieses Beispiel zeigt die erste: Die
Gruppierung ähnlicher Methoden unter einem bestimmten Namen. `Math`
enthält so zum Beispiel auch Methoden wie `sin()` und `tan()`.

Als nächstes kam in obigem Kommando ein Punkt. Was macht der Punkt?
Damit identifiziert man den Empfänger einer Mitteilung. Und die
Mitteilung lautet in diesem Fall `sqrt(9)`. Das bedeutet: “Rufe die
Methode `sqrt` (kurz für “square root”, d.h. “Wurzel”) auf und übergib
Ihr den Parameter `9`.”

Das Ergebnis dieses Methodenaufrufs ist der Wert `3.0`. Es fällt auf,
dass der Wert nicht einfach `3` ist. Das ist deshalb der Fall, weil die
Wurzel einer Zahl in den meisten Fällen keine ganze Zahl sein wird, also
liefert die Methode immer eine Fließkomma-Zahl.

Was, wenn wir nun das Ergebnis eine dieser Rechnungen behalten wollen?
Dann weisen wir das Ergebnis einfach einer Variablen zu.

    irb(main):007:0> a = 3 ** 2
    => 9
    irb(main):008:0> b = 4 ** 2
    => 16
    irb(main):009:0> Math.sqrt(a+b)
    => 5.0

So nützlich das auch für einen Taschenrechner ist: Wir entfernen uns von
der traditionellen `"Hallo, Welt!"`-Botschaft, auf die Anleitungen für
Anfänger für gewöhnlich den Fokus setzen … [Also kehren wir flugs wieder
dahin zurück.](2/)

