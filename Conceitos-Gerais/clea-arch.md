# Guia Definitivo: Clean Architecture aplicada ao Quarkus

## 1. O que é Clean Architecture? (Conceito Fundamental)

Imagine sua aplicação como uma **cebola** 🧅. Cada camada tem sua responsabilidade e as camadas internas não conhecem as externas.

**Analogia simples**: Pense numa empresa:
- **CEO** (Entities): Define as regras fundamentais do negócio
- **Gerentes** (Use Cases): Organizam e coordenam as ações
- **Supervisores** (Adapters): Fazem a ponte entre departamentos
- **Funcionários** (Frameworks): Executam tarefas específicas

### Principais Objetivos
- **Independência de frameworks**: Trocar Spring por Quarkus sem quebrar regras de negócio
- **Testabilidade**: Testar lógica sem banco de dados
- **Desacoplamento**: Mudanças numa camada não afetam outras
- **Manutenção**: Código organizado e previsível

### As 4 Camadas em Círculos Concêntricos

```mermaid
graph TD
    subgraph "Frameworks & Drivers"
        DB[(Database)]
        API[External APIs]
        UI[REST Controllers]
    end
    
    subgraph "Interface Adapters"
        GW[Gateways]
        CTRL[Controllers]
        PRES[Presenters]
    end
    
    subgraph "Application Business Rules"
        UC[Use Cases]
    end
    
    subgraph "Enterprise Business Rules"
        ENT[Entities]
    end
    
    UI --> CTRL
    CTRL --> UC
    UC --> ENT
    UC --> GW
    GW --> DB
    GW --> API
```

**Regra Fundamental**: As dependências sempre apontam para dentro. A camada interna nunca conhece a externa.

## 2. Comparando com Outras Arquiteturas

| Arquitetura | Características | Quando Usar |
|------------|----------------|-------------|
| **MVC** | Controller → Service → Repository | Projetos simples, prototipagem |
| **Hexagonal** | Ports & Adapters | Quando precisa de múltiplos adapters |
| **Onion** | Camadas concêntricas | Similar à Clean, menos formal |
| **Clean** | Use Cases explícitos, regras rígidas | Projetos complexos, longo prazo |

**Por que Clean Architecture?**
- Mais **disciplinada** que MVC
- Mais **estruturada** que Hexagonal
- **Use Cases** explícitos (diferente da Onion)

## 3. Estrutura de Pacotes no Quarkus

```
src/main/java/com/example/
├── domain/                    # 🎯 Regras de Negócio Puras
│   ├── entities/             # Objetos fundamentais
│   └── gateways/            # Contratos (interfaces)
├── application/              # 🎪 Orquestração
│   └── usecases/            # Casos de uso específicos
├── infrastructure/          # 🔧 Implementações técnicas
│   ├── repositories/        # Persistência
│   └── external/           # APIs externas
└── interfaces/              # 🌐 Pontos de entrada
    └── rest/               # Controllers REST
```

## 4. Exemplo Prático: Sistema de Pedidos

Vamos construir um sistema de pedidos passo a passo, da camada mais interna para a externa.

### 4.1 Entidade (Coração do Sistema)

```java
// domain/entities/Order.java
public class Order {
    private final String id;
    private final String customerId;
    private final BigDecimal total;
    private final LocalDateTime createdAt;

    public Order(String customerId, BigDecimal total) {
        // 🛡️ Regras de negócio SEMPRE na entidade
        if (total == null || total.compareTo(BigDecimal.ZERO) <= 0) {
            throw new IllegalArgumentException("Total deve ser positivo");
        }
        if (customerId == null || customerId.trim().isEmpty()) {
            throw new IllegalArgumentException("Cliente é obrigatório");
        }
        
        this.id = UUID.randomUUID().toString();
        this.customerId = customerId;
        this.total = total;
        this.createdAt = LocalDateTime.now();
    }
    
    // Construtor para reconstrução (vindo do banco)
    public Order(String id, String customerId, BigDecimal total, LocalDateTime createdAt) {
        this.id = id;
        this.customerId = customerId;
        this.total = total;
        this.createdAt = createdAt;
    }

    // 💰 Lógica de negócio: calcular desconto
    public BigDecimal calculateDiscountedTotal(BigDecimal discountPercent) {
        if (discountPercent.compareTo(BigDecimal.ZERO) < 0 || 
            discountPercent.compareTo(new BigDecimal("100")) > 0) {
            throw new IllegalArgumentException("Desconto deve estar entre 0% e 100%");
        }
        
        BigDecimal discount = total.multiply(discountPercent).divide(new BigDecimal("100"));
        return total.subtract(discount);
    }

    // Getters
    public String getId() { return id; }
    public String getCustomerId() { return customerId; }
    public BigDecimal getTotal() { return total; }
    public LocalDateTime getCreatedAt() { return createdAt; }
}
```

