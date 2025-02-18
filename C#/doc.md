# C# Personal doc
#### Alexis Barthelmebs -  13.02.2025

## Introduction

Via une liste de questions réponses, nous allons aborder les principaux concepts liés au langage C#, son utilisatin et les concept de programmation utiles. Ce document est divisé en plusieurs sections qui abordent des questions de difficultés progressive.

## **Questions de base**
### **Qu'est-ce que le C# et quels sont ses principaux avantages ?**
Langage fortement typé, sécurisé et compilé POO développé par Microsoft et intégré dans .NET. Possède un garbage collector, compatible Windows, web et mobile.

### **Quelle est la différence entre == et .Equals() en C# ?**
`==` est un signe d’égalité utilisé dans des conditions, `.Equals()` et une méthode qui peut être redéfinie pour avoir un comportement modifié.

### **Qu'est-ce qu'une classe en C# ? Donne un exemple.**
Une classe est un ensemble d’attributs et de méthodes pouvant être instanciée sous forme d’objets via un constructeurs qui peut être défini de plusieurs façons différentes. Ses composants peuvent avoir différentes portées.

### **Quelle est la différence entre une classe et une structure en C# ?**
Une classe est stockée sur le tas, une structure est stockée sur la pile, donc non gérée par le GC sauf si elle est implémentée par une classe. Classes mieux pour données complexes, struct mieux pour données simples. Classe instanciable via constructeur qui peut être vide, struc ne peut être construite que lorsque toutes les données sont connues. Struct est de type valeur et donc passée par copie et non référence comme les classes.

### **Qu'est-ce qu'une méthode statique ? Peux-tu donner un exemple ?**
Une méthode statique appartient à la classe et non l’instanciation de la classe.
`NomClasse.methodeStatique()`

### **Quelle est la différence entre const et readonly en C# ?**
Une `const` est défini lors de la compilation, un `readonly` est défini lors de l’instanciation d’une classe. Les deux, une fois définis ne peuvent être modifiés

### **Qu'est-ce qu'une propriété en C# et comment est-elle différente d'un champ ?**
Un champs n’as pas de contrôle d’acces, une propriété (`get` / `set`) peut contrôler son accès via la définition des méthodes.

### **Qu'est-ce qu'une interface en C# ? Donne un exemple.**
Une interface est un contrat qu’une classe ou structure qui l’implémente doit remplir. Ca permet le polymorphisme et une meilleur abstraction.

```C#
// Définition de l'interface
public interface IAnimal
{
    void Crier();  // Méthode sans implémentation
}

// Implémentation de l'interface dans une classe
public class Chien : IAnimal
{
    public void Crier()
    {
        Console.WriteLine("Wouf !");
    }
}

// Implémentation dans une autre classe
public class Chat : IAnimal
{
    public void Crier()
    {
        Console.WriteLine("Miaou !");
    }
}

// Utilisation
class Program
{
    static void Main()
    {
        IAnimal monChien = new Chien();
        IAnimal monChat = new Chat();

        monChien.Crier();  // Affiche "Wouf !"
        monChat.Crier();   // Affiche "Miaou !"
    }
}
```

### **Quelle est la différence entre abstract et sealed en C# ?**
Une classe `abstract` doit être surenregistré lors d’un héritage (classe ou méthode).
Classe `sealed` ne peut être héritée, méthode sealed ne peut être redéfinie.

```C#
abstract class Animal  
{  
    public abstract void Crier(); // Méthode abstraite (pas d’implémentation)
}  

class Chien : Animal  
{  
    public override void Crier()  // Override permet le surenregistrement
    {  
        Console.WriteLine("Wouf !");
    }  
}
```



### **Qu'est-ce que l'héritage en C# ? Donne un exemple.**
L’héritage permet à une classe de récupérer l'implémentation des méthodes et attributs d’une classe parente (selon la portée des éléments). 

