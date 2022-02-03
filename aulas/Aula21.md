# Aula 21 - Generics

Quando entramos em traits dissemos que começaríamos flexibilizar nossas funções e tipos em modelos mais genéricos, vimos que traits pode nos dar flexibilidade porem ainda não flexibilidade total.

Imaginamos que temos um enum que em seu interior vai ter os "estados de materia" (solido, liquido e gasoso) e alguns tipos que represente certos materiais

```rs
struct Ferro;
struct Ouro;
struct Cobre;
struct Oxigenio;


enum EstadoMaterial {
    Solido,
    Liquido,
    Gasoso
}
```

Vamos dizer que queremos que dentro desse enum podemos colocar o material assim combinando o material com seu estado, mas como faríamos isso? Traits?

Até poderíamos usar traits porem teríamos que implementar em cada elemento essa trait e teríamos que mexer em situações não Sized que envolveria ponteiros e tals....

É tem que ter um jeito mais simples, e tem, `generics`, então vamos usar generics para adicionar ao enum um "placeholder" para um tipo futuro e então o compilador quando for gerar nosso binário vai ver os tipos usados e vai criar um enum correspondente a cada tipo implementado. Mas isso não é caro??? em um ponto gasta um pouco a mais em tamanho de binário, mas em comparação com o gasto de RAM para o método usando traits se torna bem mais vantajoso usar generics na maioria esmagadora dos casos.

Então vamos implementar esse tal de `generics` em nosso `enum`:

```rs
struct Ferro;
struct Ouro;
struct Cobre;
struct Oxigenio;


enum EstadoMaterial<T> {
    Solido(T),
    Liquido(T),
    Gasoso(T)
}
```

Usamos o `<>` para informar que aquele tipo vai receber um tipo genérico que vai ser representado por um nome, geralmente usa-se a letra `T` como primeiro `generic` type, e por fim dentro do enum é colocado `T` como substituto do tipo concreto.

Ta mais como usaria a estrutura EstadoMaterial com o tipo genérico?

```rs
fn main() {
    // forma implícita
    let _ferro_solido = EstadoMaterial::Solido(Ferro);
    // forma explicita
    let _ouro_liquido = EstadoMaterial::Liquido::<Ouro>(Ouro);
}

```

Perceba que na forma explicita a gente usa o turbo-fish (`::<>`) para definir qual tipo genérico será usado.

Bem mas imaginamos que queremos fazer o display (em outras linguagens pode se conhecer como "fazer o ToString") de nossos tipos, para os tipos simples (Ouro, Ferro, Cobre e Oxigenio) basta simplesmente implementar a trait `std::fmt::Display`

```rs
use std::fmt;

impl fmt::Display for Ferro {
    // <'_> passa ao fmt::Formatter qual lifetime correspondente, no caso '_ é o lifetime herdado da função fmt
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        write!(f, "Ferro") // o write! é o macro responsável por construir a visualização de um item.
    }
}

impl fmt::Display for Ouro {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        write!(f, "Ouro")
    }
}

impl fmt::Display for Cobre {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        write!(f, "Cobre")
    }
}

impl fmt::Display for Oxigenio {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        write!(f, "Oxigênio")
    }
}
```

Para os tipos simples sem generics é fácil mas e para o EstadoMaterial???
Como podemos implementar uma trait de display genérica?

Bem no caso seguindo a lógica colocaríamos:

```rs

impl fmt::Display for EstadoMaterial<T> {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        match self {
            EstadoMaterial::Solido(e) => write!(f, "Solido de {}", e),
            EstadoMaterial::Liquido(e) => write!(f, "Liquido de {}", e),
            EstadoMaterial::Gasoso(e) => write!(f, "Gasoso de {}", e),
        }
    }
}
```

Infelizmente isso não funcionaria, pois o `impl` precisa ter noção de que tipos estamos usando.

```rs
impl<Ferro> fmt::Display for EstadoMaterial<Ferro> {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        match self {
            EstadoMaterial::Solido(_) => write!(f, "Solido de ferro"),
            EstadoMaterial::Liquido(_) => write!(f, "Liquido de ferro"),
            EstadoMaterial::Gasoso(_) => write!(f, "Gasoso de ferro"),
        }
    }
}
```

Bem é uma solução porem teríamos que implementar para cada subtipo que EstadoMaterial aceita, e isso tira um pouco da beleza de códigos genéricos

```rs
impl<T> fmt::Display for EstadoMaterial<T> {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        match self {
            EstadoMaterial::Solido(e) => write!(f, "Solido de {}", e),
            EstadoMaterial::Liquido(e) => write!(f, "Liquido de {}", e),
            EstadoMaterial::Gasoso(e) => write!(f, "Gasoso de {}", e),
        }
    }
}
```

Bem agora temos outro problema que é T é muito genérico e nem todos os tipos implementam `Display` para que possamos usar placeholders, como poderíamos limitar essa implementação apenas para os tipos que implementam `Display`

Podemos usar o `where` para limitar um `generic`, o `where` pode ser visto de duas forma, a forma longa:

```rs
impl<T> fmt::Display for EstadoMaterial<T>
where
    T: fmt::Display
{
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        match self {
            EstadoMaterial::Solido(e) => write!(f, "Solido de {}", e),
            EstadoMaterial::Liquido(e) => write!(f, "Liquido de {}", e),
            EstadoMaterial::Gasoso(e) => write!(f, "Gasoso de {}", e),
        }
    }
}
```

Ou também podemos usar a forma resumida com `:`

```rs
impl<T: fmt::Display> fmt::Display for EstadoMaterial<T> {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        match self {
            EstadoMaterial::Solido(e) => write!(f, "Solido de {}", e),
            EstadoMaterial::Liquido(e) => write!(f, "Liquido de {}", e),
            EstadoMaterial::Gasoso(e) => write!(f, "Gasoso de {}", e),
        }
    }
}
```

Com o `where` o display só ficará disponível caso o `T` possua implementado `Display`.

E por fim vamos testar nosso display:

```rs
fn main() {
    let a = EstadoMaterial::Solido(Ferro);
    let b = EstadoMaterial::Gasoso(Oxigenio);
    let c = EstadoMaterial::Liquido(Ouro);

    println!("{}\n{}\n{}", a, b, c)
}
```

Além de types podemos usar generics diretamente nas funções:

```rs
fn print_array<T: fmt::Display>(input: &[T]) {
    for item in input {
        println!("{} ", item);
    }
}
```

> Observação no caso de T em arrays o tipo e seus `generics` devem ser o mesmo

Caso precise de wheres com mais de uma trait pode-se usar o `+` para adicionar mais traits a restrição, e junto a isso pode-se adicionar restrição de lifetime
ex: `<T: fmt::Display + Clone + 'static>`

Bem espero que gostaram da aula, proxima aula veremos como deixar nossa trait mais genéricas ✌️
