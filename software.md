# Software

[← Torna all'indice](README.md)

Questa pagina elenca tutti i software del laboratorio, divisi per **provenienza**, cioè da dove arrivano e come si installano.

## Indice

- [Riepilogo](#riepilogo)
- [Prerequisiti comuni (EPEL, CRB, Flatpak)](#prerequisiti-comuni-epel-crb-flatpak)
- [1. Software da server di rete](#1-software-da-server-di-rete)
  - [Vivado e Vitis (Xilinx)](#vivado-e-vitis-xilinx)
  - [Cadence](#cadence)
- [2. Software da repository dnf](#2-software-da-repository-dnf)
  - [Software gia presenti nel sistema](#software-gia-presenti-nel-sistema)
  - [Pacchetti dei repository di Rocky ed EPEL](#pacchetti-dei-repository-di-rocky-ed-epel)
  - [GitHub CLI (gh)](#github-cli-gh)
- [3. Software da Flatpak](#3-software-da-flatpak)
  - [KiCad](#kicad)
  - [Thonny](#thonny)
- [4. Pacchetti rpm scaricati dal sito](#4-pacchetti-rpm-scaricati-dal-sito)
  - [VS Code](#vs-code)
  - [WaveForms e Adept Runtime (Digilent)](#waveforms-e-adept-runtime-digilent)
- [5. Software in /opt](#5-software-in-opt)
  - [Arduino IDE 1.8.19](#arduino-ide-1819)
  - [Arduino IDE 2.3.6](#arduino-ide-236)
  - [Logic 2 (Saleae)](#logic-2-saleae)
  - [GIMP](#gimp)
  - [PulseView (sigrok)](#pulseview-sigrok)
  - [GHDL](#ghdl)
- [Software provati e scartati](#software-provati-e-scartati)

## Riepilogo

| Software | Versione | Provenienza | Dove si trova | Comando | Note |
|---|---|---|---|---|---|
| [Vivado / Vitis](#vivado-e-vitis-xilinx) | 2025.1 (anche 2021.2) | NFS da Zambesi | `/tools/Xilinx/` | `vivado`, `vitis` | |
| [Cadence](#cadence) | 2024-25 | NFS da Jabba | `/eda/cadence/2024-25/` | `virtuoso`, … | |
| [LibreOffice](#software-gia-presenti-nel-sistema) | da repository | già presente nell'immagine Rocky XFCE | sistema | `libreoffice` | |
| [GTKWave](#pacchetti-dei-repository-di-rocky-ed-epel) | da repository | dnf | sistema | `gtkwave` | |
| [gcc, g++, make, …](#pacchetti-dei-repository-di-rocky-ed-epel) | da repository | dnf (gruppo "Development Tools") | sistema | `gcc`, `g++` | |
| [Python 3](#software-gia-presenti-nel-sistema) | 3.9 | già presente nell'immagine Rocky XFCE | sistema | `python3` | |
| [git](#software-gia-presenti-nel-sistema) | da repository | già presente nell'immagine Rocky XFCE | sistema | `git` | |
| [X2Go Client](#pacchetti-dei-repository-di-rocky-ed-epel) | 4.1.2.3 | dnf (EPEL) | sistema | `x2goclient` | |
| [GitHub CLI](#github-cli-gh) | da repository | dnf (repository GitHub) | sistema | `gh` | |
| [KiCad](#kicad) | 9 (canale stable) | Flatpak (Flathub) | Flatpak + `/opt/kicad-9.0.5/` | `kicad-9.0.5` | |
| [Thonny](#thonny) | da Flathub | Flatpak (Flathub) | Flatpak + `/opt/thonny/` | `thonny` | |
| [VS Code](#vs-code) | – | rpm scaricato dal sito | sistema | `code` | non si aggiorna con `dnf` |
| [WaveForms + Adept Runtime](#waveforms-e-adept-runtime-digilent) | – | sito Digilent | – | – | installazione non documentata nel dettaglio |
| [Arduino IDE](#arduino-ide-1819) | 1.8.19 | archivio dal sito | `/opt/arduino-1.8.19/` | `arduino` | |
| [Arduino IDE](#arduino-ide-236) | 2.3.6 | zip dal sito | `/opt/arduino-2.3.6/` | – | nessun comando da terminale documentato |
| [Logic 2](#logic-2-saleae) | 2.4.36 | AppImage dal sito | `/opt/logic-2.4.36/` | `logic-2.4.36` | manca l'icona nel menu |
| [GIMP](#gimp) | 3.0.6 | AppImage dal sito | `/opt/gimp-3.0.6/` | `gimp-3.0.6` | manca l'icona nel menu |
| [PulseView](#pulseview-sigrok) | 0.4.2 | AppImage dal sito | `/opt/PulseView-0.4.2/` | `pulseView-0.4.2` | |
| [GHDL](#ghdl) | 5.1.1 | sorgenti da GitHub, compilato | `/opt/ghdl-5.1.1/` | `ghdl` | |

---

## Prerequisiti comuni (EPEL, CRB, Flatpak)

Diversi software hanno bisogno di questi repository. Vanno installati **una volta sola** su ogni PC.

```bash
# Repository CRB e EPEL
sudo dnf config-manager --set-enabled crb
sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm

# Flatpak e repository Flathub
sudo dnf install flatpak
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

---

## 1. Software da server di rete

Il software non è installato sui PC ma sta su un server e arriva con NFS (vedi [Server e dischi di rete](infrastruttura.md)). Sul PC bisogna solo:

1. montare il disco di rete;
2. installare le librerie che mancano;
3. impostare l'ambiente con uno script in `/etc/profile.d/`.

### Vivado e Vitis (Xilinx)

- **Provenienza**: `/tools` da `zambesi.disi.unitn.it` (vedi [Zambesi](infrastruttura.md#zambesi--tools-xilinx)).
- **Versione in uso**: 2025.1, in `/tools/Xilinx/2025.1/`.
- **Versione vecchia**: 2021.2, ancora sul server ma oggi complicata da usare.

**Ambiente**: lo script `/etc/profile.d/xilinx.sh` carica i file `settings64.sh` di Vivado e Vitis 2025.1. Lo script è gestito con Ansible (vedi [Script in /etc/profile.d](configurazione-sistema.md#script-in-etcprofiled)).

**Driver dei cavi JTAG**: vanno installati in locale su ogni PC, come root.

```bash
cd <cartella di Vivado>/data/xicom/cable_drivers/lin64/install_script/install_drivers/
sudo ./install_drivers.sh
```

- Per la 2021.2 la cartella era `/tools/Xilinx/Vivado/2021.2/`.
- Per la 2025.1 non è documentato se i driver sono stati reinstallati.

**Libreria `libtinfo.so.5`**: per la 2021.2 è stato creato questo link:

```bash
sudo ln -s /usr/lib64/libtinfo.so.6 /usr/lib64/libtinfo.so.5
```

Non è stato verificato se serve ancora con la 2025.1.

### Cadence

- **Provenienza**: `/eda` da `jabba.disi.unitn.it` (vedi [Jabba](infrastruttura.md#jabba--eda-cadence-e-licenze)).
- **Versione**: pacchetto 2024-25, in `/eda/cadence/2024-25/`.
- **Strumenti caricati**: IC 23.10 (Virtuoso), Spectre 24.10, Xcelium 24.03, Conformal 24.10, DDI Export 23.33, SSV 23.11, Pegasus 24.10, Quantus 23.11, Assura 04.17, PVS 24.10.
- **PDK**: LFoundry LF11IS, versione 1.3.2, in `/eda/libraries/lfoundry_lf11is/PDK_LF11ISi_V1_3_2/`.
- **Licenze**: `5280@jabba.disi.unitn.it`.

**Dipendenze da installare su ogni PC:**

```bash
sudo dnf install ksh
sudo dnf --enablerepo=devel install mesa-libGLU     # libGLU.so.1
sudo dnf install libXScrnSaver                      # libXss.so.1
sudo dnf install motif                              # libXm.so.4
sudo dnf install libnsl                             # libnsl.so.1
sudo dnf --enablerepo=devel install apr-util        # libaprutil
sudo dnf install xcb-util-wm xcb-util-image xcb-util-keysyms xcb-util-renderutil
sudo dnf makecache --refresh
sudo dnf install xorg-x11-server-Xvfb
```

Nota: il link `libz.so → libz.so.1` in `/usr/lib64` esisteva già, quindi non è stato creato.

**Ambiente**: lo script `/etc/profile.d/cadence.sh` carica gli script dei vari strumenti e imposta `CDSHOME` e `LM_LICENSE_FILE`. Parte solo se `/eda` è montato. Crea anche due alias per il PDK:

| Alias | A cosa serve |
|---|---|
| `lfoundry_pdk_dircreate` | crea una cartella di lavoro per il PDK |
| `lfoundry_pdk_python` | applica la correzione Python per Virtuoso |

---

## 2. Software da repository dnf

Questi software si aggiornano insieme al sistema con `dnf update`.

### Software gia presenti nel sistema

Questi programmi erano già nell'immagine di installazione di Rocky Linux XFCE, insieme ai programmi standard del desktop. Non è servito installarli:

- **LibreOffice**
- **Python 3** (versione 3.9)
- **git**

### Pacchetti dei repository di Rocky ed EPEL

```bash
sudo dnf install gtkwave
sudo dnf groupinstall "Development Tools"           # gcc, g++, make, …
sudo dnf install x2goclient-4.1.2.3-1.el9.x86_64    # serve EPEL
```

### GitHub CLI (gh)

Arriva dal repository ufficiale di GitHub.

```bash
curl -fsSL https://cli.github.com/packages/rpm/gh-cli.repo | sudo tee /etc/yum.repos.d/gh-cli.repo
sudo dnf -y install gh
```

Nel 2026 GitHub ha cambiato la chiave GPG del repository. Il playbook Ansible `update.yml` riscrive il file `/etc/yum.repos.d/gh-cli.repo` con la nuova chiave (vedi il [repository Ansible](https://github.com/alanmasu/unitn-labelet-ansible/tree/main)).

---

## 3. Software da Flatpak

Servono i [prerequisiti](#prerequisiti-comuni-epel-crb-flatpak). Per poterli lanciare da terminale con un nome semplice si crea uno script in `/opt/<nome>/bin/` e un link in `/usr/bin/`.

### KiCad

```bash
flatpak install --from https://flathub.org/repo/appstream/org.kicad.KiCad.flatpakref
```

Script di avvio `/opt/kicad-9.0.5/bin/kicad-9.0.5`:

```bash
#!/bin/bash
flatpak run org.kicad.KiCad//stable
```

```bash
sudo chmod +x /opt/kicad-9.0.5/bin/kicad-9.0.5
sudo ln -s /opt/kicad-9.0.5/bin/kicad-9.0.5 /usr/bin/kicad-9.0.5
```

Nota: lo script usa il canale `stable` di Flatpak. Dopo un aggiornamento la versione reale può essere diversa da 9.0.5, anche se il nome dello script resta `kicad-9.0.5`.

### Thonny

```bash
flatpak install org.thonny.Thonny
```

Script di avvio `/opt/thonny/bin/thonny`:

```bash
#!/bin/bash
flatpak run org.thonny.Thonny
```

```bash
sudo chmod +x /opt/thonny/bin/thonny
sudo ln -s /opt/thonny/bin/thonny /usr/bin/thonny
```

---

## 4. Pacchetti rpm scaricati dal sito

Questi pacchetti **non si aggiornano** con `dnf update`, perché non c'è un repository configurato.

### VS Code

Pacchetto `.rpm` scaricato dal sito ufficiale:

```bash
sudo dnf install ./code_<versione>.rpm
```

**Da fare**: aggiungere il repository Microsoft, così VS Code si aggiorna con il sistema (vedi [Lavori da fare](manutenzione.md#lavori-da-fare)).

### WaveForms e Adept Runtime (Digilent)

Fonte: [guida di installazione Digilent per Linux](https://digilent.com/reference/software/waveforms/waveforms-3/getting-started-guide).

1. Installare le dipendenze:
   ```bash
   sudo dnf install qt5-qtmultimedia qt5-qtscript qt5-qtserialport
   ```
2. Installare **Adept Runtime**.
3. Installare **WaveForms**.

Versioni e file scaricati non sono documentati.

---

## 5. Software in /opt

Software scaricati come archivio o AppImage, oppure compilati. Seguono la [convenzione](README.md#convenzioni-usate) `/opt/<nome>-<versione>/` con il link in `/usr/bin/`. **Non si aggiornano da soli.**

### Arduino IDE 1.8.19

```bash
# archivio arduino-1.8.19-linux64.tar.xz scaricato dal sito ufficiale
sudo cp arduino-1.8.19-linux64.tar.xz /opt/
sudo tar -xf /opt/arduino-1.8.19-linux64.tar.xz -C /opt/
sudo ln -s /opt/arduino-1.8.19/arduino /usr/bin/arduino
```

Icona nel menu:

```bash
sudo cp /opt/arduino-1.8.19/lib/desktop.template /opt/arduino-1.8.19/arduino-1.8.19.desktop
```

Nel file `.desktop` si modificano questi campi:

```
Icon=/opt/arduino-1.8.19/lib/arduino.png
Exec=/opt/arduino-1.8.19/arduino
```

> Nota: nel log originale i due percorsi erano scritti senza la `/` iniziale (`opt/...`). Se l'icona non funziona, controllare questo.

```bash
sudo ln -s /opt/arduino-1.8.19/arduino-1.8.19.desktop /usr/local/share/applications/arduino-1.8.19.desktop
```

### Arduino IDE 2.3.6

Zip scaricato dal sito ufficiale, estratto e copiato in `/opt/arduino-2.3.6/`.

File `/opt/arduino-2.3.6/arduino-2.3.6.desktop`:

```ini
[Desktop Entry]
Name=Arduino IDE 2.3.6
Exec=/opt/arduino-2.3.6/arduino-ide %U
Terminal=false
Type=Application
Icon=/opt/arduino-2.3.6/resources/app/resources/icons/512x512.png
StartupWMClass=Arduino IDE
X-AppImage-Version=2.3.6
Comment=Arduino IDE
Categories=Development;
```

```bash
sudo ln -s /opt/arduino-2.3.6/arduino-2.3.6.desktop /usr/local/share/applications/arduino-2.3.6.desktop
```

Per la 2.3.6 non è documentato un comando da terminale (nessun link in `/usr/bin/`).

Per le regole udev delle schede Arduino vedi [Regole udev](configurazione-sistema.md#regole-udev).

### Logic 2 (Saleae)

```bash
# AppImage scaricata dal sito ufficiale
sudo mkdir -p /opt/logic-2.4.36/bin
sudo mv Logic-2.4.36-linux-x64.AppImage /opt/logic-2.4.36/
sudo chmod +x /opt/logic-2.4.36/Logic-2.4.36-linux-x64.AppImage
sudo dnf install libnsl          # dipendenza mancante
```

Script di avvio `/opt/logic-2.4.36/bin/logic-2.4.36`:

```bash
#!/bin/bash
/opt/logic-2.4.36/Logic-2.4.36-linux-x64.AppImage
```

```bash
sudo chmod +x /opt/logic-2.4.36/bin/logic-2.4.36
sudo ln -s /opt/logic-2.4.36/bin/logic-2.4.36 /usr/bin/logic-2.4.36
```

**Accesso USB**: al primo avvio l'app mostra un comando che abilita la regola udev per l'analizzatore. Quel comando è stato eseguito, ma la regola creata non è documentata.

**Manca il file `.desktop`** (vedi [Lavori da fare](manutenzione.md#lavori-da-fare)).

### GIMP

```bash
# AppImage scaricata dal sito ufficiale
sudo chmod +x GIMP-3.0.6-x86_64.AppImage
sudo mkdir -p /opt/gimp-3.0.6/bin
sudo mv GIMP-3.0.6-x86_64.AppImage /opt/gimp-3.0.6/
```

Script di avvio `/opt/gimp-3.0.6/bin/gimp-3.0.6`:

```bash
#!/bin/bash
/opt/gimp-3.0.6/GIMP-3.0.6-x86_64.AppImage
```

```bash
sudo chmod +x /opt/gimp-3.0.6/bin/gimp-3.0.6
sudo ln -s /opt/gimp-3.0.6/bin/gimp-3.0.6 /usr/bin/gimp-3.0.6
```

**Manca il file `.desktop`** (vedi [Lavori da fare](manutenzione.md#lavori-da-fare)).

### PulseView (sigrok)

AppImage scaricata da [sigrok.org](https://sigrok.org/download/binary/pulseview/PulseView-0.4.2-x86_64.AppImage).

```bash
sudo chmod +x PulseView-0.4.2-x86_64.AppImage
sudo mkdir -p /opt/PulseView-0.4.2
sudo cp PulseView-0.4.2-x86_64.AppImage /opt/PulseView-0.4.2/
sudo ln -s /opt/PulseView-0.4.2/PulseView-0.4.2-x86_64.AppImage /usr/bin/pulseView-0.4.2
```

Qui non c'è uno script di avvio: il link punta direttamente all'AppImage.

### GHDL

Simulatore VHDL. Si compila dai sorgenti presi da GitHub.

```bash
# archivio zip dei sorgenti scaricato da GitHub ed estratto in /opt/ghdl-5.1.1
sudo dnf install gcc-gnat
sudo /opt/ghdl-5.1.1/configure --prefix=/opt/ghdl-5.1.1/bin
sudo make install
sudo ln -s /opt/ghdl-5.1.1/bin/ghdl /usr/bin/
```

L'eseguibile si trova in `/opt/ghdl-5.1.1/bin/ghdl`.

---

## Software provati e scartati

| Software | Esito |
|---|---|
| **LTspice con Wine** | Non funziona. Wine è stato poi **rimosso** con Ansible, perché impediva l'aggiornamento di Mesa in Rocky 9.8. Anche winetricks, installato in `/opt/winetricks/`, non serve più; non è documentato se è stato tolto. |
| **NGSpice** | Si è provato a compilarlo, senza risultato. |
