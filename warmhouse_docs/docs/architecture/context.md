```plantuml

@startuml
title FitLife Context Diagram

top to bottom direction

!includeurl https://raw.githubusercontent.com/RicardoNiepel/C4-PlantUML/master/C4_Component.puml

Person(user, "User", "Управление устройствами, просмотр данных, настройка сценариев")
Person(admin, "Administrator", "Администрирование системы, ведение какталога устройств")
System(WarmHouse, "WarmHouse as SaaS", "Система для управления отоплением и умными устройствами в доме")

System_Ext(sensor, "Устройства партнеров", "Датчики отопления, света, ворот, наблюдения")
System_Ext(oauth, "Сервер OAuth", "Сервер авторизации (OAuth)")
System_Ext(fcm, "Firebase Cloud Messaging (FCM)", "мобильная служба уведомлений от Google (ATL, APNs, Web push-протокол)")
System_Ext(owm, "OpenWeatherMap", "Сведения о прогнозе погоды (JSON)")
System_Ext(sms, "ProstoSMS", "Смс-уведомления (HTTP API)")
System_Ext(miniio, "MinIO", "Облачное хранилище видео")

Rel(user, WarmHouse, "Использует систему")
Rel(admin,WarmHouse,"Управляет системой")
Rel(WarmHouse,sensor,"Телеметрия, команды")
Rel(WarmHouse,oauth,"Аутентификация")
Rel(WarmHouse,fcm,"Отправляет push-уведомление")
Rel(WarmHouse,owm,"Получает прогноз погоды")
Rel(WarmHouse,sms,"Отправляет уведомление с помощью смс")
Rel(WarmHouse,miniio,"Записывает видео наблюдения")
@enduml