# Aula 8 - Funções

As funções, assim como nas outras linguagens, servem para podermos dividir o código e o organizar.

Para criamos uma função, precisamos usar a palavra reservada `fn`, o nome da função, argumentos e por fim o retorno da função:

```rs
// O tipo de cada argumento vai depois do :
// e o tipo de retorno da função vai depois da ->
fn soma(n1: u8, n2: u8) -> u8 {
    n1 + n2 // Como disse anteriormente, a última linha de um bloco sem ; é o retorno desse bloco, no caso, se o bloco é uma função, se torna o retorno da função.
}

// Função sem tipo de retorno.
// Modo explícito.
// (), também conhecido como Unit, é o tipo padrão para retornos "vazios".
fn print(n1: u8) -> () {
    println!("Numero = {}", n1) // Como println! também possui retorno (), o ; nesse caso se torna opcional.
}

// Modo implícito.
fn print(n1: u8) {
    println!("Numero = {}", n1); // Só coloquei o ; para mostrar que realmente nesse caso não faz diferença.
}

// Nossa função main.
fn main() {
    // Chamando uma função.
    let res = soma(1, 4); 
    println!("{}", res) // 5
}
```

Além de parâmetros normais, funções podem entrar e retornar referências (no caso de retornar referências, tem certas regrinhas que devem serem seguidas para que não ocorra problemas de lifetime).

```rs
// No caso usaremos a referencia mutável para alterar um valor de uma variável ja existente.
fn sum_one(n: &mut u8) {
    *n += 1;
}

// Você pode retornar apenas referências que são externas a sua função, como já dito na Aula 7 com os conceitos de lifetime.
fn retornando_referencia(n: &u8) -> &u8 {
    // Todo o processamento aqui.
    return n; // Aqui só quis deixar claro que a palavra return também pode ser usada para definir o retorno de uma função, assim como em outras langs.
}

fn main() {
    let mut a = 2;
    sum_one(&mut a); // Perceba que, para passar a referencia mutável para a nossa função, devemos usar junto o &mut.
    println!("{}", a) // 
}

```
Também se usa muito as referências para trabalhar com tipos complexos. que veremos nas próximas aulas, ou para um ou mais retornos extras.

## Funções dentro de funções e Closures.

No Rust é permitido definir uma função dentro de outra função já existente.

```rs
fn main() {
    fn soma(i: i32, j: i32) -> i32 {
        i + j
    }

    let _my_sum = soma(1, 2); // Variáveis com nomes começando com _ falam para o compilador que elas não terão uso.
}
```
Mas, infelizmente, usar um atributo externo para dentro da variável não funciona, mas funciona em Closures que são funções não estáticas que dependem sua existência a uma variável.

```rs
let a = 3;
// No Rust usamos || para iniciar uma closure.
// Para closure de uma linha não precisamos de {}:
let add_to_a = |n: u8| a + n;
let _res = add_to_a(7); // res = 10


// Exemplo de closure multilinha:
let _ = || {
    // Processamento.
}
```

## Tipo Never (!)
Algumas funções podem nunca retornar para a função que a chamou. Essas funções geralmente tem o tipo de retorno never `!`.

Exemplo abaixo:
```rs
fn loop_infinito() -> ! {
   // Essa função nunca irá retornar por causa do loop.
   loop {}
}
```
