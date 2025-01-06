# BaseAuth

## Vue d'ensemble

`BaseAuth` hérite de la classe abstraite [Auth](./auth.md) et sert d'implémentation de base pour les types d'authentification utilisateur, en utilisant JWT comme méthode d'authentification. Dans la plupart des cas, l'extension des types d'authentification utilisateur peut être obtenue en héritant de « BaseAuth » au lieu d'hériter directement de la classe abstraite « Auth ».

```ts
class BasicAuth extends BaseAuth {
  constructor(config: AuthConfig) {
    // Set user collection
    const userCollection = config.ctx.db.getCollection('users');
    super({ ...config, userCollection });
  }

  // User authentication logic, called by `auth.signIn`
  // Returns user data
  async validate() {
    const ctx = this.ctx;
    const { values } = ctx.action.params;
    // ...
    return user;
  }
}
```

## Méthodes de classe

### `constructor()`

Constructeur, crée une instance de `BaseAuth`.

#### Signature

- `constructor(config: AuthConfig & { userCollection: Collection })`

#### Détails

| Paramètre        | Type         | Description                                                                                                                 |
| ---------------- | ------------ | --------------------------------------------------------------------------------------------------------------------------- |
| `config`         | `AuthConfig` | Voir [Auth - AuthConfig](./auth.md#authconfig)                                                                              |
| `userCollection` | `Collection` | Collection des utilisateurs, par exemple, `db.getCollection('users')`, voir [DataBase - Collection](../database/collection) |

### `user()`

Accesseur pour définir et obtenir des informations sur l'utilisateur, utilisant par défaut l'objet `ctx.state.currentUser`.

#### Signature

- `set user()`
- `get user()`

### `check()`

S'authentifie via le jeton de requête et renvoie les informations utilisateur.

### `signIn()`

Connexion utilisateur, génère un jeton.

### `signUp()`

Inscription des utilisateurs.

### `signOut()`

Déconnexion de l'utilisateur, expiration du jeton.

### `validate()` \*

Logique d'authentification de base, appelée par l'interface « signIn », pour déterminer si l'utilisateur peut se connecter avec succès.
