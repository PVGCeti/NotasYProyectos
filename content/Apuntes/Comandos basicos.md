---
title: Comandos basicos
---
Espacio dedicado a la recopilación de comandos básicos organizados por categorías como Git, MongoDB y más. Cada comando incluye una breve descripción y está diseñado para facilitar el acceso rápido a la información, optimizando así tu flujo de trabajo en el desarrollo.

- [[#Comandos Git]]
- [[#Comandos MongoDB]]
## Comandos Git

- `git init` ==> Crea un repositorio de código local
- `git add <file> / . / *` ==> Añade ese archivo al control de versiones (<b>.</b> añade todos los del directorio | <b>*</b> añade todos)
- `git commit -m <mensaje>` ==> Subo un cambio con un mensaje
- `git status` ==> Muestra el estado en el que está el repositorio
- `git log` ==> Muestra los commits realizados y su mensaje asociado (Persona, email, fecha)
- `git diff` ==> Muestra las diferencias del archivo
- `git log --oneline` ==> Lo muestra todo en la misma línea
- `git show <codigo commit>` ==> Muestra más detalles del commit
- `git push` ==> Lo sube al repositorio remoto
- `git restore <file> / . / *` ==> Restaura el archivo hasta el último commit
- `git restore --staged <file>` ==> Lo saca del stage
- `git revert <codigo de commit>` ==> Vuelve a antes de ese commit, es decir, revierte el commit
- `git show <commit code>` ==> Muestra los cambios del commit
- `gitk` ==> Muestra un HUD de git
- `git branch <nombre>` ==> Crea una rama 
- `git branch` ==> Muestra todas las ramas locales
- `git branch -r` ==> Muestra todas las ramas remotas
- `git branch -a` ==> Muestra todas las ramas y en la que estás
- `git branch -d <nombre-rama>` ==> Elimina la rama
- `git checkout <nombre-rama>` ==> Cambiar a la rama `<b><nombreRama></b>`
- `git merge <nombre-rama>` ==> Une la rama, con la rama actual
- `git checkout -b <nombre-rama>` ==> Crea la rama `<b><nombre-rama></b>` y nos mueve a ella
- `git remote add origin` ==> Crea el repositorio online
- `git push origin/<repositorio> <rama>` ==> Envía los cambios
- `git push origin/<repositorio> :<rama>` ==> Borra la rama en el repositorio remoto
- `git commit -a -m "<mensaje>"` ==> Hace el add y el commit
- `git tag -a <version> -m "<mensaje>"` ==> Referencia el último commit como un tag
- `git push origin --tags` ==> Sube el tag al repositorio remoto
- `git fetch --tags` ==> Descargas los tags al repositorio local

#### Configurar Git

- `git config --global user.name <nombre>` ==> Cambiamos el nombre de usuario en todos los dispositivos
- `git config --global user.email <email>` ==> Cambiamos el email en todos los dispositivos


## Comandos MongoDB

- `mongosh` ==> Para abrir MongoDB en PowerShell.
- Ver bases de datos: `show databases` / `show dbs`
- Ver la base de datos en la que estamos actualmente: `db`
- Borrar tabla: `db.<nombre base de datos>.drop()`
- Crear base de datos: `use <nombre de la base de datos>`
- Insertar valor a las tablas: `db.<nombre tabla>.insertOne({ <JSON> })`
- Insertar más de un valor a la tabla: `db.<nombre tabla>.insertMany([{ objeto 1 }, { objeto 2 }])`
- Ver las tablas de nuestra base de datos: `show collections` / `show tables`
- Mostrar objetos de una tabla: `db.<nombre tabla>.find()`
- Mostrar objetos de una tabla de forma bonita (cambia la indentación): `db.<nombre tabla>.find().pretty()`

#### Consultas

- Hacer una query/filtrar por un solo parámetro: 
  - `db.<nombre tabla>.find({ "atributo": "valor"})` 
  - `db.inventory.find({ "status": "A"})` (buscando por el atributo "status" que tenga el valor "A")

- Hacer una query/filtrar por un solo parámetro con más de un valor: 
  - `db.<nombre tabla>.find( { "atributo": { $in: [ "valor 1", "valor 2" ] } } )`
  - `db.inventory.find( { status: { $in: [ "A", "D" ] } } )` ($in significa que se encuentra en ese array de valores)

- Hacer una query/filtrar por más de un parámetro: 
  - `db.<nombre tabla>.find({"atributo 1": "valor 1", "atributo 2": "valor 2"})`
  - `db.inventory.find({"status": "A", "qty": 95})`

- Mostrar objetos de una tabla donde un valor sea menor a X: 
  - `db.<nombre tabla>.find( { "atributo": "valor", "atributo": { $lt: "valor que tiene que ser menor" } } )`
  - `db.inventory.find( { status: "A", qty: { $lt: 30 } } )`

- Mostrar objetos de una tabla por más de un parámetro sin necesidad de tener ambos: 
  - `db.<nombre tabla>.find({$or: [{"atributo 1": "valor 1"}, {"atributo 2":"valor 2"}]})`
  - `db.inventory.find({$or: [{status: "A"}, {qty:45}]})`

#### Actualización y Borrado

- Actualizar un elemento de una tabla: 
  - `db.<nombre tabla>.updateOne({"elemento"}, {$set: {"actualizacion"}})`
  - `db.inventory.updateOne({item: "paper"}, {$set: {status: "T"}})` (actualizamos el elemento "paper", cambiando su status a "T")

- Actualizar más de un elemento de una tabla: 
  - `db.<nombre tabla>.updateMany({qty: {$lt: 50}}, {$set: {status: "T"}})`

- Borrar un elemento: 
  - `db.<nombre tabla>.deleteOne({"atributo": "valor"})`
  - `db.inventory.deleteOne({"item": "paper"})` (borra solo un elemento, cuando encuentra uno)

- Borrar más de un elemento: 
  - `db.<nombre tabla>.deleteMany({"atributo": "valor"})`
  - `db.inventory.deleteMany({"status": "A"})` (borra todos los elementos, no para después de encontrar 1)
