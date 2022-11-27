# Grupo 1

## Spring Boot
## O que é
## Como Funciona Internamente
## Controller Rest
### O que é 
### Anotações e Parâmetros das Anotações
### Exemplos
### Construtores, Acessores e Modificadores Necessários
### Mapeamento de Verbos
### Exemplos
### Como Receber Dados da Chamada
### Como Retornar Dados e Status
### Classes e Métodos Adicionais
### Exemplos
----------------------------------
# Grupo 2

## Testes Unitários e de Integração
### O que são
## A Ferramenta Básica: JUnit
### O que é
### Cookbook
#### Pacotes (imports)
#### Anotações
#### Asserções
#### Execmplos
-----------------------------------
# Grupo 3

## Mocks
### O que são
### Quando usar
## A Ferramenta Mockito
### O que é
### Cookbook
### Pacotes (imports)
### Anotações e cláusulas
### Exemplos
### Testes Unitários e Mocks com Spring Rest Controller
-----------------------------------
# Grupo 4 

## Spring Data JPA
É um projeto que está dentro do sistema Spring, que auxilia na produção da camada de persistência da aplicação. **Spring Data JPA** se assemelha a uma camada de abstração acima do JPA.
O JPA ajuda a abstrair o banco de dados e consultas, transformando tudo em objeto relacional, já o **Spring Data JPA** ajuda na criação dos repositórios, dentro do framework existem diversas ferramentas uteis para todas as aplicações, sem precisar implementar nada adicional.
O **Spring Data JPA** é parte da família Spring Data que facilita a implementação de repositórios baseados em JPA. Ele facilita a construção de aplicações baseadas em Spring Data que utilizam tecnologias de acesso a dados.
Muito código clichê deve ser escrito para executar consultas simples, bem como realizar paginação e auditoria. O Spring Data JPA visa melhorar significativamente as implementações da camada de acesso aos dados, reduzindo as despesas gerais para o que é 
realmente necessário.

### Características
- Suporte sofisticado para construir repositórios baseados em Spring e JPA
- Suporte para predicados Querydsl e, portanto, consultas JPA de tipo seguro
- Auditoria transparente da classe de domínio
- Suporte a paginação, execução de consulta dinâmica, capacidade de integrar código de acesso a dados personalizados
- Validação de **@Queryconsultas** anotadas em tempo de bootstrap
- Suporte para mapeamento de entidades baseado em XML
- Configuração de repositório baseada em JavaConfig introduzindo **@EnableJpaRepositories**.
## Classes e Interfaces
## Anotações
Há diversas anotações disponíveis no Spring Data JPA, tais como:
- @Query

Define uma implementação de uma consulta com o Java Persistence Query Language (JPQL) para um repositório.

Exemplo:
```
@Query("FROM Estudante e WHERE e.matricula = :matricula");
Estudante getStudentByEnrollment(@Param("matricula") String matricula);
```
- @Procedure

Define o armazenamento de procedimentos que podem ser chamados pelo repositório

Exemplo de um procedimento:
```
@NamedStoredProcedureQueries({ 
    @NamedStoredProcedureQuery(
        name = "count_estudantes", 
        procedureName = "estudante.count_estudantes", 
        parameters = { 
            @StoredProcedureParameter(
                mode = ParameterMode.IN, 
                name = "matricula", 
                type = String.class),
            @StoredProcedureParameter(
                mode = ParameterMode.OUT, 
                name = "count", 
                type = Long.class) 
            }
    ) 
})

class Estudante {}
```
Esse procedimento acima pode ser chamado assim:
```
@Procedure(name = "count_estudantes")
long getStudentsCount(@Param("matricula") String matricula);
```
- @Lock

Através dessa anotação é possível definir um acesso exclusivo ao dado quando for executar um query no repositório. Essa anotação possuí bastante importância quando se precisa garantir que nenhuma outra transação modifique aquele dado.

Há vários modos disponíveis:
- NONE
- READ
- WRITE
- OPTIMISTIC
- OPTIMISTIC_FORCE_INCREMENT
- PESSIMISTIC_READ
- PESSIMISTIC_WRITE
- PESSIMISTIC_FORCE_INCREMENT

