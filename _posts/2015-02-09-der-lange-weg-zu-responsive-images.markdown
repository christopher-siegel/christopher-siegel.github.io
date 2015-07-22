---
layout: post
title:  "Der lange Weg zu Responsive Images"
date:   2015-02-09 19:32:03
categories: responsive
---

Die Spezifikation von HTML5 ist nach Jahren der Entwicklung endlich eine Empfehlung des W3C. Vor allem ein Element hat dabei für viel Wirbel gesorgt: Das Picture-Element. Das neue Element soll die immer wachsende Gerätevielfalt und die damit verbundenen Displaygrößen und Auflösungen bändigen und „responsive images“ endlich ohne Hilfsmittel ermöglichen. Die Geschichte der Responsive Images beginnt mir der des Responsive Webdesigns. 2010 schrieb Ethan Marcotte einen Artikel mit dem gleichnamigen Titel „Responsive Web Design“. Er war der erste der diesen Begriff benutzte und er stellte sich darunter Webseiten vor, welche sich mit relativen Breitenangaben an jeden Monitor und jedes Gerät anpassen würden. Sein Artikel enthielt ein Codebeispiel, welches eine einfache responsive (anpassungsfähige) Webseite zeigte. 

[![Die erste responsive Webseite von Ethan Marcotte](http://www.onlinewerk.info/wp-content/uploads/2015/02/first-611x137.jpg)](http://www.onlinewerk.info/wp-content/uploads/2015/02/first.jpg) 

Die erste responsive Webseite von Ethan Marcotte

Der Durchbruch dieser Idee kam mit der Veröffentlichung der neuen Webseite des Bosten Globe – die erste große Seite bei der „Responsive Design“ umgesetzt wurde. Marcotte und die Filament Group waren die Entwickler der neuen Seite. Sie dokumentierten neben dem großen Erflog auch einige Probleme bei der Entwicklung. Vor allem aber ein Problem: Bilder. Das Entwicklerteam bediente sich CSS-Befehlen um die Bilder mit der Breite des Fensters bzw. Gerätes zu skalieren. Genauso tut es heute noch beinahe jede Seite. Das Problem dabei ist, dass kleine Geräte wie Smartphones riesige Bilder laden müssen. Bei mobilem Internet (und damit verbundenen langen Ladezeiten) macht das Besuchen der Webseite keinen Spaß. Die Entwickler der Bosten Globe Seite bedienten sich eines Hacks mit Cookies und Javascript, um unterschiedlich große Bilder zu laden. Das Problem dabei war, dass moderne Browser, bevor sie überhaupt das HTML interpretieren, anfangen alle Bilder der Seite zu laden. Der Browser lädt diese Bilder also bevor er überhaupt weiß, wohin diese kommen sollen. Das Problem ist, dass das Javascript, das die Bilder austauscht, erst ausgeführt wird, wenn alle Bilder geladen sind. So muss der Benutzer nicht nur ein viel zu großes Bild, sondern zusätzlich auch noch die eigentlich angepasste Version herunterladen – es ist also schlimmer als zuvor. Einen interessanten Artikel mit dem humorvollen Namen „Responsive Images: How they Almost Worked and What We Need“ findet man auf „A List Apart“.

## Die Entstehung von Elementen

Hier ist nicht die Rede von Nukleosynthese (die wirkliche Entstehung von Atomkernen), sondern von einem neuen HTML Element. Um das Problem mit Bildern zu lösen, musste ein solches neues HTML-Element geschaffen werden. Bruce Lawson von Opera schlug als Erster ein neues &lt;picture&gt; Element vor. 

[![Bruce Lawson - Vater des &lt;picture&gt; Elements](http://www.onlinewerk.info/wp-content/uploads/2015/02/6f3ec7315ad0715ae2a5f89a52877218.jpeg)](http://www.onlinewerk.info/wp-content/uploads/2015/02/6f3ec7315ad0715ae2a5f89a52877218.jpeg)

Bruce Lawson - Vater des &lt;picture&gt; Elements


Versuche des Entwicklerteams des Boston Globe, die WHATWG oder das W3C auf das &lt;picture&gt; Element aufmerksam zu machen, scheiterten. Auch der Responsive Images Community Group (RICG) wurde kaum Beachtung geschenkt. Dieser Gruppe hatten sich mittlerweile hunderte Entwickler aus aller Welt angeschlossen, welche zusammen an einer Lösung für das Responsive Image Problem tüftelten. Doch anstatt sich den Vorschlag der Entwickler anzuhören, ignorierte die WHATWG den Antrag und schlug zum Trotz ihre eigene Lösung vor. Verständlicherweise war die gesamte Entwicklergemeinschaft verärgert. Die Aufschreie der Gemeinschaft waren laut genug, um einige neue Vorschläge hervorzubringen. Doch die Browserhersteller konnten sich nicht einigen, welcher der Vorschläge nun der beste sei. Das sprichwörtliche Salz in der Suppe lieferten Simon Pieters (Opera) und Tab Atkins (Google) mit einem kleinen aber feinen Zusatz zur Spezifikation des &lt;picture&gt; Elements:

> „Das &lt;picture&gt; Element soll ein Container für &lt;img&gt; sein!“

Wenn der Browser das HTML interpretiert, findet er zunächst das &lt;picture&gt; Element mit allen Regeln für die Darstellung. Anhand dieser Regeln kann der Browser das geeignetste Bild wählen. Nach dieser Auswahl kann der Browser dann genau das richtige Bild laden – und zwar **nur** das richtige. Gleichzeitig ignorieren alte Browser das &lt;picture&gt; Element und laden lediglich das &lt;img&gt; innerhalb. Jetzt musste das &lt;picture&gt; Element nur noch für die neusten Browser programmiert werden – ein Prozess der einige Zeit in Anspruch nahm.

## Per Crowdfunding zum &lt;picture&gt; Element

Yoav Weiss, Web-Entwickler mit C++ Kenntnissen, wollte nicht warten und startete das &lt;picture&gt; Element in C++ zu programmieren. Schnell merkte er jedoch, dass er alleine und nur in seiner Freizeit daran arbeitend, nicht so recht vorankam. Durch eine Crowdfunding Kampagne wurde tatsächlich genug Geld gesammelt, dass Weiss seine volle Zeit für das &lt;picture&gt; Element aufbringen konnte.

[![Die IndieGoGo Kampagne für Responsive Images.](http://www.onlinewerk.info/wp-content/uploads/2015/02/picture-611x371.jpg)](http://www.onlinewerk.info/wp-content/uploads/2015/02/picture.jpg) 

Die IndieGoGo Kampagne für Responsive Images.


Die Entwicklergemeinschaft wünschte sich so sehr eine Lösung für das „Responsive Image Problem“, dass es durch diesen langen und holprigen Weg heute ein &lt;picture&gt; Element gibt. Chrome 38 (auch für Android) sowie Opera 25 unterstützen bereits jetzt das &lt;picture&gt; Element. Firefox 34+ enthält es zwar auch, jedoch ist es standardmäßig ausgeschaltet und muss zunächst aktiviert werden. Apple und Microsoft haben bereits angekündigt, dass sie über eine baldige Implementierung nachdenken.