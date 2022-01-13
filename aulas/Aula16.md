# Aula 16 - Enums

`Enum` é uma estrutura especial que permite representar uma serie de elementos e seus estados em apenas uma estrutura. Um exemplo simples podemos enumerar ações de um pássaro

```rs
enum AcaoPassaro {
    Voar,
    Comer,
    Dormir,
    Procriar
}
```

Podemos então usar um `match` para verificar que condição temos selecionada

```rs

let acao = AcaoPassaro::Voar;

match acao {
    Passaro::Voar => println!("O passaro está voando"),
    Passaro::Comer => println!("O passaro está comendo"),
    Passaro::Dormir => println!("O passaro está dormindo"),
    Passaro::Procriar => println!("O passaro está procriando"),
}

// Também podemos verificar com if
if acao == Passaro::Voar {
    // acao
}
```

Também podemos usar enums para representar um tipo básico (assim como outras linguagens)

```rs
// repr define o tipo de dados que será representado
#[repr(u8)]
enum AcaoPassaro {
    Voar, // 0
    Comer = 3, // 3
    Dormir,  // 4
    Procriar // 5
}

// repare que quando não definimos com igualdade um item do enum ele usará o próximo lógico
```

No rust também podemos levar dados dentro de suas opções

```rs
enum Event {
    Mouse (u8, u8, u8), // Podemos enviar como tuplas
    Keyboard {
        key: char, // Ou por structs
        action: u8
    }
    None
}
```

Porem não podemos consumir diretamente, ou fazer pattern match para desconstruí-los do enum pois não temos ideia que opção ele está e quais dados vai ser recebido naquela opção, mas podemos usar `if` e `match`

```rs
// usaremos o enum do exemplo anterior o Event

let event = Event::Keyboard {
    key: 'a',
    action: 3
};

// if let vai checar se o enum esta na opção desejada e então podemos atribuir os valores a variáveis ou como no exemplo podemos desconstruí-los
if let Event::Mouse (x, y, z) = event {
    // todo código aki
}

// Também podemos usar o match para verificar e então o pattern match para obter nossos valores
match event {
    // nesse caso apenas usei a atribuição de vez de desconstruir a tupla
    Event::Mouse(mouse) => {
        // ...
    }
    Event::Keyboard {
        action,
        .. // podemos usar o .. para ignorar os demais fields na desconstrução e no match
    } => {
        //....
    },
    Event::None => {
        // ....
    }
}
```

Ainda podemos usar os `Impl Blocks` para definirmos comportamentos nos nossos enums assim como fazemos nas structs.
