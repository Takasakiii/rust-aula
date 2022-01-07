# Aula 13 - Modules e Controles de Acesso

Rust organiza seu código em 3 elementos, os `packages` (corresponde ao projeto) `crates` (corresponde aos binários) e modules (corresponde a segmentos do código), até agora estávamos colocando nosso código no arquivo `main.rs` que representa o inicio de nossa `crate` (tudo que está publico no incio da crate fica disponível para acesso externo). Além das restrições impostas pelas `crates` você pode criar os modules para ter mais controle.

```rs
// pub - publico para todos os módulos
// pub(super) - publico somente para o módulo pai
// pub(crate) - publico apenas dentro da crate
// pub(self) - publico apenas para si mesmo e seus módulos internos
// pub(in `path`) - publico apenas para o modulo definido no `path`

mod aluno {
    // Tudo que queremos que fique disponível fora do modulo deve ser posto como publico
    pub struct Aluno {
        pub idade: u8, // Inclusive os campos das structs
        nome: String
    }

    // Sem poder acessar completamente os campos da struct temos que fornecer uma ferramenta publica, caso ainda queremos criar ela fora do modulo
    pub fn create_aluno(nome: String, idade: u8) -> Aluno {
        Aluno {
            idade,
            nome
        }
    }

    pub fn get_nome(aluno: &Aluno) -> &str {
        &aluno.nome
    }

}

fn main() {
    let aluno = aluno::create_aluno("Carlos".to_owned(), 19);
    println!("{}", aluno::get_nome(&aluno));

    let _nome = &aluno.nome; // Erro, pois não possuímos acesso ao nome
}
```

Além da versão bloco podemos definir módulos em outros arquivos

```rs
// aluno.rs
    pub struct Aluno {
        pub idade: u8,
        nome: String
    }

    pub fn create_aluno(nome: String, idade: u8) -> Aluno {
        Aluno {
            idade,
            nome
        }
    }

    pub fn get_nome(aluno: &Aluno) -> &str {
        &aluno.nome
    }

// main.rs
    mod aluno; // Nome do modulo será sempre o nome do arquivo rust

    fn main() {
        let aluno = aluno::create_aluno("Carlos".to_owned(), 19);
        println!("{}", aluno::get_nome(&aluno));
    }
```

> Quando usamos em um arquivo só podemos definir apenas um arquivo como dono do `mod`, os demais para acessar o module deve se referenciar a partir do modulo pai (aquele que contem a definição `mod`)

Caso o modulo cresça e tenha dentro dele vários submódulos pastas também podem ser usadas para organizar nossos módulos porem temos algumas regrinhas: - O nome da pasta será o nome do módulo - A pasta deve ter obrigatoriamente um arquivo chamado `mod.rs` que vai ter a parte principal do seu modulo.

> Sub-módulos também podem ter seu acesso restrito, caso o owner do module não coloque `pub` ou outros modificadores de acesso, o modulo ficará disponível apenas para o owner

```rs
mod aluno {
    mod caracteristicas {
        // Todo modulo aki
    }

    use caracteristicas::{Atributos, get_peso, get_altura}; // o `use` pode importar o conteúdo de um module para que não precisamos ficar usando ::
    // Caso ponha um `pub` em um use poderá prover para outros módulos acesso mais restritivo a itens do modulo em questão, permitindo que somente os itens do `use` seja exportado

    pub use caracteristicas::get_sexo;

    // Todo o modulo pai aqui
}

use aluno::get_sexo;

fn main() {
    aluno::caracteristicas::get_altura() // Por o modulo caracteristicas não esta publico esse acesso é invalido

    get_sexo() // Permitimos por meio do `pub use` acessar o get_sexo()
}
```
