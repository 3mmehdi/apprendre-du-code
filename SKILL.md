---
name: apprendre-du-code
description: Transforme un chantier de code qui vient d'être fini en compétence durable pour Mehdi. Interroge d'abord sans laisser relire le diff, classe les réponses ✅/⚠️/❌, explique ensuite (y compris ce qui était juste), range la leçon dans une page de thème du coffre Obsidian et génère des cartes Anki uniquement sur ce qui a coincé. Utiliser quand Mehdi dit "qu'est-ce qu'on vient de faire", "apprends-moi ce qu'on a fait", "/apprendre-du-code", "explique-moi ce que tu as fait", "leçon de code", "j'ai compris ou pas ?", ou à la fin d'un chantier de code quand il demande à en tirer quelque chose. NE PAS utiliser pour expliquer du code sans rien écrire (répondre directement), pour une session formation à partir de notes Word (→ le circuit "session formation" de wiki/cours/ia/Workflow d'apprentissage — Parcours IA.md), ni pour relire du code à la recherche de bugs (→ /code-review).
---

# Apprendre du code — de ce qu'on vient de faire à une compétence

Objectif de Mehdi, dans ses mots : **« gagner en compétence à chaque prompt »**. Cette skill ferme la
boucle en fin de chantier ; la boucle de chaque modification est tenue par la consigne `↳ pourquoi :`
de `~/.claude/CLAUDE.md`.

> **Le principe, repris du circuit formation du coffre :**
> *« On retient ce qu'on va rechercher dans sa mémoire, pas ce qu'on relit. »*
> C'est pour ça que cette skill **interroge avant d'expliquer**. Un résumé bien écrit qu'on lit en
> hochant la tête ne produit rien dans trois mois.

## Ce que cette skill fait, et ne fait pas

**Elle fait** : interroger → classer → expliquer → ranger dans le coffre → générer des cartes Anki.

