# AuthManager

## Vue d'ensemble

`AuthManager` est le module de gestion de l'authentification des utilisateurs dans NocoBase, utilisé pour enregistrer différents types d'authentification des utilisateurs.

### Utilisation de base

```ts
const authManager = new AuthManager({
  // Key to retrieve the current authenticator identifier from the request header
  authKey: 'X-Authenticator',
});

// Set methods for storing and retrieving authenticators in AuthManager
authManager.setStorer({
  get: async (name: string) => {
    return db.getRepository('authenticators').find({ filter: { name } });
  },
});

// Register an authentication type
authManager.registerTypes('basic', {
  auth: BasicAuth,
  title: 'Password',
});

// Use authentication middleware
app.resourceManager.use(authManager.middleware());
```

### Concepts

- **Authentication type (`AuthType`)**: Different types of authentication, such as Password, SMS, OIDC, SAML, etc.
- **Authenticator**: An authenticator is a database-stored entity linked to a configuration record for a specific authentication type (`AuthType`). Multiple authenticators can exist for one authentication type, each offering diffrent authentications.
- **Authenticator name**: Unique identifier for an authenticator, used to determine the current authentication employed by the current request.

## Méthodes de classe

### `constructor()`

Constructeur, crée une instance de `AuthManager`.

#### Signature

- `constructor(options: AuthManagerOptions)`

#### Types

```ts
export interface JwtOptions {
  secret: string;
  expiresIn?: string;
}

export type AuthManagerOptions = {
  authKey: string;
  default?: string;
  jwt?: JwtOptions;
};
```

#### Details

##### AuthManagerOptions

| Attribut  | Type                        | Description                                                                                          | Défaut            |
| --------- | --------------------------- | ---------------------------------------------------------------------------------------------------- | ----------------- |
| `authKey` | `string`                    | Optionnel, clé pour enregistrer l'identifiant d'authentificateur actuel dans l'en-tête de la requête | `X-Authenticator` |
| `default` | `string`                    | Optionnel, identifiant d'authentificateur par défaut                                                 | `basic`           |
| `jwt`     | [`JwtOptions`](#jwtoptions) | Optionnel, configurer si vous utilisez JWT pour l'authentification                                   | -                 |

##### JwtOptions

| Attribut    | Type     | Description                              | Défaut            |
| ----------- | -------- | ---------------------------------------- | ----------------- |
| `secret`    | `string` | Clé secrète du jeton                     | `X-Authenticator` |
| `expiresIn` | `string` | Optionnel, période d'expiration du jeton | `7d`              |

### `setStorer()`

Définissez les méthodes de stockage et de récupération des données d'authentificateur.

#### Signature

- `setStorer(storer: Storer)`

#### Types

```ts
export interface Authenticator = {
  authType: string;
  options: Record<string, any>;
  [key: string]: any;
};

export interface Storer {
  get: (name: string) => Promise<Authenticator>;
}
```

#### Détails

##### Authenticator

| Attribut   | Type                  | Description                               |
| ---------- | --------------------- | ----------------------------------------- |
| `authType` | `string`              | Type d'authentication                     |
| `options`  | `Record<string, any>` | Configurations liées à l'authentificateur |

##### Storer

`Storer` est l'interface pour le stockage de l'authentificateur, contenant une méthode.

- `get(name: string): Promise<Authenticator>` - Obtenez l'authentificateur par identifiant. Dans NocoBase, le type renvoyé réel est [AuthModel](../../handbook/auth/dev/api#authmodel).

### `registerTypes()`

Enregistrez les types d'authentification.

#### Signature

- `registerTypes(authType: string, authConfig: AuthConfig)`

#### Types

```ts
export type AuthExtend<T extends Auth> = new (config: Config) => T;

type AuthConfig = {
  auth: AuthExtend<Auth>; // The authentication class.
  title?: string; // The display name of the authentication type.
};
```

#### Détails

| Attribut  | Type               | Description                                                                  |
| --------- | ------------------ | ---------------------------------------------------------------------------- |
| `auth`    | `AuthExtend<Auth>` | Implémentation du type d'authentification, reportez-vous à [Auth](./auth.md) |
| `title`   | `string`           | Optionnel, titre du type d'authentification affiché sur le frontend          |

### `listTypes()`

Obtenez une liste des types d’authentification enregistrés.

#### Signature

- `listTypes(): { name: string; title: string }[]`

#### Details

| Attribut  | Type     | Description                            |
| --------- | -------- | -------------------------------------- |
| `name`    | `string` | Identifiant du type d'authentification |
| `title`   | `string` | Titre du type d'authentification       |

### `get()`

Obtenez un authentificateur.

#### Signature

- `get(name: string, ctx: Context)`

#### Détails

| Attribut  | Type      | Description              |
| --------- | --------- | ------------------------ |
| `name`    | `string`  | ID de l'authentificateur |
| `ctx`     | `Context` | Contexte de la requête   |

### `middleware()`

Middleware d’authentification. Obtenez l'authentificateur actuel et effectuez l'authentification de l'utilisateur.
