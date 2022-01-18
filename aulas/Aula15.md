# Aula 15 - Const, Static e Type

Nessa aula focaremos em artigos menores da linguagem Rust, mas que ainda são muito importantes para nosso dia a dia. Para começarmos, a keyword `type`. Com ela, é possível criar apelidos a tipos já existentes:

```rs
type Angulo = i32; // Basta isso e o apelido para aquele tipo está criado, são úteis principalmente para facilitar a codificação e a legibilidade do código.
type Posicao = (i32, i32, i32);
type Jogador = (Angulo, Posicao);

fn create_jogador(posicao: Posicao) -> Jogador {
    // ...toda a lógica aqui.
    (posicao, (x, y, z))
}
```

Podemos criar apelidos para valores com a keyword `const`, pois elas funcionam como uma variável para o compilador dar replace nos locais usados em tempo de compilação:

```rs
const ARRAYS_SIZE: usize = 20; // Nas const precisamos obrigatoriamente fornecer explicitamente o tipo.

fn main() {
    let notas = [0; ARRAYS_SIZE];
    // Todo seu código aqui.
}

```

Também podemos rodar pequenas funções no momento da compilação para definir nossos valores constantes:

```rs
// Observe que a palavra const foi usada na função para indicar ao compilador que é uma função que funciona em tempo de compilação.
const fn criar_default_arr() -> &'static [u8] {
    &[1, 2, 3, 4, 5]
}

const ARR: &[u8] = criar_default_arr();
```

> Funções constantes só podem invocar outras funções constantes, e elas não podem ser compridas ou complexas, apenas devem servir de ferramenta rápida para ajudar a criar suas constantes.

Há algumas aulas atrás, mencionamos que além dos lifetimes `'a`, `'b`, `'c`, etc, temos ainda o lifetime `'static`, que apenas serve para indicar que a referência deve existir pelo ao menos até o final do programa. Com isso, algumas coisas podem garantir uma referência estática: variáveis constantes, variáveis owned (ou seja, que não são referências), variáveis static, vazamentos de memória (que podem ser forçados) e `unsafe`.

Falando em variáveis criadas `static`, elas são quase como constantes, porém são criadas quando nosso programa inicia ou quando se chega na função onde ela foi definida.

```rs
struct MyDataStruct {
    // Atributos.
}

// Repare que assim como a const, devemos usar o tipo explícito.
static DATA: MyDataStruct = MyDataStruct {
    // Atributos.
};
```

Podemos ainda usar as `const fn` para definir tipos nas `static`s, também podemos definir a `static` como mutável, porém isso geraria problemas com o lifetime, borrow, etc, por conta disso ,`static mut` são `unsafe` (por enquanto são técnicas não recomendadas, a menos que domine o que está fazendo, falaremos mais de `unsafe` no futuro).
