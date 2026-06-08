## Git Configureren

| Commando | Uitleg |
|----------|--------|
| `git config --global -l` | Globale git configuratie weergeven |
| `git config --local -l` | Lokale folder git configuratie weergeven |
| `git config --global user.name <JOUW_NAAM>` | Instellen van je naam *(belangrijk voor verificatie)* |
| `git config --global user.email <JOUW@EMAIL.ADRES>` | Instellen van je email *(belangrijk voor verificatie)* |

<br/>

### Snelle configuratie

```bash
git config --global push.default simple
git config --global core.autocrlf true   # Windows only!
git config --global core.autocrlf input  # macOS/Linux only!
git config --global init.defaultBranch main
git config --global pull.rebase true     # Bespaart heel veel merge-issues!
git config --global rebase.autoStash true
git config --global core.ignorecase false
```

<br/>

### SSH Key genereren

Om data te encrypteren en te handtekenen genereer je een **ssh-keygen**.  
Terug te vinden in `[je home folder]/.ssh`

```bash
ssh-keygen              # key-value pair genereren
ssh -T git@github.com   # kijken of je gelinkt bent
```

> **Tip:** Verborgen bestanden weergeven op macOS: `Shift + Cmd + .`

---

## Git Commits

### Introductie

- Om veranderingen bij te houden, maken we gebruik van **commits**
- Commits bekijken: `git log` in je git-map
- Commits bevatten één of meerdere wijzigingen in onze repository
- Commits hebben altijd een bijhorende **commit message**
- Commits hebben altijd een **unieke commit hash**

### Best Practices

- Elke commit heeft altijd een duidelijk doel
- **Schrijf duidelijke commit messages!**
- Gebruik de conventie: `type: beschrijving` (bv. `feat: add login page`, `fix: correct typo`)
- Houd commits klein en gefocust op één wijziging

---

## Basiscommando's — Algemene Workflow

<img width="600" src="https://github.com/HOGENT-IT-Lab/gititdone-workshop/blob/main/slides_test/img/git-workflow.png?raw=true"/>

<br/>

### Absolute basisflow

|Commando | Uitleg|
|---------|-------|
| `git add .` | Alles toevoegen aan staging |
| `git commit -m"commit message"` | Wijzigingen committen met een duidelijke boodschap |
| `git push` | Lokale repository naar een remote repository pushen |

<br/>

### Overige

| Commando | Uitleg |
|----------|--------|
| `git status` | Huidige toestand/situatie bekijken |
| `git log` | Historie/commits weergeven |
| `git log --oneline --graph` | Compacte geschiedenis met branch-visualisatie |
| `git init` | Van een lokale map een git repository maken |
| `git clone <URL>` | Een repository binnenhalen (klonen) |
| `git add <FILE>` | Wijzigingen in working directory aan staging toevoegen |
| `git add .` | Alles toevoegen aan staging |
| `git commit -m"commit message"` | Wijzigingen committen met een duidelijke boodschap |
| `git push` | Lokale repository naar een remote repository pushen |


<br/>

--- 

### Remote Repositories

| Commando | Uitleg |
|----------|--------|
| `git fetch` | Remote data ophalen *(verandert je working copy NIET)* |
| `git merge` | Lokale repository samenvoegen met working directory |
| `git pull` | Tegelijk fetchen en mergen |

> **Let op:** Bij `git fetch`, altijd `git merge` doen wanneer je wijzigingen effectief wilt toepassen.

<br/>

### Bij Fouten

| Commando | Uitleg |
|----------|--------|
| `git reset` | Staged changes terugzetten naar working directory |
| `git reset HEAD~1` | Laatste commit ongedaan maken *(wijzigingen blijven behouden)* |
| `git reset --hard HEAD~1` | Laatste commit én wijzigingen permanent verwijderen ⚠️ |
| `git revert <HASH>` | Veilig een commit ongedaan maken via een nieuwe commit |

<br/>

### Remote Beheren

| Commando | Uitleg |
|----------|--------|
| `git remote` | Checken of je repository gelinkt is aan een remote |
| `git remote -v` | Meer info weergeven (URL, push/fetch) |
| `git remote add origin <URL>` | Remote toevoegen aan lokale repository |

---

## Git Branches

Branches laten je toe om **parallel te werken** zonder de hoofdcode te verstoren.  
De standaard branch heet doorgaans `main` (of vroeger `master`).

<br/>

### Branch Commando's

