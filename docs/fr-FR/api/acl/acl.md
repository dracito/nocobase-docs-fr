# ACL

## Vue d'ensemble

`ACL` est le module de gestion des permissions de NocoBase. Il est responsable de la gestion des rôles utilisateurs, de l'enregistrement des permissions et des autorisations. Il évalue aussi la politique de permission et le contrôle des accèss.

### Concepts

- **Resource**: Collections, ou ressources personnalisées enregistrées. Se reférer à [`@nocobase/resourcer`](../resourcer/resource-manager.md).
- **Action**: Une interface d'opération pour une ressource, comme la création, la lecture, la modification, la suppression ou d'autres actions personnalisées. Se reférer à [`@nocobase/actions`](../actions).
- **Strategy**: Configure des permission globales pour les rôles, comme des permissions pour une opération sur une ressource comme la création, la lecture, la modification, la suppression, l'importation, l'exportation, et les permission système comme configurer l'interface utilisateur.
- **Snippet**: Définit une collection d'opérations, permettant un gestion unifié de permission sur opération. Les identifiants de Snippet peuvent être associés en utilisant les règles [minimatch](https://github.com/isaacs/minimatch).

## Méthode de classe

### `define()`

Défini un rôle.

#### Signature

- `define(options: DefineOptions): ACLRole`

#### Définitions du type

```typescript
export interface DefineOptions {
  role: string;
  strategy?: string | AvailableStrategyOptions;
  actions?: ResourceActionsOptions;
  snippets?: string[];
}

export interface AvailableStrategyOptions {
  displayName?: string;
  actions?: false | string | string[];
  allowConfigure?: boolean;
  resource?: '*';
}

export interface ResourceActionsOptions {
  [actionName: string]: RoleActionParams;
}

export interface RoleActionParams {
  fields?: string[];
  filter?: any;
  own?: boolean;
  whitelist?: string[];
  blacklist?: string[];
}
```

#### Détails

##### DefineOptions

