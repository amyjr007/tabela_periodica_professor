# Tabela Periódica — Versão Professor

Versão da Tabela Periódica Interativa para **uso em sala de aula**: todas as
interações e animações do app, **sem a narração**. O professor conduz a aula
falando por cima, no ritmo dele.

**Abrir:** https://amyjr007.github.io/tabela_periodica_professor/

Prof. Amauri Junior

---

## Como usar em sala

A barra de controle fica no canto inferior esquerdo:

| Controle | O que faz |
|---|---|
| `◀◀` / `▶▶` | Volta / avança uma etapa do tópico |
| `⏸` | Congela tudo — animação, destaques e a sequência inteira |
| `1x` | Alterna a velocidade: 1x → 1.5x → 2x |
| `✕` | Recolhe a barra (ela some do caminho; passe o mouse pra trazer de volta) |

**Atalhos de teclado**, úteis com apresentador ou teclado sem fio:

- **espaço** — pausa e continua
- **seta direita / esquerda** — próxima etapa / etapa anterior

O botão redondo "próximo" que aparece à direita é do próprio app: nas pausas
da narrativa ele espera um toque pra seguir. É o que dá o controle do ritmo
durante a explicação.

O app abre direto no menu de módulos, sem capa e sem o tutorial guiado.

---

## Como esta versão funciona

As animações do app original **não são independentes do áudio — são
disparadas por ele**: 76 handlers de `ended` e 401 `setTimeout` sincronizados
com a narração. Simplesmente remover os mp3 faria metade das interações
travar, esperando um evento que nunca chegaria.

Em vez disso, o construtor `Audio` é substituído por um **áudio fantasma**:
não emite som, mas conta o tempo usando a **duração real** de cada mp3 (lida
com `ffprobe` e embutida no arquivo). As 130 chamadas `new Audio(...)` do app
seguem funcionando sem uma linha alterada, e as animações mantêm o ritmo
exato do original.

O áudio virtual, os `setTimeout` e os `setInterval` rodam todos do **mesmo
relógio virtual**. Por isso congelar esse relógio congela o app inteiro em
sincronia, e a velocidade acelera tudo junto: animação e narrativa nunca se
desencontram.

Consequência prática: **nenhum mp3 é baixado**. São ~1,3 MB no lugar dos
~32 MB do app completo — abre rápido no projetor da escola, mesmo com
internet ruim.

---

## Origem do arquivo

Este repositório é **gerado**, não editado à mão. O `index.html` sai do app
principal pelo script `build_aula.js`, que vive lá:

    amyjr007/app-tabela-periodica  →  node build_aula.js

Quando o app principal mudar, rode o script de novo e publique o resultado
aqui. Editar o `index.html` direto neste repositório faz o trabalho ser
perdido na próxima geração.

Não há `sw.js` nem `manifest.json` de propósito: sem service worker, o
professor sempre pega a versão mais recente, sem cache velho no meio.
