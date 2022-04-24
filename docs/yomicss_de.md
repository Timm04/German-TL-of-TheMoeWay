# Yomichan Benutzerdefinierte CSS

!!! Information "Das ist ein optionales Tutorial"
    Wenn du Yomichan einrichten willst, kannst du das [hier](/yomichan) finden.
!!! Information "Yomichan Update"
    Yomichan wurde aktualisiert und hat seitdem alte Klassen geändert und neue hinzugefügt. Dieser Leitfaden wird nicht mehr funktionieren. Bitte komme später wieder, wenn diese Nachricht entfernt worden ist.

Das Aussehen des Yomichan-Pop-up-Fensters kann mit benutzerdefinierten [CSS](https://en.wikipedia.org/wiki/CSS) verändert werden.

In den Einstellungen: Auf der v2-Einstellungsseite kannst du darauf zugreifen, indem du auf "Erscheinungsbild" und dann auf "Benutzerdefiniertes CSS konfigurieren" und "Popup-CSS" gehst.

Es gibt viele, viele fortgeschrittene Änderungen, die du mit benutzerdefiniertem CSS machen kannst, aber ich werde nur auf ein paar Beispiele eingehen.

## Das gescannte Wort

Das gescannte Wort kann mit folgendem Code verändert werden.
```css
/* Für Kanji */
.kanji-link {

}

/* Für Kana & alphanumerische Zeichen */
.source-text {

}

/* For Furigana */
ruby {

}
```

!!! Information "Furigana Farben"
	Wenn für den Selektor "Ruby" keine Farbe angegeben ist, wird sie von der Klasse "source-text" überschrieben.

### Beispiel: Änderung von Schriftgröße und Farbe des gescannten Worts

Mit dem folgenden CSS:
```css
.kanji-link {
	font-size: 25px;
	color: #00FBFF;

}

.source-text {
	font-size: 25px;
	color: #FA73FF;

}
```
Resultat:
![Yomichan CSS Demo](img/yomicss1.jpg)

Dies demonstriert die unterschiedlichen Schriftfarben für Kanji und Kana. Du kannst sie nach Herzenslust bearbeiten.

## Definition

Die Definition kann mit der nachstehenden Klasse geändert werden:

```css
.glossary {

}
```

###  Beispiel: Änderung der Schriftart von Definitionen

!!! Information "Benutzerdefinierte Schriftarten"
	Um eine bestimmte Schriftart in Yomichan zu verwenden, muss die Schriftart auf Ihrem System installiert sein. Du kannst installierte Schriftarten finden in:
	Windows: `C:\Windows\Fonts`
	Linux: `/usr/share/fonts` oder `/usr/share/local/fonts` oder `$HOME/.fonts`
	macOS: `/System/Library/Fonts` oder `$HOME/Library/Fonts`
	Neu installierte Schriftarten erfordern möglicherweise einen Neustart des Systems, damit sie von Yomichan erkannt werden.

Mit dem folgenden CSS:
```css
.glossary {
	font-family: MS Mincho; /* Japanische-Serif Schriftart von Windows */

}
```
Resultat:
![Yomichan CSS Demo](img/yomicss2.jpg)

Dies demonstriert die Änderung der Schriftart für Definitionen in MS Mincho. Linux-Benutzer können alternativ "Noto Serif CJK JP" verwenden.

!!! Frage "Schriftart installiert, aber nicht erkannt?"
    Versuch `""` zu verwenden bspw. `font-family: "07やさしさゴシック";`

### Beispiel: DJTs Quiz-Schriftart verwenden (lustig)

Mit dem folgenden CSS:
```css
/* Du musst 衡山毛筆フォント行書 installiert haben, damit das funktioniert. */
.glossary {
	   font-family: "衡山毛筆フォント行書";

}
```
Resultat:
![Yomichan CSS Demo](img/yomicss3.jpg)

Du kannst dies nach Herzenslust bearbeiten.

## Tags

Tags kann mit den nachstehenden Klassen geändert werden:

```css
.tag {

}

.tag-label {

}

.tag-label-content {

}
```




### Beispiel: Alle Tags entferenn

Mit dem folgenden CSS:
```css
.tag {
    display: none;

}
```

Resultat:
![Yomichan CSS Demo](img/yomicss4.jpg)

Du kannst dies nach Herzenslust bearbeiten.

### Beispiel: Farben der Tags ändern

Du kannst die Farbe der Tags ändern, indem du die folgenden Variablen änderst:

```css
:root { /* Light Theme */
# :root[data-theme="dark"]  { /* Uncomment for dark theme */

    --tag-text-color:
    --tag-border-color:
    --tag-default-background-color:
    --tag-name-background-color:
    --tag-expression-background-color:
    --tag-popular-background-color:
    --tag-frequent-background-color:
    --tag-archaism-background-color:
    --tag-dictionary-background-color:
    --tag-frequency-background-color:
    --tag-part-of-speech-background-color:
    --tag-search-background-color:
    --tag-pitch-accent-dictionary-background-color:

}
```

### Beispiel: Kompletes Nord Style

Das komplete Nord-Style:

```css
body {
    background: #2E3440;

}

.kanji-link {
    color: #ebffff;

}

.source-text {
    color: #ebffff;

}


.glossary {
    color: #ebffff;

}


:root {
    --background-color: #2E3440;
    --tag-text-color: white;
    --tag-border-color: transparent
    --tag-default-background-color: #88C0D0;
    --tag-name-background-color: #88C0D0;
    --tag-expression-background-color: #88C0D0;
    --tag-popular-background-color: #88C0D0;
    --tag-frequent-background-color: #88C0D0;
    --tag-archaism-background-color: #88C0D0;
    --tag-dictionary-background-color: #8FBCBB;
    --tag-frequency-background-color: #81A1C1;
    --tag-part-of-speech-background-color: #88C0D0;
    --tag-search-background-color: #88C0D0;
    --tag-pitch-accent-dictionary-background-color: #5E81AC;
    --accent-color: #8FBCBB;
    --text-color: #ebffff;
    --pitch-accent-annotation-color: #ebffff;
    --input-background-color: #3B4252;
    --reason-text-color: #5E81AC;
    --notification-text-color: #ebffff;
    --notification-background-color: #3B4252;
    --progress-bar-track-color: #D8DEE9;
    --light-border-color: #E5E9F0;
    --sidebar-background-color: #2E3440;
    --sidebar-button-background-color: transparent;
    --sidebar-button-background-color-hover: #81A1C1;
    --sidebar-button-background-color-active: #6d88a3;
    --sidebar-button-danger-background-color: transparent;
    --sidebar-button-danger-background-color-hover: #BF616A;
    --sidebar-button-danger-background-color-active: #8a373f;
    --sidebar-button-icon-color: #ebffff;
    --sidebar-button-disabled-icon-color: #808c8c;
    --sidebar-button-danger-icon-color: #ebffff;

}

::-webkit-scrollbar {
    display: none;

}
```

Resultat:
![Yomichan CSS Demo](img/yomicssnord.jpg)

## Hintergrund

Der Hintergrund kann mit dem unten stehenden Selektor bearbeitet werden:

```css
body {

}
```

### Beispiel: Hintergrundfarbe ändern

Mit dem folgenden CSS:
```css
body {
    background: #FFA8F8;

}
```

Resultat:
![Yomichan CSS Demo](img/yomicss5.jpg)

Du kannst dies nach Herzenslust bearbeiten.

### UBild als Hintergrund verwenden

*Chromium-Anweisungen für jetzt*

Um dies zu tun, musst du Yomichan Dev-Build von GitHub anstelle des Chrome Web Stores installieren, weil wir die Dateien der Erweiterung manipulieren werden, die markiert werden, wenn du sie aus dem Chrome Web Store bekommst.
Du kannst es unten finden.
[[GitHub]](https://github.com/FooSoft/yomichan/releases)

!!! Warnuing "Yomichan Daten"
    Alle Yomichan-Daten, einschließlich der Wörterbücher, werden zurückgesetzt. Möglicherweise kannst du die Dateien einfach von deiner aktuellen Yomichan-Installation kopieren.

Downloade **yomichan-chrome-dev.zip** und entpackt es irgendwo auf deinem Computer.
Geh zu **chrome://extensions** und aktiviere **Entwicklermodus** oben rechts.
Clicke auf "**Packete laden**" wähle den Ordner aus, worin du die .zip hinextrahiert hast.

Gehe nun zu deinem Yomichan-Pfad, der derselbe Ort sein sollte, an dem du es entpackt hast, und lege dein gewünschtes Bild in den Ordner "fg".

Jetzt kannst du das folgende CSS unten ausführen.

```css
body {
   background-image: url("name_of_file.png") !important;

}
```

Resultat:
![Yomichan CSS Demo](img/yomicss10.jpg)

## Sonstiges

Einige verschiedene Modifikationen, die ich cool finde.

### Entfernen des Kennzeichens für den aktuellen Eintrag

Seit der Aktualisierung von Yomichan wurde ein blauer Indikator hinzugefügt. Das war für viele langjährige Yomichan-Nutzer ein Ärgernis.

Mit dem folgenden CSS:
```css
.term-expression-current-indicator {
	display: none;

}
```

Resultat:
![Yomichan CSS Demo](img/yomicss6.jpg)

### Fortschrittsbalken entfernen

Mit dem folgenden CSS:
```css
.top-progress-bar-container {
	display: none;

}
```

### Farben für Indikatoren und Fortschrittsbalken

Wenn dir die aktuelle Farbe des Indikators und des Balkens nicht gefällt, kannst du sie auch ändern, anstatt sie zu entfernen.
Du kannst den Pseudoklassen-Selektor und die benutzerdefinierte Eigenschaft unten hinzufügen:
```css
:root {
	--accent-color: #FF00DD; /* Ändert die Farbe zu Pink */
	--progress-bar-track-color: #00F7FF; /* Ändert die Fortschrittsbalkenfarbe zu blau */

}
```
Wenn du ein dunkles Thema verwendest, musst du dies stattdessen haben:
```css
:root[data-theme="dark"]  {
```

Resultat:
![Yomichan CSS Demo](img/yomicss7.jpg)

### Entfernen von Edikt-Markierungen

In Edict-Wörterbüchern wie JMdict und KireiCake gibt es Markierungen, die wie ○ aussehen, was für manche störend sein kann.
Du kannst sie entfernen, indem du das untenstehende CSS verwendest:

```css
.term-glossary-list {
  padding: 0;
  list-style-type: none;

}
```

Resultat:
![Yomichan CSS Demo](img/yomicss8.jpg)


### Hell-/Dunkelmodus variierendes CSS

Ein Beispiel für unterschiedliche CSS für helle und dunkle Modi findest du unten.

```css
:root[data-theme="dark"] {
    --glossary-color: #a89984;
    --ruby-color: #b8bb26;
    --kanji-text-color: #fabd2f;
    --background-color: #282828;
    --accent-color: #689d6a;
    --progress-bar-track-color: #458588;

}

:root {
    --glossary-color: #504945;
    --ruby-color: #98971a;
    --kanji-text-color: #d79921;
    --background-color: #fbf1c7;
    --accent-color: #689d6a;
    --progress-bar-track-color: #458588;

}

.kanji-link {
    font-family: UD Digi Kyokasho N-R;

}

ruby {
    color: var(--ruby-color);

}

.source-text {
    font-family: UD Digi Kyokasho N-R;

}

.term-glossary {
    color: var(--glossary-color);

}
```