| Propriété  | Type                                                                | Description                                                                    |
| ---------- | ------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| `role`     | `string`                                                            | Identifiant unique pour le rôle                                                |
| `strategy` | `string` \| [`AvailableStrategyOptions`](#availablestrategyoptions) | Optionnel, Stratégie d'accès global pour le rôle                               |
| `actions`  | [`{ [actionName: string]: RoleActionParams; }`](#roleactionparams)  | Optionnel, configuration des autorisation pour les actions                     |
| `snippets` | `string[]`                                                          | Optionnel, définit les snippets pour lesquels le rôle à l'autorisation d'accès |

##### AvailableStrategyOptions

| Propriété        | Type                              | Description                                                                |
| ---------------- | --------------------------------- | -------------------------------------------------------------------------- |
| `displayName`    | `string`                          | Optionnel, le nom affiché de la stratégie                                  |
| `action`         | `false` \| `string` \| `string[]` | Optionnel, les interfaces d'opération                                      |
| `allowConfigure` | `boolean`                         | Optionnel, s'il faut autoriser la configuration de l'interface utilisateur |
| `resource`       | `*`                               | S'applique à toutes les ressources                                         |

##### RoleActionParams

| Propriété   | Type       | Description                                                                                                                                 |
| ----------- | ---------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| `fields`    | `string[]` | Optionnel, champs de la table de données sur lesquels opérer                                                                                |
| `filter`    | `any`      | Optionnel, filtrer les paramètres qui doivent être remplis, seuls les enregistrements qui remplissent les conditions peuvent être exploités |
| `own`       | `boolean`  | Optionnel, s'il faut opérer uniquement sur des enregistrements créés par soi-même                                                           |
| `whitelist` | `string[]` | Optionnel, liste blanche des champs accessibles, seuls les champs de la liste blanche sont accessibles                                      |
| `blacklist` | `string[]` | Optionnel, liste noire des champs inaccessibles, les champs de la liste noire ne sont pas accessibles                                       |

### `can()`

Détermine l'autorisation d'exécuter une action et renvoie les paramètres finaux de l'action. Renvoie `null` si aucune autorisation.

#### Signature

- `can(options: CanArgs): CanResult | null`

#### Type Definitions

```typescript
interface CanArgs {
  role: string;
  resource: string;
  action: string;
  ctx?: any;
}

interface CanResult {
  role: string;
  resource: string;
  action: string;
  params?: any;
}
```

#### Details

##### CanArgs

| Property   | Type     | Description                       |
| ---------- | -------- | --------------------------------- |
| `role`     | `string` | Identifiant de rôle               |
| `resource` | `string` | Identifiant de ressource          |
| `action`   | `string` | Identifiant d'action              |
| `ctx`      | `any`    | Optionnel, contexte de la requête |

##### CanResult

| Property   | Type     | Description                    |
| ---------- | -------- | ------------------------------ |
| `role`     | `string` | Identifiant de rôle            |
| `resource` | `string` | Identifiant de ressource       |
| `action`   | `string` | Identifiant d'action           |
| `params`   | `any`    | Optionnel, paramètres d'action |

### `registerSnippet()`

Enregistre un snippet.

#### Signature

- `registerSnippet(snippet: SnippetOptions)`

#### Type Definitions

```typescript
export type SnippetOptions = {
  name: string;
  actions: string[];
};
```

#### Details

| Property  | Type       | Description                                                                                                                              |
| --------- | ---------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| `name`    | `string`   | Identifiant snippet, peuvent être mis en correspondance en utilisant les règles [minimatch](https://github.com/isaacs/minimatch). Par exemple, `auth.auth` matches `auth.*`. |
| `actions` | `string[]` | Opérations de ressources incluses dans le snippet, au format `resource:action`. Par exemple, `users:list`.                              |

### `setAvailableAction()`

Sets allowed actions.

#### Signature

- `setAvailableAction(name: string, options: AvailableActionOptions = {})`

```typescript
export interface AvailableActionOptions {
  displayName?: string;
  aliases?: string[] | string;
  resource?: string;
  // Perform operations on new data
  onNewRecord?: boolean;
  // Allow configuring fields
  allowConfigureFields?: boolean;
}
```

#### Details

| Property               | Type                   | Description                                                                                                      |
| ---------------------- | ---------------------- | ---------------------------------------------------------------------------------------------------------------- |
| `displayName`          | `string`               | Optionnel, nom d'affichage de l'action                                                                           |
| `aliases`              | `string` \| `string[]` | Optionnel, alias d’action. Par exemple, les alias pour les opérations `get` et `list` sont tous deux des `view`. |
| `resource`             | `string`               | Optionnel, ressource                                                                                             |
| `onNewRecord`          | `boolean`              | Optionnel, si l'opération s'applique à de nouvelles données, comme une opération de création                     |
| `allowConfigureFields` | `boolean`              | Optionnel, s'il faut autoriser la configuration des champs                                                       |

### `setAvailableStrategy()`

Définit les stratégies autorisées pour les actions.

#### Signature

- `setAvailableStrategy(name: string, options: AvailableStrategyOptions)`

Se référer à [AvailableStrategyOptions](#availablestrategyoptions).

### `allow()`

Définit les conditions dans lesquelles les opérations sont autorisées.

```typescript
acl.allow('plugins', '*', 'public');
```

#### Signature

- `allow(resourceName: string, actionNames: string[] | string, condition?: string | ConditionFunc)`

#### Type

```typescript
export type ConditionFunc = (ctx: any) => Promise<boolean> | boolean;
```

#### Details

| Parameter      | Type                        | Description                                                                         | Défaut   |
| -------------- | --------------------------- | ----------------------------------------------------------------------------------- | -------- |
| `resourceName` | `string`                    | Ressource                                                                           | -        |
| `actionNames`  | `string` \| `string[]`      | Action                                                                              | -        |
| `condition`    | `string` \| `ConditionFunc` | Optionnel, identifiant de condition prédéfini ou fonction d'évaluation de condition | `public` |

Identifiants de condition prédéfinis :

- `public`: Interface publique.
- `loggedIn`: Autorisé quand l'utilisateur est connecté.
- `allowConfigure`: Autorisé lorsque le rôle d'utilisateur actuel est autorisé à configurer l'interface utilisateur.

### `addFixedParams()`

Ajoute des paramètres fixes aux opérations, en les fusionnant avec les paramètres de requête actuels.

```typescript
acl.addFixedParams('users', 'list', () => {
  return {
    filter: {
      id: {
        $eq: 1,
      },
    },
  };
});
```

#### Signature

- `addFixedParams(resource: string, action: string, merger: Merger)`

#### Type Definitions

```typescript
export type Merger = () => object;
```

#### Details

| Parameter  | Type     | Description                                               |
| ---------- | -------- | --------------------------------------------------------- |
| `resource` | `string` | Ressource                                                 |
| `action`   | `string` | Action                                                    |
| `merger`   | `Merger` | Fonction renvoyant l'objet de paramètre fixe à ajouter    |

### `use()`

Ajoute le middleware `ACL`.

```typescript
acl.use(async () => {
  return async function (ctx, next) {
    // ...
    await next();
  };
});
```

#### Signature

- `use(fn: any, options?: ToposortOptions)`

#### Details

Se référer à [Middleware](../../development/server/middleware).

### `middleware()`

Middleware de contrôle d’accès NocoBase.
