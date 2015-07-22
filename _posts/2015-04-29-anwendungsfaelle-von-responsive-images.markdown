---
layout: post
title:  "Anwendungsfälle von Responsive Images"
date:   2015-04-29 8:50:10
categories: responsive
---

Echte responsive Images – in purem HTML und ohne Hacks – finden immer häufiger Benutzung auf Webseiten. Die Geschichte des <picture> Elements wurde bereits in "Der lange Weg zu Responsive Images" beschrieben, in diesem Artikel soll es um die konkrete Anwendung von Responsive Images gehen. Doch das neue <picture> Element und das neue src-set Attribut können für eine ganze Reihe verschiedener Anwendungsfälle benutzt werden. Nicht immer ist das neue <picture> Element überhaupt nötig. Im folgenden Artikel werden verschiedene Anwendungsfälle beispielhaft erklärt. Für die Auswahl der korrekten Vorgehensweise, muss zunächst der Anwendungsfall genauer bestimmt werden. Es gibt vier mögliche Anwendungsfälle der neuen Syntax:

*   Veränderung der Bild**größe** anhand von Regeln
*   Optimierung für Displays mit vielen **DPI** (Neudeutsch: Retina)
*   Auswahl verschiedener **MIME-**typen je nach Unterstützung
*   Veränderung des Bild**inhaltes** anhand des Kontexts

Es lassen sich also **Größe, DPI, MIME **und **Inhalte** ändern. MIME, oder auch **Internet Media Type** genannt, und klassifiziert welche Daten der Webserver sendet – ob es beispielsweise Text, ein HTML-Dokument oder ein JPG-Bild ist.  

## Inhalte ändern (oder "Art Direction")

<pre><picture>
     <source
            media="(min-width: 1024px)"
            srcset="baum-gesamt.jpg">
     <img
            src="baum-ausschnitt.jpg" alt="Ein responsiver Baum">
</picture></pre>

Bei diesem Beispiel, würden Browserfenster mit einer Breite von 1024 CSS Pixeln oder größer, das Bild mit dem gesamten Baum abbilden; in kleineren Fenstern würde der Ausschnitt angezeigt werden.

