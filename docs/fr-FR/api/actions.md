# @nocobase/actions

## Vue d'ensemble

Le package `@nocobase/actions` encapsule les méthodes liées au CRUD fréquemment utilisées. En l'enregistrant simplement auprès du [ResourceManager](./resourcer/resource-manager), les interfaces d'opération CRUD peuvent être globalement ajoutées aux ressources système.

### Utilisation de base

```typescript
import * as actions from `@nocobase/actions`;

const resourceManager = new ResourceManager({
  // ...options
});

resourceManager.registerActionHandlers(actions);
```

## Méthode d'Action

### create

Crée une ressource. `POST /api/<resource>:create`.

```shell
curl "http://localhost:13000/api/users:create" \
  -X POST \
  -H "Content-Type: application/json" \
  -d '{"username": "admin"}'
```

#### Corps de la requête

| Nom du paramètre  | Type  | Description                                                |
| ----------------- | ----- | ---------------------------------------------------------- |
| `[key: string]`   | `any` | Paires clé-valeur représentant les champs de ressources    |

### list

Récupère une liste de ressources. `GET /api/<resource>:list`.

```shell
curl -X GET http://localhost:13000/api/users:list
```

#### Paramètres

| Paramètre  | Type       | Description                                                           | Défaut  |
| ---------- | ---------- | --------------------------------------------------------------------- | ------- |
| `filter`   | `Filter`   | Paramètres de filtrage, voir [Filter Operators](./database/operators) | -       |
| `fields`   | `string[]` | Champs à récupérer                                                    | -       |
| `except`   | `string[]` | Champs à exclure                                                      | -       |
| `appends`  | `string[]` | Champs associés à ajouter                                             | -       |
| `sort`     | `string[]` | Paramètres de tri                                                     | -       |
| `paginate` | `boolean`  | S'il faut utiliser la pagination                                      | `true`  |
| `page`     | `number`   | Numéro de page actuel                                                 | `1`     |
| `pageSize` | `number`   | Nombre d'éléments par page                                            | `20`    |

### get

Récupère une ressource spécifique. `GET /api/<resource>:get`.

```shell
curl -X GET http://localhost:13000/api/users:get?filterByTk=1
```

#### Paramètres

| Paramètre    | Type               | Description                                                           | Défaut  |
| ------------ | ------------------ | --------------------------------------------------------------------- | ------- |
| `filterByTk` | `number \| string` | Filtrer par valeur de clé primaire                                    | -       |
| `filter`     | `Filter`           | Paramètres de filtrage, voir [Filter Operators](./database/operators) | -       |
| `fields`     | `string[]`         | Champs à récupérer                                                    | -       |
| `except`     | `string[]`         | Champs à exclure                                                      | -       |
| `appends`    | `string[]`         | Champs associés à ajouter                                             | -       |

### update

Met à jour une ou plusieurs ressources. `PUT /api/<resource>:update`.

```shell
curl "http://localhost:13000/api/users:update?filterByTk=1" \
  -X PUT \
  -H "Content-Type: application/json" \
  -d '{"username": "admin"}'
```

#### Paramètres

| Paramètre    | Type               | Description                                                              |
| ------------ | ------------------ | ------------------------------------------------------------------------ |
| `filter`     | `Filter`           | Paramètres de filtrage, voir [Filter Operators](./database/operators)    |
| `filterByTk` | `number \| string` | Filtrer par valeur de clé primaire                                       |

*Remarque : Au moins un des éléments « filter » ou « filterByTk » doit être fourni.*

#### Corps de la requête

| Nom du paramètre  | Type  | Description                                                 |
| ----------------- | ----- | ----------------------------------------------------------- |
| `[key: string]`   | `any` | Paires clé-valeur représentant les champs de ressources     |

### destroy

Supprime une ou plusieurs ressources. `DELETE /api/<resource>:destroy`.

```shell
curl -X DELETE http://localhost:13000/api/users:destroy?filterByTk=1
```

#### Paramètres

| Paramètre    | Type               | Description                                                           |
| ------------ | ------------------ | --------------------------------------------------------------------- |
| `filter`     | `Filter`           | Paramètres de filtrage, voir [Filter Operators](./database/operators) |
| `filterByTk` | `number \| string` | Filtrer par valeur de clé primaire                                    |

*Remarque : Au moins un des éléments « filter » ou « filterByTk » doit être fourni.*

### firstOrCreate

