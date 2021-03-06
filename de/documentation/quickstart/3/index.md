---
layout: page
title: "Ruby in 20 Minuten"
lang: de
---

Erzeugen wir nun ein Greeter-Objekt und benutzen es:

    irb(main):035:0> g = Greeter.new("Patrick")
    => #<0x16cac>
    irb(main):036:0> g.sag_hallo
    Hallo, Patrick!
    => nil
    irb(main):037:0> g.sag_tschuess
    Tschuess, Patrick, bis bald!
    => nil</0x16cac>

Wenn `g` einmal erzeugt wurde, merkt es sich, dass der Name Patrick ist.
Hmm, und wenn wir direkt auf den Namen im Objekt zugreifen wollen?

    irb(main):038:0> g.@name
    SyntaxError: compile error
    (irb):52: syntax error
            from (irb):52

Nö, das geht offensichtlich nicht.

## Objekten unter die Haube geschaut

Instanzvariablen werden im Inneren des Objekts verborgen. Sie sind nicht
so versteckt, dass man sie nicht mehr sieht, wenn man das Objekt
untersucht, und es gibt auch andere Wege, auf sie zuzugreifen, aber Ruby
benutzt den guten objektorientieren Ansatz der Datenkapselung.

Welche Methoden existieren nun für Greeter-Objekte?

    irb(main):039:0> Greeter.instance_methods
    => ["method", "send", "object_id", "singleton_methods",
        "__send__", "equal?", "taint", "frozen?",
        "instance_variable_get", "kind_of?", "to_a",
        "instance_eval", "type", "protected_methods", "extend",
        "eql?", "display", "instance_variable_set", "hash",
        "is_a?", "to_s", "class", "tainted?", "private_methods",
        "untaint", "sag_hallo", "id", "inspect", "==", "===",
        "clone", "public_methods", "respond_to?", "freeze",
        "sag_tschuess", "__id__", "=~", "methods", "nil?", "dup",
        "instance_variables", "instance_of?"]

Hoppla, das sind aber ganz schön viele! Wir haben doch nur zwei Methoden
definiert. Was ist hier also los? Es handelt sich hier um **alle**
Methoden für das Greeter-Objekt – die komplette Liste – inklusive der
Methoden, die von Eltern-Klassen definiert wurden. Wenn wir nur die
Methoden auflisten wollen, die für Greeter definiert wurden, können wir
aber festlegen, dass die Eltern-Klassen nicht berücksichtigt werden
sollen, indem wir `false` als Parameter angeben.

    irb(main):040:0> Greeter.instance_methods(false)
    => ["sag_hallo", "sag_tschuess"]

Aha, das sieht schon besser aus! Nun schauen wir mal, auf welche
Methoden unser Greeter-Objekt reagiert:

    irb(main):041:0> g.respond_to?("name")
    => false
    irb(main):042:0> g.respond_to?("sag_hallo")
    => true
    irb(main):043:0> g.respond_to?("to_s")
    => true

Es kennt also `sag_hallo` und `to_s`. (Die zuletzt genannte Methode ist
jedem Objekt per Voreinstellung bekannt – sie wandelt etwas in einen
String um.) Aber es kennt nicht `name`.

## Für Klassen-Veränderungen ist es nie zu spät

Aber was, wenn wir es ermöglichen wollen, dass man den Namen ansehen
oder ändern kann? Ruby liefert eine einfache Möglichkeit, Zugriff auf
die Variablen eines Objekts zu gewähren.

    irb(main):044:0> class Greeter
    irb(main):045:1>   attr_accessor :name
    irb(main):046:1> end
    => nil

In Ruby kann man eine Klasse jederzeit verändern. Das ändert keine
Objekte, die bereits existieren, aber es beeinflusst sämtliche neuen
Objekte, die wir erzeugen. Erzeugen wir also ein neues Objekt und
spielen ein bisschen mit dessen `@name`-Eigenschaft herum.

    irb(main):047:0> g = Greeter.new("Andreas")
    => #<0x3c9b0>
    irb(main):048:0> g.respond_to?("name")
    => true
    irb(main):049:0> g.respond_to?("name=")
    => true
    irb(main):050:0> g.sag_hallo
    Hallo, Andreas!
    => nil
    irb(main):051:0> g.name="Bettina"
    => "Bettina"
    irb(main):052:0> g
    => #<0x3c9b0>
    irb(main):053:0> g.name
    => "Bettina"
    irb(main):054:0> g.sag_hallo
    Hallo, Bettina!
    => nil</0x3c9b0></0x3c9b0>

Die Benutzung von `attr_accessor` hat zwei neue Methoden für uns
definiert: Mit `name` erhält man den Wert, mit `name=` setzt man ihn.

## Die Begrüßung von allem und jedem – MegaGreeter vernachlässigt niemanden!

Die Greeter-Klasse ist nicht unbedingt interessant, sie kann nur eine
Person gleichzeitig behandeln. Was, wenn wir eine Art MegaGreeter
hätten, der entweder die ganze Welt, eine Person oder eine Liste von
Leuten begrüßen könnte?

Schreiben wir das lieber in eine Textdatei, statt es direkt in den
interaktiven Ruby-Interpreter IRB zu tippen.

Um IRB zu beenden, gib “quit” oder “exit” ein, oder drücke einfach
Strg-D.

    #!/usr/bin/env ruby
    
    class MegaGreeter
      attr_accessor :names
    
      # Erzeuge das Objekt
      def initialize(names = "Welt")
        @names = names
      end
    
      # Sag Hallo zu allen
      def sag_hallo
        if @names.nil?
          puts "..."
        elsif @names.respond_to?("each")
    
          # @names ist eine Liste, durchlaufe sie!
          @names.each do |name|
            puts "Hallo, #{name}!"
          end
        else
          puts "Hallo, #{@names}!"
        end
      end
    
      # Sag Tschuess zu allen
      def sag_tschuess
        if @names.nil?
          puts "..."
        elsif @names.respond_to?("join")
          # Verbinde die Listenelemente mit einem Komma
          puts "Tschuess, #{@names.join(", ")}, bis bald!"
        else
          puts "Tschuess, #{@names}, bis bald!"
        end
      end
    
    end
    
    
    if __FILE__ == $0
      mg = MegaGreeter.new
      mg.sag_hallo
      mg.sag_tschuess
    
      # Aendere den Namen in "Maximilian"
      mg.names = "Maximilian"
      mg.sag_hallo
      mg.sag_tschuess
    
      # Aendere den Namen in ein Array von Namen
      mg.names = ["Albert", "Bianca", "Carl-Heinz",
        "David", "Engelbert"]
      mg.sag_hallo
      mg.sag_tschuess
    
      # Aendere in nil
      mg.names = nil
      mg.sag_hallo
      mg.sag_tschuess
    end

Speichere diese Textdatei als “ri20min.rb” und starte es mit “ruby
ri20min.rb”. Die Ausgabe sollte sein:

    Hallo, Welt!
    Tschuess, Welt, bis bald!
    Hallo, Maximilian!
    Tschuess, Maximilian, bis bald!
    Hallo, Albert!
    Hallo, Bianca!
    Hallo, Carl-Heinz!
    Hallo, David!
    Hallo, Engelbert!
    Tschuess, Albert, Bianca, Carl-Heinz, David, Engelbert, bis bald!
    ...
    ...
{: .code .output-code}

In dieses Beispiel wurden einige neue Dinge gestreut, auf die wir
sogleich [näher eingehen werden](../4/).

