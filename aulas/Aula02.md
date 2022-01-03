# Aula 2 - Inciando

Para começar um projeto no Rust você deve usar o comando `cargo new <nome_do_projeto>` (obs: o nome do projeto deve seguir o padrão snake_case,
ou seja, ao invés de espaços se usa o `_`).


O Cargo gerará a árvore do nosso projeto:

  
```
(nome_do_projeto)
  |- src (a pasta que vai conter todo o nosso código)
  |   |- main.rs (arquivo principal do projeto,
  |      todo projeto precisa ter um main.rs)
  |- Cargo.toml (arquivo com as configurações do projeto e suas  
  |  dependências)
  |- Cargo.lock (arquivo de lock de versões da arvore de dependências)
  |- target (pasta que pode aparecer que contem as dependências e dados  
     da compilação do programa)
```

Para compilarmos e executarmos o projeto usamos o `cargo run` que deve já mostrar nosso `Hello World!`.

Antes de começarmos a mexer com o código é importante ter em mente os tipos básicos das variáveis do Rust, ainda mais que no Rust a tipagem é fundamental e muito importante, são eles:

```
bool - true e false

i8 - número inteiro de 8 bits
i16 - número inteiro de 16 bits
i32 - número inteiro de 32 bits
i64 - número inteiro de 64 bits
i128 - número inteiro de 128 bits

Eles também tem sua versão sem números negativos, assim tendo o bit do sinal como bit numérico: u8, u16, u32, u64 e u128.

Temos também um tipo inteiro que depende da arquitetura, que seria o usize e isize, que seu tamanho pode variar de 32-64 bits dependendo da CPU e do SO.


Além dos números inteiros temos também os números de ponto flutuante:
f32 e f64, sendo sua principal diferença a precisão e o tamanho de bits (32/64)

E, por fim, o tipo char, que representa 1 caractere (no caso do Rust, 1 caractere UTF-8)
```
