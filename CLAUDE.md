# CLAUDE.md — Istruzioni per agenti Claude Code

Questo file configura il comportamento di Claude Code in questo progetto.
Viene letto automaticamente ad ogni sessione.

---

## Identità e contesto

- **Utente:** Matteo De Petris (`matteo.depetris61@gmail.com`)
- **Ambiente:** Claude Code sul web (esecuzione remota, container effimero)
- **Repository scope:** limitato al repo corrente

---

## Regole generali

1. **Lingua:** rispondi sempre in italiano, salvo che il codice o i commenti tecnici richiedano l'inglese.
2. **Commit e push:** sviluppa sempre su branch dedicato (`claude/<nome-branch>`), mai direttamente su `main` o `master` senza permesso esplicito.
3. **Non creare PR** a meno che l'utente non lo chieda esplicitamente.
4. **Non modificare** file di configurazione sensibili (secrets, `.env`, credenziali) senza conferma esplicita.
5. **Commenta il codice** solo quando il "perché" non è ovvio — niente docstring ridondanti.
6. **Non aggiungere funzionalità extra** non richieste: niente refactoring preventivo, niente astrazioni inutili.

---

## Workflow Git

```bash
# Branch di sviluppo
git checkout -b claude/<feature-name>

# Push
git push -u origin claude/<feature-name>

# In caso di errore di rete: retry con backoff (2s, 4s, 8s, 16s)
```

### Messaggio di commit standard

```
<descrizione concisa in inglese>

Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_<id>
```

---

## Integrazione GitHub

Usa **esclusivamente i tool MCP GitHub** (`mcp__github__*`) per:
- Leggere PR e issue
- Creare branch
- Pushare file
- Commentare PR
- Verificare CI

**Non usare** `gh` CLI — non è disponibile nell'ambiente remoto.

### Sottoscrizione eventi PR

Per monitorare una PR usa `subscribe_pr_activity`. Gli eventi arrivano come
`<github-webhook-activity>` e svegliano la sessione automaticamente.
Non usare `sleep` o polling attivo.

---

## Ambiente remoto — limitazioni

| Cosa | Disponibile |
|---|---|
| Lettura/scrittura file | ✅ |
| Bash, comandi shell | ✅ |
| Git (fetch/commit/push) | ✅ |
| MCP GitHub | ✅ |
| Browser / UI visiva | ❌ |
| `gh` CLI | ❌ |
| Sessione persistente | ❌ (committare prima di chiudere) |

> Il container è **effimero**: tutto ciò che non viene committato e pushato viene perso.

---

## Checklist prima di chiudere una sessione

- [ ] Tutte le modifiche sono state committate
- [ ] Il branch è stato pushato (`git push -u origin <branch>`)
- [ ] Se richiesta, la PR è stata creata
- [ ] Nessun file sensibile (`.env`, secrets) è stato incluso nel commit

---

## Istruzioni specifiche per questo progetto

> Aggiungi qui le istruzioni specifiche del progetto, ad esempio:
> - stack tecnologico usato
> - comandi per avviare il dev server (`npm run dev`, ecc.)
> - convenzioni di naming
> - struttura delle cartelle
> - test runner (`npm test`, `pytest`, ecc.)

---

## Come usare questo file in altri progetti

Copia questo file nella root del progetto come `CLAUDE.md`.
Claude Code lo legge automaticamente all'avvio di ogni sessione.
Personalizza la sezione "Istruzioni specifiche per questo progetto" per ogni repo.
