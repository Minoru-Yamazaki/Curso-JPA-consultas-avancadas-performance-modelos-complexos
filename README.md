## Curso Alura - Java e JPA: consultas avançadas, performance e modelos complexos

### @Embedded e @Embeddable - Composição:
![image](https://github.com/Minoru-Yamazaki/Curso-JPA-consultas-avancadas-performance-modelos-complexos/assets/73949493/007b8a40-7661-4e9c-b65a-d344628aea69)


### Obs:
>> Por padrão todo relacionamento toOne é *EAGER* e todo toMany é *LAZY*. Para tornar as consultas mais perfomáticas, adicionar a anotação (fetch = FetchType.LAZY) em relacionamentos toOne.
![image](https://github.com/Minoru-Yamazaki/Curso-JPA-consultas-avancadas-performance-modelos-complexos/assets/73949493/1d7c8c15-3066-4082-8fd1-0c1b120885d9)

### Join Fetch:
>> Caso precise carregar atributos que estejam anotados como *LAZY*, pode-se criar uma nova consulta usando "Query" da seguinte forma:
>> ![image](https://github.com/Minoru-Yamazaki/Curso-JPA-consultas-avancadas-performance-modelos-complexos/assets/73949493/30308d99-e249-4ced-8673-1200371156b0)
>> Dessa forma, o atributo "cliente" será carregado na consulta, mesmo que esteja como carregamento *LAZY*.
link: https://github.com/Minoru-Yamazaki/Curso-JPA-consultas-avancadas-performance-modelos-complexos/blob/master/src/main/java/br/com/alura/loja/dao/PedidoDao.java
