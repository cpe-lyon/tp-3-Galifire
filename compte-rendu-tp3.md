*Raphaël DUMAS - 3ICS*

# TP 3 - Utilisateurs, groupes et permissions

## Exercice 1. Gestion des utilisateurs et des groupes

1. Afin de créer les groupes "Dev" et "Infra", il faut utiliser les commandes `sudo groupadd Dev` et `sudo groupadd Infra`

2. On veut créer 4 utilisateurs, Alice, Bob, Charlie & Dave, avec un dossier personnel pour chaque. Pour ceci, il faut entrer les commandes suivantes : 
    `sudo useradd Alice -m` `sudo useradd Bob -m` `sudo useradd Charlie -m` `sudo useradd Dave -m`

3. On cherche maintenant à assigner chaque utilisateur créé aux groupes créés ci-dessus. On réalise ceci avec les commandes suivantes :
    `sudo usermod -a -G Alice/Bob/Dave Dev` `sudo usermod -a -G Bob/Charlie/Dave Infra`

4. On souhaite désormais afficher les membres du groupe Infra. On peut utiliser `grep 'Infra' /etc/group` ou alors `getent group Infra`

5. On veut changer le groupe propriétaire des dossiers personnels des utilisateurs créés. Pour ceci, il faut rentrer les commandes suivantes : `sudo chgrp -R Dev /home/Alice/` `sudo chgrp -R Dev /home/Bob/` `sudo chgrp -R Infra /home/Charlie/` `sudo chgrp -R Infra /home/Dave/`

6. Pour changer le groupe primaire des utilisateurs, il faut utiliser ces commandes : `sudo usermod Alice -g Dev` `sudo usermod Bob -g Dev` `sudo usermod Charlie -g Infra``sudo usermod Dave -g Infra`

7. On souhaite créer 2 dossiers pour les 2 groups créés, Dev et Infra. On exécute alors les commandes `mkdir /home/Dev` et `mkdir /home/Infra`. On y donne ensuite la propriété aux groupes respectifs avec `sudo chgrp -R Dev /home/Dev/` et `sudo chgrp -R Infra /home/Infra/`. Enfin, on souhaite y ajouter les permissions pour le groupe. On exécute alors les commandes `sudo chmod 770 /home/Dev/` et `sudo chmod 770 /home/Infra/`

8. Pour que seul le propriétaire du fichier puisse le renommer et le supprimer, il faut enlever le droit d'écriture aux autres. Il faut donc appliquer la commande `sudo chmod 740 /home/Dev/` et `sudo chmod 740 /home/Infra/`

9. Il est impossible d'ouvrir une session en tant qu'utilisateur Alice, car à sa création, aucun mot de passe n'a été spécifié. Tant qu'il n'y en a pas, impossible de se connecter avec ce compte.

10. On définit un mot de passe pour l'utilisateur Alice par la commande `sudo passwd Alice`

11. Pour trouver l'uid et le gid d'un utilisateur, on rentre la commande `sudo id Alice`

12. Avec la commande `id -nu 1003` on trouve directement l'utilisateur associé (ici, Bob)

13. Avec la commande `getent group Dev`, le terminal nous donne l'id du groupe, ainsi que les utilisateurs contenus.

14. On souhaite trouver un groupe via son gId, il faut alors rentrer la commande `getent group GID | cut -d: -f1`. Ici, il s'agit du groupe Dev.

15. Pour supprimer Charlie du groupe Infra, il faut utiliser la commande `sudo gpasswd -d Charlie Infra`. Charlie est donc supprimé des membres du groupe Infra, et perd les permissions qui lui sont associés.

16. On souhaite faire diverses modifications sur le compte de Dave.
    Pour changer sa date d'expiration, la commande est `sudo usermod -e 2021-06-21 Dave`
    Pour faire en sorte que Dave doive changer son mot de passe tous les 90 jours maximum, avec 5 jours de délai minimum, et qu'il soit prévenu 14 jours avant l'expiration, il faut entrer la commande `sudo chage -m 5 -M 90 -W 14 Dave`

