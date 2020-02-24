# Audio-Visualisation mit LED-Streifen

In diesem Projekt werden wir einen LED-Streifen mit dem Raspberry Pi verbinden um Musik mittels
verschiedener Visualisationen sichtbar zu machen. Als Audioquelle kann man das eigene Handy oder
ein Mikrofon benutzen.
Die hier angegebenen Links sind kein muss, sondern nur zur besseren Orientierung beim besorgen der
benötigten Materialien. Es gibt z.b. viele Arten von Klinke Y-Adaptern. Welchen man davon für dieses
Projekt benutzt, ist völlig egal.
Wer fertig ist und Lust hat noch etwas mehr zu machen, kann zusätzlich noch die Erweiterung machen
um die verschiedenen Effekte und die Helligkeit des LED-Streifen mit einer Fernbedienung zu steuern.

Das wird benötigt:

- Raspberry Pi, Modell: 2/3/3B+ oder 4, empfohlen wird ein Raspberry Pi 3B+
    (https://www.amazon.de/dp/B07BFH96M3/)
- Eine Stromversorgung: Liefert Strom für Raspberry Pi und den LED-Streifen
    (https://www.amazon.de/dp/B06XCMQ212)
- Micro-SD Karte: mindestens 8GB (https://www.amazon.de/dp/B073K14CVB)
- Dupont Kabel: 3 Kabel für Teil 1, 3 Kabel für Teil 2 (https://www.conrad.de/de/p/joy-it-rb-cb1-
    25-jumper-kabel-raspberry-pi-13x-drahtbruecken-buchse-13x-drahtbruecken-buchse-0-30-m-
    bunt-inkl-pins-1192229.html)
- LED-Streifen: darf höchstens 256 LEDs haben und in der Beschreibung muss “WS281...”
    stehen (https://www.amazon.de/dp/B07TJDS1PZ/)
- Für die Audio-Quelle: Entweder Variante **(1)** : man schließt ein Handy oder Laptop an von wo
    die Musik kommt, oder Variante **(2)** : man nutzt ein Mikrofon
       **(1)** Für die erste Variante benötigen wir 3 Dinge:
       1. eine USB-Soundkarte
       (https://www.amazon.de/dp/B01N905VOY)
       2. ein Klinke auf Klinke-Kabel
       (https://www.amazon.de/dp/B00B79JGE4/)
       3. ein Klinke Y-Adapter
       (https://www.amazon.de/dp/B0198L22BQ/)
       Alles muss so angeschlossen werden wie auf Bild
       1. Am Klinke Y-Adapter, welcher im Handy steckt,
       ist jetz noch ein Steckplatz frei: hier kann man
       seine Lautsprecher oder Kopfhörer anschließen.
       **(2)** Für die zweite Variante benötigen wir nur ein
       USB-Mikrofon, z.B. dieses:
       https://www.amazon.de/dp/B071171DBP
- (Für die Erweiterung) Infrarot-Empfänger und Fernbedienung:
    https://www.amazon.de/ dp /B07J2PWNWF/

```
Bild 1
```

# Hardware zusammenbauen

**1.** Zuerst müssen die Kabel verbunden werden, siehe Bild 2.
Das weiße und das rote Kabel aus dem LED-Streifen müssen mit der Stromversorgung verbunden
werden ( 1 ). Einfach mit einem Schraubenzieher die Schraube am Verbindungsstück etwas lockern.
Falls an dem weißen und roten Kabel keine Metalldrähte am Ende zu sehen sind, muss die Isolierung
(das weisse/ rote plastik) des Kabels vorsichtig mit einem Messer ein wenig entfernt werden, so dass
ein paar Milimeter Draht vorne aus dem Kabel rausguckt.
Dann einfach das weisse Kabel in das Loch mit dem Minussymbol (-) stecken, das rote kabel in das
loch mit dem (+)-Symbol und anschließend die Schrauben wieder fest ziehen. Über diese Kabel werden
der LED-Streifen und der Raspberry Pi mit Strom versorgt. Deshalb wird ab hier keine extra
Stromversorgung für den Pi mehr benötigt. (Normalerweise wird der Pi mit einem USBC- oder Micro-
USB-Kabel mit Strom versorgt).
**2.** Als nächstes verbinden wir den LED-Streifen mit dem Raspberry Pi.
Hierfür müssen wir die drei Kabel, welche zu einem Stecker zusammengefasst sind mit Hilfe der
Dupont Kabel verbinden. Einfach 3 Kabel nehmen (Welche Farben man benutzt ist egal, ich hab
einfach zufällig ausgewählt).
Auf der einen Seite stecken wir jeweils ein Metallstäbchen (Pin) in jedes der drei Enden, so dass man
wie in ( 2 ) zu sehen diese dann in den Stecker des LED-Streifens steckt.
**3.** Die anderen Enden werden jetzt mit den GPIO-Steckplätzen (die vielen Metallstäbchen auf dem Pi)
verbunden:
Eines der drei Kabel welches aus dem LED-Streifen kommt ist enweder teilweise, oder komplett Rot
(eines der beiden Äußeren Kabel). Dieses Kabel wird auf Pin 2 (5V) gesteckt (ganz oben rechts in der

```
Bild 2
```

Grafik, in unserem Beispiel, ist das Kabel orange welches das rote Kabel von dem LED-Streifen
verlängert).
Das zweite Kabel ist das mittlere Kabel, in der Regel in der Farbe grün. Dieses Verbinden wir mit Pin
12 (GPIO 18).
Jetzt ist nur noch ein Kabel übrig welches angeschlossen werden muss. In den meisten Fällen ist es
weiss, und wird mit Pin 6 (Ground) verbunden.

# Software einrichten

Wenn alles fertig angeschlossen ist, die SD-Karte bereit ist und im Pi steckt, kann er jetzt eingeschaltet
werden. Sobald der Pi gestartet ist, kann es los gehen. Die Kommandos müssen einfach in einem
Terminal-Fenster nacheinander eingefügt werden:

**1.** Zuerst müssen wir das System updaten und die benötigten Pakete installieren. Hierfür öffnen wir ein
Terminal-Fenster und geben folgenden Zeile ein:
sudo apt-get update && sudo apt-get upgrade -y && sudo apt-get install gcc make build-essential
python-dev git scons swig python-numpy python-scipy python-pyaudio -y
**2.** Dann müssen wir die Boot-Konfiguration ändern
sudo geany /boot/config.txt
Hier müssen folgende Zeilen verändert werden (es ist möglicherweise ein # am Anfang welches
entfernt werden muss):
    dtparam=audio=on
**3.** Jetzt müssen die benötigten dateien heruntergeladen und installiert werden.:
git clone https://github.com/rarebu/led-strip-fun.git
cd rpi_ws281x
scons
cd python
sudo python setup.py install
**4.** Dann müssen wir den Audio-Ausgang deaktivieren
sudo geany /etc/modprobe.d/snd-blacklist.conf
dann diese Zeile ganz am Ende einfügen:
    blacklist snd_bcm
**5.** Dann den Audio-Eingang konfigurieren:
sudo geany /etc/asound.conf
In dieser Datei müssen nun folgende Zeilen eingefügt werden falls sie noch nicht existieren, oder
geändert werden falls sie schon da sind:

```
pcm.!default {
```

```
type hw
card 1
}
ctl.!default {
type hw
card 1
}
```
**6.** Jetzt das Standard-Audio-Gerät konfigurieren
sudo geany /usr/share/alsa/alsa.conf
Ändere folgende Zeilen

```
defaults.ctl.card 0
defaults.pcm.card 0
```
zu diesen hier:

```
defaults.ctl.card 1
defaults.pcm.card 1
```
**7.** Jetzt wird das System neu gestartet. Entweder man klickt sich mit der Maus zum Ausschaltmenü,
oder man gibt einfach sudo reboot in das Terminal ein.
**8.** Im nächsten Schritt testen wir den LED-Streifen. Hierfür müssen wir zuerst folgende Datei
bearbeiten:
sudo geany /home/pi/led-strip-fun/rpi_ws281x/python/examples/strandtest.py
Je nachdem wieviele LEDs euer LED-Streifen hat, muss die Anzahl hier eingetragen werden: (In
diesem Beispiel hat der LED-Streifen 120 LEDs)
    LED_COUNT 120
    LED_PIN 18
Jetzt können wir testen ob wir alles richtig gemacht haben:
sudo python /home/pi/led-strip-fun/rpi_ws281x/python/examples/strandtest.py
Jetzt solltet ihr auf dem LED-Streifen eine Testanimation mit ein paar schönen Farbspielen sehen
können.
Das Programm kann mit den Tasten STRG + C wieder gestoppt werden.
**9.** Jetzt können wir die Visualisation testen. Hierfür müssen wir ebenfalls zuerst das Python-Script
bearbeiten:
sudo geany /home/pi/led-strip-fun/rpi_ws281x/python/examples/config.py
Folgende Zeilen müssen angepasst werden. Bei N_PIXELS müssen wie in Schritt 8, die Anzahl der
LEDs angegeben werden:
    DEVICE = 'pi'
    LED_INVERT = False
    USE_GUI = False
    DISPLAY_FPS = False


## N_PIXELS = 120

Mit diesem Befehl startet man die Visualisation:
sudo python /home/pi/led-strip-fun/rpi_ws281x/python/examples/visualization.py
Jetzt sollte die Visualisation laufen :)

Das Programm kann ebenfalls mit den Tasten STRG + C wieder gestoppt werden.
Es gibt 3 verschiedene Effekte: Spectrum, Scroll und Energy. Um zwischen den Effekten zu wechseln,
muss dieser im Python-Script angegeben werden:
sudo geany /home/pi/led-strip-fun/rpi_ws281x/python/examples/visualization.py
Suche nach dieser Zeile: visualization_effect = visualize_spectrum
Anstelle von “spectrum” kann man “scroll” oder “energy” eintragen.

**10**. Automatisches starten der Visualisation beim Einschalten des Pis
Damit man nicht jedes mal wenn man den Pi einschält alles von Hand starten muss, fügen wir das
Python-Script jetzt zum Autostart hinzu.
Einfach folgendes in ein Terminal eingeben:
sudo crontab -e
Beim ersten mal wird man gefragt welchen Texteditor man verwenden soll, wir entscheiden uns hier
für den ersten.
1 eingeben und Enter drücken.
Jetzt muss man mit den pfeiltasten bis ans ende der Datei gehen und dort dann folgende Zeile einfügen:
@reboot python /home/pi/led-strip-fun/audio-reactive-led-strip/python/visualization.py
speichere und schließe die datei mit: “STRG + O” (dann mit Enter bestätigen) und dann “STRG + X”


# Erweiterung

**1.** Folgendes muss in das Terminal eingegeben werden um zunächst das Infrarot-Modul auf dem
Raspberry Pi zu installieren:
sudo apt-get install lirc -y
Keine Panik, die Installation wird folgenden Fehler melden: “Failed to start Flexible IR remote
input/output application support"
Das ist kein Problem, wir müssen jetzt nur den nächsten Befehl eingeben, und dann das Infrarot-Modul
erneut installieren:
sudo cp /etc/lirc/lirc_options.conf.dist /etc/lirc/lirc_options.conf
sudo apt-get install lirc -y
**2.** Jetzt müssen wir noch ein paar änderungen an der Infrarot-Konfiguration vornehmen:
sudo geany /etc/lirc/lirc_options.conf
Folgende Zeilen müssen geändert werden:
    driver = default
    device = /dev/lirc
**3.** Als nächstes geben wir folgenden Befehl ein um die Konfiguration lesbar zu machen:
sudo cp /etc/lirc/lircd.conf.dist /etc/lirc/lircd.conf
**4.** Jetzt starten wir das Infrarot-Modul:
sudo systemctl restart lircd.service
**5.** Ändern des Automatischen starten, so dass ab jetzt die Fernbedienung beim einschalten des Pis
benutzt werden kann:
sudo crontab -e
Hier haben wir in Punkt 10 eine neue Zeile eingefügt. Diese müssen wir jetzt wieder löschen (einfach
mit den Pfeiltasten bis zum Ende der Datei gehen und dann die letzte Zeile löschen und diese hier dafür
einfügen:
    @reboot python /home/pi/led-strip-fun/led-strip-ir.py
Jetzt nur noch mit “STRG + O” (mit Enter bestätigen) speichern, und dann mit “STRG + X” das
Programm wieder verlassen.
**6.** Die Fernbedienung
Die Fernbedienung ist so eingestellt, dass die tasten 0, 100+ und 200+ die Helligkeit regeln. 0 ist das
    dunkelste, 200+ ist das Hellste.
    Die Tasten 1, 2 und 3 sind zum wechseln zwischen den verschiedenen
    Visualisationseffekten.


Credits to https://github.com/jgarff/rpi_ws281x and https://github.com/scottlawsonbc/audio-reactive-led-strip