| Commando | Uitleg |
|----------|--------|
| `git branch` | Alle lokale branches weergeven |
| `git branch -a` | Alle branches (lokaal + remote) weergeven |
| `git branch <NAAM>` | Een nieuwe branch aanmaken |
| `git switch <NAAM>` | Naar een bestaande branch gaan |
| `git switch -c <NAAM>` | Nieuwe branch aanmaken én er meteen naartoe gaan |
| `git checkout <NAAM>` | Naar een branch gaan *(oudere manier)* |
| `git checkout -b <NAAM>` | Nieuwe branch aanmaken én er naartoe gaan *(oudere manier)* |
| `git branch -d <NAAM>` | Branch verwijderen *(enkel als al gemerged)* |
| `git branch -D <NAAM>` | Branch verwijderen *(forceer, ook zonder merge)* ⚠️ |
| `git push origin <NAAM>` | Branch pushen naar remote |
| `git push origin --delete <NAAM>` | Remote branch verwijderen |

<br/>

### Branch Strategie — Best Practices

- **`main`** — stabiele, productie-klare code
- **`develop`** — integratiebranch voor nieuwe features
- **`feature/naam`** — nieuwe functionaliteit uitwerken
- **`fix/naam`** — bugfixes uitwerken
- **`hotfix/naam`** — urgente fixes rechtstreeks op main

> Werk **nooit rechtstreeks** op `main`. Maak altijd een feature-branch aan.

<br/>

### Mergen

| Commando | Uitleg |
|----------|--------|
| `git merge <BRANCH>` | Een branch samenvoegen met de huidige branch |
| `git merge --no-ff <BRANCH>` | Merge commit forceren *(zelfs bij fast-forward)* |
| `git merge --abort` | Merge afbreken bij een conflict |

<br/>

### Rebasen

| Commando | Uitleg |
|----------|--------|
| `git rebase <BRANCH>` | Jouw commits bovenop een andere branch plaatsen |
| `git rebase --continue` | Rebase verderzetten na het oplossen van een conflict |
| `git rebase --abort` | Rebase volledig afbreken |
| `git rebase -i HEAD~<N>` | Interactief de laatste N commits herwerken *(squash, rename...)* |

> **Gouden regel:** Rebase **nooit** op een gedeelde/remote branch waarop anderen werken!

<br/>

### Merge Conflicts

- Git is slim, maar de git history moet logisch zijn!
- Conflicten ontstaan wanneer twee branches **dezelfde regels** anders aanpassen
- Git markeert het conflict in het bestand met `<<<<<<<`, `=======`, `>>>>>>>`

**Stappenplan bij een conflict:**
1. `git status` — bekijk welke bestanden conflicten bevatten
2. Open de bestanden en los de conflicten manueel op
3. `git add <FILE>` — markeer als opgelost
4. `git commit` — rond de merge commit af

> **Tip:** `git pull --rebase` gebruiken bij een pull lost de meeste merge-conflicten preventief op!

---

## Git Forks

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

## Handige overige:
| Stap                                                                          | Uitleg                                                                                     |
| ----------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| `git fetch --prune`                                                           | Synchroniseert met remote en verwijdert lokale references naar verwijderde remote branches |
| `git fetch -p`                                                                | Korte versie van `git fetch --prune`                                                       |
| `git branch -vv`                                                              | Toont lokale branches + gekoppelde remote status (`gone` indien verwijderd op remote)      |
| `git branch -vv \| grep ': gone]'`                                            | Filtert branches waarvan de remote branch niet meer bestaat                                |
| `git branch -vv \| grep ': gone]' \| awk '{print $1}' \| xargs git branch -d` | Verwijdert lokaal alle branches waarvan remote branch verwijderd is                        |
| `git branch -vv \| grep ': gone]' \| awk '{print $1}' \| xargs git branch -D` | Force delete van lokale branches waarvan remote branch verwijderd is                       |
| `git remote prune origin`                                                     | Verwijdert verouderde remote tracking branches zonder fetch                                |
| `git config --global fetch.prune true`                                        | Zet automatisch prunen aan bij elke `git fetch` of `git pull`                              |
| `git pull --rebase`                                                           | Haalt wijzigingen binnen zonder extra merge commit                                         |
| `git stash`                                                                   | Slaat tijdelijke niet-gecommitete wijzigingen op                                           |
| `git stash pop`                                                               | Zet laatste stash terug en verwijdert deze uit stash lijst                                 |
| `git clean -fd`                                                               | Verwijdert ongevolgde bestanden en mappen                                                  |
| `git reset --hard HEAD`                                                       | Zet working directory volledig terug naar laatste commit                                   |
| `git reflog`                                                                  | Toont geschiedenis van HEAD wijzigingen, handig om verloren commits terug te vinden        |