### 4.2 Gateway (Contrato/Interface)

```java
// domain/gateways/OrderRepository.java
public interface OrderRepository {
    Order save(Order order);
    Optional<Order> findById(String id);
    List<Order> findByCustomerId(String customerId);
}

// domain/gateways/NotificationGateway.java
public interface NotificationGateway {
    void sendOrderConfirmation(Order order);
}
```

### 4.3 Caso de Uso (Orquestrador)

```java
// application/usecases/CreateOrderUseCase.java
@ApplicationScoped
public class CreateOrderUseCase {
    private final OrderRepository orderRepository;
    private final NotificationGateway notificationGateway;

    public CreateOrderUseCase(OrderRepository orderRepository, 
                             NotificationGateway notificationGateway) {
        this.orderRepository = orderRepository;
        this.notificationGateway = notificationGateway;
    }

    public Order execute(String customerId, BigDecimal total) {
        // 1. Criar pedido (regras na entidade)
        Order order = new Order(customerId, total);
        
        // 2. Salvar
        Order savedOrder = orderRepository.save(order);
        
        // 3. Notificar (orquestração)
        notificationGateway.sendOrderConfirmation(savedOrder);
        
        return savedOrder;
    }
}
```

### 4.4 Implementação da Infraestrutura

```java
// infrastructure/repositories/OrderEntity.java
@Entity
@Table(name = "orders")
public class OrderEntity extends PanacheEntity {
    public String customerId;
    public BigDecimal total;
    public LocalDateTime createdAt;

    // Conversão para Domain
    public Order toDomain() {
        return new Order(
            this.id.toString(),
            this.customerId,
            this.total,
            this.createdAt
        );
    }

    // Conversão do Domain
    public static OrderEntity fromDomain(Order order) {
        OrderEntity entity = new OrderEntity();
        entity.customerId = order.getCustomerId();
        entity.total = order.getTotal();
        entity.createdAt = order.getCreatedAt();
        return entity;
    }
}

// infrastructure/repositories/OrderRepositoryImpl.java
@ApplicationScoped
public class OrderRepositoryImpl implements OrderRepository {

    @Override
    public Order save(Order order) {
        OrderEntity entity = OrderEntity.fromDomain(order);
        entity.persist();
        
        // Retorna com ID gerado
        return new Order(
            entity.id.toString(),
            order.getCustomerId(),
            order.getTotal(),
            order.getCreatedAt()
        );
    }

    @Override
    public Optional<Order> findById(String id) {
        return OrderEntity.findByIdOptional(Long.valueOf(id))
                .map(entity -> ((OrderEntity) entity).toDomain());
    }

    @Override
    public List<Order> findByCustomerId(String customerId) {
        return OrderEntity.list("customerId", customerId)
                .stream()
                .map(entity -> ((OrderEntity) entity).toDomain())
                .toList();
    }
}
```

### 4.5 Controller (Interface Externa)

