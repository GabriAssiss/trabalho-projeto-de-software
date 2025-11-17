@startuml der

entity Usuario {
  *id : int
  --
  nome : varchar
  email : varchar
  senha_hash : varchar
  data_criacao : datetime
  ativo : boolean
}

entity Cliente {
  *id : int [FK]
  --
  saldo_pontos : int
}

entity Administrador {
  *id : int [FK]
  --
}

entity Loja {
  *id : int
  --
  nome : varchar
  endereco : varchar
}

entity Gerente {
  *id : int [FK]
  --
  id_loja : int [FK]
  saldo_cota : int
}

entity CatalogoItem {
  *id : int
  --
  nome : varchar
  descricao : varchar
  foto_url : varchar
  custo_pontos : int
  ativo : boolean
}

entity Resgate {
  *id : int
  --
  id_cliente : int [FK]
  id_item : int [FK]
  id_loja : int [FK]
  data_resgate : datetime
  pontos_usados : int
  status : enum
}

entity ExtratoPontos {
  *id : int
  --
  id_cliente : int [FK]
  quantidade : int
  origem : varchar
  destino : varchar
  data : datetime
  tipo : enum
}

entity EnvioPontos {
  *id : int
  --
  id_cliente : int [FK]
  id_gerente : int [FK]
  quantidade : int
  comprovante_url : varchar
  data_envio : datetime
}

entity CotaMensal {
  *id : int
  --
  id_gerente : int [FK]
  data_recebimento : datetime
  cota_total : int
}

entity NotificacaoResgate {
  *id : int
  --
  id_resgate : int [FK]
  id_gerente : int [FK]
  data_envio : datetime
  lida : boolean
}



Usuario ||--|| Cliente : 
Usuario ||--|| Administrador 
Usuario ||--|| Gerente 


Loja ||--o{ Gerente 
Loja ||--o{ Resgate 
Loja ||--o{ CatalogoItem 

Cliente ||--o{ ExtratoPontos 
Cliente ||--o{ EnvioPontos 
Cliente ||--o{ Resgate 

Gerente ||--o{ EnvioPontos 
Gerente ||--o{ CotaMensal 

CatalogoItem ||--o{ Resgate 
Resgate ||--o{ NotificacaoResgate 

@enduml