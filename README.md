# Git — Uitgebreide Referentie

## Mentaal model: hoe git denkt

Voor je commando's leert helpt het om te weten waar je wijzigingen "wonen". Git kent vier zones waar een bestand zich kan bevinden, en bijna elk commando verplaatst iets van de ene zone naar de andere.

- **Working directory** — je echte bestanden op schijf, waar je in typt
- **Staging area (index)** — het wachtkamertje voor wat in de volgende commit gaat. `git add` zet iets hierin
- **Local repository** — je commit-geschiedenis op je eigen machine. `git commit` zet de staging area hierin vast
- **Remote repository** — de geschiedenis op de server (GitHub/GitLab). `git push` stuurt je lokale commits hierheen

De hele basisworkflow is dus: wijzig in working directory → `add` naar staging → `commit` naar local → `push` naar remote. Snap je die vier zones, dan snap je het grootste deel van git.

**HEAD** is een aanwijzer naar "waar je nu bent" in de geschiedenis, meestal de laatste commit van je huidige branch. `HEAD~1` is één commit terug, `HEAD~2` twee terug, enzovoort. Veel "ongedaan maken"-commando's gebruiken deze notatie.

---

## Git configureren

| Commando | Uitleg |
|----------|--------|
| `git config --global -l` | Globale git configuratie weergeven |
| `git config --local -l` | Lokale folder git configuratie weergeven |
| `git config --global user.name <JOUW_NAAM>` | Instellen van je naam *(belangrijk voor verificatie)* |
| `git config --global user.email <JOUW@EMAIL.ADRES>` | Instellen van je email *(belangrijk voor verificatie)* |
| `git config --global core.editor <EDITOR>` | Standaard editor instellen (bv. `"code --wait"` of `nano`) |
| `git config --global alias.<NAAM> <COMMANDO>` | Een snelkoppeling maken (bv. `alias.st status`) |

Het verschil tussen `--global` en `--local`: global geldt voor alle repositories op je machine, local enkel voor de repository waar je in staat. Local overschrijft global. Er is ook `--system` (geldt voor alle gebruikers op de machine), maar dat heb je zelden nodig.

### Snelle configuratie

```bash
git config --global push.default simple
git config --global core.autocrlf true   # Windows only!
git config --global core.autocrlf input  # macOS/Linux only!
git config --global init.defaultBranch main
git config --global pull.rebase true     # Bespaart heel veel merge-issues!
git config --global rebase.autoStash true
git config --global core.ignorecase false
git config --global fetch.prune true      # Ruimt verwijderde remote branches automatisch op
```

Wat deze regels doen: `core.autocrlf` regelt hoe git omgaat met regeleindes (Windows gebruikt CRLF, macOS/Linux LF), wat conflicten voorkomt als een team gemengde systemen gebruikt. `pull.rebase true` zorgt dat een pull je werk netjes bovenop de binnengehaalde commits zet in plaats van een merge commit te maken, wat je geschiedenis lineair en leesbaar houdt.

### SSH key genereren

Om data te encrypteren en te ondertekenen genereer je een **ssh-keygen**.
Terug te vinden in `[je home folder]/.ssh`

```bash
ssh-keygen -t ed25519 -C "jouw@email.adres"   # moderne, aanbevolen keytype
ssh -T git@github.com                          # kijken of je gelinkt bent
cat ~/.ssh/id_ed25519.pub                       # publieke key tonen om te kopiëren
```

Na het genereren plak je de **publieke** key (`.pub`) in je GitHub/GitLab-instellingen onder SSH keys. Je private key (zonder `.pub`) deel je nooit met iemand. SSH is een alternatief voor HTTPS waarbij je niet telkens je wachtwoord of token moet ingeven.

> **Tip:** Verborgen bestanden weergeven op macOS: `Shift + Cmd + .`

---

## .gitignore

Niet alles hoort in version control. Build-output, dependencies (`node_modules`), geheime sleutels en lokale configuratiebestanden laat je buiten je repository met een `.gitignore`-bestand in de root.

```
node_modules/
dist/
.env
*.log
.DS_Store
```

