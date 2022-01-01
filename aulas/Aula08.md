# Aula 8 - Funções

As funções assim como as outras linguagens servem para podermos dividir o código e organizar nosso código

Para criamos uma função a gente precisamos usar a palavra reservada `fn`, o nome da função, argumentos e por fim o retorno da função

```rs
// o tipo de cada argumento vai depois dos :
// e tipo de retorno da função vai depois da ->
fn soma(n1: u8, n2: u8) -> u8 {
    n1 + n2 // como disse anteriomente a ultima linha de um bloco sem ; é o retorno desse bloco, no caso se o bloco é uma função se torna o returno da função
}

// função sem tipo de retorno
// modo explicito
// () ou tambem conhecido como unit é o tipo padrão para retornos "vazios"
fn print(n1: u8) -> () {
    println!("Numero = {}", n1) // Como println! tambem possui retorno () o ; nesse caso se torna opcional
}

// modo inplicito
fn print(n1: u8) {
    println!("Numero = {}", n1); // so coloquei o ; para mostrar que realmente nesse caso não faz diferença
}

// Nossa função main
fn main() {
    // Chamando uma função
    let res = soma(1, 4); 
    println!("{}", res) // 5
}
```

Alem de parametros normais funções podem entrar e retornar referencias (no caso de retornar referencias tem certas regrinhas que devem serem seguidas para que não ocorra problemas de Lifetime)

```rs
// No caso usaremos a referencia mutavel para alterar um valor de uma variavel ja existente
fn sum_one(n: &mut u8) {
    *n += 1;
}

// você apenas pode retornar referencias que são extenos a sua função, como ja dito na Aula7 com os conseitos de Lifetime
fn retornando_referencia(n: &u8) -> &u8 {
    // todo processamento aki
    return n; // aqui so quis deixar claro que a palavra return tambem pode ser usada para definir o retorno de uma função assim como em outras langs
}

fn main() {
    let mut a = 2;
    sum_one(&mut a); // Perceba que para passar a referencia mutavel para a nossa função devemos usar junto o &mut
    println!("{}", a) // 
}

```
Tambem se usa muito as referencias para trabalhar com tipos complexos que veremos nas próximas aulas, ou para um ou mais retornos extras

## Funções dentro de funções e Closures

No rust é permitido definir uma função dentro de outra função já existente

```rs
fn main() {
    fn soma(i: i32, j: i32) -> i32 {
        i + j
    }

    let _my_sum = soma(1, 2); // variaveis com nomes começando com _ falam para o compilador que elas não terão uso
}
```
Mas infelizmente usar um atributo exteno para dentro da variavel não funciona, mas funciona em Closures que são funções não estaticas que dependem sua existencia a uma variavel.

```rs
let a = 3;
// no rust usamos || para iniciar uma closure
// para closure de uma linha não precisamos de {}
let add_to_a = |n: u8| a + n;
let _res = add_to_a(7); // res = 10


// Exemplo de closure multilinha
let _ = || {
    // processamento 
}

```

## Tipo Never (!)
Algumas funções pode nunca retornar para a função que a chamou, essas funções geralmente tem o tipo de retorno never `!`

Exemplo abaixo:
```rs
fn loop_infinito() -> ! {
   // essa função nunca irá retornar por causa do loop
   loop {}
}
```

