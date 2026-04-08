---
date: 26-11-2025
tags:
  - IT
  - Security
  - DevOps
status: ready
sticker: lucide//file
---

| Под темы | Заметка                                                                                                                                                                                             |
| -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|          | **realm** - изолированная область, управляющая набором пользователей, их учетными данными, ролями и группами. Область может управлять только теми сущностями, которые входят в нее.                 |
|          | **master realm** - главный realm, управляет другими realm, создает глобальных админов. Также используется keycloak для своих внутренних нужд. (сюда входят: admin-cli, account, broker)             |
|          | **clients** - сущности, которые могут отправлять запрос в keycloak на аутентификацию пользователя. (клиентами являются приложения, сервисы)                                                         |
| scope    | **client scopes** - область доступа, определяющий, какие данные и разрешения получит клиент.                                                                                                        |
|          | **client scopes - acr** - добавляет в токен acr уровень аутентификации (MFA, пароль)                                                                                                                |
|          | **client scopes - address** - добавляет адрес пользователя в токен.                                                                                                                                 |
|          | **client scopes - basic** - включает базовые claims - sub, name, preferred_username, given_name, family_name                                                                                        |
|          | **client scopes - email** - добавляет email и email_verified claims в ID и Access токен.                                                                                                            |
|          | **client scopes - microprofile-jwt** - обеспечивает совместимость с Eclipse MicroProfile JWT.                                                                                                       |
|          | **client scopes - okkline_access** - разрешает выдачу offline refresh token (долгоживущий)                                                                                                          |
|          | **client scopes - phone** - добавляет phone_number и phone_number_verified.                                                                                                                         |
|          | **client scopes - profile** - включает claims: name, given_name, family_name, middle_name, nickname, preferred_username, profile, picture, website, gender, birthdate, zoneinfo, locale, updated_at |
|          | **client scopes - role_list** - включает список ролей пользователя в SAML assertions.                                                                                                               |
|          | **client scopes - roles** - добавляет роли пользователя в access token.                                                                                                                             |
|          | **realm roles** - роль глобальна для всего реалма.                                                                                                                                                  |
|          | **composite role** - роль, содержащая в себе несколько ролей.                                                                                                                                       |
| roles    | **roles - admin** - включает административные права - может управлять пользователями, клиентами, ролями и конфигурацией realm.                                                                      |
|          | **roles - create-realm** - позволяет пользователю создавать новые realm.                                                                                                                            |
|          | **roles - default-roles-master** - набор ролей, назначаемых по умолчанию всем новым пользователям master realm.                                                                                     |
|          | **roles - offline_access** - дает возможность получать offline refresh tokens.                                                                                                                      |
|          | **roles - uma_authorization** - пользователь может делегировать доступ к своим ресурсам другим пользователям.                                                                                       |
| auth     | **browser** - основной поток для входа пользователей через браузер.                                                                                                                                 |
|          | **clients** - поток аутентификации для клиентов                                                                                                                                                     |
|          | **direct grant** - поток для ресурсного владельца, когда пользователь напрямую вводит логин и пароль.                                                                                               |
|          | **docker auth** - поток аутентификации docker-клиентов при доступе к приватному Docker.                                                                                                             |
|          | **first broker login** - поток, срабатывает при первом входе пользователя через внешний Identity Provider.                                                                                          |
|          | **registration** - поток регистрации новых пользователей.                                                                                                                                           |
|          | **reset credentials** - поток для восстановления доступа, если пользователь забыл пароль.                                                                                                           |
| токен    | **OAuth** и **OpenID** - открытые протоколы для безопасной аутентификации и авторизации пользователя в приложениях и API.                                                                           |
|          | **Opaque token** - непрозрачный токен, клиент не может понять, что в token.                                                                                                                         |
|          | **JWT** - самостоятельный токен, в себе несет всю информацию.                                                                                                                                       |
| GT       | **Grant Types** - способ получения Access Token от сервера авторизации в OAuth 2.0/OpenID Connected.                                                                                                |
|          | **Authorization Code** - стандартный безопасный flow для веб-приложений, где сервер клиента может хранить секреты.                                                                                  |
|          | **Authorization Code + PKCE** - модификация для публичных клиентов, чтобы предотвратить атаку перехвата кода.                                                                                       |
|          | **Client Credentials Flow** - приложение запрашивает токен от собственного имени, без пользователя.                                                                                                 |
|          | **Device Code Flow** - пользователь вводит код на другом устройстве.                                                                                                                                |
|          | **Refresh Token Flow** - можно получить новый token без повторной аутентификации.                                                                                                                   |
|          | **Implicit Flow** - token выдается сразу без обмена кода.                                                                                                                                           |
|          | **ROPC Flow** - клиент запрашивает логин/пароль и обменивает их на токен.                                                                                                                           |
| SAML     | **SAML** - открытый стандарт для организации единого входа между разными доменами.                                                                                                                  |
| Пример   | создание realm (Manage realms -> Create realm)                                                                                                                                                      |
|          | создание пользователя (Users -> Add User)                                                                                                                                                           |
|          | создание роли (Realm roles -> Create role)                                                                                                                                                          |
|          | создание scope (Client scopes -> Create client scope -> настройки, в том числе тип)                                                                                                                 |
|          | создание клиента (Clients -> Create client)                                                                                                                                                         |
|          | копирование кредов (Clients -> Credentials -> Client Secret -> Copy)                                                                                                                                |
### Вывод
---
