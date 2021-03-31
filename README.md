# Compte Rendu du TP 3 : Gestion des paquets 
###### par Philippe CHARRAT, le 05/02/2021 
#### Avant-Propos : Toutes les commandes sont exécutées en tant qu'administrateur (`root`), dans un effet de mise en page et de répétition : j'ai retiré `sudo` des commandes suivantes. `sudo` étant la commande qui permet une exécution en administrateur. 

### Question 1 : Commandes de base
- Question 1 : Pour mettre à jour le système, il faut appliquer les commandes suivantes : `apt-get update | apt-get upgrade`. Les instructions sont les suivantes : 
  - `apt-get update`  : Commande pour mettre à jour de la liste des paquets disponibles. Aucun paramètre nécessaire en entrée.  
  - `apt-get upgrade` : Commande pour mettre à jour le système d'installation des paquets disponibles. 
- Question 2 : Pour créer une nouvelle commande nommée `maj`, qui automatisera la mise à jours du système. Nous allons utiliser la commande `alias maj="apt-get update | apt-get upgrade"`. La commande sont composées des trois éléments suivant : 
  - `alias` : utiliser pour substituer de longue commande avec un "diminutif". 
  - `maj` : nom de l'alias choisi par l'utilisateur.  
  - `apt-get update | apt-get upgrade` : instruction à remplacer, (cf. question 1).
  - *Remarque : La commande ne fonctionne qu'en tant que super-utilisateur `root`, sinon une erreur apparait à cause du mot de passe à saisir.*
  - Pour avoir un alias qui fonctionne au prochain redémarrage, il faut inscrire la commande dans le fichier `.bashrc` dans la catégorie *"some more Is aliases"*. 
- Question 3 : Pour obtenir les 5 derniers paquets installés, il faut utiliser `cat /var/log/dpkg.log | grep installed | tail -n 5`. Les trois commandes sont : 
  - `cat /var/log/dpkg.log` : `cat` est la commande pour afficher le fichier `/var/log/dpkg.log` qui contient l'historique des installations des paquets.  
  - `grep installed` : `grep` est la commande pour trier et afficher les lignes contenant la chaine `installed`. Nous récupérons tous la liste des paquets installés. 
  - `tail -n 5` : `tail` est la commande pour afficher les `X` (5 dans notre cas) lignes du résultat.  
  - Sortie dans le terminal : 
    - `2021-02-08 13:27:34 status installed mime-support:all 3.64ubuntu1`
    - `2021-02-08 13:27:34 status installed gnome-menus:amd64 3.36.0-1ubuntu1`
    - `2021-02-08 13:27:37 status installed man-db:amd64 2.9.1-1`
    - `2021-02-08 13:27:37 status installed libreoffice-common:all 1:6.4.6-0ubuntu0.20.04.1`
    - `2021-02-08 13:27:37 status installed desktop-file-utils:amd64 0.24-1ubuntu3`
  - Les 5 paquets sont : `mime-support gnome-menus:amd64` `man-db:amd64` `libreoffice-common:all` `desktop-file-utils:amd64`.
- Question 4 : Pour obtenir les paquets installés avec `apt install`. Il faut utiliser `cat /var/log/apt/history.log | grep "apt install"` avec les commandes : 
  - `cat /var/log/apt/history.log` : `cat` pour lire le fichier. `/var/log/apt/history.log` est un fichier qui contient l'historique des installations de paquets avec le système `apt`.
  - `grep "apt install"` : `grep` pour filtrer et afficher les lignes avec `"apt install`.
  - *Remarque : Cette question a été réalisée après le TP, pour avoir plus de paquets affichés.*
  - Sortie du terminal :   
    - ` Commandline : apt install tree`
    - ` Commandline : apt install mlocate` 
    - ` Commandline : apt install aptitude`
    - ` Commandline : apt install make` 
    - ` Commandline : apt install libncurses5-dev libncursesw5-dev` 
    - ` Commandline : apt install build-essential` 
    - ` Commandline : apt install checkinstall` 
- Question 5 : Pour connaitre le nombre de modules installés sur  la machine, il existe deux méthodes possibles : 
  - Avec le système `dpkg`, les commande sont `dpkg --get-selections | wc -l` avec : 
    - `dpkg --get-selections` : `dpkg` est un outil de gestion de package. L'option `--get-selections` sort la liste des paquets, sans options supplémentaires ceux sont les paquets installés qui sont envoyé. 
    - `wc -l` : une commande qui va compter le nombre de lignes de la sortie.
    - Dans notre cas, la réponse est *1656 paquets* installés.
  - Le nombre de modules peut être obtenue avec `apt`, avec les commandes suivantes `apt list --installed | wc -l` composé de : 
    - `apt list --installed` : `apt` est un outil de gestion de package. `list` est un l'option qui va ressortir une liste (des modules) en fonction de critère de recherche. `--installed` indique que les critères attendus sont les modules installés sur le sysytème. 
    - `wc -l` : une commande qui va compter le nombre de lignes de la sortie.
    - Dans notre cas, la réponse est *1653 paquets* installés.
  - Deux raisons peuvent expliquer la différence de trois modules : 
    - 1er raison : La sortie de `dpkg --get-selections` contient de la remise en forme. Le souci est que notre commande `wc -l` ne permet pas de faire la différence entre des lignes "superflus" et les paquets. 
    - 2ème raison: `dpkg --get-selections` liste les paquets. Mais il existe deux status pour les paquets , *ii* pour les modules installés. Le second état *rc* sont des paquets qui ont été supprimés mais dont le système a gardé les fichiers de configuration.
- Question 6 : Pour connaitre la liste de tous les paquets disponibles sur Ubuntu, une des commandes possibles est `apt list  | wc -l` avec :
  - `apt list` : `apt` est un outil de gestion de package. `list` est un l'option qui va ressortir une liste (des modules) en fonction de critère de recherche. Sans option, tous les modules sont listés. 
  - `wc -l` : commande qui va compter le nombre de lignes de la sortie.
  - *62571 paquets* sont disponibles au téléchargement via `apt`, (le 12/02/2021).
- Question 7 : Pour connaitre les informations d'un paquet, une des commande est `apt show hollywood` avec : 
  - `apt show` : commande pour afficher les informations relatives au paquet.
  - `nom` : nom du paquet, dans l'exemple c'est le module `Hollywood`.  
  - Les paquets `Hollywood`, `Glances`, et `tldr` sont :  
    - `Glances` : Application pour terminal qui va afficher l'état des ressources du système.
    - `tldr` : Manuel participatif contenant la description d'une fonction avec des exemples pratiques.
    - `Hollywood` : Application qui imite les terminales de Hackers des films hollywoodien. 
- Question 8 : La commande `apt search` permet de chercher des mots-clés dans les paquets. Dans notre cadre, le mot-clé est `sudoku`. L'exécution donne les résultats suivant :   
  - `fltk1.1-games/focal 1.1.10-26ubuntu2 amd64  Fast Light Toolkit - example games: checkers, sudoku`
  - `fltk1.3-games/focal 1.3.4-10build1 amd 6Fast Light Toolkit - example games: checkers, sudoku`
  - `gnome-sudoku/focal,now 1:3.36.0-1 amd64 [installed,automatic] Sudoku puzzle game for GNOME`
  - `hitori/focal 3.34.0-1 amd64 logic puzzle game similar to sudoku`
  - `ksudoku/focal 4:19.12.3-0ubuntu1 amd64 Sudoku puzzle game and solver`
  - `libqqwing-dev/focal 1.3.4-1.1build1 amd64 tool for generating and solving Sudoku puzzles (development)` 
  - `libqqwing2v5/focal,now 1.3.4-1.1build1 amd64 [installed,automatic] tool for generating and solving Sudoku puzzles (library)`
  - `nudoku/focal 1.0.0-1 amd64 ncurses based sudoku games`
  - `qqwing/focal 1.3.4-1.1build1 amd64 tool for generating and solving Sudoku puzzles (application)`
  - `sudoku/focal 1.0.5-2build3 amd64 console based sudoku`
  - `texlive-games/focal 2019.202000218-1 all TeX Live: Games typesetting`
  - Nous pouvons en déduire que la séquence `sudoku` est contenu dans 11 paquets.   

- Exercice 2 : Pour connaitre le paquet qui a installée la commande `ls`, nous allons utiliser la suite de commande suivante `dpkg -S $( which -a ls )` avec : 
  - `dpkg -S` :  l'option `search` recherche un fichier dans les paquets. 
  - `$( which -a fichier )` : `which` retourne le chemin du fichier qui sera excécutés dans l'environnement. L'option `-a` permet de chercher tous les chemins possibles. Dans notre cas `fichier` est `ls`.
  - Le résultat est : 
    - `serdpkg-query: no path found matching pattern /usr/bin/ls`
    - `coreutils: /bin/ls`
    - Nous pouvons en déduire que `ls` est issue du paquet `coreutils`. 
  - Pour automatiser, nous allons utiliser l'instruction suivante :
    - `dpkg -S $( which -a $1 )` : la différence est le `$1` qui représente le premier argument passé avec la commande. 
    - Exemple avec la commande `gedit`, un éditeur de texte, soit `origine-commande gedit` : 
      - `gedit: /usr/bin/gedit`
      - `dpkg-query: no path found matching pattern /bin/gedit`
    - Nous pouvons en conclure que `gedit` vient du paquet `gedit`. 

