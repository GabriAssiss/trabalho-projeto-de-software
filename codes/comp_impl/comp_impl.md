@startuml comp_impl
skinparam componentStyle rectangle



package "Frontend" {
    component "Web App Gerente" as FE_Gerente
    component "Dashboard Admin" as FE_Admin
    component "Web App Cliente" as FE_Cliente
}

package "Backend API" {
    component "Pontos Service" as S_Pontos
    component "Notificação Service" as S_Notificacao
    component "Gerente Service" as S_Gerente
    component "Auth Service" as S_Auth
    component "Admin Service" as S_Admin
    component "Catálogo Service" as S_Catalogo
    component "Cliente Service" as S_Cliente
    component "Resgate Service" as S_Resgate
}

package "Database Layer" {
    component "ORM / Repository Layer" as DB_ORM
    component "Banco de Dados" as DB
}

package "External Services" {
    component "Email/SMS Push Service" as EXT_Push
    component "Storage de Imagens (S3/Cloud)" as EXT_Storage
}

' ===========================================================================================
' FRONTEND → BACKEND
' ===========================================================================================

FE_Gerente --> S_Gerente
FE_Gerente --> S_Pontos
FE_Gerente --> S_Catalogo
FE_Gerente --> S_Cliente
FE_Gerente --> S_Resgate
FE_Gerente --> S_Auth

FE_Admin --> S_Admin
FE_Admin --> S_Gerente
FE_Admin --> S_Auth
FE_Admin --> S_Notificacao
FE_Admin --> S_Catalogo

FE_Cliente --> S_Cliente
FE_Cliente --> S_Pontos
FE_Cliente --> S_Resgate
FE_Cliente --> S_Catalogo
FE_Cliente --> S_Auth



S_Gerente --> DB_ORM
S_Admin --> DB_ORM
S_Cliente --> DB_ORM
S_Pontos --> DB_ORM
S_Resgate --> DB_ORM
S_Catalogo --> DB_ORM
S_Auth --> DB_ORM

DB_ORM --> DB

S_Notificacao --> EXT_Push
S_Admin --> EXT_Storage
S_Catalogo --> EXT_Storage
S_Cliente --> EXT_Push

@enduml
