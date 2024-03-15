## Curso Alura - Java e JPA: consultas avançadas, performance e modelos complexos

### @Embedded e @Embeddable - Composição:
Utilizamos a anotação *@Embedded* quando queremos que os atributos da classe que compõe a classe principal seja persistida como se fosse da classe principal, exemplo:
```java
@Entity
@Table(name = "clientes")
public class Cliente {

    @Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long id;

	@Embedded
	private DadosPessoais dadosPessoais;

  // outros atributos
```
No banco será criado uma tabela 'clientes' contendo os atributos explícitos na classe Cliente, como 'id' e também será criados os atributos da classe 'DadosPessoais'.
Obs.: A classe DadosPessoais deve estar anotada com *@Embeddable*

### Obs:
Por padrão todo relacionamento toOne é *EAGER* e todo toMany é *LAZY*. Para tornar as consultas mais perfomáticas, adicionar a anotação (fetch = FetchType.LAZY) em relacionamentos toOne.
```java
@ManyToOne(fetch = FetchType.LAZY)
private Pedido pedido;
```

### Join Fetch:
Caso precise carregar atributos que estejam anotados como *LAZY*, pode-se criar uma nova consulta usando "Query" da seguinte forma:
```java
public Pedido buscarPedidoComCliente(Long id) {
    return em.createQuery("SELECT p FROM Pedido p JOIN FETCH p.cliente WHERE p.id = :id", Pedido.class)
        .setParameter("id", id)
        .getSingleResult();
}
```
Dessa forma, o atributo "cliente" será carregado na consulta, mesmo que esteja como carregamento *LAZY*.

### Retornar somente alguns campos da tabela:
Para algumas consultas, onde não há a necessidade do carregamento de todos os campos, é necessário criar uma classe que represente os campos requeridos, exemplo:
```java
public List<RelatorioDeVendasVo> relatorioDeVendas() {
    String jpql = "SELECT new br.com.alura.loja.vo." +
        "RelatorioDeVendasVo(produto.nome, SUM(item.quantidade), MAX(pedido.data)) "
        + "FROM Pedido pedido "
        + "JOIN pedido.itens item "
        + "JOIN item.produto produto "
        + "GROUP BY produto.nome "
        + "ORDER BY item.quantidade DESC";
    return em.createQuery(jpql, RelatorioDeVendasVo.class)
        .getResultList();
}
```
A classe *RelatorioDeVendasVo* precisa do contrutor para (String, Long, LocalDate) para retornar os campos das classes *Produto*, *Item* e *Pedido* 
