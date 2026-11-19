# Medtrum EasyView — Integrazione per Home Assistant

🇬🇧 [English](README.md) | 🇮🇹 Italiano | 🇷🇺 [Русский](README_RU.md)
---

## ❤️ Sostieni il progetto

Se trovi utile questa integrazione, puoi supportarne lo sviluppo:

[![Boosty](https://img.shields.io/badge/Boosty-Supporta-F15F2C?style=for-the-badge&logo=boosty&logoColor=white)](https://boosty.to/alex_khmelenko)

Ogni donazione aiuta a sviluppare l'integrazione più velocemente: nuove funzionalità, correzioni di bug e supporto per le pompe di insulina (in programma).

[![Apri la tua istanza Home Assistant e aggiungi un repository in HACS.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=outsiderz&repository=easyview-easyfollow&category=integration)

![HACS Custom](https://img.shields.io/badge/HACS-Custom-41BDF5?style=flat-square)
![GitHub Release](https://img.shields.io/github/v/release/outsiderz/easyview-easyfollow?style=flat-square)
![GitHub License](https://img.shields.io/github/license/outsiderz/easyview-easyfollow?style=flat-square)

Questa integrazione permette di visualizzare i dati CGM (Continuous Glucose Monitor) in tempo reale dai sensori **Medtrum** in Home Assistant, tramite il cloud **EasyView**.

---

## Come funziona

Il sensore Medtrum trasmette i dati all'app **EasyView** sul telefono del paziente. Tramite la funzione di condivisione Follow, questi dati vengono inviati al cloud e resi disponibili a un secondo account tramite la funzione **EasyView Follow**.

L'integrazione Home Assistant si connette a questo account Follow per leggere i dati in tempo reale.

---

## Prerequisiti

- Un sensore Medtrum CGM attivo
- L'app **EasyView** installata sul telefono del paziente, con un account Medtrum registrato
- Un **secondo account Medtrum** da usare come account Follow (può essere un tuo account secondario)
- Home Assistant con HACS installato

---

## Passo 1 — Invita l'account Follow da EasyView

Sul telefono del paziente, nell'app **EasyView**:

1. Apri l'app e vai su **Impostazioni**
2. Seleziona **Follow** o **Condivisione**
3. Inserisci l'indirizzo email del secondo account
4. Invia l'invito

---

## Passo 2 — Accetta l'invito e verifica l'accesso

Sul dispositivo dove hai ricevuto l'invito:

1. Accetta l'invito tramite email o notifica
2. Accedi all'account EasyView Follow
3. Verifica che i dati del paziente siano visibili

---

## Passo 3 — Installa l'integrazione in Home Assistant

### Tramite HACS

[![Apri la tua istanza Home Assistant e aggiungi un repository in HACS.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=outsiderz&repository=easyview-easyfollow&category=integration)

1. Clicca il pulsante sopra o apri HACS manualmente
2. Vai su **Integrations** → clicca sui tre punti in alto a destra → **Custom repositories**
3. Inserisci l'URL del repository: `https://github.com/outsiderz/easyview-easyfollow`
4. Seleziona la categoria **Integration**
5. Clicca **Add**
6. Cerca **EasyView** in HACS e installalo
7. Riavvia Home Assistant

### Installazione manuale

1. Copia la cartella `custom_components/easyview` nella directory `custom_components` di Home Assistant
2. Riavvia Home Assistant

---

## Passo 4 — Configura l'integrazione

1. In Home Assistant vai su **Impostazioni** → **Dispositivi e servizi**
2. Clicca **Aggiungi integrazione** e cerca **EasyView**
3. **Passo 1 — Credenziali:**
   - **Email** e **Password** dell'account EasyView Follow
   - **Unità di misura** — `mg/dL` o `mmol/L`
4. **Passo 2 — Soglie:**
   - **Soglia glicemia alta** — valore sopra il quale si attiva il sensore *Alto* (default: 180 mg/dL o 10.0 mmol/L)
   - **Soglia glicemia bassa** — valore sotto il quale si attiva il sensore *Basso* (default: 70 mg/dL o 3.9 mmol/L)
   
   Le soglie vengono inserite nell'unità scelta al passo 1.
5. Clicca **Invia**

Se le credenziali sono corrette, l'integrazione si connetterà e i sensori verranno creati automaticamente.

---

## Sensori disponibili

Per ogni utente monitorato vengono creati:

| Sensore | Descrizione | Unità |
|---------|-------------|-------|
| Glicemia | Valore corrente | mg/dL o mmol/L |
| Tendenza glicemia | Testo + icona (Stabile, In aumento, In diminuzione, ecc.) | — |
| Stato sensore | Normale / Riscaldamento / Necessita calibrazione | — |
| Batteria | Percentuale batteria sensore | % |
| Minuti dall'ultimo aggiornamento | Minuti dall'ultima lettura | min |
| Calibrazione tra | Misurazioni rimanenti alla prossima calibrazione | — |
| Durata sensore | Tempo di funzionamento totale del sensore | min |
| Alto | On / Off (soglia configurabile) | — |
| Basso | On / Off (soglia configurabile) | — |

---

## Scheda per la dashboard

Per mostrare il valore sulla dashboard, [SugarTV Card](https://github.com/wiltodelta/homeassistant-sugartv-card) (HACS) funziona direttamente con questa integrazione. Seleziona il sensore della glicemia e la scheda rileverà automaticamente la tendenza e i minuti dall'ultimo aggiornamento:

```yaml
type: custom:sugartv-card
glucose_value: sensor.patient_glucose
```

![SugarTV Card in uno slot largo e uno verticale](https://raw.githubusercontent.com/wiltodelta/homeassistant-sugartv-card/v0.22.0/sugartv-card-layouts.png)

---

## Modifica delle soglie di allerta

Le soglie possono essere modificate in qualsiasi momento senza reinstallare:

1. In Home Assistant vai su **Impostazioni** → **Dispositivi e servizi**
2. Trova l'integrazione **EasyView** e clicca sull'icona ✏️ **Configura**
3. Modifica la **Soglia glicemia alta** e/o la **Soglia glicemia bassa**
4. Clicca **Invia**

Le soglie vengono visualizzate nell'unità scelta durante la configurazione (mg/dL o mmol/L).

---

## Riautenticazione

Se l'integrazione smette di funzionare a causa di una sessione scaduta:

1. In Home Assistant vai su **Impostazioni** → **Dispositivi e servizi**
2. Vedrai una notifica sull'integrazione EasyView — clicca **Riautentica**
3. Reinserisci le credenziali

Non è necessario rimuovere e reinstallare l'integrazione.

---

## Localizzazione

L'integrazione è completamente localizzata in:

- 🇬🇧 English
- 🇮🇹 Italiano
- 🇷🇺 Русский

I nomi dei sensori **e i valori di stato** (tendenza, stato) vengono tradotti automaticamente in base alla lingua di Home Assistant.

---

## Problemi comuni

| Problema | Soluzione |
|----------|-----------|
| Errore di autenticazione | Controlla email e password dell'account Follow |
| Nessun dato visibile | Verifica che l'invito sia stato accettato e i dati visibili nell'account Follow |
| Sensori non disponibili | Controlla la connessione internet e riavvia l'integrazione |
| Batteria mostra valore errato | Aggiorna all'ultima versione dell'integrazione |

---
---

## ❤️ Sostieni il progetto

Se trovi utile questa integrazione, puoi supportarne lo sviluppo:

[![Boosty](https://img.shields.io/badge/Boosty-Supporta-F15F2C?style=for-the-badge&logo=boosty&logoColor=white)](https://boosty.to/alex_khmelenko)

Ogni donazione aiuta a sviluppare l'integrazione più velocemente: nuove funzionalità, correzioni di bug e supporto per le pompe di insulina (in programma).

## Crediti

**Manutentore attuale:** [@outsiderz](https://github.com/outsiderz)

Questa integrazione era originariamente basata sul progetto [dmsmartech/easyview](https://github.com/dmsmartech/easyview) (Licenza MIT). Da allora è stata sostanzialmente riscritta:

- 🩸 Client API riscritto per gli account Medtrum EasyView Follow
- 🌍 Localizzazione completa (inglese, russo, italiano)
- 🎛️ Configurazione in due passaggi con soglie nelle unità scelte
- 📊 Nuovi sensori aggiunti (calibrazione, durata sensore)
- 🎨 Icone brand locali per Home Assistant 2026.3+
- 🔧 Correzioni di bug critici

**Licenza:** MIT — vedi [LICENSE.txt](LICENSE.txt)

L'uso dell'API si basa sul progetto open source [nl-ruud/nightscout-easyview](https://github.com/nl-ruud/nightscout-easyview).

Un ringraziamento speciale alla community di Home Assistant per i test e il feedback.