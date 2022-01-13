# Aula 16 - Enums

`Enum` é uma estrutura especial que permite representar uma série de elementos e seus estados em apenas uma estrutura. Um exemplo simples, em que podemos enumerar ações de um pássaro:

```rs
enum AcaoPassaro {
    Voar,
    Comer,
    Dormir,
    Procriar
}
```

Podemos então usar um `match` para verificar que condição temos selecionada:

```rs
let acao = AcaoPassaro::Voar;

match acao {
    Passaro::Voar => println!("O pássaro está voando."),
    Passaro::Comer => println!("O pássaro está comendo."),
    Passaro::Dormir => println!("O pássaro está dormindo."),
    Passaro::Procriar => println!("O pássaro está procriando."),
}

// Também podemos verificar com if:
if acao == Passaro::Voar {
    // Ação.
}
```

Também podemos usar enums para representar um tipo básico (assim como em outras linguagens):

```rs
// repr define o tipo de dados que será representado.
#[repr(u8)]
enum AcaoPassaro {
    Voar, // 0
    Comer = 3, // 3
    Dormir,  // 4
    Procriar // 5
}

// Repare que quando não definimos com igualdade um item do enum, ele usará o próximo lógico.
```

No Rust também podemos levar dados dentro de suas opções:

```rs
enum Event {
    Mouse (u8, u8, u8), // Podemos enviar como tuplas
    Keyboard {
        key: char, // ou por structs.
        action: u8
    }
    None
}
```

Porém não podemos consumir diretamente, ou fazer pattern matching para desconstruí-los do enum, pois não temos ideia de que opção ele está e quais dados vão ser recebidos naquela opção, mas podemos usar `if` e `match`:

```rs
// Usaremos o enum do exemplo anterior o Event:

let event = Event::Keyboard {
    key: 'a',
    action: 3
};

// if let vai checar se o enum está na opção desejada, para então podermos atribuir os valores a variáveis ou, como no exemplo, podemos desconstruí-los:
if let Event::Mouse (x, y, z) = event {
    // Todo o código aqui.
}

// Também podemos usar o match para verificar, e então o pattern matching para obter nossos valores:
match event {
    // Nesse caso, apenas usei a atribuição em vez de desconstruir a tupla.
    Event::Mouse(mouse) => {
        // ...
    }
    Event::Keyboard {
        action,
        .. // Podemos usar o .. para ignorar os demais fields na desconstrução e no match.
    } => {
        // ...
    },
    Event::None => {
        // ...
    }
}
```

Ainda podemos usar os [`Impl Blocks`](Aula14.md) para definirmos comportamentos nos nossos enums, assim como fazemos nas structs.
