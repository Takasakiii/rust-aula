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
`String` é o tipo gerenciavel de uma String, sendo um `smart pointer` (Que iremos ver mais futuramente em uma aula dedicada a eles), esse smart pointer aponta para uma área onde ele gerencia uma `str`.

Como internamete uma `String` é composta por uma `str` facilmente você pode transformar `String` em `str` bastando apenas um cast simples com `as` usando uma referencia de uma `String` ou passando a referencia para um lugar que explicitamente precise de uma `str`

```rs
// Cria uma String vazia
let _string = String::new();

// Tambem podemos usar o metodo de conversão simples para obter nossa String
let _string = String::from("Ola Amigo");

//Usando o mesmo conversor so que chamado a partir da str
let _string: String = "Abacaxi".into();

// Usando o clone (função de clonagem de tipos complexos (Mais sobre ela no futuro)) 
let _string = "Banana".clone();

//Tambem podemos usar o metodo de transformação de tipos para string
let _string = "Pera".to_string();

// E por fim a função de trasmutação para tipos gerenciados
let _string = "Uva".to_owned();
```
> Na comunidade rust as formas mais apoiadas de transmutar `str`em `String` são a `from`, `into`, `to_string` e `to_owned`

## Concatenando `String`s
Assim como na maioria das langs o Rust usa o `+` para chamar o concatenador de Strings
```rs
let _a = "Meu nome é ".to_owned() + "Pedro";
```
> Para concatenar Strings o lado primario da concatenação precisa ser do tipo `String`

## `format!`
Quando precisamos manipular strings de formas mais complexas podemos usar o macro `format!` que permite usar os mesmos placeholders do macro `println!`
```rs
let nome = "Pedro";
let idade = 18;
let sexo = "Masculino";
let _formated_string = format!("Meu nome é {}, tenho {} anos, meu sexo ;é {}.", nome, idade, sexo);
```

## Substring
Para fazer uma substring, pode facilmente usar o `[]` com um range para delimitar o tamanho da substring
```rs
let _slice = &"Ola como vai você"[1..5];
```
> Ao fazer uma substring com `[]` ele gerará uma `str` e como previamente falado uma `str` não pode viver na stack então precisamos fazer uma referencia para podermos utiliza-lá

## String get chars
Para obter os `char`s de uma `String` ou `str` usamos o método `chars` que retornará um iterator da string

## Parse
Caso queira transformar uma `string` em um tipo básico podemos usar a função `parse` o tipo resultante deve ser passado por Turbofish `::<>`, ele retornará o resultado em um `Result` que por enquanto usaremos o `unwrap` para desempacotar a força o retorno.
```rs
let three_in_str = "3";
let _three_in_u8 = three_in_str.parse::<u8>().unwrap();
```
> Se a operação falhar por estarmos usando a força (com o `unwrap`) para obter o resultado isso gerará um `panic` que é o crash do Rust

## Stdin
Para obtermos uma interação com o console precisamos usar a entrada padrão o `stdin` que permitirá pausar o console e realizar uma leitura.
```rs
// Criei uma função para facilitar a manipulação do stdin
fn scanln() -> String {
    let mut buffer = String::new();
    io::stdin().read_line(&mut buffer).unwrap(); // unwrap aki de novo pois essa função pode retornar errors
    buffer
}

fn main() {
    println!("Digite seu nome:")
    let _nome = scanln();
}
```