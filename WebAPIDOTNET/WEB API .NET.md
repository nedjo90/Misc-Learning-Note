---
share: "true"
---

	
```cs
WebApplication builder = WebApplication.CreateBuilder(args);

WebApplication app = builder.Build();

app.UseHttpsRedirection();

app.run();
```


# Qu'est-ce qu'une Web API et pourquoi en créer une ?

Une Web API (Application Programming Interface) est une interface de programmation applicative qui permet à différents composants logiciels de communiquer entre eux. Dans une application, nous pouvons identifier généralement trois composants principaux : l'interface utilisateur, la logique métier et la base de données. Ces applications sont utilisées dans une multitude d'environnements tels que les ordinateurs, les smartphones, les SmartTVs, les montres connectées, etc. Chacun de ces environnements présente des contraintes spécifiques en termes d'outils de développement, ce qui nécessite souvent le développement de solutions adaptées à chaque plateforme.

L'intérêt de créer une Web API réside dans la possibilité de découpler les différents composants de l'application. En isolant la logique métier et la base de données dans une API accessible via le réseau, nous pouvons centraliser la gestion des données et de la logique métier sur des serveurs, indépendamment des contraintes d'environnement. Ainsi, les clients, qu'ils soient des applications web, mobiles ou autres, peuvent accéder aux fonctionnalités de l'application via cette interface standardisée. Cela permet une meilleure réutilisation du code, une simplification du développement pour différentes plateformes et une évolutivité accrue de l'application. En résumé, créer une Web API facilite la création d'applications multiplateformes et la communication entre celles-ci, offrant ainsi une expérience utilisateur homogène et cohérente.

Les API sont créées pour deux types de scénarios :

1. Partager des données, telles que des informations provenant d'une base de données.
2. Partager des fonctionnalités, représentant la logique métier de l'application.

Peu importe ce qui est partagé, ces échanges se font généralement sous forme de fonctions. Ces fonctions sont accessibles via Internet, habituellement via le protocole HTTP. Les Web API permettent à ces fonctions d'être accessibles à distance, souvent hébergées sur un serveur distant. En résumé, les Web API sont des fonctions hébergées à distance sur le web, accessibles via des requêtes HTTP, qui permettent de partager des données et des fonctionnalités entre différentes applications.

# How Web API works (Theory)

Les fonctions typiques fournies par une API avec une base de données sont regroupées sous l'acronyme CRUD, qui signifie :

- Create (Créer)
- Read (Lire)
- Update (Mettre à jour)
- Delete (Supprimer)

Une Web API est connectée à un serveur qui est constamment à l'écoute d'un port, en attente de requêtes envoyées par les applications clientes. Lorsqu'elle reçoit une requête, elle la transmet à la Web API qui l'interprète et fournit une réponse correspondante. Ces requêtes sont généralement effectuées via le protocole HTTP.

# Comment savoir quelle fonctionnalité doit être exécutée lors de la réception de requêtes http ?

Lors de la réception de requêtes HTTP, la Web API détermine quelle fonctionnalité doit être exécutée en fonction de la méthode de requête (verbe) et de son contenu. Ce processus est appelé routage (routing).

Une requête HTTP est composée d'une section d'en-tête (header) et d'un corps (body). Dans l'en-tête, différents types de méthodes de requête (verbes) peuvent être spécifiés, notamment :

- POST (Create) : utilisé pour créer de nouvelles ressources.
- GET (Read) : utilisé pour récupérer des données existantes.
- PUT/PATCH (Update) : utilisé pour mettre à jour des données existantes.
- DELETE (Delete) : utilisé pour supprimer des données existantes.

Lorsqu'une requête arrive sur le port d'écoute du serveur de l'API, la première tâche de l'API est d'orienter la requête vers la bonne fonction en fonction de la méthode de requête spécifiée. Ce processus de détermination de la fonction à exécuter en fonction de la méthode de requête entrante est essentiel pour le fonctionnement correct de l'API.

```cs
//si notre api recoit une requete de type get sur le chemin (endpoint) /shirts il retourne la fonction :  
  
app.MapGet("/shirts", () => "Reading all the shirts");  
  
// si notre api recoit une requete de type get sur le chemin (endpoint) /shirts/{id} il retourne la fonction:   
app.MapGet("shirts/{id}", (int id) => $"Reading shirt with ID: {id}");  
  
// si notre api recoit une requete Post sur le chemin (endpoint) /shirts il retourne la fonction:  
app.MapPost("/shirts", () => "Creating a shirt.");  
  
// si notre api recoit une requete Put sur le chemin (endpoint) /shirts il retourne la fonction:  
app.MapPut("/shirts/{id}", (int id) => $"Updating shirt with ID: {id}");  
  
//si notre api recoit une requete Delete sur le chemin (endpoint) /shirts/{id} il retourne la fonction:  
app.MapDelete("/shirts/{id}", (int id) => $"Deleting shirt with ID: {id}");
```

La fonction `Map` (Cartographier) est utilisée dans ASP.NET Core pour associer des chemins (endpoints) à des fonctions de gestion des requêtes. Cette fonction permet de définir des routes pour différents types de requêtes HTTP (GET, POST, PUT, DELETE) et de spécifier quelles fonctions doivent être exécutées lorsque ces routes sont accédées.

En utilisant Map, vous pouvez associer un chemin spécifique à une ou plusieurs méthodes HTTP et définir les actions à entreprendre lorsque ces routes sont atteintes. Par exemple, vous pouvez utiliser MapGet (CartographierObtenir) pour associer une fonction à une route qui répondra uniquement aux requêtes GET, et MapPost (CartographierPoster) pour associer une fonction à une route qui répondra uniquement aux requêtes POST.

Lorsque votre application reçoit une requête, ASP.NET Core parcourt les routes définies avec Map et détermine laquelle correspond à la requête entrante en fonction du chemin et de la méthode HTTP spécifiés. Ensuite, la fonction associée à cette route est exécutée pour répondre à la requête.

Definition 
`Endpoint`
```md
Un "endpoint" est un point d'accès spécifique dans une API (Interface de Programmation d'Application) où les clients peuvent interagir avec les fonctionnalités fournies par cette API. Il représente une URL particulière à laquelle les requêtes HTTP peuvent être adressées pour effectuer des opérations spécifiques, telles que récupérer, créer, mettre à jour ou supprimer des données. Chaque endpoint est associé à une action ou à une fonction particulière dans l'API, déterminant ainsi le comportement de la requête et de la réponse associée. En résumé, un endpoint est un point de terminaison distinct dans une API qui définit les interactions possibles entre les clients et les services offerts par cette API.
```

## What is a Web API Framework

Je m'excuse pour cela. Voici la version révisée en utilisant les mots anglais normalement et le français entre parenthèses :

Rappel : une API (Application Programming Interface) a pour fonction principale de donner accès à distance à des fonctions.

Le développeur a pour principal objectif de développer ces fonctions.

Un Framework (Cadre) d'une API Web va venir gérer tout ce qui est autour de l'API 
`(API = client -> requête -> routage -> fonction -> réponse -> client)`

Parmi les fonctions du Framework, nous retrouverons :

- L'authentification et l'autorisation qui interviendront avant le routage.
- Le Model Binding (Liaison de Modèle) qui interviendra après le routage. Les fonctions mises à disposition par l'API ont besoin, pour être exécutées, des informations envoyées par la requête HTTP. Ci-dessous, `{id}` qui sera envoyé en paramètre de la fonction est géré par le Framework.

```cs
app.MapDelete("/shirts/{id}", (int id) => $"Deleting shirt with ID: {id}");
// ici c'est un entier mais ça aurait pu être un objet
```

