# C# Personal doc
#### Alexis Barthelmebs -  13.02.2025

## Introduction

Through a list of questions and answers, we will go through the main  concetps and application of the C# language. This document is divised in sections from the most basic topics to the most advanced ones.

## **Questions de base**
### **Qu'est-ce que le C# et quels sont ses principaux avantages ?**
Langage fortement typé, sécurisé et compilé POO développé par Microsoft et intégré dans .NET. Possède un garbage collector, compatible Windows, web et mobile.

### **Quelle est la différence entre == et .Equals() en C# ?**
== est un signe d’égalité utilisé dans des conditions, .Equals() et une méthode qui peut être redéfinie pour avoir un comportement modifié.

### **Qu'est-ce qu'une classe en C# ? Donne un exemple.**
Une classe est un ensemble d’attributs et de méthodes pouvant être instanciée sous forme d’objets via un constructeurs qui peut être défini de plusieurs façons différentes. Ses composants peuvent avoir différentes portées et la notion d’héritage touche les classes.

### **Quelle est la différence entre une classe et une structure en C# ?**
Une classe est stockée sur le tas, une structure est stockée sur la pile. Class mieux pour données complexes, struct mieux pour données simples. Classe instanciable via constructeur qui peut être vide, struc est construite lorsque toutes les données sont connues. Struct est de type valeur et donc passée par copie et non référence comme les classes.

### **Qu'est-ce qu'une méthode statique ? Peux-tu donner un exemple ?**
Une méthode statique appartient à la classe et non l’instanciation de la classe.
NomClasse.methodeStatique()

### **Quelle est la différence entre const et readonly en C# ?**
Const est défini lors de la compilation, readonly est défini lors de l’instanciation d’une classe. Les deux, une fois définis ne peuvent être modifiés

### **Qu'est-ce qu'une propriété en C# et comment est-elle différente d'un champ ?**
Un champs n’as pas de contrôle d’acces, une propriété (get / set) peut contrôler son accès via la définition des méthodes.

### **Qu'est-ce qu'une interface en C# ? Donne un exemple.**
Une interface est un contrat qui définit des méthodes qu’une classe qui l’implémente doit remplir. Ca permet le polymorphisme et une meilleur abstraction

### **Quelle est la différence entre abstract et sealed en C# ?**
Abstract doit être surenregistré lors d’un héritage (class ou méthode).
Classe sealed ne peut être héritée, méthode sealed ne peut être redéfinie.

### **Qu'est-ce que l'héritage en C# ? Donne un exemple.**
L’héritage permet à une classe de récupérer les méthodes et attributs d’une classe parente qu’elle implémente. Ex Parent (int a, b, methode add(int a, int b)) et child : Parent avec. Child.Add(number1, number2) c’est ok.

## **Questions intermédiaires**

### **Qu'est-ce que la surcharge de méthodes (method overloading) en C# ? Donne un exemple.**
L’overloading c’est créer une méthode X et la définir plusieurs fois avec des arguments différents. La méthode doit avoir le même nom mais des arguments différents, donc une signature différente à chaque fois.

### **Qu'est-ce que la surcharge d'opérateurs (operator overloading) en C# ? Donne un exemple.**
Surcharge opérateur permet de redéfinir le comportement d’un opérateur dans le cadre d’une classe pour la manipulation d’objet.

### **Qu'est-ce que la gestion des exceptions en C# ? Comment utilises-tu try, catch, et finally ?**
La gestion d’exception se passe via un try catch. Le code est effectué dans une bulle, le try. Si une erreur survient, elle éclate la bulle et est récupérée par le catch qui récupère la trace et est capable de la remonter / donner. Une fois le catch fait, s’il y a un finally, le code du finally sera effectué et le programme continuera. Cette gestion d’exception permet d’encapsuler des parties de code ou tout et permettre de continuer l’exécution du soft sans coupure.

### **Qu'est-ce qu'un délégué (delegate) en C# ? Donne un exemple.**
Gérer un évènement : le delegate est l’eventhandler. C’est lui qui va notifier les autres objets abonnés qu’une action se produit.
Définition delegate, définition methode1(std), methode2(delegate). Au mieux de faire method1(std), on va faire methode2(std, del action){action(std)}.
Avoir des callback. Une méthode qui prend un delegate en argument. Lors de l’appel de la méthode on lui met soit une fonction lambda soit une autre méthode en argument pour qu’elle soit effectuée quand voulu.
Créer une liste de méthode à faire. Comme un set de méthode à exécuter.
 
### **Quelle est la différence entre un délégué et un événement en C# ?**
Un évènement se base sur le fonctionnement d’un délégué. Un délégué est une entité capable de stocker / faire un lien vers une méthode. On peut lui ajouter des méthodes dans sa liste de fonction à exécuter. On peut invoquer un delegué pour qu’il trigger toutes les méthodes