Récupère ou crée une ressource. `POST /api/<resource>:firstOrCreate`.

```shell
curl "http://localhost:13000/api/users:firstOrCreate?filterKeys[]=username" \
  -X POST \
  -H "Content-Type: application/json" \
  -d '{"username": "admin", "nickname": "Admin"}'
```

#### Paramètres

| Paramètre    | Type       | Description                                                                      |
| ------------ | ---------- | -------------------------------------------------------------------------------- |
| `filterKeys` | `string[]` | Champs du corps de la requête utilisés pour rechercher des ressources existantes |

#### Corps de la requête

| Nom du paramètre | Type  | Description                                             |
| ---------------- | ----- | ------------------------------------------------------- |
| `[key: string]`  | `any` | Paires clé-valeur représentant les champs de ressources |

### updateOrCreate

Met à jour ou crée une ressource. `POST /api/<resource>:updateOrCreate`.

```shell
curl "http://localhost:13000/api/users:updateOrCreate?filterKeys[]=username" \
  -X POST \
  -H "Content-Type: application/json" \
  -d '{"username": "admin", "nickname": "Admin"}'
```

#### Paramètres

| Paramètre    | Type       | Description                                                                      |
| ------------ | ---------- | -------------------------------------------------------------------------------- |
| `filterKeys` | `string[]` | Champs du corps de la requête utilisés pour rechercher des ressources existantes |

#### Corps de la requête

| Nom du paramètre   | Type  | Description                                                |
| ------------------ | ----- | ---------------------------------------------------------- |
| `[key: string]`    | `any` | Paires clé-valeur représentant les champs de ressources    |

### move

Déplace les ressources en ajustant l'ordre. Généralement utilisé pour le tri par glisser-déposer dans les pages. `POST /api/<resource>:move`.

```shell
curl -X POST "http://localhost:13000/api/users:move?sourceId=1&targetId=2"
```

#### Paramètres

| Paramètre     | Type                       | Description                                                            | Défaut  |
| ------------- | -------------------------- | ---------------------------------------------------------------------- | ------- |
| `sourceId`    | `targetKey`                | ID de l'élément à déplacer                                             | -       |
| `targetId`    | `targetKey`                | ID de l'élément avec lequel échanger les positions                     | -       |
| `sortField`   | `string`                   | Nom du champ où est stocké le tri                                      | `sort`  |
| `targetScope` | `string`                   | Portée de tri, une ressource peut être triée selon différentes portées | -       |
| `sticky`      | `boolean`                  | S'il faut coller l'élément déplacé                                     | -       |
| `method`      | `insertAfter` \| `prepend` | Méthode d'insertion : s'il faut insérer avant                          |

### set

Définit les objets associés à une ressource. `POST /api/<resource>/<resourceKey>/<association>:set`.

```shell
curl "http://localhost:13000/api/users/1/roles:set" \
  -X POST \
  -H "Content-Type: application/json" \
  -d '["admin", "member"]'
```

#### Corps de la requête

- `TargetKey | TargetKey[]` - Tableau de valeurs de clé primaire pour les objets associés.

### add

Ajoute des objets associés à une ressource. `POST /api/<resource>/<resourceKey>/<association>:add`.

```shell
curl "http://localhost:13000/api/users/1/roles:add" \
  -X POST \
  -H "Content-Type: application/json" \
  -d '["admin"]'
```

#### Corps de la requête

- `TargetKey | TargetKey[]` - Tableau de valeurs de clé primaire pour les objets associés.

### remove

Supprime les objets associés d'une ressource. `POST /api/<resource>/<resourceKey>/<association>:remove`.

```shell
curl "http://localhost:13000/api/users/1/roles:remove" \
  -X POST \
  -H "Content-Type: application/json" \
  -d '["admin"]'
```

#### Corps de la requête

- `TargetKey | TargetKey[]` - Tableau de valeurs de clé primaire pour les objets associés.

### toggle

Bascule les objets associés à une ressource, en les ajoutant s'ils n'existent pas ou en les supprimant s'ils existent. `POST /api/<resource>/<resourceKey>/<association>:toggle`.

```shell
curl "http://localhost:13000/api/users/1/roles:toggle" \
  -X POST \
  -H "Content-Type: application/json" \
  -d '["admin", "member"]'
```

#### Corps de la requête

- `TargetKey | TargetKey[]` - Tableau de valeurs de clé primaire pour les objets associés.
