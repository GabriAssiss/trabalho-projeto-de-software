@startuml SaldoExtrato

[*] --> EntrandoNaTelaPrincipal :

state EntrandoNaTelaPrincipal

state TelaPrincipal

EntrandoNaTelaPrincipal --> TelaPrincipal : carregamento

state TelaSaldoExtrato

TelaPrincipal --> TelaSaldoExtrato : selecionar "Saldo e Extrato"

state ExibindoSaldoExtrato

TelaSaldoExtrato --> ExibindoSaldoExtrato : carregar dados / getBalance()

ExibindoSaldoExtrato --> TelaPrincipal : voltar
@enduml


@startuml Login
[*] --> AbrindoSite


state AbrindoSite
state Desconectado
state Conectado


state J1 <<choice>>


AbrindoSite --> J1

J1 --> Desconectado
J1 --> Conectado

Desconectado --> Autenticando : autenticar(email, senha)
Autenticando --> Desconectado : falha de autenticação
Autenticando --> Conectado : credenciais válidas

Conectado --> Desconectado : logout / expiração de sessão


Conectado --> [*]
Desconectado --> [*]

@enduml

@startuml Cadastro


[*] --> TelaCadastro

state TelaCadastro
state PreenchendoDados
state ValidandoDados
state VerificandoDisponibilidade
state CadastroProcessando
state CadastroSucesso
state ErroDadosInvalidos
state ErroUsuarioExistente
state CadastroErro

TelaCadastro --> PreenchendoDados

PreenchendoDados --> ValidandoDados : clicar "Cadastrar" / register(email, nome, senha)*

ValidandoDados --> VerificandoDisponibilidade : dados válidos
ValidandoDados --> ErroDadosInvalidos : campos inválidos

ErroDadosInvalidos --> PreenchendoDados : corrigir

VerificandoDisponibilidade --> CadastroProcessando : disponível
VerificandoDisponibilidade --> ErroUsuarioExistente : email já cadastrado

ErroUsuarioExistente --> PreenchendoDados : usar outro email

CadastroProcessando --> CadastroSucesso : criado com sucesso
CadastroProcessando --> CadastroErro : erro no servidor

CadastroErro --> PreenchendoDados : tentar novamente

CadastroSucesso --> [*]

@enduml

@startuml Catalogo

[*] --> TelaPrincipalCliente

state TelaPrincipalCliente
state TelaCatalogo
state VisualizandoItem
state ConfirmacaoResgate
state ResgateProcessando
state ResgateErro
state ResgateSucesso

TelaPrincipalCliente --> TelaCatalogo : selecionar "Catálogo"
TelaCatalogo --> TelaPrincipalCliente : voltar

TelaCatalogo --> VisualizandoItem : selecionar item
VisualizandoItem --> TelaCatalogo : voltar

VisualizandoItem --> ConfirmacaoResgate : clicar "Resgatar"

ConfirmacaoResgate --> ResgateProcessando : confirmar

ResgateProcessando --> ResgateErro : saldo insuficiente
ResgateProcessando --> ResgateSucesso : saldo suficiente

ResgateSucesso --> TelaCatalogo : OK
ResgateErro --> VisualizandoItem

@enduml

@startuml EnviarPontos

' Ponto Inicial
[*] --> TelaPrincipalGerente

state TelaPrincipalGerente
state TelaEnvioPontos
state ValidandoComprovante
state InserirQuantidade
state EnvioProcessando
state EnvioErro
state EnvioSucesso
state ErroComprovante

TelaPrincipalGerente --> TelaEnvioPontos : selecionar "Enviar Pontos"

TelaEnvioPontos --> ValidandoComprovante : anexar comprovante

ValidandoComprovante --> InserirQuantidade : aprovado
ValidandoComprovante --> ErroComprovante : inválido

ErroComprovante --> TelaEnvioPontos : corrigir

InserirQuantidade --> EnvioProcessando : enviar quant > 0 e quant < cota

EnvioProcessando --> EnvioErro : excede cota
EnvioProcessando --> EnvioSucesso : dentro da cota

EnvioErro --> InserirQuantidade : ajustar quantidade

EnvioSucesso --> TelaEnvioPontos : OK

@enduml

@startuml GerenciarGerentes
skinparam state {
  BackgroundColor White
  BorderColor Black
  FontColor Black
}

[*] --> TelaPrincipalAdmin

state TelaPrincipalAdmin

state TelaGerentes

TelaPrincipalAdmin --> TelaGerentes : selecionar "Gerenciar Gerentes"
TelaGerentes --> TelaPrincipalAdmin : voltar

state CadastroGerente
state EdicaoGerente
state DesativarGerente

TelaGerentes --> CadastroGerente : criar novo
CadastroGerente --> TelaGerentes : salvo

TelaGerentes --> EdicaoGerente : editar existente
EdicaoGerente --> TelaGerentes : salvo

TelaGerentes --> DesativarGerente : desativar
DesativarGerente --> TelaGerentes : atualizado
@enduml

@startuml CatalogoAdm
skinparam state {
  BackgroundColor White
  BorderColor Black
  FontColor Black
}


[*] --> TelaPrincipalAdmin

state TelaPrincipalAdmin

state TelaCatalogoAdmin

TelaPrincipalAdmin --> TelaCatalogoAdmin : selecionar "Catálogo"
TelaCatalogoAdmin --> TelaPrincipalAdmin : voltar

state CadastroItem
state EdicaoItem
state ExcluirItem

TelaCatalogoAdmin --> CadastroItem : criar item
CadastroItem --> TelaCatalogoAdmin : salvo

TelaCatalogoAdmin --> EdicaoItem : editar item
EdicaoItem --> TelaCatalogoAdmin : salvo

TelaCatalogoAdmin --> ExcluirItem : excluir item
ExcluirItem --> TelaCatalogoAdmin : removido
@enduml

@startuml Cota
skinparam state {
  BackgroundColor White
  BorderColor Black
  FontColor Black
}

' Ponto Inicial
[*] --> TelaPrincipalAdmin

state TelaPrincipalAdmin

state TelaCotas

TelaPrincipalAdmin --> TelaCotas : selecionar "Cotas"
TelaCotas --> TelaPrincipalAdmin : voltar

state EditandoCota

TelaCotas --> EditandoCota : alterar valores
EditandoCota --> TelaCotas : salvar

@enduml