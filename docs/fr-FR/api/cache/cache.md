# Cache

## Méthodes de base

Vous pouvez vous référer à la documentation de [node-cache-manager](https://github.com/node-cache-manager/node-cache-manager).

- `get()`
- `set()`
- `del()`
- `reset()`
- `wrap()`
- `mset()`
- `mget()`
- `mdel()`
- `keys()`
- `ttl()`

## Autres méthodes

### `wrapWithCondition()`

Similaire à `wrap()`, mais peut décider d'utiliser ou non la mise en cache en fonction des conditions.

```ts
async wrapWithCondition<T>(
  key: string,
  fn: () => T | Promise<T>,
  options?: {
    // External parameter to control whether to use cache results
    useCache?: boolean;
    // Determine whether to cache based on data results
    isCacheable?: (val: unknown) => boolean | Promise<boolean>;
    ttl?: Milliseconds;
  },
): Promise<T> {
```

### `setValueInObject()`

Lorsque le contenu mis en cache est un objet, modifie la valeur d'une clé spécifique.

```ts
async setValueInObject(key: string, objectKey: string, value: unknown)
```

### `getValueInObject()`

Lorsque le contenu mis en cache est un objet, récupère la valeur d'une clé spécifique.

```ts
async getValueInObject(key: string, objectKey: string)
```

### `delValueInObject()`

Lorsque le contenu mis en cache est un objet, supprime une clé spécifique.

```ts
async delValueInObject(key: string, objectKey: string)
```
