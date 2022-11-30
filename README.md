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
É um projeto que está dentro do sistema Spring, que auxilia na produção da camada de persistência da aplicação. **Spring Data JPA** se assemelha a uma camada de abstração acima do JPA.\
O JPA ajuda a abstrair o banco de dados e consultas, transformando tudo em objeto relacional, já o **Spring Data JPA** ajuda na criação dos repositórios, dentro do framework existem diversas ferramentas uteis para todas as aplicações, sem precisar implementar nada adicional.
O **Spring Data JPA** é parte da família Spring Data que facilita a implementação de repositórios baseados em JPA. Ele facilita a construção de aplicações baseadas em Spring Data que utilizam tecnologias de acesso a dados.\
Muito código clichê deve ser escrito para executar consultas simples, bem como realizar paginação e auditoria. O Spring Data JPA visa melhorar significativamente as implementações da camada de acesso aos dados, reduzindo as despesas gerais para o que é realmente necessário.

### Características
- Suporte sofisticado para construir repositórios baseados em Spring e JPA
- Suporte para predicados Querydsl e, portanto, consultas JPA de tipo seguro
- Auditoria transparente da classe de domínio
- Suporte a paginação, execução de consulta dinâmica, capacidade de integrar código de acesso a dados personalizados
- Validação de **@Queryconsultas** anotadas em tempo de bootstrap
- Suporte para mapeamento de entidades baseado em XML
- Configuração de repositório baseada em JavaConfig introduzindo **@EnableJpaRepositories**.
## Classes e Interfaces
Classes e interfaces reduzem a complexidade e a quantidade de código fonte que seria de responsabilidade do programador nas operações de CRUD.

O Spring Data JPA consegue relacionar uma tabela com uma classe e as colunas desta tabela como os atributos da classe:

```
@Entity
@Table(name = "disciplinas")
public class Disciplina {

  @Id
  @GeneratedValue(strategy = GenerationType.SEQUENCE)
  private long id;

  @Column(name = "codigo_disciplina")
  private String codigoDisciplina;

  @Column(name = "nome")
  private String nome;

  public long getId() {
    return id;
  }

  public String getCodigoDisciplina() {
    return codigoDisciplina;
  }

  public void setCodigoDisciplina(String codigoDisciplina) {
    this.codigoDisciplina = codigoDisciplina;
  }

  public String getNome() {
    return nome;
  }

  public void setNome(String nome) {
    this.nome = nome;
  }

  public Disciplina(String codigoDisciplina, String nome) {
    this.codigoDisciplina = codigoDisciplina;
    this.nome = nome;
  }

  @Override
	public String toString() {
		return "Tutorial [codigoDisciplina=" + codigoDisciplina + ", nome=" + nome "]";
	}
}
```
Precisamos criar uma interface específica para cada classe de entidade e, dentro delas, estender a interface **JpaRepository**. Ao herdar a interface **JpaRepository**, fornece métodos do CRUD, tais como: **delete**, **save**, **find** e etc. Exemplo de interface da classe **Disciplina**:

```
public interface DisciplinaRepository extends JpaRepository<Disciplina, Long> {
  Disciplina findByCodigoDisciplinaAndTurma(String codigoDisciplina, int turma);
}
```
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
Neste exemplo, demonstraremos como configurar o Spring Framework para se comunicar com o banco de dados usando JPA e Hibernate como fornecedor do JPA. Os benefícios de usar o Spring Data é que ele remove muito código clichê e fornece uma
e implementação mais legível da camada DAO. Além disso, ajuda a tornar o código livremente acoplado e, como tal, alternar entre diferentes fornecedores de JPA é uma questão de configuração.
Então, vamos configurar o banco de dados para o exemplo. Usaremos o banco de dados MySQL para esta demonstração. Criamos uma tabela "funcionário" com
2 colunas como mostrado:

```
CREATE TABLE ‘employee‘ (
‘employee_id‘ bigint(20) NOT NULL AUTO_INCREMENT,
‘employee_name‘ varchar(40) ,
PRIMARY KEY (‘employee_id‘)
)

```
Em primeiro lugar, criamos a classe Employee com EmployeesId e EmployeesName. A classe Person será a entidade que vamos armazenar e recuperar do banco de dados usando o JPA.
O @Entity marca a classe como a Entidade JPA. Mapeamos as propriedades da classe Employee com as colunas da classe Employee tabela e a entidade com a própria tabela de funcionários usando a anotação @Table.
O método toString é substituído para que possamos obter uma saída significativa quando imprimirmos a instância da classe.