Exemplo:
```
@Lock(LockModeType.NONE)
@Query("FROM Estudante e WHERE e.matricula = :matricula");
Estudante getStudentByEnrollment(@Param("matricula") String matricula);
```
- @Modifying

Define que um dado do repositório será modificado.

Exemplo:
```
@Modifying
@Query("UPDATE Estudante e SET e.name = :name WHERE e.matricula = :matricula")
void changeStudentName(@Param("matricula") String matricula, @Param("name") String name);
```
- @EnableJpaRepositories

Essa anotação indica que repositórios JPA serão usados no projeto. Essa anotação é usada em conjunto com a @Configuration
```
@Configuration
@EnableJpaRepositories
class JPAConfiguration {}
```

O Spring procura por repositórios nos pacotes da classe que possuir a anotação @Configuration. Isso pode ser alterado da seguinte forma:
```
@Configuration
@EnableJpaRepositories(basePackages = "br.pucrs.persistence.dao")
class JPAConfiguration {}
```

## Anotações de Mapeamento
Há cerca de diversas anotações de mapeamento no Spring Data JPA. Muitas dessas derivadas do próprio JPA. O exemplo de algumas são:

- @Entity

Define uma entidade para armazenar um objeto. 
Também é usada para especificar que um repositório pertence ao Spring Data JPA.

Exemplo:
```
@Entity
public class Estudante {}

// Repositório
interface EstudanteRepository extends Repository<Estudante, String> { … }
```
- @Id

Essa anotação é utilizada para mapear qual campo estará relacionado a chave primária na tabela do banco. Essa anotação é obrigatória.

Exemplo com a entidade estudante:
```
@Entity
public class Estudante {
    @Id
    private String matricula;
}
```
- @Table

É usado para definir o nome da tabela que será relacionada a entidade no banco. 

Exemplo com a entidade estudante:
```
@Table(name = "estudante")
@Entity
public class Estudante {
    @Id
    private String matricula;
}
```
- @OneToOne

Define um relacionamento de um para um entre duas entidades. Essa anotação é usada nas propriedades que terão essa ligação.

Exemplo com a entidade estudante:

```
@Table(name = "estudante")
@Entity
public class Estudante {
    @Id
    private String matricula;

    @OneToOne(cascade = CascadeType.ALL)
    @JoinColumn(name = "endereco_id", referencedColumnName = "id")
    private Endereco endereco;
}
```

No caso acima, a entidade endereço possui uma ligação um pra um com um estudante. É usada a anotação @JoinColumn para mapear a entidade endereço através do seu id.

Quanto a entidade endereço:

```
@Table(name = "endereco")
@Entity
public class Endereco {
    @Id
    private Long id;

    @OneToOne(mappedBy = "endereco")
    private Estudante estudante;
}
```
## Cookbook
### Como usar
### Exemplos
## Testes Unitários e Mocks com Repositórios
Para testar o repositório é recomendado utilizar um banco em memória, para evitar problemas no banco de produção.
Com isso, é preciso adicionar a dependência que será responsável pelo banco em memória.
```
<dependecy>
  <groupId>com.h2database</groupId>
  <artifactId>h2</artifactId>
  <scope>test</scope>
</dependecy>
```
Iniciando a implantação, é preciso adicionar algumas anotações para que possamos testar unitariamente

- **@RunWith(SpringRunner.class)**
Permite lançar um contexto de aplicação para o teste (SpringTestContext)

- **@DataJpaTest**
Responsável por fazer várias configurações que facilitam na criação dos testes

- **@Autowired**
Para a injeção do repositório que desejamos testar