```java
// interfaces/rest/OrderResource.java
@Path("/orders")
@Produces(MediaType.APPLICATION_JSON)
@Consumes(MediaType.APPLICATION_JSON)
public class OrderResource {

    private final CreateOrderUseCase createOrderUseCase;

    public OrderResource(CreateOrderUseCase createOrderUseCase) {
        this.createOrderUseCase = createOrderUseCase;
    }

    @POST
    public Response createOrder(@Valid CreateOrderRequest request) {
        try {
            Order order = createOrderUseCase.execute(
                request.customerId, 
                request.total
            );
            
            return Response.status(201)
                    .entity(OrderResponse.fromDomain(order))
                    .build();
                    
        } catch (IllegalArgumentException e) {
            return Response.status(400)
                    .entity(Map.of("error", e.getMessage()))
                    .build();
        }
    }
}

// DTOs
public class CreateOrderRequest {
    @NotNull
    public String customerId;
    
    @NotNull
    @DecimalMin("0.01")
    public BigDecimal total;
}

public class OrderResponse {
    public String id;
    public String customerId;
    public BigDecimal total;
    public String createdAt;

    public static OrderResponse fromDomain(Order order) {
        OrderResponse response = new OrderResponse();
        response.id = order.getId();
        response.customerId = order.getCustomerId();
        response.total = order.getTotal();
        response.createdAt = order.getCreatedAt().toString();
        return response;
    }
}
```

## 5. Testabilidade: O Grande Benefício

### Teste da Entidade (Regras de Negócio)

```java
class OrderTest {

    @Test
    void shouldCreateValidOrder() {
        Order order = new Order("CUST123", new BigDecimal("100.00"));
        
        assertNotNull(order.getId());
        assertEquals("CUST123", order.getCustomerId());
        assertEquals(new BigDecimal("100.00"), order.getTotal());
    }

    @Test
    void shouldRejectNegativeTotal() {
        assertThrows(IllegalArgumentException.class, () -> 
            new Order("CUST123", new BigDecimal("-10.00"))
        );
    }

    @Test
    void shouldCalculateDiscount() {
        Order order = new Order("CUST123", new BigDecimal("100.00"));
        BigDecimal discounted = order.calculateDiscountedTotal(new BigDecimal("10"));
        
        assertEquals(new BigDecimal("90.00"), discounted);
    }
}
```

### Teste do Caso de Uso (Sem Infraestrutura)

```java
@ExtendWith(MockitoExtension.class)
class CreateOrderUseCaseTest {

    @Mock
    private OrderRepository orderRepository;
    
    @Mock
    private NotificationGateway notificationGateway;

    private CreateOrderUseCase useCase;

    @BeforeEach
    void setUp() {
        useCase = new CreateOrderUseCase(orderRepository, notificationGateway);
    }

    @Test
    void shouldCreateOrderSuccessfully() {
        // Given
        String customerId = "CUST123";
        BigDecimal total = new BigDecimal("100.00");
        
        when(orderRepository.save(any(Order.class)))
            .thenAnswer(invocation -> {
                Order order = invocation.getArgument(0);
                // Simula ID gerado pelo banco
                return new Order("ORDER123", order.getCustomerId(), 
                               order.getTotal(), order.getCreatedAt());
            });

        // When
        Order result = useCase.execute(customerId, total);

        // Then
        assertEquals("ORDER123", result.getId());
        assertEquals(customerId, result.getCustomerId());
        assertEquals(total, result.getTotal());
        
        verify(orderRepository).save(any(Order.class));
        verify(notificationGateway).sendOrderConfirmation(result);
    }
}
```

## 6. Benefícios da Clean Architecture com Quarkus

### ✅ Independência de Frameworks
```java
// Posso trocar Panache por JPA puro, MongoDB, etc.
// Apenas mudo a implementação do OrderRepositoryImpl
```

### ✅ Testabilidade Máxima
```java
// Testo regras de negócio sem banco
// Testo casos de uso com mocks
// Testo controllers com TestRestTemplate
```

### ✅ Flexibilidade
```java
// Posso ter múltiplos adapters:
// - OrderRepositoryJpaImpl
// - OrderRepositoryMongoImpl  
// - OrderRepositoryRedisImpl
```

### ✅ Evolução Gradual
```java
// Começar simples e evoluir:
// 1. CRUD básico
// 2. Adicionar validações
// 3. Integrar APIs externas
// 4. Adicionar eventos
```

## 7. Projeto Prático: Implementação Completa

### Estrutura Final do Projeto