| **Modificateur**        | **Accessible dans la classe** | **Accessible dans les classes dérivées** | **Accessible dans le même assembly** | **Accessible en dehors de l’assembly** |
|-------------------------|-------------------------------|------------------------------------------|--------------------------------------|----------------------------------------|
| `private`               | ✅ Oui                        | ❌ Non                                    | ❌ Non                                | ❌ Non                                  |
| `protected`             | ✅ Oui                        | ✅ Oui                                    | ❌ Non                                | ❌ Non                                  |
| `internal`              | ✅ Oui                        | ✅ Oui (si dans le même assembly)         | ✅ Oui                                | ❌ Non                                  |
| `protected internal`    | ✅ Oui                        | ✅ Oui                                    | ✅ Oui                                | ❌ Non                                  |
| `public`                | ✅ Oui                        | ✅ Oui                                    | ✅ Oui                                | ✅ Oui                                  |
| `private protected`     | ✅ Oui                        | ✅ Oui (si dans le même assembly)         | ❌ Non                                | ❌ Non                                  |


Exemple:
```C#
class Parent
{
    private int secret = 42;       // Non accessible en héritage
    protected int valeur = 10;     // Accessible aux classes dérivées
}

class Enfant : Parent
{
    public void Afficher()
    {
        // Console.WriteLine(secret);  // Erreur : Non accessible
        Console.WriteLine(valeur);    // OK : Hérité de Parent
    }
}
```


## **Questions intermédiaires**

### **Qu'est-ce que la surcharge de méthodes (method overloading) en C# ? Donne un exemple.**
L’overloading c’est créer une méthode X et la définir plusieurs fois une signature différente.
Exemple:
```C#
public int doAction(int a, int b) {return a + b;}
public int doAction(int a) {return math.Sqrt(a);}
```

### **Qu'est-ce que la surcharge d'opérateurs (operator overloading) en C# ? Donne un exemple.**
Surcharge opérateur permet de redéfinir le comportement d’un opérateur dans le cadre d’une classe pour la manipulation d’objet.

```C#
public class Point
{
    public int X, Y;
    public Point(int x, int y) { X = x; Y = y; }

    // Surcharge de l'opérateur "+"
    public static Point operator +(Point p1, Point p2) => new Point(p1.X + p2.X, p1.Y + p2.Y);
}

class Program
{
    static void Main()
    {
        Point p1 = new Point(3, 4), p2 = new Point(1, 2);
        Point p3 = p1 + p2;  // Utilisation de l'opérateur "+" surchargé
        Console.WriteLine($"({p3.X}, {p3.Y})");  // Affiche "(4, 6)"
    }
}
```

### **Qu'est-ce que la gestion des exceptions en C# ? Comment utilises-tu try, catch, et finally ?**
La gestion d’exceptions se passe via un `try` `catch` `finally`. Le code est exécuté dans une bulle: le `try`. Si une erreur survient, elle éclate la bulle. Le code dans le `catch` est exécuté. Le `catch` peut récupérer et rendre utilisable la nature et la trace de l'exception. Une fois le code dans le `catch` exécuté, le code du `finally` sera exécuté s'il est présent. Cette gestion d’exceptions permet de splitter le code en parties distinctes et de récupérer les status d'exécutions de chaques parties. Une levée d'exception peut ou peut ne pas être bloquante. C'est à l'utilisateur de définir son comportement.

### **Qu'est-ce qu'un délégué (delegate) en C# ? Donne un exemple.**
Un `delegate` est un type qui permet de référencer des méthodes. Il peut être utilisé pour:
- appeler une méthode
- stocker une méthode dans une liste d'exécution

```C#
public delegate T RetourGenerique<T>();

class Program
{
    static void Main()
    {
        RetourGenerique<int> delInt = RetournerEntier;
        Console.WriteLine(delInt());  // Affiche 42

        RetourGenerique<string> delString = RetournerTexte;
        Console.WriteLine(delString());  // Affiche Hello World
    }

    static int RetournerEntier() => 42;
    static string RetournerTexte() => "Hello World";
}
```
On a aussi la possibilité de mettre en place des callbacks:

