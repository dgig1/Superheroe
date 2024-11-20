# Superheroe
## Trabalho de Danilo Gigliotti RA 10443431 e Gabriela Nellessen RA 10441930

O objetivo dessa tarefa é de transformar uma aplicação em HTML, CSS e JavaScript em uma aplicação NextJS 14
]
### Estrutura de Projeto NextJS 14

A estrutura do diretório ficou dessa forma

```plaintext
Superheroe/
├── app/
│   └── heroes/
│       ├── globals.css
│       ├── layout.js
│       └── page.js
├── components/
│       └── HeroeCard.jsx
└── ...
```



### 1. Arquivo Global de Estilos: globals.css

- O que faz: Define os estilos gerais da aplicação, como cores de fundo, fontes e layout básico.
- Onde é usado: É importado no arquivo layout.jsx para que os estilos sejam aplicados a todas as páginas da aplicação.
  
# Destaques:

`html, body:` Configura o fundo da página como cinza claro, usa a fonte monospace, e remove margens e espaçamentos padrão.
`heroes`: Estiliza o container que agrupa os cards dos heróis. Define um layout flexível com múltiplas linhas.
`heroes article`: Configura os cards dos heróis, incluindo dimensões, bordas arredondadas, e espaçamento.
`heroes article img`: Aplica bordas arredondadas à imagem do herói e limita sua altura máxima.


```CSS
html, body {
  background-color: #f0f0f0;
  font-family: monospace;
  height: 100%;
  margin: 0;
  padding: 0;
}

#heroes {
  display: flex;
  flex-flow: row wrap;
  width: 100%;
  padding: 20px;
}

#heroes article {
  height: 720px;
  width: 300px;
  background-color: #fff;
  border-radius: 10px;
  margin: 10px;
}

#heroes article img {
  border-radius: 10px 10px 0 0;
  width: 100%;
  max-height: 400px;
}

#heroes h1 {
  text-align: center;
}

#heroes article p {
  padding: 0 10px;
  width: calc(100% - 20px);
}

#heroes article p span {
  height: 10px;
  display: block;
  margin-top: 5px;
  border-radius: 5px;
}
```

### 2. Componente Reutilizável: HeroCard.jsx

- O que faz: Renderiza os dados de um herói individualmente em um card estilizado.
- Onde é usado: É chamado dentro da página `/heroes/page.jsx` para exibir cada herói.
  
# Detalhes Internos:
Recebe um objeto hero como props, contendo informações como nome, imagem, e estatísticas do herói.
Exibe a imagem do herói, nome, e duas barras de progresso para intelligence e strength.
As barras de progresso são criadas usando estilos inline, onde a largura é definida como uma porcentagem baseada nos valores recebidos.

```JavaScript
export default function HeroCard({ hero }) {
    const { name, powerstats, image } = hero;
    return (
      <article>
        <img src={image.url} alt={`${name}'s image`} />
        <h1>{name}</h1>
        <p>
          Intelligence:{" "}
          <span
            style={{
              width: `${powerstats.intelligence}%`,
              backgroundColor: "#F9B32F",
            }}
          ></span>
        </p>
        <p>
          Strength:{" "}
          <span
            style={{
              width: `${powerstats.strength}%`,
              backgroundColor: "#FF7C6C",
            }}
          ></span>
        </p>
      </article>
    );
  }
  
```

### 3. Página de Heróis: HeroesPage
- O que faz: Obtém dados dos heróis a partir de uma API externa e os exibe em uma página com múltiplos cards.
- Onde está localizada: No arquivo `/app/heroes/page.jsx`, acessível pela rota `/heroes.`

# Como funciona: Define uma constante *BASE_URL* com o endpoint da API de heróis.
Usa a função assíncrona fetchHero para buscar os dados de cada herói pelo ID.
fetch é usado para realizar a chamada HTTP GET.
Caso a requisição falhe, lança um erro.

# A função HeroesPage:
Lista os IDs dos heróis (200 e 465).
Utiliza Promise.all para realizar as requisições simultaneamente.
Renderiza um container #heroes, onde cada card de herói é exibido utilizando o componente HeroCard.

```JavaScript
/import HeroCard from "Superheroe/components/HeroCard";

const BASE_URL = "https://superheroapi.com/api.php/4995282617154105/";
const ACCESS_TOKEN = "1bc518e0c6e9483484ea8c8198fdf8bc";

async function fetchHero(id) {
  const res = await fetch(`${BASE_URL}${id}`);
  if (!res.ok) throw new Error("Falha ao buscar os dados");
  return res.json();
}

export default async function HeroesPage() {
  const heroIds = [200, 465]; // IDs dos heróis
  const heroes = await Promise.all(heroIds.map(fetchHero));

  return (
    <div id="heroes">
      {heroes.map((hero) => (
        <HeroCard key={hero.id} hero={hero} />
      ))}
    </div>
  );
}

```

### 4. Layout Global: layout.jsx

- O que faz: Fornece uma estrutura de layout comum para todas as páginas.
- Onde está localizada: Em `/app/layout.jsx.`

# Como funciona:
Define os metadados da aplicação, como título e descrição, usados em elementos <head> do HTML.
Renderiza o conteúdo da aplicação dentro da tag <body> do HTML, aplicando os estilos globais do `globals.css`.
Este layout é automaticamente aplicado a todas as rotas sob a pasta `/app`.

```JavaScript
import "Superheroe/src/heroes/globals.css";

export const metadata = {
  title: "Superheroes App",
  description: "Exibição de heróis usando API",
};

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
```

### Fluxo Geral da Aplicação
- O Usuário Visita a Página de Heróis:
Acessando `/heroes`, o Next.js chama a função HeroesPage para renderizar a página.

- Busca de Dados na API:
IDs dos heróis (200 e 465) são enviados para a API da SuperHero. A função fetchHero retorna as informações dos heróis em formato JSON.

- Renderização de Componentes:
A página usa os dados da API para renderizar múltiplos componentes HeroCard, cada um representando um herói.

- Exibição na Interface do Usuário:
Os cards são exibidos em um layout responsivo, estilizados de acordo com as regras definidas em globals.css.

### Benefícios da Arquitetura
- Modularidade:
Cada parte da aplicação (estilo, lógica de busca, layout) está separada, facilitando a manutenção.

- Reutilização de Código:
O componente HeroCard pode ser usado em outras partes da aplicação caso necessário.

- Performance:
O uso de Server-Side Rendering (SSR) garante que os dados dos heróis sejam buscados no servidor antes da página ser enviada ao navegador.

- Facilidade de Expansão:
Novos heróis podem ser adicionados facilmente apenas inserindo seus IDs na lista de heroIds na página /heroes.

### Conclusão
A aplicação demonstra como integrar APIs externas e gerenciar componentes em Next.js, unindo eficiência, modularidade e design responsivo. É uma base sólida para expandir funcionalidades ou criar projetos semelhantes.