### **Qu'est-ce qu'un lambda expression en C# ? Donne un exemple.**
Fonction anonyme qui peut être définie dans le code directement. Notées (paramètre) => expression
Genre Func<int, int> square = x => x * x ; dans le cas où l’on veut un retour
Sinon,
Action<int> log = x => {Console.WriteLine($”number of try: {x}”)}
Func et Action sont basées sur des delegate.

### **Qu'est-ce que LINQ (Language Integrated Query) en C# ? Donne un exemple.**
Le LINQ est un ensemble de fonctionnalités C# utilisées pour requêter sur des collections de données. Utile dans un cadre où l’on traite des données (bdd, xml, SQL). Ca ressemble à du SQL d’une certaine manière.
Evite les for et foreach.
```C#
var evenNumbers =   from n in numbers
                    where n % 2 == 0
                    select n;
```
ou
```C#
var evenNumbers = numbers.Where(n => n% 2 == 0);
objet.Where(p => p.Age > 25).Select(p => p.Name);
```
ou
```C#
var sortedPeople = people.OrderBy(p => p.Age);
var groupedByAge = people.GroupBy(p => p.Age);
```
Le LINQ utilise:
```C#
.Where .Select .OrderBy .GroupBy 

.Join(obj1 => obj1.Id, obj2 => obj2.randId, (obj1, obj2) => new { obj1.Name, obj2.Date}

.Count .Sum .Average .Min .Max

.First .FirstOrDefault // retourne premier élément, retour exception | Null ou 0

.Single .SingleOrDefault // vérifie qu’il n’y a qu’un élément

.Any // Au moin un rempli la condition 
.All // Tous les éléments remplissent la condition

.Distinct .Concat
```

### **Quelle est la différence entre IEnumerable et IQueryable en C# ?**
IEnum => ok pour collection locales, exécuté côté client, récupère toute la collection puis applique les filtres

IQuery => ok pour bdd, transforme la requête en SQL et ok LINQ=>SQL, permet le Lazy Loading. Ok pour Entity Framework (ORM => badd gérée via des objets).

Lazy Loading avec déclaration en virtual.

### **Qu'est-ce que la réflexion (reflection) en C# ? Donne un exemple.**
Mécanisme qui permet de manipuler des types dynamiquement pendant l’exécution. Flexible mais coûteux en performance.

### **Qu'est-ce que la sérialisation en C# ? Donne un exemple.**
Permet le transfert, stockage ou la communication de données. On prend un objet on le transforme en binaire, xml, Json en le sérialisant. On peut ainsi le stocker en bdd ou fichier pour ensuite le désérialiser et pouvoir accéder à cet objet.

## **Questions avancées**

### **Qu'est-ce que la programmation asynchrone en C# ? Comment utilises-tu async et await ?**
Asynchrone = non bloquant.
Une méthode doit être notée async pour déclarer son asynchronicité. Elle doit retourner  un Task ou Task<T>. Lorsqu’elle est appelée elle est précédée d’un await pour indiquer que l’on attend la fin d’exécution de la méthode.

### **Quelle est la différence entre Task et Thread en C# ?**
Un thread est un fil d’exécution indépendant qui exécute du code de manière concurrent à d’autres threads. Il est géré par le système d’exploitation. Couteux, il dispose de ses propres ressources système.Pas adapté pour de nombreuses tâches courtes.
Une Task est une abstraction plus légère qui se base sur un thread pool. Elle est gérée par le gestionnaire de tâche .NET et est adaptée aux tâches courtes ou les opérations asynchrones.

### **Qu'est-ce que le garbage collection en C# ? Comment fonctionne-t-il ?**
Un garbage collector à pour but de libérer la mémoire des objets qui ne sont plus utilisées. Il va suprimer de la mémoire les objets non référrencés du tas en privilégiant les objets les plus jeunes lorsqu’e le tas est plein ou que certaines conditions sont remplies

### **Qu'est-ce que le pattern Dispose en C# ? Pourquoi est-il important ?**
le pattern dispose est un pattern utilisé pour libérer les ressources non managées. Il est important parce qu’il permet d’optimiser l’exécution du code via la libération des ressources allouées par l’OS. avec le pattern Dispo, utiliser un using libère automatiquement les ressources encapsulées.

### **Qu'est-ce que la mémoire managée et la mémoire non managée en C# ?**
les ressources managées (tas) sont gérées automatiquement via le garbage collector. Les ressources non managées doivent être gérées manuellement via des using ou un dispose ou un finalizer.
RM : mémoire .NET / variables, objets etc / threads - tasks & thread (en partie) - timers.
RNM: fichier, sockets, connection bdd, mutex / sémaphore.

### **Qu'est-ce que le pattern Singleton en C# ? Donne un exemple.**
pattern qui assure qu’une classe n’a qu’une seule instance et fournit un point d’accès global à cette instance. 


```C#

public class Singleton
{
    private static readonly Singleton _instance = new Singleton(); // Unique instance


    private Singleton() { } // Empêche `new Singleton()`


    public static Singleton Instance => _instance; // Retourne l'instance unique
}
```



- constructeur privé : empêche l’instanciation de la classe en dehors d’elle meme
- instance unique static: initialisée qu’une fois lors du chargement
- Accès contrôlé via prop static: 