```C# 
using System;

public delegate void CallbackDelegate(string message, bool success);

class Program
{
    static void Main()
    {
        // Appel de la méthode avec un seul callback générique
        EffectuerOperation(Callback);
    }

    // Méthode qui effectue une tâche et appelle un callback
    static void EffectuerOperation(CallbackDelegate callback)
    {
        try
        {
            // Simuler une tâche qui prend du temps
            Console.WriteLine("Traitement en cours...");
            System.Threading.Thread.Sleep(2000);  // Simulation d'une pause de 2 secondes

            // Si l'opération réussit
            callback("Opération terminée avec succès.", true);
        }
        catch (Exception ex)
        {
            // En cas d'erreur, on passe le message d'erreur au callback
            callback($"Erreur : {ex.Message}", false);
        }
    }

    // Callback générique pour gérer succès ou erreur
    static void Callback(string message, bool success)
    {
        if (success)
        {
            Console.WriteLine("Succès: " + message);
        }
        else
        {
            Console.WriteLine("Échec: " + message);
        }
    }
}
```
Le multicast delegate permet de stocker plusieurs méthodes et de les exécuter en FIFO.
Dans le cas d'un multicast, les méthodes suivantes sont utiles:
- `.GetInvocationList()` permet de retourner un tableau de `delegate[]` chacun représentant une méthode liée.
- `.DynamicInvoke()` permet d'appeler une méthode clibée avec les argument correspondant: 
```C#
invocationList[1].DynamicInvoke("Sélectionner une méthode spécifique");
```  
 
### **Quelle est la différence entre un délégué et un événement en C# ?**
Un événement se base sur le fonctionnement d’un délégué, mais il existe des différences clés dans leur utilisation et leur gestion.

- Délégué : Un délégué est un type de référence qui peut être associé à une ou plusieurs méthodes avec des signatures spécifiques. Il sert essentiellement de pointeur de fonction, permettant d’invoquer des méthodes dynamiquement. Un délégué peut être invoqué directement, et on peut lui ajouter ou retirer des méthodes de façon flexible à tout moment.

- Événement : Un événement est un mécanisme de notification basé sur un délégué, mais avec des restrictions supplémentaires. Un événement permet à un objet de signaler qu’une action a eu lieu, et à d’autres objets (les abonnés) de réagir en conséquence. En revanche, un événement offre une sécurité supplémentaire en limitant la façon dont il peut être manipulé. Par exemple, on ne peut pas invoquer un événement directement de l'extérieur de la classe qui le déclare, ce qui protège les abonnés de manipulations indésirables.

Un événement déclare un délégué, mais empêche d'ajouter ou de supprimer des méthodes en dehors de la classe définissant l'événement. On peut utiliser des méthodes add et remove pour gérer les abonnements à l'événement.

```C#
public class MyClass
{
    public delegate void MyDelegate(string message);
    public event MyDelegate OnMessageReceived; // Déclaration d'un événement

    public void TriggerEvent()
    {
        OnMessageReceived?.Invoke("Un message est reçu");
    }
}

public class Program
{
    public static void Main()
    {
        MyClass myClass = new MyClass();
        myClass.OnMessageReceived += MessageHandler; // Abonnement à l'événement

        myClass.TriggerEvent();  // Déclenche l'événement
    }

    static void MessageHandler(string message)
    {
        Console.WriteLine(message);
    }
}
```

### **Qu'est-ce qu'un lambda expression en C# ? Donne un exemple.**
Fonction anonyme qui peut être définie dans le code directement. Notées (paramètre) => expression:

```C#
Func<int, int> square = x => x * x; // dans le cas où l’on veut un retour. Sinon:
Action<int> log = x => { Console.WriteLine($”number of try: {x}”) }
``` 
`Func<>` et `Action<>` sont basés sur des delegate.

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
IEnumerable:
- collection locales
- exécuté côté client
- récupère toute la collection puis applique les filtres

IQueryable:
- pour bdd
- transforme la requête en SQL 
- LINQ=>SQL, permet le Lazy Loading.
- Entity Framework (ORM => bdd gérée via des objets).

pour le lazy loading, les propriétés doivent être déclarées `virtual`.

### **Qu'est-ce que la réflexion (reflection) en C# ? Donne un exemple.**
Mécanisme qui permet de manipuler des types dynamiquement pendant l’exécution. Flexible mais coûteux en performance.

