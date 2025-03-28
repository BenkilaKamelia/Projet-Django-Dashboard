

# **📌 Explication des Vues Django - To-Do List 📝**

---

## **1️⃣ `TodoAppView(request)` → Affichage de la liste des tâches 📋**
```python
def TodoAppView(request):
    all_items = Item.objects.all()
    print(all_items)
    return render(request, 'todolist.html', {'all_items': all_items, 'ACTION_URL': '/todo/'})
```
### **🔎 Explication :**  
- 🗂 **`all_items = Item.objects.all()`** → Récupère **toutes les tâches** depuis la base de données.
- 🖥 **`print(all_items)`** → Affiche les éléments récupérés dans la console (pour débogage).
- 🎨 **`render(request, 'todolist.html', {...})`** → Affiche la page `todolist.html` avec :  
  - **📌 `all_items`** : Toutes les tâches à afficher.  
  - **🔗 `ACTION_URL`** : URL du formulaire pour ajouter une nouvelle tâche.

---

## **2️⃣ `AddTodo(request)` → Ajouter une tâche ➕**
```python
def AddTodo(request):
    new_item = Item(content=request.POST['content'])
    if request.POST['content'].strip() != '':
        new_item.save()
    return HttpResponseRedirect('/')
```
### **🔎 Explication :**  
- 📝 **`request.POST['content']`** → Récupère le contenu entré par l’utilisateur.
- 🆕 **`new_item = Item(content=request.POST['content'])`** → Crée une **nouvelle tâche**.
- ❌ **`if request.POST['content'].strip() != '':`** → Vérifie que le champ **n’est pas vide**.
- 💾 **`new_item.save()`** → Sauvegarde la tâche dans la base de données.
- 🔄 **`return HttpResponseRedirect('/')`** → Redirige l’utilisateur vers la page d’accueil après l’ajout.

---

## **3️⃣ `DeleteTodo(_, item_id)` → Supprimer une tâche 🗑**
```python
def DeleteTodo(_, item_id):
    item_to_delete = Item.objects.get(id=item_id)
    item_to_delete.delete()
    return HttpResponseRedirect('/')
```
### **🔎 Explication :**  
- 🆔 **`item_id`** → Identifiant de la tâche à supprimer.
- 🔍 **`item_to_delete = Item.objects.get(id=item_id)`** → Récupère la tâche spécifique.
- ❌ **`item_to_delete.delete()`** → Supprime la tâche de la base de données.
- 🔄 **`return HttpResponseRedirect('/')`** → Redirige l’utilisateur vers la liste des tâches.

> **💡 Remarque :** Le `_` dans `(_, item_id)` signifie qu’on ignore `request`, car on n’en a pas besoin ici.

---

## **4️⃣ `EditTodo(request, item_id)` → Modifier une tâche ✏️**
```python
def EditTodo(request, item_id):
    all_items = Item.objects.all()
    item_to_edit = Item.objects.get(id=item_id)
    return render(request, 'todolist.html', {'edit_item': item_to_edit, 'all_items': all_items})
```
### **🔎 Explication :**  
- 📂 **`all_items = Item.objects.all()`** → Récupère toutes les tâches (affichage de la liste).
- 🔍 **`item_to_edit = Item.objects.get(id=item_id)`** → Récupère la tâche **spécifique** à modifier.
- 🎨 **`render(request, 'todolist.html', {...})`** → Affiche la page avec :
  - **🖍 `edit_item`** : La tâche en cours de modification.
  - **📌 `all_items`** : Toutes les autres tâches.

> **💡 Cette vue sert à afficher un **formulaire pré-rempli** pour modifier une tâche.**

---

## **5️⃣ `UpdateTodoItem(request, item_id)` → Mettre à jour une tâche 🔄**
```python
def UpdateTodoItem(request, item_id):
    item_to_update = Item.objects.get(id=item_id)
    item_to_update.content = request.POST['content']
    item_to_update.save()
    return HttpResponseRedirect('/')
```
### **🔎 Explication :**  
- 🆔 **`item_id`** → Identifiant de la tâche à modifier.
- 🔍 **`item_to_update = Item.objects.get(id=item_id)`** → Récupère l’élément concerné.
- 📝 **`item_to_update.content = request.POST['content']`** → Met à jour le texte de la tâche.
- 💾 **`item_to_update.save()`** → Sauvegarde les modifications.
- 🔄 **`return HttpResponseRedirect('/')`** → Redirige vers la page principale.

---

# **📊 Récapitulatif des fonctionnalités 🚀**
| 🆔 Vue | ✨ Fonction | 📝 Description |
|------|----------|-------------|
| `TodoAppView(request)` | 📋 Affichage des tâches | Affiche la liste des tâches existantes. |
| `AddTodo(request)` | ➕ Ajouter une tâche | Ajoute une nouvelle tâche à la base de données. |
| `DeleteTodo(_, item_id)` | 🗑 Supprimer une tâche | Supprime une tâche spécifique selon son ID. |
| `EditTodo(request, item_id)` | ✏️ Modifier une tâche | Affiche le formulaire de modification d’une tâche spécifique. |
| `UpdateTodoItem(request, item_id)` | 🔄 Mettre à jour une tâche | Modifie une tâche avec les nouvelles informations. |

---

**🎯 En résumé :**  
✅ Ce projet Django permet de gérer une **To-Do List** avec des actions CRUD (Create, Read, Update, Delete).  
✅ Simple et efficace pour débuter avec Django et les bases de données.  

