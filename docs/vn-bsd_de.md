# Visual novels auf FreeBSD

Visual novels sind nur Microsoft Windows Programme, daher wirst du Wine benutzen müssen.

Folge den Schritten um VNS auf FreeBSD spielen zu können.

## Japanisch und UTF-8 Support

Standardmäßig ist FreeBSD nicht in der Lage, japanischen Text anzuzeigen.
### Installiere eine Japanische Schriftart

Stelle sicher, dass du eine japanische Schriftart hast, bevor du fortfährst.
Ich empfehle die folgenden Schriftarten:
**ja-font-ipa**
**ja-font-koruri**
**ja-font-sazanami**
**ja-font-kochi**
**ja-font-mplus-ipa**
**ja-font-vlgothic**
**hanazono-fonts-ttf**

Du kannst sie aus dem Binärpaket heraus installieren, indem du das tust:
```bash
sudo pkg install ja-font-ipa ja-font-koruri ja-font-sazanami ja-font-kochi ja-font-mplus-ipa ja-font-vlgothic hanazono-fonts-ttf
```
Du kannst überprüfen, ob dein FreeBSD-System in der Lage ist, japanischen Text anzuzeigen, wenn du den japanischen Text unten sehen kannst:

> てすとテスト試験

### Unicode UTF-8 aktivieren

Außerdem verwendet FreeBSD standardmäßig keinen Unicode. Daher werden japanische Dateinamen nicht korrekt dargestellt. Du musst diese Zeilen zu deiner `/etc/login.conf` hinzufügen

```{linenums="25 1" hl_lines="3 4"}
default:\
			[...]
			:charset=UTF-8:\
			:lang=en_US.UTF-8:
```
Erstelle nun die Datenbank für diese Datei neu:

```bash
sudo cap_mkdb /etc/login.conf
```
Log dich aus mit `pkill -u $USER` und log dich wieder ein

!!! Information "KDE5/SDDM"
    Wenn du KDE benutzt, erstelle eine Datei namens `locale.sh` in `$HOME/.config/plasma-workspace/env/`. Wenn das Verzeichnis nicht existiert, erstelle es einfach. Füge den folgenden Inhalt hinzu:
	```{linenums="1 1" hl_lines="1 2"}
	export LANG=en_US.UTF-8
	export MM_CHARSET=UTF-8
	```

## Grafik-Treiber

Dies ist wichtig, da wir Hardware-Beschleunigung benötigen. Falls du noch keine Grafiktreiber auf deinem FreeBSD-System installiert hast, solltest du das jetzt tun.