- La Model Validation (Validation de Modèle ), qui intervient après le Model Binding. Le principe est simple : nous ne pouvons pas faire confiance à l'utilisateur qui peut commettre une erreur ou être mal intentionné. On va ainsi tester et valider les données envoyées en paramètre des fonctions mises à disposition par notre API. Si les informations sont validées, alors nous les laisserons être à disposition des fonctions de l'API pour exécution.
- Le Exception Handling (Gestion des Exceptions), pendant l'exécution des fonctions de l'API, celle-ci doit être capable de gérer les exceptions.
- Le Result Formatting (formatage du résultat), il faut mettre en forme le résultat des fonctions de l'API pour qu'elle puisse renvoyer à l'utilisateur (client) une réponse dans une forme adaptée.

Résumé:

Une application (client) envoie une requête HTTP à une Web API. Cette requête passe par la phase d'authentification et d'autorisation, puis est routée vers les fonctions de l'API. Le model binding extrait de la requête les informations nécessaires à la fonction de l'API, puis valide ces données et exécute la fonction. Si une exception est levée, l'API gère cette exception. Ensuite, elle met en forme le retour de la fonction pour la réponse HTTP et enfin envoie la réponse à l'application (client).

Nous allons traiter en détail chacun de ses points.
![[Communication Client-Server.png]]
## Middleware Pipeline

Dans la terminologie Microsoft chacune des étapes vu précédentes sont considérés comme des "Middleware component" et l'ensemble est appelé "Middleware Pipeline"

Dans le Framework ASP.NET: toutes les méthodes commençant par "Use" mise à disposition par l'objet "WebApplication" sont des middleware component (ci-dessous quelques exemples):

```cs
app.UseAuthorization(); // Gestion des autorisations
app.UseAuthentication(); // Gestion de l'authentification
app.UseRouting(); // Gestion du routage des requêtes
app.UseExceptionHandler(); // Gestion des exceptions
app.UseCors(); // Gestion des autorisations de ressources entre différents origines
app.UseStaticFiles(); // Servir des fichiers statiques (HTML, CSS, JavaScript, etc.)
app.UseSession(); // Gestion des sessions utilisateur
app.UseHttpsRedirection(); // Redirection HTTP vers HTTPS
app.UseResponseCompression(); // Compression des réponses HTTP pour une meilleure performance

```

Les middleware component agissent comme des filtres des requêtes entrantes, ce sont des gestionnaires de requêtes.

![[Middleware Pipeline.png]]

## ASP.NET CORE Web API

Il existe deux méthodes courantes pour créer des Web API avec ASP.NET Core :

1. **API Minimal :** Cette approche consiste à définir les endpoints (chemins) directement dans les méthodes `MapGet`, `MapPost`, etc., sans utiliser de contrôleurs. Bien que pratique pour des applications simples, elle peut devenir difficile à gérer avec un grand nombre de requêtes.
    
2. **Utilisation des Contrôleurs :** Avec cette approche, les contrôleurs sont utilisés pour organiser les endpoints de manière plus structurée. Un contrôleur est simplement une classe qui regroupe les endpoints liés à une certaine fonctionnalité ou ressource.
    

Jusqu'ici, nous avons examiné l'approche de l'API minimal. Cependant, l'un des défis rencontrés avec cette méthode est la gestion d'un grand nombre de requêtes.

Pour utiliser des contrôleurs, vous devez créer un dossier nommé "Controllers" qui contiendra toutes les classes de contrôleurs. Selon la convention, les noms de ces classes doivent suivre le format "NomRessourceController". Par exemple, un contrôleur pour gérer les utilisateurs pourrait être nommé "UserController".

Cette approche offre une meilleure organisation des endpoints et facilite la gestion des fonctionnalités de l'API.

Les contrôleurs d'une Web API possèdent deux caractéristiques principales :

1. Les classes de contrôleurs héritent de la classe ControllerBase.
2. Elles utilisent l'attribut C# `[ApiController]`.

```cs
using Microsoft.AspNetCore.Mvc;  
  
namespace WebAPI.Controllers;  
  
[ApiController]  
public class ShirtsController : ControllerBase  
{  
}
```

Nous pouvons maintenant mettre dans notre controller chacun des endpoints concernant notre contrôleur, ici "shirts"

voila la réorganisation de notre endpoint shirt dans le contrôleur: ShirtController

```cs
using Microsoft.AspNetCore.Mvc;  
  
namespace WebAPI.Controllers;  
  
[ApiController]  
public class ShirtsController : ControllerBase  
{  
    public string GetShirts()  
    {  
        return "Reading all the shirts";  
    }  
  
    public string GetShirtById(int id)  
    {  
        return $"Reading shirt: {id}";  
    }  
  
    public string CreateShirt()  
    {  
        return $"Creating a shirt";  
    }  
  
    public string UpdateShirt(int id)  
    {  
        return $"Updating shirt: {id}";  
    }  
  
    public string DeleteShirt(int id)  
    {  
        return $"Deleting shirt: {id}";  
    }  
      
}```

Rappel: les contrôleurs sont justes des organisateurs de endpoints

A présent que nous avons amené tous les méthodes dans le contrôleur, comment oriente-t-on (map) vers la bonne méthode les requêtes http entrante?



`Route`
```md
Une "route" dans le contexte du développement web et des API désigne un chemin spécifique ou une URL à laquelle une requête HTTP est adressée. Elle définit la correspondance entre une requête entrante et une action spécifique à exécuter dans l'application web. Les routes sont utilisées pour diriger les requêtes vers les bons gestionnaires de requêtes, souvent des contrôleurs ou des fonctions, qui sont chargés de traiter la demande et de retourner une réponse appropriée. En résumé, une route est un moyen de définir la manière dont les différentes actions et ressources d'une application web sont accessibles via des URL spécifiques.
```


 Cette procédure s'appelle ici "action method" (méthode d'action ou méthode de traitement)

Notre class de type "WepApplication" a une méthode MapControllers (assigne au controleur) qui va nous permettre d'orienter les requêtes entrantes vers les controllers.

```cs
app.MapControllers();
```

Il faudra ensuite orienter vers les fonctions voulu dans les controllers et pour cela il faut utiliser l'attribut `[Route("/chemin")]` (chemin/itinéraire)sur les fonctions et pour le type de requête l'attribut `[HttpTypeDeRequete]`

exemple:

```cs
[HttpGet]  //<= Http + verb
[Route("/shirts")]  // <= url de la requête
public string GetShirts()  
{  
    return "Reading all the shirts";  
}
```

Nous avons donc pu remplir les mêmes fonctions d'une api minimal avec:
-> MapControllers qui orient la requête entrante vers nos controllers
-> `[Route("/shirts")]` qui spécifie le chemin de la requête (url) (chemin/itinéraire)
-> `[HttpGet]` qui spécifie le type de requête

```cs
using Microsoft.AspNetCore.Mvc;  
  
namespace WebAPI.Controllers;  
  
[ApiController]  
public class ShirtsController : ControllerBase  
{  
    [HttpGet]  
    [Route("/shirts")]  
    public string GetShirts()  
    {  
        return "Reading all the shirts";  
    }  
      
    [HttpGet]  
    [Route("/shirt/{id}")]  
    public string GetShirtById(int id)  
    {  
        return $"Reading shirt: {id}";  
    }  
  
    [HttpPost]  
    [Route("/shirts")]  
    public string CreateShirt()  
    {  
        return $"Creating a shirt";  
    }  
  
    [HttpPut]  
    [Route("/shirts/{id}")]  
    public string UpdateShirt(int id)  
    {  
        return $"Updating shirt: {id}";  
    }  
  
    [HttpDelete]  
    [Route("/shirts/{id}")]  
    public string DeleteShirt(int id)  
    {  
        return $"Deleting shirt: {id}";  
    }  
}
```

Cependant, Il manque des configurations nécessaire au bon fonction de notre MapControllers.

Nous allons ajouter aux services de l'objet WebApplication la méthode AddControllers.

Que fait exactement AddControllers ?

