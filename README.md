# Klasean Git erabiltzeko oinarrizko jarraibideak

Klasean Git erabiliz lan egiteko oinarrizko pausoak:

## 1. Repositorioa klonatu
Lehenik eta behin, klaseko ereduaren biltegia zure ordenagailura jaitsi:
```bash
git clone https://github.com/jiturriondobeitia/KlasekoEredua.git
```

Jarraian lanean arituko gara: Fitxategietan aldaketak egin, fitxategi berriak gehitu edo ezabatu.

## 2. Aldaketak egiaztatu
Biltegi barruan kokatuta gaudela lokalean eta Github-eko zerbitzariaren arteko aldaketak ikusteko:
```bash
git status
```

## 3. Fitxategiak gehitu
Zer fitxategi igo nahi dituzun zehaztu:
```bash
git add <fitxategia1> <fitxategia2>
# Edo aldaketa guztiak gehitzeko:
git add .
```

## 4. Commit bat sortu
Fitxategiak igotzeko commit bat egin, deskribapen labur batekin:
```bash
git commit -m "Nire lehenengo commit-a"
```

## 5. Git identitatea konfiguratu
Biltegian lehenengo aldiz aldaketak egiten badira, zure izena eta posta elektronikoa ezarri behar dira:
```bash
git config --global user.email "jiturriondobeitia@fpsanturtzilh.eus"
git config --global user.name "Jon Iturri"
```

## 6. Commit-ak zerbitzarira igo
Azkenik, zure commit-ak GitHub-era igo:
```bash
git push
```

> ### 📋 Adibidea  
> Imaginatu gure biltegian hiru motatako aldaketak ditugula:
>
> | Egoera          | Fitxategia      | Zer egin                   | Git komandoa                                |
> |-----------------|-----------------|----------------------------|---------------------------------------------|
> | Berria          | berria.txt      | Commit-ean gehitu          | `git add berria.txt`                        |
> | Aldatua         | aldatua.txt     | Aldaketak prestatu         | `git add aldatua.txt`                       |
> | Ezabatua        | ezabatua.txt    | Ezabatzea berretsi         | `git rm ezabatua.txt`                      |
>
> **`git status` komandoak horrelako zehozer erakutsiko zuen:**
> ```bash
> git status
>
> # zer adarretan ari garen lanean erakusten du ("main" adar lehenetsia da)
> On branch main
> Changes not staged for commit:
>   modified:   aldatua.txt
>   deleted:    ezabatua.txt
> Untracked files:
>   new file:   berria.txt
>
> # Aldaketa guztiak prestatu commit-a egiteko
> git add berria.txt aldatua.txt
> git rm ezabatua.txt
> # Commit-a egin horren aldaketaren deskripzioarekin batera
> git commit -m "berria.txt fitxategia gehituta, aldatuta.txt fitxategia aldatuta eta ezabatua.txt fitxategia ezabatua"
> # Commit-aren aldaketak GitHub-era igo
> git push
> ```



# 🔄 Zerbitzaritik fitxategiak eguneratzea

Klasean lanean gabiltzanean, baliteke beste ikasle edo irakasle batzuek biltegian aldaketak egitea.  
Horregatik, **zure biltegia eguneratu** behar da aldian behin, azken bertsioa edukitzeko.

---

## 1️. Zerbitzariko aldaketak ikusi
Lehenik, egiaztatu ea zure biltegia **zaharkituta** dagoen edo **berria** den:
fetch komandoak GitHub-etik azken informazioa ekarriko du, baina ez du fitxategirik aldatuko zure ordenagailuan.
```bash
git fetch
```
Ondoren, konparatu zure egoera:
```bash
git status
```
Git-ek adieraziko dizu ea zure branch-a (adibidez, main) atzeratuta dagoen (behind) edo eguneratuta (up to date).

| Egoera (Git-en mezua) | Adiera                                                                                                    |
| --------------------- | --------------------------------------------------------------------------------------------------------- |
| `up to date`          | Zure biltegia **eguneratuta** dago, ez dago aldaketarik deskargatzeko.                                    |
| `behind`              | Zure biltegia **atzera** dago; beste norbaitek aldaketak igo ditu eta **jaitsi** behar dituzu.            |
| `ahead`               | Zure biltegia **aurreratuta** dago; zuk aldaketak egin dituzu baina **oraindik ez dituzu igo**.           |
| `diverged`            | Zure eta zerbitzariaren artean **desberdintasunak** daude; bateratu egin behar da (`merge` edo `rebase`). |

## 2. Aldaketak deskargatu eta aplikatu

Zerbitzariko aldaketak zure ordenagailura ekartzeko eta aplikatzeko:
```bash
git status
```

Komando honek bi gauza egiten ditu:
```bash
git pull
```

Aldaketak deskargatu (fetch)

Eta automatikoki bateratu zure fitxategiekin (merge)
