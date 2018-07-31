# Correction de code HTML dans les ressources d'une application Windows obsolète

Article écrit le 30 juillet 2018 par [oktomus](http://oktomus.com).

Je vais vous expliquer comment je me suis débrouillé pour corriger un problème bloquant sur un vieux logiciel Windows obsolète.

## Tout d'abord, un peu de contexte

Il y a quelques années, j'ai acquis une montre GPS avec laquelle un logiciel non-libre est fournis — c'est une montre *ASCOS* accompagnée de *GPS Master v2.0.14*. Ce dernier utilise l'*API Google Maps* pour afficher les différents trajets réalisés. Voilà à quoi ça ressemble quand ça fonctionne:

<img src="screen_success_gps_master.jpg" width="761" height="591">

Il y a plein de choses que je reproche à cet outils mais l'essentiel des fonctionnalités est là. En revanche, ce logiciel n'est plus mis à jour depuis plusieures années et est devenu inutilisable à cause de problèmes de compatibilité. On a une pop-up d'erreur qui apparaît à chaque interaction et la carte n'est plus là:

<img src="screen_fail_gps_master.jpg" width="765" height="591">

## Pourquoi ça pète !?

C'est une application native qui utilise une *webview*, une instance d'*Internet Explorer* qui tourne dans le logiciel — argh. Le problème courant est que ces *webview* ont tendance à utiliser *Internet Explorer 7* au lieu de la dernière version disponible sur votre PC (aujourd'hui *IE 11*). Et bien évidement, *IE 7* n'est pas compatible avec l'*API* de *Google Maps*.

Plus d'informations dans la [documentation dev de Google](https://developers.google.com/maps/documentation/javascript/error-messages#unsupported-browsers).

## Ajouter une balise de compatibilité 

Pour régler ce souçis de compatibilité, il suffit d'ajouter une balise `meta` dans le document *HTML* utilisé pour afficher map dans la *webview*. Cette ligne permet de forcer la version d'*IE* à utiliser:

```
<meta http-equiv="x-ua-compatible" content="IE=edge">
```

Le plus compliqué reste à trouver comment ajouter ce morceau de code dans le logiciel. Dans le cas de *GPS Master* (et de beaucoup d'autres logiciels), vous pouvez voir en ouvrant *GPS Master.exe* avec un éditeur qu'il y a de l'*HTML* directement lisible (assurez vous de faire une copie pour ne pas faire de bétises). 

Un logiciel est un ensemble de ressources et ici on veut modifié toutes ses ressources HTML. Cela est possible et très facile en utilisant un éditeur de ressources Windows comme [*Resource Hacker*](http://http://angusj.com/resourcehacker/). Ce dernier permet d'ouvrir l'éxécutable, de modifier son contenu et de créer un nouvel éxécutable. Vous allez donc ouvrir le fichier *GPS Master.exe* puis ajouter la ligner ci-dessus dans tous les blocs `head` *HTML*. Si un ligne existe déjà (par exemple: `<meta http-equiv="x-ua-compatible" content="IE=EmulateIE7">`, remplacez là).

Vous pouvez ensuite compiler les fichiers modifiés (`F5`) et exporter votre nouvel éxécutable (`File > Save As`). En lancant ce dernier, vous n'aurez plus de soucis !

<img src="screen_resource_hacker.jpg" width="959" height="744">

