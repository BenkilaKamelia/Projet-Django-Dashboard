### Explication des méthodes HTTP : **POST, PUT, DELETE, GET**

Les méthodes HTTP permettent de communiquer avec un serveur et d'effectuer différentes opérations sur les ressources. Voici une explication détaillée des méthodes **POST**, **PUT**, **DELETE**, et **GET** :

---

## 🔹 1. GET - Récupérer des données

📌 **Description** :  
La méthode **GET** est utilisée pour récupérer des données à partir d'un serveur. C'est la méthode la plus courante utilisée pour afficher du contenu dans un navigateur.

📌 **Caractéristiques** :
- Elle ne modifie pas les données sur le serveur.
- Les données sont envoyées via l'URL (ex. `?id=5`).
- Les requêtes **GET** peuvent être mises en cache.

📌 **Exemple en Django** :

```python
from django.http import JsonResponse
from todo.models import Item

def get_todos(request):
    todos = list(Item.objects.values())  # Convertir les objets en liste de dictionnaires
    return JsonResponse({'todos': todos})
```

📌 **Requête avec Fetch API (JavaScript)** :

```javascript
fetch('/api/todos/')
    .then(response => response.json())
    .then(data => console.log(data));
```

---

## 🔹 2. POST - Ajouter une ressource

📌 **Description** :  
La méthode **POST** est utilisée pour **envoyer** des données au serveur afin de créer une nouvelle ressource.

📌 **Caractéristiques** :
- Elle **envoie des données** dans le **corps** de la requête.
- Elle **ne peut pas être mise en cache**.
- Elle est utilisée pour **soumettre un formulaire**, **ajouter un élément** à une base de données.

📌 **Exemple en Django** :

```python
from django.http import JsonResponse
from django.views.decorators.csrf import csrf_exempt
import json
from todo.models import Item

@csrf_exempt  # Désactiver la protection CSRF pour cet exemple
def add_todo(request):
    if request.method == 'POST':
        data = json.loads(request.body)
        new_item = Item(content=data['content'])
        new_item.save()
        return JsonResponse({'message': 'Todo ajouté avec succès', 'id': new_item.id})
```

📌 **Requête avec Fetch API (JavaScript)** :

```javascript
fetch('/api/todos/', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json'
    },
    body: JSON.stringify({ content: "Acheter du lait" })
})
.then(response => response.json())
.then(data => console.log(data));
```

---

## 🔹 3. PUT - Modifier une ressource existante

📌 **Description** :  
La méthode **PUT** est utilisée pour **mettre à jour** une ressource existante en envoyant **toutes les nouvelles données**.

📌 **Caractéristiques** :
- Elle **remplace complètement** une ressource existante.
- Elle est souvent utilisée pour **mettre à jour une tâche** dans une liste.

📌 **Exemple en Django** :

```python
@csrf_exempt
def update_todo(request, item_id):
    if request.method == 'PUT':
        data = json.loads(request.body)
        try:
            item = Item.objects.get(id=item_id)
            item.content = data['content']
            item.save()
            return JsonResponse({'message': 'Todo mis à jour avec succès'})
        except Item.DoesNotExist:
            return JsonResponse({'error': 'Todo non trouvé'}, status=404)
```

📌 **Requête avec Fetch API (JavaScript)** :

```javascript
fetch('/api/todos/1/', {
    method: 'PUT',
    headers: {
        'Content-Type': 'application/json'
    },
    body: JSON.stringify({ content: "Acheter du pain" })
})
.then(response => response.json())
.then(data => console.log(data));
```

---

## 🔹 4. DELETE - Supprimer une ressource

📌 **Description** :  
La méthode **DELETE** est utilisée pour supprimer une ressource spécifique.

📌 **Caractéristiques** :
- Elle **supprime définitivement** une ressource du serveur.
- Elle est souvent utilisée pour **supprimer une tâche** d'une liste de tâches.

📌 **Exemple en Django** :

```python
@csrf_exempt
def delete_todo(request, item_id):
    if request.method == 'DELETE':
        try:
            item = Item.objects.get(id=item_id)
            item.delete()
            return JsonResponse({'message': 'Todo supprimé avec succès'})
        except Item.DoesNotExist:
            return JsonResponse({'error': 'Todo non trouvé'}, status=404)
```

📌 **Requête avec Fetch API (JavaScript)** :

```javascript
fetch('/api/todos/1/', {
    method: 'DELETE'
})
.then(response => response.json())
.then(data => console.log(data));
```

---

## 📌 Récapitulatif

| Méthode  | Utilisation |
|----------|------------|
| **GET**    | Récupérer des données (lecture seule) |
| **POST**   | Ajouter une nouvelle ressource |
| **PUT**    | Mettre à jour complètement une ressource existante |
| **DELETE** | Supprimer une ressource |

### Exemple de routes Django :

```python
from django.urls import path
from todo.views import get_todos, add_todo, update_todo, delete_todo

urlpatterns = [
    path('api/todos/', get_todos, name='get_todos'),
    path('api/todos/', add_todo, name='add_todo'),
    path('api/todos/<int:item_id>/', update_todo, name='update_todo'),
    path('api/todos/<int:item_id>/', delete_todo, name='delete_todo'),
]
```

---

## 🚀 Conclusion

Ces méthodes sont essentielles pour créer des **API REST** en Django et permettent d'interagir efficacement avec une base de données. En combinant Django avec **Fetch API**, vous pouvez facilement **ajouter**, **modifier**, **récupérer** et **supprimer** des tâches dans une application To-Do List. 🎯
