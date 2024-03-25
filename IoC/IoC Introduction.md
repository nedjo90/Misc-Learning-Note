
 Le principe d'IoC (inversion de control) recommande l'inversion de différents type de contrôle dans une architecture orienté objet afin d'obtenir un couplage faible entre les classes d'application. Le contrôle fait référence à toutes les responsabilités supplémentaires qu'une classe a, autre que sa responsabilité principale, telles que le contrôle du flux d'une application ou le contrôle de la création et de la liaison des objets dépendants ( rappel: SRP - Principe de Responsabilité Unique). Il est obligatoire si l'on souhaite faire du TDD.


Différence entre `Principe de Conception` ==(Design Principle)== et `Modèle de Conception` ==(Design Pattern)==

### Design Principle

C'est une méthode de haut niveau pour la conception de meilleurs logiciels.
Il ne fournit pas de méthodes d'implémentation et n'est pas limité à un language.
Le principe SOLID (SRP, OCP, LSP, DIP) est l'un des principes de conception le plus connu.

Example:

Le principe de responsabilité unique (SRP - Single Responsability stipule qu'une classe doit avoir une seule raison de changer. C'est un constat de haut niveau qu'il faut garder en tête lorsque l'on conçoit ou créer une classe. SRP ne fournit pas d'implémentation spécifique.


### Design Pattern

C'est une solution d'implémentation de bas niveau, communément retrouvé dans des problématiques en orienté objet. En d'autres termes, il suggère une manière spécifique d'implémentation pour une problème spécifique en programmation orienté objet.

Example:

Si on souhaite créer une classe qui ne peut avoir qu'une seule instance à la fois, on peut utiliser le modèle conceptuel du `Singleton` qui suggère la meilleur manière d'implémenter de ce type de classe.

![[Screenshot 2024-03-25 at 11.56.34.png]]


### Dependancy Inversion Principle (DIP)

Le principe d'inversion de dépendance permet également d'obtenir un couplage faible entre classes. Il est recommander d'utiliser IoC et DIP ensemble pour obtenir un couplage faible.

Le DIP suggère les modules de haut niveau ne doivent pas dépendre de modules de bas niveau. Les deux doivent dépendre d'abstraction.

Le DIP est un modèle conceptuel qui implémente le principe de IoC pour inverser la création d'objets dépendant.

### IoC Container

L'IoC Container est un environnement qui gère automatiquement les injection de dépendance à travers l'application, nous permettant de pas avoir besoin d'y consacrer à son implémentation.

Nous ne pouvons pas atteindre un faible couplage uniquement avec IoC. Nous aurons besoins pour cela d'utiliser DIP, DI et IoC container. La figure ci-dessous illustre comment nous allons réaliser une architecture à faible couplage étape par étape dans les sections à venir.

![[Screenshot 2024-03-25 at 13.39.57.png]]

