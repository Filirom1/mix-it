# Devops chez Kelkoo

Collaboration entre les devs et les ops.

* industrialisation du process de dev
* industrialisation de l'infra
* faciliter la comm entre les deux

## Contexte

### 1999:

Déploiement manuel de tar.gz
10 serveurs
10+ DB
1 composant

### 2004: rachat par Yahoo

Outils Yahoo

### 2008: Indépendance.

Mix des outils
150 serveurs physiques et autant en virtuel.

## Conception du projet

OS:
* cloner
* configurer

Application:
* développer
* packager
* configurer
* déployer

## Puppet

Brique principale utilisée pour l'OS et l'application au niveau de la gestion de la conf et des déploiements.

Puppet: propre langage (manifest), une sorte de petit langage de script.
A l'époque choix de puppet face à BCFG2 et chef n'existait pas encore.

## OS: Cloner Configurer

Installer des serveurs de manière identique et répétable.
Outil: cobbler pour le déploiement de l'image.

Et puppet pour la configuration.

Puppet configuré de façon centralisé:

* ssh
* user
* snmp
* repos yum
* resaux
* dns
* ...

## Développer / Packager

Maven en dehors de l'utilisation courante permet aussi:

* génération de composants (archetype) :
  * processing
  * webapp
  * schema DB
  * plugin de monitoring
  * web services (création de la structure de répertoire + squelette de
    code)
* génération de rpm
  * sources
  * libs
  * script de lancement
  * fichier de context (webapp tomcat)
  * crontab
  * ...
* deuxième rpm pour de déploiement (puppet)
  * manifest puppet
  * valeur par defaut
  * template

## Plugin maven RPM kelkoo

pom.xml on rajoute le type de composant, plus d'autres infos.

Les composants sont versionnées et les versions sont insérées dans le RPM.

On gère les dépendances avec RPM

## Configuration

clé: valeur -> trop complexe à gérer

Ajout de la notion de rôle, que l'on va déployer sur les serveurs
physique.

## Geppetto : Backoffice de configuration

Dans puppet notion d'héritage est limitée.

Geppetto

Sur un ensemble de serveurs quel composants on veut deployer et quel
clé/valeur on veut deployer ?

Les serveurs vont hériter de rôles.

On peut faire de l'héritage sur les rôles.

A partir de l'ensemble des rôles on génère un seul fichier pour puppet
qu'il faut déployer.

## Punch

Outil en CLI de déploiement.

Restore: installe sur le serveur tout ce qui est défini dans le
backoffice

Apply: installe sans passer par le serveur

Wrapper autours de puppet.

Etape:

* récupère conf sur le back office (2 composants)
* installe les rpms de déploiement sur la machine
* calcul des changements
* désinstallation des vieux composants
* installation des nouveaux composants

## Etapes de mise en prod

Le dev génère les archetypes avec de développer.
Dev et commit sur SVN.
Intégration continue sur Jenkins
Sur Jenkins création des packages
Dépos des composants sur un repo YUM privé

Sysadmin se log sur Geppetto, choisi les composants à deployer sur le
serveur.

Sur le serveur: punch restore

Le serveur récupère les composants sur le repo YUM privé.


## Utilisation locale

Pas de repo Yum distant, pas d'intégration continue.

Simuler Geppetto en donnant le fichier généré.

Utiliser punch en lui spécifiant le fichier en paramètre.

Le dev va faire exactement la même chose que les ops pour installer un
composant.

Tous le monde utilise le même outillage.

## Chez Kelkoo

Les outils de monitoring sont créé de la même façon.

Concernant la conf des OS, Geppetto n'est pas utilisé mais un outil
proche: Kermit

## DevOps

industrialisation du dev : OK

Comm entre les équipes: les mêmes outils sont utilisés en dev et en op
-> même langage

Industrialisation de l'infra : in progress
 * blue/green: 2 fois la prod. ET on balance de l'un à l'autre
 * infrastructure as a service: net yet

## Les options possibles pour y arriver soit même

Si on a du temps : Construire sa solution (mode Kelkoo), utilisation de paquet système, un
vrai plus.

Si on a de l'argent :  DeployIT ou Nolio (mais
difficile pour utiliser les outils en dev)

Gérer des images de déploiement. Préfabriquer l'image OS

S'adapter au PAAS : Google App Engine,


## DevOps & Scrum chez Kelkoo

On désigne une personne de l'op à participer au scrum. Mais beaucoup
d'activité au quotidien, difficile de se libérer.

Service architect, plus dispo, mais il manque de service architect.

Le service architect est présent à un certain nombre de scrum mais pas
tous.
