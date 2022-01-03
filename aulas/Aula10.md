# Aula 10 - Strings e Stdin

O Rust quando queremos armazenar textos assim como as demais linguagens de programação usamos Strings, o Rust possui dois tipos para representar uma String o `str` e a `String`. Pode parecer confuso no incio mas o rust tem seus motivos.

## `str`
O `str` é a versão raw da string, ou seja sua representação mais basica, possui um tamanho fixo e é imutavel por naturesa, como seu valor nunca é conhecido em tempo de compilação seu valor sempre está alocado fora da stack então sempre será vista como uma referencia (referencia que aponta para a real posição na memoria onde está a string).

Toda string que colocamos de forma literal em nosso código se tornam `str` com detalhe que seu lifetime é definido como `'static` (lifetime `'static` é uma classe especial de lifetime que é dado a todas as referencias que sua existencia é garantida a qualquer momento, ou seja ela não desaparecem de acordo com o contexto, e no caso de uma string literal, colocada diretamente no código ela sempre existirá dado que ela é carregada na memoria como parte fundamental do programa junto do binario).

```rs
let _a = "Minha string literal";
// Usando o r# o rust permite entrar com um texto raw e ele fará a conversão para uma string
let _b = r#"Minha
String
Literal
"#;
```

## `String`
`String` é o tipo gerenciável de uma String, sendo um `smart pointer` (que iremos ver mais futuramente em uma aula dedicada a eles), smart pointer esse que aponta para uma área onde ele gerencia uma `str`.

Como internamente uma `String` é composta por uma `str`, que facilmente você pode transformar `String` em `str` bastando apenas um cast simples com `as` usando uma referência de uma `String` ou passando a referência para um lugar que explicitamente precise de uma `str`:

```rs
// Cria uma String vazia:
let _string = String::new();

// Também podemos usar o método de conversão simples para obter nossa String:
let _string = String::from("Olá Amigo");

// Usando o mesmo conversor só que chamado a partir da str:
let _string: String = "Abacaxi".into();

// Usando o clone (função de clonagem de tipos complexos (Mais sobre ela no futuro)):
let _string = "Banana".clone();

// Também podemos usar o método de transformação de tipos para String:
let _string = "Pêra".to_string();

// E, por fim, a função de transmutação para tipos gerenciados:
let _string = "Uva".to_owned();
```
> Na comunidade Rust, as formas mais apoiadas de transmutar `str`em `String` são a `from`, `into`, `to_string` e `to_owned`.

## Concatenando `String`s
Assim como na maioria das langs, o Rust usa o `+` para chamar o concatenador de Strings:

```rs
let _a = "Meu nome é ".to_owned() + "Pedro";
```
> Para concatenar Strings, o lado primário da concatenação precisa ser do tipo `String`:

## `format!`
Quando precisamos manipular Strings de formas mais complexas, podemos usar o macro `format!`, que permite usar os mesmos placeholders do macro `println!`:

```rs
let nome = "Pedro";
let idade = 18;
let sexo = "Masculino";
let _formatted_string = format!("Meu nome é {}, tenho {} anos e meu sexo é {}.", nome, idade, sexo);
```

## Substring
Para fazer uma substring, pode facilmente usar o `[]` com um range para delimitar o tamanho da substring:

```rs
let _slice = &"Ola como vai você"[1..5];
```

> Ao fazer uma substring com `[]` ele gerará uma `str` e como previamente falado uma `str` não pode viver na stack então precisamos fazer uma referência para podermos utilizá-la.

## Get String Chars
Para obtermos os `char`s de uma `String` ou de uma `str`, usamos o método `chars`, que retornará um iterator da string.

```rs
let nome = "João";
// O .chars() retorna um iterador dos caracteres, e o .enumerate() retorna um novo iterador que retorna tanto cada item do iterador quanto o index, que começa em 0 e aumenta em 1 a cada item.
for (index, letra) in nome.chars().enumerate() {
    println!("Letra n° {}: {}", index, letra);
}
```

## Parse
Caso queira transformar uma `String` em um tipo básico podemos usar a função `parse`. O tipo resultante deve ser passado por um turbofish `::<>`, e retornará o resultado em um `Result` (que veremos mais sobre em uma aula futura), que, por enquanto, será usado o `unwrap` para desempacotar à força o retorno.

```rs
let three_in_str = "3";
let _three_in_u8 = three_in_str.parse::<u8>().unwrap();
```
> Se a operação falhar por estarmos usando à força (com o `unwrap`) para obter o resultado, isso gerará um `panic` que é o crash do Rust.

## Stdin
Para obtermos uma interação com o console, precisamos usar a entrada padrão: o `stdin`, que permitirá pausar o console e realizar uma leitura.
```rs
// Criei uma função para facilitar a manipulação do stdin para leitura de linhas:
fn scanln() -> String {
    let mut buffer = String::new();
    io::stdin().read_line(&mut buffer).unwrap(); // Unwrap aqui de novo, pois essa função pode retornar erros.
    buffer
}

fn main() {
    println!("Digite seu nome:")
    let _nome = scanln();
}
```