Utilisation: 
- Serialization / Deserialization dynamique
- Inspection et manipulation des types au rutime
- Tests unitaires et Mocking avancé

```C#
using System;
using System.Reflection;

class Program
{
    static void Main()
    {
        Type type = typeof(Person); // Obtenir le type de la classe
        Console.WriteLine($"Nom de la classe : {type.Name}");

        // Lister les propriétés de la classe
        foreach (PropertyInfo prop in type.GetProperties())
        {
            Console.WriteLine($"Propriété : {prop.Name} - Type : {prop.PropertyType}");
        }
    }
}

class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
}
```

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

### **Qu'est-ce que le pattern Observer en C# ? Donne un exemple.**
Même principe que l'Event, l'Observer repose sur une `Interface` alors que l'Event repose sur un `delegate`.

Pattern utilisé où un objet `Observable` comporte un ou une liste d'objets `Observer` pour lesquels il peut exécuter une méthode définie dans l'interface IObserver.

mécanique:
`public class Observable` comporte:
- une `list<IObserver>` d'observateurs de portée `private` pour contenir les abonnements.
- des méthodes publiques pour `.Add` et `.Remove` des éléments de la liste.
- une méthode `private void blablAction(...)` qui va effectuer l'action sur l'ensemble des objets dans la liste des obervateurs.

```C#
public interface IObserver  
{  
    void Update(string message);  
}  

public class Observable  
{  
    private List<IObserver> _observers = new();  

    public void AddObserver(IObserver observer) => _observers.Add(observer);  
    public void RemoveObserver(IObserver observer) => _observers.Remove(observer);  

    public void NotifyObservers(string message)  
    {  
        foreach (var observer in _observers)  
            observer.Update(message);  
    }  
}
```

### **Qu'est-ce que le pattern Dependency Injection en C# ? Donne un exemple.**

Une injection de dépendance (DI) est le fait de mettre en arugment d'un constructeur des éléments tel qu'une classe ou un objet déjà existant.

Cela fonctionne bien avec les services.
Un service représente une ou un ensemble d'actions transversales et réutilisable.
Pour le reste, métier avec état, on choisira une classe.
Un service est une construction mentale, il n'y a pas de syntaxe spécifique en C# pour la déclarer.

### **Qu'est-ce que le pattern Repository en C# ? Donne un exemple.**

Le pattern repository implémente les méthodes CRUD pour l'accès au données d'un projet. Il représente une interface à utiliser pour interagir avec les données projet et se base sur l'implémentation de la méthodo CRUD.


## Questions pratiques

### **Écris une méthode en C# qui inverse une chaîne de caractères.**

```C#
static char[] invertCharList(string message)
{
    char[] invertedCharBuffer = new char[message.Length];
    int length = message.Length;

    for (int i = 0; i < length; i++)
    {
        invertedCharBuffer[i] = message[length - 1 - i];  
    }

    return invertedCharBuffer;

}
```


### **Écris une méthode en C# qui vérifie si une chaîne de caractères est un palindrome.**

```C#
static bool palindromeCheck(string message)
{
    bool palindromeState = true;

    for (int i = 0 ; i < message.Length / 2 ; i++)
    {
        if (message[i] != message[message.Length - 1 - i])
        {
            palindromeState = false;
            break;
        }
    }
    return palindromeState;
}
```
### **Écris une méthode en C# qui calcule la factorielle d'un nombre.**

```C#
    static int facto(int number)
    {
        Console.WriteLine("+");
        int result = 1;
        while (number > 1) 
        {   
            result *= number;
            number--;
        }
        return result;
    }
```


### **Écris une méthode en C# qui trouve le nombre le plus grand dans un tableau d'entiers.**

```C#
    static int max(int[] numberTable)
    {
        int result = 0;

        foreach (var item in numberTable)
        {
            if (item > result) result = item;
        }

        // return numberTable.Max();

        return result;
    }
```

### **Écris une méthode en C# qui trie un tableau d'entiers en utilisant l'algorithme de tri à bulles.**

