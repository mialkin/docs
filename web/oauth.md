# OAuth 2.0

**OAuth 2.0** is the protocol for authorization.

OAuth is commonly used as a way for Internet users to grant websites or applications access to their information on other websites but without giving them the passwords.

Generally, OAuth provides clients a "secure delegated access" to server resources on behalf of a resource owner. It specifies a process for resource owners to authorize third-party access to their server resources without sharing their credentials. Designed specifically to work with Hypertext Transfer Protocol (HTTP), OAuth essentially allows access tokens to be issued to third-party clients by an authorization server, with the approval of the resource owner. The third party then uses the access token to access the protected resources hosted by the resource server.

## Яндекс OAuth

1\. Создаем приложение в [↑ Яндекс.OAuth](https://oauth.yandex.ru):

```text
ID: 32f435e3a13e439ee34b893cde64a913b
Пароль: 736da1460be448a0aa4a0b00ac155b03
Callback URL: https://example.abcd/my_redirect_uri
Время жизни токена: Не менее, чем 1 год
Дата создания: 14.10.2020
```

2\. [↑ Получаем токен](https://yandex.ru/dev/oauth/doc/dg/tasks/get-oauth-token.html/), для это в бразуре вбиваем:

```text
https://oauth.yandex.ru/authorize?response_type=token&client_id=32f435e3a13e439ee34b893cde64a913b
```

где значение `client_id` – полученный выше ID приложения.

В ответ на запрос выше Яндекс переправит на адрес:

```text
https://example.abcd/my_redirect_uri#access_token=AgAAAAALzs92AAaMUcdTBr8wZExbk3Yj-vs-_lP&token_type=bearer&expires_in=31536000
```

где значение `AgAAAAALzs92AAaMUcdTBr8wZExbk3Yj-vs-_lP` — это токен доступа.

3\. Используя полученный токен можно попорбовать отправить запросы, например, на [↑ Полигоне](https://yandex.ru/dev/disk/poligon/) Яндекс.Диска:

```bash
curl -X GET --header 'Accept: application/json' --header 'Authorization: OAuth AgAAAAALzs92AAaMUcdTBr8wZExbk3Yj-vs-_lP' 'https://cloud-api.yandex.net/v1/disk'

```