Lorsque vous appelez `AddControllers()`, ASP.NET Core configure divers services pour prendre en charge les contrôleurs dans votre application :

- Configuration de la prise en charge de l'acheminement des requêtes HTTP vers les contrôleurs appropriés.
- Activation de la gestion de la sérialisation et de la désérialisation des données JSON pour les actions des contrôleurs. Cela signifie que les contrôleurs peuvent recevoir des objets JSON dans les requêtes et retourner des objets JSON dans les réponses.
- Configuration des autres fonctionnalités de base nécessaires au fonctionnement des contrôleurs, telles que la validation des modèles et la gestion des erreurs.

En résumé, `builder.Services.AddControllers()` ajoute les services nécessaires pour activer le support des contrôleurs dans votre application ASP.NET Core, permettant ainsi de créer des API Web et des endpoints pour gérer les requêtes HTTP.

```cs
WebApplicationBuilder builder = WebApplication.CreateBuilder(args);  

// Add services to the container  
builder.Services.AddControllers();  

WebApplication app = builder.Build();  
  
app.UseHttpsRedirection();  

//map the request to the controllers
app.MapControllers();

app.Run();
```

Nous avons utiliser l'attribut `[Route("/chemin)]` pour définir le chemin de chacune des fonctions de notre controller.
Une autre de manière de faire et de mettre l'attribut `[Route("[controller]")]` sur la classe controller. Grâce au nommage il identifiera le chemin et nous aurons plus qu'à définir les arguments de la fonction à travers  l'attribut `[HttpType]`, exemple => `[HttpGet("{id}")]`.

Afin d'être plus organiser nous ajouterons une base a notre chemin de routing `[Route("api/[controller]")]`

```cs
using Microsoft.AspNetCore.Mvc;  
  
namespace WebAPI.Controllers;  
  
[ApiController]  
[Route("api/[controller]")]  
public class ShirtsController : ControllerBase  
{  
    [HttpGet]  
    public string GetShirts()  
    {  
        return "Reading all the shirts";  
    }  
      
    [HttpGet("{id}")]  
    public string GetShirtById(int id)  
    {  
        return $"Reading shirt: {id}";  
    }  
  
    [HttpPost]  
    public string CreateShirt()  
    {  
        return $"Creating a shirt";  
    }  
  
    [HttpPut("{id}")]  
    public string UpdateShirt(int id)  
    {  
        return $"Updating shirt: {id}";  
    }  
  
    [HttpDelete("{id}")]  
    public string DeleteShirt(int id)  
    {  
        return $"Deleting shirt: {id}";  
    }  
}
```

Notre url est donc /api/shirts.

## Model Binding

Qu'est ce que le model binding ?

Le model binding est le processus de mapping (assignation) des données de la requête HTTP aux paramètres de la méthode de traitement (action method). Cela permet de récupérer les données de la requête et de les associer directement aux paramètres de la méthode du contrôleur, simplifiant ainsi leur utilisation dans le code de l'application.

Exemple:

```cs
[HttpGet("{id}")]
public string GetShirtById(int id)  
{  
    return $"Reading shirt: {id}";  
}
```

Dans cet exemple, `[HttpGet("{id}")]` correspond à la route (chemin / itinéraire) de la requête HTTP. Le paramètre `id` de type entier dans la méthode d'action `public string GetShirtById(int id)` est associé à cette route via le processus de model binding. Ainsi, lorsque la requête HTTP est reçue avec un identifiant spécifique dans l'URL, ce paramètre est automatiquement peuplé avec cette valeur, ce qui permet à la méthode de récupérer et de traiter les données en fonction de cet identifiant.

Pour mieux comprendre nous allons voir comment est stockée la requête http.

![[http request description.png]]

- POST => le type de requête (verb), la méthode.
- /advisor/selectBeerTaste.do => le chemin (path/route) (chemin/itinéraire)
- Host: (...) keep-alive => l'en-tête de la requête (header)
	- il y a plusieurs pair de clé - valeur (key value pair):
		- Host(key) : (value)
		- Accept(key) : (value)
		- Accept-Encoding(key) : (value)
		- Accept-Charset (key) : (value)
		- Keep-Alive (key) : (value)
		- Connection (key) : (value)
- Color=dark&taste=malty => le corps de la requête (body / payload)
	- un corps fait sens avec les méthodes post, put et patch, il sera souvent ignoré pour delete et get
Les données viennent de l'ensemble de la requête http sauf de la méthode (verb)

Exemple:

Nous voulons en plus de l'id de shirt ça couleur :

```cs
[HttpGet("{id}/{color}")]  
public string GetShirtById(int id, string color)  
{  
    return $"Reading shirt: {id}, color {color}";  
}
```

![[Postman 1.png]]

Cette exemple illustre que les données peuvent être mappé (assigné) à partir du route (chemin/itinéraire).

Nous pouvons spécifié d'où vient la donnée, ainsi si la donnée ne provient pas du route (chemin/itinéraire) la méthode action va lever une exception.
Pour cela on donne l'attribut `[FromRoute]` au paramètre concerné:

```cs
[HttpGet("{id}/{color}")]
public string GetShirtById(int id,[FromRoute] string color)  
{  
    return $"Reading shirt: {id}, color: {color}";  
}
```

La donnée peut également venir d'une 'Query' (requête). On utilisera alors sur le paramètre de la méthode action l'attribut `[FromQuery]`:

```cs
[HttpGet("{id}")]  
public string GetShirtById(int id,[FromQuery] string color)  
{  
    return $"Reading shirt: {id}, color: {color}";  
}
```

Pour précis le query la requête ce construira de cette manière:

```http
GET http://localhost:5253/api/shirts/9?color=blue
```

On précise le paramètre de notre query à l'aide du `?` `color=` (le nom du paramètre) `value` (valeur envoyé en paramètre).

![[Http request with FromQuery.png]]

La donnée peut venir de l'entête (header):

```cs
[HttpGet("{id}")]  
public string GetShirtById(int id,[FromHeader(Name = "Color")] string color)  
{  
    return $"Reading shirt: {id}, color: {color}";  
}
```

L'attribut `[FromHeader(Name = "Color")]` prend en paramètre une clé (key) ici `Name` précise nom de la clé qui est notre paramètre `"Color"`.

![[Http request with FromHeader.png]]

Nous avons donc vu que les données peuvent venir de:
- route avec [FromRoute] (chemin/itinéraire)
- query avec [FromQuery]
- header avec [FromHeader]

En général c'est à partir de ces endroits que nous faisons la liaison des données (binding data).

Nous pouvons faire une liaison des données à partir du corps (body) de la requête pour des données plus complexe.

Pour cela nous utilisons en général la méthode `Post` ou `Put`.

Cette manière permet de faire une liaison de donnée (binding data) plus complexe est le model binding ou liaison des modèles. 
Cela fait référence au processus par lequel les données fournies dans une requête HTPP sont automatiquement associées à des objets de modèle dans une application web, généralement dans le but de les utiliser dans le traitement de cette requête.

Afin de bien organiser notre code nous allons créer une classe avec le nom de notre `route` (chemin/itinéraire) qui va prendre cette responsabilité, placé dans un dossier `Models`.

Nous aurons donc une arborescence qui se présentera comme ceci:
![[Project tree.png]]

Avec notre classe qui pourrait être déclarée de cette manière:

```cs
namespace WebAPI.Models;  
  
public class Shirt  
{  
    public int ShirtId { get; set; }  
  
    public string? Brand { get; set; }  
  
    public string Color { get; set; }  
  
    public int Size { get; set; }  
  
    public string? Gender { get; set; }  
  
    public double Price { get; set; }  
}
```


Notre méthode de liaison des données (controller) aura pour attribut `[FromBody]`

```cs
[HttpPost]  
public string CreateShirt([FromBody]Shirt shirt)  
{  
    return $"Creating a shirt";  
}
```


