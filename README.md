# Psc
import jakarta.persistence.*;

@Entity
@Table(name = "Avaliacoes")
public class Avaliacoes {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer idAvaliacoes;

    @ManyToOne
    @JoinColumn(name = "idProcedimento")
    private Procedimentos procedimento;

    private Integer nota;
    private Integer data;

    @ManyToOne
    @JoinColumn(name = "idCliente")
    private Cliente cliente;

    private String comentario;

    // Getters e setters...
}
                                                                                                                                                                                                 Procedimentos.java                                                                                                                                                                   import jakarta.persistence.*;
import java.util.List;

@Entity
@Table(name = "Procedimentos")
public class Procedimentos {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer idProcedimentos;

    private String nome;
    private Integer preco;
    private String descricao;

    @OneToMany(mappedBy = "procedimento")
    private List<Avaliacoes> avaliacoes;

    @OneToMany(mappedBy = "procedimento")
    private List<Agendamentos> agendamentos;

    @OneToMany(mappedBy = "procedimento")
    private List<VendasDetalhe> vendasDetalhes;

    // Getters e setters...
}
                                                                                                                                                                                                          Funcionario.java                                                                                                                                                                       import jakarta.persistence.*;
import java.util.List;

@Entity
@Table(name = "Funcionario")
public class Funcionario {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer idFuncionario;

    private String nome;
    private Integer telefone;
    private String endereco;
    private Integer CPF;
    private String email;
    private Integer curriculo;

    @OneToMany(mappedBy = "funcionario")
    private List<Agendamentos> agendamentos;

    // Getters e setters...
}
                                                                                                                                                                                      Cliente.java                                                                                                                                                                                                                  import jakarta.persistence.*;
import java.util.List;

@Entity
@Table(name = "Cliente")
public class Cliente {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer idCliente;

    private String nome;
    private Integer telefone;
    private String endereco;
    private Integer CPF;
    private String email;

    @OneToMany(mappedBy = "cliente")
    private List<Avaliacoes> avaliacoes;

    @OneToMany(mappedBy = "cliente")
    private List<Agendamentos> agendamentos;

    @OneToMany(mappedBy = "cliente")
    private List<VendasDetalhe> vendasDetalhes;

    // Getters e setters...
}
                                                                                                                                                                                                                                          Agendamentos.java                                                                                                                                                                           import jakarta.persistence.*;

@Entity
@Table(name = "Agendamentos")
public class Agendamentos {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer idAgendamentos;

    @ManyToOne
    @JoinColumn(name = "idProcedimentos")
    private Procedimentos procedimento;

    @ManyToOne
    @JoinColumn(name = "idFuncionario")
    private Funcionario funcionario;

    @ManyToOne
    @JoinColumn(name = "idCliente")
    private Cliente cliente;

    private Integer data;
    private Integer horario;
    private String local;
    private String metodoDePagamento;

    // Getters e setters...
}
                                                                                                                                                                                                   Vendas.java                                                                                                                                                                             import jakarta.persistence.*;
import java.util.List;

@Entity
@Table(name = "Vendas")
public class Vendas {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer idVendas;

    @ManyToOne
    @JoinColumn(name = "idProcedimentos")
    private Procedimentos procedimento;

    private Integer data;

    @OneToMany(mappedBy = "venda")
    private List<VendasDetalhe> detalhes;

    // Getters e setters...
}
                                                                                                                                                                                                                VendasDetalhe.java                                                                                                                                                               import jakarta.persistence.*;

@Entity
@Table(name = "Vendas_detalhe")
public class VendasDetalhe {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer idVendasDetalhe;

    @ManyToOne
    @JoinColumn(name = "idProcedimentos")
    private Procedimentos procedimento;

    private Integer idPagamento;

    @ManyToOne
    @JoinColumn(name = "idCliente")
    private Cliente cliente;

    @ManyToOne
    @JoinColumn(name = "idVendas")
    private Vendas venda;

    // Getters e setters...
}
                                                                                                                                                                                                    SCRIPT SQL COMPLETO (CREATE TABLEs)                                                                                                                     CREATE TABLE Cliente (
    idCliente INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(255),
    telefone INT,
    endereco VARCHAR(255),
    CPF INT,
    email VARCHAR(255)
);

CREATE TABLE Funcionario (
    idFuncionario INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(255),
    telefone INT,
    endereco VARCHAR(255),
    CPF INT,
    email VARCHAR(255),
    curriculo INT
);

CREATE TABLE Procedimentos (
    idProcedimentos INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(255),
    preco INT,
    descricao VARCHAR(255)
);