```C#
    static int[] bubbleSort(int[] table)
    {
        bool change = false;
        int length = table.Length;
        for (int i = 0; i < table.Length - 1; i++)
        {
            for (int j = 0; j < length - 1; j++)
            {
                if (table[j] > table[j + 1])
                {
                    int buff = table[j];
                    table[j] = table[j + 1];
                    table[j + 1] = buff;
                    change = true;
                }
            }
            length--;

            if (change == false) break;
        }

        return table;
    }
```

### **Écris une classe en C# qui implémente le pattern Singleton.**

```C#
public class Singleton
{
    private static readonly Singleton _instance = new Singleton(); // Unique instance
    private Singleton() { } // Empêche `new Singleton()`
    public static Singleton Instance => _instance; // Retourne l'instance unique
}
```

### **Écris une méthode en C# qui utilise LINQ pour filtrer une liste d'objets.**

```C#
var items = new List<object>{12,"Apple",7,"Banana",5,"Orange", 18,"Grape",3,"Peach"};

var resultsString = items.OfType<string>().ToList();
foreach (var item in resultsString) Console.WriteLine(item);

var resultsInt = items.OfType<int>().ToList();
foreach (var item in resultsInt) Console.WriteLine(item);
```



### **Écris une méthode en C# qui utilise async et await pour effectuer une requête HTTP.**

```C#
    static async Task<int[]> asyncAwait(int[] table)
    {
        return await Task.Run(() =>  bubbleSort(table));
    }
```

## **Questions sur les concepts de programmation**


### **Qu'est-ce que la programmation orientée objet (POO) ? Quels sont ses principes de base ?**

La programmation objet est basée sur l'existence de classes instantiables en objet. On peut ainsi, via l'objet créé, accéder à ses champs et méthodes. De ce concept émergent 4 piliers de la POO:

- Encapsulation : regrouper les données et les méthodes dans un même objet tout en masquant les détails internes.
- Abstraction : exposer uniquement les fonctionnalités nécessaires tout en cachant les implémentations internes.
- Héritage : permettre la création de nouvelles classes à partir d'anciennes pour réutiliser et étendre le code.
- Polymorphisme : permettre à un objet de se comporter de différentes manières en fonction de son type.

Via l'instanciation de classes, on peut accéder à ses champs et méthodes via l'objet créé. 

### **Quelle est la différence entre l'encapsulation, l'héritage et le polymorphisme en POO ?**

L'encapsulation permet de regrouper les données et méthodes dans un même objet tout en masquand les détails internes.

L'héritage permet à une classe de récupérer les champs et méthodes issus de la classe parent tout en implémentant de nouvelles méthodes et champs détaillés dans la classe enfant.

Le polymorphisme permet à une objet de se comporter de manière différente selon son type. on a le polymorphisme par héritage, par surcharge et par interface.


### **Qu'est-ce que le polymorphisme en C# ? Donne un exemple.**

Le polymorphisme permet de redéfinir un comportement en fonction d'éléments propres à l'objet créé.

```C#
using System;

public class Animal
{
    public virtual void FaireDuBruit() 
    {
        Console.WriteLine("L'animal fait du bruit.");
    }
}

public class Chien : Animal
{
    public override void FaireDuBruit() 
    {
        Console.WriteLine("Le chien aboie.");
    }
}

public class Chat : Animal
{
    public override void FaireDuBruit() 
    {
        Console.WriteLine("Le chat miaule.");
    }
}

class Program
{
    static void Main()
    {
        Animal monChien = new Chien();
        Animal monChat = new Chat();

        monChien.FaireDuBruit();  // Affiche : "Le chien aboie."
        monChat.FaireDuBruit();   // Affiche : "Le chat miaule."
    }
}

```

### **Qu'est-ce que l'encapsulation en C# ? Donne un exemple.**

L'encapsulation est le fait de délimiter la portée de champs ou de méthodes dans une classe:

```C#
class exemple
{
    public int exNumber;
    private int _exNumber;

}
```