| Commando | Uitleg |
|----------|--------|
| `git rm --cached <FILE>` | Een al getrackt bestand uit git halen maar lokaal behouden |
| `git rm -r --cached <MAP>` | Hetzelfde voor een hele map (bv. per ongeluk gecommit `node_modules`) |
| `git check-ignore -v <FILE>` | Nagaan welke regel in `.gitignore` een bestand negeert |

> **Let op:** `.gitignore` werkt enkel op bestanden die nog niet getrackt zijn. Stond een bestand al in een commit, dan moet je het eerst met `git rm --cached` verwijderen.

---

## Git commits

### Introductie

- Om veranderingen bij te houden, maken we gebruik van **commits**
- Commits bekijken: `git log` in je git-map
- Commits bevatten één of meerdere wijzigingen in onze repository
- Commits hebben altijd een bijhorende **commit message**
- Commits hebben altijd een **unieke commit hash** (een SHA-1 vingerafdruk van de inhoud)

### Best practices

- Elke commit heeft altijd een duidelijk doel
- **Schrijf duidelijke commit messages!**
- Gebruik de conventie: `type: beschrijving` (bv. `feat: add login page`, `fix: correct typo`)
- Houd commits klein en gefocust op één wijziging

### Conventional commits

Een veelgebruikte standaard voor commit messages, handig omdat tools er automatisch changelogs uit kunnen genereren:

