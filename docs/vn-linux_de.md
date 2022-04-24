# Visual novels auf Linux

Visual novels sind nur Microsoft Windows Programme, daher wirst du Wine benutzen müssen. Das funktioniert besonders gut auf Linux.

Folge den Schritten um VNS auf Linux spielen zu können

## Installiere Wine & Dependencies

=== "Arch"

	Du musst [multilib] und [community] aktivieren, bevor du den Command ausführst. Entferne die Kommentar `[multilib]` und `[community]` Section in `/etc/pacman.conf`.

	```bash
	sudo pacman -S wine-staging winetricks lutris giflib lib32-giflib libpng lib32-libpng libldap lib32-libldap gnutls lib32-gnutls mpg123 lib32-mpg123 openal lib32-openal v4l-utils lib32-v4l-utils libpulse lib32-libpulse alsa-plugins lib32-alsa-plugins alsa-lib lib32-alsa-lib libjpeg-turbo lib32-libjpeg-turbo libxcomposite lib32-libxcomposite libxinerama lib32-libxinerama ncurses lib32-ncurses opencl-icd-loader lib32-opencl-icd-loader libxslt lib32-libxslt libva lib32-libva gtk3 lib32-gtk3 gst-plugins-base-libs lib32-gst-plugins-base-libs gst-plugins-good lib32-gst-plugins-good gst-plugins-bad gst-plugins-bad-libs gst-plugins-ugly libgudev vulkan-icd-loader lib32-vulkan-icd-loader cups samba dosbox
	```

	*Das mag nach einer Menge "Ballast" aussehen, aber vor allem für ältere Spiele wirst du alle diese Funktionen benötigen.

	Jetzt werden wir *CDemu* brauchen, das dazu da ist, um einigen VN vorzugaukeln, dass die Original-Disc für die VN eingelegt ist, damit man das Spiel spielen kann.
	```bash
	sudo pacman -S cdemu-client cdemu-daemon
	```

	Um CDemu verwenden zu können, muss das VHBA-Modul installiert werden.
	```bash
	sudo pacman -S vhba-module
	```

	!!! Information "Benutzerdefinierte und LTS-Kernel"
    Wenn du einen Custom- oder LTS-Kernel verwendest, installiere `vhba-module-dkms`. Ansonsten installiere `vhba-module`.

	Der CDemu-Dienst wird mit dem Kernelmodul geladen.

	Wenn die Treiber für CD/DVD-Laufwerke nicht automatisch geladen werden, kannst du sie manuell laden.
	```bash
	sudo modprobe -a sg sr_mod vhba
	```

=== "Ubuntu"

	Zunächst musst du die 32-Bit-Architektur aktivieren.
	```bash
	sudo dpkg --add-architecture i386
	```
	Downloade den WineHQ repository key:
	```bash
	wget -nc https://dl.winehq.org/wine-builds/winehq.key
	```
	Jetzt füge den WineHQ repository key hinzu:
	```bash
	sudo apt-key add winehq.key
	```
	Füge das repository hinzu:

	```bash
	sudo add-apt-repository 'deb https://dl.winehq.org/wine-builds/ubuntu/ focal main' -y
	```
	!!! Information "Andere Ubuntu-Versionen"
    Wenn du eine andere Version von Ubuntu verwendest, musst du dies stattdessen tun. Ersetzte `groovy` mit dem Codenamen der Ubuntu-Version, die du benutzt. Dieser ist für 20.10:
		```bash
		sudo add-apt-repository 'deb https://dl.winehq.org/wine-builds/ubuntu/ groovy main' -y
		```
	Füge PPA's für Lutris hinzu:
	```bash
	sudo add-apt-repository ppa:lutris-team/lutris -y
	```

	Aktualisiere Packete:
	```bash
	sudo apt update
	```
	Jetzt installiere die stabile Version von Wine:
	```bash
	sudo apt-get install --install-recommends winehq-stable -y
	```
	Installiere Lutris, CDEmu und andere Bibliotheken:
	```bash
	sudo apt-get install lutris libgnutls30:i386 libldap-2.4-2:i386 libgpg-error0:i386 libxml2:i386 libasound2-plugins:i386 libsdl2-2.0-0:i386 libfreetype6:i386 libdbus-1-3:i386 libsqlite3-0:i386 libgstreamer-plugins-base1.0-0:i386 libgstreamer-plugins-good1.0-0:i386 libgstreamer-plugins-bad1.0-0:i386 libgudev-1.0-0:i386 ocl-icd-dev:i386 -y
	```
	Jetzt müssen wir `winetricks` manuel installieren weil die Version im Repository bereits veraltet ist und Fehler verursacht.
	Erstens, wget die binary:
	```bash
	wget https://raw.githubusercontent.com/Winetricks/winetricks/master/src/winetricks
	```
	Benutze `chmod` um es in eine ausführbare Datei zu verwandeln:
	```bash
	chmod +x winetricks
	```
	Kopiere es in dein `/usr/bin` damit es in einer Befehlszeile verwendet werden kann.
	```bash
	sudo cp winetricks /usr/bin
	```

	Optional kannst du für einige VNs auch *CDemu* verwenden, das VNs vorgaukelt, dass die Original-Disc eingelegt ist, damit du das Spiel spielen kannst.

	Füge PPA's für CDEmu hinzu:
	```bash
	sudo add-apt-repository ppa:cdemu/ppa -y
	```
	Aktualisiere package repositories:
	```bash
	sudo apt update
	```
	Jetzt müssen wir das VHBA-Modul installieren, falls du es noch nicht hast.
	```bash
	sudo apt-get install vhba-dkms -y
	```
	Der CDemu-Dienst wird mit dem Kernelmodul geladen.