Exemplo teste unitário de repositório:
```
@RunWith(SpringRunner.class)
@DataJpaTest
public class StudentRepositoryTest {

	@Autowired
	private StudentRepository studentRepository;

	@Test
	public void createShouldPersistData(){
		Student student = new Student( name: "Alex", email: "alexelias@pucrs.com.br");
		this.studentRepository.save(student) ;
		assertThat (student.getId()).isNotNull() ;
		assertThat (student.getName()).isEqualTo("Alex") ;
		assertThat (student.getEmail()).isEqualTo("alexelias@pucrs.com.br") ;
	}

	@Test
	public void deleteShouldPersistData(){
		Student student = new Student( name: "Alex", email: "alexelias@pucrs.com.br") ;
		this.studentRepository.save(student);
		studentRepository.delete(student) ;
		assertThat (studentRepository.findOne(student.getId())).isNull() ;
	}
}
```

Para mockarmos um repositório é preciso utilizar a anotação:
- **@MockBean**

No teste, para utilizarmos o mock, precisamos usar a biblioteca **Mockito**. A função Mockito.when("Metodo a ser mockado").thenReturn("o que deve ser retornado") é responsável por mockar o resposotório.

Exemplo de mock com repositório:
```
public class VendaServiceTest extends AplicationConfigTest {

    @MockBean
    private VendaRepository repository;

    @MockBean
    private EstoqueService estoqueService;

    @MockBean
    private ClienteRepository clienteRepository;

    @MockBean
    private TitulosReceberService titulosReceberService;

    @Autowired
    private VendaService vendaService;

    @Test
    @DisplayName("deve efetuar uma venda")
    public void deveEfetuarVenda() {
        VendaDto.ItemVendaDto item = VendaDto.ItemVendaDto.builder()
                .produtoId("id")
                .valorUnitario(BigDecimal.TEN)
                .quantidade(BigDecimal.ONE)
                .build();

        List<VendaDto.ItemVendaDto> itens = Collections.singletonList(item);
        VendaDto dto = VendaDto.builder()
                .itens(itens)
                .cliente(null)
                .valorRecebido(BigDecimal.TEN)
                .mobile(true)
                .build();

        Venda venda = Mockito.mock(Venda.class);
        Mockito.when(repository.save(ArgumentMatchers.any(Venda.class))).thenReturn(venda);

        vendaService.efetuarVenda(dto);

        Mockito.verify(clienteRepository, Mockito.never()).findById(ArgumentMatchers.anyString());
        Mockito.verify(titulosReceberService, Mockito.times(1)).gerarTituloAPartirVenda(ArgumentMatchers.eq(venda), dto);
        Mockito.verify(repository, Mockito.times(1)).save(ArgumentMatchers.any(Venda.class));
        Mockito.verify(estoqueService, Mockito.times(1)).reduzirNoEstoque(ArgumentMatchers.anyString(), ArgumentMatchers.any(BigDecimal.class));
    }

    @Test
    @DisplayName("deve remover uma venda")
    public void deveRemoverVenda() {
        String vendaId = "id-mock";
        Venda venda = Mockito.mock(Venda.class);
        Mockito.when(venda.getValorRecebido()).thenReturn(BigDecimal.TEN);
        Mockito.when(venda.getItens()).thenReturn(Collections.emptyList());
        Mockito.when(venda.getValorTotal()).thenReturn(BigDecimal.TEN);
        Optional<Venda> vendaOptoOptional = Optional.of(venda);
        Mockito.when(repository.findById(ArgumentMatchers.eq(vendaId))).thenReturn(vendaOptoOptional);

        vendaService.removerVenda(vendaId);
        Mockito.verify(repository, Mockito.times(1)).delete(ArgumentMatchers.any(Venda.class));
        Mockito.verify(estoqueService, Mockito.never()).adicionarNoEstoque(ArgumentMatchers.any(), ArgumentMatchers.any());
        Mockito.verify(titulosReceberService, Mockito.never()).removerTitulo(ArgumentMatchers.any());
    }
}
```
------------------------------------
# Grupo 5

## Spring Security

## O que é
## Como funciona
### Cenários de uso (User details, OAuth com Token)
### Cadeia de Filtros
### Autorização e Roles
## Cookbook
### Como configurar
### Exemplos
### Autenticação com User Details
### Exemplos
### Autorização (com roles) de RestControllers e Beans
### Exemplos
### Autenticação com OAuth e Toke
### Exemplos
### Autorização (com roles) de RestControllers e Beans
### Exemplos
