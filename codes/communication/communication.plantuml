@startuml communication "1. Visualizar Catálogo de Prêmios"

actor Cliente
participant "App Cliente" as UI
participant "CatalogoController" as Controller
participant "CatalogoService" as Service
participant "PremioRepository" as Repository
participant "PremioDTO" as DTO
participant "Storage" as S3
database "DB" as DB

Cliente -> UI : 1: abrir catálogo
UI -> Controller : 2: listarPremios()
Controller -> Service : 3: listarPremios()
Service -> Repository : 4: findAll()
Repository -> DB : 5: buscarPremios()
Service -> S3 : 6: obter URLs das imagens
Service -> DTO : 7: new PremioDTO(premio, url)
Service -> Controller : 8: lista de PremioDTO
Controller -> UI : 9: 200 OK
UI -> Cliente : 10: exibe catálogo

@enduml

@startuml communication "2. Resgatar Prêmio"

actor Cliente
participant "App Cliente" as UI
participant "ResgateController" as Controller
participant "ResgateService" as Service
participant "ClienteRepository" as ClienteRepo
participant "TransacaoRepository" as TransacaoRepo
participant "NotificacaoService" as NT
database "DB" as DB

Cliente -> UI : 1: selecionar prêmio e confirmar resgate
UI -> Controller : 2: resgatarPremio(clienteId, premioId)
Controller -> Service : 3: resgatarPremio()
Service -> ClienteRepo : 4: findById(clienteId)
ClienteRepo -> DB : 5: consultarSaldo(clienteId)

Service -> TransacaoRepo : 5.1: save(RESGATE)
TransacaoRepo -> DB : 5.1.1: registrarTransacao
Service -> NT : 5.2: enviarNotificacaoGerente(loja)
Service -> Controller : 5.3: sucesso
Controller -> UI : 5.3.1: 200 OK
UI -> Cliente : 5.3.2: resgate confirmado

Service -> Controller : 5.4: (lança SaldoInsuficienteException)
Controller -> UI : 5.4.1: 400 Bad Request
UI -> Cliente : 5.4.2: pontos insuficientes

@enduml

@startuml communication "3. Gerente: Enviar Pontos (Distribuição)"

actor Gerente
participant "App Gerente" as UI
participant "PontosController" as Controller
participant "PontosService" as Service
participant "GerenteRepository" as GerenteRepo
participant "TransacaoRepository" as TransacaoRepo
participant "DistribuirPontosDTO" as DTO
database "DB" as DB

Gerente -> UI : 1: enviar pontos (cliente + qtd)
UI -> Controller : 2: distribuirPontos(DistribuirPontosDTO)
Controller -> Service : 3: distribuirPontos(dto)
Service -> GerenteRepo : 4: findById(gerenteId)
GerenteRepo -> DB : 5: verificarCota


Service -> TransacaoRepo : 5.1: save(GANHO_JOGO)
TransacaoRepo -> DB : 5.1.1: registrarTransacao
Service -> GerenteRepo : 5.2: save(gerente.debitarCota)
GerenteRepo -> DB : 5.2.1: updateCota(novaCota)
Service -> Controller : 5.3: envio confirmado
Controller -> UI : 5.3.1: 200 OK

Service -> Controller : 5.4: (lança CotaInsuficienteException)
Controller -> UI : 5.4.1: 400 Bad Request

@enduml

@startuml communication "4. Gerente: Consultar Saldo da Cota"

actor Gerente
participant "App Gerente" as UI
participant "PontosController" as Controller
participant "PontosService" as Service
participant "GerenteRepository" as Repository
participant "CotaDTO" as DTO
database "DB" as DB

Gerente -> UI : 1: consultar saldo da cota
UI -> Controller : 2: getCota(gerenteId)
Controller -> Service : 3: getCota(gerenteId)
Service -> Repository : 4: findById(gerenteId)
Repository -> DB : 5: buscarCota(gerenteId)
Service -> DTO : 6: new CotaDTO(cota)
Service -> Controller : 7: cotaDTO
Controller -> UI : 8: 200 OK (cotaDTO)
UI -> Gerente : 9: exibe saldo restante

@enduml

@startuml communication "5. Admin: Definir Cota Mensal"

actor Admin
participant "Dashboard Admin" as UI
participant "AdminController" as Controller
participant "AdminService" as Service
participant "GerenteRepository" as Repository
database "DB" as DB

