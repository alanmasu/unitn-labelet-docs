# Laboratorio di Elettronica – Documentazione dei PC

Questa documentazione descrive **il laboratorio nel suo insieme**: quali server usa, quali dischi di rete monta, quali software ci sono, da dove arrivano e come si installano.

> **Per Veronica e Davide**: manca la parte sulle home di ateneo (`/home_ateneo/<USER>`). La sezione da compilare è [Home di ateneo](infrastruttura.md#home-di-ateneo--home_ateneo).

## Il laboratorio in breve

| Voce | Valore |
|---|---|
| Numero di PC | 9 |
| Indirizzi IP | da `10.196.30.91` a `10.196.30.99` |
| Sistema operativo | Rocky Linux 9.8 (installato come 9.6 XFCE, poi aggiornato) |
| Desktop | XFCE |
| Utente amministratore locale | `tech` (usa `sudo`) |

Tutti i PC hanno la stessa configurazione.

## Pagine della documentazione

| Pagina | Cosa contiene |
|---|---|
| [Server e dischi di rete](infrastruttura.md) | Server usati dal laboratorio, mount NFS, server delle licenze, home di ateneo |
| [Software](software.md) | Elenco di tutti i software: da dove arrivano, come si installano, come si avviano |
| [Configurazione di sistema](configurazione-sistema.md) | Installazione del sistema operativo, partizioni, SSH, X2Go, regole udev, script di avvio |
| [Manutenzione](manutenzione.md) | Link al repository Ansible e ultimo deploy, lavori da fare, storico delle modifiche |

## Convenzioni usate

- I software che non arrivano da un pacchetto `dnf` stanno in **`/opt/<nome>-<versione>/`**.
- Se serve uno script di avvio, sta in **`/opt/<nome>-<versione>/bin/`**.
- Per lanciare il programma da terminale c'è un **link simbolico in `/usr/bin/`**.
- Le variabili d'ambiente dei software grandi (Xilinx, Cadence) si impostano con gli script in **`/etc/profile.d/`**. Il sistema li esegue per ogni utente quando entra. Vedi [Configurazione di sistema](configurazione-sistema.md#script-in-etcprofiled).
