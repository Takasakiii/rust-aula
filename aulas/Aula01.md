# Aula 1 - Sobre o Rust

## O que é Rust:

Rust é uma linguagem de programação baixo nível/nível de sistema, que nasceu para que aplicações nesse nível tenham a devida segurança (em questão de crash e memória) e paralelismo.


Como linguagem ela se adéqua bem a qualquer tipo de projeto indo de programar firmwares a sistemas web, jogos até mesmo binários para Android (não confundir com app, pois ainda não tem ferramentas para ela gerar os manifestos e activities).


## O que é preciso para programar em Rust:
 - Windows:
    - [Visual Studio Build Tools](https://visualstudio.microsoft.com/pt-br/downloads/)
    - [Rustup](https://www.rust-lang.org/pt-BR/learn/get-started)
 - Linux
    - Setup de build da sua distribuição 
        - Ubuntu e derivados: `sudo apt install build-essential`
        - Fedora e derivados: `sudo dnf groupinstall "Development Tools" "Development Libraries"`
        - Outras distros: ter instalado o: `make`, `gcc` e o `gcc-c++`
    - [Rustup](https://www.rust-lang.org/pt-BR/learn/get-started)



Extensões do Visual Studio Code:
 - [rust-analyzer](https://marketplace.visualstudio.com/items?itemName=matklad.rust-analyzer)
 - [Even Better TOML](https://marketplace.visualstudio.com/items?itemName=tamasfe.even-better-toml) (opcional)