[[AMD/ATI Radeon and Intel Graphics]](https://wiki.freebsd.org/Graphics)
[[NVIDIA Graphics]](https://wiki.ghostbsd.org/index.php/Nvidia_driver_installation)

## Wine und dependencies installieren

!!! Tipp "Tipp"
    Wenn du Probleme mit den Binärpaketen hast, versuche, sie aus den Ports zu kompilieren.

Wine unter FreeBSD hat im Vergleich zu seinem Linux-Pendant eine eingeschränkte Funktionalität. Du kannst zum Beispiel nicht WOW64 (64-Bit-Windows, aber 32-Bit-Anwendungen) verwenden. Außerdem muss du das Paket `i386-wine` von Wine verwenden, wenn du ein amd64-System benutzt. Lasse dich nicht von diesem Namen täuschen, wenn du auf einem 32-Bit-System arbeitest, verwende das reguläre `wine`-Paket. Die fehlende WOW64-Unterstützung ist jedoch kein Problem für Visual Novels, da diese ohnehin alle 32-Bit sind.
Installiere zuerst alle benötigten Abhängigkeiten für Wine, um sicherzustellen, dass du nicht in der "Wine-Abhängigkeitshölle" landest:

!!! Warnung "lib32"
	Du musst lib32 haben, damit das funktioniert! Prüfe, ob `/usr/lib32` existiert! Wenn du sie bei der Installation von FreeBSD nicht bekommen hast, hole sie jetzt.
	Holen Sie sich `lib32.txz` für Ihre FreeBSD-Version[hier](https://download.freebsd.org/ftp/releases/amd64/)
	Führe dann `sudo tar -C / -xpf lib32.txz` und anschließend `sudo freebsd-update` aus.

```bash
sudo pkg install p7zip cabextract freetype libosmesa libpcap libjpeg-turbo sane-backends ncurses ocl-icd liberation-fonts-ttf libgphoto2 json-c unixODBC nss_mdns gstreamer1 gstreamer1-plugins-good gstreamer1-plugins-ugly gstreamer-plugins-bad alsa-plugins libx11 libXcursor libXi libXext libXxf86vm libXrandr libXinerama libGLU libXrender libzip lcms2 cups libxml2 libxslt flac libICE libSM libXtst libXcomposite openal-soft gtk3 libva libexif mpg123
```

Installiere nun Wine:

```bash
sudo pkg install i386-wine-devel
```

Wir benötigen nun `winetricks`. Verwende nicht das Programm aus dem FreeBSD-Repository, da es veraltet ist und Kopfschmerzen verursachen wird.
Lade mit `curl` das neueste winetricks-Binary herunter.
```bash
curl https://raw.githubusercontent.com/Winetricks/winetricks/master/src/winetricks --output winetricks
```
Verwende `chmod`, um es ausführbar zu machen.
```bash
chmod +x winetricks
```
Kopiere es nun in Ihr `/usr/local/bin`, damit du es in der Befehlszeile verwenden kannst.
```bash
sudo cp winetricks /usr/local/bin
```

## Konfigurieren von Wine und Installieren von Laufzeiten

### Erstellen des Wineprefixes

Zunächst müssen wir ein 32-Bit-Wine-Präfix erstellen, da dies die beste Kompatibilität bietet und alle Visual Novels ohnehin 32-Bit sind.

```bash
WINEPREFIX=~/.winevn WINEARCH=win32 wineboot
```

Wenn Wine zum ersten Mal gestartet wird, musst du es Gecko und Mono Pakete installieren lassen.

### Mit Winetricks
Jetzt müssen wir die allgemeinen weiterverteilbaren Dateien wie DirectX, Visual C++ Runtimes und .NET Framework 3.5 und andere Dinge installieren, die für das Funktionieren von Video-Cutscenes erforderlich sind.

!!! Tipp "Optional: Font smoothing"
    Du kannst `winetricks fontsmooth=rgb` machen, weil die Schrift sonst einfach schrecklich ist.

!!! Tipp "Optional: GUI Verbesserungen"
	Du kannst den Registrierungs-Editor mit `wine regedit` öffnen und [this .reg file](https://cdn.discordapp.com/attachments/813105334763126814/813105422285799464/wine_breeze_colors.reg) importieren, die GUI sollte dann schön und sauber aussehen.

Zuerst muss man die neuesten Patches für Winetricks besorgen, sonst funktioniert es nicht.
```bash
sudo winetricks --self-update
```
Benutze jetzt Winetricks.

```bash
WINEPREFIX=~/.winevn winetricks ffdshow quartz wmp9 d3dx9 dotnet35 vcrun2003 vcrun2005 vcrun2008 vcrun2010 vcrun2012 vcrun2013 vcrun2015
```
!!! Tipp "ffdshow"
	Kontrolliere bei der Installation von ffdshow unbedingt jeden einzelnen Codec/jedes Format, sonst funktioniert es nicht!

Für einige VN, wie z.B. TYPE-MOON's, können LAVFilter für die Videowiedergabe erforderlich sein.
```bash
WINEPREFIX=~/.winevn winetricks lavfilters
```
!!! Fehler "ffdshow and LAVFilters"
	Einige Spiele können kaputt gehen, wenn du sowohl ffdshow als auch LAVFilters installiert hast! Experimentiere unbedingt!

Führe dann diesen Befehl aus, um DLL-Überschreibungen zu deaktivieren und stattdessen die nativen DLLs zu verwenden:

```bash
WINEPREFIX=~/.winevn winetricks alldlls=default
```

### Japanische Schriftarten mit Wine

Du musst jetzt japanische Schriftarten in Wine installieren. Bitte laden Sie das Paket unten herunter.
[[Google Drive]](https://drive.google.com/file/d/1OiBgAmt3vPRu08gPpxFfzrtDgarBGszK/view?usp=drivesdk)
Entpacke die Datei und verschiebe die Schriftdateien in den Ordner `Fonts` in `~/.winevn/drive_c/windows/Fonts`

!!! Frage "Wieso installieren wir nicht `cjkfonts` in winetricks?"
	Weil es für VNs nicht richtig funktioniert.

## Visual Novels installieren

FreeBSD verfügt nicht über CDEmu, FUSEISO oder etwas Ähnliches, so dass du die Disk-Image-Datei regelmäßig einbinden musst. Glücklicherweise macht FreeBSD keinen Unterschied zwischen virtuellen und physischen Laufwerken.
ABER... FreeBSD unterstützt das einfache Einbinden von Laufwerken nicht, ohne mdconfig zu verwenden und als ROOT auszuführen! Selbst für den Zugriff auf ein gemountetes Disk-Image benötigst du root-Rechte.
Da du Wine nicht als root ausführen kannst, werden wir keine Image-Dateien mounten. Also werden wir 7zip verwenden.

### ISO extrahieren

!!! Information "MDS/MDF"
	Wenn du Alkohol 120% CD/DVD-Images (MDS/MDF) hast, musst du Tools wie `mdf2iso` verwenden. Die übliche Syntax lautet:
	```bash
	mdf2iso <source>.MDF <output>.ISO
	```
    In anderen Fällen kannst du die MDF-Datei umbenennen, um die ISO-Erweiterung zu verwenden, und sie wird einfach funktionieren.

Zuerst erstellen wir unseren Zielordner und legen `cd` dort ab.
```bash
mkdir ~/extracted
cd ~/extracted
```

Extrahiere mit `7z`:

```bash
7z -x /path/to/<disc_image>.ISO
```
### Den Installer laufen lassen

Jetzt können wir endlich das Installationsprogramm mit Wine starten.

```bash
LC_ALL="ja_JP.UTF-8" TZ="Asia/Tokyo" WINEPREFIX=~/.winevn wine <setup_executable>.exe
```
!!! Information ".msi Installer"
	Wenn die ausführbare Datei des Installationsprogramms eine .msi-Datei ist, führe sie mit "msiexec" anstelle von "wine" aus.

Fahre mit der Installation fort. Beachte bitte das Installationsverzeichnis. C:\" ist relativ zu "$WINEPREFIX/drive_c".

### Die Visual Novel starten

Ich habe Kanon Memorial Edition für den Guide verwenden. Ich kann `cd` in `$WINEPREFIX/drive_c/KEY/KANON_ME_ALL` speichern und dann `REALLIVE.EXE` mit Wine ausführen, um die VN jederzeit zu starten.

```bash
cd /path/to/visualnovelfolder
LC_ALL="ja_JP.UTF-8" TZ="Asia/Tokyo" WINEPREFIX=~/.winevn wine <vn_executable.exe>
```

!!! Information "AlphaROMdiE"
    Für AlphaROMdiE muss man es mit Wine und der ausführbaren Datei VN als Argument ausführen. Es sollte wie folgt aussehen:
	```bash
	LC_ALL="ja_JP.UTF-8" TZ="Asia/Tokyo" WINEPREFIX=~/.winevn wine AlphaROMdiE.exe <VN_executable.exe>
	```
    Für die 1. Option musst du eine leere Datei `disable_conv` an demselben Ort wie AlphaROMdiE.exe erstellen, für die 3. Option musst du eine leere Datei `other_engine` an demselben Ort wie AlphaROMdiE.exe erstellen. Das kannst du so tuen:
	```bash
	touch disable_conv other_engine
	```

Und viola! hier ist eine Demonstration von 2 VNs, die gleichzeitig arbeiten:

![Image](img/vnbsd1.jpg)

Und die eigentlichen Spiele funktionieren, siehe:

![Image](img/vnbsd2.jpg)

Jetzt kehre zu [Visual Novel Guide](https://learnjapanese.moe/vn/#playing-visual-novels-to-learn-japanese) zurück und finde heraus wie man Textractor benutzt, das auch mit Wine funktioniert. Bitte beachten Sie, dass es mit der Umgebungsvariablen `LC_ALL=ja_JP.UTF-8` gestartet werden muss.

!!! Tipp "Brauchst du eine Verknüpfung?"
	Du kannst Verknüpfungen zu deinem `${HOME}/Desktop` hinzufügen. Erstelle eine Datei wie "[vn].desktop". Du kannst sie auch in `/usr/local/share/applications` ablegen, damit sie in Anwendungsstartprogrammen erscheint. Hier ist, wie ich Kanon von einer Verknüpfung starte (auch mit dem Symbol!)
	```conf
	#!/usr/bin/env xdg-open
	[Desktop Entry]
	Name=Kanon Memorial Edition
	Name[ja]=Kanon メモリアルエディション 全年齢対象版
	Icon=/home/shoui/.winevn/drive_c/KEY/KANON_ME_ALL/KANON_ME_ALL.ICO
	Exec=LC_ALL=ja_JP.UTF-8 TZ=Asia/Tokyo wine ~/.winevn/drive_c/KEY/KANON_ME_ALL/RealLive.exe
	Terminal=false
	Type=Application
	Categories=Game
	```
	Denke daran, dass Pfade zu Symbolen absolut sein müssen!
??? Tipp "Vorschau"
	![Image](img/vnbsd3.gif)

### Fehlerbehebung: Fuguriya VNs

VNs von Fuguriya wie *Sono Hanabira ni Kuchizuke o* und *Hanahira* werden standardmäßig nicht mit Wine gestartet. Folge den untenstehenden Schritten, um dies zu beheben.

Downloade fjfix [[hier]](https://cdn.discordapp.com/attachments/813105334763126814/832650409167945798/fjfix.tar.gz).
Extrahiere das Archiv.
Jetzt im Terminal, führe das aus:
```bash
LC_ALL=ja_JP.UTF-8 WINEPREFIX=~/.winevn wine fjfix.exe -f /path/to/MGD
```

Jetzt sollte es funktionieren.

<h3>Fandest du das nützlich? Unterstützt mich doch auf Patreon!</h3>

[:fontawesome-brands-patreon: Unterstützt mich auf Patreon](https://www.patreon.com/shoui){: .md-button }