[![Bei verschiedenen Auflösungen werden verschiedene Bilder geladen](http://www.onlinewerk.info/wp-content/uploads/2015/02/art-direction.jpg)](http://www.onlinewerk.info/wp-content/uploads/2015/02/art-direction.jpg) 

Bei verschiedenen Auflösungen werden verschiedene Bilder geladen.

Mit einer Inhaltsänderung lässt sich so auch das Bild durch ein komplett neues Bild bzw. eine veränderte Version des Bildes austauschen. So lassen sich beispielsweise breite Diagramme auf kleinen Monitoren neu anordnen um eine vertikale Darstellung zu zeigen.  

## MIME-Typ ändern

<pre><picture>
    <source
           srcset="baum.svg"
           type="image/svg">
    <img
           src="baum.jpg" alt="Baum der ein JPG oder SVG ist">
</picture></pre>

Durch dieses Beispiel zeigen Browser die SVGs unterstützen die SVG-Version des Bildes an, während alle anderen die JPG-Version anzeigen. So lassen sich eher exotische Formate wie SVG und WEBP mit einer Alternative anbieten, falls diese nicht unterstützt werden. 

[![Unser Baum-Bild aus dem ersten Beispiel ist als SVG zugegebener Maßen etwas seltsam, wird aber von SVG-unterstützenden Browsern angezeigt und fällt ansonsten auf JPG zurück.](http://www.onlinewerk.info/wp-content/uploads/2015/02/svg-fallback.jpg)](http://www.onlinewerk.info/wp-content/uploads/2015/02/svg-fallback.jpg) 

Unser Baum-Bild aus dem ersten Beispiel ist als SVG zugegebener Maßen etwas seltsam, wird aber von SVG-unterstützenden Browsern angezeigt und fällt ansonsten auf JPG zurück.


## DPI ändern

<pre><img
         src="baum.jpg" alt="Achtung! Selbstschärfender Baum!"
         srcset="baum@2x.jpg 2x, baum@3x.jpg 3x">
</pre>

Dieses Beispiel zeigt wie mit einem einzelnen <img> Element drei verschiedene Situationen abgefangen warden können. Je nachdem wie hochauflösend das Display ist (2x oder 3x Pixelanzahl), wird ein unterschiedlich hochauflösendes Bild geladen. 

[![*billiger Photoshop unschärfe Effekt um Retina zu simulieren*](http://www.onlinewerk.info/wp-content/uploads/2015/02/retina.jpg)](http://www.onlinewerk.info/wp-content/uploads/2015/02/retina.jpg)

*billiger Photoshop unschärfe Effekt um Retina zu simulieren*

Letztendlich hat jedoch der Browser auch noch ein Mitspracherecht. Hat das Gerät beispielsweise eine 2.5x Auflösung, wird der Browser entscheiden ob das 2x oder 3x besser geeignet ist (anhand der Internetverbindung und anderen Faktoren).  

## Bildgröße ändern

<pre><img
         src="baum-400.jpg" alt="Ein Baum der mit dem Display wächst"
         sizes="(min-width: 640px) 80vw, 100vw"
         srcset="baum-200.jpg 200w,
                 baum-400.jpg 400w,
                 baum-800.jpg 800w,
                 baum-1200.jpg 1200w">
</pre>

Der wohl häufigste Anwendungsfall von responsive Images ist die Verwendung unterschiedlich großer Bilder, um auf mobilen Endgeräten Bandbreite und Ladezeiten zu sparen. 

[![Anhand der Angaben im srcset Attribut, wird die beste Bildgröße ausgewählt. Zusätzlich kann die Breite je nach Auflösung angepasst werden (Links z.B. 80% des Viewports).](http://www.onlinewerk.info/wp-content/uploads/2015/02/sizes.jpg)](http://www.onlinewerk.info/wp-content/uploads/2015/02/sizes.jpg) 

Anhand der Angaben im srcset Attribut, wird die beste Bildgröße ausgewählt. Zusätzlich kann die Breite je nach Auflösung angepasst werden (Links z.B. 80% des Viewports).

Für Browser-Fenster mit einer Breite von 640 CSS Pixel oder mehr, läd ein Bild mit 80% Breite des sichtbaren Bereichs (80vw - **v**iewport **w**idth). Bei kleineren Fenstern füllt das Bild die gesamte Breite aus (100vw). Der Browser entscheidet sich anhand der Bildbreite und Bildschirmauflösung zusätzlich für eine der möglichen Größen (200px, 400px, 800px oder 1200px).  

## Kombination aller Anwendungsfälle

Alle der Aufgeführten Beispiele lassen sich beliebig kombinieren. Dabei spielt die Reihenfolge der Bedingungen und Bildquellen eine Rolle, denn der Browser nimmt automatisch den ersten Media-Query der zutrifft. Ein Extrembeispiel ist die Kombination aller vier Möglichkeiten: **Ändern der Bildgröße, DPI, Inhalte und MIME-Typen in einem <pre><picture></pre> Element**

<pre><picture>
    <source
           media="(min-width: 1280px)"
           sizes="50vw"
           srcset="baum-gesamt-200.webp 200w,
                   baum-gesamt-400.webp 400w,
                   baum-gesamt-800.webp 800w,
                   baum-gesamt-1200.webp 1200w,
                   baum-gesamt-1600.webp 1600w,
                   baum-gesamt-2000.webp 2000w"
           type="image/webp">
    <source                         
           sizes="(min-width: 640px) 60vw, 100vw"                         
           srcset="baum-ausschnitt-200.webp 200w,
                   baum-ausschnitt-400.webp 400w,
                   baum-ausschnitt-800.webp 800w,
                   baum-ausschnitt-1200.webp 1200w,
                   baum-ausschnitt-1600.webp 1600w,
                   baum-ausschnitt-2000.webp 2000w"
           type="image/webp">
    <source                         
           media="(min-width: 1280px)"
           sizes="50vw"
           srcset="baum-gesamt-200.jpg 200w,
                   baum-gesamt-400.jpg 400w,
                   baum-gesamt-800.jpg 800w,
                   baum-gesamt-1200.jpg 1200w,
                   baum-gesamt-1600.jpg 1800w,
                   baum-gesamt-2000.jpg 2000w">
    <img                         
           src="baum-ausschnitt-400.jpg"alt="Bäume Bäume Bäume Wald"
           sizes="(min-width: 640px) 60vw, 100vw"
           srcset="baum-ausschnitt-200.jpg 200w,
                   baum-ausschnitt-400.jpg 400w,
                   baum-ausschnitt-800.jpg 800w,
                   baum-ausschnitt-1200.jpg 1200w,
                   baum-ausschnitt-1600.jpg 1600w,
                   baum-ausschnitt-2000.jpg 2000w">
</picture></pre>

Browser-Fenster mit einer Breite von 1280 CSS Pixel oder mehr werden den gesamten Baum mit einer Breite von 50% des sichtbaren Bereichs anzeigen (50vw). Browser-Fenster mit einer Breite von 640-1279 CSS Pixeln, stellen einen Ausschnitt des Baums in 60% Breite dar (60vw). Für kleinere Fenster (Smartphones), wird das Bild in 100% Breite dargestellt. In allen Fällen wählt der Browser anhand der Bildbreite und Bildschirmauflösung die optimale Bildgröße aus den verfügbaren Größen aus (200px, 400px, 800px, 1200px, 1600px oder 2000px). Zusätzlich zu all dem, werden die Bilder je nach Browser-Unterstützung im WebP-Format oder als JPG angezeigt. 

[![Durch Kombination aller Methoden, lassen sich sehr komplexe Anwendungsfälle bewältigen.](http://www.onlinewerk.info/wp-content/uploads/2015/02/kombination.jpg)](http://www.onlinewerk.info/wp-content/uploads/2015/02/kombination.jpg) 

Durch Kombination aller Methoden, lassen sich sehr komplexe Anwendungsfälle bewältigen.


## Fazit

Wie man sieht, gibt es jede Menge verschiedene Anwendungsfälle für Responsive Images. Dabei ist für den bekanntesten Anwendungsfall, das Ändern der Bildgröße, nicht einmal ein <picture> Element nötig. Durch Angabe mehrerer MIME-Types lassen sich moderne Bildformate wie SVG und WebP einfach verwenden, ohne ältere Browser auszuschließen. <picture> und srcset bieten viele Möglichkeiten und zunehmende Browserunterstützung. [Bereits 40%](http://caniuse.com/#search=picture "Bereits 40%") der weltweiten Internetbenutzer haben einen Browser der das <picture> Element unterstützt. Bei [srcset sind es 49%](http://caniuse.com/#search=srcset "srcset sind es 49%").   Kostenloses Stockphoto von [gratisography](http://www.gratisography.com/ "gratisography").
