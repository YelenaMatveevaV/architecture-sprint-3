```plantuml

@startuml
title WarmHouse Container Diagram

top to bottom direction

!includeurl https://raw.githubusercontent.com/RicardoNiepel/C4-PlantUML/master/C4_Component.puml

Person(user, "User", "Управление устройствами, просмотр данных, настройка сценариев")
Person(admin, "Administrator", "Администрирование системы, ведение какталога устройств")
System(WarmHouse, "WarmHouse as SaaS", "Система для управления отоплением и умными устройствами в доме")

Container_Boundary(WarmHouse, "WarmHouse as SaaS") {
  Container(WebApp, "Web WarmHouse", "Java, Spring", "Просмотр сведений, команды")
  Container(MobileApp, "Mobile WarmHouse", "Kotlin, Swift", "Управление приборами, профилем, сценариями, просмотр сведений")

  Container(PostgreSQL, "PostgreSQL", "PostgreSQL", "Справочники, данные, схема")
  Container(Redis, "Redis", "Redis", "Кэширование")
  Container(MongoDB, "MongoDB", "MongoDB", "Сценарии использования приборов")

  Container(ApiGateway, "ApiGateway", "APISIX Gateway", "Маршрутизация, аутентификация")
  Container(Config, "Configuration", "Consul, Consul-kv", "Конфигурации")
  Container(GatewaySevice, "GatewaySevice", "Go/Python", "Общение с IoT устройствами по протоколам MQTT, HTTP")
  Container(GatewayVideo, "GatewayVideo", "C++/FFmpeg ", "Шлюз для обработки видепотока")
  Container(VideoProcessing, "VideoProcessing", "C++/GStreamer ", "Сжатие и кодирование видео, интеграция с облачным хранилищем")

  Container(UserManagement, "UserManagement", "Java/Go", "Регистрация, роли, профили, аутентификация и авторизация")
  Container(DeviceManagement, "DeviceManagement", "Java/Python", "Подключение устройств, хранение метаданных, REST API управления устройствами")
  Container(ScenarioEngine, "ScenarioEngine", "Node.js/Python", "Создание и выполнение правил, интеграция с сервисом погоды")
  Container(Telemetry, "Telemetry", "Python/Java", "Сбор данных с устройств, аналитика, хранение истории")
  
  Container(MessageBroker, "MessageBroker", "Kafka", "Асинхронная коммуникация между сервисами")

}

System_Ext(sensor, "Устройства партнеров", "Датчики отопления, света, ворот, наблюдения")
System_Ext(oauth, "Сервер OAuth", "Сервер авторизации (OAuth)")
System_Ext(fcm, "Firebase Cloud Messaging (FCM)", "мобильная служба уведомлений от Google (ATL, APNs, Web push-протокол)")
System_Ext(owm, "OpenWeatherMap", "Сведения о прогнозе погоды (JSON)")
System_Ext(sms, "ProstoSMS", "Смс-уведомления (HTTP API)")
System_Ext(miniio, "MinIO", "Облачное хранилище видео")

Rel(user, MobileApp, "Использует систему")
Rel(user, WebApp, "Использует систему")
Rel(admin,WebApp,"Управляет системой")
Rel(GatewaySevice,sensor,"Телеметрия, команды")
Rel(UserManagement,oauth,"Аутентификация")
Rel(ApiGateway,oauth,"Аутентификация")
Rel(ScenarioEngine,fcm,"Отправляет push-уведомление")
Rel(ScenarioEngine,owm,"Получает прогноз погоды")
Rel(ScenarioEngine,sms,"Отправляет уведомление с помощью смс")
Rel(VideoProcessing,miniio,"Записывает видео наблюдения")

Rel(ApiGateway,Redis,"Кеширование")
Rel(UserManagement,PostgreSQL,"Хранение данных пользователей, ролей, профилей")
Rel(DeviceManagement,PostgreSQL,"Метаданные приборов")
Rel(Telemetry,PostgreSQL,"Хранение данных, истории")
Rel(ScenarioEngine,MongoDB,"Сценарии использования приборов")

Rel(MobileApp,ApiGateway,"REST API")
Rel(WebApp,ApiGateway,"REST API")

Rel(UserManagement,Config,"")
Rel(DeviceManagement,Config,"")
Rel(ScenarioEngine,Config,"")
Rel(UserManagement,Config,"")
Rel(Telemetry,Config,"")

Rel(ApiGateway,MessageBroker,"Rest Api")
Rel(MessageBroker,GatewaySevice,"Команды управления приборами")
Rel(GatewaySevice,ApiGateway,"Телеметрия")
Rel(GatewayVideo,VideoProcessing,"Сжатие и сохранение видео")

Rel(MessageBroker,DeviceManagement,"Операции REST API")
Rel(MessageBroker,ScenarioEngine,"Операции REST API")
Rel(MessageBroker,UserManagement,"Операции REST API")
Rel(MessageBroker,Telemetry,"Операции REST API")
@enduml