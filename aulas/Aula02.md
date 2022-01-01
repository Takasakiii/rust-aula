# Aula 2 - Inciando

Para começar um projeto no rust você deve usar o comando `cargo new` 
(nome_do_projeto (obs: O nome do projeto deve seguir o padrão snake_case
ou seja invés de espaços se usa o `_`))'


O cargo gerará a arvore do nosso projeto:

  
```
(nome_projeto)
  |- src (a pasta que vai conter todos nosso código)
  |   |- main.rs (arquivo principal do projeto,
  |      todo projeto precisa ter um main.rs)
  |- Cargo.toml (Arquivo com as configurações do projeto e suas  
  |  dependências)
  |- Cargo.lock (Arquivo de lock de versões da arvore de dependências)
  |- target (Pasta que pode aparecer que contem as dependências e dados  
     da compilação do programa)
```

Para compilarmos e executar o projeto usamos o `cargo run` que deve ja mostrar nosso `hello world`

Antes de começarmos a mexer com o código é importante ter em mente os tipos das variáveis básicas do Rust, ainda mais que no Rust a tipagem é fundamental e muito importante, são eles:

```
bool - true e false

i8 - numero inteiro de 8 bits
i16 - numero inteiro de 16 bits
i32 - numero inteiro de 32 bits
i64 - numero inteiro de 64 bits
i128 - numero inteiro de 128 bits

eles tambem tem sua versao sem numeros negativos, assim tendo o bit do sinal como bit numerico: u8, u16, u32, u64 e u128

temos tambem um tipo inteiro que depende da arquitetura que seria o usize e isize, que seu tamanho pode variar de 32-64 bits dependendo da CPU e o SO


alem dos numeros inteiros temos tambem os de ponto flutuante
f32 e f64 sendo sua principal diferença a precisão e o tamanho de bits (32/64)

e por fim o tipo char que representa 1 caractere (no caso do rust 1 caractere utf-8)
``