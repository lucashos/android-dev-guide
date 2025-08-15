
# Android Development

## Algoritmos
- Two Pointers
- Dynamic Programming
## Boas práticas
- Clean Code
- SOLID
    - Separation of Concerns
    - Open-Closed
    - Liskov Substitution
    - Interface Segregation
    - Dependency Inversion
- DRY
    - Dont Repeat Yourself
- KISS
    - Keep it Simple and Stupid
## Architecture
- Clean Architecture
## Kotlin

### Coroutines
#### Flows
##### Conceitos
- Emissão vs Coleta
    - Emit
        - Produzir valores no fluxo
    - Collect
        - Consumir valores do fluxo
- Retenção de estado
    - Alguns fluxos “lembram” o último valor emitido (ex: StateFlow).
    - Permite que um novo coletor receba o estado atual imediatamente.
- Replay
    - Capacidade de reenviar para novos coletores um número X de valores antigos
- Multicast
    - Coletores compartilham a mesma emissão no mesmo instante (sincronizados).
- Unicast
    - Cada coletor tem sua própria instância da emissão (isolado).
- Hot
    - Produz valores independentemente de ter coletores
    - Coletores podem “entrar no meio” da emissão e receber apenas valores futuros (ou alguns valores passados, se configurado)
    - Execução compartilhada entre todos os coletores
    - Exemplo:
        no ponto em que está tocando.
- Cold
    - Só começa a produzir valores quando alguém coleta
    - Para cada coletor, o bloco de emissão é reexecutado
    - Execução não compartilhada entre coletores
    - Exemplo
        
#### Tipos
- Flow
    - Cold
        - Produz valores quando houver coletor
        - Sem valor inicial
        - Sem reprodução de valores antigos
        - Novos coletores recebem os dados de uma nova execução
        - Unicast
        - Sem retenção de estado
        
- StateFlow
    - Hot
    - Multicast
    - Inicia a emissão independente de coletores
    - Com retenção de estado
    - Coletor sempre recebe o último valor
    - Caso de uso
        - UI States 
- SharedFlow
    - Hot
    - Multicast
    - Retenção de estado através de  replay
    - Inicia a emissão independente de coletores
    - Pode ser configurado com replay para o coletor receber os valores anteriores
    - Caso de uso
        - Eventos
        - Broadcast

| Característica            | Flow              | StateFlow           | SharedFlow              |
| ------------------------- | ----------------- | ------------------- | ----------------------- |
| Tipo                      | Cold              | Hot                 | Hot                     |
| Multicast?                | Não               | Sim                 | Sim                     |
| Mantém último valor?      | Não               | Sim                 | Opcional (via `replay`) |
| Começa emissão sozinho?   | Não               | Sim                 | Sim                     |
| Coletor recebe histórico? | Não               | Último valor sempre | Configurável (`replay`) |
| Uso comum                 | Dados sob demanda | Estado da UI        | Eventos / broadcast     |


#### CoroutineScopes
- GlobalScope
- ViewModelScope
- LifecycleScope
- CoroutineScope

#### Execução
- Paralela
    - Async
    - Promise.await()
- Serie
    - suspend fun
    - await suspendFun()

#### Scope Functions
| Function | Returns           | Reference | Use case                                                                               |
| -------- | ----------------- | --------- | -------------------------------------------------------------------------------------- |
| `let`    | Lambda result     | `it`      | When performing an operation and returning a different result. Useful for null checks. |
| `run`    | Lambda result     | `this`    | When configuring an object or computing a result.                                      |
| `with`   | Lambda result     | `this`    | Similar to `run`, but used with existing objects.                                      |
| `also`   | Same object (`T`) | `it`      | When performing side effects without modifying the object.                             |
| `apply`  | Same object (`T`) | `this`    | When modifying an object and returning it.                                             |


## Android

### Architecture
#### MVP

Separação entre lógica e UI

---

- Model
    - Regra de negócios
    - Acesso a dados
    - Repositórios
- View
    - UI
- Presenter
    - Coordena a comunicação entre UI e Model
    - Recebe eventos da View -> Processa a lógica -> Model -> Atualiza a View

---

- Vantagens
    - Fácil manutenção
    - Curva de aprendizado
    - Bom para projetos legados e com restrição técnica
    - Separação entre UI e Lógica
- Desvantagens
    - Alto acoplamento
    - God Presenters
    - Manutenção mais difícil para telas mais complexas
    - Boilerplate code
    - Lidar com lifecycle na mão
    - Fluxo de dados bidirecional
    - Estado espalhado
#### MVVM

Ligar o estado da UI ao ViewModel para que esta reaja automaticamente (reatividade) através de observáveis (LiveData/Flows) expostos

---

- Model
    - Dados
    - Lógica
- View
    - UI
    - Observa o estado exposto pelo ViewModel
- ViewModel
    - Jetpack component
    - Mantém estado da UI
    - Expõe observables
    - Executa a lógica

---

- Vantagens
    - Menos boilerplate que MVP
    - Reatividade
    - Separação mais limpa entre UI e ViewModel
    - Evita MemoryLeaks
    - Fluxo de dados unidirecional
    - Estado pode estar tanto espalhado quanto centralizado no ViewModel
    - Lifecycle awareness
- Desvantagens
    - Estado pode estar tanto espalhado quanto centralizado no ViewModel
    - Boilerplate baixo-médio
    - Curva de aprendizado média
#### MVI

Um único estado imutável e previsível para a tela. Cada ação do usuário gera um novo estado.

---
    
- Model
    - Representa todo o estado da tela (imutável)
- View
    - Renderiza o estado e envia intents (ações do usuário)
- Intent
    - Descreve o que o usuário quer fazer (“Carregar lista”, “Atualizar dados”)
- Reducer
    - Recebe o estado atual + evento e devolve novo estado.

---

- Vantagens
    - Fluxo de dados estritamente unidirecional
    - Altíssimo desacoplamento
    - Estado imutável
    - Estado centralizado
    - Mais fácil de debugar
    - Ideal para arquiteturas reativas
    - Previsibilidade de estados
    - Melhora testabilidade por se tratar de intenções
- Desvantagens
    - Verboso
    - Curva de aprendizado mais alta
    - Pode gerar muito boilerplate
### Jetpack Components
- ViewModel
- LiveData
- Workmanager
- Deeplinks
## System Design
- DDD - Domain Driven Design
- TDD - Test Driven Design
