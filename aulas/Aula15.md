# Aula 15 - Const, Static e Type

Nessa aula focaremos em artigos menores da linguagem rust, mas que ainda sim são muito importantes para nosso dia a dia. Para começarmos a keyword `type` com ela é possível criar apelidos a tipos já existentes

```rs
type Angulo = i32; // basta isso e o apelido para aquele tipo esta criado, são uteis principalmente para facilitar a codificação e a legibilidade do código
type Posicao = (i32, i32, i32);
type Jogador = (Angulo, Posicao);

fn create_jogador(posicao: Posicao) -> Jogador {
    // .... toda logica aki
    (posicao, (x, y, z))
}
```

Podemos criar apelidos para valores com a keyword `const`, elas funcionam como uma variável para o compilador dar replace nos locais usados em tempo de compilação.

```rs
const ARRAYS_SIZE: usize = 20; // nas const precisamos obrigatoriamente fornecer explicitamente o tipo

fn main() {
    let notas = [0; ARRAYS_SIZE];
    // todo seu código aki
}

```

Também podemos rodar pequenas funções no momento da compilação para definir nossos valores constantes

```rs
// observe que a palavra const foi usada na função para indicar ao compilador que é uma função que funciona em tempo de compilação
const fn criar_default_arr() -> &'static [u8] {
    &[1, 2, 3, 4, 5]
}

const ARR: &[u8] = criar_default_arr();
```

> Funções constantes so podem invocar outras funções constantes e elas não podem ser compridas ou complexas, apenas deve servir de ferramenta rápida para ajudar criar suas constantes

Algumas aulas atrás mencionamos que além dos lifetimes `'a`, `'b`, `'c`, etc temos ainda o lifetime `'static` que apenas serve para indicar que a referencia deve tender pelo menos até o final do programa. Com isso algumas coisas podem garantir uma referencia estática, variáveis criado constante, variáveis owned (ou seja que não são referencias), variáveis static e vazamentos de memória (que podem ser forçados) e `unsafe`.

Falando em variáveis criadas `static` elas são quase como constantes porem são criadas quando nosso programa inicia ou chega na função onde ela foi definida.

```rs
struct MyDataStruct {
    // atributos
}

// repare assim como a const devemos usar o tipo explicito
static DATA: MyDataStruct = MyDataStruct {
    // atributos
};
```

Podemos ainda usar as `const fn` para definir tipos em nas `statics` e podemos também definir a static como mutável porem isso geraria problemas com o lifetime, borrow, etc por conta disso `static mut` são `unsafe` (por enquanto são técnicas não recomendadas a menos que domine o que está fazendo, falaremos mais de `unsafe` no futuro)