```
order-service/
├── src/main/java/com/example/orders/
│   ├── domain/
│   │   ├── entities/Order.java
│   │   ├── gateways/OrderRepository.java
│   │   └── gateways/NotificationGateway.java
│   ├── application/
│   │   └── usecases/
│   │       ├── CreateOrderUseCase.java
│   │       ├── FindOrderUseCase.java
│   │       └── ListOrdersByCustomerUseCase.java
│   ├── infrastructure/
│   │   ├── repositories/
│   │   │   ├── OrderEntity.java
│   │   │   └── OrderRepositoryImpl.java
│   │   └── notifications/
│   │       └── EmailNotificationGateway.java
│   └── interfaces/
│       └── rest/
│           ├── OrderResource.java
│           ├── requests/CreateOrderRequest.java
│           └── responses/OrderResponse.java
├── src/test/java/
└── pom.xml
```

### Configuração do pom.xml

```xml
<dependencies>
    <dependency>
        <groupId>io.quarkus</groupId>
        <artifactId>quarkus-resteasy-reactive-jackson</artifactId>
    </dependency>
    <dependency>
        <groupId>io.quarkus</groupId>
        <artifactId>quarkus-hibernate-orm-panache</artifactId>
    </dependency>
    <dependency>
        <groupId>io.quarkus</groupId>
        <artifactId>quarkus-jdbc-postgresql</artifactId>
    </dependency>
    <dependency>
        <groupId>io.quarkus</groupId>
        <artifactId>quarkus-hibernate-validator</artifactId>
    </dependency>
</dependencies>
```

## 8. Próximos Passos: Projetos Evolutivos

### 🚀 Nível Iniciante: CRUD Completo
- Implementar todas operações CRUD
- Adicionar validações robustas
- Testes unitários completos

### 🚀 Nível Intermediário: Integrações
- Gateway para API externa de clientes
- Sistema de notificações por email
- Implementar diferentes repositórios (PostgreSQL + Redis)

### 🚀 Nível Avançado: Microserviço Completo
- Integração com Apache Kafka
- Observabilidade com Micrometer + Prometheus
- Deploy com containers
- Circuit Breaker para resiliência

## 9. Diagrama de Sequência: Fluxo Completo

Vamos visualizar como funciona o fluxo de criação de um pedido através de todas as camadas:

### 9.1 Fluxo Principal: Criação de Pedido

```mermaid
sequenceDiagram
    participant Client as 📱 Cliente
    participant Controller as 🌐 OrderResource<br/>(Interface)
    participant UseCase as 🎪 CreateOrderUseCase<br/>(Application)
    participant Entity as 🎯 Order<br/>(Domain)
    participant Repository as 🔧 OrderRepositoryImpl<br/>(Infrastructure)
    participant Database as 🗄️ PostgreSQL
    participant Notification as 📧 NotificationGateway<br/>(Infrastructure)
    participant EmailAPI as 📮 Email Service

    Note over Client, EmailAPI: 🚀 Início do Fluxo de Criação

    Client->>+Controller: POST /orders<br/>{"customerId": "CUST123", "total": 100.00}
    
    Note over Controller: 🛡️ Validação de entrada<br/>(Bean Validation)
    
    Controller->>+UseCase: execute("CUST123", 100.00)
    
    Note over UseCase: 🎭 Orquestração do caso de uso
    
    UseCase->>+Entity: new Order("CUST123", 100.00)
    
    Note over Entity: ⚖️ Aplicação das regras de negócio<br/>- Validar total > 0<br/>- Validar customerId não nulo<br/>- Gerar ID único<br/>- Definir timestamp
    
    alt 🚫 Regras de negócio violadas
        Entity-->>UseCase: IllegalArgumentException
        UseCase-->>Controller: Exception
        Controller-->>Client: 400 Bad Request<br/>{"error": "Total deve ser positivo"}
    else ✅ Regras válidas
        Entity-->>-UseCase: Order criada com sucesso
        
        Note over UseCase: 💾 Persistir o pedido
        
        UseCase->>+Repository: save(order)
        Repository->>+Database: INSERT INTO orders...
        Database-->>-Repository: ID gerado (ex: 1001)
        Repository-->>-UseCase: Order com ID atualizado
        
        Note over UseCase: 📨 Enviar notificação
        
        UseCase->>+Notification: sendOrderConfirmation(order)
        Notification->>+EmailAPI: POST /send-email<br/>{"to": "customer@email.com", "subject": "Pedido Criado"}
        EmailAPI-->>-Notification: Email enviado
        Notification-->>-UseCase: Notificação enviada
        
        UseCase-->>-Controller: Order completa
        
        Note over Controller: 🔄 Conversão Domain → DTO
        
        Controller-->>-Client: 201 Created<br/>{"id": "1001", "customerId": "CUST123", "total": 100.00}
    end

    Note over Client, EmailAPI: ✅ Fluxo Concluído com Sucesso
```