| Modificateur           | Même classe | Classes dérivées | Même assembly | Partout |
|------------------------|:-----------:|:----------------:|:-------------:|:-------:|
| `private`             | ✅ Oui      | ❌ Non           | ❌ Non        | ❌ Non  |
| `protected`           | ✅ Oui      | ✅ Oui           | ❌ Non        | ❌ Non  |
| `internal`            | ✅ Oui      | ✅ Oui (dans le même assembly) | ✅ Oui | ❌ Non |
| `protected internal`  | ✅ Oui      | ✅ Oui (dans le même assembly) | ✅ Oui | ❌ Non |
| `private protected`   | ✅ Oui      | ✅ Oui (seulement dans le même assembly) | ❌ Non | ❌ Non |
| `public`              | ✅ Oui      | ✅ Oui           | ✅ Oui        | ✅ Oui  |



### **Qu'est-ce que l'abstraction en C# ? Donne un exemple.**



### **Quelle est la différence entre une classe abstraite et une interface en C# ?**

Une classe abstraite est une classe incomplète qui sert de base aux classes qui vont en hériter. 
Cette classe abstraite va donc pouvoir définir les champs et méthodes dont les classes et méthodes hériterons.

Une interface, défini la signature des méthodes et le champs que la classe qui l'implémente devra comporter. 

La classe abstraite détermine le comportement des méthodes, l'interface ne défini que la signature de ces méthodes.


### **Qu'est-ce que la composition en C# ? Donne un exemple.**

La coposition permet de créer une relation "A une" là où l'instanciation d'une classe créé une relation "est un".

Exemple voiture / moteur.

Au lieu d'hériter, le composition, permet à l'avoir voiture d'avoir un moteur. 
Cela se fait via l'intégration de l'instanciation de l'objet moteur dans la classe voiture.


### **Qu'est-ce que l'agrégation en C# ? Donne un exemple.**

Dans le même cadre que la composition, l'aggrégation se base sur l'ajout à un objet d'autres objets existants.

Exemple:

Dans le même cadre du couple voiture moteur, la voiture a un moteur, si la voiture disparait, son moteur aussi. Mais la voiture comporte également plusieurs conducteurs qui existent indépendemment de la voiture.


### **Qu'est-ce que le couplage et la cohésion en programmation ?**

Le couplage, à l'inverse de la composition est une pratique visant à réduire la dépendance de classe avec d'autres classes. On va sortir l'instanciation d'objets à relation "A un" d'une classe en créant des interfaces et en faisant l'instanciation de cet objet hors de la classe contenante.

La cohésion est une notion portant sur la réponsabilité d'une classe. Si sa responsabilité est limitée à son champs d'action, sa cohésion est forte. Si sa responsabilité est étendue (classe avec action spécifiques métier qui balayent plusieurs application) sa cohésion sera faible.


### **Qu'est-ce que le principe SOLID en programmation orientée objet ?**

bonne pratique.

- S : responsabilité unique, revient à maximiser la cohésion

- O : Open Close, revient ouvrir une classe à son extension mais fermée à la modification. Une classe doit pouvoir recevoir de nouvelles méthodes et champs défini mais ne dois pas être modifiée pour enrichir son comportement.

- L : substitution de liskov : on doit pouvoir utiliser une classe dérivée à la place de la mère sans que son comportant soit modifié.

- I : Une interface ne doit pas forcer une classe à implémenter des classes qu'elle n'utilise pas

- D : Inversion de dépendance, une classe ne doit pas dépendre d'un implémentation concrète mais d'une abstraction. revient à réduire la copmposition.


| Principe | Explication | Objectif |
|----------|------------|----------|
| **S** - Single Responsibility | Une classe = Une seule responsabilité | Code clair et facile à maintenir |
| **O** - Open/Closed | Ouvrir à l'extension, fermer à la modification | Éviter de modifier du code existant |
| **L** - Liskov Substitution | Une sous-classe doit respecter le comportement de sa superclasse | Garantir la cohérence des classes |
| **I** - Interface Segregation | Une classe ne doit pas être forcée d’implémenter des méthodes inutiles | Interfaces spécifiques et adaptées |
| **D** - Dependency Inversion | Dépendre des abstractions plutôt que des implémentations concrètes | Réduire le couplage et faciliter l’évolutivité |