Admin -> UI : 1: definir cota mensal (gerenteId, pontos)
UI -> Controller : 2: atualizarCota(UpdateCotaDTO)
Controller -> Service : 3: atualizarCota(dto)
Service -> Repository : 4: findById(gerenteId)
Repository -> DB : 5: findById(gerenteId)
Service -> Repository : 6: save(gerente.setCota(pontos))
Repository -> DB : 7: updateCota()
Service -> Controller : 8: cota atualizada
Controller -> UI : 9: 200 OK
UI -> Admin : 10: confirmação

@enduml

@startuml communication "6.1 Cadastrar Gerente"

actor Admin
participant "Dashboard Admin" as UI
participant "GerenteService" as Service
participant "GerenteDTO" as DTO
participant "GerenteRepository" as Repository
database "DB" as DB

Admin -> UI : 1: cadastrar gerente
UI -> Service : 2: registerGerente(data)
Service -> DTO : 3: toGerente(data)
Service -> Repository : 4: save(gerente)
Repository -> DB : 5: save(gerente)
Service -> DTO : 6: toGerenteDTO(gerente)
Service -> UI : 7: operação confirmada
UI -> Admin : 8: 201 Ok

@enduml

@startuml communication "6.2 Atualizar Gerente"

actor Admin
participant "Dashboard Admin" as UI
participant "GerenteService" as Service
participant "GerenteDTO" as DTO
participant "GerenteRepository" as Repository
database "DB" as DB

Admin -> UI : 1: atualizar gerente
UI -> Service : 2: updateGerente(data)
Service -> DTO : 3: toGerente(data)
Service -> Repository : 4: save(gerente)
Repository -> DB : 5: save(gerente)
Service -> DTO : 6: toGerenteDTO(gerente)
Service -> UI : 7: operação confirmada
UI -> Admin : 8: 200 Ok

@enduml

@startuml communication "6.3 Deletar Gerente"

actor Admin
participant "Dashboard Admin" as UI
participant "GerenteService" as Service
participant "GerenteRepository" as Repository
database "DB" as DB

Admin -> UI : 1: deletar gerente
UI -> Service : 2: deletarGerente()
Service -> Repository : 3: deletarGerente(gerente)
Repository -> DB : 4: deletar(gerente)
Service -> UI : 5: operação confirmada
UI -> Admin : 6: 204 Ok

@enduml

@startuml communication "7 Autenticação"

actor Usuario
participant "Tela de Login" as UI
participant "AuthController" as Controller
participant "AuthService" as Service
database "Banco de Dados" as DB

Usuario -> UI : 1: inserir email + senha / clicar "Entrar"
UI -> Controller : 2: autenticar(email, senha)
Controller -> Service : 3: autenticar(email, senha)
Service -> DB : 4: consultarUsuarioPorEmail(email)

Service -> Controller : 5: token + dados do usuário
Controller -> UI : 6: acesso liberado
UI -> Usuario : 7: acesso liberado

Service -> Controller : 5.1: erro de autenticação
Controller -> UI : 5.1.1: erro de autenticação
UI -> Usuario : 5.1.2: mensagem de erro

@enduml

@startuml communication "8 Saldo"

actor Cliente
participant "App Cliente" as UI
participant "ClientController" as Controller
participant "ClientService" as Service
participant "ClienteRepository" as ClienteRepo
participant "SaldoDTO" as DTO
database "DB" as DB

Cliente -> UI : 1: visualizar saldo
UI -> Controller : 2: getSaldo()
Controller -> Service : 3: getSaldo(clientId)
Service -> ClienteRepo : 4: findClientById(clientId)
ClienteRepo -> DB : 5: findClientById(clientId)
Service -> DTO : 6: toSaldoDTO(cliente.getSaldo())
Service -> Controller : 7: saldo
Controller -> UI : 8: 200 ok
UI -> Cliente : 9: exibe informações

@enduml

@startuml communication "9 Extrato"

actor Cliente
participant "App Cliente" as UI
participant "ClientController" as Controller
participant "ClientService" as Service
participant "ClienteRepository" as ClienteRepo
participant "ExtratoRepository" as ExtratoRepo
database "DB" as DB

Cliente -> UI : 1: visualizar extrato
UI -> Controller : 2: getExtrato()
Controller -> Service : 3: getExtrato(clientId)
Service -> ClienteRepo : 4: findClientById(clientId)
ClienteRepo -> DB : 5: findClientById(clientId)
Service -> ExtratoRepo : 6: findAll(cliente)
Service -> Controller : 7: extrato
Controller -> UI : 8: 200 ok
UI -> Cliente : 9: exibe informações

@enduml