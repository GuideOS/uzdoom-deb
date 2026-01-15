# UZDoom Debian Package Build Instructions

Dieses Repository enthält die Debian-Packaging-Dateien für **UZDoom**, einen modernen Source Port für das klassische Spiel DOOM.

## Voraussetzungen

### System-Anforderungen
- Debian 13 (Trixie) oder höher / Ubuntu 24.04+
- ~2 GB freier Festplattenspeicher für den Build
- Internetzugang (für Git-Clone und Dependencies)

### Build-Tools installieren
```bash
sudo apt-get update
sudo apt-get install -y git debhelper devscripts
```

## Schnellanleitung

### 1. UZDoom-Quellcode klonen
```bash
git clone https://github.com/UZDoom/UZDoom.git
cd UZDoom
```

### 2. Debian-Packaging-Dateien kopieren
```bash
# Dieses debian-Verzeichnis ins geklonte Repository kopieren
cp -r /pfad/zu/diesem/debian .
```

### 3. Build-Dependencies installieren
```bash
sudo apt-get install -y \
  cmake \
  g++ \
  make \
  libsdl2-dev \
  libvpx-dev \
  libasound2-dev \
  libgtk-3-dev \
  libbz2-dev \
  zlib1g-dev \
  libjpeg-dev \
  libfluidsynth-dev \
  libgme-dev \
  libopenal-dev \
  libmpg123-dev \
  libsndfile1-dev
```

### 4. Paket bauen
```bash
dpkg-buildpackage -us -uc -b
```

Das fertige `.deb`-Paket befindet sich nach dem erfolgreichen Build im **übergeordneten Verzeichnis** (`../`).

### 5. Paket installieren
```bash
cd ..
sudo dpkg -i uzdoom_1.0.0-1_amd64.deb
```

Falls Abhängigkeiten fehlen:
```bash
sudo apt-get install -f
```

## Paket-Details

### Enthaltene Komponenten
- **Binary:** `/usr/bin/uzdoom`
- **Game Assets:** `/usr/share/games/uzdoom/*.pk3`
- **Soundfonts:** `/usr/share/games/uzdoom/soundfonts/`
- **FM Banks:** `/usr/share/games/uzdoom/fm_banks/`
- **Desktop-Eintrag:** `/usr/share/applications/org.zdoom.UZDoom.desktop`
- **Icon:** `/usr/share/pixmaps/uzdoom.png`
- **AppStream-Metadaten:** `/usr/share/metainfo/org.zdoom.UZDoom.metainfo.xml`

### Paket-Informationen
- **Paketname:** uzdoom
- **Version:** 1.0.0-1
- **Architektur:** amd64
- **Größe:** ~28 MB
- **Installierte Größe:** ~57 MB

## UZDoom starten

Nach der Installation kann UZDoom auf verschiedene Arten gestartet werden:

### Über das Anwendungsmenü
Suche nach "UZDoom" im Anwendungsmenü unter Spiele.

### Über die Kommandozeile
```bash
# Mit IWAD-Datei:
uzdoom -iwad /pfad/zur/doom.wad

# Mit PWADs:
uzdoom -iwad doom2.wad -file mein-mod.wad

# Hilfe anzeigen:
uzdoom --help
```

**Hinweis:** Du benötigst eine DOOM IWAD-Datei (doom.wad, doom2.wad, plutonia.wad, tnt.wad, etc.) um zu spielen.

## Debian-Packaging-Dateien

### Struktur
```
debian/
├── changelog          # Versionshistorie
├── control            # Paket-Metadaten und Dependencies
├── copyright          # Lizenzinformationen
├── rules              # Build-Anweisungen (Makefile)
├── uzdoom.desktop     # Desktop-Eintrag
├── uzdoom.docs        # Dokumentations-Dateien
├── uzdoom.install     # Zusätzliche zu installierende Dateien
└── source/
    └── format         # Paket-Format (3.0 native)
```

### Anpassungen

#### Version ändern
Bearbeite `debian/changelog` und aktualisiere die Version:
```bash
dch -i  # Öffnet Editor für neuen Changelog-Eintrag
```

#### Dependencies anpassen
Bearbeite `debian/control` und ändere die `Build-Depends` oder `Depends` Felder.

#### Build-Optionen
Bearbeite `debian/rules` um CMake-Optionen anzupassen:
```makefile
override_dh_auto_configure:
	dh_auto_configure -- \
		-DCMAKE_BUILD_TYPE=Release \
		-DCMAKE_INSTALL_PREFIX=/usr \
		-DSYSTEMINSTALL=ON \
		-DEINE_OPTION=ON
```

## Fehlerbehebung

### Build schlägt fehl: "unerfüllte Bauabhängigkeiten"
```bash
# Automatische Installation aller Dependencies:
sudo apt-get build-dep .
```

### "Error copying file ... org.zdoom.UZDoom.metainfo.xml"
Diese Warnung kann ignoriert werden. Das Paket wird trotzdem erfolgreich gebaut.

### libvpx fehlt
```bash
sudo apt-get install libvpx-dev
```

### Paket-Build bereinigen
```bash
debian/rules clean
# oder
dh clean
```

## Paket deinstallieren

```bash
sudo dpkg -r uzdoom
```

Oder mit Konfigurationsdateien:
```bash
sudo dpkg --purge uzdoom
```

## Links

- **UZDoom GitHub:** https://github.com/UZDoom/UZDoom
- **ZDoom Wiki:** https://zdoom.org/wiki/
- **Forum:** https://forum.zdoom.org/
- **Discord:** https://dsc.gg/zdoom

## Lizenz

UZDoom ist unter der **GNU General Public License v3.0 oder höher** lizenziert.

Siehe `debian/copyright` für vollständige Lizenzinformationen.

## Support

Bei Problemen mit dem Debian-Paket:
1. Prüfe die Build-Logs in `/tmp/`
2. Überprüfe installierte Dependencies
3. Konsultiere das UZDoom Wiki
4. Frage im Forum oder Discord

---

**Build-Datum:** Januar 2026  
**Maintainer:** UZDoom Team
