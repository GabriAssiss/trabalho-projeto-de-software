@startuml Use Cases

actor Cliente
actor Administrador
actor Gerente

package "Candy" {

  
        usecase "Consultar Saldo"                       as UC_Cliente_Saldo
        usecase "Consultar Extrato"                     as UC_Cliente_Extrato
        usecase "Visualizar Doces Cadastrados"          as UC_Cliente_VerDoces
        usecase "Trocar Pontos por Doces"               as UC_Cliente_Troca
    

    
        usecase "Cadastrar Doces"                       as UC_Admin_CadDoces
        usecase "Editar Doces"                          as UC_Admin_EditDoces
        usecase "Cadastrar Gerente"                     as UC_Admin_CadGerente
        usecase "Deletar Gerente"                       as UC_Admin_DelGerente
        usecase "Atualizar Gerente"                     as UC_Admin_UpdGerente
        usecase "Enviar Pontos (Cota) para Gerente"     as UC_Admin_EnviarCota
    


        usecase "Consultar Saldo"                       as UC_Gerente_Saldo
        usecase "Consultar Extrato"                     as UC_Gerente_Extrato
        usecase "Receber Pontos (Cota)"                 as UC_Gerente_ReceberCota
        usecase "Distribuir Pontos para Clientes"       as UC_Gerente_Distribuir
    
}




Cliente --> UC_Cliente_Extrato
Cliente --> UC_Cliente_Saldo
Cliente --> UC_Cliente_VerDoces
Cliente --> UC_Cliente_Troca

Administrador --> UC_Admin_CadDoces
Administrador --> UC_Admin_EditDoces
Administrador --> UC_Admin_CadGerente
Administrador --> UC_Admin_DelGerente
Administrador --> UC_Admin_UpdGerente
Administrador --> UC_Admin_EnviarCota

Gerente --> UC_Gerente_Saldo
Gerente --> UC_Gerente_Extrato
Gerente --> UC_Gerente_ReceberCota
Gerente --> UC_Gerente_Distribuir

UC_Cliente_Extrato --> UC_Cliente_Saldo : <<include>>
UC_Cliente_Troca --> UC_Cliente_VerDoces : <<extend>>

UC_Admin_EditDoces --> UC_Admin_CadDoces : <<include>>
UC_Admin_DelGerente --> UC_Admin_CadGerente : <<include>>
UC_Admin_EnviarCota --> UC_Admin_CadGerente : <<include>>

UC_Gerente_Distribuir --> UC_Gerente_Saldo : <<include>>

@enduml