### **Qu'est-ce que le pattern Factory en C# ? Donne un exemple.**
Le pattern factory permet d’avoir une classe d’aiguillage pour la création d’objets. via un sélecteur, ce sont les input qui vont nous permettre de choisir quel objet créer. `switch` / `if` / `réflection` / `dictionnaire`.

Exemple:
```C#
public class Animal
{
    public string Nom { get; }
    public Animal(string nom) => Nom = nom;
}

public static class AnimalFactory
{
    public static Animal CreerAnimal(string nom) => new Animal(nom);
}

// Utilisation
Animal chien = AnimalFactory.CreerAnimal("Chien");
Console.WriteLine(chien.Nom); // Affiche "Chien"
```







Qu'est-ce que le pattern Observer en C# ? Donne un exemple.
Qu'est-ce que le pattern Dependency Injection en C# ? Donne un exemple.
Qu'est-ce que le pattern Repository en C# ? Donne un exemple.
Questions pratiques
Écris une méthode en C# qui inverse une chaîne de caractères.
Écris une méthode en C# qui vérifie si une chaîne de caractères est un palindrome.
Écris une méthode en C# qui calcule la factorielle d'un nombre.
Écris une méthode en C# qui trouve le nombre le plus grand dans un tableau d'entiers.
Écris une méthode en C# qui trie un tableau d'entiers en utilisant l'algorithme de tri à bulles.
Écris une méthode en C# qui implémente le pattern Singleton.
Écris une méthode en C# qui utilise LINQ pour filtrer une liste d'objets.
Écris une méthode en C# qui utilise async et await pour effectuer une requête HTTP.
Écris une méthode en C# qui sérialise un objet en JSON.
Écris une méthode en C# qui désérialise un objet JSON en un objet C#.
Questions sur les concepts de programmation
Qu'est-ce que la programmation orientée objet (POO) ? Quels sont ses principes de base ?
Quelle est la différence entre l'encapsulation, l'héritage et le polymorphisme en POO ?
Qu'est-ce que le polymorphisme en C# ? Donne un exemple.
Qu'est-ce que l'encapsulation en C# ? Donne un exemple.
Qu'est-ce que l'abstraction en C# ? Donne un exemple.
Quelle est la différence entre une classe abstraite et une interface en C# ?
Qu'est-ce que la composition en C# ? Donne un exemple.
Qu'est-ce que l'agrégation en C# ? Donne un exemple.
Qu'est-ce que le couplage et la cohésion en programmation ?
Qu'est-ce que le principe SOLID en programmation orientée objet ?
Questions sur les frameworks et outils
Qu'est-ce que .NET Core et en quoi est-il différent de .NET Framework ?
Qu'est-ce que ASP.NET Core et quels sont ses avantages ?
Qu'est-ce que Entity Framework Core et comment est-il utilisé en C# ?
Quelle est la différence entre Entity Framework et Dapper ?
Qu'est-ce que le middleware en ASP.NET Core ?
Qu'est-ce que la dépendance injectée (Dependency Injection) en ASP.NET Core ?
Qu'est-ce que le routage (routing) en ASP.NET Core ?
Qu'est-ce que le modèle MVC (Model-View-Controller) en ASP.NET Core ?
Qu'est-ce que le modèle MVVM (Model-View-ViewModel) en C# ?
Qu'est-ce que le modèle Razor Pages en ASP.NET Core ?
Questions sur les tests
Qu'est-ce que les tests unitaires en C# ? Donne un exemple.
Quelle est la différence entre les tests unitaires et les tests d'intégration ?
Qu'est-ce que le framework de test xUnit en C# ?
Qu'est-ce que le framework de test NUnit en C# ?
Qu'est-ce que le framework de test MSTest en C# ?
Qu'est-ce que le mocking en C# ? Donne un exemple.
Qu'est-ce que le framework Moq en C# ?
Qu'est-ce que le TDD (Test-Driven Development) ?
Qu'est-ce que le BDD (Behavior-Driven Development) ?
Qu'est-ce que le code coverage en C# ?
Questions sur les bonnes pratiques
Quelles sont les bonnes pratiques pour écrire du code propre en C# ?
Qu'est-ce que le principe DRY (Don't Repeat Yourself) en programmation ?
Qu'est-ce que le principe KISS (Keep It Simple, Stupid) en programmation ?
Qu'est-ce que le principe YAGNI (You Aren't Gonna Need It) en programmation ?
Qu'est-ce que le principe SOLID en programmation orientée objet ?
Qu'est-ce que le principe de séparation des préoccupations (Separation of Concerns) ?
Qu'est-ce que le principe de responsabilité unique (Single Responsibility Principle) ?
Qu'est-ce que le principe ouvert/fermé (Open/Closed Principle) ?
Qu'est-ce que le principe de substitution de Liskov (Liskov Substitution Principle) ?
Qu'est-ce que le principe d'inversion de dépendance (Dependency Inversion Principle) ?

