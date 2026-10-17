# Regole di disegno dell'albero BackHere

Documento di riferimento. Versione corrente: **v1.1 (2026-10-18)**. Copia immutabile della precedente: `regole-albero_v1.0.md`.
Codice di riferimento: `index.html` (ultimo commit del repo; layout = `313c4ac` + chiusura filtro `4b75750` + radici sotto segnaposto).
Regola di manutenzione: ogni modifica alle regole crea una nuova versione in fondo a questo file; la versione precedente resta nel registro e NON si cancella. Per tornare indietro: `git checkout <commit> -- index.html`.

## 1. Regole correnti (v1.1)

### Struttura
1. Sviluppo **verticale**, mai orizzontale: le generazioni scendono dall'alto al basso.
2. I **fratelli** stanno sulla stessa riga, affiancati e paralleli (ordine: `sibOrder` se presente, altrimenti anno di nascita).
3. Se i fratelli superano la larghezza massima (`WMAX` = 2200 px) vanno a capo su una nuova riga, con un corridoio laterale (`WRAP_MARGIN` = 18 px) per le linee.
4. Il **coniuge di un'altra famiglia** sta di lato, fuori dalla fratria, **unito** al partner da un segmento; i **figli** scendono in verticale dal punto di unione della coppia.
5. Un genitore **singolo** (padre non noto) genera i figli senza coniuge inventato; un coniuge senza cognome noto resta con solo il nome.
6. Più alberi si impilano in **fasce verticali** (`forestGap` = 80 px); un albero il cui figlio è già posizionato si unisce alla fascia di quel figlio, con la radice una riga sopra.

### Linee
7. Nessuna linea **verticale** si sovrappone a quella di un'altra famiglia (salvo l'origine della stessa coppia).
8. Le linee **si possono incrociare**: quando due linee non rappresentano lo stesso legame, quella orizzontale si interrompe per un breve tratto (GAP = 4 px) al punto di incrocio.
9. Nessuna linea attraversa un riquadro; nessun riquadro si sovrappone a un altro.
10. Le linee delle famiglie usano corsie (bus lane) distinte per famiglia; colore per famiglia.
11. Ogni connessione genitore→figlio parte dal punto della coppia, non dal singolo genitore.

### Riquadri
12. Misure: `nodeW` 150, `nodeH` 44, `coupleGap` 30, `sibGap` 16, `rowH` 130.
13. Pallino di stato: verde = confermato da più fonti, giallo = fonte unica/indizi, rosso = nessun riscontro.

### Filtro generazioni
14. Una generazione = 28 anni (`GEN_YEARS`), contata da oggi. Selettore: Tutte, 10 … 3. Apertura sul sito: **3 generazioni**, centrate su Marco Antonio.
15. Il filtro mostra anche, per ogni persona visibile: i **fratelli**, i **genitori**, i **fratelli dei genitori** (zii/prozii), il **genitore comune** di questi ultimi e i **coniugi**. Così nessun nome resta senza collegamento.
16. Le persone **senza alcun legame** compaiono solo con "Tutte".
17. Scegliere una persona fuori dalle generazioni selezionate porta il filtro a "Tutte".

### Radici e segnaposto (nuovo in v1.1)
- Una persona il cui unico genitore è un nodo-gruppo (segnaposto tipo "altri rami") conta come **radice**: non sparisce dal disegno.
- Una fratria con barra comune in foto ma genitore non leggibile riceve un **segnaposto** "Genitori di X (non identificati)", così i fratelli stanno su una riga con la linea.
- Le persone senza alcun legame restano nell'elenco "non ancora collegate" sotto l'albero.

### Rilettura delle foto (nuovo in v1.1)
- I legami si correggono solo dove **due letture indipendenti coincidono** o le **date** lo impongono (genitore nato dopo il figlio = errore). I casi con una sola lettura vanno in `docs/audit-foto-2026-10-18.md` per verifica manuale.

### Dati
18. Ogni legame ha `type` (`genitore` o `coniuge`), `from`, `to`, `sources`, `validation`. Un coniuge noto solo per nome ma non come persona NON resta come `toName`: si crea la persona.
19. Ogni dato riportato dall'utente è aggiunto con fonte `greg-AAAA-MM-GG`; i dati dubbi diventano domande aperte, non fatti.

### Controlli automatici obbligatori prima di pubblicare
Playwright su 3 generazioni e su "Tutte": 0 riquadri sovrapposti, 0 linee dentro i riquadri, 0 sovrapposizioni di linee verticali/orizzontali di famiglie diverse.

## 2. Registro delle versioni (non cancellare)

| Versione | Commit | Data | Cosa cambia | Stato |
|---|---|---|---|---|
| v0.1 | `92e0cca` | 2026-10-17 | Layout indentato: X = profondità, Y = anno di nascita; rapporto ~21:1 | superata |
| v0.2 | `c59bc8a` | 2026-10-17 | Alberi impilati in fasce verticali; figli sempre sotto i genitori | superata |
| v0.3 | `ee5dae4` | 2026-10-17 | Fratelli affiancati con ritorno a capo (larghezza max), corridoio laterale | superata |
| v0.4 | `8767b07` | 2026-10-17 | Rami Garaventa/Garbugino, linee a corsie; ultima versione senza filtro | base del v1.0 |
| v0.5 | `313c4ac` | 2026-10-17 | Selettore generazioni (Tutte, 10…3), apertura su 3 | base del v1.0 |
| v0.6 | `abca91c` | 2026-10-17 | Righe per data, colori per cognome, ingombro a contorno | **annullata** da Greg (orizzontale, linee lunghe); resta in git |
| — | `a6eecd1` | 2026-10-17 | Ripristino del layout 313c4ac | ripristino |
| v1.0 | `4b75750` / `6fe3d0a` | 2026-10-18 | Chiusura del filtro (fratelli, genitori, zii, coniugi); isolati solo in "Tutte" | superata (copia: regole-albero_v1.0.md) |
| **v1.1** | `e1d8cf3` | 2026-10-18 | Radici sotto nodi-gruppo, segnaposto per fratrie senza genitore, regola di rilettura foto; ~70 legami corretti dal pannello | **corrente** |

### Idee provate e scartate (non riproporre senza consenso)
- Righe per data valida per tutto l'albero: rompe lo sviluppo verticale compatto o produce larghezza ~11.500 px.
- Colori per cognome: provati in v0.6, annullati insieme al resto.

## 3. Come aggiornare queste regole
1. Copiare la sezione 1 in una nuova riga del registro (sezione 2) con il commit corrente.
2. Salvare la versione attuale come copia immutabile `regole-albero_vX.Y.md`, poi modificare la sezione 1 e aumentare la versione.
3. Salvare lo stesso file su Google Drive (cartella BackHere) e nel progetto.