=== "Debian"

	✅ Getestet mit Debian 11 Bullseye

	Zunächst muss die 32-Bit-Architektur aktiviert werden.
	```bash
	sudo dpkg --add-architecture i386
	```
	Installiere den WineHQ repository key:
	```bash
	wget -nc https://dl.winehq.org/wine-builds/winehq.key
	```
	Jetzt füge den WineHQ repository key hinzu:
	```bash
	sudo apt-key add winehq.key
	```
	Füge den WineHQ repository zu dein `/etc/apt/sources.list`hinzu:

	`deb https://dl.winehq.org/wine-builds/debian/ bullseye main`

	Du brauchst das **Deb Multimedia** Repository für CDEmu, das benutzt wird, um VNs auszutricksen, dass die Disc eingelegt ist, wenn kein Crack verfügbar ist.
	Verwende `wget`, um den GPG-Schlüsselring zu erhalten:
	```bash
	wget https://www.deb-multimedia.org/pool/main/d/deb-multimedia-keyring/deb-multimedia-keyring_2016.8.1_all.deb
	```
	Installiere deb file wie üblich:
	```bash
	sudo dpkg -i deb-multimedia-keyring_2016.8.1_all.deb
	```
	Füge das repository zu `/etc/apt/sources.list` hinzu:

	`deb http://www.deb-multimedia.org bullseye main`

    Du musst auch `contrib` und `non-free` an deine Haupt-Repository-Zeile in `/etc/apt/sources.list` anhängen, falls du das nicht schon getan hast.
	Beispiel:
	`deb http://deb.debian.org/debian bullseye main contrib non-free`

	Schließlich aktualisiere package repositories:
	```bash
	sudo apt update
	```

	Nun installiert man alle benötigten Pakete, einschließlich Wine, Lutris, CDEmu und andere Wine-Dependencies.
	```bash
	sudo apt install --install-recommends winehq-stable lutris dbus-x11 libmirage-plugins gcdemu cdemu-client cdemu-daemon libgnutls30:i386 libldap-2.4-2:i386 libgpg-error0:i386 libxml2:i386 libasound2-plugins:i386 libsdl2-2.0-0:i386 libfreetype6:i386 libdbus-1-3:i386 libsqlite3-0:i386 libgstreamer-plugins-base1.0-0:i386 gstreamer1.0-plugins-good:i386 libgstreamer-plugins-bad1.0-0:i386 libgudev-1.0-0:i386 ocl-icd-dev:i386 -y
	```

	Nun müssen wir `winetricks` manuell installieren, da die Version aus dem Repository bereits veraltet ist und Fehler verursacht.
	Zuerst, wget die binary:
	```bash
	wget https://raw.githubusercontent.com/Winetricks/winetricks/master/src/winetricks
	```
	Benutze `chmod` um es in eine ausführbare Datei zu verwandeln:
	```bash
	chmod +x winetricks
	```
	Kopiere es nun in dein `/usr/bin`, damit es in einer Befehlszeile verwendet werden kann.
	```bash
	sudo cp winetricks /usr/bin
	```
	Jetzt müssen wir das VHBA-Modul installieren, falls du es noch nicht hast.
	```bash
	sudo apt install vhba-dkms
	```
	Der CDemu-Dienst wird mit dem Kernelmodul geladen

