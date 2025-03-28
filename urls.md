
## 🌐 **Configuration des URLs de l'application To-Do**
Ce fichier **définit les routes** (URL) de votre application Django et **associe chaque URL à une fonction** dans `views.py`.  

### 🔹 **Imports nécessaires**
```python
from django.contrib import admin
from django.urls import path
from todo.views import DeleteTodo, EditTodo, TodoAppView, AddTodo, UpdateTodoItem
```
📌 **Explication :**  
- `django.contrib.admin` → Active l'interface d'administration Django.  
- `django.urls.path` → Permet de définir les routes (URLs).  
- `todo.views` → Importe les vues nécessaires de l'application `todo` :  
  - `DeleteTodo` → Supprime une tâche.  
  - `EditTodo` → Ouvre une tâche en mode édition.  
  - `TodoAppView` → Affiche la liste des tâches.  
  - `AddTodo` → Ajoute une nouvelle tâche.  
  - `UpdateTodoItem` → Met à jour une tâche existante.  

---

### 🚀 **Définition des URLs**
```python
urlpatterns = [
    path("admin/", admin.site.urls),
    path("", TodoAppView, name="todolist"),
    path("todo/", AddTodo, name="addTodo"),
    # Delete todo
    # Edit todo
    # Update todo
]
```
📌 **Explication des routes :**  
1️⃣ `path("admin/", admin.site.urls)`  
   - Active l'interface d'administration Django.  
   - Accessible via **`/admin/`**.  

2️⃣ `path("", TodoAppView, name="todolist")`  
   - Route **principale** (`""`, c'est-à-dire `/`).  
   - Affiche la liste des tâches en appelant la vue `TodoAppView`.  
   - Utilise `name="todolist"` pour être référencé ailleurs (ex: `url 'todolist'`).  

3️⃣ `path("todo/", AddTodo, name="addTodo")`  
   - **Ajoute une nouvelle tâche** via un formulaire.  
   - Appelle la vue `AddTodo`.  
   - Accessible via **`/todo/`**.  
   - Peut être utilisé dans les templates avec `{% url 'addTodo' %}`.  

4️⃣ **Commentaires pour les routes manquantes**  
   - `# Delete todo` → Devrait être une route pour supprimer une tâche.  
   - `# Edit todo` → Devrait être une route pour modifier une tâche existante.  
   - `# Update todo` → Devrait être une route pour sauvegarder les modifications d’une tâche.  

💡 **Remarque :** Ces routes sont incomplètes ! Il manque les URL pour **supprimer, éditer et mettre à jour** une tâche.  

---

## ✅ **Amélioration du fichier avec les routes manquantes**
Pour compléter le fichier, on peut ajouter les routes suivantes :
```python
urlpatterns = [
    path("admin/", admin.site.urls),
    path("", TodoAppView, name="todolist"),
    path("todo/add/", AddTodo, name="addTodo"),
    path("todo/delete/<int:item_id>/", DeleteTodo, name="deleteTodo"),
    path("todo/edit/<int:item_id>/", EditTodo, name="editTodo"),
    path("todo/update/<int:item_id>/", UpdateTodoItem, name="updateTodoItem"),
]
```
📌 **Explication des ajouts :**  
- `"todo/add/"` → Ajoute une nouvelle tâche.  
- `"todo/delete/<int:item_id>/"` → Supprime une tâche avec un ID spécifique.  
- `"todo/edit/<int:item_id>/"` → Charge une tâche existante pour l'éditer.  
- `"todo/update/<int:item_id>/"` → Met à jour une tâche après modification.  

---

## 🎯 **Résumé**
- Ce fichier **connecte** les URLs de l’application avec les **vues** correspondantes.  
- Chaque `path()` définit une **route**, associée à une **vue Django**.  
- L’application manque encore certaines routes (`delete`, `edit`, `update`).  
- Il est recommandé d’ajouter ces routes pour avoir une **To-Do List complète**. 🚀✅
