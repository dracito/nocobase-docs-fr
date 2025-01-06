# Auth

## Vue d'ensemble

`Auth` est une classe abstraite pour les types d'authentification utilisateur, définissant les interfaces requises pour compléter l'authentification utilisateur. Pour étendre de nouveaux types d'authentification utilisateur, vous devez hériter de la classe `Auth` et implémenter ses méthodes. L'implémentation de base peut être appelée [BaseAuth](./base-auth.md).

```ts
interface IAuth {
  user: Model;
  // Check the authentication status and return the current user.
  check(): Promise<Model>;
  signIn(): Promise<any>;
  signUp(): Promise<any>;
  signOut(): Promise<any>;
}

export abstract class Auth implements IAuth {
  abstract user: Model;
  abstract check(): Promise<Model>;
  // ...
}

class CustomAuth extends Auth {
  // check: Authentication
  async check() {
    // ...
  }
}
```

## Propriétés de l'instance

### `user`

Informations de l'utilisateur authentifié.

#### Signature

- `abstract user: Model`

## Class Methods

### `constructor()`

Constructoeur, crée une instance de `Auth`.

#### Signature

- `constructor(config: AuthConfig)`

#### Types

```ts
export type AuthConfig = {
  authenticator: Authenticator;
  options: {
    [key: string]: any;
  };
  ctx: Context;
};
```

#### Details

##### AuthConfig

| Attribut        | Type                                            | Description                                                                                                                               |
| --------------- | ----------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `authenticator` | [`Authenticator`](./auth-manager#authenticator) | Modèle de données Authentificateur, le type réel dans les applications NocoBase est [AuthModel](../../handbook/auth/dev/api.md#authmodel) |
| `options`       | `Record<string, any>`                           | Configurations liées à l'Authentificateur                                                                                                 |
| `ctx`           | `Context`                                       | Contexte de la requête                                                                                                                    |

### `check()`

Authentification de l'utilisateur, renvoie les informations de l'utilisateur. Il s'agit d'une méthode abstraite que tous les types d'authentification doivent implémenter.

#### Signature

- `abstract check(): Promise<Model>`

### `signIn()`

Login utilisateur.

#### Signature

- `signIn(): Promise<any>`

### `signUp()`

Enregistrement de l'utilisateur.

#### Signature

- `signUp(): Promise<any>`

### `signOut()`

Logout utilisateur

#### Signature

- `signOut(): Promise<any>`
