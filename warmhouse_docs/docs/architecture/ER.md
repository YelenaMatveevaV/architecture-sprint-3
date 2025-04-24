
@startuml

entity User {
  *id (PK)
  --
  email
  password_hash
  role_id (FK)
  created_at
}

entity Role {
  *id (PK)
  --
  name
  permissions
}

entity Device {
  *id (PK)
  --
  type_id (FK)
  status
  vendor
  created_at
}

entity Place {
  *id (PK)
  --
  user_id (FK)
  name
  square
  height
  created_at
}

entity DeviceType {
  *id (PK)
  --
  type
  protocol
  created_at
}

entity Place_Device {
  *place_id (FK)
  *device_id (FK)
  --
  date_from
  date_to
}

entity TelemetryData {
  *id (PK)
  --
  device_id (FK)
  metric_type
  value
  timestamp
}

entity Event {
  *id (PK)
  --
  type
  device_id (FK)
  user_id (FK)
  timestamp
  payload
}

entity ScenarioExecution {
  *id (PK)
  --
  scenario_id
  trigger_event_id (FK)
  status
  start_time
  end_time
}

entity Notification {
  *id (PK)
  --
  user_id (FK)
  type
  content
  status
  created_at
}

entity AuditLog {
  *id (PK)
  --
  user_id (FK)
  action
  target_entity
  timestamp
  details
}

' Связи
User }|--|| Role : "role_id"
User ||--o{ Place : "user_id"
Place_Device }o--|| Place : "user_id"
Place_Device }o--|| Device : "device_id"
Device ||--o{ TelemetryData : "device_id"
Device ||--o{ Event : "device_id"
DeviceType ||--o{ Device : "type_id
User ||--o{ Event : "user_id"
Event ||--o{ ScenarioExecution : "trigger_event_id"
User ||--o{ Notification : "user_id"
User ||--o{ AuditLog : "user_id"


note top of Place_Device
  Многие-ко-многим между
  Place и Device
end note

note right of TelemetryData
  Хранит метрики: температура,
  потребление энергии и т.д.
end note

note left of ScenarioExecution
  scenario_id ссылается
  на MongoDB (ObjectId)
end note

@enduml