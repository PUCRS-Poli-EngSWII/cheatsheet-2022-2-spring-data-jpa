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
### O que é
É um projeto que está dentro do sistema Spring, que auxilia na produção da camada de persistência da aplicação. **Spring Data JPA** se assemelha a uma camada de abstração acima do JPA.
O JPA ajuda a abstrair o banco de dados e consultas, transformando tudo em objeto relacional, já o **Spring Data JPA** ajuda na criação dos repositórios, dentro do framework existem diversas ferramentas uteis para todas as aplicações, sem precisar implementar nada adicional.
## Classes e Interfaces
## Anotações
Há as seguintes anotações disponíveis no Spring Data JPA:
1. @Query

Define uma implementação de uma consulta com o Java Persistence Query Language (JPQL) para um repositório.

Exemplo:
```
@Query("FROM Estudante e WHERE e.matricula = :matricula");
Estudante getStudentByEnrollment(@Param("matricula") String matricula);
```
2. @Procedure

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
3. @Lock

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
4. @Modifying

Define que um dado do repositório será modificado.

Exemplo:
```
@Modifying
@Query("UPDATE Estudante e SET e.name = :name WHERE e.matricula = :matricula")
void changeStudentName(@Param("matricula") String matricula, @Param("name") String name);
```
5. @EnableJpaRepositories

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
### Exemplos
## Cookbook
### Como usar
### Exemplos
## Testes Unitários e Mocks com Repositórios
### Como fazer
### Exemplos
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
