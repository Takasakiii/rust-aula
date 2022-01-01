# Aula 1 - Sobre o Rust

## O que é Rust:

Rust é uma linguagem de programação baixo nível / nivel de sistema, que nasceu para que aplicações nesse nivel tenham a devida segurança (em questão de crash e memoria) e paralelismo.


Como linguagem ela se adequá bem a qualquer tipo de projeto indo de programar firmwares a sistemas web, jogos até mesmo binarios para android (não confundir com app, pois ainda não tem ferramentas para ela gerar os manifestos e activits)


## O que é preciso para programar em Rust:
 - Windows:
    - [Visual Studio Build Tools](https://visualstudio.microsoft.com/pt-br/downloads/)
    - [Rustup](https://www.rust-lang.org/pt-BR/learn/get-started)
 - Linux
    - Setup de build da sua distribuição 
        - Ubuntu e derivados: `sudo apt install build-essential`
        - Fedora e derivados: `sudo dnf groupinstall "Development Tools" "Development Libraries"`
        - Outras distros ter instalado o: `make`, `gcc` e o `gcc-c++`
    - [Rustup](https://www.rust-lang.org/pt-BR/learn/get-started)



Visual Studio Code Extensions:
 - [rust-analyzer](https://marketplace.visualstudio.com/items?itemName=matklad.rust-analyzer)
 - [Even Better Toml (opcional)](https://marketplace.visualstudio.com/items?itemName=tamasfe.even-better-toml)