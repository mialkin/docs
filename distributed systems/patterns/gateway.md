# Gateway

## Что такое api gateway

API Gateway — один из основных паттернов, вокруг которого строится микросервисная архитектура. Его реализация преследует
ряд целей:

* Routing Мы выставляем во внешний мир один адрес, но так как у нас много микросервисов, нам необходимо перенаправлять
  запросы на инстансы этих микросервисов.
* Построение бэкенда для фронтенда одностраничных приложений. Вместо того чтобы делать МРы в балансер можно выставить
  один gateway
* API Gateway это способ разделения клиентского программного интерфейса от вашей внутренней реализации. Когда клиент
  делает запрос, шлюз API автоматически(не совсем) разбивает его на несколько запросов, направляет их в нужные места,
  выдает ответ и все отслеживает.
* Безопасность — одна из самых важных задач, которые решает API Gateway, потому что эта система стоит на входе в нашу
  инфраструктуру и именно на границе мы решаем вопрос безопасности, после чего уже имеем дело с доверенной зоной (DMZ).
* Кеширование.
* Ratelimit
* Метрики, логирование, трейсинг
* QoS (circuit breakers and retries)

## ocelot умер, да здравствует YARP

| [ocelot](https://ocelot.readthedocs.io/en/latest/)                                                          | [YARP](https://microsoft.github.io/reverse-proxy/): Yet Another Reverse Proxy                                                                                 |
|-------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [не понятно что с поддержкой](https://github.com/ThreeMammals/Ocelot/issues/1525) последний комит 20 января | Новый [проект](https://microsoft.github.io/reverse-proxy/) от Microsoft                                                                                       |
| есть история что ocelot выгружает запросы в память (не нашел пруфов)                                        | YARP is a library to help create reverse proxy servers that are high-performance, production-ready, and highly customizable. (с) Microsoft                    |
| умеет [аггрегировать](https://microsoft.github.io/reverse-proxy/) простейшие запрос запросы                 | за аггрегироваинем замечен не был                                                                                                                             |
| умеет подставлять параметры из токена в роутинг /profile -> /users/{userId}/profile                         | Увы не умеет, работает с трансформациями попроще, трансформацию нужно написать самому                                                                         |
| на один эндпоинт можно повесить одну схему авторизации, и перечислить required claims                       | Работает полноценно с [авторизационными политиками](https://docs.microsoft.com/ru-ru/aspnet/core/security/authorization/policies?view=aspnetcore-6.0) дотнета |
| Http запросы                                                                                                | Поддерживает [GRPC](https://microsoft.github.io/reverse-proxy/articles/grpc.html)                                                                                                                                         |

## проблематика: несколько провайдеров авторизаций и политики

(!!!описать задачу поподробнее!!! добавить схемку)
В нашем проекте у нас есть несколько участников процесса для каждого из которых мы предоставляем свою авторизацию

* ADFS для внутренних агентов
* SSO для клиентов и внешних агентов
* ApiKey - интеграция с банками-партнерами

В случае с оцелотом нужно было бы делать три разных апи, либо три разных домена под каждое апи, либо каждый
  экдпоинт доступный и по ADFS и по SSO выставлять под разными именами/с разными префиксами

## реализация

Авторизационный провайдер SSO и его sequence ADFS (Bearer токен из коробки)
ApiKey ссылка на нугет Примеры авторизационных политик + пример конфига
// plantuml 
## Проблемки

swagger не поддерживается

## ссылки и рефернесы

[ПОСМОТРЕТЬ ДОПОЛНИТЬ](https://docs.microsoft.com/en-us/dotnet/architecture/microservices/architect-microservice-container-applications/direct-client-to-microservice-communication-versus-the-api-gateway-pattern)
[ПОСМОТРЕТЬ ПРО ОЦЕЛОТ](https://docs.microsoft.com/en-us/dotnet/architecture/microservices/multi-container-microservice-net-applications/implement-api-gateways-with-ocelot)
[1](https://docs.microsoft.com/en-us/azure/architecture/microservices/design/gateway)
[2](https://docs.microsoft.com/en-us/azure/architecture/patterns/backends-for-frontends)
[3](https://docs.microsoft.com/en-us/azure/architecture/patterns/gateway-offloading)
[4](https://docs.microsoft.com/en-us/azure/architecture/patterns/gateway-routing)
[5](https://docs.microsoft.com/en-us/azure/architecture/patterns/gateway-aggregation)
