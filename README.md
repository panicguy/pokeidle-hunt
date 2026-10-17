# /hunt for PokeIdle

Integração direta do **/hunt**, originalmente distribuído como extensão do navegador.

O script analisa o Pokémon ativo e compara as hunts disponíveis para mostrar estimativas de **XP/h**, **ouro/h**, **abates/h**, tempo para matar e risco.

## Arquivo

`pokeidle-hunt.js`

O arquivo reúne, nesta ordem:

1. CSS da interface
2. hook do WebSocket
3. leitura dos módulos e catálogos do jogo
4. motor de cálculo
5. medição de geometria/ritmo das hunts
6. interface `/hunt`

## Como adicionar ao PokeIdle

Coloque o arquivo em uma pasta pública do projeto, por exemplo:

```text
/public/js/pokeidle-hunt.js
```

Carregue **antes do script principal que cria o WebSocket do jogo**:

```html
<script src="/js/pokeidle-hunt.js?v=1.0.3"></script>
<script src="/js/app.js"></script>
```

Se o cliente principal usar `type="module"`, a regra é a mesma: o `/hunt` precisa ser carregado antes do código que abre a conexão do jogo.

## Por que a ordem importa?

O hook envolve `window.WebSocket` e adiciona um listener às mensagens recebidas do próprio domínio. Se o socket principal já tiver sido criado antes do carregamento do `/hunt`, ele pode perder o pacote inicial de estado (`welcome`).

## Atualização automática dos dados do jogo

A integração continua lendo módulos e catálogos do próprio PokeIdle em tempo de execução, incluindo dados de:

- golpes e cooldowns;
- Outland;
- Megas;
- evoluções;
- stats e overrides;
- recompensas e itens.

Assim, várias alterações do jogo passam a ser refletidas após um F5 sem precisar copiar tabelas para este repositório.

## Segurança

A versão integrada mantém o comportamento observado na extensão original: ela lê mensagens recebidas pelo WebSocket do jogo para calcular e exibir informações. O código deste repositório não adiciona envios de comandos ao WebSocket.

## Manutenção

Este repositório é a fonte oficial da versão integrada. Mudanças futuras podem ser feitas diretamente em `pokeidle-hunt.js` e versionadas por commits.
