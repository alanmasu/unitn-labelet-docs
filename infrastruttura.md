# Server e dischi di rete

[← Torna all'indice](README.md)

I PC del laboratorio dipendono da alcuni server esterni. Senza questi server alcuni software (Xilinx, Cadence) e le home degli utenti **non sono disponibili**.

## Indice

- [Server e dischi di rete](#server-e-dischi-di-rete)
  - [Indice](#indice)
  - [Riepilogo](#riepilogo)
  - [Pacchetti necessari per NFS](#pacchetti-necessari-per-nfs)
  - [Zambesi – /tools (Xilinx)](#zambesi--tools-xilinx)
  - [Jabba – /eda (Cadence) e licenze](#jabba--eda-cadence-e-licenze)
  - [Home di ateneo – /home\_ateneo](#home-di-ateneo--home_ateneo)
  - [Come controllare i mount](#come-controllare-i-mount)

## Riepilogo

| Server | Cosa offre | Dove si monta sui PC | Tipo | Usato da |
|---|---|---|---|---|
| `zambesi.disi.unitn.it` | export `/tools` | `/tools` | NFS, da `/etc/fstab` | Vivado, Vitis |
| `jabba.disi.unitn.it` | export `/eda` | `/eda` | NFS, da `/etc/fstab` | Cadence, PDK LFoundry |
| `jabba.disi.unitn.it` | server licenze, porta `5280` | – | FlexLM | Cadence |
| ⚠️ DA COMPILARE | home di ateneo | `/home_ateneo/<USER>` | ⚠️ DA COMPILARE | utenti |

## Pacchetti necessari per NFS

```bash
sudo dnf install nfs-utils
systemctl status rpcbind      # controllare che il servizio sia attivo
```

## Zambesi – /tools (Xilinx)

Contiene le installazioni di Xilinx (Vivado e Vitis). Il software sul server lo gestiamo noi.

Versioni presenti:

- **2025.1** – quella in uso: `/tools/Xilinx/2025.1/`
- **2021.2** – ancora presente in `/tools/Xilinx/Vivado/2021.2/` e `/tools/Xilinx/Vitis/2021.2/`, ma oggi è complicata da usare

Creazione del punto di mount:

```bash
sudo mkdir /tools
```

Riga in `/etc/fstab`:

```
zambesi.disi.unitn.it:/tools /tools nfs defaults 0 0
```

Per montarlo subito senza riavviare:

```bash
sudo mount /tools
```

## Jabba – /eda (Cadence) e licenze

Contiene la suite Cadence (`/eda/cadence/2024-25/`) e il PDK LFoundry (`/eda/libraries/lfoundry_lf11is/`).

Creazione del punto di mount:

```bash
sudo mkdir /eda
```

Riga in `/etc/fstab`:

```
jabba.disi.unitn.it:/eda /eda nfs defaults 0 0
```

Per montarlo subito senza riavviare:

```bash
sudo mount /eda
```

**Server delle licenze Cadence**: `5280@jabba.disi.unitn.it`, oppure con l'indirizzo IP `5280@192.168.160.6`. Il valore si imposta nella variabile `LM_LICENSE_FILE` dentro lo script [cadence.sh](configurazione-sistema.md#script-in-etcprofiled).

## Home di ateneo – /home_ateneo

> ⚠️ **DA COMPILARE** – sezione da completare a cura di Veronica e Davide.

Sui PC si montano le home di ateneo in `/home_ateneo/<USER>`.

| Voce | Valore |
|---|---|
| Server | ⚠️ |
| Protocollo (NFS, CIFS, …) | ⚠️ |
| Come avviene il mount (fstab, autofs, pam_mount, …) | ⚠️ |
| File di configurazione coinvolti | ⚠️ |
| Pacchetti necessari | ⚠️ |
| Autenticazione degli utenti | ⚠️ |

## Come controllare i mount

Su un singolo PC:

```bash
df -h
findmnt /tools
findmnt /eda
```

Su tutti i PC insieme si usa il playbook Ansible `check_mounts.yml`. Controlla `/eda`, `/tools` e `/home_ateneo/<USER>`. Vedi il [repository Ansible](https://github.com/alanmasu/unitn-labelet-ansible/tree/main).
