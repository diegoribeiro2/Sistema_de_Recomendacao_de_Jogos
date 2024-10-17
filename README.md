# Estudo de Caso: Sistema de Recomendação de Jogos

## Introdução

Neste estudo, apresento um projeto que visa construir um sistema de recomendação de jogos utilizando a técnica de filtragem colaborativa baseada em itens. O objetivo é criar um modelo capaz de sugerir os 10 jogos mais similares a um título fornecido pelo usuário, utilizando um dataset extraído da plataforma Steam, que contém informações valiosas sobre jogos e seus jogadores.

Para isso, calculei avaliações implícitas, já que os dados de notas diretas dos usuários não estão disponíveis. Essas avaliações foram estimadas a partir do tempo que cada jogador dedicou a cada jogo, oferecendo uma perspectiva única para as recomendações.

Além de desenvolver o modelo de recomendação, também disponibilizei uma API que permite aos usuários informar o nome de um jogo e receber sugestões de títulos similares em resposta. 

## Filtragem Colaborativa

A filtragem colaborativa é uma técnica amplamente utilizada para construir sistemas de recomendação. Diferente de métodos que dependem de características e descrições dos itens, essa abordagem se baseia nas interações entre usuários e itens para identificar padrões e gerar sugestões personalizadas.

Na filtragem colaborativa baseada em itens, a similaridade entre jogos é avaliada através das interações dos usuários. Por exemplo, se um jogador desfrutou de "The Witcher 3", o sistema pode recomendar outros RPGs que também foram apreciados por usuários que jogaram "The Witcher", como "Skyrim", "Dragon Age" e "Mass Effect".

### Benefícios da Filtragem Colaborativa

- **Independência de Dados Descritivos:** O modelo não necessita de informações detalhadas sobre os jogos, apenas do histórico de interações dos usuários.
- **Flexibilidade com Itens Novos:** A técnica é capaz de lidar com novos jogos, mesmo sem descrições prévias.
- **Personalização das Recomendações:** As sugestões são adaptadas às preferências de cada usuário.

### Limitações da Filtragem Colaborativa

- **Novos Usuários ou Itens:** Pode enfrentar dificuldades ao lidar com novos usuários ou jogos sem histórico suficiente.
- **Falta de Explicação nas Recomendações:** A lógica por trás das sugestões pode não ser imediatamente clara para o usuário.

## Dataset

O dataset utilizado consiste em duas tabelas principais:

- **Players:** Contém informações sobre cada jogador, como ID, nome e horas totais jogadas.
- **Games:** Relaciona cada jogo com os IDs dos jogadores que o experimentaram, junto ao tempo dedicado por cada um.

Essas tabelas permitem identificar quais jogos um jogador possui e o tempo que investiu em cada um. As interações entre jogos e jogadores possibilitam comparações, utilizando as horas jogadas para calcular avaliações implícitas, dividindo o tempo dedicado a cada jogo pelo total de horas do jogador.

## Pré-processamento

Antes da aplicação do algoritmo de filtragem colaborativa, algumas etapas de pré-processamento dos dados são essenciais:

1. **Remoção de Dados Irrelevantes:** Excluí jogadores e jogos com informações escassas para evitar overfitting.
2. **Normalização das Avaliações:** Ajustei as avaliações implícitas para uma escala padrão, como de 0 a 1 ou de 1 a 5.
3. **Tratamento de Valores Ausentes:** Preenchi dados faltantes com zero, indicando que o usuário não jogou aquele jogo.
4. **Conversão de Dados:** Transformei os dados em uma matriz de usuários x itens, preparando a entrada para o modelo.

Com o pré-processamento concluído, o dataset estará adequado para a aplicação do algoritmo de similaridade selecionado.

## Modelos de Similaridade

Várias métricas podem ser empregadas para calcular a similaridade entre itens em um sistema de recomendação baseado em filtragem colaborativa:

- **Distância Euclidiana:** Avalia a distância geométrica entre os vetores de avaliações. Distâncias menores indicam maior similaridade.
- **Cosseno:** Calcula o cosseno do ângulo entre os vetores de avaliação, com valores próximos a 1 indicando alta similaridade.
- **Correlação de Pearson:** Mede a correlação linear entre as avaliações. Resultados próximos de 1 ou -1 denotam forte similaridade.

Cada métrica possui vantagens e desvantagens, sendo necessário testar e comparar qual delas proporciona melhores recomendações para o dataset em uso. Diferentes limiares de similaridade também podem ser experimentados para ajustar o número de itens semelhantes retornados.

## Recomendação de Itens Similares

Com o modelo de similaridade implementado, é possível gerar recomendações com base nos jogos que o usuário já consumiu. O processo inclui as seguintes etapas:

1. **Seleção de um Item de Entrada:** Escolhi um jogo já avaliado pelo usuário.
2. **Identificação de Itens Similares:** Encontrei os N jogos mais similares ao item selecionado, com base no modelo de similaridade.
3. **Filtragem de Itens Consumidos:** Excluí títulos que o usuário já experimentou.
4. **Retorno das Recomendações:** Apresentei os N jogos restantes como sugestões.

Por exemplo, se o usuário jogou "The Witcher 3" e deseja recomendações, o sistema pode sugerir RPGs similares que ainda não foram jogados, garantindo que as sugestões sejam relevantes e personalizadas.

## API de Recomendação

Para tornar as recomendações acessíveis, desenvolvi uma API (Interface de Programação de Aplicações). Esta API serve como um ponto de contato que aceita requisições HTTP com o nome de um jogo de entrada e retorna as 10 recomendações semelhantes geradas pelo modelo.

Os principais componentes da API incluem:

- **Servidor Web:** Responsável por receber as requisições HTTP e enviar respostas em formato JSON.
- **Endpoint:** A URL onde as requisições devem ser dirigidas.
- **Lógica do Modelo:** O código Python que realiza o processamento e gera as recomendações.
- **Banco de Dados:** Armazena informações sobre jogos e dados do modelo.
- **Container Docker:** Facilita a implementação da aplicação.

Ao receber uma requisição, a API executa a lógica de recomendação, consulta os dados no banco e retorna um JSON com os 10 jogos sugeridos. Essa interface pode ser integrada a uma aplicação web, app móvel ou qualquer sistema que deseje fornecer recomendações personalizadas.

## Considerações Finais

A construção de um sistema de recomendação é um processo multifacetado que abrange desde a coleta e preparação de dados até a implementação de um modelo e a disponibilização de recomendações através de uma API.

Neste estudo de caso, a abordagem de filtragem colaborativa se mostrou adequada, pois não requer informações descritivas dos jogos e consegue capturar relações de similaridade com base nas interações dos usuários. Após testar diferentes modelos e limiares de similaridade, o sistema é capaz de oferecer recomendações personalizadas de novos jogos com base nas preferências dos usuários.

Esse tipo de modelo é extremamente valioso para aprimorar a experiência do usuário, proporcionando recomendações relevantes e fomentando o engajamento em plataformas como a Steam.
