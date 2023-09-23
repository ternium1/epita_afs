# Installation ocaml et connexion AFS

## Install debian

(à faire)


## installation de ocaml

* ouvrir un terminal et taper `su - root` et rentrer le mot de passe root configuré dans l'installation
Il faut ensuite ajouter son utlisateur au groupe sudo, pour se faire:

* dans le terminal ouvert en root, il faut taper `usermod -aG sudo <nom d'utilisateur>`
* ensuite taper `visudo`
* Dans l'éditeur de texte, descendre à la ligne `root   ALL=(ALL:ALL) ALL`
* rajouter la ligne `<nom d'utilisateur>    ALL=(ALL:ALL) ALL` **ATTENTION: le séparateur entre le nom d'utilisateur et le groupement de ALL est un TAB et pas des espaces**
* Faire Ctrl+X pour quitter et appuyer sur O ou Y (selon la langue) pour enregistrer


Ensuite il faut maintenant installer OCaml.

* dans le même terminal, faire `exit`, on repasse en utilisateur normal.
Copier coller les commandes suivantes (ca va demander le mdp de l'utilisateur):

```bash
sudo apt update
sudo apt upgrade
sudo apt install git ocaml opam
```


* une fois terminé taper: `opam init`
Les dépendances vont s'installer, il se peut qu'il demande le mot de passe

Ensuite faire `opam install ocaml graphics utop`


## Accéder à l'AFS

* générer une clé SSH: `ssh-keygen`, laisser par défaut l'emplacement et éventuellement sépcifier un mdp

* aller sur cri.epita.fr et ajouter sa clé SSH (pour la voir: `cat ~/.ssh/id_rsa.pub`
* créer et editer le fichier `~/.ssh/config` (emacs ou vim) et ajouter
```
Host ssh.cri.epita.fr
    GSSAPIAuthentication yes
    GSSAPIDelegateCredentials yes

```

* installer les paquets krb5-config krb5-user: `sudo apt install krb5-config krb5-user` l'installateur va demander des info (realm ou royaume, laisser vide, ou effacer les options remplies)
* créer un dossier ou sera monté l'AFS: `mkdir afs` (on peut lui donner un autre nom)
* Génerer un ticket Kerberos (pour l'authentification): `kinit prenom.nom@CRI.EPITA.FR` (le domaine est bien en majuscule)
* monter l'AFS: `sshfs -o reconnect login@ssh.cri.epita.fr:/afs/cri.epita.fr/user/l/lo/login/u/ <nom du dossier qu'on vient de créer>`



* executer le fichier fractals.ml: `utop fractals.ml` (s'assurer d'être dans le bon directory)
Voila!