| Principe SOLID | Composition | Agrégation | Couplage | Cohésion |
|---------------|------------|------------|----------|----------|
| **S** - Single Responsibility | ++ (permet de mieux séparer les responsabilités) | ++ (évite de surcharger une classe) | -- (un fort couplage complique la séparation des responsabilités) | ++ (une classe bien définie a une forte cohésion) |
| **O** - Open/Closed | + (facilite l'extension avec des objets réutilisables) | + (permet d’ajouter des fonctionnalités via des objets associés) | -- (modifier une classe couplée brise le principe) | + (une forte cohésion facilite l'extension sans modifier l'existant) |
| **L** - Liskov Substitution | + (permet de structurer proprement les objets substituables) | = (pas d’impact direct) | -- (un couplage fort peut empêcher la substitution correcte) | ++ (favorise des classes avec un comportement cohérent) |
| **I** - Interface Segregation | = (pas directement lié) | = (pas directement lié) | -- (un fort couplage force l'implémentation d’interfaces non pertinentes) | ++ (évite d’avoir des classes implémentant des méthodes inutiles) |
| **D** - Dependency Inversion | ++ (favorise l’injection de dépendances et la flexibilité) | ++ (évite la dépendance forte aux implémentations concrètes) | -- (le couplage aux implémentations concrètes viole ce principe) | + (une bonne séparation des responsabilités facilite l’inversion des dépendances) |

## **Questions sur les frameworks et outils**

### **Qu'est-ce que .NET Core et en quoi est-il différent de .NET Framework ?**

.Net Core est la nouvelle version open source, avec compatibilité étendue (pc, mac, linux) de .Net Framework.

### **Qu'est-ce que ASP.NET Core et quels sont ses avantages ?**

ASP.Net est un framwork Web dev pas microsoft.
ASP.Net Core est flexible et puissant et permet de créer des app web modernes et performantes.

### **Qu'est-ce que Entity Framework Core et comment est-il utilisé en C# ?**

Entity Framwork Core est un ORM (object relational mapper) qui permet de gérer l'interaction entre une app et une bdd.

### **Quelle est la différence entre Entity Framework et Dapper ?**

Dapper est un micro ORM, plus performant car ne comporte pas d'abstraction, il nécessite de créer ses requêtes SQL directement.

### **Qu'est-ce que le middleware en ASP.NET Core ?**

composant logiciel qui intercept, manipule ou modifie les requêtes HTTP. Authentication, authorization, Error Handling, Statif file mgt, routing & CORS.

### **Qu'est-ce que la dépendance injectée (Dependency Injection) en ASP.NET Core ?**



### **Qu'est-ce que le routage (routing) en ASP.NET Core ?**



### **Qu'est-ce que le modèle MVC (Model-View-Controller) en ASP.NET Core ?**

- Modèle: reflète le comportement métier
- Vue : représente grapiquement les éléments d'intercation utilisateur
- Controller: lie le modèle avec la vue. Gère le traitement des données pour qu'elles soient utilisables par la vue. Le controlleur n'as pas accès aux éléments de la vue

### **Qu'est-ce que le modèle MVVM (Model-View-ViewModel) en C# ?**

Même principe que le MVC mais le VueModel a un accès direct aux éléments de la vue (text box, button etc)

### **Qu'est-ce que le modèle Razor Pages en ASP.NET Core ?**

Razor Page intègre la logique controlleur dans la vue directement.

## **Questions sur les tests**

### **Qu'est-ce que les tests unitaires en C# ? Donne un exemple.**

Un test unitaire revient à tester le comportement et la sortie d'une unité spécifique (méthode) de manière isolée pour s'assurer qu'elle fonctionne correctement.
Cela permet de tester les interactions avec les dépendances.

On utilise des mocks ou stubs pour isoler la fonction du reste de l'application.

### **Quelle est la différence entre les tests unitaires et les tests d'intégration ?**

Test d'intégration test plusieurs unités ou composants du système (bdd, API, services).
On s'assure que les composants fonctionnent bien ensemble.

### **Qu'est-ce que le framework de test xUnit en C# ?**

xunit est un framework de test unitaire. Il permet de créer et d'exécuter des test automatisés pour vérifier le bon fonctionnement du code.

basé sur les balises [Fact], [Theory].

Simple, efficace et supporte l'injection de dépendance.

```C#
using Xunit;

public class CalculatorTests
{
    [Fact]
    public void Add_ReturnsCorrectSum()
    {
        var calculator = new Calculator();
        var result = calculator.Add(2, 3);
        Assert.Equal(5, result);
    }
}
```

### **Qu'est-ce que le framework de test NUnit en C# ?**

Comme xUnit mais plus ancien / mature, framwork de test unitaire, basé sur les `Assert.AreEqual()` ou `Assert.IsTrue()` etc..

```C#
using NUnit.Framework;

[TestFixture]
public class CalculatorTests
{
    [Test]
    public void Add_ReturnsCorrectSum()
    {
        var calculator = new Calculator();
        var result = calculator.Add(2, 3);
        Assert.AreEqual(5, result);
    }
}
```

### **Qu'est-ce que le framework de test MSTest en C# ?**

MSTest permet de créer des test unitaires. Il est étroitement lié à visual Studio et est souvent utilisé dans des projets Microsoft.

### **Qu'est-ce que le mocking en C# ? Donne un exemple.**

Le mockeing est une pratique qui vise à créer des objets factices pour simuler le comportement de dépendances externes.

### **Qu'est-ce que le framework Moq en C# ?**

c'est une librairie permettant le mocking pour .Net.

### **Qu'est-ce que le TDD (Test-Driven Development) ?**

c'est un design pattern qui vise à créer les tests pour les fonctionnalités souhaitées avant de les développer.
3 étapes:
- Ecrire le test
- Ecrire le code & faire le test
- Refactorer le code si nécessaire

### **Qu'est-ce que le BDD (Behavior-Driven Development) ?**

design pattern qui prend en compte, avant tout, le comportement de fonctionnalités prévues. Leur description doit être faite selon le GWT (given, when, then). Il favorise la collaboration métier (dev, testeur, resp métier) et mets en avant l'automatisation des tests.

### **Qu'est-ce que le code coverage en C# ?**

Le code coverage représente la partie du code couverte par les tests automatisés.

## **Questions sur les bonnes pratiques**

### **Quelles sont les bonnes pratiques pour écrire du code propre en C# ?**

- Utiliser des noms explicits et significatifs
- Respecter les convention de nommage
- Eviter les méthodes longues
- Utiliser l'injection de dépendance (faible couplage - injecter un service via le ctor vs inst dans la classe)
- Eviter les répétition, DRY
- Limiter la complexité dans le code
- Commenter mais pas de surcharge
- Gérer les exceptions
- Utiliser les test autom

### **Qu'est-ce que le principe DRY (Don't Repeat Yourself) en programmation ?**

Eviter la définitions d'élements qui se répettent, privilégier leur isolement pour utilisation multiple.

### **Qu'est-ce que le principe KISS (Keep It Simple, Stupid) en programmation ?**

Ne pas complexifier les méthodes, fonctionnement. 

### **Qu'est-ce que le principe YAGNI (You Aren't Gonna Need It) en programmation ?**

Ajouter les méthodes et fonctionnalités lorsqu'elles sont nécessaires, pas uniquement de base.

### **Qu'est-ce que le principe de séparation des préoccupations (Separation of Concerns) ?**

séparation du code en zone / modules spécifique. Augmentation de la cohésion.

### **Qu'est-ce que le principe de responsabilité unique (Single Responsibility Principle) ?**

le principe de responsabilité unique est le fait d'assigner un objectif fonctionnel à une classe.

### **Qu'est-ce que le principe ouvert/fermé (Open/Closed Principle) ?**

ouvert à l'enrichissement d'une classe mais fermé à la modification de l'existant. (evite la régression)

### **Qu'est-ce que le principe de substitution de Liskov (Liskov Substitution Principle) ?**

Les éléments de la classe parente ne doivent pas être modifié dans les classes héritantes.

### **Qu'est-ce que le principe d'inversion de dépendance (Dependency Inversion Principle) ?**

Une classe ne doit pas dépendre d'objets directement mais via des abstractions.
