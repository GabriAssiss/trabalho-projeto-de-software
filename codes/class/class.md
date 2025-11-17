@startuml Class

enum TransactionType {
  GANHO_JOGO
  RESGATE
  GANHO_COMPRA
}
enum Status {
  APROVADO
  PENDENTE
  NEGADO
}


class Usuario {
  id: int
  nome: String
  email: String
  senhaHash: String  
  autenticar(email, senha): boolean
  fazerLogout(): void
}

class Cliente {
  + saldoPontos(): int
  + consultarSaldo(): int
  + consultarExtrato(): List<Transacao>
  + resgatarPremio(Premio, Loja): Transacao
}

class Administrador {
  + cadastrarGerente(data): GerenteDeLoja
  + deletarGerente(GerenteDeLoja)
  + definirCotaGerente(GerenteDeLoja, cota): void
  + definirDetalhesPremio(Premio, valido)
  + deletarPremio(Premio)
  + atualizarInformacoesPremio(Premio): void
}

class GerenteDeLoja {
  cotaPontos: int
  pontosDisponiveis: int
  
  + distribuirPontos(Cliente, qtd, comprovante, String): Transacao
  + consultarSaldoDistribuicao(): int
  + consultarHistorico(Cliente): List<Transacao>
}

class Loja {
  idLoja: Long
  nome: String
  endereco: String
}

class Premio {
  idPremio: Long
  descricao: String
  fotoURL: String
  customPoints: int
  ativo: boolean
}

class Transacao {
  idTransacao: Long
  data: Date
  tipo: TransactionType
  valorPontos: int
  comprovanteImg: String
  status: Status
}


Usuario <|-- Cliente
Usuario <|-- Administrador
Usuario <|-- GerenteDeLoja


Cliente "1" -- "0..*" Transacao
Loja "1" -- "0..*" GerenteDeLoja
Loja "1" -- "0..*" Transacao

Administrador "0..1" -- "0..*" Premio

Transacao "0..1" -- "0..N" Premio
Transacao *-- TransactionType
Transacao *-- Status

Cliente -- "0..1" Loja

@enduml