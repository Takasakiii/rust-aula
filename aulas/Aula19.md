# Aula 19 - Traits

Não vou mentir estava evitando esse assunto (mas agora que chegamos nele vamos entrar em um novo mundo de programação genérica), `trait` é uma das bases fundamentais do rust em termos de construção da linguagem, até agora vimos a rigidez do rust e os tipos de dados até parece que reaproveitamento de código é um tabu, que usar um mesmo método ou mesma função em tipos semelhantes é um absurdo..... Mas não.... Rust tem muita flexibilidade a primeira ferramenta que irá trazer isso são as `traits`.

## Mas que são `traits`?

Bem se você ja programou em linguagens orientada a objetos uma `trait` vai te familiarizar muito com uma `interface` que você define um contrato (serie de métodos que as classes que implementarem tem que seguir em prol de uma generalização). As `traits` tem exatamente esse mesmo proposito.

## Criando uma trait

Criar uma `trait` é muito simples, bastando usar a palavra reservada `trait` e criar seus métodos

```rs
trait AcoesHumanas {
    fn comer(&mut self, alimento: Alimento) -> bool; //caso algum tipo implemente essa trait o método comer precisa ser implementado
    fn mexer_a_boca(&self, velocidade: u8);

    // Podemos também definir comportamentos padrão para nossa trait que pode ser especializados sendo sobrescritos na implementação
    fn falar(&self, palavras: &str) {
        self.mexer_a_boca(200); // So se tem conhecimento de self.mexer_a_boca porque ele participa da trait, não da para usar métodos não conhecidos
        println!("Ele falou {}", palavras)
    }
}
```

## Implementando uma `trait`

Implementar uma trait não é complexo, bastando apenas usar o `impl block`.

Vamos implementar a nossa trait que usamos como exemplo a `AcoesHumanas`

```rs
struct Homem {
    fome: u32,
}

// Usamos o for para definir onde estamos implementando
impl AcoesHumanas for Homem {
    // Os métodos devem ter a mesma assinatura da trait
    fn comer(&mut self, alimento: Alimento) -> bool {
        if (alimento.calorias > 100) {
            self.fome += alimento.calorias;
            true
        } else {
            false
        }
    }

    // Devemos implementar todos os métodos abstratos

    fn mexer_a_boca(&self, velocidade: u8) {
        // ....
    }

    // No caso do homem não quero especializar o método falar, então não irei reimplementa-lo
}

struct Mulher {
    fome: u32,
    determinacao: u32
}

impl AcoesHumanas for Mulher {
    // ... Imaginem o resto da implementação aqui

    // Já aqui para mulheres quero dar override no método falar
    fn falar(&self, palavras: &str) {
        self.mexer_a_boca(100);
        println!("Ela falou {} com {} de determinação", palavras, self.determinacao)
    }
}
```

## Traits como Params

Como temos agora um tipo genérico podemos usa-lo no lugar de tipos concretos usando a keyword `impl`

```rs
fn acoes_diarias(pessoa: &mut impl AcoesHumanas) {
    pessoa.comer(Pera::new());
    pessoa.falar("Ola")
}


fn main() {
    let mut mulher = Mulher {
        fome: 30,
        determinacao: 20
    };

    let mut homem = Homem {
        fome: 100
    };

    acoes_diarias(&mut homem);
    acoes_diarias(&mut mulher);
}
```

## Extensions

Podemos ainda adicionar com as traits comportamentos a tipos que não nos pertence (estendendo o tipo), por exemplo podemos criar um método que adiciona 3 a qualquer numero do tipo `i32`

```rs
trait AdicionaTres {
    fn adiciona_tres(&self) -> i32;
}

impl AdicionaTres for i32 {
    fn adiciona_tres(&self) -> i32 {
        self + 3
    }
}

fn main() {
    println!("{}", 6.adiciona_tres());
}
```

> Lembre-se so podemos implementar as `traits` nos mesmo módulos que criamos elas ou que criamos nosso tipo