Employee.java
```
package com.jcg.bean;
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Table;
@Entity
@Table(name="employee")
public class Employee
{
@Id
@GeneratedValue(strategy=GenerationType.AUTO)
@Column(name = "employee_id")
private long employeeId;
@Column(name="employee_name")
private String employeeName;
public Employee()
{
}
public Employee(String employeeName)
{
this.employeeName = employeeName;
}
public long getEmployeeId()
{
return this.employeeId;
}
public void setEmployeeId(long employeeId)
{
this.employeeId = employeeId;
}
public String getEmployeeName()
{
return this.employeeName;
}
public void setEmployeeName(String employeeName)
{
this.employeeName = employeeName;
}
@Override
public String toString()

{
return "Employee [employeeId=" + this.employeeId + ", employeeName=" + this ←-
.employeeName + "]";
}
}

```
### Exemplos
Uma vez que a Entidade esteja pronta, definimos a interface para armazenamento e recuperação da entidade, ou seja, criaremos um Data Access Interface.

EmployeeDao.java
```
package com.jcg.dao;
import java.sql.SQLException;
import com.jcg.bean.Employee;
public interface EmployeeDao
{
void save(Employee employee) throws SQLException;
Employee findByPrimaryKey(long id) throws SQLException;
}

```

Em seguida, tentaremos implementar a interface de acesso a dados e criar o objeto de acesso a dados real que modificará o Person entity.


EmployeeDaoImpl.java
```
package com.jcg.impl;
import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;
import org.springframework.stereotype.Repository;
import org.springframework.transaction.annotation.Propagation;
import org.springframework.transaction.annotation.Transactional;
import com.jcg.bean.Employee;
import com.jcg.dao.EmployeeDao;
@Repository("EmployeeDaoImpl")
@Transactional(propagation = Propagation.REQUIRED)
public class EmployeeDaoImpl implements EmployeeDao
{
@PersistenceContext
private EntityManager entityManager;
@Override
public void save(Employee employee)
{
entityManager.persist(employee);
}
@Override
public Employee findByPrimaryKey(long id)
{
Employee employee = entityManager.find(Employee.class, id);
return employee;
}

/**
* @return the entityManager
*/
public EntityManager getEntityManager()
{
return entityManager;
}
/**
* @param entityManager the entityManager to set
*/
public void setEntityManager(EntityManager entityManager)
{
this.entityManager = entityManager;
}
}

```
A classe DAO Implementation é anotada com @Repository, que marca como um Repository Bean e solicita o Spring Bean Factory para carregar o Bean. @Transactional pede ao container para fornecer uma transação para usar os métodos desta classe. Propagation.REQUIRED denota que a mesma transação é usada se uma estiver disponível quando vários métodos que exigem transação são aninhadas. O contêiner cria uma única transação física no banco de dados e várias transações lógicas para cada método aninhado. No entanto, se um método não conseguir concluir uma transação com sucesso, toda a transação física será rolou para trás. Uma das outras opções é Propagation.REQUIRES_NEW, em que uma nova transação física é criada para cada método. Existem outras opções que ajudam a ter um bom controle sobre o gerenciamento de transações.
A anotação @PersistenceContext informa ao contêiner para injetar uma instância de entitymanager no DAO. 
A classe implementa os métodos save e findbyPk que salvam e buscam os dados usando a instância de EntityManager injetada.
Agora definimos nossa Unidade de persistência no Persistence.xml que é colocado na pasta META-INF em src. Nós então mencione a classe cujas instâncias serão usadas Persisted. Para este exemplo, é a entidade Employee que criamos anteriormente.


