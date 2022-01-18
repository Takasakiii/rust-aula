# Aula 17 - Panic, Result e Option

Até agora quando mechemos com `options` e `results` usávamos o `.unwrap()` para pegar o conteúdo de dentro delas o que as vezes resultava no console um `panic`

`panic` é o resultado quando algo muito inesperada ocorreu e o rust teve que abortar o processamento daquela thread (como ainda estamos com código simples temos apenas uma única thread), quando o programa entra em modo `panic` nada mais pode ser recuperado apenas uma nova execução do programa ou `spawn` (é como chamamos o inicializador de threads) da nova thread.

Nós também podemos colocar nos nossos códigos pontos de condição inesperadas usando alguns macros, o clássico `panic!` que apenas emite um `panic` genérico, temos também o `todo!` que assinala um ponto em construção no código (ele além de gerar panic quando chegar nele, ele também pode ignorar o check tipo de retorno de funções, podendo ser muito util para montar esqueletos das nossas funções), também temos o `unimplemented!` que assina que uma função não foi implementada (ele funciona como o `todo!` porem com uma resposta no console diferente) e por fim o `unreachable!` que serve para indicar que a execução nunca deveria chegar naquele ponto (usa-se para pontos que você tem certeza que nunca vai chegar).

Todos os tipos de `panic` podem receber uma string (com placeholders) para definir uma mensagem de motivo do panic.

```rs
fn sort(arr: &mut [i8; 5]) {
    unimplemented!("Sort ainda não foi implementado.")
}

sort([1, 4, -1, 10, 14]); // Ao chamar a função um panic ira ser emitido e o programa encerrado
```

Mas também queremos poder ter errors que possam ser tratados sem matar nossa `thread` para isso o rust tem um `enum` especial chamado `Result` que encapsula dois tipos genéricos (um para o dado a ser retornado e outro para o erro).

O `Result` pode ser manipulado tanto como um simples `enum` tanto com os métodos exclusivos dele

```rs
enum AgeError {
    IdadeMenorQue0,
    IdadeMaior200Anos
}

// Voce pode usar qualquer tipo para representar um Erro
fn int_to_valid_age(number: i32) -> Result<i32, AgeError> {
    if number <= 0 {
        return Error(AgeError::IdadeMenorQue0);
    }

    if number > 200 {
        return Error(AgeError::IdadeMaior200Anos);
    }

    Ok(idade)
}

fn main() {
    let idade = 12;

    // O jeito de tratativa de erro mais comum é usando match com pattern match
    match int_to_valid_age {
        Ok(idade) => println!("idade = {}", idade),
        Err(AgeError::IdadeMenorQue0) => println!("Idade invalida, menor que 0"),
        Err(AgeError::IdadeMaior200Anos) => println!("Idade Maior que 200")
    }
}

```

Se quisermos representar apenas a presença e ausência de um valor podemos usar o enum `Option`, seus estados podem ser `None` e `Some` que representa a ausência ou presença do valor (quase como um tipo Nullable)

```rs
struct Person {
    nome: String,
    idade: u8,
    sexo: Option<String>
}

let carla = Person {
    nome: "Carla".to_owned(),
    idade: 20,
    sexo: None
};

let james = Person {
    nome: "James".to_owned(),
    idade: 34,
    sexo: Some("Masculino".to_owned())
}
```

Podemos ainda usando os metodos especiais do `Result` e `Option` transformar-se entre si

```rs
// result to option
let my_value = function().ok() // ok transforma uma Result numa Option


// option to result
let my_value = function2().ok_or("O elemento veio None".to_owned()); // Para a conversão inversa é necessário passar o valor do erro caso o elemento seja None
```

Também podemos manipular os valores do `Result` e `Option` sem desempacota-los com o `map`

```rs
let dois = Some(2);
let quatro = dois.map(|e| e + 2); // mudamos o valor interno da some para e + 2 que nesse caso seria 4, caso fosse None essa função apenas seria ignorada
```

No rust em funções cuja o retorno seja `Option` ou `Result` podemos usar o operador `?` para encaminhar um erro para a função que a utilizar facilmente

```rs
enum AgeError {
    IdadeMenorQue0,
    IdadeMaior200Anos
}

// Voce pode usar qualquer tipo para representar um Erro
fn int_to_valid_age(number: i32) -> Result<i32, AgeError> {
    if number <= 0 {
        return Error(AgeError::IdadeMenorQue0);
    }

    if number > 200 {
        return Error(AgeError::IdadeMaior200Anos);
    }

    Ok(idade)
}

struct Person {
    name: String,
    age: i32
}

impl Person {
    fn new(name: String, age: i32) -> Result<Self, AgeError> {
        let age = int_to_valid_age(age)?; // Caso o result der Error ele interrompera a função e encaminha-ra o erro para a função pai

        Self {
            name,
            age
        }
    }
}
```

Juntamente a isso podemos ainda usar o `map_err` que auxilia a conversão de um tipo de erro para outro

```rs
#[derive(Debug)] //Iremos abordar derive no futuro, mas pense que adicionamos com isso as funções responsáveis pelo placeholder {:?}
enum AgeError {
    IdadeMenorQue0,
    IdadeMaior200Anos
}

enum PersonError {
    InvalidPerson(String)
    // ...
}

//...

impl Person {
    fn new(name: String, age: i32) -> Result<Self, PersonError> {
        let age = int_to_valid_age(age)
            .map_err(|err| format!("Age Error: {:?}", err))?; // Com isso transformamos AgeError em PersonError e usamos o ? para ja encaminhar os erros

        Self {
            name,
            age
        }
    }
}
```
