# Configurazione di sistema

[← Torna all'indice](README.md)

Qui c'è tutto quello che non è un'applicazione: installazione del sistema operativo, servizi e file di configurazione comuni a tutti i PC.

## Indice

- [Configurazione di sistema](#configurazione-di-sistema)
  - [Indice](#indice)
  - [Installazione del sistema operativo](#installazione-del-sistema-operativo)
  - [Repository aggiuntivi](#repository-aggiuntivi)
  - [SSH](#ssh)
  - [X2Go server e desktop XFCE](#x2go-server-e-desktop-xfce)
  - [Script in /etc/profile.d](#script-in-etcprofiled)
  - [File di default per i nuovi utenti (/etc/skel)](#file-di-default-per-i-nuovi-utenti-etcskel)
  - [Regole udev](#regole-udev)
  - [Disattivazione del beep (pcspkr)](#disattivazione-del-beep-pcspkr)

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

## X2Go server e desktop XFCE

X2Go permette di usare il desktop dei PC da remoto. Comandi eseguiti:

```bash
sudo dnf update
sudo dnf install epel-release
sudo dnf groupinstall "xfce"
sudo dnf install lightdm
sudo dnf config-manager --set-enabled crb
sudo dnf -y update
sudo dnf install x2goserver x2goserver-xsession
```

Nelle note originali c'erano anche questi comandi, **ma non sono stati eseguiti**:

- `dnf config-manager --set-enabled powertools`
- `dnf copr enable stenstorp/lightdm`

**Compositing di XFCE disattivato**: il compositing del window manager di XFCE (xfwm4) dà problemi nelle sessioni X2Go. Per questo è stato disattivato per tutti i nuovi utenti con il file:

```
/etc/skel/.config/xfce4/xfconf/xfce-perchannel-xml/xfwm4.xml
```

È una copia del file standard di XFCE. L'unica differenza è che l'opzione `use_compositing` è disattivata (`false`).

Il file in `/etc/skel` vale solo per gli utenti creati dopo la modifica. Gli utenti che esistevano già lo cambiano a mano editanto il file.

## Script in /etc/profile.d

Questi script preparano l'ambiente per Xilinx e Cadence. Il sistema li esegue ogni volta che un utente entra.

| File | Cosa fa | Parte solo se |
|---|---|---|
| `/etc/profile.d/xilinx.sh` | carica `settings64.sh` di Vivado e Vitis 2025.1 | bash ≥ 4.2, `/tools` montato, non già eseguito |
| `/etc/profile.d/cadence.sh` | carica gli script Cadence, imposta `CDSHOME`, `LM_LICENSE_FILE` e gli alias del PDK | bash ≥ 4.2, `/eda` montato, non già eseguito |

Le variabili `XILINX_SETUP_DONE` e `CADENCE_SETUP_DONE` impediscono che gli script vengano eseguiti due volte.

**I file "ufficiali" stanno nel repository Ansible**, nella cartella [`files/`](https://github.com/alanmasu/unitn-labelet-ansible/tree/main/files). Per cambiarli:

1. modificare il file nel repository;
2. distribuirlo con i playbook `deploy_xilinx.yml` e `deploy_cadence.yml` (vedi il [README del repository](https://github.com/alanmasu/unitn-labelet-ansible/tree/main)).

Non modificarli a mano sui singoli PC.

> **Nota**: fino al 12/11/2026 `xilinx.sh` controllava per errore `/eda` invece di `/tools`. L'errore è corretto nel commit [1231b61](https://github.com/alanmasu/unitn-labelet-ansible/commit/1231b61964ed7bce53f700705182f0cae4360396), da cui il 12/11/2026 è stato eseguito il deploy sui PC.

## File di default per i nuovi utenti (/etc/skel)

Quando si crea un nuovo utente, i file di `/etc/skel/` vengono copiati nella sua home.

- **`/etc/skel/.config/xfce4/xfconf/xfce-perchannel-xml/xfwm4.xml`**: compositing di XFCE disattivato, per X2Go (vedi [sopra](#x2go-server-e-desktop-xfce)).

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
