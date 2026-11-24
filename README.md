# apprendre-du-code

**Une skill Claude Code qui transforme un chantier de code terminé en compétence.** Elle interroge
avant d'expliquer, et ne fabrique des cartes de révision que sur ce qui a coincé.

> *A Claude Code skill that turns a finished piece of work into competence. It quizzes you before
> explaining, files the lesson into an Obsidian vault, and only generates spaced-repetition cards
> for what you got wrong. Written in French.*

---

## Le problème

Coder avec un agent va vite, et c'est exactement le risque : on accepte du code qu'on n'a pas compris,
et six mois plus tard on ne sait pas défendre ses propres choix. Un résumé de fin de session ne règle
rien, parce que **relire n'est pas apprendre**.

> On retient ce qu'on va rechercher dans sa mémoire, pas ce qu'on relit.

D'où le parti pris central : **la skill interroge d'abord**, sans laisser rouvrir le diff.

## Les deux étages

**En continu** — une consigne permanente (voir `CLAUDE.md.exemple`) : à chaque modification où un
**choix** a été fait, l'agent ajoute une ligne, et une seule.

```
↳ pourquoi : \b dans la regex, sans quoi « rag » matcherait « leverage »
```

Elle dit *pourquoi*, jamais *ce que fait* le code : ça se lit dans le diff. ⛔ Rien sur un renommage,
une typo, un import. Si tout est commenté, plus rien n'est lu.

**En fin de chantier** — la skill ramasse ces lignes, plutôt que de tout re-déduire du diff :

1. **Étape 0** — décider s'il y a seulement quelque chose à apprendre, et **s'arrêter sinon**
2. **Interroger** — 2 à 3 questions, une par message, sans rouvrir le fichier
3. **Classer** ✅ / ⚠️ / ❌
4. **Expliquer**, y compris ce qui était juste
5. **Ranger** une section dans la page du thème
6. **Générer les cartes** uniquement sur les ⚠️ et ❌
7. **Détecter la troisième récidive** et proposer un garde-fou à la place d'une carte de plus

## Les quatre décisions qui font la différence

**L'étape 0 peut refuser de produire quoi que ce soit.** Si le chantier n'a contenu aucun choix
technique, la skill le dit et s'arrête. Une leçon inventée apprend une fausse règle et crée une carte
qu'on révisera pendant des mois : son coût n'est pas nul, il est négatif.

**Les questions sont contraintes par un tableau d'exemples**, sinon elles dérivent vers du fermé :

| ✅ | ❌ | Pourquoi |
|---|---|---|
| « Qu'est-ce qui casse si on retire le `\b` ? » | « À quoi sert `\b` ? » | La seconde est une définition, on la récite sans comprendre |
| « Un script lit cette sortie. Qu'en conclut-il ? » | « Quel code de sortie on a mis ? » | La seconde teste la mémoire du diff |

**Une bonne réponse pour une mauvaise raison vaut ⚠️, pas ✅.** C'est le cas qui se retourne contre
soi en entretien.

**L'explication est écrite même quand la réponse était juste** ; seule la génération de cartes dépend
du classement. Réviser ce qu'on maîtrise vole du temps aux révisions utiles.

## Ce que le premier passage réel a produit

La skill a été éprouvée sur un vrai bug : un filtre qui cherchait ses mots-clés **en sous-chaîne**,
au point de faire remonter une offre de réacteurs nucléaires dans une veille sur l'IA
(`"rag"` est contenu dans `"leverage"`). Correction en bornes de mot : **73 résultats retenus → 43**.

Et elle a sorti quelque chose que je n'avais pas vu : sur trois questions, **deux fois mon réflexe a
été d'ajouter un garde-fou ou un message d'erreur** — alors qu'aucun des deux bugs ne produisait
d'erreur. Le code faisait exactement ce qu'on lui avait demandé, et ce qu'on lui avait demandé était
faux. C'est précisément le genre d'angle mort qu'un résumé de session ne montre jamais.

## Installation

```bash
cp -r apprendre-du-code ~/.claude/skills/
cat CLAUDE.md.exemple >> ~/.claude/CLAUDE.md
```

Puis adapter `$VAULT` dans `SKILL.md`. La sortie suppose un coffre Obsidian et le plugin
**Obsidian_to_Anki** (cartes `START` / `END`, sans crochets).

> ⚠️ **Le piège de la plomberie Anki**, appris à mes dépens : le plugin a un réglage
> `Scan Directory`. Tout ce qui vit en dehors n'est **jamais lu**, et il ne le signale pas. Des cartes
> parfaitement valides n'existent tout simplement pas. Vérifier ce réglage avant de chercher ailleurs.
>
> Et dans un fichier de documentation, écrire les blocs d'exemple avec des crochets — `[START]` —
> sinon le scanner en fait de vraies cartes.

## Voisin

[claude-code-guardrails](https://github.com/3mmehdi/claude-code-guardrails) — les hooks qui empêchent
l'agent de dériver pendant qu'il travaille. Celui-ci s'occupe de ce qu'on en retient après.

## Licence

MIT. Voir [LICENSE](LICENSE).

## Mises à jour

- **24/11/2026** : mode entraînement (exercices sur son propre code quand il n'y a pas de chantier derrière). Règles Anki précisées : paquet selon le langage de la leçon, fins de ligne `
` obligatoires, vérification des identifiants après le scan.