### 9.2 Fluxo de Busca de Pedido

```mermaid
sequenceDiagram
    participant Client as 📱 Cliente
    participant Controller as 🌐 OrderResource
    participant UseCase as 🎪 FindOrderUseCase
    participant Repository as 🔧 OrderRepositoryImpl
    participant Database as 🗄️ PostgreSQL

    Client->>+Controller: GET /orders/1001
    Controller->>+UseCase: execute("1001")
    UseCase->>+Repository: findById("1001")
    Repository->>+Database: SELECT * FROM orders WHERE id = 1001
    
    alt 🚫 Pedido não encontrado
        Database-->>Repository: Resultado vazio
        Repository-->>UseCase: Optional.empty()
        UseCase-->>Controller: OrderNotFoundException
        Controller-->>Client: 404 Not Found
    else ✅ Pedido encontrado
        Database-->>-Repository: Dados do pedido
        Repository-->>-UseCase: Optional<Order>
        UseCase-->>-Controller: Order
        Controller-->>-Client: 200 OK<br/>{"id": "1001", "customerId": "CUST123"}
    end
```

### 9.3 Fluxo com Falha de Infraestrutura

```mermaid
sequenceDiagram
    participant Client as 📱 Cliente
    participant Controller as 🌐 OrderResource
    participant UseCase as 🎪 CreateOrderUseCase
    participant Entity as 🎯 Order
    participant Repository as 🔧 OrderRepositoryImpl
    participant Database as 🗄️ PostgreSQL
    participant Notification as 📧 NotificationGateway

    Client->>+Controller: POST /orders
    Controller->>+UseCase: execute(dados)
    UseCase->>+Entity: new Order(dados)
    Entity-->>-UseCase: Order válida
    
    UseCase->>+Repository: save(order)
    Repository->>+Database: INSERT INTO orders...
    
    Note over Database: 💥 Falha de conexão<br/>ou constraint violation
    
    Database-->>-Repository: SQLException
    Repository-->>-UseCase: DatabaseException
    
    Note over UseCase: 🔄 Rollback automático<br/>(Transação Quarkus)
    
    UseCase-->>-Controller: DatabaseException
    Controller-->>-Client: 500 Internal Server Error<br/>{"error": "Erro interno do servidor"}
    
    Note over Client: 📝 Log detalhado no servidor<br/>Cliente recebe erro genérico
```

### 9.4 Análise dos Benefícios Arquiteturais

**🎯 Separação Clara de Responsabilidades**
- **Controller**: Apenas conversão HTTP ↔ Domain
- **UseCase**: Orquestração e regras de aplicação
- **Entity**: Regras de negócio puras
- **Repository**: Apenas persistência

**🛡️ Proteção das Regras de Negócio**
- Validações na Entity são invioláveis
- UseCase não conhece detalhes HTTP ou banco
- Mudanças na infraestrutura não afetam o domínio

**🧪 Facilidade de Testes**
- **Entity**: Teste unitário puro (sem mocks)
- **UseCase**: Mock apenas dos gateways
- **Controller**: Teste de integração com TestRestTemplate

**🔄 Flexibilidade de Evolução**
- Trocar PostgreSQL por MongoDB: apenas Repository
- Adicionar cache: decorator no Repository
- Mudar notificação: apenas NotificationGateway

## Conclusão

A Clean Architecture com Quarkus oferece:

1. **Código Limpo**: Cada camada tem sua responsabilidade
2. **Flexibilidade**: Troque tecnologias sem dor
3. **Testabilidade**: Teste tudo, até regras complexas
4. **Manutenibilidade**: Evolua o código com confiança
5. **Performance**: Quarkus + arquitetura limpa = velocidade

**Lembre-se**: Comece simples, aplique os conceitos gradualmente e sempre mantenha as regras de dependência: **de fora para dentro, nunca o contrário**.