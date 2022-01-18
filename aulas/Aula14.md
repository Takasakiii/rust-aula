# Aula 14 - Impl Blocks

Até aqui, vimos que caso queremos criar funções que manipulam uma struct, a melhor saída era usar um module para organizar, porém podemos, tanto nas structs quanto nos enums (veremos ainda no futuro) vincular funções, constantes e valores estáticos à struct através do `impl` block:

```rs
pub struct Aluno {
    nome: String,
    sexo: u8
}


// Tudo que estiver nesse bloco será vinculado a Aluno.
impl Aluno {
    pub fn new(nome: String, sexo: u8) -> Self { // Dentro dos impl blocks, podemos referenciar a struct que estamos vinculando como Self, mas caso queira, pode apenas,no lugar, usar o nome da struct Aluno.
        Self {
            nome,
            sexo
        }
    }

    pub fn sexo(&self) -> String { // O self no primeiro argumento referencia uma variável do tipo Aluno, que entrará como referência.
        match self.sexo {
            1 => String::from("Masculino"),
            _ => String::from("Feminino")
        }
    }
}

fn main() {
    let aluno = Aluno::new("Carlinhos".to_owned(), 1);
    println!("{}", aluno.sexo()); // O sexo() usa como self a variável aluno vinculada a ele, e também poderia ser chamado por Aluno::sexo(&aluno).
}
```

## Self vs self vs mut self vs &self vs &mut self

- `Self`: referencia ao tipo do `impl` block, sempre referenciando ao tipo o qual você está implementando;
- `self`: entrada de variável do tipo implementado pelo impl block, consumindo a mesma, caso não seja primitiva ou implemente `Copy` (ainda veremos no futuro);
- `mut self`: mesma condição de cima, porém com o modificador de mutabilidade;
- `&self`: referencia da variável `self`, fazendo assim com que self não seja consumido caso não seja copiável;
- `&mut self`: referencia mutável de `self`, podendo assim realizar modificações dentro de `self`.

> `impl` blocks só podem ser implementados junto da criação do tipo ou junto da criação da trait a ser implementado para extensão (veremos isso nas aulas de trait).
