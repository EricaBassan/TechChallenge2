
<h1 align="center">Previsão Bolsa de Valores</h1>
<h4 align="center">Repositório criado para o tech challenge 2 do curso de data analytics da FIAP</h4>

<p align="center">
  <a href="">
    <img src="Imagens\Stock_figure.png"
         alt="version">
  </a>

</p>




## Sumário

- [Sobre](#sobre)
- [Bibliotecas](#bibliotecas)
- [Analise exploratoria de dados (EDA)](#analise-exploratoria-de-dados-eda)
- [Métodos aplicados](#metodos-aplicados)
- [Resultados](#resultados)
  


# Sobre
Este projeto foi criado para atender os requisitos do projeto Tech Challenge da [Faculdade de Tecnologia - FIAP](https://postech.fiap.com.br/?gclid=Cj0KCQjwnf-kBhCnARIsAFlg49228y9z3y6lf_mWZEekgcxZRZBDavxtRT-zAUNs33TZOJtXpGVMNlAaAue5EALw_wcB).<br>
O Tech Challenge foi ser divido em três etapas:
- Aplicação de um modelo de machine learning para projetar os valores de fechamento da bolsa de valores (Ibovespa).
- Atinigir uma acurácia maior do que 70% com o modelo.
- Justificar métodos realizados e os resultados obtidos.

# Bibliotecas

Foram utilizadas as seguintes bibliotecas para o desenvolvimento do projeto:

-Pandas (https://pandas.pydata.org/);

-Numpy (https://numpy.org/);

-Statsmodels (https://www.statsmodels.org/stable/index.html);

-Matplotlib (https://matplotlib.org/);

-Seaborn (https://seaborn.pydata.org/);

-Pmdarima (https://pypi.org/project/pmdarima/);

-Prophet (https://pypi.org/project/prophet/);

-Sklearn (https://scikit-learn.org/stable/).

# Analise exploratoria de dados (EDA)

Iniciamos o projeto com a análise dos dados.

Exportamos uma base de dados histórica dos últimos 10 anos da Ibovespa, com o valor do fechamento mensal da bolsa de valores (https://br.investing.com/indices/bovespa-historical-data), para avaliar o comportamento dos dados.

Através do gráfico obtido (figura 1), podemos observar que antes de 2021, os valores tinham um comportamento que evidenciava uma certa tendencia a crescimento do fechamento. 

Após o ano de 2021, o gráfico começa a apresentar um comportamento diferente, em comparação com o passado, que vem se mantendo até presente data.

Somando essa observação com o fato de que ambos os contextos geopolitico e economico atuais são bem divergentes dos anos anteriores, consideramos que os valores anteriores a 2021 poderiam levar o modelo a uma tendencia incorreta da situação atual da bolsa de valores.

Dessa forma, para o nosso modelo estamos utilizando uma base histórica diária do mercado de ações, com valores que se iniciam em jan/21 - até presente data.


<p align="center">
  <a href=" ">
    <img src="Imagens\Fechamento Ibovespa últimos 10 anos.PNG" alt="webapp-architecture">
  </a>
  <figcaption style="font-size: smaller; text-align: center;">Figura 1</figcaption>
</p>


Prosseguindo com a analise exploratória, criamos mais um gráfico com os valores diários da Ibovespa de jan/21 até ago/23, para analisar o comportamento dos valores (figura 2).

Observando o gráfico não encontramos uma tendencia evidente dos dados.

<p align="center">
  <a href=" ">
    <img src="Imagens\Fechamento Ibovespa-2021-2023.PNG" alt="webapp-architecture">
  </a>
  <figcaption style="font-size: smaller;">Figura 2</figcaption>
</p>

Assim, criamos um gráfico com a média e o desvio padrão junto dos dados originais (figura 3);

<p align="center">
    <img src="Imagens\Fechamento-media-desvio-padrao.PNG">
  </a>
  <figcaption style="font-size: smaller;">Figura 3</figcaption>
</p>

O próximo gráfico é um histrograma, indicando a distribuição dos dados (figura 4);


<p align="center">
  <a href=" ">
    <img src="Imagens\Histrograma-ibovespa.PNG">
 </a>
  <figcaption style="font-size: smaller;">Figura 5</figcaption>
</p>

E por fim, temos um gráfico analisando a % de variancia acumulada das ações (figura 5), indicando o desempenho do mercado nos ultimos anos:

<p align="center">
  <a href=" ">
    <img src="Imagens\Acumulo-variacao-porcental-ibovespa.PNG">
   </a>
  <figcaption style="font-size: smaller;">Figura 5</figcaption>
</p>

# Arquitetura
Esta é uma visão geral da arquitetura do TechNews.

<p align="center">
  <a href="">
    <img src=".github\images\architecture-overview.png">
  </a>
  <figcaption style="font-size: smaller;">Figura 5</figcaption>
</p>

## Web App

A concepção da aplicação foi fundamentada no padrão arquitetural MVC (Model View Controller), sendo implementada por meio do ASP.NET Core.

No âmbito do negócio, sua responsabilidade é requisitar os recursos restritos disponibilizados pela API de notícias e, posteriormente, apresentando esses elementos aos usuários.

Quanto à segurança, a aplicação assume a responsabilidade de transmitir os dados referentes à criação de usuários ou as credenciais de acesso ao Authorization Server. Mais detalhes em [Segurança](#segurança)


Caso as credenciais estiverem corretas, a aplicação irá receber um JWT assinado (JWS - Json Web Signature) e irá autenticar o usuário via cookie. Então, para cada requisição feita à API de notícias o mesmo JWT será enviado pelo header.

<p align="center">
  <a href="">
    <img src=".github\images\webapp-architecture.png" alt="webapp-architecture">
  </a>
</p>

## Core API

Work in Progress

## Auth API (Authorization Server)

Foi adotado o estilo arquitetural REST (Representational State Transfer) com camadas, utilizando ASP.NET Core.

A camada de <b>Filtros</b> lidam com exceções e tratamento de Model State inválidos.

A camada de <b>Controllers</b> direciona o fluxo das requisições. É responsável por expor os parâmetros públicos da chave assimétrica, realizar chamadas ao Identity para autenticação/autorização do usuário e criar o JWT utilizando as classes de Serviços.

A camada de <b>Data</b> se integra com classes do Identity (User e Role) e com o Entity Framework para mapeamento de dados.

A camada de <b>Services</b> possui serviços com responsabilidades diversas como: gerenciar (buscar ou persistir) a chave privada no Azure Key Vault, assinar um token digitalmente com criptografia RSA e a criação da chave assimétrica através de criptografia RSA.

O <b>Background Service</b> que vemos abaixo é uma parte da camada de serviços. Ele constitui uma solução simples para a rotação da chave privada que gera os tokens. O ideal é possuir uma solução mais robusta, consistindo em uma aplicação que gerencia a rotação da chave para todas as instâncias de aplicações que a utilizam.

<p align="center">
  <a href="">
    <img src=".github\images\architecture-auth.png" alt="api-architecture">
  </a>
</p>

# Segurança

A orquestração do fluxo de autenticação do Tech News foi fundamentada na documentação do [OAuth 2.0](https://datatracker.ietf.org/doc/html/rfc6749) bem como na documentação do [JWT para Access Tokens OAuth 2.0](https://datatracker.ietf.org/doc/html/rfc9068).

<p align="center">
  <a href="">
    <img src=".github\images\auth-flow-01.png" alt="api-architecture">
  </a>
</p>

<p align="center">
  <a href="">
    <img src=".github\images\auth-flow-02.png" alt="api-architecture">
  </a>
</p>

<p align="center">
  <a href="">
    <img src=".github\images\auth-flow-0201.png" alt="api-architecture">
  </a>
</p>

<p align="center">
  <a href="">
    <img src=".github\images\auth-flow-03.png" alt="api-architecture">
  </a>
</p>

<p align="center">
  <a href="">
    <img src=".github\images\auth-flow-04.png" alt="api-architecture">
  </a>
</p>



## Rotação das Chaves
Para a rotação da chave privada optamos por uma solução simples para o tech challenge, um <b>background service</b>. O ideal seria uma solução mais robusta, consistindo em uma aplicação que gerencia a rotação da chave para todas as instâncias de aplicações que a utilizam. 

O serviço rotaciona a chave privada a cada X dias (parametrizado por variável). Utiliza-se o algoritmo de criptografia assimétrica [RSA](https://pt.wikipedia.org/wiki/RSA_(sistema_criptogr%C3%A1fico)) para a criação de uma nova chave. 

Os parâmetros privados da chave são persistidos no Azure Key Vault, enquanto os parâmetros públicos são encapsulados em um JWK (Json Web Key) e expostos em uma URL com uma lista de JWKS (Json Web Key Set). Por exemplo <b>url-api/jwks</b>. 

São esses parâmetros públicos disponíveis nessa URL que as API's de recursos protegidos irão validar o JWT recebido.

## Prevenção contra possíveis ataques

Algumas camadas adicionais de segurança foram implementadas para evitar alguns dos ataques mais comuns.

| Nome | Prevenção Implementada|
| :---------: | :---------: |
| <p style="width:260px; text-align: left;">SQL Injection</p> | <p style="text-align: left;">Qualquer acesso aos dados é feito através de procedures parametrizadas e do ORM.</p> |
| <p style="width:260px; text-align: left;">Brute Force</p> | <p style="text-align: left;">Lockout após X tentativas erradas de autenticação, Hash de senhas utilizando algoritmo Bcrypt e formato rígido de senha (mínimo: 8 caracteres, 1 dígito, 1 minúscula, 1 maiúscula e 1 caracter especial)</p> |
| <p style="width:260px; text-align: left;">Cross Site Scripting (XSS)</p> | <p style="text-align: left;">Validações server-side do que recebemos do browser, cookies de autenticação como HTTP Only e criptografado para evitar acessá-los por script.</p> |
| <p style="width:260px; text-align: left;">Cross Site Request Forgery (CSRF)</p> | <p style="text-align: left;">Validação de Anti Forgery Token e CORS (habilitado por padrão pelo ASP .NET Core).</p> |
| <p style="width:260px; text-align: left;">Man in the Middle</p> | <p style="text-align: left;">Habilitado HSTS para informar ao cliente que somente requisições HTTPS são aceitas e redirecionamento de protocolos HTTP para HTTPS.</p> |


# CI / CD

O CI / CD do TechNews consiste em três pipelines: <b>Azure Resources</b>, <b>Build</b> e <b>Deploy</b>.

O pipeline de <b>Azure Resources</b> cria todos os recursos descritos na [Arquitetura](#arquitetura), fazendo uso dos ARM Templates disponíveis na pasta "azure". Os recursos criados são: Key Vault, Container Registry, SQL Databases e Blob Storage.

O pipeline de <b>Build</b> assume o papel de Integração Contínua (CI), realizando a compilação das aplicações juntamente com suas dependências. Além disso, compila as imagens com base os dockerfiles, gerando os artefatos que são publicados no Container Registry.

O pipeline de <b>Deploy</b> assume o papel de Entrega Contínua (CD), criando as instâncias dos containers no Azure Container Instance com base nas imagens do Container Registry.

<!-- TODO: - Database scripts ou Migrations -->

# Executando a aplicação
É possível executar a aplicação realizando a configuração manualmente, ou utilizando Docker (recomendado).

## Docker
Para rodar localmente, é possível utilizar o Docker.  
Abaixo o passo a passo para executar a aplicação localmente:
- Realizar o clone do projeto na pasta desejada:
    bash
        git clone https://github.com/pistoladas-group/tech-challenge-02.git
    
- Configurar certificados para habilitar conexão via https:
    bash
        dotnet dev-certs https -ep "$env:USERPROFILE\.aspnet\https\technews.pfx"  -p "OVmTv9lykb0)>m=wWcQaJ"
        dotnet dev-certs https --trust
    
- Utilizar o comando abaixo para subir a aplicação utilizando docker-compose:
    bash
        docker-compose -f docker-compose.debug.yml up --build
    