persistence.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<persistence xmlns="https://java.sun.com/xml/ns/persistence"
version="1.0">
<persistence-unit name="jcgPersistence" transaction-type="RESOURCE_LOCAL" >
<class>com.jcg.bean.Employee</class>
</persistence-unit>
</persistence>
```

Agora configuramos o Spring Container usando o arquivo spring-configuration.xml.


spring-configuration.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="https://www.springframework.org/schema/beans"
xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns:aop="https://www. ←-
springframework.org/schema/aop"
xmlns:context="https://www.springframework.org/schema/context" xmlns:tx="https:// ←-
www.springframework.org/schema/tx"
xsi:schemaLocation="https://www.springframework.org/schema/beans
https://www.springframework.org/schema/beans/spring-beans-3.0.xsd
https://www.springframework.org/schema/aop
https://www.springframework.org/schema/aop/spring-aop-3.0.xsd
https://www.springframework.org/schema/context
https://www.springframework.org/schema/context/spring-context-3.0.xsd
https://www.springframework.org/schema/tx
https://www.springframework.org/schema/tx/spring-tx.xsd">
<context:component-scan base-package="com.jcg" />
<bean id="dataSource"
class="org.springframework.jdbc.datasource.DriverManagerDataSource">
<property name="driverClassName" value="com.mysql.jdbc.Driver" />
<property name="url" value="jdbc:mysql://localhost:3306/jcg" />
<property name="username" value="root" />
<property name="password" value="toor" />
</bean>
<bean id="jpaDialect" class="org.springframework.orm.jpa.vendor.HibernateJpaDialect ←-
" />
<bean id="entityManagerFactory"
class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean">
<property name="persistenceUnitName" value="jcgPersistence" />
<property name="dataSource" ref="dataSource" />
<property name="persistenceXmlLocation" value="META-INF/persistence.xml" />
<property name="jpaVendorAdapter" ref="jpaVendorAdapter" />
<property name="jpaDialect" ref="jpaDialect" />
<property name="jpaProperties">
<props>
<prop key="hibernate.hbm2ddl.auto">validate</prop>
<prop key="hibernate.dialect">org.hibernate.dialect.MySQL5Dialect</prop>
</props>
</property>
</bean>
<bean id="jpaVendorAdapter"
class="org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter">
</bean>
<bean id="txManager" class="org.springframework.orm.jpa.JpaTransactionManager">
<property name="entityManagerFactory" ref="entityManagerFactory" />
<property name="dataSource" ref="dataSource" />
<property name="jpaDialect" ref="jpaDialect" />
</bean>
<tx:annotation-driven transaction-manager="txManager" />
</beans>

```
Definimos os beans que precisamos no arquivo spring-configuration.xml. A fonte de dados contém as propriedades básicas de configuração, como URL, nome de usuário, senha e o nome de classe do driver JDBC. Criamos um EntityManagerFactory usando o LocalContainerEntityManagerFactoryBean. as propriedades são fornecidos como a fonte de dados, persistenceUnitName, persistenceUnitLocation, dialeto etc. A instância de EntityManager é injetado deste FactoryBean na instância EmployeeDaoImpl. A linha 51 no XML acima solicita que o contêiner Spring gerencie transações usando o contêiner Spring. A classe TransactionManagerProvider é a classe JpaTransactionManager. Agora que concluímos todo o trabalho duro, é hora de testar a configuração:
A classe SpringDataDemo extrai o EmployeeDaoImpl e tenta salvar uma instância de Employee na tabela de funcionários e recuperar a mesma instância do banco de dados.


SpringDataDemo.java

```
package com.jcg;
import java.sql.SQLException;
import org.springframework.beans.BeansException;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import com.jcg.bean.Employee;
import com.jcg.dao.EmployeeDao;
public class SpringDataDemo
{
public static void main(String[] args)
{
try
{
ApplicationContext context = new ClassPathXmlApplicationContext(" ←-
resources\\spring-configuration.xml");
//Fetch the DAO from Spring Bean Factory
EmployeeDao employeeDao = (EmployeeDao)context.getBean(" ←-
EmployeeDaoImpl");
Employee employee = new Employee("Employee123");
//employee.setEmployeeId("1");
//Save an employee Object using the configured Data source
employeeDao.save(employee);
System.out.println("Employee Saved with EmployeeId "+employee. ←-
getEmployeeId());
//find an object using Primary Key
Employee emp = employeeDao.findByPrimaryKey(employee.getEmployeeId ←-
());
System.out.println(emp);
//Close the ApplicationContext
((ConfigurableApplicationContext)context).close();
}
catch (BeansException | SQLException e)
{
e.printStackTrace();
}
}
}

```
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