**Elle ne fait pas** :
- Chercher des bugs : c'est `/code-review`
- Expliquer du code à la volée sans rien écrire : répondre directement, sans la skill
- Traiter une session de formation à partir de notes Word : c'est le circuit **« session formation »**
  de [[wiki/cours/ia/Workflow d'apprentissage — Parcours IA]]
- **Inventer une leçon quand il n'y en a pas** — voir l'étape 0

## Mode entraînement — quand il n'y a pas de chantier derrière

Demandé par Mehdi le 2026-10-13 : *« tu me poses des problèmes, je réponds et on regarde si j'ai bon,
et on en sort des fiches »*, sur son code **et** sur des exercices écrits pour l'occasion.

Déclencheurs : « entraîne-moi sur Python », « pose-moi des problèmes », « exercice du jour ».

**Ce qui change** : l'étape 0 ne s'applique pas (aucun diff à juger) et l'étape 1 est remplacée par le
choix de la matière, dans cet ordre de préférence :

| Source | Quand la préférer |
|---|---|
| **Son propre code de production** (scripts, agents planifiés) | Par défaut. Il l'a écrit, les conséquences sont réelles, et c'est le format d'une épreuve technique courante : relire du Python, sans IA |
| **Un bug déjà vécu**, retrouvé dans `wiki/cours/dev/` ou `wiki/log.md` | Pour vérifier qu'une leçon ancienne tient encore. Si elle ne tient pas, c'est le signal de l'étape 7 |
| **Un exercice écrit sur mesure** | Quand aucun code à lui ne couvre la notion visée. Écrire du code **plausible**, pas un exercice d'école : une fonction qu'une vraie veille ou un vrai script d'ingestion contiendrait |

⛔ **Jamais les Problem Sets de CS50P** : le cours interdit l'IA dessus (seul le CS50 Duck est
autorisé), et le certificat saute. Les deux pistes ne se
croisent pas : CS50P apprend à **écrire** du Python, ce mode entraîne à en **relire**.

**Tout le reste est inchangé** : une question à la fois (étape 2), classement sans complaisance
(étape 3), explication même sur un ✅ (étape 4), rangement par thème (étape 5), cartes sur les ⚠️ et
❌ uniquement (étape 6), et la règle de la troisième fois (étape 7).

⚠️ **Le piège propre à ce mode** : sans diff pour borner le sujet, on dérive vers le quiz de
définitions. La question doit toujours porter sur **ce qui casse** et **pourquoi**, jamais sur ce
qu'une syntaxe veut dire.

## Étape 0 — Est-ce qu'il y a seulement quelque chose à apprendre ?

⏭ **Sautée en mode entraînement.** Avant tout, regarder ce qui a bougé. **S'il n'y a pas eu de choix technique**, s'arrêter et le dire :

> « Rien à en tirer cette fois : trois renommages et une chaîne corrigée, aucun choix technique.
> Je ne fabrique pas une leçon là où il n'y en a pas. »

⚠️ **C'est l'étape la plus importante de la skill.** Une leçon inventée sur un changement trivial
apprend une fausse règle, et elle produit une carte Anki qu'on révisera pendant des mois. Le coût
d'une leçon vide n'est pas nul : il est négatif.

Seuil indicatif : au moins un choix d'approche, un arbitrage, un piège évité, ou une correction dont
la cause était non évidente.

## Étape 1 — Rassembler la matière

Dans cet ordre, sans rien demander à Mehdi :

| Source | Ce qu'on en tire |
|---|---|
| Les lignes `↳ pourquoi :` de la session | **La matière première.** Les choix déjà verbalisés sur le moment |
| `git diff` (non commité) et `git log -p` des commits du jour | Ce qui a réellement changé |
| Les erreurs rencontrées en cours de route | Souvent la meilleure leçon : la cause, pas le symptôme |

Si le dépôt n'a pas de git, travailler sur ce qui s'est passé dans la session.

## Étape 2 — Interroger, **une question à la fois**

**2 à 3 questions maximum.** Une seule par message : posées toutes ensemble, Mehdi les survole.

⛔ **Ne pas lui laisser relire le diff avant de répondre.** Si le code est encore à l'écran, le dire :
« sans regarder le fichier ».

**Ce qui fait une bonne question :**

| ✅ Bonne | ❌ Mauvaise | Pourquoi |
|---|---|---|
| « Pourquoi on écrit dans un fichier temporaire avant de remplacer, plutôt que d'écrire directement ? » | « Est-ce que tu as compris le `os.replace` ? » | La seconde se répond par oui |
| « Qu'est-ce qui casse si on retire le `\b` de la regex ? » | « À quoi sert `\b` dans une regex ? » | La seconde est une définition, on peut la réciter sans comprendre |
| « Un script appelant lit la sortie de cette commande. Qu'est-ce qu'il conclut si elle sort en 0 avec rien ? » | « Quel code de sortie on a mis ? » | La seconde teste la mémoire du diff, pas la compréhension |

**Règles** :
- Jamais de question fermée (oui/non)
- Jamais la réponse contenue dans la question
- Porter sur **les raisons et les conséquences**, pas sur les faits du code
- Formuler comme **une question qu'un recruteur pourrait poser** — c'est la même exigence que les cartes
- Mehdi peut répondre à l'oral (Wispr Flow) : accepter une réponse parlée, imprécise dans la forme

## Étape 3 — Classer, sans complaisance

| | Sens |
|---|---|
| ✅ | Juste, et pour la bonne raison |
| ⚠️ | Idée correcte, mécanisme flou, ou juste par accident |
| ❌ | Faux, ou « je ne sais pas » |

⚠️ **Une bonne réponse pour une mauvaise raison est un ⚠️, pas un ✅.** C'est exactement le cas qui
se retourne contre lui en entretien. Le dire franchement, ce n'est pas un examen à faire réussir.

## Étape 4 — Expliquer, **y compris ce qui était juste**

Décision de Mehdi (29/09) : l'explication est écrite **quelle que soit** la réponse, parce qu'il peut
oublier plus tard. Le classement décide des cartes, pas de l'explication.

Pour chaque point, trois blocs courts :

1. **La leçon** — la règle générale, en une ou deux phrases, formulée pour être vraie ailleurs que dans ce fichier
2. **L'exemple** — le bout de code réel du chantier, avec l'alternative écartée
3. **L'exercice** — une situation légèrement différente où la règle s'applique, sans donner la réponse

## Étape 5 — Ranger dans le coffre

**Une page par thème, jamais une page par session.** Un fichier par session remplit le coffre de
petites notes et rend le sujet introuvable ; c'est la règle déjà posée dans le circuit formation.

```
$VAULT/wiki/cours/dev/<thème>.md
```

Thèmes : `bash`, `python`, `git`, `sécurité`, `astro`, `sql`, `regex`, `api`… Créer le fichier s'il
n'existe pas, avec le frontmatter du coffre (`type: cours`, `created`, `updated`, `tags`) et le bloc
`TARGET DECK` en tête. Sinon, **ajouter une section** à la page existante :

```markdown
## <titre de la leçon> — AAAA-MM-JJ

**Contexte** : <projet, une ligne>
**Ce qui s'est passé** : <le choix ou le piège, deux lignes>

### La leçon
### L'exemple
### L'exercice
```

Si la leçon relève de plusieurs thèmes, choisir **celui où Mehdi la chercherait**, et poser un
wikilink depuis l'autre. Jamais deux copies.

`$VAULT` = le chemin absolu de votre coffre — à utiliser en chemin absolu, la skill
tourne aussi depuis les projets clients.

## Étape 6 — Les cartes Anki, **seulement sur les ⚠️ et ❌**

Une carte par point mal maîtrisé. **Zéro carte si tout était ✅** — réviser ce qu'on sait déjà est
du temps volé aux révisions utiles.

Format exact (plomberie existante, à ne pas réinventer) :

```
TARGET DECK
Formation IA::Shell

START
Basic
Front: Un script de supervision lit la sortie d'une commande qui sort en 0 avec une liste vide. Que conclut-il, et pourquoi est-ce dangereux ?
Back: Il conclut « il n'y a rien », alors que la vraie réponse était « je n'ai pas pu répondre ». Un « je ne sais pas » écrit comme un « il n'y en a pas » produit une fausse certitude, et personne ne vient la corriger.
Tags: code bash codes-sortie
END
```

- `START` / `END` **sans crochets**. Avec crochets, le plugin ignore la carte en silence :
  **77 cartes perdues** avant le correctif du 2026-07-06.
- `TARGET DECK` en tête de page (**sans crochets**), et **jamais de `<…>` dans une carte** : Anki les lit comme des balises HTML et les efface. Après le scan, vérifier que chaque carte a reçu sa ligne `<!--ID: …-->` ; sinon elle n'est pas dans Anki (le plugin ne réessaie jamais une page déjà enregistrée). ⛔ **Fins de ligne `\n` obligatoires** : le plugin ne reconnaît aucune carte dans une page en `\r\n`. Sous Windows, écrire par script avec `open(p, 'w', encoding='utf-8', newline='')`, jamais un `open(p, 'w')` nu ; ou passer par l'outil Edit.
- Paquet : **`Formation IA::Python`** pour une leçon tirée de code Python, **`Formation IA::Shell`** pour une leçon tirée de bash / shell (règle de Mehdi, 2026-10-03 : le paquet suit le **langage** de la leçon). `TARGET DECK` vaut pour toute la page : une page de thème prend le langage de ses cartes ; si un thème mélange les deux, séparer en deux pages (`<thème> (Python)`, `<thème> (shell)`) plutôt que de mal ranger des cartes. ⚠️ Sans `TARGET DECK`, les cartes tombent dans le paquet par défaut du plugin (`Anglais 2026::Sessions`).
- ⛔ **Jamais en carte** : prix, numéros de version, noms de modèles, dates. On garde les **concepts**
  et les **raisons** — le reste périme et fait réviser du faux.
- **Une carte = une idée**, formulée comme une question qu'un recruteur pourrait poser.
- Rappeler à Mehdi de lancer `Obsidian_to_Anki: Scan vault` pour que les cartes existent vraiment.

## Étape 7 — La troisième fois, ce n'est plus une leçon

Avant de conclure, chercher si le même piège a déjà une section dans la page du thème.

**S'il apparaît pour la troisième fois**, le dire et proposer de le sortir de l'apprentissage :

> « C'est la troisième fois que ce piège revient. Selon ta doctrine, il ne relève plus d'une leçon
> mais d'un garde-fou. Je te propose un hook / une règle plutôt qu'une carte de plus. »

C'est la doctrine du coffre appliquée à Mehdi lui-même : *si la même correction revient une troisième
fois, elle ne relève plus d'une consigne mais d'un hook*. Une carte qu'on rate trois fois ne sera pas
mieux apprise la quatrième.

## Ce qui fait échouer cette skill

| Piège | Ce qui se passe | La parade |
|---|---|---|
| Fabriquer une leçon sur un changement trivial | Fausse règle apprise, carte révisée des mois | Étape 0, s'arrêter et le dire |
| Poser les 3 questions d'un coup | Mehdi survole et répond en surface | Une question par message |
| Valider une réponse juste pour une mauvaise raison | Le trou reste, et il ressort en entretien | ⚠️, pas ✅ |
| Créer une page par session | Le coffre se remplit, le thème devient introuvable | Une page par thème, une section par leçon |
| Mettre une version ou un prix en carte | On révise du périmé | Concepts et raisons uniquement |
| Écrire la carte avec `[START]` | Le plugin l'ignore sans rien dire | Sans crochets, toujours |