Nous pourrions à ce moment faire une liaison de donnée à partir du body de la requête de cette manière:

```http
POST http://localhost:5253/api/Shirts  
Content-Length: 112
Content-Type: */*; charset=UTF-8
Connection: Keep-Alive
User-Agent: Apache-HttpClient/4.5.14 (Java/17.0.9)
Accept-Encoding: br,deflate,gzip,x-gzip
  
{  
	  "ShirtId": 1,
	  "Brand": "MyBrand",
	  "Color": "green",
	  "Size": 40,
	  "Gender": "Male",
	  "Price": 99.99
}
```

Et voici la réponse de  notre programme à cette requête

```http
HTTP/1.1 200 OK
Content-Type: text/plain; charset=utf-8
Date: Fri, 15 Mar 2024 10:38:30 GMT
Server: Kestrel
Transfer-Encoding: chunked

Creating a shirt

Response code: 200 (OK); Time: 6ms (6 ms); Content length: 16 bytes (16 B)
```

Sur Postman:
![[Postman 2.png]]

Egalement nous pouvons passer le type de donnée Form avec l'attribut `[FromForm]`.

Les données de type Form sont formé avec la paire clé-valeur (key-value pair).


```cs
[HttpPost]  
public string CreateShirt([FromForm]Shirt shirt)  
{  
    return $"Creating a shirt";  
}
```

la requête sur Postman ressemblera alors à ceci:

![[Postman 4.png]]

De manière brut voici la requête:

```http
POST /api/shirts HTTP/1.1  
User-Agent: PostmanRuntime/7.37.0  
Accept: */*  
Cache-Control: no-cache  
Postman-Token: dd788486-ede3-4b3d-bc90-4e4b0447b915  
Host: localhost:5253  
Accept-Encoding: gzip, deflate, br  
Connection: keep-alive  
Content-Type: multipart/form-data; boundary=--------------------------231181920181382574095771  
Content-Length: 601  
  
----------------------------231181920181382574095771  
Content-Disposition: form-data; name="ShirtId"  
  
2  
----------------------------231181920181382574095771  
Content-Disposition: form-data; name="Brand"  
  
Your brand  
----------------------------231181920181382574095771  
Content-Disposition: form-data; name="Color"  
  
Yellow  
----------------------------231181920181382574095771  
Content-Disposition: form-data; name="Size"  
  
38  
----------------------------231181920181382574095771  
Content-Disposition: form-data; name="Price"  
  
30.50  
----------------------------231181920181382574095771--  
```

On voit bien que l'on donne pour chaque données une disposition `Content-Disposition`
qui est `form-data`, on fait appelle a `name=`pour définir notre clé et enfin on donne la valeur.
On remarque que cette donnée passe également par la corps de la requête.

Definition

`Model Binding`
```md
La "liaison de modèle" (model binding) est un processus dans le développement d'applications web où les données fournies par l'utilisateur, souvent via une requête HTTP, sont automatiquement associées à des objets de modèle dans le code de l'application. Cette liaison permet de convertir les données entrantes dans un format compatible avec les objets de modèle définis dans l'application, facilitant ainsi leur utilisation dans le traitement des requêtes et la manipulation des données. En résumé, le model binding permet de relier les données de la requête aux structures de données internes de l'application, simplifiant ainsi le développement et la gestion des interactions utilisateur.
```

