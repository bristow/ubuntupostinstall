===============================
Script de post-installation pour Ubuntu
===============================

Avec ce script Python (forké depuis nicolargo/ubuntupostinstall),
vous ne perdrez plus de temps pour installer les versions LTS d'Ubuntu.

Le script est un peu simplifié par rapport à l'original.

Le script peut :

* Installer des dépots Ubuntu (deb repos, PPA, ...)
* Installer des paquets logiciels
* Configurer des fichiers personnalisés (.bashrc, .vimrc, ...)
* Configurer éventuellement l'interface utilisateur (Unity et Gnome Shell)
* Lancer des commandes
* J'ai rajouté aussi une section debs qui permet de télécharger des debs et de les installer (skype, teamviewer...)

## Comment utiliser ce script ?

Il suffit de télécharger et exécuter en utilisant ces commandes :

    $ wget https://raw.github.com/bristow/ubuntupostinstall/master/ubuntu-14.04-postinstall.py
    $ chmod a+x ubuntu-14.04-postinstall.py
    $ sudo ./ubuntu-14.04-postinstall.py

Par défaut, ce script utilise et télécharge ce fichier de conf :

https://github.com/bristow/ubuntupostinstall/blob/master/ubuntu-14.04-unity-postinstall.cfg

## On peut utiliser un autre fichier de conf si besoin

Utilisation de l'option -c, ce qui permet de sélectionner un autre fichier de conf.

Par exemple, on peut utiliser le fichier de conf suivant pour gnomeshell :

    $ sudo ./ubuntu-14.04-postinstall.py -c https://raw.github.com/bristow/ubuntupostinstall/master/ubuntu-14.04-gnomeshell-postinstall.cfg

Si vous voulez utiliser un fichier de conf local (adapté à vos besoins):

    $ sudo ./ubuntu-14.04-postinstall.py -c mycfg.cfg

## Créer votre propre fichier de configuration

Le fichier de configuration est organisé en section.

### preactions section

This is the first section of the configuration file.

A line starting with the action_ string (following by the action name) defines a 
action (command line) to be executed.

The action name will be displayed during the script execution.

The lines will be executed before all the others steps.

Example:

    [preactions]
    action_dummy = dpkg -l > /tmp/pkg-before.txt

The dummy action will create a /tmp/pkg-before.txt with a listing of 
all the packages installed on your system.

### repos section

In this section, user can install the repositories (deb repository or PPA).

* ppa_xxx = ppa:ppauser/ppaname > Add the ppa:ppauser/ppaname to the system
* pkg_xxx = pkglist             > Add the package list (space separed) to the system
* url_xxx = http://reposurl     > Add the repository URL to the system 
* key_xxx = key                 > Add the repository key to the system 

xxx define the name of the action and will be displayed during the script execution.

Example:

    [repos]
    ppa_glances = ppa:arnaud-hartmann/glances-stable
    pkg_glances = glances
    
Install the Glances PPA on the system and install the glances software.

### packages section

From this section, you can install all your softwares, gathered by 
function, needs...

If the item starts with remove_ then packets are uninstalled.

Example:

    [packages]
    network = iftop ifstat
    dev = vim git 

Install iftop, ifstat, vim and git. Display "Install network packages" and "Install 
dev packages" during the script execution.

Example:

    [packages]
    remove_unuse = eclipse

Remove eclipse. Display "Remove unuse" during the script execution. 

### dotfiles section

This section is dedicated to the dot files (.bash, .vimrc...) installed in your 
home folder. 

The script can install the following dot files from URL:

* bashrc: BASH main configuration file
* bashrc_prompt: BASH prompt configuration
* bashrc_aliases: BASH aliases
* vimrc: VIM main configuration file
* htoprc: HTOP main configuration file

Example:

    [dotfiles]
    bashrc = https://raw.github.com/nicolargo/dotfiles/master/bashrc

Create the ~/.bashrc from the https://raw.github.com/nicolargo/dotfiles/master/bashrc

### unity and gnome3 sections

Configure Unity or Gnome Shell:

* theme: Configure the GTK theme (name)
* icons: Configure the icons theme (name)
* cursors: Configure the cursors theme (name)
* conky: Conky main configuration file (URL)

For the themes, packages have to be installed in the repos or packages sections.

    [gnome3]
    theme = Boomerang
    icons = Faenza
    cursors = DMZ-White
    conky = https://raw.github.com/nicolargo/ubuntupostinstall/master/conkyrc

Configure Gnome Shell with the Boomerang GTK theme, Faenza icons and DMZ-White 
cursors. Configure Conky with the https://raw.github.com/nicolargo/ubuntupostinstall/master/conkyrc 
configuration file.

You can NOT use both unity and gnome3 section in the same .cfg file.

### postactions section

This is the last section of the configuration file.

A line starting with the action_ string (following by the action name) defines a 
action (command line) to be executed.

The action name will be displayed during the script execution.

The lines will be executed after all the others steps.

Example:

    [postactions]
    action_dummy = dpkg -l > /tmp/pkg-after.txt

The dummy action will create a /tmp/pkg-before.txt with a listing of 
all the packages installed on your system after the script execution.

## Contribuer ?

Besoin d'une nouvelle fonction ? 

Bug trouvé ?

Remplir une issue ici : https://github.com/nicolargo/ubuntupostinstall/issues/new