17. L'interpréteur de commandes de l'utilisateur root est un hash (#)

18. L'utilisateur nobody sert uniquement à exécuter ce qui requiert peu de permissions. Cela sert aussi en cas de piratage, car il limite les dégâts en n'ayant des permissions minimales.

19. Par défaut, la commande sudo garde le mot de passe en mémoire pendant 15 minutes. Si l'on souhaite que le sudo oublie le mot de passe, il faut entrer la commande `sudo -K`

## Exercice 2 - Gestion des permissions

1. Après avoir créé un répertoire test, contenant un fichier, les permissions sont les suivantes : accès en écriture, lecture et exécution pour l'utilisateur, lecture et exécution pour le groupe et les autres. Pour le fichier, lecture et écriture pour l'utilisateur, lecture seule pour le groupe et les autres.

2. On retire tous les droits sur fichier avec `sudo chmod 000 /home/test/fichier`. On tente ensuite de l'exécuter avec `sudo nano fichier` et on peut quand même l'ouvrir et l'éditer. Malgré toutes les permissions supprimées, le super utilisateur garde quand même tous les droits

3. On redonne les droits à l'utilisateur avec `sudo chmod 700 /home/test/fichier`. On souhaite ensuite remplacer le texte par la commande `echo "echo Hello" > fichier`. Cependant, l'utilisateur normal n'a toujours pas la permission d'écrire dans le fichier.

4. Avec l'utilisateur classique, il est impossible d'exécuter le fichier. En revanche, avec root, cela marche complètement.

5. En se retirant les permissions de lecture tout en étant dans le dossier, il devient impossible de lister le contenu du répertoire. En revanche on peux l'exécuter, sans avoir accès à son contenu.

6. On retire les droits d'écriture avec la commande `sudo chmod 555 /home/test` et `sudo chmod 555 /home/test/nouveau` et il devient alors impossible de l'éditer. Même en rétablissant les permissions sur le répertoire, cela ne change rien, on ne peut toujours pas éditer le fichier "nouveau". On peut alors supprimer le fichier, mais l'invite à néanmoins besoin d'une confirmation. 

7. On peut essayer d'accéder au fichier test lorsque l'on en a retiré les permissions d'exécution. On ne peut ni créer, ni supprimer ni éditer de fichier. On ne peut pas non plus se déplacer dans le répertoire. En revanche on peut en lister le contenu, mais sans voir les permissions.

8. En retablissant les droits du répertoire, se déplaçant dedans puis retirant à nouveau les droits d'exécutions, les remarques sont les mêmes. En revanche, on peut toujours revenir au répertoire parent grâce à `cd ..` sinon on serait bloqué dans le dossier.

9. La commande à entrer pour avoir qu'un utilisateur du groupe puisse avoir accès en lecture au fichier "fichier" est `sudo chmod 750 /home/test/fichier`

10. Afin d'avoir un umask très restrictif pour les autres sauf l'utilisateur, il faut exécuter la commande `umask 077`

11. Pour avoir un umask permissif, donnant le droit en lecture et exécution à tous, mais seulement nous en écriture, il faut entrer la commande `umask 022`

12. Pour un masque équilibré, qui nous donne un accès complet, ainsi qu'un accès en lecture au groupe, il faut entrer un umask comme ceci : `umask 027`

13. On cherche à transcrire les commandes suivantes 
- chmod u=rx,g=wx,o=r fic --> `chmod 534 r-x-wxr--`
- chmod uo+w,g-rx fic en sachant que les droits initiaux de fic sont r--r-x--- -> `chmod 602 rw-----w-`
- chmod 653 fic en sachant que les droits initiaux de fic sont 711 `rw-r-x-wx`
- chmod u+x,g=w,o-r fic en sachant que les droits initiaux de fic sont r--r-x--- `chmod 520 r-x-w----`

14. Avec la commande `ll /etc/passwd` on obtient les permissions du fichier qui sont les suivantes : `-rw-r--r--` soit `chmod 644`