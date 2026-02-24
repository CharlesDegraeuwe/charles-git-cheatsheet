## Git Configureren:
| Commando                                          | Uitleg                                                |
|---------------------------------------------------|-------------------------------------------------------|
| git config --global -l                            | configureren van git account (mail, naam en           |
| git config --local -l                             | locale folder git configuratie                        |
| git config --global user.name <JOUW_NAAM>         | Instellen van je naam *(belangrijk voor verificatie)* |
| git config --global user.email <JOUW@EMAIL.ADRES> | Intellen van je email *(belangrijk voor verificatie)* |

### Snelle configuratie:
```
git config --global push.default simple
git config --global core.autocrlf true   # <- Windows only!
git config --global core.autocrlf input  # <- macOS/Linux only!
git config --global init.defaultBranch main
git config --global pull.rebase true # <- Bespaart heel veel merge-issues!
git config --global rebase.autoStash true
git config --global core.ignorecase false
```
Om data te encrypteren en handtekenen genereer je **ssh-keygen** <br/>
terug te vinden in [je home folder]/.ssh

**Genereren van ssh key-value pair:**
```
ssh-keygen          #k-v pair genereren
ssh -T git@github.com           #kijken of je gelinkt bent
```
**Checken van verborgen files:**
```
<shift> + <cmd> + .
```

## Git -commits:
### Introductie:
* Om veranderingen bij te houden, maken we gebruik van commits
* Commits bekijken: git log in git-mapje
* Commits bevatten één of meerdere wijzigingen in onze repository
  <br/><br/>
* Commits hebben altijd een bijhorende commit message
* Commits hebben altijd een unieke commit hash

### Best practices:
* Commit heeft altijd doel
* **Schrijf duidelijke commit messages bro !!!!**

### Git ignore:


## Basiscommando's - algemeen:
*Hieronder commando's voor de basis git workflow*

<img width="600" src="https://github.com/HOGENT-IT-Lab/gititdone-workshop/blob/main/slides_test/img/git-workflow.png?raw=true"/>
<br/><br/>

### Basis workflow: 

| Commando                        | Uitleg                                                      |
|---------------------------------|-------------------------------------------------------------|
| git status                      | Huidige toestand/situatie bekijken                          |
| git log                         | Historie/commits weergeven                                  |
| git init                        | Van een lokale map een git repository maken                 |
| git clone <URL>                 | Een repository binnenhalen (klonen)                         |
| git add <FILE>                  | Wijzigingen in working directory aan staging toevoegen      |
| git add .                       | Alles toevoegen aan staging                                 |
| git commit                      | Wijzigingen in stage aan de lokale git-repository toevoegen |
| git commit -m"*commit message*" | Meteen een (duidelijke!) commit message toevoegen           |
| git push                        | Lokale repository naar een remote repository                |


### Remote repositories: 
| Commando  | Uitleg                                                                                                                                       |
|-----------|----------------------------------------------------------------------------------------------------------------------------------------------|
| git fetch | Remote repository naar lokale repository <br/> ***(Dit verandert je working copy niet, het updatet enkel de data in je lokale repository)*** |
| git merge | Lokale repository naar working directory                                                                                                     |
| git pull  | Tegelijk fetchen en mergen                                                                                                                   |
|           |                                                                                                                                              |
