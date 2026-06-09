---
description: 'Prepare a commit message that follows the commit-convention format by verifying `/validate`, requiring an explicit JIRA key, and ensuring it passes the `commit-msg` hook to produce consistent, automatable commit history when committing changes in a JIRA-tracked project.'
---

Prépare un commit conforme au standard `commit-convention`.

1. **Pré-requis** — vérifier que `/validate` est passé vert. Sinon, refuser de
   committer et l'indiquer.
2. **Clé JIRA** — utiliser `$ARGUMENTS` si fournie. Si absente :
   **la demander** explicitement. Ne jamais inventer de clé.
3. **Rédaction** — produire le message au format :
   ```
   <type>(<scope>): <résumé impératif ≤72 car>  [<CLE-JIRA>]

   <description fonctionnelle : valeur métier / utilisateur + pourquoi>
   ```
   - `type` ∈ feat|fix|refactor|test|docs|chore|build|ci|perf|style
   - description fonctionnelle réelle (pas une paraphrase du diff technique).
4. **Vérification** — le message doit passer le hook `commit-msg`
   (`scripts/hooks/commit-msg`). Si le hook rejette, corriger et réessayer.

Afficher le message final proposé avant de committer.
