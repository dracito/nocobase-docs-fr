# CacheManager

## Vue d'ensemble

CacheManager est basé sur [node-cache-manager](https://github.com/node-cache-manager/node-cache-manager) et fournit la gestion du module de mise en cache pour NocoBase. Les types de cache intégrés sont :

- **memory**: Fourni le lru-cache par défaut de node-cache-manager.
- **redis**: Pris en charge par node-cache-manager-redis-yet pour la mise en cache Redis.

D'autres types peuvent être étendus et enregistrés via l'API.

### Concepts

- **Store** : définit une méthode de mise en cache, y compris une méthode d'usine pour créer des caches et d'autres configurations associées. Chaque méthode de mise en cache possède un identifiant unique fourni lors de l'inscription. Les deux méthodes de mise en cache intégrées correspondent aux identifiants uniques `memory` et `redis`.

- **Store Factory Method** : fournie par `node-cache-manager` et les packages d'extension associés, utilisée pour créer des caches. Les exemples incluent `memory` fournie par `node-cache-manager` par défaut et `redisStore` fourni par `node-cache-manager-redis-yet`. Dans ce contexte, l'objet à fournir correspond à [`StoreOptions`](#storeoptions), qui est le premier paramètre de la méthode `caching` dans `node-cache-manager`.

- **Cache** : Une classe encapsulée par NocoBase, fournissant des méthodes liées à l'utilisation du cache. Lors de l'utilisation réelle de la mise en cache, les opérations sont effectuées sur les instances de `Cache`. Chaque instance `Cache` possède un identifiant unique, qui sert d'espace de noms pour distinguer les différents modules.

## Méthodes de classe

### `constructor()`

#### Signature

- `constructor(options?: CacheManagerOptions)`

#### Type

```ts
export type CacheManagerOptions = Partial<{
  defaultStore: string;
  stores: {
    [storeType: string]: StoreOptions;
  };
}>;

type StoreOptions = {
  store?: 'memory' | FactoryStore<Store, any>;
  close?: (store: Store) => Promise<void>;
  // global config
  [key: string]: any;
};
```

#### Détails

##### CacheManagerOptions

| Propriété      | Type                           | Description                                                                                                                                                          |
| -------------- | ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `defaultStore` | `string`                       | Identifiant unique du type cache par défaut.                                                                                                                                                              |
| `stores`       | `Record<string, StoreOptions>` | Enregistre les types de cache. La clé est l'identifiant unique du type de cache et la valeur est un objet contenant la méthode d'enregistrement et les configurations globales.                                                                                                                                             |

##### StoreOptions

| Propriété       | Type                                   | Description                                                                                                                                                                                              |
| --------------- | -------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `store`         | `memory` \| `FactoryStore<Store, any>` | La méthode store factory, correspondant au premier paramètre de la méthode `caching`.                                                                                                                                                                                               |
| `close`         | `(store: Store) => Promise<void>`      | Optionnel, pour les middlewares qui nécessitent l’établissement d’une connexion, tels que Redis, fournissez une méthode de rappel pour fermer la connexion. Le paramètre est l’objet renvoyé par la méthode store factory. |
| `[key: string]` | `any`                                  | Autres configurations globales pour le magasin, correspondant au deuxième paramètre de la méthode `caching`.                                                                                                |

#### `options` par défaut

```ts
import { redisStore, RedisStore } from 'cache-manager-redis-yet';

const defaultOptions: CacheManagerOptions = {
  defaultStore: 'memory',
  stores: {
    memory: {
      store: 'memory',
      // global config
      max: 2000,
    },
    redis: {
      store: redisStore,
      close: async (redis: RedisStore) => {
        await redis.client.quit();
      },
    },
  },
};
```

Le paramètre `options` sera fusionné avec les options par défaut. Les options par défaut existantes peuvent être omises. Par exemple:

```ts
const cacheManager = new CacheManager({
  stores: {
    defaultStore: 'redis',
    redis: {
      // redisStore is already provided in the default options, so only need to provide redisStore configuration.
      url: 'redis://localhost:6379',
    },
  },
});
```

### `registerStore()`

Enregistrez une nouvelle méthode de mise en cache. Exemple:

```ts
import { redisStore } from 'cache-manager-redis-yet';

cacheManager.registerStore({
  // Unique identifier for the store
  name: 'redis',
  // Factory method for creating the store
  store: redisStore,
  // Callback method for closing the store connection
  close: async (redis: RedisStore) => {
    await redis.client.quit();
  },
  // Global configurations
  url: 'xxx',
});
```

#### Signature

- `registerStore(options: { name: string } & StoreOptions)`

### `createCache()`

Créez un cache. Exemple:

```ts
await cacheManager.createCache({
  name: 'default', // Unique identifier for the cache
  store: 'memory', // Unique identifier for the store
  prefix: 'mycache', // Automatically adds the 'mycache:' prefix to cache keys, optional
  // Other store configurations, custom configurations that will be merged with global store configurations
  max: 2000,
});
```

#### Signature

- `createCache(options: { name: string; prefix?: string; store?: string; [key: string]: any }): Promise<Cache>`

#### Détails

##### options

| Propriété       | Type     | Description                                        |
| --------------- | -------- | -------------------------------------------------- |
| `name`          | `string` | Identifiant unique du cache                        |
| `store`         | `string` | Identifiant unique du store                        |
| `prefix`        | `string` | Optionnel, Préfixe automatiquement ajouté aux clés de cache |
| `[key: string]` | `any`    | Autres configurations de magasin personnalisé     |

When `store` is omitted, the `defaultStore` will be used. In this case, the caching
Lorsque `store` est omis, le `defaultStore` sera utilisé. Dans ce cas, la mise en cache
