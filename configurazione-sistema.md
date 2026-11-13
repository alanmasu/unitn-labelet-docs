# Configurazione di sistema

[← Torna all'indice](README.md)

Qui c'è tutto quello che non è un'applicazione: installazione del sistema operativo, servizi e file di configurazione comuni a tutti i PC.

## Indice

- [Configurazione di sistema](#configurazione-di-sistema)
  - [Indice](#indice)
  - [Installazione del sistema operativo](#installazione-del-sistema-operativo)
  - [Repository aggiuntivi](#repository-aggiuntivi)
  - [SSH](#ssh)
  - [File di default per i nuovi utenti (/etc/skel)](#file-di-default-per-i-nuovi-utenti-etcskel)
  - [Regole udev](#regole-udev)
  - [Disattivazione del beep (pcspkr)](#disattivazione-del-beep-pcspkr)
  - [Script in /etc/profile.d](#script-in-etcprofiled)

## Installazione del sistema operativo

- **Sistema**: Rocky Linux 9.6 XFCE, poi aggiornato alla 9.8.
- **Tastiera**: italiana.

Partizioni:

| Partizione | Dimensione |
|---|---|
| swap | 8 GB |
| `/` | 50 GB |
| partizioni di sistema | quelle create dall'installer |
| `/scratch` | tutto lo spazio che resta |

Per controllare le partizioni:

```bash
df -h
lsblk
```

## Repository aggiuntivi

Su ogni PC sono attivi:

- **CRB** e **EPEL**
- **Flathub** (per Flatpak)
- **gh-cli** (GitHub CLI)

I comandi sono in [Software → Prerequisiti comuni](software.md#prerequisiti-comuni-epel-crb-flatpak) e in [GitHub CLI](software.md#github-cli-gh).

## SSH

Il server SSH è attivo, così si può entrare nei PC da remoto (serve anche ad Ansible):

```bash
sudo systemctl enable sshd.service
```

## File di default per i nuovi utenti (/etc/skel)

Quando si crea un nuovo utente, i file di `/etc/skel/` vengono copiati nella sua home.

- **`/etc/skel/.config/xfce4/xfconf/xfce-perchannel-xml/xfwm4.xml`**: compositing di XFCE disattivato, per X2Go (vedi [X2Go server](software.md#x2go-server-e-desktop-xfce)).

## Regole udev

Le regole udev permettono agli utenti normali di usare i dispositivi USB senza `sudo`.

| Regole | Per cosa | File |
|---|---|---|
| PlatformIO (ufficiali) | schede Arduino e altre schede di sviluppo | `/etc/udev/rules.d/99-platformio-udev.rules` |
| Saleae Logic | analizzatore logico | creata usando il comando fornito dall'app al primo avvio |

Installazione delle regole PlatformIO, dalla [guida ufficiale](https://docs.platformio.org/en/stable/core/installation/udev-rules.html):

```bash
curl -fsSL https://raw.githubusercontent.com/platformio/platformio-core/develop/platformio/assets/system/99-platformio-udev.rules | sudo tee /etc/udev/rules.d/99-platformio-udev.rules
sudo udevadm control --reload-rules
sudo udevadm trigger
```

## Disattivazione del beep (pcspkr)

Per spegnere il beep del sistema:

```bash
sudo rmmod pcspkr
echo "blacklist pcspkr" | sudo tee -a /etc/modprobe.d/blacklist.conf
```

## Script in /etc/profile.d

> ⚠️ **Dipendono dai dischi di rete.** Questi script servono solo se sono montati `/tools` (da [Zambesi](infrastruttura.md#zambesi--tools-xilinx)) ed `/eda` (da [Jabba](infrastruttura.md#jabba--eda-cadence-e-licenze)). Se un mount manca, lo script corrispondente non fa nulla e il software non si carica. Vedi [Server e dischi di rete](infrastruttura.md).

Questi script preparano l'ambiente per Xilinx e Cadence. Il sistema li esegue ogni volta che un utente entra.

| File | Cosa fa | Parte solo se |
|---|---|---|
| `/etc/profile.d/xilinx.sh` | carica `settings64.sh` di Vivado e Vitis 2025.1 | bash ≥ 4.2, `/tools` montato, non già eseguito |
| `/etc/profile.d/cadence.sh` | carica gli script Cadence, imposta `CDSHOME`, `LM_LICENSE_FILE` e gli alias del PDK | bash ≥ 4.2, `/eda` montato, non già eseguito |

Le variabili `XILINX_SETUP_DONE` e `CADENCE_SETUP_DONE` impediscono che gli script vengano eseguiti due volte.

**I file "ufficiali" stanno nel repository Ansible**, nella cartella [`files/`](https://github.com/alanmasu/unitn-labelet-ansible/tree/main/files). Per cambiarli:

1. modificare il file nel repository;
2. distribuirlo con i playbook `deploy_xilinx.yml` e `deploy_cadence.yml` (vedi il [README del repository](https://github.com/alanmasu/unitn-labelet-ansible/tree/main)).

Non fare modifiche a mano sui singoli PC, usare la repo `ansible` per tenere tutto sincronizzato e versionato.