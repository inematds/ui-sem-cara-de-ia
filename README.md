# UI sem cara de IA

[![UI sem cara de IA](guia/assets/banner.jpg)](https://inematds.github.io/ui-sem-cara-de-ia/guia/)

**🇧🇷 [Português](README.md) · 🇺🇸 [English](README.en.md) · 🇪🇸 [Español](README.es.md)**

## O que é

UI sem cara de IA é um kit gratuito para quem cria sites e aplicativos com Claude Code ou Codex. Ele traz um verificador que lê o seu projeto e aponta o que entrega o trabalho automático: cor e cantos de fábrica, aviso feito à mão, botão que o teclado não alcança e excesso de efeitos. Depois, ele escreve o pedido de correção para você colar na sua IA. Precisa de Python 3 para rodar. Não instala nada no seu projeto e não altera nenhum arquivo.

## 📖 Guia de uso

Guia completo: **https://inematds.github.io/ui-sem-cara-de-ia/guia/**

[🎬 Vídeo: veja o kit em ação](https://www.youtube.com/watch?v=blXnqoGoltM)

## Peça para a sua IA analisar

```text
Baixe o kit https://github.com/inematds/ui-sem-cara-de-ia numa pasta temporária, fora do meu projeto.
Leia o README e rode o verificador kit/checar_ui.py apontando para a pasta do meu projeto.
Se faltar Python 3.9 ou mais novo, me avise antes de instalar qualquer coisa.
Confira cada alerta no código e me diga, em português simples, quais são reais, quais são falsos
e por onde vale começar. Não mude nenhum arquivo do projeto antes do meu sim.
```

---

## Instalação técnica

Requisito: Python 3.9 ou mais novo. O kit usa só a biblioteca padrão.

```bash
git clone https://github.com/inematds/ui-sem-cara-de-ia
cd ui-sem-cara-de-ia
python3 kit/testes/run.py                      # 119 testes, sem rede
python3 kit/checar_ui.py exemplo/antes         # 8 alertas
python3 kit/checar_ui.py exemplo/depois        # nenhum alerta
python3 kit/checar_ui.py /caminho/do/projeto
python3 kit/checar_ui.py /caminho/do/projeto --pedido > pedido.txt
```

Substitua `/caminho/do/projeto` pela pasta real que deseja analisar. Mantenha o kit fora do seu projeto. Códigos de saída: `0` sem alertas, `1` com alertas, `2` erro de uso ou nenhum arquivo de interface encontrado. Use `--json` para integrar com outras ferramentas.

### O que ele procura

| Grupo | Alerta | Como detecta |
|---|---|---|
| Identidade | Cantos de fábrica | `--radius` igual a `0.625rem` ou `0.5rem` num tema com `--primary` |
| Identidade | Cor sem marca | `--primary` igual a um padrão do shadcn/ui ou cinza em OKLCH, HSL ou hexadecimal |
| Identidade | Fonte de sempre | Só Geist ou Inter (next/font, @fontsource ou CSS), num projeto com esse tema |
| Comportamento | Aviso feito à mão | Estado ou componente de toast sem biblioteca de avisos no `package.json` da pasta ou acima dela |
| Comportamento | Gaveta sem arrasto | Componente de gaveta sem eventos de toque ou ponteiro e sem biblioteca própria |
| Comportamento | Menu sem setas | Menu de comandos sem `ArrowDown` e sem biblioteca própria |
| Comportamento | Clique sem teclado | `div`, `span`, `p`, `li`, `tr`, `td`, `img`, seções, títulos, `motion.*` ou `a` sem `href`, com clique (React, Vue ou Svelte) e sem `tabIndex` que permita foco |
| Excesso | Muitos destaques | Mais de um efeito conhecido em página de rota, `App` ou HTML, olhando dois níveis de importação |

São alertas de leitura de código. O verificador não abre a tela, não mede contraste e não julga se o design é bom. A regra de clique verifica se o elemento recebe foco; não confirma que Enter ou Espaço executam a ação. Um alerta pode ser falso, e uma tela sem alertas ainda pode estar ruim.

Limites: ignora arquivos de teste, comentários, `components/ui`, pastas geradas e, em `public/`, tudo que não seja HTML. Elementos com `{...props}` não são acusados, porque as propriedades podem trazer o foco. `svg`, `i`, `label`, `ul`, `nav` e componentes próprios com clique não são analisados. Efeitos fora da lista interna não são contados. Cliques ligados por `addEventListener` não são vistos. Identidade só é analisada quando o projeto usa um tema com `--primary` e `--radius`, como o do shadcn/ui; cores direto em classes do Tailwind, temas dentro de `<style>` e fontes carregadas por link não entram. Rode na raiz do projeto.

Cobertura: ele só lê telas em HTML, JSX, TSX, Vue, Svelte, Astro e MDX. Sem nenhuma delas, sai com código `2` e não aprova. Se encontrar telas em PHP, EJS, Blade, Pug, Twig ou parecidas ao lado das que sabe ler, avisa que o resultado não vale para elas.

### Arquivos

| Caminho | Para quê |
|---|---|
| `kit/checar_ui.py` | Verificador |
| `kit/medir_pastas.py` | Total de vários projetos, sem nomes |
| `kit/PEDIDOS.md` | Pedidos prontos para a IA |
| `kit/bibliotecas.json` | Bibliotecas conferidas, com site e licença |
| `kit/skill/ui-sem-cara-de-ia/` | Skill para Claude Code: copie para `.claude/skills/` |
| `exemplo/antes`, `exemplo/depois` | Mesmo cadastro, com e sem os problemas |
| `FATOS.md` | Fontes e medições |

## O processo, do início ao fim

| # | Passo | Comando | Quem faz |
|---|---|---|---|
| 1 | Baixar o kit | `git clone https://github.com/inematds/ui-sem-cara-de-ia` | 🧠 você |
| 2 | Conferir que funciona | `python3 kit/testes/run.py` | ⚙️ script |
| 3 | Verificar o projeto | `python3 kit/checar_ui.py <pasta>` | ⚙️ script |
| 4 | Gerar o pedido | `python3 kit/checar_ui.py <pasta> --pedido > pedido.txt` | ⚙️ script |
| 5 | Corrigir | Cole `pedido.txt` no Claude Code ou no Codex | 🤖 sua IA |
| 6 | Olhar a tela | Teste no celular e só com o teclado | 🧠 você |
| 7 | Verificar de novo | `python3 kit/checar_ui.py <pasta>` | ⚙️ script |

## Veja também

[Kit Design com IA](https://github.com/inematds/kit-design-ia): skills para definir a direção visual. Este kit confere o resultado no código.

## Licença

MIT para o código e os textos deste kit. As bibliotecas citadas têm licenças próprias, listadas em `kit/bibliotecas.json`. Este kit é independente e não tem vínculo com elas.

Grátis no [INEMA.CLUB](https://inema.club).