CREATE TABLE Avaliacoes (
    idAvaliacoes INT PRIMARY KEY AUTO_INCREMENT,
    idProcedimento INT,
    nota INT,
    data INT,
    idCliente INT,
    comentario VARCHAR(255),
    FOREIGN KEY (idProcedimento) REFERENCES Procedimentos(idProcedimentos),
    FOREIGN KEY (idCliente) REFERENCES Cliente(idCliente)
);

CREATE TABLE Agendamentos (
    idAgendamentos INT PRIMARY KEY AUTO_INCREMENT,
    idProcedimentos INT,
    idFuncionario INT,
    idCliente INT,
    data INT,
    horario INT,
    local VARCHAR(255),
    metodo_de_pagamento VARCHAR(255),
    FOREIGN KEY (idProcedimentos) REFERENCES Procedimentos(idProcedimentos),
    FOREIGN KEY (idFuncionario) REFERENCES Funcionario(idFuncionario),
    FOREIGN KEY (idCliente) REFERENCES Cliente(idCliente)
);

CREATE TABLE Vendas (
    idVendas INT PRIMARY KEY AUTO_INCREMENT,
    idProcedimentos INT,
    data INT,
    FOREIGN KEY (idProcedimentos) REFERENCES Procedimentos(idProcedimentos)
);

CREATE TABLE Vendas_detalhe (
    idVendasDetalhe INT PRIMARY KEY AUTO_INCREMENT,
    idProcedimentos INT,
    idPagamento INT,
    idCliente INT,
    idVendas INT,
    FOREIGN KEY (idProcedimentos) REFERENCES Procedimentos(idProcedimentos),
    FOREIGN KEY (idCliente) REFERENCES Cliente(idCliente),
    FOREIGN KEY (idVendas) REFERENCES Vendas(idVendas)
);
Repositórios Spring Data JPA                                                                                                                                                public interface ClienteRepository extends JpaRepository<Cliente, Integer> {}
public interface FuncionarioRepository extends JpaRepository<Funcionario, Integer> {}
public interface ProcedimentosRepository extends JpaRepository<Procedimentos, Integer> {}
public interface AvaliacoesRepository extends JpaRepository<Avaliacoes, Integer> {}
public interface AgendamentosRepository extends JpaRepository<Agendamentos, Integer> {}
public interface VendasRepository extends JpaRepository<Vendas, Integer> {}
public interface VendasDetalheRepository extends JpaRepository<VendasDetalhe, Integer> {}
                                                                                                                                                                                                     Endpoints REST (Controllers)                                                                                                                                                 ClienteController.java                                                                                                                                                                 @RestController
@RequestMapping("/clientes")
public class ClienteController {

    @Autowired
    private ClienteRepository repo;

    @GetMapping
    public List<Cliente> listar() {
        return repo.findAll();
    }

    @PostMapping
    public Cliente criar(@RequestBody Cliente c) {
        return repo.save(c);
    }

    @GetMapping("/{id}")
    public Cliente buscar(@PathVariable Integer id) {
        return repo.findById(id).orElse(null);
    }

    @PutMapping("/{id}")
    public Cliente atualizar(@PathVariable Integer id, @RequestBody Cliente c) {
        c.setIdCliente(id);
        return repo.save(c);
    }

    @DeleteMapping("/{id}")
    public void remover(@PathVariable Integer id) {
        repo.deleteById(id);
    }
}
                                                                                                                                                                                                FuncionarioController.java                                                                                                                                                              @RestController
@RequestMapping("/funcionarios")
public class FuncionarioController {

    @Autowired
    private FuncionarioRepository repo;

    @GetMapping
    public List<Funcionario> listar() {
        return repo.findAll();
    }

    @PostMapping
    public Funcionario criar(@RequestBody Funcionario f) {
        return repo.save(f);
    }

    @PutMapping("/{id}")
    public Funcionario atualizar(@PathVariable Integer id, @RequestBody Funcionario f) {
        f.setIdFuncionario(id);
        return repo.save(f);
    }

    @DeleteMapping("/{id}")
    public void remover(@PathVariable Integer id) {
        repo.deleteById(id);
    }
}
                                                                                                                                                                                                  ProcedimentosController.java                                                                                                                                              @RestController
@RequestMapping("/procedimentos")
public class ProcedimentosController {

    @Autowired
    private ProcedimentosRepository repo;

    @GetMapping
    public List<Procedimentos> listar() {
        return repo.findAll();
    }

    @PostMapping
    public Procedimentos criar(@RequestBody Procedimentos p) {
        return repo.save(p);
    }
}
