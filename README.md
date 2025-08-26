# Projeto: Engenharia de Dados e Análise de Documentos (Freelance)

Este repositório documenta um trabalho freelance focado em engenharia de dados e automação de processos. O objetivo foi extrair, tratar e unificar dados de aproximadamente 60 arquivos PDF, cada um com 16 páginas, que continham listas de presença e votação em um formato vetorial, impossibilitando a simples cópia do texto.

## Desafio Técnico
 Os arquivos PDFs não permitiam a extração de texto de forma convencional, pois as informações eram armazenadas como dados vetoriais (basicamente, "desenhos" de texto). Isso exigiu uma abordagem mais avançada para converter essas imagens em texto legível por máquina e, posteriormente, estruturar os dados para análise.


## Tecnologias e Ferramentas Utilizadas
* **Python:** Linguagem de programação principal.

* **Jupyter Notebooks:** Utilizados para o desenvolvimento e a exploração de cada etapa do processo.

* **Pandas:** Essencial para a manipulação, tratamento e análise de dados tabulares.

* **PyTesseract:** Biblioteca Python para realizar a extração de texto.

* **Regex (Expressões Regulares):** Utilizada para extrair padrões e garantir a formatação correta dos dados.

* **Fuzzywuzzy:** Biblioteca para realizar comparações de strings de forma "difusa" (aproximada), útil para unificar dados com pequenas variações.

---

## O Fluxo de Trabalho
O projeto foi dividido em etapas lógicas para garantir a precisão e a integridade dos dados:

### 1. Extração de Dados e OCR
O primeiro passo foi converter cada página dos 60 PDFs em imagens PNG. Em seguida, utilizei um OCR (Reconhecimento Óptico de Caracteres), com a biblioteca PyTesseract, para extrair o texto de cada uma dessas imagens. Um OCR é uma tecnologia que "lê" o texto de uma imagem e o converte em um formato digital, como um arquivo de texto.

### 2. Tratamento e Padronização
O texto extraído pelo OCR raramente é perfeito. As tabelas foram convertidas em DataFrames do Pandas, onde apliquei Regex para identificar e extrair padrões como nomes, CPFs e valores, garantindo que os dados estivessem limpos e organizados corretamente.

### 3. Unificação de Dados com Fuzzy Join
A principal dificuldade era unificar a lista de presença com a de votação, pois os nomes poderiam ter pequenas variações (erros de digitação, abreviações, etc.). Para resolver isso,izei a técnica de **Fuzzy Join**:

* O código utiliza a biblioteca `fuzzywuzzy` para calcular a similaridade entre os nomes das duas tabelas.
* A função `fuzzy_join` itera sobre a lista de presença (`df1`), buscando a melhor correspondência para cada nome na lista de votação (`df2`) usando `process.extractOne`.
* Se a similaridade (`fuzz.token_sort_ratio`) for maior que um limite (`threshold=90`), a correspondência é considerada válida.
* Por fim, as tabelas são unidas (`merge`) usando os índices das correspondências, garantindo que mesmo nomes ligeiramente diferentes sejam corretamente associados.

### 4. Exportação e Resultados
Após unificar as tabelas, o resultado foi exportado para um arquivo .xlsx. A tabela final foi rica em informações, permitindo cruzar a presença com o voto de cada pessoa. Isso gerou insights valiosos para o cliente, que pôde utilizar os dados de forma confiável em uma audiência. O projeto demonstra a capacidade de transformar dados desestruturados em informações úteis e prontas para uso em negócios.