- Exercice 3 : Pour avoir une commande qui indique si un paquet et installé ou non, une des solutions est la création d'un script. Dans un premier temps, il faut créer un fichier du nom de la commande voulue. Exemple : `touch INSTALLORNOTINSTALL` avec `touch`, la création d'un fichier texte et `INSTALLORNOTINSTALL` le nom du fichier (ne pas mettre d'extension). Dans un deuxième temps, il faut ajouter les instructions suivantes : 
  ```
    #!/bin/bash #en-tête obligatoire pour dire que c'est un script.  
    variable=$(apt list --installed | grep $1 | wc -l) # affichage de la liste de tous les modules installés ou l'on va filtrer avec la séquence passé en paramètre ($1). Enfin, wc compte le nombre de ligne de sortie. 
    if [ $variable -eq 0]  # Si variable vaut 0 donc aucun paquet installé contient le paramètre.
    then 
      echo "NOT INSTALL"
    else # Sinon au moins 1 contient le paramètre utilisateurs. 
      echo "INSTALL"
    fi 
  ```
  - En cas d'exécution pour le paquet `gedit`, la commande est : `INSTALLORNOTINSTALL gedit`. La réponse est :  `INSTALL`.
  - En cas d'exécution pour le paquet `ouaisouais` (inexistant), la commande est : `INSTALLORNOTINSTALL ouaisouais`. La réponse est :  `NOT INSTALL`.

- Exercice 4 : Pour lister les programmes livrés avec un fichier, il faut utiliser la commande est `dpkg -L nom`. `-L` est l'option qui affiche la liste des fichiers qui appartiennent au paquet `nom` ( dans notre cas `coreutils`). En l'appliquant, la sortie donne (tronqué pour cause lisibilité) :
  - `/usr/share/man/man1/[.1.gz`
  - `usr/share/man/man1/wc.1.gz`
  - `/usr/share/man/man1/who.1.gz`
  - `/usr/share/man/man1/whoami.1.gz`
  - `/usr/share/man/man1/yes.1.gz`
  - `/usr/share/man/man8`
  - A l'aide d'internet et la documentation, nous pouvons déduire que le paquet `coreutils` est un des paquetages du projet GNU. Celui-ci définie des commandes comme `cat`, `ls`, etc... 
  - Avec `wc -l`, nous pouvons observer que `coreutils` ajoute 236 fichiers. 
  - Pour connaitre le but et syntaxte de la commande `[`, nous pouvons utiliser la fonction `/usr/bin/[ --help ` qui retourne le manuel d'utilisation. Nous pouvons en déduire que la commande `[` est une syntaxe différente de la commande `test` qui vérifie les attributs d'un fichier ou contenue d'une variable.  
  
- Exercice 5 : Aptitude est un gestionnaire de paquets issue de `apt`. Une de ses spécificités est la possibilité d'utiliser une interface graphique (textuelle) pour la gestion des paquets.
  - Interface graphique général : 
    ![Image 1](https://github.com/CPELyon/tp-3-paquets---groupe-2-philippeCharrat/blob/master/crtp3_image.PNG)
  - Liste des paquets non-installés : 
    ![Image 1](https://github.com/CPELyon/tp-3-paquets---groupe-2-philippeCharrat/blob/master/crtp3_image2.PNG)
  - Option de recherche d'un paquet : 
  - ![Image 1](https://github.com/CPELyon/tp-3-paquets---groupe-2-philippeCharrat/blob/master/crtp3_image3.PNG)
  - Information relative à un paquet (sur l'image le paquet `lynx`) : 
    ![Image 1](https://github.com/CPELyon/tp-3-paquets---groupe-2-philippeCharrat/blob/master/crtp3_image5.PNG)
  - Installation d'un paquet (sur l'image le paquet `lynx`) : 
    ![Image 1](https://github.com/CPELyon/tp-3-paquets---groupe-2-philippeCharrat/blob/master/crtp3_image4.PNG)
  - A l'aide de la documentation et de recherche internet complémentaire : 
    - `lynx` : Navigateur web textuelle utilisable via terminal. La navigation est au clavier et il est utilisé pour les déficiences visuelles. 
    - `emacs` : Éditeur de texte modulaire.

- Exercice 6 : Dans certain cas, les paquets ne sont pas disponibles sur un dépôt officiel d'Ubuntu et nécessite l'utilisation d'un dépot personnel (nommé PPA). Pour cet exemple, nous allons installer JAVA depuis le dépôt d'Oracle. 
   - Ajout du PPA et du paquet JAVA
     ```
     sudo add-apt-repository ppa:linuxuprising/java # Ajout du PPA
     sudo apt update # Mise à jours des liste des paquets disponibles.
     sudo apt install oracle-java15-installer # Installation JAVA 
     ```
   - Observation du fichier /etc/apt/sources.list.d avec la commande : `cat /etc/apt/sources.list.d` 
     - `/linuxuprising-ubuntu-java-focal.list`
     - `deb http://ppa.launchpad.net/linuxuprising/java/ubuntu focal main`
     - `deb-src http://ppa.launchpad.net/linuxuprising/java/ubuntu focal main`
     - Nous pouvons observer qu'un nouveau PPA à été ajouté à la liste des dépôts possibles. A noté que les PPA sont hébergés sur la plateforme launchpad. 

- Exercice 7 : Pour installer un paquet depuis son code source, il y a plusieurs étapes à suivre : 
  - 1er étape : télécharger le code source via une plateforme (exemple : gitLab ou github). Pour des raisons de practicités, il est possible d'utiliser un logiciel de gestion de version comme git. Avec celui-ci, la commande `git clone https://gitlab.com/jallbrit/cbonsai` permet de copier le répertoire cbonsai dans notre répertoire courant. 
  - 2ème étape : installer les paquets complémentaires pour le bon fonctionnement du programme. Dans l'exemple de cbonsai, les suivant étés nécessaires ( avec une instalaltion via `apt install` (cf. Question 4)) :
    - `make` : utilitaire pour la compilation et l'édition de lien.  
    - `libncurses5-dev libncursesw5-dev` : utiliser pour ncurse (API pour interface de menu déroulant). 
    - `build-essential` : utilitaire dans la construction de paquet.
    - `checkinstall` : expliqué dans la suite de l'exercice.  
  - Résultat intermédiaire : la commande `cbonsai` est exécutable par l'utilisateur, à condition de se placer dans le répertoire 
  - 3ème étape : "convertion" du code en paquet avec la commande `checkinstall`. 
    - Une question va apparaitre pour créer une section doc (`Should I create a default set of package docs?  [y]: y`) avant l'éxecution.   
    - Un paquet a été créé `cbonsai_20210212-1_amd64.deb` et la commande `cbonsai` est utilisable. 
    - Il est possible de supprimer ce paquet avec la commande `dpkg -r cbonsai`. 
  - Résultat : Tous les utilisateurs peuvent utiliser la commande `cbonsai`. Un  exemple : 
    - `su -l alice` : changement de compte vers l'utilisateur alice.
    - `cbonsai` fonctionne.  

- Exercice 8 : Création de dépôt  
   - Création d'un paquet avec `dpkg-deb`.
     - 1ere étape : création de l'arborescence du paquet avec la commande `mkdir`. Dans notre cas, il se nomme `origine-commande` et doit avoir l'arborescence suivante : 
        ```
          $ tree (commande pour afficher une arborescence) 
         origine-commande 
                 └── DEBIAN
                        ├── control (fichier avec les informations du paquet)
                        └── usr
                            └── bin
                                └── origine-commande (script) 
        ```
     - * Attention, il s'agit d'une arborescence minimum. Il est conseillé d'ajouter de la doc et scripts, ([plus d'informations](https://alp.developpez.com/tutoriels/debian/creer-paquet/))*
     - Le fichier control, doit contenir les informations suivantes : 
        ```
          Package: origine-commande    #nom du paquet
          Version: 0.1 #numéro de version
          Maintainer: Philippe CHARRAT #votre nom
          Architecture: all #les architectures cibles de notre paquet (i386, amd64...)
          Description: Cherche l'origine d'une commande
          Section: utils #notre programme est un utilitaire
          Priority: optional #ce n'est pas un paquet indispendable
        ```
     - Création du paquet avec la commande `dpkg-deb --build origine-commande`
     - La commande `ls -ll` affiche le paquet `origine-commande.deb`, donc il a bien été créé.  
  - Création d'un dépôt : 
    - création de l'arborescence suivante avec la commande `mkdir` : 
       ```
            repo-cpe 
              ├── packages 
              └── conf
                    └── distribution (fichier avec les informations du dépot)
       ```
    - Le fichier control contiendra les informations : 
       ```
           Origin: Dépôt de Philippe CHARRAT
           Label: repo-cpe
           // Suite: stable
           Codename: focal //Pour connaitre notre distribution, utiliser la commande : lsb_release -a
           Architectures: i386 amd64    #(architectures cibles)
           Components: universe #(correspond à notre cas)
           Description: Dépôt contenant les paquets de Philippe CHARRAT dans le cadre de son cursus. 
       ```  
    - `reprepro` est un utilitaire pour la gestion des paquets et `-b` le paramètre pour définir le répertoire de base. La commande est : `reprepro -b . export` avec `.` est le répertoire courant (dans notre cas `/home/philippe/repo-cpe`).     
    - Pour placer les paquets souhaités, il est possible d'utiliser les commandes `mv` ou `cp` vers le dossier packages. Dans un second temps, il faut inscrire les paquets dans le dépôt avec : `reprepro -b . includedeb focal origine-commande.deb`. 
    - Il faut inscrire le nouveau dépôt dans notre liste de sources disponible, en créant le fichier `repo-cpe.list` dans le répertoire  `/etc/apt/sources.list.d` (exemple : `vim /etc/apt/sources.list.d/repo-cpe.list`). Celui-ci contiendra la ligne : 
        ```
          deb file:/home/philippe/repo-cpe focal multiverse
        ```
    - *Ne pas oublier de mettre à jour la liste des dépôts avec `sudo apt update`*
 - Sécurisation du dépôt : 
   - Créer une paire de clés de chiffrement assymétrique avec la commande `gpg --gen-key`. Pour les générer, il faut renseigner son nom, son email et une paraphrase ( dans mon cas : `darksideofthem00n`). Il est obligatoire d'avoir des chiffres et lettres. 
   - Modifier le fichier de configuration de dépôt (exemple : `vim conf/distribution`) en ajoutant le paramètre : `SignWith: yes`. 
   - Ajouter les clés au dépot avec les commandes `reprepro --ask-passphrase -b . export` et `gpg --export -a "auteur" > public.key`.
   - Ajouter une clé dans les clés fiables avec la commande `sudo apt-key add public.key`.
    
# Compte Rendu du TP 4 
###### par Philippe CHARRAT, le 05/02/2021 
#### Avant-Propos : Toutes les commandes sont exécutés en tant qu'administrateur (`root`), dans un effet de mise en page et de répétition : j'ai retiré `sudo` des commandes suivantes. `sudo` étant la commande qui permet une exécution en administrateur. 

### Partie 1 : Gestion des utilisateurs et des groupes
- Question 1 : Pour créer  un groupe, il faut utiliser la commande : `groupadd dev` avec `dev` qui est le nom du groupe créé. 
- Question 2 : Pour créer un compte utilisateur, la commande est `useradd alice --create-home --shell /bin/bash` avec deux options : 
  - `alice` : le nom de l'utilisateur créé.
  - `create-home` : création du dossier personnel associé à l'utilisateur.
  - `shell` : sélection du shell pour exécuter les commandes du terminal utilisateur. Dans notre cas, nous avons choisi `/bin/bash` qui est un shell relativement complet.  
- Question 3 : Pour ajouter un utilisateur dans un groupe, la commande est `adduser alice dev` avec : 
  - `alice` : le nom de l'utilisateur à ajouter.
  - `dev` : le nom du groupe destination. 
- Question 4 : La première méthode est la commande `getent group infra`, avec `getent` une commande pour les bases de données (issue de l'article [suivant](linuxize.com/post/how-to-list-groups-in-linux)). `group` correspond à la base de données appelée et `infra` au mot clé recherché. </br> La seconde méthode est composée de deux instructions : 
  - `cut -d : -f1,4 /etc/group` : Les lignes dans `/etc/group` sont composés d'éléments séparés par ':'. `cut` va scinder le fichier en colonnes. L'option `-d :` indique que le séparateur est le caractère ':'. `-f 1,4` indique la récupération des colonnes 1 et 4. 
  - `grep infra` : `grep` permet de filtrer les résultats et d'afficher ceux contenant le mot clé attendu (dans notre cas : infra ou dev).
  - *Remarque : pour le groupe dev, on a plusieurs groupes qui contiennent *dev* donc la sortie donne plusieurs lignes.* 
  - Commande complète : `cut -d : -f1,4 /etc/group | grep infra` 
  - Résultat avec le groupe `infra` pour la seconde méthode : `infra:dave,bob,charlie`
- Question 5 : Pour modifier le propriétaire ou le groupe, la commande est `chown -R alice:infra /home/alice` avec les options : 
  - `-R` : Modifie les répertoires et leurs contenus de manière récursive. 
  - `alice:infra` : le nouveau propriétaire (`alice`) et le nouveau groupe (`infra`).
  - `/home/alice` : le répertoire concerné. 
- Question 6 : Pour changer de groupe intial d'un utilisateur, la commande est `usermod -g dev alice` avec les options : 
  - `-g` : paramètre pour indiquer la modification du groupe **initial**.
  - `nomdugroupe` : nouveau groupe assigné, dans notre cas dev.
  - `user` : nom de l'utilisateurs affecté, dans notre cas alice.
- Question 7 : Pour créer un dossier partagé entre tous les membres d'un groupe (exemple : infra). La commande est composée de trois parties :
  - `mkdir /home/infra` : création d'un dossier nommé `infra` dans le répertoire `/home`.
  - `chown -R root:infra /home/infra` : la commande `chown` réalise la modification de propriété d'un élements. Le `-R` indique que le changement sera appliqué de manière récursive sur les sous-dossiers et contenus du répertoire. `root:infra` dit que le nouveau propriétaire est `root` et le nouveau groupe propriétaire est `infra`. `/home/infra` indique le dossier concerné par les changements. 
  - `chmod 770 /home/infra` : `chmod` permet un changement des permissions sur un éléments. `770` indique que le propriétaire et membres du groupe propriétaire auront toutes les permissions sur ce dossier, les autres aucune. `/home/infra` indique le dossier concerné. 
  - La commande complète : `mkdir /home/infra |chown -R root:infra /home/infra | chmod 770 /home/infra`
- Question 8 : Dans le dossier partagé, seuls les propriétaires peuvent renommer ou supprimer leurs fichiers. La commande est `chmod g+t /home/infra`. Le *t* est le sticky bit, il s'agit d'une permission spéciale pour que seul le propriétaire puisse supprimer ou renommer son fichier. Le *g* pour modifier les permissions du groupe. 
- Question 9 : Nous ne pouvons pas nous connecter sur les comptes créés (alice, charlie, ...), il est nécessaire d'avoir un mot de passe configurer pour l'utilisateur.   
- Question 10: Pour configurer le mot de passe d'un compte, il faut appliquer la commande : `passwd alice`. Cela va demander de saisir deux fois le mot de passe. Une fois cette manipulation effectuée, le compte est utilisable.
- Question 11: Pour connaitres l'id utilisateur (UID), la commande est `id -u alice` et l'on obtient l'id suivant : `1001'. 
- Question 12: Pour connaitre le nom de l'utilisateur à partir de son UID. La commande`id -nu 1003` retourne `charlie`. 
- Question 13: Pour connaitre l'id d'un groupe en fonction de son nom. Nous allons utiliser les trois instructions suivantes : 
  - `cat /etc/group` : Affiche le fichier /etc/group.   
  - `grep infra` :  Affiche les lignes avec `infra` (nom du groupe).
  - `awk -F : '{print $ 3}'` : Affiche la troisième colonne donc le GID ( à l'aide de la doc : [ici](http://doc.ubuntu-fr.org/tutoriel/gestion_utilisateurs_et_groupes_en_ligne_de_commande))
  - Commande complète : `cat /etc/group |grep infra |awk -F : '{print $ 3}' 
  - *Remarque : pour le groupe dev, on a plusieurs groupes qui contienne *dev* donc la sortie donne plusieurs lignes.* 
  - La réponse pour `alice`, est l'id `1001`. 
- Question 14: Pour connaitre le nom d'un groupe en fonction de l'identifiant. Nous allons utiliser les trois instructions suivantes : 
  - `cat /etc/group` : Affiche le fichier /etc/group.   
  - `grep 1003` :  Affiche les lignes avec `1003` (id du groupe).
  - `awk -F : '{print $ 1}'` : Affiche la première colonne à savoir le nom ( à l'aide de la doc : [ici](http://doc.ubuntu-fr.org/tutoriel/gestion_utilisateurs_et_groupes_en_ligne_de_commande))
  - Commande complète : `cat /etc/group |grep 1003 |awk -F : '{print $ 1}'`
  - La réponse pour le GUID 1003 est `charlie`. 
- Question 15: La commande `gpasswd --delete charlie infra` va retirer le membre `charlie` du groupe `infra`. A l'aide de `cat /etc/group`, nous pouvons observer que charlie a été retiré du groupe infra. Avec `id charlie`, le groupe initial reste `infra`. Nous pouvons en déduire que l'utilisateur "reste" dans le groupe tant qu'un nouveau ne lui est pas attibué mais n'est plus référencé dans celui-ci. 
- Question 16: Pour faire expirer un compte à une date précise, nous utilisons la commande `usermod -e 2021/06/01 dave`. L'option *e* est pour indiquer la présence de la date d'expiration (au format AAAA/MM/DD). Pour la configuration du mot de passe, la commande est `chage -m 5 -M 90 -W 14 -I 30 dave` avec les options suivantes :
  - `-m X`: Le mot de passe ne peut être modifié avant X jours, avec 5 jours dans notre cas.
  - `-M X`: Le mot de passe est expiré s'il n'est pas modifié avant X jours, 90 dans notre cas.
  - `-W X`: Si la date d'expiration du mdp est dans X (14) jours, l'utilisateur reçoit un message d'alerte. 
  - `-I X`: Le compte est bloqué si le mot de passe est expiré depuis X (30) jours.
- Question 17: Pour connaitre le shell utilisé par un utilisateur, la commande est composée de 3 parties :
  - `cat /etc/passwd` : Affichage du fichier `/etc/passwd` qui contient tous les utilisateurs enregistrés sur la machine.
  - `grep root` : Trie et récupération des lignes contenant le mot clé `root`
  - `awk -F : '{print $ 7}'` : Affichage de la 7ième colonne, le shell de l'utilisateur. 
  - *Remarque : Plusieurs utilisateurs ou groupes se nomment `*root*`*
  - Commande complète : `cat /etc/passwd | grep root | awk -F : '{print $ 7}'` 
  - Sortie : `/bin/bash /usr/sbin/nologin`, on en déduit que `/bin/bash` est le shell du root. Après modification de la ligne (en affichant la première colonne), il apparait que  `/usr/sbin/nologin` est le shell de l'utilisateur `nm-openvpn`.
- Question 18: Avec la commande `cat /etc/passwd | grep nobody`, la sortie est : `nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin`. Cet utilisateur a pour unique fonction d'éxecuter des programmes. C'est une sécurité de Linux qui va faire un utilisateur avec un minimum de permissions. En exécutant des scripts, s'il se fait corrompre par une attaque alors aucun fichier ne sera impacté.
- Question 19: Quand la commande `sudo` est utilisée, la session va mémoriser pendant 15 minutes le mot de passe administrateur. Pendant ce laps de temps, l'utilisateur peut exécuter sudo sans demande de mot de passe. Pour finir la session, il faut appliquer la commande `sudo -k` avec le paramètre `-k` pour remettre à zéro la session de temps.

### Partie 2 : Gestion de permissions

- Question 1 : Après la création du dossier avec la commande `mkdir` et du fichier avec `touch`. La commande `ls -ll .` va lister les fichiers, leurs propriétaires et permissions. Le `.` indique le fichier courant. Les sorties sont :
  -`drwxrwxr-x 2 philippe philippe 4096 Feb  5 16:22 test` : Pour le répertoire `test`, le propriétaire (`philippe`) à les droits de lecture/ecriture/exécution. Le groupe `philippe` et les "autres" ont des droits de lecture/exécution.
  -`-rw-rw-r-- 1 philippe philippe 78 Feb 5 16:22 fichier` :  Pour le fichier `fichier`, le propriétaire (`philippe`) et le groupe `philippe` ont les droits de lecture/ecriture. Les "autres" ont le droit de lecture.
- Question 2 : Si l'on supprime tous les droits avec la commande : `chmod 000 fichier`. `000` indique aucune permission aux propriétaire, groupe et autres. Avec la commande `vim fichier` (vim est un éditeur de texte), deux possibilités : 
  - utilisateur `philippe` : permission refusée.
  - utilisateur `root` : possibilité de modifier/supprimer le fichier. Le super utilisateur `root` n'est pas impacté par les permissions sur le fichier. 
- Question 3 : Pour la commande `echo "Hello World" > fichier`, deux cas de figure possibles : 
  - utilisateur `philippe` ou du groupe `philippe`, le contenu de `fichier` est remplacé par "Hello World".
  - autre (exemple : utilisateur `alice`), la permission est refusée. Ceci est cohérent car cela est une écriture sur `fichier`. Or, à l'aide de la question 1, il a été établi que les autres utilisateurs n'ont pas de droit d'écriture sur `fichier`. 
- Question 4 : Pour la commande `./fichier`, deux cas de figure : 
  -utilisateur `philippe` ou du groupe `philippe` ou autre : Permission denied. Les permissions montrent que `x` (exécution) n'est pas autorisé par défaut sur les fichiers. 
  -`root` : exécution du fichier, mais erreur car le fichier n'est pas un script. Comme pour la question 2, `root` n'est pas impacté par les permissions du fichier donc peut l'exécuter.  
- Question 5 :  Avec la commande `chmod 330`, les permissions de lecture sont retirées (explication de l'utilisation du `chmod` ci-après). Les commandes suivantes : 
  - `ls -ll test` : `ls: cannot open directory 'test': Permission denied`. Il n'est plus possible de lister les contenues du fichier.
  - `mkdir test_test` : Possibilité de créer un répertoire ou fichier. 
  - `vim test/fichier` : Possibilité de modifier un fichier. 
- Question 6 : Si les permissions du dossier `test`  sont Exécution/Lecture (`chmod 550 test`), il est possible de modifier des fichiers existants. Mais les actions de créer ou supprimer un fichier (exemple : commande `touch nomdufichier`) sont interdites. Pour le fichier `nouveau`, sans la permission d'écriture, il est dans un état de lecture-seule (exemple : `vim fichier`). 
- Question 7 :Avec la commande `chmod 660`, les permissions d'exécution sont retirées. Le résultat des commandes suivantes : 
  - `ls -ll test` : `ls: cannot open directory 'test': Permission denied`. Il n'est plus possible de lister les contenus du répertoire.
  - `mkdir test_test` : Il est impossible de créer un fichier/réperoire. 
  - `vim test/fichier` : Il est impossible de modifier un fichier.  
  - `cd test/` : Il est impossible se déplacer dans le dossier `test`.  
- Question 8 : Une fois dans le répertoire `test`, avant d'exécuter `chmod 660`. Il est impossible de faire les commandes comme dans la question 7. A noter que `cd ..` fonctionne car il s'agit des permissions du dossier parent, (dans notre cas `/home/philippe` avec les permissions 755). 
- Question 9 : Pour qu'un fichier (nommé `fichier` dans notre cas), soit accessible en lecture seule pour les membres de notre groupe, il faut appliquer la commande  `chmod 740 fichier` qui correspond à : 
  - `chmod` : Commande pour changer les permissions d'un fichier/dossier. 
  - `740` : les permissions sont au format `XYZ` avec `X` qui correspond aux permissions de l'utilisateur propriétaire. `Y` sont celles des utilisateurs du groupe et `Z` pour les autres utilisateurs. Pour que le fichier soit en lecture seule pour les utilisateurs du groupe, il faut passer `Y` à 4. (Le 4 représente la permission de lecture en UNIX).
- Question 10: Le `umask` est la commande qui permet de définir les permissions par défaut lors de la création d'un fichier ou répertoire. La consigne est d'avoir un masque qui n'accorde aucune permission aux autres utilisateurs.  
  - `umask 077` : les permissions sont au format `XYZ` avec `X` qui correspond aux permissions de l'utilisateur propriétaire. `Y` sont celles des utilisateurs du groupe et `Z` pour les autres utilisateurs. Attention, les permissions sont inversées donc `0` correspond à tous les droits et `7` aucun droit. `077` donne tous les droits à l'utilisateurs et aucun droits aux autres.
  - Après l'application du masque, la commande `mkdir se` pour créer un répertoire `se` puis `ls -ll`, la réponse est : `drwx------ 2 alice dev 4096 Feb  5 17:16 se`. Nous pouvons en déduire que les permissions sont cohérente.
- Question 11: La consigne est d'avoir un masque permissif qui autorise les utilisateurs à lire ou traverser les dossiers.  
  - `umask 022` : `022` donne tous les droits à l'utilisateur propriétaire, le `2` indique que seul le droit d'écriture n'est pas autorisé aux autres.
  - `drwxr-xr-x 2 alice dev 4096 Feb  5 17:18 tre`, nous pouvons en conclure que les autorisations sont accordées comme attendu. A noter qu'en cas de création de fichier (avec `touch gyu` par exemple), la sortie est : `fichier : -rw-r--r-- 1 alice dev    0 Feb  5 17:19 gyu` donc la permission d'exécution n'est pas ajoutée sur le fichier. 
- Question 12: La consigne est d'avoir un masque équilibré qui autorise les utilisateurs du groupe à lire ou traverser les dossiers. 
  - `umask 027` : `027` donne tous les droits à l'utilisateur propriétaire, le `2` indique que seul le droit d'écriture n'est pas accordé aux membres du groupe. `7` aucune permission pour les autres membres. 
  - Création d'un répertoire `tres` donne : `drwxr-x--- 2 alice dev 4096 Feb  5 17:18 tres ` et création de fichier `trees`: `-rw-r----- 1 alice dev 0 Feb  5 17:19 trees`. Nous pouvons en conclure que les permissions sont celles attendues. 
- Question 13: 
  - `chmod u=rx, g=wx, o=r fic` : `u=rx` indique que l'utilisateur à les droits de lecture (4) et d'exécution (1) soit 5. `g=wx` indique que les membres du groupe ont le droit d'écriture (2) et d'exécution (1) soit 3. `o=r` indique que les "autres" ont un droit de lecture (4) et `fic` est le nom du fichier. En conclusion, la commande pourrait s'écrire : `chmod 534 fic`.
  - `chmod uo+w, g-rx fic` : `uo+w` indique que l'utilisateur (u) et les autres (o) "gagnent" un droit d'écriture (2), soit `u=rw- (6)` et `o=-w- (2)`. `g-rw` indique que les membre du groupe (g) "perdent" les droits de lecture et exécution soit `g=--- (0)`. La commande pourrait s'écrire `chmod 602.
  - `chmod 653 fic` : `653` est au format `ugo`. Le `6` indique que l'utilisateur aura le droit de lecture (4) et écriture (2). Le `5` indique que les membres du groupes (g) auront les droits de lecture (4) et d'exécution (1). Le `3` indique que les autres (o) auront le droit d'écriture (2) et d'exécution (1).
  - `chmod u+x,g=w,o-r` : `u+x` indique que le propriétaire "gagne" le droit d'exécution, soit `u=rw- (6)`. `g=w` indique que les membres du groupe ont le droit d'écriture soit `g=-w- (2)`. `o-r` indique que les autres ont "perdu" le droit d'écriture soit `o=--- (0)`. La commande pourrait s'écrire : `chmod 620`.  
- Question 14: la commande `ls -ll /etc | grep passwd` permet d'afficher la ligne suivante `-rw-r--r--  1 root root  3066 Feb  5 15:02 passwd`. Ce fichier est une base de donées qui contient les informations sur les utilisateurs. Ce fichier appartient à l'utilisateur `root` qui peut lire et écrire dedans, cela est cohérent car il est le seul à pouvoir créer/modifier/supprimer des utilisateurs sur le système. Un utilisateur classique peut être amené à consulter cette liste mais il ne doit pas pouvoir la modifier (ce serait très dangereux), donc il a une permission en lecture seule.

# Compte Rendu du TP 5 : Gestion de disque  
###### par Philippe CHARRAT, le 05/02/2021 
#### Avant-Propos : Toutes les commandes sont exécutées en tant qu'administrateur (`root`), dans un effet de mise en page et de répétition : j'ai retiré `sudo` des commandes suivantes. `sudo` étant la commande qui permet une exécution en administrateur. 

### Exercice 1 : Disques et Partitions 
- Question 1 : Pour la création d'un second disque dur, nous allons utiliser l'interface de *Virtual-Box* avec les paramètres suivants : 
  - Le disque dur aura une taille dynamiquement allouée et sera de 5 Go.
  - Se rendre dans les paramètres de la VM, dans la section *Stockage* :   
    ![](https://github.com/CPELyon/tp5-disques---groupe-2-philippeCharrat/blob/master/TP5_I0.png) 
  - Aller dans la partie controleur *SATA* et choisir l'option créer un disque :  
    ![](https://github.com/CPELyon/tp5-disques---groupe-2-philippeCharrat/blob/master/TP5_I1.PNG)  
- Question 2 : Pour vérifier l'ajout du disque sur la VM, il existe plusieurs méthodes. J'ai décidé d'utiliser l'utilitaire *Gparted*, un éditeur de partition de l'environnement GNOME (plus d'informations : [ici](https://fr.wikipedia.org/wiki/GParted)). A l'aide de celui-ci, la fenêtre suivante est apparue. Nous pouvons en déduire que le disque a bien été ajouté.  
    ![](https://github.com/CPELyon/tp5-disques---groupe-2-philippeCharrat/blob/master/TP5_I2.PNG)
- Question 3 : Pour créer une ou des partitions sur le disque, il faut suivre les étapes suivantes : 
  - Dans un terminal, lancer la commande : `fdisk disque` avec `fdisk` un outil de partition de disque dur. Dans notre cas, `disque` se nomme `/dev/sdb` (cf. figure 3). 
  - *fdisk* est un outil textuel, la suite se déroule dans un terminal. "L'interface" est la suivante : 
    ```
      Welcome to fdisk (util-linux 2.34).
    
      Command (m for help) :
    ```
  - Pour créer une nouvelle partition, l'option est `n`.  
    - `p` : la partition doit être primaire ou étendue, (cette option n'étant pas étudié dans le TP, nous avons fait le choix de n'utiliser que des primaires).
    - `+2000M` : correspond à la taille de la partition crée. `M` indique des méga-octects, donc cette option est pour la création d'une partition de *2Go*.
    -  Résultat : `Created a new partition 1 of type 'Linux' and of size 2 GiB.`
  -  Les partitions crées sont de type linux. Pour le modifier, il faut appliquer la démarche suivante : 
     - `Command : t` : l'option `t` est pour le changement de type.
     - Il est possible d'obtenir la liste de tous les types avec l'option `l`.
     - Dans notre cas, la partition de 3G devait être en NTFS. Soit l'option 7.
  - *Attention : bien penser à enregistrer vos changements avec l'otpion `w`.*
  - Il est possible d'observer les partitions avec la commande `fdisk -l disque`. Dans notre cas avec `/dev/sdb`, la sortie donne : 
    ```
      Device     Boot   Start      End     Sectors Size Id Type 
      /dev/sdb1         2048  4098047     4096000   2G 83 Linux 
      /dev/sdb2          4098048 10242047 6144000   3G  7 HPFS/NTFS/exFAT
    ```
- Question 4 : Pour utiliser les partitions, il est nécessaire de les formater. Une des méthodes est la commande `mkfs.extension partition`, avec `.extension` qui varie en fonction du type de la partition. Dans notre cas, il y a deux cas : 
  - `/dev/sdb1` du type Linux. Le système de fichier est ext4 [plus d'info](https://fr.wikipedia.org/wiki/Ext4). La commande complète est :  `mkfs.ext4 /dev/sdb1`
  - `/dev/sdb2` du type NTFS. Le système de fichier est NTFS [plus d'info](https://fr.wikipedia.org/wiki/NTFS_(Microsoft)). La commande complète est :  `mkfs.ntfs -f /dev/sdb2`
- Question 5 : La commande `df -T` ne permet pas de voir les partitions `sdb1` et `sdb2`. Le manuel dit que `df` indique les quantités des éléments *montés*. Dans notre cas, les partitions n'ont pas encore été montées, cela est donc cohérent.  
- Question 6 : Pour monter une partition, il y a deux étapes : 
  - `vim /etc/fstab ` : il faut modifier le fichier `/etc/fstab` qui contient la liste des disques utilisés au démarrage. Cette modification permet qu'à chaque démarrage, les disques sont montés automatiquement. 
    - `/dev/sdb1 /data auto defaults  0 0` : indique que la partition `/dev/sdb1` sera montée sur le dossier `/data`.
    - `/dev/sdb2 /win auto defaults 0 0` : indique que la partition `/dev/sdb1` sera montée sur le dossier `/win`.
  - Création des dossiers `/data` et `/win`.
- Question 7 : Pour monter un disque, il faut utiliser la commande `mount disque fichier` avec `disque` le disque à monter comme `/dev/sdb1` ou `/dev/sdb2`. `fichier` indique l'emplacement des dossiers avec `/data` et `/win` dans notre cas. Une fois la machine redémarrée, la commande `df -T` donne : 
  - `/dev/sdb1                         ext4       1983056    6000   1858272   1% /data` 
  - `/dev/sdb2                         NTFS       1983056    6000   1858272   1% /win`
  - Nous pouvons en conclure que le montage a fonctionné pour les deux disques. 
- Question 8 : Pour monter une clé USB, le "problème" est que notre machine est une machine virtuelle. Ainsi, quand une clé USB est connecté : nous devons indiquer à *Virtual-Box* que l'élement est pour la VM. Pour ce faire, nous devons aller dans la barre de Menu et séléctionner Périphériques. Dans ce menu, nous pouvons observer tous les périphériques connectés sur l'ordinateur. Si un est selectionné alors il "disparait" de Windows pour apparaitre sur la VM.  
    ![](https://github.com/CPELyon/tp5-disques---groupe-2-philippeCharrat/blob/master/TP5_I3.png) 
- Question 9 : L'ajout des *Additions invités* sur la machine virtuelle a été "fatal" pour la machine. La configuration surement trop juste a provoqué l'abandon de l'exercice.  

## Exercice 2 : Partitionnement LVM
- Nous allons utiliser l'utilitaire Logical Volume Manager (LVM) pour créer et expxloiter un disque supplémentaire.  
- Etape 1 : Pour réutiliser le disque `/dev/sdb`. Il faut dans un premier temps supprimer les partitions montées précédement avec les commandes suivantes : 
  - `umount fichier` : démontage de la partition vers le fichier, donc dans notre cas `fichier` vaut `/data` et `/win`.
  - `vim /etc/fstab ` : Suppression des lignes pour monter les disques vers `/data` et `/win`, au démarrage. 
- Etape 2 : Il faut modifier les partitions, il faut utiliser la commande : `fdisk`.
  - `d` : pour supprimer les partitions. Dans notre cas, il faut l'appliquer deux fois. 
  - `n` : pour créer une partition de 5 Go (100% du disque).
  - `t` : pour modifier le type de Linux vers LVM (code : 8e). 
- Etape 3 : Il faut créer un nouveau volume physique qui correspond à notre partition qui va être géré par *LVM*. Pour cela, la commande est : 
  - `pvcreate /dev/sdb1` :  `pvcreate` la commande de création du nouveau volume et `/dev/sdb1` son nom.
  -  `pvdisplay` : commande pour afficher les volumes présents sur la machine. 
  -  La sortie donne :  
    ```
       --- NEW Physical volume --- PV Name  /dev/sdb1  VG Name   PV Size     <5.00 GiB
    ```
  - Nous pouvons observer que le volume `/dev/sb1` a été créé et qu'il prend les 5 Go du disque.
- Etape 4 : Il faut créer un groupe de volume (VG) qui est un ensemble qui regroupe plusieurs disques physiques. Pour le créer, il faut utiliser la commande : 
  - `vgcreate vg00 /dev/sdb1` : `vgcreate`, la commande pour créer les groupes. `vg00` est le nom donné au groupe et `/dev/sdb1` le disque physique affecté.
  -  `vgdisplay` : la commande pour afficher les groupes de volume. 
  -  Sortie : 
     ```
        --- Volume group ---  VG Name               vg00
     ```
  - Nous pouvons en déduire que le groupe *vg00* a été créé. 
- Etape 5 : Le groupe ne suffit pas, il faut créer un volume logique (pseudo-partition), il faut appliquer la commande avec les paramètres suivants : 
  - `lvcreate` : commande de création d'un volume logique.
  - `-l 100%FREE` : taille de l'espace du volume logique. Le paramètre `-l` pour une taille relative (en %) et `-L` pour une taille fixe, exemple : `-L 50g`.
  - `-n nom` : paramètre `-n` pour donner le nom au volume logique. Dans notre cas, le nom est `lvData`.
  - `groupe` : affectation du volume logique dans un groupe. Dans notre cas, le groupe sélectionné est `vg00`
  - `lvcreate -l 100%FREE -n lvData vg00` : Commande complète. 
  - La commande `lvscan` permet de lister les volumes logiques.
  - Nous obtenons la sortie suivante : 
    ```
      ACTIVE            '/dev/vg00/lvData' [<5.00 GiB] inherit
    ```
- Etape 6 : Dans le volume logique, il faut créer une partition et la formater au format `ext4`. Dans un second temps, il faut monter la partition.
  - `mkfs -t ext4 nom` : commande de formatage de données au format `ext4`. Dans notre cas, la partition est `/dev/mapper/vg00-lvData`.
  - `mount nom /dir` : commande pour monter une partition vers le dossier */dir*. Dans notre cas, la partition est `/dev/mapper/vg00-lvData` et `/data` le dossier de direction.
  - `vim /etc/fstab ` : commande pour automatiser le montage d'une partition au boot (cf. 1.6).
  - `df -h` : commande pour afficher les partitions et confirmer que la partition a été créée. 
  - Nous obtenons la sortie suivante : 
    ```
      /dev/mapper/vg00-lvData            4.9G   20M  4.6G   1% /data
    ```
- Etape 7 : Pour ajouter de l'espace de stockage vers le groupe, il faut appliquer le processus suivant : 
  - Création d'un nouveau disque (cf. question 1.1), il sera disponible sous le nom `/dev/sdc`. 
  - `fdisk` : création d'une partition (`n`) et changement du type vers *LVM* (8e). 
  - `pvcreate nom` : création du volume physqiue `nom`. Dans notre cas, son nom est `/dev/sdc1`. 
  - `vgextend vg00 /dev/sdc1` : extension du groupe `vg00` avec l'ajout de la partition `/dev/sdc1`.
  - `lvextend --size +5G /dev/vg00/lvData` : ajout de 5G sur le volume logique *VL*.
  - `resize2fs /dev/mapper/vg00-lvData` : changement de la taille de système du fichier. Dans notre cas, le système de fichier `/dev/mapper/vg00-lvData`.
  - Une fois la manipulation effectuée, la commande : `df -h` donne : 
  ```
    /dev/mapper/vg00-lvData           ext4      10251540   22992   9688088   1% /data
  ```
  - Nous pouvons observer que `/dev/mapper/vg00-lvData` est de 10G donc la manipulation est réussie. 
- Maintenant la machine possède un volume de 10 Giga supplémentaire sur deux disques différents.

# Compte Rendu du TP 6 : Service Réseau  
###### par Philippe CHARRAT, le 05/03/2021 
#### Avant-Propos : Toutes les commandes sont exécutées en tant qu'administrateur (`root`), dans un effet de mise en page et de répétition : j'ai retiré `sudo` des commandes suivantes. `sudo` étant la commande qui permet une exécution en administrateur. 

### Exercice 1 : Adressage IP 
- Plage d'adresse IP : 172.16.0.0/23
- Le découpage en sous-réseaux : 
   - Sous-Réseau 1 : 
      - 38 machines 
      - Nombre d'adresse : 62
      - Première adresse : 172.16.0.1
      - Dernière adresse : 172.16.0.62
      - Masque Réseau : 26
      - Adresse Réseau : 172.16.0.0
      - Adresse de Broadcast : 172.16.0.63  
   - Sous-Réseau 2 : 
      - 32 machines 
      - Nombre d'adresse : 62
      - Première adresse : 172.16.0.65
      - Dernière adresse : 172.16.0.126
      - Masque Réseau : 26
      - Adresse Réseau : 172.16.0.64
      - Adresse de Broadcast : 172.16.0.127
   - Sous-Réseau 3 : 
      - 52 machines 
      - Nombre d'adresse : 62
      - Première adresse : 172.16.0.129
      - Dernière adresse : 172.16.0.190
      - Masque Réseau : 26
      - Adresse Réseau : 172.16.0.128
      - Adresse de Broadcast : 172.16.0.191
   - Sous-Réseau 4 : 
      - 35 machines 
      - Nombre d'adresse : 62
      - Première adresse : 172.16.0.193
      - Dernière adresse : 172.16.0.254
      - Masque Réseau : 26
      - Adresse Réseau : 172.16.0.192
      - Adresse de Broadcast : 172.16.0.255 
   - Sous-Réseau 5 : 
      - 34 machines 
      - Nombre d'adresse : 62
      - Première adresse : 172.16.1.1
      - Dernière adresse : 172.16.1.62
      - Masque Réseau : 26
      - Adresse Réseau : 172.16.1.0
      - Adresse de Broadcast : 172.16.1.63 
   - Sous-Réseau 6 : 
      - 37 machines 
      - Nombre d'adresse : 62
      - Première adresse : 172.16.1.65
      - Dernière adresse : 172.16.1.126
      - Masque Réseau : 26
      - Adresse Réseau : 172.16.1.64
      - Adresse de Broadcast : 172.16.0.127
    - Sous-Réseau 7 : 
      - 25 machines 
      - Nombre d'adresse : 30
      - Première adresse : 172.16.0.129
      - Dernière adresse : 172.16.0.158
      - Masque Réseau : 26
      - Adresse Réseau : 172.16.0.128
      - Adresse de Broadcast : 172.16.0.159       
### Environnement du TP 
- Pour ce TP, nous allons avoir besoin de deux machines différentes. Les configurations sont les suivantes : 
   - Server : 
     - OS : Ubuntu server
     - Rôle : DHCP, DNS
     - Deux cartes réseaux : 
       - NAT : Accès à internet via la machine.
       - Réseau Privé : Accès au réseau 192.168.100.0/24.
   
   - Client : 
     - OS : Ubuntu server
     - Rôle : /
     - Réseau Privé : Accès au réseau 192.168.100.0/24.

### Installation d'un serveur DHCP 

- Rôle du serveur DHCP : Pour qu'un équipement communique sur un réseau, il attend certains paramètres. À savoir : 
  - Adresse IP : adresse qui identifie une machine unique, composée de deux parties. La partie Réseau identique pour tous les équipements d'un même réseau et la partie unique à chaque machine. 
  - Masque : élément composé de bit qui permet de séparer la partie réseau de la partie machine d'une adresse IP. 
  - Gateway : adresse pour "sortir" du réseau.  
- Quand on connecte un nouvel appareil sur un réseau, celui-ci ne possède pas les paramètres. Il existe deux méthodes pour lui renseigner, la première est à la main. Mais cela peut être long et il y a des risques de conflits d'adressage. La seconde méthode est l'utilisation d'un serveur DHCP. Ce serveur va transmettre les paramètres et peut aussi envoyer un DNS ou avoir des plages d'adresses IP possibles. Pour configurer un serveur DHCP sur une machine, il faut suivre le processus suivant : 
  -  Etape 1 : Installation du paquet `isc-dhcp-server` avec la commande `sudo apt install isc-dhcp-server`. Pour démarer un service, la commande vérifier le bon fonctionnement de celui-ci, il est possible d'utiliser la commande `systemctl start isc-dhcp-server`. La commande `systemctl status isc-dhcp-server` permet d'afficher l'état et les log du paquet `isc-dhcp-server`. 
    - Une première exécution retourne *failed* car nous n'avons pas configuré le serveur.   
  -  Etape 2 : Définir une adresse Ip, celle-là sera permante et utiliser par les autres équipements pour joindre le serveur. Dans notre cas, l'IP sera `192.168.100.1`.
    - 1er étape : Vérifier que l'interface est activée avec la commande `ip link set enp0s8 up`. Il est possible de le vérifier avec la commande `ifconfig`. 
      - Le résultat (tronqué) montre trois cartes réseaux, cela est cohérent : 
      ```
         * np0s3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
         * enp0s8: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
         * lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
      ```
    - 2ème étape : Fixer l'adresse Ip et le masque avec la commande `ip addr add 192.168.100.1/24 dev enp0s8`.
  - Etape 3 : La configuration du serveur DHCP est dans le fichier `/etc/dhcp/dhcpd.conf`. Avec un éditeur de texte comme *vim*, il est possible de le configurer. 
    - `default-lease-time` : correspond à la durée de bail d'une adresse avant expiration. Dans notre cas 600 secondes.
    - `max-lease-time` : durée maximale d'un bail en seconde. Dans notre cas 600 secondes. 
  - Etape 4 : Configuration des ports du DHCP dans le fichier `
  - Etape 5 : Vérifier la configuration avec la commande `dhcpd -t`. S'il n'y a pas de message d'erreurs, alors le service peut être démarré avec `systemctl start`
- Une fois que le client se connecte sur le réseau, il va émettre une requête DHCP  de type DHCPREQUEST. Le serveur va lui répondre en lui donnant une des adresses de la plage. 
  ```
   Mar  6 10:03:09 server dhcpd[2624]: DHCPREQUEST for 192.168.100.20 (192.168.100.1) from 08:00:27:0f:51:96 via enp0s8
   Mar  6 10:03:09 server dhcpd[2624]: DHCPACK on 192.168.100.20 to 08:00:27:0f:51:96 via enp0s8
  ```
- Nous pouvons observer que le client (à l'adresse MAC : ) récupère l'adresse `192.168.100.100` du serveur. 
- Le fichier `/var/lib/dhcp/dhcp.lease`contient l'historique des attributions d'adresses par le DHCP.
- Le ping fonctionne.
- Il est possible d'imposer des adresses avec la configuration suivante : 
  ```
   host client1 {
        hardware ethernet 08:00:27:0f:51:96; // L'adresse MAC du client 
        fixed-address 192.168.100.20; // L'adresse IP
   }
  ```
- Dans ce cas, le client va récupérer l'adresse *192.168.100.20*. 
   
### Accès à internet depuis le client  
- Avec notre configuration actuelle, le client à accès à notre réseau mais il ne peut pas aller sur internet. Nous allons devoir appliquer les changements suivants : 
  - *IP Forward* est le procédé pour faire du routage de notre serveur vers internet. De base, il est désactivé. Pour l'activer, il faut aller dans le fichier `/etc/sysctl.conf` (avec `vim` ou `gedit`) et décommenter la ligne : 
    - `#net.ipv4.ip_forward=1`
  - Ensuite, il faut relancer `sysctl` avec la commande `sysctl -p /etc/sysctl.conf`.
  - Pour configurer le routage, il faut configurer le mécanisme NAT. Celui-ci permet de modifier l'adresse de destination d'un paquet (convertion d'une ip privé vers publique).  La commande est : `iptables --table nat --append POSTROUTING --out-interface enp0s3 -j MASQUERADE`. 
- Une fois le système relancé, notre client à accès à internet et des commandes comme `ping 8.8.8.8` fonctionne.

### Installation d'un DNS   
- Rôle du serveur DNS : Pour qu'un utilisateur puisse communiquer avec un serveur  distant sans rentrer son adresse IP, des noms ont été attribués aux serveurs. Un *Domain Name Serveur* (DNS) est un serveur qui contient une table qui fait le lien entre un nom et son adresse. Pour en installer un sur notre serveur, il faut suivre le processus suivant : 
  - Etape 1 : Installation du paquet `Bind9` avec la commande `apt install bind9`.
  - Etape 2 : Pour une configuration rapide et simple d'un DNS, il est conseillé d'utiliser *forwarders*. Ce mode ne contient aucune information en cache et va contacter un DNS public. 
    -  Dans le fichier `/etc/bind/named.conf.options`, décommenter la ligne `forwarders`.
    -  Ajouter un DNS publique comme `8.8.8.8` ou `1.1.1.1`.
    -  Relancer le service *Bind9* avec `systemctl restart bind9`.
- Une fois la configuration effectuée, les clients peuvent utiliser le protocole DNS. Il est possible de naviguer sur internet avec un navigateur comme *Lynx*.

### Ajout d'une zone privé dans le DNS 
- But : Dans un réseau privée, en ajoutant une zone privé pour pinguer les équipements du même réseau. 
  -  Etape 1 : Déclaration des zones direct ou indirecte dans le fichier `/etc/bind/named.conf.local`.
     -  Zone directe : 
        ```
            zone "tpadmin.local" {
               type master;
               file "etc/bind/db.tpadmin.local";
            }
        ``` 
     -  Zone indirecte : 
        ```
            zone "100.168.192.in-addr.arpa" {
               type master;
               file "etc/bind/db.t&92.168.100";
            }
        ```
  - Etape 2 : Copier les fichiers de configuration initial *db.*, dans notre cas `db.tpadmin.local` et `dv.192.168.100`.
  - Etape 3 : Modifier les fichiers créé avec : 
     -  Zone directe : 
        ```
            $TTL    604800
            @       IN      SOA     server.tpadmin. charrat.p.gamil.com. (
                                       0603         ; Serial
                                       ... (tronqué) 
            ;
            @       IN      NS      tpadmin.
            @       IN      A       192.168.100.1
            @       IN      AAAA    ::1
        ``` 
     -  Zone indirecte : 
        ```
            $TTL    604800
            @       IN      SOA     server.tpadmin.local. root.localhost. (
                                          1         ; Serial
                                          ... (tronqué)
            @       IN      NS      server.
            1       IN      PTR     server.
        ```
  - Etape 4 : Vérification des fichiers avec les commandes suivantes : 
     ```
         named-checkconf named.conf.local // Vérification du fichier de configuration
         named-checkzone tpadmin.local /etc/bind/db.tpadmin.local // Vérification de la zone directe
         named-checkzone 100.168.192.in-addr.arpa /etc/bind/db.192.168.100 // Vérification de la zone indirecte
     ```
  - Etape 5 : Relancer le processus `Bind9` avec la commande `systemctl restart bind9`. 
- Les client et serveur peuvent se pinguer.   

# Compte Rendu du TP 7 : Boot, services et processus 
###### par Philippe CHARRAT, le 12/03/2021 
#### Avant-Propos : Toutes les commandes sont exécutées en tant qu'administrateur (`root`), dans un effet de mise en page et de répétition : j'ai retiré `sudo` des commandes suivantes. `sudo` étant la commande qui permet une exécution en administrateur. 

### Personnalisation de GRUB 
- Par défaut, le menu de GRUB est désactivé. Il est donc nécessaire de le ré-activer en modifiant deux paramètres dans le fichier de paramétrages `/etc/default/grub` (exemple : `vim /etc/default/grub`).  
  - `GRUB_TIMEOUT_STYLE=nom` : Le style est définie par le paramètre `nom`. Par défaut, la valeur est `hidden`, donc le menue ne s'affiche pas. Celui-ci peut être affiché avec le paramètre passé à `menu`. (Ligne : `GRUB_TIMEOUT_STYLE=menu`) 
  - `GRUB_TIMEOUT=X` : Le temps d'affichage du menue va être de X secondes, pour une durée de 10 seconde la commande est : `GRUB_TIMEOUT=10`
  - Pour générer le fichier de configuration, il faut appliquer la commande : `update-grub`. 
- Avec la commande `reboot`, nous pouvons vérifier que le menue pendant 10 secondes au démarrage.
-  Pour modifier la résolution du menue GRUB, dans le fichier `/etc/default/grub` il faut faire décommenter la ligne : 
  - `GRUB_GFXMODE=resolution1,resolution2` : composée de trois parties : 
    -  `GRUB_GFXMODE` : Paramètre de gestion de résolution 
    -  `resolution1` : indique sous la forme de `longxhaut`, dans notre cas : la première résolution est : 1280x1024.
    -  `resolution2` : Si la première résolution est impossible, la seconde est utilisée. Dans notre cas : 640x480.
    -  La commande complète : `GRUB_GFXMODE=1280x1024,640x480`. 

- Pour ajouter une image de fond dans notre menue, il faut suivre deux étapes : 
  - 1 : Redimensionner l'image en fonction de la taille de la résolution. Pour ce faire, nous allons utiliser l'utilitaire `ImageMagick`. 
    - `convert ./"test.jpg" -resize 1280x1024! -depth 16 ./"test_2.jpg"` : création d'une image `test_2.jpg` avec les dimmensions. 
    - `cp ./test_2.jpg /boot/grub/` : copie de l'image dans le dossier `/boot/grub`. 
  - 2 : Modification du fichier `/etc/default/grub` 
    - `GRUB_BACKGROUND="/boot/grub/test_2.jpg"`

- Pour ajouter un thème au menue, il faut suivre deux étapes : 
  - 1 : installation d'un thème pré-définie. Dans notre cas, nous avons sélectionné le thème `ubuntu_mate`.
    - `apt install grub2-themes-ubuntu-mate` : installation des fichiers nécessaire pour utiliser le thème. 
  - 2 : Modificaction du fichier de paramètrage : 
    - `GRUB_THEME="/boot/grub/themes/ubuntu-mate/theme.txt"` 
    - *Remarque : Commentez la ligne GRUB_BACKGROUND*. 

- Pour ajouter une ou des entrées dans le menue GRUB, il faut ajouter les lignes suivantes dans le fichier `/etc/grub.d/40_custom` : 
   ```
      menuentry "Nom de la fonction" {
          // TODO : Instruction 
      }
   ```
  - Dans notre cas, pour ajouter une instruction pour redémarer la machine : 
  ```
    menuentry "Redémarrer la machine" {
        reboot
    }
  ```

### Modules et Noyau 
- But : Un module est un morceau de code qui permet l'ajout de fonction sur le noyau. Il y en a de tout type comme de la gestion de périphérique ou des protocoles réseaux ([Informations complémentaires](https://doc.ubuntu-fr.org/tutoriel/tout_savoir_sur_les_modules_linux)). Il est possible de gérer des modules supplémentaires avec la démarche suivante.  
- Prérequis : 
  - Avoir le module `build-essential`, avec tous les fonctions pour compiler et utiliser des programmes en C. 
  - Avoir un programme avec les fonctions voulue, dans notre cas le code est fournie ci-dessous : 
     ```
      // Ajout des bibliothèques 
      #include<linux/module.h>
      #include<linux/kernel.h>
      // Définition des informations du module
      MODULE_LICENSE("GPL");
      MODULE_AUTHOR("JohnDoe"); // A changer
      MODULE_DESCRIPTION("Modulehelloworld");
      MODULE_VERSION("Version1.00");
      // Définition des fonctions 
      int init_module(void) {
        // printk : une fonction pour afficher des messages dans la console du noyau
        printk(KERN_INFO"[Helloworld]- Lafonction init_module() est appelée.\n");
        return0;
      }
      
      void cleanup_module(void) {
        printk(KERN_INFO"[Helloworld]- Lafonction cleanup_module() est appelée.\n");
      }
     ```
  - Avoir un programme de *Makefile* pour générer le module : 
    ```     
      obj-m += hello.o
      all:
        make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules
      clean:
        make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
      install:
        cp ./hello.ko /lib/modules/$(shell uname -r)/kernel/drivers/misc 
    ```
  
  - Compiler le programme avec la commande : `make install`. Cela génère le fichier `hello.ko`. 

- Pour ajouter un module dans le noyau, il faut utiliser la commande `insmod nom`. Cette commande va ajouter le module `nom` dans le noyau. Dans notre cas, le nom du module `hello.ko`. Cela donne la commande : `insmod hello.ko`. 
- Pour confirmer l'ajout du module dans le noyau, deux méthodes possibles : 
  - `lsmod` : affichage des informations sur les modules du noyau. Pour filtrer et vérifier que le module `hello` a été ajouté. Il est possible d'utiliser la fonction `grep hello`. 
    - Résultat : `hello   16384 0`
  - `dmesg` : affichage de la console du noyau. 
    - Résultat : `[516.056679] [Hello World] - La fonction init_modules est appelée.`     
- Pour obtenir des information sur le module, il faut utiliser la commande `modinfo nompaquet`. Dans notre cas avec le paramètre `nompaquet` à `hello.ko` :  
  - Résultat : 
     ```
      filename: /homme/philippe/hello.ko
      version: version 1.0
      description: Module HELLO WORLD
      ... (tronqué)
     ```
- Pour décharger le module `hello.ko`, il faut appliquer la commande `rmmod hello.ko`. 
  - Résultat avec la commande `dmesg` : 
    ```
      [1911.64376] [Hello world]- La fonction clean_up() a été appelée.
    ```
  - Conclusion : le module a été décharger du noyau. 

### Interception de signaux 
- But : Etude des signaux envoyés par l'utilisateurs vers des processus. Pour se faire, nous allons utiliser la commande `trap` pour les intercepter.
-  Pré-requis : 
  - utilisation d'un script pour capter toutes les entrées clavier de l'utilisateur et envoyer vers le fichier `tmp.txt`. 
    - Le code : 
    ```
      #!/bin/bash //header pour un script .sh
      read "$REPLY"; echo "$REPLY" > tmp.txt // Capture des entrées avec la commande read et echo pour orienter vers le fichier tmp.txt
    ```
    - Ajout des droits avec la commande : `chomd +1 script.sh`
- Différrent cas : 
  -  `CTRL+Z`: le script est stoppé 
     - Résultat : `[1]+ Stopped  ./script.sh`
     - Le script est passé en arrière-plan. Il est possible de remettre le processus *./script.sh* en premier plan avec la commande `fg`. 
  - `CTRL+C`: le script est interrompu 
    - Résultat : pas de sortie. 
- Interception des signaux avec la commande `trap`.
  - `kill -l` : Commande pour lister les 64 signaux possibles et leurs numéros. 
  - Dans le script `script.sh`, il faut ajouter les lignes : 
    ```
      ligne 2 : trap "echo je fait du ménage; rm tmp.txt" 2 // Interception du signal 2 : SIGINT (CTRL+C) ->  suppresion du fichier tmp.txt
      ligne 3 : trap "echo impossible de me passer en BG" 20 // Interception du signal 20 : SIGTSTP 
    ```
  - Résultat : 
    - `CTRL+C` : "je fait du ménage", comportement attendue.
    - `CTRL+Z` : "impossible de me passer en BG", mais le processus ne se place pas en background. Il se met en erreur et il faut utiliser la commande `kill numéroduprocessus`.  
  - Remarque : Quand le script est stoppé alors qu'aucune entrée à été saisie, un message d'erreur apparait. Cela est du à la tentative de suppresion d'un fichier qui n'existe pas. Une méthode pour ne plus avoir d'erreur consciste à modifier la ligne 2 du scirpt avec : 
    ```
      ligne 2 : trap "echo je fait du ménage; test -e tmp.txt && rm tmp.txt || echo fichier inexistant" 2 // Interception du signal 2 : SIGINT (CTRL+C) ->  suppresion du fichier tmp.txt 
    ```
    - `test -e tmp.txt ` : commande qui retourne 1 si le fichier existe et 0 sinon. 
    - Deux possibilités : 
      - fichier existe : Suppression de celui-ci. 
      - fichier inexistant : Message `fichier inexistant` 

###  Surveillance de l’activité du système
- La commande `w` va afficher des informations relatives aux utilisateurs connectés sur notre machine. Dans notre cas, le résultat est le suivant : 
  ``` 
    15:28:13 up 3 users, load average : 0.19 0.12 0.38
    user (...)
    philippe tty2 (...)
  ```
  - Cette commande donne trois informations clées : 
    - Les informations sur l'utilisateur comme son nom ou son tty. 
    - Le temps de JPU attaché au tty.
    - Le PCPU en cours (exemple : -bash). 
- La commande `htop` va afficher les différentes informations comme l'utilisation des CPU, de la Mémoire ou le nombre de tâches en cours. 
- Pour récupérer l'historique des connexions, il est possible d'utiliser `last`. Cette commande va afficher une liste contenant les informations comme le nom, le tty ou la date. Il est possible d'ajouter des options pour obtenir un nombre de connexions (`-n x (le nombre)` ou à une certaine date.
- La commande `cat /proc/version` va permettre d'obtenir les informations en lien avec la version du noyau. 
- La commande `lscpu --json` va permettre d'obtenir les informations du processeurs au format json. 
- La commande `journalctl`est une commande qui va permettre de récupérer différents logs sous la forme de journal. Il existe de nombreuses options dont deux ci-dessous: 
  - `--list-boots` : Pour un affichage des derniers boots.  *
  - `--boots=numéro_du_boot` : Pour obtenir les informations complète relative à ce boot.   
# Compte Rendu du TP 8 - Docker
###### par Philippe CHARRAT, le 19/03/2021 
#### Avant-Propos : Toutes les commandes sont exécutées en tant qu'administrateur (`root`), dans un effet de mise en page et de répétition : j'ai retiré `sudo` des commandes suivantes. `sudo` étant la commande qui permet une exécution en administrateur. 

### Avant Propos 
- *Docker* est un logiciel utilisé pour lancer des applications dans des conteneurs logiciels. Ces conteneurs sont des solutions pour exécuter des applications dans des environnements "plus légés" qu'une machine virtuelle. Pour se faire, l'hyperviseur et les OS invité sont remplacé par un  système de conteunarisation. Ainsi les ressources ne sont pas alloués exclusivement à un docker.

### Pré-requis  
- Pour pouvoir télécharger et utiliser le logiciel Docker, il faut les bibliothèque suivantes : 
  - `apt-transport-https` : Gestion des authentifications entre clients et serveurs par certificats. 
  - `ca-certificates` : Gestion des autorisations de certification par autorité de confiance.   
  - `curl` : Bibliothèque de requête aux URL.  
  - `gnupg` : Bibliothèque pour l'implémentation du standard OpenPGP vers le GNU. 
  - `lsb-release` : Fonction pour connaitre la version de Linux. 
- *Remarque :* il est possible d'installer les bibliothèque avec la commande `apt install nomDuModule`.
- Il faut ajouter la clé publique de Docker pour la cryptographie. Avec la commande : `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o/usr/share/keyrings/docker-archive-keyring.gp`.
- Il faut le dépôt Docker à la liste des sources (pour apt) avec la commande : `$ echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg]https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee/etc/apt/sources.list.d/docker.list > /dev/null`.
- Une fois les manipulations effectués, il faut faire une mi se à jours du système `apt` avec `apt update`. 

### Installation et première exécution
- Nous allons télécharger les paquets suivants :
  - `docker-ce` : paquet contenant les fonctions de Docker.
  - `docker-ce-cli` : Paquet pour utiliser Docker en ligne de commande.
  - `containerd.io`
     
- Pour exécuter un Docker, il faut utiliser la commande : `docker run nomDuDocker`. Un exemple est `hello-world` à la place de `nomDuDocker`.   
  - Le résultat : 
  ```
    Hello from Docker ! (...)
  ```
  
### Utilisation dans le cadre d'application plus poussée 
- Pour lancer un serveur Nginx, nous devons utiliser la commande `docker run -d -p 8080:80 nginx` qui contient les paramètres suivants : 
  - `docker run` : Commande pour exécuter le docker. 
  - `-d ` : le processus est détacher de la console, l'équivalent du symbole `&` en linux. 
  - `-p 8080:80` : Définie le port de connexion `8080` et sa redirection `80`. 
  - `nginx` : logicviel libre de serveur web.
  - Résultat : Un site est accessible à l'adresse `127.0.0.1:8080` avec une page html.  
- *Remarque* : Peut être utilisé avec de nombreux modules qui feront varier les paramètres en entrées. 
- Il est possible de lister les Docker en cours d'exécution avec la commande `docker ps`
  - Résultat : 
  ```
    Names          Images 
    fc6c2b4cf0aa    ngnix
  ```
- Pour se connecter à notre Docker et faire des modifications (comme la modification d'une page html). Il faut utiliser la commande `docker exec -ti nomSession bash` avec les arguments suivants : 
  - `docker exec` : la commande de connexion. 
  - `-ti` : paramètre pour avoir un shell opérationel.
  - `nomSession` : Identifiant du Docker, dans notre cas : fc6c2b4cf0aa. 
- * Remarque * : Pour stopper le docker, il faut utiliser la commande `docker stop nomSession`   

### Utilisation de Dockerfile
- But : Les Dockerfile permettent une configuration rapide d'une image, étape par étape. Pour ce Dockefile, nous allons réaliser la configuration d'un serveur en Node.js (issue de Javascript).
- Etape 0 : Utilisation/Inspiration d'un code : ` https://github.com/OpenClassrooms-Student-Center/ghost-cms`
- Etape 1 : Création du fichier `Dockerfile` contenant toutes les instructions nécessaires à la configuration d'un service (dans notre cas `nodejs`). 
   ```
      FROM debian:9 // En-tête
      RUN apt-get update -yd \ // mise à jours des paquets en mode silencieux 
      && apt-get install curl gnupg -yg \ // installation des paquets curl et gnupg
      && curl -sL https://deb.nodesource.com/setup_10.x  // téléchargement du paquet NodeJS
      && apt-get install nodejs -yq \
      && apt-get clean -y
      
      ADD . /app/ // Ajout du dossier app dans l'image du Docker
      WORKDIR /app // Equivalent de cd en linux
      RUN npm install // Installation du paquet NodeJS
      
      EXPOSE 2368 // Port d'écoute de l'application
      VOLUME /app/logs 
      
      CMD npm run start // Exécution du démarrage
   ```
   
- Etape 2 : Construction de l'image du Docker avec la commande `docker build -t ocr-docker-build .`. Celle ci composé de 3 éléments : 
  - `docker build ` : instruction de construction du Docker.
  - `-t nomDuPaquet` : paramètre pour donner nommer le nom au Docker, à savoir *ocr-docker-build*. 
  - `nomDuRépertoire` : Répertoire pour la construction.

- Etape 3 : Lancement du Docker avec la commande `docker run -d -p 2368:2368 ocr-docker-build`. Celle ci composé de 3 éléments : 
  - `docker run ` : instruction de lancement du Docker.
  - ` -d -p 2368:2368` : paramètre pour donner pour l'ouverture du port 2386 (cf. Dockerfile). 
  - `nomDuPaquet` : Dans notre cas *ocr-docker-build*.

- Résultat : Le serveur NodeJS est active à l'adresse *127.0.0.1:2386*.

### Utilisation dans le cadre d'application multi-conteneurs
- But : Docker Compose est un outil pour des applications multi-services. Dans notre exemple, nous allons installer un serveur Word-Press qui nécessiste un serveur web (intégré à Wordpress) et une base de données, via MySQL. 
- Etape 1 : Instalation du service Docker Compose avec `sudo curl -L "https://github.com/docker/compose/releases/download/version/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose && sudo chmod +x /usr/bin/docker-compose`.
  - `version` : Dans notre cas, la version était 1.28.6 
- Etape 2 : Configuration du Docker-Compose avec le fichier `docker-compse.yml` avec un éditeur de texte (exemple : `vim` ou `gedit`). 
   ``` 
    version: '3'
    services:
      // Configuration de MySQL (Base de Données)
      db:
        image: mysql:5.7
        volumes:
          - db_data:/var/lib/mysql
        restart: always
        // Configuration des paramètres
        environment:
          MYSQL_ROOT_PASSWORD: somewordpress
          MYSQL_DATABASE: wordpress
          MYSQL_USER: wordpress
          MYSQL_PASSWORD: wordpress
      
      // Configuration de WordPress
      wordpress:
        depends_on:
          - db
        image: wordpress:latest
        ports:
          - "8000:80"
        restart: always
        // Configuration des paramètres
        environment:
          WORDPRESS_DB_HOST: db:3306
          WORDPRESS_DB_USER: wordpress
          WORDPRESS_DB_PASSWORD: wordpress
          WORDPRESS_DB_NAME: wordpress

    volumes:
      db_data: {} 
    ```  
- Etape 3 : Récupérer les images des services avec la commande `docker-compose pull`.
- Etape 4 : Exécution du code avec la commande `docker-compose up -d` pour activer les services. 
- Remarque : Les deux services sont activés et accessible à l'addresse `http://localhost:8000`.  