Nous avons pu voir plusieurs manière d'assigner des données `(map data)` par la liaison des données `(model binding)` de la requête vers les paramètres à travers du:
- Route (chemin/itiniéraire)
- Query (paramètres de données envoyées dans la requête)
- Header (l'en-tête)
- Body (Le corps)
- Form (formulaire)

Il existe d'autres manières de faire liaison de données (data bindnig), celles-ci représentent l'essentiel et particulièrement celles utilisaient pour les données.

# Model Validation
`

Nous avons vu que les liaisons de modèle (model binding) intervient avant les méthodes de traitement, elles ont la responsabilités de fournir les données en paramètre de la méthode de traitement.

Cependant que ce passe-t-il dans le cas d'une erreur dans les données ? Qui va filtrer cette donnée ? Comment cette donnée sera filtrée ?

C'est le développeur qui dans la méthode de traitement va analyser et valider ou rejeter les données envoyées en paramètre. Ceci peut-être fait manuellement mais asp.net core 8.0 fournit des fonctionnalités permettant de gérer cette situation. Ces fonctionnalités sont appelés Model Validation.

Defintion 

`Model Validation`
```md
La "validation de modèle" (Model Validation) est un processus dans le développement d'applications web où les données entrantes sont vérifiées pour s'assurer qu'elles respectent les règles et les contraintes définies pour un modèle de données spécifique. Cette validation est généralement effectuée avant que les données ne soient utilisées ou enregistrées dans la base de données de l'application.

La validation de modèle vise à garantir que les données fournies par l'utilisateur ou le système externe sont valides, complètes et cohérentes avec les attentes de l'application. Elle peut inclure la vérification des formats, des types de données, des contraintes de longueur, des valeurs autorisées, ainsi que des règles métier spécifiques à l'application.

En cas d'échec de la validation, des erreurs sont généralement renvoyées à l'utilisateur, lui indiquant les problèmes rencontrés avec les données saisies. Cela permet de garantir l'intégrité des données et d'améliorer la qualité et la fiabilité de l'application.
```


Dans le Model Validation fournit par ASP.NET core, ce traitement intervient entre la liaison des modèle (Model Binding) et la méthode de traitement des données (Method Action).

![[Model schema.png]]

Un des manières de faire ce traitement de la validation de model est l'annotation des données (data annotation).

Pour annoter nos données, nous allons donner des attributs aux propriétés des objets de nos models comme `[Required]` lorsque l'on veut rendre obligatoire ou rendre nullable nos propriétés dans le cas contraire à l'aide du symbole `?` .

```cs
using System.ComponentModel.DataAnnotations;  
  
namespace WebAPI.Models;  
  
public class Shirt  
{  
    [Required]  
    public int ShirtId { get; set; }  
  
    [Required]  
    public string? Brand { get; set; }  
  
    [Required]  
    public string Color { get; set; }  
  
    public int? Size { get; set; }  
  
    [Required]  
    public string? Gender { get; set; }  
  
    public double? Price { get; set; }  
}
```


C'est annotations sont très simples, elles ne changent pas la nature des classes.
Lorsque l'on donne ces attributs à des propriétés aux objets utilisés en paramètre de nos méthodes de traitement (action method), asp.net va automatiquement utiliser ces annotations pendant l'étape de validation de modèle pour valider les données envoyées par la requêtes.


Si nous effectuons la requêtes ci-dessous
(Requête en brut)
```http
POST /api/shirts HTTP/1.1  
Content-Type: application/json  
User-Agent: PostmanRuntime/7.37.0  
Accept: */*  
Cache-Control: no-cache  
Postman-Token: 6888d88a-57c7-4dd5-be66-5642d7a3f08c  
Host: localhost:5253  
Accept-Encoding: gzip, deflate, br  
Connection: keep-alive  
Content-Length: 20  
  
{  
  "ShirtId": 1}  
  
###
```

(Requête sur Postman)
![[Postman 3.png]]

Voici la réponse que l'on reçoit:

```json
{

"type": "https://tools.ietf.org/html/rfc9110#section-15.5.1",

"title": "One or more validation errors occurred.",

"status": 400,

"errors": {

"Brand": [

"The Brand field is required."

],

"Color": [

"The Color field is required."

],

"Gender": [

"The Gender field is required."

]

},

"traceId": "00-791a358b91e83d345cb9afe396a684bb-1f1b5c081232db67-00"

}
```

- une ou plusieurs erreurs de validations ont été relevées
- Brand est requis
- Color est requis
- Gender est requis
	- le code d'erreur retourné est `400`qui signifie mauvaise requête `(Bad Request)`

Cette réponse a été générée par le modèle de validation du framework asp.net web api en format json qui a été exécuté juste avant la méthode de traitement (action method) et l'a donc empêchée.
En cas de validation nous aurions en réponse le retour de la méthode de traitement. 
Mettons en forme la requête de manière à ce qu'elle soit acceptée par le modèle de validation.

```http
POST /api/shirts HTTP/1.1

Content-Type: application/json

User-Agent: PostmanRuntime/7.37.0

Accept: */*

Cache-Control: no-cache

Postman-Token: 21c0011d-cb9b-4eea-a4e1-d3bb62c97c87

Host: localhost:5253

Accept-Encoding: gzip, deflate, br

Connection: keep-alive

Content-Length: 87

{

"ShirtId": 1,

"Brand": "My Brand",

"Color": "Red",

"Gender": "Male"

}
```

![[Postman 5.png]]

# Voici la réponse:

```http
HTTP/1.1 200 OK

Content-Type: text/plain; charset=utf-8

Date: Fri, 15 Mar 2024 15:18:50 GMT

Server: Kestrel

Transfer-Encoding: chunked

Creating a shirt
```

![[Postman 6.png]]

Le code de réponse est `200` notre requête est donc passé par le modèle de validation et la réponse est bien la valeur de retour de notre méthode de traitement (action method)

```http
Creating a shirt
```

L'annotation des données est donc bien une manière de configurer en asp.net le modèle de validation. Ici nous avons utiliser uniquement l'attribut `[Required]`, il existe d'autres attributs qui permettent de faire de la validation des données. 
Explorons d'autres attributs: [Liste des attributs d'annotation des données](https://learn.microsoft.com/fr-fr/dotnet/api/system.componentmodel.dataannotations.requiredattribute?view=net-8.0)

# ValidationAttribute

L'annotation des données permet de valider une à la fois chaque attribut de la classe modèle.
Comment gérer une logique plus complexe qui requière la vérification de plusieurs propriétés à la fois ?

Dans cette situation nous pouvons créer notre propres attributs c'est ce qu'on appelle `ValidationAttribut`.

Imaginons ce scénario
=> pour les shirts pour hommes doivent être supérieurs à 8
=> pour les shirts pour femmes doivent être supérieurs à 6

Pour cela on va ce créer un dossier `Validations` qui va contenir tous les attributs que nous souhaitons créer où l'on créera une classe pour chacun des attributs.

Si un attribut est spécifique à un modèle nous utiliserons la convention de nommage:
`ModelName_WhatItDoAttribut` 

Cette classe héritera de la classe `: ValidationAttribute`

Enfin nous allons override la méthode IsValid.

```cs
protected override ValidationResult? IsValid(object? value, ValidationContext validationContext)
```

on s'assure que le paramètre ValidationContext est bien du même type que la classe qui l'invoque et si elle n'est pas null nous pouvons mettre à execution notre logique:

```cs
Shirt? shirt = validationContext.ObjectInstance as Shirt;
if (shirt != null)
{
	//notre logique
}
```

Exemple:

```cs
protected override ValidationResult? IsValid(object? value, ValidationContext validationContext)  
{  
    Shirt? shirt = validationContext.ObjectInstance as Shirt;  
    if (shirt != null && !string.IsNullOrWhiteSpace( shirt.Gender))  
    {  
        if (shirt.Gender.Equals("men", StringComparison.OrdinalIgnoreCase) && shirt.Size < 8)  
        {  
            return new ValidationResult("For men's shirt, the size has to be greater or equal to 8");  
        }  
        if (shirt.Gender.Equals("women", StringComparison.OrdinalIgnoreCase) && shirt.Size < 6)  
        {  
            return new ValidationResult("For women's shirt, the size has to be greater or equal to 6");  
        }  
    }  
    return ValidationResult.Success;  
}
```

Avec une requête valide (Gender = women && size >= 6):
![[Postman 7.png]]

Avec une requête invalide (Gender = women && size < 6):
![[Postman 8.png]]
# Web API return type


Jusqu'ici nos endpoints ont uniquement retourné des string. 
Comment retourner d'autres types ?

Pour simuler une base de données déclarons dans notre `ShirtController`:

```cs
List<Shirt> _listOfShirt = new List<Shirt>()  
{  
    new Shirt {ShirtId = 1, Brand = "adidas", Color = "blue", Gender = "men", Price = 9.99, Size = 10},  
    new Shirt {ShirtId = 2, Brand = "nike", Color = "orange", Gender = "men", Price = 19.99, Size = 11},  
    new Shirt {ShirtId = 3, Brand = "jenifer", Color = "pink", Gender = "women", Price = 29.99, Size = 6},  
    new Shirt {ShirtId = 4, Brand = "etam", Color = "white", Gender = "women", Price = 39.99, Size = 7}  
};
```


et modifier notre endpoint tel que:

```cs
[HttpGet("{id:int}")]  
public Shirt GetShirtById(int id)  
{  
    return _listOfShirt.First(x => x.ShirtId == id);;  
}
```

Voici le résultat de notre requête sur postman:

![[Postman 9.png]]

En faisant une requête sur l'id 2 nous recevons bien le shirt avec l'id 2 de notre base de données (liste).

Cependant, que ce passe t il si la requête est faites sur un id qui n'existe pas ?
![[Postman 10.png]]
 Nous avons un message d'erreur pas très compréhensible.
 
Afin d'y remédier nous allons mettre en place la gestion de cette erreur.

nous notre endpoint ne retournera plus un type (ici Shirt) mais `IActionResult`.

Nous pourrons ainsi retourner :

- Ok(`variable`) => 200 + variable en json
-  NotFound() => 404 "Not Found"

```cs
[HttpGet("{id:int}")]
public IActionResult GetShirtById(int id)  
{  
    Shirt? shirt =  _listOfShirt.FirstOrDefault(x => x.ShirtId == id);  
    if (shirt == null)  
    {  
        return NotFound();  
    }  
    return Ok(shirt);  
}
```

Voici la requête sur Postman
![[Screenshot 2024-03-19 at 18.41.06.png]]

Nous pouvons ajouter une autre valeur de retour dans le cas d'un argument non recevable, par exemple valeur négative pour notre endpoint
- BadRequest() => 400 "Bad Request"

```cs
[HttpGet("{id:int}")]  
public IActionResult GetShirtById(int id)  
{  
    if (id <= 0)  
        return BadRequest();  
    Shirt? shirt =  _listOfShirt.FirstOrDefault(x => x.ShirtId == id);  
    if (shirt == null)  
    {  
        return NotFound();  
    }  
    return Ok(shirt);  
}
```

Voici sur la requête Postman :
![[Screenshot 2024-03-19 at 18.40.22.png]]

A présent sur nos Web API nous privilégierons `IActionResult` comme valeur de retour de nos endpoints.
Grâce à cette méthode nos testes seront aussi plus simple à mettre en place.

# In Memory Repository

Dans la section précédente nous avons utilisé une liste dans notre controller pour simuler une base de donnée. En réalité, un il est préférable de créer un dossier dédier dans notre dossier model que nous nommerons `Repositories` pour le cas ou nous souhaitons stocker des données dans la mémoire de notre programme.

Ici nous stockons des données pour `Shirt` nous créerons une classe `ShirtRepository` pour stocker notre liste que nous mettrons en `static` pour y accéder sans instanciation.

Voici un aperçu de notre arborescence:

![[Screenshot 2024-03-19 at 23.19.41.png]]


et notre classe `ShirtRepository`
```cs
public static class ShirtRepository  
{  
    public static List<Shirt> ListOfShirt = new List<Shirt>()  
    {  
        new Shirt {ShirtId = 1, Brand = "adidas", Color = "blue", Gender = "men", Price = 9.99, Size = 10},  
        new Shirt {ShirtId = 2, Brand = "nike", Color = "orange", Gender = "men", Price = 19.99, Size = 11},  
        new Shirt {ShirtId = 3, Brand = "jenifer", Color = "pink", Gender = "women", Price = 29.99, Size = 6},  
        new Shirt {ShirtId = 4, Brand = "etam", Color = "white", Gender = "women", Price = 39.99, Size = 7}  
    };  
  
}
```

Il y a également deux autres méthodes que nous pouvons a présent implémenter ici:

```cs
public static bool ShirtExist(int id)  
{  
    return ListOfShirt.Any(x => x.ShirtId == id);  
}  
  
public static Shirt? GetShirtById(int id)  
{  
    return ListOfShirt.FirstOrDefault(x => x.ShirtId == id);  
}
```

Nous pourrons utiliser ces méthodes dans nos controller.

# Model Validation with Action Filter

Comme dans notre méthode de traitement ci-dessous, jusqu'ici nous avons validé les données directement dans nos méthodes, cependant ceci ne respect pas le principe d'une méthode une responsabilité.
```cs
public IActionResult GetShirtById(int id)  
{  
    if (id <= 0)  
        return BadRequest();  
    Shirt? shirt = ShirtRepository.GetShirtById(id);  
    if (shirt == null)  
    {  
        return NotFound();  
    }  
    return Ok(shirt);  
}```

Nous avons également effectuer une validation des arguments envoyés par la requête ==avant== le traitement des données (nos model `model binding`), comme ci-dessous:

```cs
  
public class Shirt_EnsureCorrectSizingAttribute : ValidationAttribute  
{  
    protected override ValidationResult? IsValid(object? value, ValidationContext validationContext)  
    {  
        Shirt? shirt = validationContext.ObjectInstance as Shirt;  
        if (shirt != null && !string.IsNullOrWhiteSpace( shirt.Gender))  
        {  
            if (shirt.Gender.Equals("men", StringComparison.OrdinalIgnoreCase) && shirt.Size < 8)  
            {  
                return new ValidationResult("For men's shirt, the size has to be greater or equal to 8");  
            }  
            if (shirt.Gender.Equals("women", StringComparison.OrdinalIgnoreCase) && shirt.Size < 6)  
            {  
                return new ValidationResult("For women's shirt, the size has to be greater or equal to 6");  
            }  
        }  
        return ValidationResult.Success;  
    }  
}```

Nous allons a présent effectuer une validation ==avant== l'execution de la méthode de traitement (method action / controller).

Cette procédure se nomme ==`Action Filter`== (filtre d'action ou filtre de traitement) . Elle est une extension de la middleware pipeline d'ASP.NET core [Documentation Microsoft filters](https://learn.microsoft.com/en-us/aspnet/core/mvc/controllers/filters?view=aspnetcore-8.0) :

Comme nous pouvons le voir (Action Invocation Pipeline (Filter Pipeline)), avec ASP.NET Core la requête passe par un tunnel de filtre au cours de son traitement:
![The request is processed through Other Middleware, Routing Middleware, Action Selection, and the Action Invocation Pipeline. The request processing continues back through Action Selection, Routing Middleware, and various Other Middleware before becoming a response sent to the client.](https://learn.microsoft.com/en-us/aspnet/core/mvc/controllers/filters/_static/filter-pipeline-1.png?view=aspnetcore-8.0)

Et elle peut traverser plusieurs filtres dans cette pipeline:
![[Filters.png]]

Ici nous traitons `Action Filters`, qui est déclenché avant notre méthode d'action et après notre méthode d'action.

En bref on court circuite la pipeline si un des filtres n'est pas valide

Pour cela nous allons créer un dossier `Filters`à la racine et créer une classe qui effectuera le filtre que l'on nommer `NomDuModel_CeQuElleTraiteFilterAttribute`
(exemple : `Shirt_ValidateShirtIdFilterAttribute`). Cette classe héritera de la classe `ActionFilterAttribute` et nous pourrons override ces méthodes pour effectuer nos traitement.
Ici nous utiliserons la méthode `OnActionExecuting` car nous souhaitons filtrer avant notre méthode action (dans le cas contraire c'est à dire après nous aurions pu utiliser la méthode `OnActionExecuted`).
![[Screenshot 2024-03-20 at 00.26.17.png]]

```cs
  
public class Shirt_ValidateShirtIdFilterAttibute : ActionFilterAttribute  
{  
    public override void OnActionExecuting(ActionExecutingContext context)  
    {  
        //Ici il nous faut les arguments envoyés en paramètre de la méthode pour pouvoir les traiter  
        //en l'occurence un entier nullable        int? shirtId = context.ActionArguments["id"] as int?;  
        if (shirtId.HasValue)  
        {  
            if (shirtId <= 0)  
            {  
                //Ici on identifie la propriété de notre modèle qui est n'est pas valide ainsi que le message que l'on souhaite retourner  
                context.ModelState.AddModelError("ShirtId", "ShirtId is invald");  
                //Nous définissons la réponse à la requête pour ce filtre  
                context.Result = new BadRequestObjectResult(context.ModelState);  
            }  
            //Si l'id n'existe pas dans notre liste de donnée nous souhaitons également court circuité la pipeline  
            else if (!ShirtRepository.ShirtExist(shirtId.Value))  
            {  
                //Ici on identifie la propriété de notre modèle qui est n'est pas valide ainsi que le message que l'on souhaite retourner  
                context.ModelState.AddModelError("ShirtId", "ShirtId doesn't exist");  
                //Nous définissons la réponse à la requête pour ce filtre  
                context.Result = new NotFoundObjectResult(context.ModelState);     
            }  
        }  
          
    }  
}```


Voici la réponse dans le cas ou id est inférieur ou égal à zéro
![[Screenshot 2024-03-20 at 00.27.32.png]]

et le cas où id n'existe pas (supérieur à zéro)
![[Screenshot 2024-03-20 at 00.28.30.png]]

Nous avons une partie des réponses qui est comme attendu mais pas disposés comme nous le souhaiterions. Voici comment corriger ceci:

Au lieu de passer notre `context.ModelState` comme résultat nous allons utiliser la classe:
`ValidationProblemDetails` comme ci-dessous

```cs
  
public class Shirt_ValidateShirtIdFilterAttibute : ActionFilterAttribute  
{  
    public override void OnActionExecuting(ActionExecutingContext context)  
    {  
        //Ici il nous faut les arguments envoyés en paramètre de la méthode pour pouvoir les traiter  
        //en l'occurence un entier nullable        int? shirtId = context.ActionArguments["id"] as int?;  
        if (shirtId.HasValue)  
        {  
            if (shirtId <= 0)  
            {  
                //Ici on identifie la propriété de notre modèle qui est n'est pas valide ainsi que le message que l'on souhaite retourner  
                context.ModelState.AddModelError("ShirtId", "ShirtId is invald");  
                //Nous définissons la réponse à la requête pour ce filtre  
                ValidationProblemDetails problemDetails = new ValidationProblemDetails(context.ModelState)  
                {  
                    Status = StatusCodes.Status400BadRequest  
                };  
                context.Result = new BadRequestObjectResult(problemDetails);  
            }  
            //Si l'id n'existe pas dans notre liste de donnée nous souhaitons également court circuité la pipeline  
            else if (!ShirtRepository.ShirtExist(shirtId.Value))  
            {  
                //Ici on identifie la propriété de notre modèle qui est n'est pas valide ainsi que le message que l'on souhaite retourner  
                context.ModelState.AddModelError("ShirtId", "ShirtId doesn't exist");  
                ValidationProblemDetails problemDetails = new ValidationProblemDetails(context.ModelState)  
                {  
                    Status = StatusCodes.Status404NotFound  
                };  
                //Nous définissons la réponse à la requête pour ce filtre  
                context.Result = new NotFoundObjectResult(problemDetails);     
            }  
        }  
          
    }  
}
```

voici à présent la réponse à nos requêtes

![[Screenshot 2024-03-20 at 00.36.19.png]]

![[Screenshot 2024-03-20 at 00.36.40.png]]

Nous avons a présent des réponses à des erreurs dans un format plus standardisé


# Create Endpoint

Pour montrer la manière de créer un endpoint nous allons reprendre l'exemple le plus basic:

```cs
[HttpGet]  
public IActionResult GetShirts()  
{  
    return Ok("Reading all the shirts");  
}```

Actuellement nous retournons uniquement un string, notre objectif est de retourner la list de tous les shirts. Pour cela nous allons travailler sur notre  ShirtRepository pour donner accès à cette donnée.

Créons une méthode GetShirts, tel que:

```cs
public static List<Shirt> GetShirts()  
{  
    return ListOfShirt;  
}
```

et modifions notre endpoint tel que:

```cs
[HttpGet]  
public IActionResult GetShirts()  
{  
    return Ok(ShirtRepository.GetShirts());  
}
```

Nous avons encore une fois pu garantir la responsabilité unique.

Appliquons ceci à notre endpoint suivant:

```cs
[HttpPost]  
public IActionResult CreateShirt(Shirt shirt)  
{  
    return Ok($"Creating a shirt");  
}
```

Nous appliquons la logique ci-dessous dans notre controleur pour une creation dans notre base de donnée

```cs
[HttpPost]  
public IActionResult CreateShirt([FromBody]Shirt? shirt)  
{  
    //on vérifie si l'argument n'est pas null  
    if (shirt == null)  
        return BadRequest();  
    //On vérifie s'il n'existe pas dans notre base de donnée pour éviter le doublon  
    Shirt? existingShirt = ShirtRepository.GetShirtProperties  
        (shirt.Brand, shirt.Color, shirt.Gender, shirt.Size);  
    if (existingShirt != null) return BadRequest();  
    //on l'ajoute à notre list  
    ShirtRepository.AddShirt(shirt!);  
    //Par convention dans les web api nous retournons un CreatedAtAction  
    return CreatedAtAction(nameof(GetShirtById), new {Id = shirt.ShirtId}, shirt);  
}
```

Dans notre repository voici la méthode pour vérifier si l'argument existe:

```cs
public static Shirt? GetShirtProperties(string? brand, string? color, string? gender, int? size)  
{  
    return ListOfShirt.FirstOrDefault(  
        x => !string.IsNullOrWhiteSpace(brand) &&  
             !string.IsNullOrWhiteSpace(x.Brand) &&  
             x.Brand.Equals(brand, StringComparison.OrdinalIgnoreCase) &&  
             !string.IsNullOrWhiteSpace(color) &&  
             !string.IsNullOrWhiteSpace(x.Color) &&  
             x.Brand.Equals(color, StringComparison.OrdinalIgnoreCase) &&  
             !string.IsNullOrWhiteSpace(gender) &&  
             !string.IsNullOrWhiteSpace(x.Gender) &&  
             x.Brand.Equals(gender, StringComparison.OrdinalIgnoreCase) &&  
             size.HasValue && x.Size.HasValue && size.Value == x.Size.Value  
             );  
}
```

Si nous tentons de créer un shirt qui existe déjà nous recevons bien en répons un erreur 400:

![[Screenshot 2024-03-20 at 10.38.38.png]]

Dans le cas contraire nous avons bien code retour 201 (created):
![[Screenshot 2024-03-20 at 10.40.29.png]]

Dans le header de la réponse on peut retrouver la `Location` qui nous permet de faire une requête pour obtenir le shirt que nous venons de créer:

![[Screenshot 2024-03-20 at 10.41.38.png]]

Nous pouvons constater que le nouveau shirt a bien été créée :
![[Screenshot 2024-03-20 at 10.42.47.png]]

# Validating Create Endpoint with Action Filter

Dans notre endpoint précédent nous effectuer certaines verifications directement dans notre méthode, comme vu précédemment ceci ne respect pas le principe de responsabilité unique de la méthode. Pour résoudre ce problème nous allons mettre en place un filtre d'action comme vu précédemment:

```cs
  
public class Shirt_ValidateCreateShirtFilterAttribute : ActionFilterAttribute  
{  
    public override void OnActionExecuting(ActionExecutingContext context)  
    {  
        Shirt? shirt = context.ActionArguments["shirt"] as Shirt;  
        if (shirt == null)  
        {  
            //Ici on identifie la propriété de notre modèle qui est n'est pas valide ainsi que le message que l'on souhaite retourner  
            context.ModelState.AddModelError("Shirt", "Shirt is null");  
              
            //Nous définissons la réponse à la requête pour ce filtre  
            ValidationProblemDetails problemDetails = new ValidationProblemDetails(context.ModelState)  
            {  
                Status = StatusCodes.Status400BadRequest  
            };  
            context.Result = new BadRequestObjectResult(problemDetails);  
        }  
        else  
        {  
            //On vérifie s'il n'existe pas dans notre base de donnée pour éviter le doublon  
            Shirt? existingShirt = ShirtRepository.GetShirtProperties  
                (shirt.Brand, shirt.Color, shirt.Gender, shirt.Size);  
            if (existingShirt != null)  
            {  
                //Ici on identifie la propriété de notre modèle qui est n'est pas valide ainsi que le message que l'on souhaite retourner  
                context.ModelState.AddModelError("Shirt", "Shirt already exist");  
              
                //Nous définissons la réponse à la requête pour ce filtre  
                ValidationProblemDetails problemDetails = new ValidationProblemDetails(context.ModelState)  
                {  
                    Status = StatusCodes.Status400BadRequest  
                };  
                context.Result = new BadRequestObjectResult(problemDetails);  
            }  
        }  
          
    }  
}
```

et notre controller:

```cs
[HttpPost]  
[Shirt_ValidateCreateShirtFilter]  
public IActionResult CreateShirt([FromBody]Shirt? shirt)  
{  
      
    //on l'ajoute à notre list  
    ShirtRepository.AddShirt(shirt);  
      
    //Par convention dans les web api nous retournons un CreatedAtAction  
    return CreatedAtAction(nameof(GetShirtById), new {Id = shirt.ShirtId}, shirt);  
}
```

Implémentons également notre endpoint ci-dessous:

```cs
[HttpPut("{id}")]  
public IActionResult UpdateShirt(int id)  
{  
    return Ok($"Updating shirt: {id}");  
}
```

Dans notre repository créons la méthode permettant de mettre à jour des données:

```cs
public static void UpdateShirt(Shirt shirt)  
{  
    Shirt shirtToUpdate = ListOfShirt.First(x => x.ShirtId == shirt.ShirtId);  
    shirtToUpdate.Brand = shirt.Brand;  
    shirtToUpdate.Gender = shirt.Gender;  
    shirtToUpdate.Color = shirt.Color;  
    shirtToUpdate.Size = shirt.Size;  
    shirtToUpdate.Price = shirt.Price;  
}
```

Et mettons en place notre controleur:

```cs
[HttpPut("{id}")]  
public IActionResult UpdateShirt(int id, Shirt shirt)  
{  
    //On vérifie si l'id correspond à l'id du shirt envoyé  
    if (id != shirt.ShirtId)  
        return BadRequest();  
      
    // on met à jour notre donnée  
    ShirtRepository.UpdateShirt(shirt);  
      
    //Type de retour conventionnel pour une mise à jour de donnée dans la BDD 
    //Status 204
    return NoContent();  
}
```

Cependant, notre contrôleur n'envisage pas la situation ou cette même donnée serait supprimé
juste avant l'execution de notre méthode UpdateShirt, nous pourrions faire un contrôle de son existence juste avant mais rien ne garanti que cette suppression arrive juste entre les deux logiques. Pour résoudre ce problème nous allons gérer cette exception tel que:

```cs
[HttpPut("{id}")]  
public IActionResult UpdateShirt(int id, Shirt shirt)  
{  
    //On vérifie si l'id correspond à l'id du shirt envoyé  
    if (id != shirt.ShirtId)  
        return BadRequest();  
      
    try  
    {  
        // on met à jour notre donnée  
        ShirtRepository.UpdateShirt(shirt);  
    }  
    catch  
    {  
        //Ici on lève la possibilité que le shirt n'existe plus  
        if (ShirtRepository.ShirtExist(shirt.ShirtId))  
            return NotFound();  
        throw;  
    }  
      
    //Type de retour conventionnel pour une mise à jour de donnée dans la BDD  
    return NoContent();  
}
```

Et voici notre réponse avec un status 204
![[Screenshot 2024-03-20 at 11.29.51.png]]

On voit bien que la couleur a été mis à jour à black:
![[Screenshot 2024-03-20 at 11.31.45.png]]

Provoquons l'erreur pour verifier la réponse:
![[Screenshot 2024-03-20 at 12.13.01.png]]

En faisant une requête update sur un objet qui n'existe pas nous avons bien un status de réponse 404 avec un corps approprié.
Afin de valider correctement la requête, il faut valider notre paramètre `id` à l'aide de l'attribute
`[Shirt_ValidateShirtIdFilter]` que nous avons créé précédemment.

```cs
[HttpPut("{id}")]  
[Shirt_ValidateShirtIdFilter]  
public IActionResult UpdateShirt(int id, Shirt shirt)  
{  
    //On vérifie si l'id correspond à l'id du shirt envoyé  
    if (id != shirt.ShirtId)  
        return BadRequest();  
    try  
    {  
        // on met à jour notre donnée  
        ShirtRepository.UpdateShirt(shirt);  
    }  
    catch  
    {  
        //Ici on lève la possibilité que le shirt n'existe plus  
        if (!ShirtRepository.ShirtExist(shirt.ShirtId))  
            return NotFound();  
        throw;  
    }  
      
    //Type de retour conventionnel pour une mise à jour de donnée dans la BDD  
    return NoContent();  
}
```

Enfin, toujours dans le principe de la responsabilité unique nous allons mettre en place un filtre d'action `(Action filter)` plutôt que de faire des validations dans notre endpoint:
`
```cs
  
public class Shirt_ValidateUpdateShirtFilterAttribute : ActionFilterAttribute  
{  
    public override void OnActionExecuting(ActionExecutingContext context)  
    {  
        //base.OnActionExecuting(context);  
        int? id = context.ActionArguments["id"] as int?;  
        Shirt? shirt = context.ActionArguments["shirt"] as Shirt;  
        if (id.HasValue && shirt != null && id.Value != shirt.ShirtId)  
        {  
            context.ModelState.AddModelError("ShirtId", "ShirtId is not the same as id");  
            //Nous définissons la réponse à la requête pour ce filtre  
            ValidationProblemDetails problemDetails = new ValidationProblemDetails(context.ModelState)  
            {  
                Status = StatusCodes.Status400BadRequest  
            };  
            context.Result = new BadRequestObjectResult(problemDetails);  
        }  
    }  
}
```

et notre endpoint se présentera comme ci-dessous avec ce nouveau filtre:

```cs
[HttpPut("{id}")]  
[Shirt_ValidateShirtIdFilter]  
[Shirt_ValidateUpdateShirtFilter]  
public IActionResult UpdateShirt(int id, Shirt shirt)  
{  
    try  
    {  
        // on met à jour notre donnée  
        ShirtRepository.UpdateShirt(shirt);  
    }  
    catch  
    {  
        //Ici on lève la possibilité que le shirt n'existe plus  
        if (!ShirtRepository.ShirtExist(shirt.ShirtId))  
            return NotFound();  
        throw;  
    }  
      
    //Type de retour conventionnel pour une mise à jour de donnée dans la BDD  
    return NoContent();  
}
```

Nous avons bien notre erreur ainsi que notre message lorsque l'id en url est différent de celui envoyé dans le corps:
![[Screenshot 2024-03-20 at 12.29.58.png]]

et Il ne restera qu'a extraire notre try catch pour respecter le principe de responsabilité unique.

# Exception Handling with Exception Filter

Dans notre dossier filtre nous allons créer un sous dossier `ExceptionFilters`, dédié à cette responsabilité et un autre sous dossier `ActionFilters`qui contiendra les filtres que nous avons créer jusqu'ici pour les actions.

Voici notre arborescence:
![[Screenshot 2024-03-20 at 13.25.28.png]]

Et nous gérons l'exception dans ce nouveau filtre en faisant hériter notre filtre de ExceptionFilterAttribute
```cs
public class Shirt_HandleExceptionsFilterAttribute : ExceptionFilterAttribute  
{  
    public override void OnException(ExceptionContext context)  
    {  
        //on utilise le context pour récupérer les paramètre  
        //ici on utilise route data pour récupérer les arguments de la requête        string? strShirtId = context.RouteData.Values["id"] as string;  
        //On le passe en int pour effectuer nos tests  
        if (int.TryParse(strShirtId, out int shirtId))  
        {  
            //on court circuit notre traitement si on rentre dans la condition  
            if (!ShirtRepository.ShirtExist(shirtId))  
            {  
                context.ModelState.AddModelError("ShirtId", "Shirt is doesn't exist anymore");  
                //Nous définissons la réponse à la requête pour ce filtre  
                ValidationProblemDetails problemDetails = new ValidationProblemDetails(context.ModelState)  
                {  
                    Status = StatusCodes.Status404NotFound  
                };  
                context.Result = new NotFoundObjectResult(problemDetails);  
            }  
        }  
    }  
}
```

Finalement on assigne l'attribut à notre Endpoint:

```cs
[HttpPut("{id}")]  
[Shirt_ValidateShirtIdFilter]  
[Shirt_ValidateUpdateShirtFilter]  
[Shirt_HandleExceptionsFilter]  
public IActionResult UpdateShirt(int id, Shirt shirt)  
{  
     // on met à jour notre donnée  
     ShirtRepository.UpdateShirt(shirt);  
     //Type de retour conventionnel pour une mise à jour de donnée dans la BDD  
    return NoContent();  
}
```

Provoquons l'erreur sur Postman

![[Screenshot 2024-03-20 at 13.34.45.png]]

Nous pouvons remarquer que nous avons le message d'erreur: 
`ShirtId doesn't exist` au lieu de `ShirtId doesn't exist anymore`

Ceci est du à notre filtre action `[Shirt_ValidateShirtIdFilter]`, à des fins de tests nous pouvons le mettre en commentaire, et nous avons alors cette réponse :

![[Screenshot 2024-03-20 at 13.34.11.png]]

## Create Delete Endpoint

Tout d'abord, implémentons la méthode qui nous permet de supprimer un item dans notre repository:

```cs
public static void DeleteShirt(int id)  
{  
        Shirt? shirt = GetShirtById(id);  
        if (shirt != null)  
            ListOfShirt.Remove(shirt);  
}
```

Et dans notre endpoint après avoir donné l'attribut de filtre d'action sur l'id nous exectuons notre méthode de suppression:

```cs
[HttpDelete("{id}")]  
[Shirt_ValidateShirtIdFilter]  
public IActionResult DeleteShirt(int id)  
{  
    //nous le stockons dans une variable pour l'utiliser dans notre réponse  
    Shirt? shirt = ShirtRepository.GetShirtById(id);  
    ShirtRepository.DeleteShirt(id);  
    return Ok(shirt);  
}
```

Testons, avec un id qui n'existe pas:

![[Screenshot 2024-03-20 at 13.49.25.png]]

Ensuite testons un id qui existe:
![[Screenshot 2024-03-20 at 13.50.02.png]]