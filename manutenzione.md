# Manutenzione

[← Torna all'indice](README.md)

## Indice

- [Ansible](#ansible)
- [Lavori da fare](#lavori-da-fare)
- [Storico delle modifiche](#storico-delle-modifiche)

## Ansible

I PC si gestiscono tutti insieme con Ansible, dal repository [unitn-labelet-ansible](https://github.com/alanmasu/unitn-labelet-ansible/tree/main). Il suo README spiega come prepararlo e come usare i playbook: aggiornamento del sistema, distribuzione degli script in `/etc/profile.d/`, controllo dei mount.

**Ultimo deploy**: il 12/11/2026 è stato eseguito il deploy dal commit [1231b61](https://github.com/alanmasu/unitn-labelet-ansible/commit/1231b61964ed7bce53f700705182f0cae4360396).

## Lavori da fare

- [ ] Creare i file `.desktop` per [Logic 2](software.md#logic-2-saleae) e [GIMP](software.md#gimp).
- [ ] Aggiungere il repository Microsoft per [VS Code](software.md#vs-code), così si aggiorna con `dnf`.
- [ ] Compilare la sezione [Home di ateneo](infrastruttura.md#home-di-ateneo--home_ateneo) (Veronica e Davide).

## Storico delle modifiche

| Data | Modifica |
|---|---|
| 09/10/2025 | Installazione di Rocky Linux 9.6 XFCE e di tutti i software di base |
| 11/11/2025 | Creati gli script `/etc/profile.d/xilinx.sh` e `cadence.sh`; tolte le parti corrispondenti dal `.bashrc` |
| 12/11/2025 | Installate le regole udev per PlatformIO e Arduino |
| 13/11/2025 | Provato lo smontaggio automatico di `/eda` all'uscita (solo PC 03); poi abbandonato |
| 04/11/2026 | Aggiornamento con Ansible: rimosso Wine, aggiornata la chiave GPG di GitHub CLI, aggiornato tutto il sistema (Rocky 9.8), installato `yum-utils`, riavvio |
| 12/11/2026 | Uniformati gli script per Vivado e Cadence, ora gestiti da Ansible |
| 12/11/2026 | Corretto `xilinx.sh`: ora controlla `/tools` invece di `/eda`. Deploy eseguito dal commit [1231b61](https://github.com/alanmasu/unitn-labelet-ansible/commit/1231b61964ed7bce53f700705182f0cae4360396) |
