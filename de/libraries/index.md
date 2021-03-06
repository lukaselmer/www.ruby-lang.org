---
layout: page
title: "Bibliotheken"
lang: de
---

Es gibt unzählige faszinierende und nützliche Bibliotheken für Ruby, von
denen viele im praktischen *gem*-Format zur Verfügung stehen. Andere
Bibliotheken werden als gewöhnlches Archiv angeboten (.zip oder
.tar.gz). Nun musst du nur noch wissen, wie du diese Bibliotheken finden
und installieren kannst.

###  <a name="finding-libraries" />Bibliotheken finden

[**RubyForge**][1] ist eine beliebte Anlaufstelle für Ruby-Bibliotheken.
Ein guter Einstiegspunkt ist die [software map][2], in der die
Bibliotheken nach Themengebiet aufgelistet sind. Solltest du irgendwann
einmal eine eigene Bibliothek veröffentlichen wollen, kannst du dein
[Projekt bei RubyForge registrieren][3], um einen kostenlosen
Subversionzugang, Webspace sowie Mailinglisten zu bekommen.

Das [**Ruby Application Archive**][4] (kurz: RAA) ist ein Verzeichnis
von Ruby-Software aller Art, kategorisiert nach jeweiliger Funktion.
Momentan enthält die Kategorie [Database][5] die meisten Einträge, dicht
gefolgt von [Net.][6] [HTML][7] und [XML][8] sind ebenfalls sehr
beliebt. Es gibt sogar 4 Einträge unter [Physics][9]!

###  <a name="using-rubygems" />RubyGems

Der Windows-Installer enthält RubyGems bereits. Für die meisten anderen
Betriebssysteme muss es nachinstalliert werden. Eine Anleitung dazu
[findest du weiter unten](#installing-rubygems).

#### Gems suchen

Der Befehl **search** sucht nach Gems eines bestimmten Namens. Um einen
Gem zu finden, dessen Name das Wort “html” enthält, gibst du
beispielsweise Folgendes ein:

 `
 $ gem search html --remote

 *** REMOTE GEMS ***

 html-sample (1.0, 1.1)
    A sample Ruby gem, just to illustrate how RubyGems works.
` (*Der Parameter `--remote` bewirkt, dass die offiziellen Rubyforge-Gems
durchsucht werden.*)

#### Einen Gem installieren

Sobald du weißt, welchen Gem du installieren willst, gib Folgendes ein:

 `
 $ gem install html-sample
` Du kannst sogar nur eine spezielle Version der Bibliothek installieren,
indem du den Parameter `--version` übergibst:

 `
 $ gem install html-sample --version 1.0
` #### Alle Gems auflisten

Um eine komplette Liste aller Gems auf Ruyforge zu erhalten, verwende
diesen Befehl:

 `
 $ gem list --remote
` Um lediglich die Gems aufzulisten, die installiert sind, lass den
Parameter weg:

 `
 $ gem list
` Weiterführende Informationen zur Verwendung von RubyGems findest du im
[**offiziellen Handbuch**][10], welches auch Beispiele enthält, wie du
Gems in deinen Ruby-Skripten verwenden kannst.

###  <a name="installing-rubygems" />RubyGems installieren

Um RubyGems zu installieren musst du zunächst die “aktuelle Version von
RubyGems herunterladen”http://rubyforge.org/frs/?group\_id=126. Entpacke
das Archiv und führe das enthaltene Skript `setup.rb` aus. Unter manchen
Betriebssystemen könnte es sein, dass du das mit Root-Privilegien tun
musst.

Unter Linux zum Beispiel folgendermaßen:

 `
$ tar xzvf rubygems-0.9.0.tar.gz
$ cd rubygems-0.9.0
$ su -
# ruby setup.rb
` Wenn du weitere Hilfe benötigst, wirf einen Blick auf das [**Kapitel
über die Installation**][11] im RubyGems-Handbuch.



[1]: http://rubyforge.org/ 
[2]: http://rubyforge.org/softwaremap/trove_list.php 
[3]: http://rubyforge.org/register/ 
[4]: http://raa.ruby-lang.org/ 
[5]: http://raa.ruby-lang.org/cat.rhtml?category_major=Library;category_minor=Database 
[6]: http://raa.ruby-lang.org/cat.rhtml?category_major=Library;category_minor=Net 
[7]: http://raa.ruby-lang.org/cat.rhtml?category_major=Library;category_minor=HTML 
[8]: http://raa.ruby-lang.org/cat.rhtml?category_major=Library;category_minor=XML 
[9]: http://raa.ruby-lang.org/cat.rhtml?category_major=Library;category_minor=Physics 
[10]: http://rubygems.org/read/chapter/1 
[11]: http://rubygems.org/read/chapter/3 