=== "Fedora"

	Füge Wine repository hinzu.

	Fedora 33:
	```bash
	sudo dnf config-manager --add-repo https://dl.winehq.org/wine-builds/fedora/33/winehq.repo
	```
	Fedora 32:
	```bash
	sudo dnf config-manager --add-repo https://dl.winehq.org/wine-builds/fedora/32/winehq.repo
	```

	Installieren nun alle build dependencies.

	```bash
	sudo dnf install alsa-plugins-pulseaudio.i686 glibc-devel.i686 glibc-devel libgcc.i686 libX11-devel.i686 freetype-devel.i686 libXcursor-devel.i686 libXi-devel.i686 libXext-devel.i686 libXxf86vm-devel.i686 libXrandr-devel.i686 libXinerama-devel.i686 mesa-libGLU-devel.i686 mesa-libOSMesa-devel.i686 libXrender-devel.i686 libpcap-devel.i686 ncurses-devel.i686 libzip-devel.i686 lcms2-devel.i686 zlib-devel.i686 libv4l-devel.i686 libgphoto2-devel.i686 cups-devel.i686 libxml2-devel.i686 openldap-devel.i686 libxslt-devel.i686 gnutls-devel.i686 libpng-devel.i686 flac-libs.i686 json-c.i686 libICE.i686 libSM.i686 libXtst.i686 libasyncns.i686 liberation-narrow-fonts.noarch libieee1284.i686 libogg.i686 libsndfile.i686 libuuid.i686 libva.i686 libvorbis.i686 libwayland-client.i686 libwayland-server.i686 llvm-libs.i686 mesa-dri-drivers.i686 mesa-filesystem.i686 mesa-libEGL.i686 mesa-libgbm.i686 nss-mdns.i686 ocl-icd.i686 pulseaudio-libs.i686 sane-backends-libs.i686 tcp_wrappers-libs.i686 unixODBC.i686 samba-common-tools.x86_64 samba-libs.x86_64 samba-winbind.x86_64 samba-winbind-clients.x86_64 samba-winbind-modules.x86_64 mesa-libGL-devel.i686 fontconfig-devel.i686 libXcomposite-devel.i686 libtiff-devel.i686 openal-soft-devel.i686 mesa-libOpenCL-devel.i686 opencl-utils-devel.i686 alsa-lib-devel.i686 gsm-devel.i686 libjpeg-turbo-devel.i686 pulseaudio-libs-devel.i686 pulseaudio-libs-devel gtk3-devel.i686 libattr-devel.i686 libva-devel.i686 libexif-devel.i686 libexif.i686 glib2-devel.i686 mpg123-devel.i686 mpg123-devel.x86_64 libcom_err-devel.i686 libcom_err-devel.x86_64 libFAudio-devel.i686 libFAudio-devel.x86_64
	```

	```bash
	sudo dnf groupinstall "C Development Tools and Libraries"
	sudo dnf groupinstall "Development Tools"
	```
	Du brauchst auch paar Packete von [rpmfusion](https://rpmfusion.org)

	```bash
	sudo dnf install gstreamer-plugins-base-devel gstreamer-devel.i686 gstreamer.i686 gstreamer-plugins-base.i686 gstreamer-devel gstreamer1.i686 gstreamer1-devel gstreamer1-plugins-base-devel.i686 gstreamer-plugins-base.x86_64 gstreamer.x86_64 gstreamer1-devel.i686 gstreamer1-plugins-base-devel gstreamer-plugins-base-devel.i686 gstreamer-ffmpeg.i686 gstreamer1-plugins-bad-free-devel.i686 gstreamer1-plugins-bad-free-extras.i686 gstreamer1-plugins-good-extras.i686 gstreamer1-libav.i686 gstreamer1-plugins-bad-freeworld.i686
	```

	Installiere Wine:

	```bash
	sudo dnf install wine
	```

	Dann Winetricks:

	Zuerst, wget die binary:
	```bash
	wget https://raw.githubusercontent.com/Winetricks/winetricks/master/src/winetricks
	```
	Benutze `chmod` um es in eine ausführbare Datei zu verwandeln:
	```bash
	chmod +x winetricks
	```
	Kopiere es nun in dein `/usr/bin`, damit es in einer Befehlszeile verwendet werden kann.
	```bash
	sudo cp winetricks /usr/bin
	```

	Und nun CDEmu, aktiviere zuerst das rok/cdemu COPR repository:

	```bash
	sudo dnf copr enable rok/cdemu
	```
	Installiere CDEmu daemon und Clients:

	```bash
	sudo dnf install cdemu-daemon cdemu-client gcdemu
	```

	Lade das VHBA Kernel Modul:
	```bash
	sudo akmods
	sudo systemctl restart systemd-modules-load.service
	```

=== "openSUSE"

	✅ Getestet mit openSUSE Tumbleweed 20210408

	```bash
	sudo zypper install wine winetricks lutris cdemu-client cdemu-daemon gstreamer-plugins-good gstreamer-plugins-good-32bit gstreamer-plugins-base gstreamer-plugins-base-32bit gstreamer-plugins-libav gstreamer-plugins-libav-32bit gstreamer-plugins-bad gstreamer-plugins-bad-32bit gstreamer-plugins-ugly gstreamer-plugins-ugly-32bit libgudev-1_0-0 libgudev-1_0-0-32bit libSDL2-2_0-0 libjpeg-turbo
	```
	Lade das VHBA module in dein Kernel.

	```bash
	sudo modprobe -a sg sr_mod vhba
	```

=== "Gentoo"

	Stell sicher, dass dein Kernel mit folgenden Optionen kompiliert wurde:

	* `CONFIG_BLK_DEV_SR`
	* `CONFIG_ISO9660_FS`
	* `CONFIG_UDF_FS`
	* `CONFIG_IA32_EMULATION` (for 32 bit support)

	und dass du eine Multilib-Einrichtung hast.

	Es wird empfohlen, die folgenden globalen Use-Flags in deiner `make.conf` zu haben:

	* `X`
	* `pulseaudio`
	* `jpeg`
	* `png`

	Im gentoo wiki findet man weitere Informationrmationen über die verschiedenen Versionen von wine, die unter gentoo verfügbar sind [hier](https://wiki.gentoo.org/wiki/Wine). `wine-vanilla` sollte fast alles problemlos laufen. Stelle sicher, dass das Flag `abi_x86_32` für wine-vanilla nicht ausgeschaltet ist (es ist standardmäßig aktiviert), weitere Informationrmationen findet man im Wiki [hier](https://wiki.gentoo.org/wiki/Wine#32-bit_vs_64-bit).

	Installiere deps.
	```bash
	sudo emerge -v app-emulation/wine-vanilla virtual/wine games-util/lutris app-cdr/cdemu app-emulation/winetricks
	```

	Lade die vhba Module für cdemu
	```bash
	sudo modprobe vhba
	```

	Um das Modul beim Start automatisch zu laden, editieren deine Datei `/etc/conf.d/modules` und fügen diese Zeile hinzu:

	```toml
	modules="vhba"
	```

	Damit der CDEmu-Daemon automatisch von der Benutzeroberfläche gestartet werden kann, muss dbus aktiviert sein. Dazu muss dbus ausgeführt werden:

	```bash
	sudo rc-update add dbus default
	## or if systemd the default init
	sudo systemctl enable dbus
	```

## Wine konfigurieren und runtimes installieren

### Erstellen des Wineprefix

Zunächst müssen wir ein 32-Bit-Wine-Präfix erstellen, da dies die beste Kompatibilität bietet und 64-Bit für VNs unnötig ist.

```bash
WINEPREFIX=~/.winevn WINEARCH=win32 wineboot
```

### Mit winetricks

Jetzt müssen wir die allgemeinen weiterverteilbaren Dateien wie DirectX, Visual C++ Runtimes und .NET Framework 3.5 und andere Dinge installieren, die für das Funktionieren von Video-Cutscenes erforderlich sind.

!!! Tipp "Optional: Schönere Schrift"
	Du kannst `winetricks fontsmooth=rgb` mache denn ohne sie ist die Schrift einfach furchtbar.

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
Die Installation von DXVK verbessert die Leistung leicht, da es sich um eine Vulkan-Implementierung von D3DX9 handelt.
```bash
WINEPREFIX=~/.winevn winetricks dxvk
```
!!! Warnung "Vulkan Unsupported Systems"
	Wenn dein System Vulkan nicht unterstützt, darfst du DXVK nicht installieren. Wenn du DXVK entfernen möchtest, befolge die Anweisungen [hier](https://github.com/doitsujin/dxvk)

Für einige VN, wie z.B. TYPE-MOON's, können LAVFilter für die Videowiedergabe erforderlich sein.
```bash
WINEPREFIX=~/.winevn winetricks lavfilters
```
!!! Fehler "ffdshow und LAVFilters"
	Einige Spiele können kaputt gehen, wenn du sowohl ffdshow als auch LAVFilters installiert hast! Experimentiere unbedingt!

Führe dann diesen Befehl aus, um DLL-Überschreibungen zu deaktivieren und stattdessen die nativen DLLs zu verwenden:

```bash
WINEPREFIX=~/.winevn winetricks alldlls=default
```
!!! Frage "Hast du Probleme?"
	Du kannst es mit `winetricks alldlls=builtin`zurücksetzen.

### Japanische Schriftarten mit Wine

Du musst jetzt japanische Schriftarten in Wine installieren. Bitte laden Sie das Paket unten herunter.
[[Google Drive]](https://drive.google.com/file/d/1OiBgAmt3vPRu08gPpxFfzrtDgarBGszK/view?usp=drivesdk)
Entpacke die Datei und verschiebe die Schriftdateien in den Ordner `Fonts` in `~/.winevn/drive_c/windows/Fonts`

!!! Frage "Wieso installieren wir nicht `cjkfonts` in winetricks?"
	Weil es für VNs nicht richtig funktioniert.

## Lutris Initial Setup

### Umgebungsvariablen

Sobald das erledigt ist, können wir Lutris einrichten.

Öffne Lutris und klicke auf das Plus-Symbol in der oberen linken Ecke und dann auf Läufer installieren.

![Image](img/vnlinux1.jpg)

Scrolle nach unten, suche "Wein" und klicke auf das Zahnradsymbol daneben.

![Image](img/vnlinux2.jpg)

Gehe nun zu Systemoptionen und setze die Umgebungsvariablen wie unten gezeigt und klicke auf Speichern.

- Key: `LC_ALL`	Value: `ja_JP.UTF-8`
- Key: `TZ`	Value: `Asia/Tokyo`

!!! Tipp "Wine prefix in lutris"
	Vergesse nicht, dies mit einzubeziehen, also stelle sicher, dass das Weinpräfix auch in Lutris einzustellen.
![Image](img/vnlinux3.jpg)

## Visual Novels installieren

Ich werde 古色迷宮輪舞曲～HISTOIRE DE DESTIN～ aus Demonstrationsgründen verwenden. Visual Novels kommen normalerweise in .ISO Formaten vor und falls nicht, an .MDS/.MDF Datein die mit `cdemu` geöffnet werden können.

### Disc images

Navigieren Sie zum Pfad des Disk-Images:
```bash
cd /path/to/visualnovelfolder
```
!!! Information "MDS"
	Lade die .MDS-Datei zuerst mit CDEmu. Der Linux-Befehl `mount` wird sie nicht mounten.
	```bash
	cdemu load 0 /path/to/mds_image.MDS
	```
!!! Information "UDF Volumes"
	Lade das UDF-Volume mit CDEmu. Der Linux-Befehl `mount` mit dem Argument `-t udf` wird nicht empfohlen.
	```bash
	cdemu load 0 /path/to/udf_volume
	```
!!! Frage "Grund für CDEmu Mount?"
	Es sollte normalerweise in `/run/media/user/image_name`sein

Wenn du eine normale ISO9660-ISO-Datei verwendest, erstelle einen Einhängepunkt für sie.
```bash
sudo mkdir -p /media/cdrom0
```
Jetzt können wir unser ISO in unseren Einhängepunkt einhängen.
```bash
sudo mount -o loop file.ISO /media/cdrom0
```
Wenn alles gut gegangen ist, kannst du den Inhalt des Images sehen, indem du `ls /media/cdrom0` oder was auch immer dein Einhängepunkt ist:ISO zu unserem Einhängepunkt machst.

![Image](img/vnlinux4.jpg)

### Den Installer laufen lassen

!!! Information "Localegen"
	Generiere ja_JP.UTF-8 locale zuerst.

Als Nächstes werden wir die Setup-Datei mit `wine` ausführen:

```bash
LC_ALL="ja_JP.UTF-8" TZ="Asia/Tokyo" WINEPREFIX=~/.winevn wine <setup_executable>.exe
```
!!! Information ".msi installer"
	Wenn die ausführbare Datei des Installationsprogramms eine .msi-Datei ist, führe sie mit "msiexec" anstelle von "wine" aus.

Fahre mit der Installation fort. Das Spiel kann in `~/.winevn/drive_c/Program Files` oder wo du willst installier werden.

## Lutris hinzufügen

Jetzt können wir das Spiel zu Lutris hinzufügen, damit wir es schnell starten können.

Zurück in Lutris, klicke auf das Plus-Symbol in der Ecke, füge den Namen der VN hinzu, wähle Wine als Runner und navigiere unter "Game Options" zur ausführbaren Datei des Spiels. Du solltest auch die Präfix-Architektur auf 32-bit ändern.

![Image](img/vnlinux5-sup.jpg)

!!! Tipp "AlphaROMdiE"
    Für AlphaROMdiE muss man AlphaROMdiE als ausführbare Datei zu Lutris hinzufügen, und den Dateinamen .exe der Visual Novel als Argument. Wenn du Wine in einer Kommandozeile verwendest, sollte es ungefähr so aussehen:
	```bash
	LC_ALL="ja_JP.UTF-8" TZ="Asia/Tokyo" WINEPREFIX=~/.winevn  wine AlphaROMdiE.exe <VN_executable.exe>
	```
    Für die 1. Option musst du eine leere Datei `disable_conv` an demselben Ort wie AlphaROMdiE.exe erstellen, für die 3. Option musst du eine leere Datei `other_engine` an demselben Ort wie AlphaROMdiE.exe erstellen. Das kannst du so tuen:
	```bash
	touch disable_conv other_engine
	```

![Image](img/vnlinux5.jpg)

!!! Warnung "Vulkan Unsupported Systems"
	Wenn dein System Vulkan nicht unterstützt, musst du DXVK in den "Runner options" deaktivieren, dann wird auf das normale D3DX9 zurückgegriffen. DXVK ist eine Vulkan-Implementierung von Direct3D, so dass wir es auf, auch wenn Vulkan kann nicht viel von einem Unterschied machen zu halten.

Jetzt kannst du es einfach in Lutris starten!

![Image](img/vnlinux6.jpg)

!!! Fehler "Error: オリジナルディスクを入れてください"
    Lade das .ISO-Image mit `cdemu`. Auch wenn Linux ISO9660-Images ohne CDEmu einbinden kann, scheinen *Wine*-Programme das eingebundene Image nicht zu finden.
	```bash
	cdemu load 0 /path/to/image.ISO
	```

und viola!

![Image](img/vnlinux7.jpg)

Jetzt kehre zu [Visual Novel Guide](https://learnjapanese.moe/vn/#playing-visual-novels-to-learn-japanese) zurück und finde heraus wie man Textractor benutzt, das auch mit Wine funktioniert. Man kann Textractor auch zu Lutris hinzufügen für einen schenllen Zugriff.

## Linux: Fehlerbehebung

### Shift-JIS

Kredit: Broken Dragon Translation

Tu dies, wenn du alles richtig gemacht hast, aber der VN-Text noch immer so aussieht: ????? ?????????? ????????????
Normalerweise musst du das nicht tun.
Dies bedeutet, dass deine VN ja_JP.sjis anstelle von ja_JP.UTF-8 verwenden muss.

Du musst ja_JP.sjis [hier](https://cdn.discordapp.com/attachments/813105334763126814/825472692558889022/ja_JP.sjis.zip) downloaden.

Extrahiere die Datei in ein merkbares Verzeichnis auf deinem Computer und "cd" in dieses Verzeichnis.

```bash
cd /path/to/ja_JP.sjis
```

Kompiliere nun local mit `localedef`:
```bash
localedef -i ja_JP -f SHIFT_JIS ./ja_JP.sjis --no-Warnungs=ascii
```

Mit `sed`, bearbeite dein locale.gen

```bash
sed -i '/ja_JP.UTF-8 UTF-8/a ja_JP.SJIS SHIFT_JIS  ' /etc/locale.gen
```
Jetzt generiere locales:

```bash
locale-gen
```

Du kannst dann die Umgebungsvariable `LC_ALL` in Lutris in `ja_JP.sjis` ändern.

### MPEG-1 video wird nicht abgespeilt

Sehe https://bugs.winehq.org/show_bug.cgi?id=52448 für die Behebung.

### Fuguriya VNs

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