| Type | Wanneer |
|------|---------|
| `feat:` | Nieuwe functionaliteit |
| `fix:` | Bugfix |
| `docs:` | Enkel documentatie |
| `style:` | Opmaak, geen logica-wijziging (spaties, komma's) |
| `refactor:` | Code herwerken zonder gedrag te wijzigen |
| `test:` | Tests toevoegen of aanpassen |
| `chore:` | Onderhoud (dependencies, config) |

### Commits aanpassen

| Commando | Uitleg |
|----------|--------|
| `git commit --amend` | Laatste commit aanpassen (message of vergeten bestanden toevoegen) |
| `git commit --amend --no-edit` | Vergeten wijziging toevoegen aan laatste commit zonder de message te wijzigen |
| `git commit -am "message"` | Stagen én committen in één stap *(enkel voor al getrackte bestanden)* |

> **Let op:** `--amend` herschrijft de laatste commit, dus gebruik het nooit op een commit die je al gepusht hebt naar een gedeelde branch.

---

## Basiscommando's — algemene workflow

### Absolute basisflow

| Commando | Uitleg |
|----------|--------|
| `git add .` | Alles toevoegen aan staging |
| `git commit -m "commit message"` | Wijzigingen committen met een duidelijke boodschap |
| `git push` | Lokale repository naar een remote repository pushen |

### Overige

| Commando | Uitleg |
|----------|--------|
| `git status` | Huidige toestand/situatie bekijken |
| `git status -s` | Compacte versie van status |
| `git log` | Historie/commits weergeven |
| `git log --oneline --graph` | Compacte geschiedenis met branch-visualisatie |
| `git log --oneline --graph --all` | Hetzelfde, maar voor alle branches tegelijk |
| `git init` | Van een lokale map een git repository maken |
| `git clone <URL>` | Een repository binnenhalen (klonen) |
| `git clone --depth 1 <URL>` | Enkel de laatste commit klonen *(sneller, "shallow clone")* |
| `git add <FILE>` | Wijzigingen in working directory aan staging toevoegen |
| `git add -p` | Per stuk (hunk) kiezen wat je staget *(handig voor nette commits)* |
| `git commit -m "commit message"` | Wijzigingen committen met een duidelijke boodschap |
| `git push` | Lokale repository naar een remote repository pushen |
| `git push -u origin <BRANCH>` | Pushen én de branch koppelen aan remote *(eerste keer)* |

### Verschillen bekijken

| Commando | Uitleg |
|----------|--------|
| `git diff` | Verschil tussen working directory en staging |
| `git diff --staged` | Verschil tussen staging en laatste commit |
| `git diff <BRANCH1> <BRANCH2>` | Verschil tussen twee branches |
| `git show <HASH>` | Wat een specifieke commit precies wijzigde |

### Remote repositories

| Commando | Uitleg |
|----------|--------|
| `git fetch` | Remote data ophalen *(verandert je working copy NIET)* |
| `git merge` | Lokale repository samenvoegen met working directory |
| `git pull` | Tegelijk fetchen en mergen |
| `git pull --rebase` | Fetchen en je werk bovenop de remote zetten *(nettere geschiedenis)* |

> **Let op:** Bij `git fetch`, altijd `git merge` doen wanneer je wijzigingen effectief wilt toepassen. `fetch` is veilig en kijkt enkel, `pull` past meteen toe.

### Bij fouten

| Commando | Uitleg |
|----------|--------|
| `git reset` | Staged changes terugzetten naar working directory |
| `git reset HEAD~1` | Laatste commit ongedaan maken *(wijzigingen blijven behouden)* |
| `git reset --soft HEAD~1` | Laatste commit ongedaan maken maar wijzigingen in staging houden |
| `git reset --hard HEAD~1` | Laatste commit én wijzigingen permanent verwijderen ⚠️ |
| `git revert <HASH>` | Veilig een commit ongedaan maken via een nieuwe commit |
| `git restore <FILE>` | Wijzigingen in een bestand ongedaan maken *(moderne manier)* |
| `git restore --staged <FILE>` | Een bestand uit staging halen zonder de wijziging te verliezen |

Het verschil tussen `reset` en `revert`: `reset` verplaatst je terug in de tijd en verwijdert commits uit de geschiedenis, wat gevaarlijk is op gedeelde branches. `revert` laat de geschiedenis staan en voegt een nieuwe commit toe die het effect van een oude commit ongedaan maakt, wat veilig is voor gedeelde code.

De drie soorten reset samengevat: `--soft` houdt je wijzigingen in staging, `--mixed` (de standaard) houdt ze in je working directory, `--hard` gooit ze volledig weg.

### Remote beheren

| Commando | Uitleg |
|----------|--------|
| `git remote` | Checken of je repository gelinkt is aan een remote |
| `git remote -v` | Meer info weergeven (URL, push/fetch) |
| `git remote add origin <URL>` | Remote toevoegen aan lokale repository |
| `git remote set-url origin <URL>` | URL van een bestaande remote wijzigen |
| `git remote remove <NAAM>` | Een remote verwijderen |

---

## Git branches

Branches laten je toe om **parallel te werken** zonder de hoofdcode te verstoren.
De standaard branch heet doorgaans `main` (of vroeger `master`). Een branch is technisch gezien niets meer dan een verplaatsbare aanwijzer naar een commit, daarom is het aanmaken ervan zo goedkoop en snel.

### Branch commando's

| Commando | Uitleg |
|----------|--------|
| `git branch` | Alle lokale branches weergeven |
| `git branch -a` | Alle branches (lokaal + remote) weergeven |
| `git branch -vv` | Lokale branches + gekoppelde remote status |
| `git branch <NAAM>` | Een nieuwe branch aanmaken |
| `git switch <NAAM>` | Naar een bestaande branch gaan |
| `git switch -c <NAAM>` | Nieuwe branch aanmaken én er meteen naartoe gaan |
| `git checkout <NAAM>` | Naar een branch gaan *(oudere manier)* |
| `git checkout -b <NAAM>` | Nieuwe branch aanmaken én er naartoe gaan *(oudere manier)* |
| `git branch -m <OUD> <NIEUW>` | Een branch hernoemen |
| `git branch -d <NAAM>` | Branch verwijderen *(enkel als al gemerged)* |
| `git branch -D <NAAM>` | Branch verwijderen *(forceer, ook zonder merge)* ⚠️ |
| `git push origin <NAAM>` | Branch pushen naar remote |
| `git push origin --delete <NAAM>` | Remote branch verwijderen |

> `switch` en `restore` zijn de modernere, duidelijkere opvolgers van `checkout`, dat vroeger te veel verschillende dingen deed (van branch wisselen én bestanden herstellen). Nieuwere git-versies raden ze aan.

### Branch strategie — best practices

- **`main`** — stabiele, productie-klare code
- **`develop`** — integratiebranch voor nieuwe features
- **`feature/naam`** — nieuwe functionaliteit uitwerken
- **`fix/naam`** — bugfixes uitwerken
- **`hotfix/naam`** — urgente fixes rechtstreeks op main

> Werk **nooit rechtstreeks** op `main`. Maak altijd een feature-branch aan.

### Mergen

| Commando | Uitleg |
|----------|--------|
| `git merge <BRANCH>` | Een branch samenvoegen met de huidige branch |
| `git merge --no-ff <BRANCH>` | Merge commit forceren *(zelfs bij fast-forward)* |
| `git merge --squash <BRANCH>` | Alle commits van een branch samenvoegen tot één wijziging |
| `git merge --abort` | Merge afbreken bij een conflict |

Een **fast-forward** merge gebeurt als de huidige branch geen eigen nieuwe commits heeft: git schuift dan gewoon de aanwijzer vooruit zonder merge commit. Met `--no-ff` forceer je toch een merge commit, wat handig is om in de geschiedenis te zien dat er een feature-branch bestaan heeft.

### Rebasen

| Commando | Uitleg |
|----------|--------|
| `git rebase <BRANCH>` | Jouw commits bovenop een andere branch plaatsen |
| `git rebase --continue` | Rebase verderzetten na het oplossen van een conflict |
| `git rebase --abort` | Rebase volledig afbreken |
| `git rebase -i HEAD~<N>` | Interactief de laatste N commits herwerken *(squash, rename...)* |

Merge versus rebase, kort: **merge** behoudt de exacte geschiedenis en voegt een merge commit toe (de geschiedenis vertakt en komt weer samen). **Rebase** herschrijft je commits alsof je vanaf het nieuwste punt was begonnen, wat een rechte, lineaire geschiedenis oplevert maar de oorspronkelijke commit-hashes verandert.

Interactieve rebase (`-i`) is krachtig: je kan commits samenvoegen (squash), hernoemen (reword), van volgorde wisselen of weggooien, voor je ze pusht. Ideaal om een rommelige reeks lokale commits op te kuisen tot een nette set.

> **Gouden regel:** Rebase **nooit** op een gedeelde/remote branch waarop anderen werken!

### Merge conflicts

- Git is slim, maar de git history moet logisch zijn!
- Conflicten ontstaan wanneer twee branches **dezelfde regels** anders aanpassen
- Git markeert het conflict in het bestand met `<<<<<<<`, `=======`, `>>>>>>>`

Hoe je de markeringen leest: alles tussen `<<<<<<<` en `=======` is jouw versie (de huidige branch), alles tussen `=======` en `>>>>>>>` is de binnenkomende versie. Je verwijdert de markeringen en houdt de juiste combinatie over.

**Stappenplan bij een conflict:**
1. `git status` — bekijk welke bestanden conflicten bevatten
2. Open de bestanden en los de conflicten manueel op
3. `git add <FILE>` — markeer als opgelost
4. `git commit` — rond de merge commit af

> **Tip:** `git pull --rebase` gebruiken bij een pull lost de meeste merge-conflicten preventief op!

---

## Git forks

- Een **fork** is een persoonlijke kopie van iemand anders zijn repository op GitHub/GitLab
- Handig om bij te dragen aan open-source projecten zonder directe toegang
- Workflow: **fork → clone → branch → push → pull request**

| Stap | Uitleg |
|------|--------|
| Fork aanmaken | Via de GitHub/GitLab UI op de originele repo |
| `git clone <JOUW_FORK_URL>` | Fork lokaal binnenhalen |
| `git remote add upstream <ORIGINEEL_URL>` | Originele repo als upstream toevoegen |
| `git fetch upstream` | Updates van originele repo ophalen |
| `git merge upstream/main` | Updates samenvoegen met jouw fork |

> Na het pushen naar je fork, maak je een **Pull Request (PR)** aan om je wijzigingen voor te stellen aan de originele repo.

Het verschil tussen `origin` en `upstream`: `origin` is jouw fork (waar je naartoe pusht), `upstream` is de originele repo (waar je updates van ophaalt). Je houdt je fork up-to-date door regelmatig van upstream te fetchen en te mergen.

---

## Tags & releases

Tags markeren een specifiek punt in de geschiedenis, meestal een versie. In tegenstelling tot branches bewegen ze niet mee.

| Commando | Uitleg |
|----------|--------|
| `git tag` | Alle tags weergeven |
| `git tag <NAAM>` | Een lichte tag maken op de huidige commit |
| `git tag -a v1.0.0 -m "message"` | Een geannoteerde tag maken *(met auteur en datum)* |
| `git tag -a v1.0.0 <HASH>` | Een oude commit achteraf taggen |
| `git push origin <TAG>` | Eén tag naar remote pushen |
| `git push origin --tags` | Alle tags naar remote pushen |

Geannoteerde tags (`-a`) zijn aan te raden voor releases omdat ze auteur, datum en een bericht opslaan. Lichte tags zijn enkel een naam op een commit. Veel CI/CD-pipelines starten een productie-deploy wanneer een tag in de vorm `vX.Y.Z` gepusht wordt.

---

## Stash — werk tijdelijk wegzetten

Soms moet je snel van branch wisselen maar heb je niet-afgewerkte wijzigingen. Met stash zet je die tijdelijk opzij zonder ze te committen.

| Commando | Uitleg |
|----------|--------|
| `git stash` | Niet-gecommitte wijzigingen opzij zetten |
| `git stash -u` | Idem, inclusief niet-getrackte bestanden |
| `git stash list` | Alle stashes weergeven |
| `git stash pop` | Laatste stash terugzetten en uit de lijst halen |
| `git stash apply` | Stash terugzetten maar in de lijst behouden |
| `git stash drop` | Een stash verwijderen |
| `git stash clear` | Alle stashes verwijderen |

---

## Geavanceerd: gericht ingrijpen

| Commando | Uitleg |
|----------|--------|
| `git cherry-pick <HASH>` | Eén specifieke commit van een andere branch overnemen |
| `git bisect start` | Binair zoeken naar de commit die een bug introduceerde |
| `git blame <FILE>` | Per regel tonen wie wat wanneer wijzigde |
| `git reflog` | Geschiedenis van HEAD-wijzigingen, handig om verloren commits terug te vinden |

`git reflog` is je vangnet: zelfs na een `reset --hard` of een mislukte rebase staan de oude commits hier vaak nog een tijd in, zodat je ze kan terughalen. `git bisect` is goud waard bij het opsporen van wanneer een bug binnensloop: je markeert een goede en een slechte commit, en git laat je via opsplitsing snel de schuldige commit vinden.

---

## Opkuisen & onderhoud

| Stap | Uitleg |
|------|--------|
| `git fetch --prune` | Synchroniseert met remote en verwijdert lokale references naar verwijderde remote branches |
| `git fetch -p` | Korte versie van `git fetch --prune` |
| `git branch -vv \| grep ': gone]'` | Filtert branches waarvan de remote branch niet meer bestaat |
| `git branch -vv \| grep ': gone]' \| awk '{print $1}' \| xargs git branch -d` | Verwijdert lokaal alle branches waarvan remote branch verwijderd is |
| `git branch -vv \| grep ': gone]' \| awk '{print $1}' \| xargs git branch -D` | Force delete van lokale branches waarvan remote branch verwijderd is |
| `git remote prune origin` | Verwijdert verouderde remote tracking branches zonder fetch |
| `git config --global fetch.prune true` | Zet automatisch prunen aan bij elke `git fetch` of `git pull` |
| `git pull --rebase` | Haalt wijzigingen binnen zonder extra merge commit |
| `git stash` | Slaat tijdelijke niet-gecommitete wijzigingen op |
| `git stash pop` | Zet laatste stash terug en verwijdert deze uit stash lijst |
| `git clean -fd` | Verwijdert ongevolgde bestanden en mappen ⚠️ |
| `git clean -fdn` | Toont eerst wat `clean` zou verwijderen *(droogtest)* |
| `git reset --hard HEAD` | Zet working directory volledig terug naar laatste commit ⚠️ |
| `git gc` | Ruimt de repository op en comprimeert de geschiedenis |

> **Tip:** Doe bij `git clean` altijd eerst de droogtest met `-fdn` voor je echt verwijdert. Eens weg met `clean`, dan staat het ook niet in git en is het echt weg.
