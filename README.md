# API-rest-Java-

Vamos construir uma API REST simples usando Java 17, Spring Boot 3, Spring Data JPA e Swagger para documentação. Aqui está um passo a passo com os códigos necessários:

# 1. Configuração do Projeto

Primeiro, crie um novo projeto Spring Boot utilizando [Spring Initializr](https://start.spring.io/):

- Project: Maven Project
- Language: Java
- Spring Boot: 3.x.x
- Dependencies: 
  - Spring Web
  - Spring Data JPA
  - H2 Database (ou outro banco de dados)
  - Spring Boot DevTools
  - Springdoc OpenAPI (Swagger)

Após gerar o projeto, extraia e abra em sua IDE preferida.

# 2. Configuração do `pom.xml`

No arquivo `pom.xml`, adicione as dependências do Springdoc para a documentação da API:

```xml
<dependency>
    <groupId>org.springdoc</groupId>
    <artifactId>springdoc-openapi-ui</artifactId>
    <version>1.6.14</version>
</dependency>
```

# 3. Configuração do Banco de Dados

Se você estiver usando o H2 como banco de dados, configure a conexão no arquivo `src/main/resources/application.properties`:

```properties
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=password
spring.h2.console.enabled=true
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

# 4. Modelagem da Entidade

Crie uma nova classe chamada `Produto` no pacote `com.exemplo.demo.model`:

```java
package com.exemplo.demo.model;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;

@Entity
public class Produto {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String nome;
    private Double preco;

    // Getters e Setters
}
```

# 5. Repositório

Crie um repositório para a entidade `Produto` no pacote `com.exemplo.demo.repository`:

```java
package com.exemplo.demo.repository;

import com.exemplo.demo.model.Produto;
import org.springframework.data.jpa.repository.JpaRepository;

public interface ProdutoRepository extends JpaRepository<Produto, Long> {
}
```

# 6. Serviço

Crie um serviço para a lógica de negócios no pacote `com.exemplo.demo.service`:

```java
package com.exemplo.demo.service;

import com.exemplo.demo.model.Produto;
import com.exemplo.demo.repository.ProdutoRepository;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class ProdutoService {
    
    private final ProdutoRepository produtoRepository;

    public ProdutoService(ProdutoRepository produtoRepository) {
        this.produtoRepository = produtoRepository;
    }

    public List<Produto> listarProdutos() {
        return produtoRepository.findAll();
    }

    public Produto adicionarProduto(Produto produto) {
        return produtoRepository.save(produto);
    }
}
```

# 7. Controlador

Crie um controlador para expor a API no pacote `com.exemplo.demo.controller`:

```java
package com.exemplo.demo.controller;

import com.exemplo.demo.model.Produto;
import com.exemplo.demo.service.ProdutoService;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/api/produtos")
public class ProdutoController {

    private final ProdutoService produtoService;

    public ProdutoController(ProdutoService produtoService) {
        this.produtoService = produtoService;
    }

    @GetMapping
    public ResponseEntity<List<Produto>> listarProdutos() {
        return ResponseEntity.ok(produtoService.listarProdutos());
    }

    @PostMapping
    public ResponseEntity<Produto> adicionarProduto(@RequestBody Produto produto) {
        return new ResponseEntity<>(produtoService.adicionarProduto(produto), HttpStatus.CREATED);
    }
}
```

# 8. Configuração do Swagger

Agora, você pode acessar a documentação do Swagger no seu navegador. Inicie a aplicação e acesse `http://localhost:8080/swagger-ui.html`.

# 9. Deploy no Railway

Para fazer o deploy no Railway:

1. Crie uma conta em [Railway](https://railway.app/).
2. Crie um novo projeto e conecte seu repositório GitHub.
3. Configure as variáveis de ambiente, se necessário, e inicie o deploy.

# Conclusão

Com isso, você terá uma API REST básica utilizando Java 17, Spring Boot 3, Spring Data JPA e Swagger. Você pode expandir essa estrutura básica adicionando mais funcionalidades, como validações e tratamento de exceções.
