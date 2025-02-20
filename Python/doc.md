# 50 Questions Techniques sur Python  

## Syntaxe et Bases  
1. Quelle est la différence entre `is` et `==` ? 

`==` compare des valeurs, `is` compare des identités.

2. Comment déclarer une fonction en Python ?  

avec le mot clé `def ma_fonction(params..):`

3. Quelle est la différence entre une liste et un tuple ?  

une liste est modifiable, un tuple est immuable

4. Que retourne `bool([])`, `bool({})` et `bool(0)` ? 

False car vide. bool() détecte les éléments vide à false.

5. Comment inverser une liste en Python ? 

avec:
- [::-1]
- .reverse()
- reversed()

6. Quelle est la différence entre `deepcopy` et `copy` ?  

copy copie les référrences
deepcopy copie les valeurs

7. Comment fusionner deux dictionnaires en Python 3.9+ ?  

- avec l'opérateur `|`
- avec .update()
- avec ** => fusion = {**d1, **d2}


8. Quelle est la sortie de `print(3 * 'abc')` ?

"abcabcabc"

9. Qu'est-ce qu'un `lambda` en Python ?  

un lambda est une fonction anonyme.
```python
lambda arguments : expression

# Fonction lambda qui additionne deux nombres
addition = lambda x, y: x + y
print(addition(3, 5))  # 8

# Trier une liste de tuples par le deuxième élément
liste = [(1, 2), (3, 1), (5, 4)]
liste_triee = sorted(liste, key=lambda x: x[1])
print(liste_triee)  # [(3, 1), (1, 2), (5, 4)]
```

10. Quelle est la sortie de `[i for i in range(3)] == list(range(3))` ?  

True.

- [i for i in range(3)] est une compréhension de liste qui génère une liste contenant les valeurs [0, 1, 2], car range(3) produit les nombres 0, 1, 2.

- list(range(3)) crée également une liste à partir de range(3), soit [0, 1, 2].

## Structures de Contrôle et Boucles  
11. Comment sortir d’une boucle `for` prématurément ?  

Avec un `break`.

12. Quelle est la différence entre `continue` et `pass` ?

continue permet de stopper l'itération en cours et de passer à la suivante.

pass est un placeholder utiliser pour maintenir la structure du code.

13. Peut-on avoir un `else` après une boucle `for` ?  

Oui, le `else` sera exécuté si la boucle `for` est terminée "naturellement", si l'exécution ne rencontre pas de `break`.

14. Comment créer un générateur en Python ?

Un générateur permet de produire une séquence de valeur au fur et à mesure de l'exécution via un `yield`.

le générateur doit être initialisé (stocké dans une variable) et intéré grâce à `next()`.

ou peut être utilisé avec un for:
```python
for val in compte():
    print(val)
```

15. Quelle est la différence entre `map()` et `list comprehension` ?  

`map(fonction, iterable1, iterable2...)` permet de donner à une fonction des itérables qui seront itérés. les résultats sont donnés en sortie de la fonction.

dans le cas où un itérable devrait être une constance, utiliser `itertools.repeat(valeur)`.

ou

```python 
map(lambda x, y: x + y, numbers, [constant] * len(numbers))
```

et pour la list comprehension:
```python
result = [x ** 2 for x in numbers]
```

## Gestion des Erreurs et Exceptions  
16. Comment lever une exception personnalisée en Python ?  

En créant une classe d'exception héritant de (Exception):

```C#
# Définition d'une exception personnalisée
class MonException(Exception):
    def __init__(self, message="Une erreur personnalisée s'est produite"):
        # On peut ajouter des détails ou des messages personnalisés
        self.message = message
        super().__init__(self.message)

# Lever l'exception personnalisée
def test_function():
    raise MonException("C'est une exception personnalisée !")

try:
    test_function()
except MonException as e:
    print(f"Erreur capturée : {e}")
```

17. Que fait le bloc `finally` dans un `try-except` ?  
18. Quelle est la différence entre `assert` et `raise` ?  
19. Peut-on capturer plusieurs exceptions dans un seul `except` ?  
20. Quelle est la meilleure pratique pour attraper des exceptions spécifiques ?  

## Programmation Orientée Objet (OOP)  
21. Quelle est la différence entre `@staticmethod` et `@classmethod` ?  
22. Comment hériter d'une classe en Python ?  
23. Qu’est-ce que le `self` dans une classe Python ?  
24. Comment définir un attribut privé dans une classe ?  
25. Quelle est la différence entre une méthode d'instance et une méthode de classe ?  

## Gestion de la Mémoire et Performance  
26. Qu’est-ce que le ramasse-miettes en Python ?  
27. Comment éviter les références circulaires en Python ?  
28. Qu'est-ce que `sys.getsizeof()` ?  
29. Quelle est la différence entre `del` et `gc.collect()` ?  
30. Comment utiliser les générateurs pour économiser de la mémoire ?  

## Manipulation de Fichiers et Systèmes  
31. Comment lire un fichier ligne par ligne en Python ?  
32. Quelle est la différence entre `open('file.txt', 'r')` et `open('file.txt', 'rb')` ?  
33. Comment lister tous les fichiers d’un dossier en Python ?  
34. Quelle bibliothèque utiliser pour manipuler des fichiers ZIP ?  
35. Comment vérifier si un fichier existe en Python ?  

## Modules et Imports  
36. Quelle est la différence entre `import module` et `from module import *` ?  
37. Comment créer un module Python ?  
38. Que fait `if __name__ == "__main__":` ?  
39. Comment recharger un module sans redémarrer l’interpréteur Python ?  
40. Quelle est la différence entre un package et un module ?  

## Multithreading et Multiprocessing  
41. Quelle est la différence entre `threading` et `multiprocessing` ?  
42. Qu’est-ce que le GIL en Python ?  
43. Comment créer un thread en Python ?  
44. Quand utiliser `asyncio` au lieu de `threading` ?  
45. Comment partager des variables entre processus en Python ?  

## Outils et Bonnes Pratiques  
46. Qu’est-ce que `venv` en Python ?  
47. Comment profiler un script Python ?  
48. Quelle est la différence entre `pip` et `conda` ?  
49. Comment gérer les dépendances d’un projet Python ?  
50. Comment écrire un test unitaire en Python avec `unittest` ?  
