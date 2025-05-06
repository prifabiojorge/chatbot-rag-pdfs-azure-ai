# Chatbot com RAG (Retrieval-Augmented Generation) no Azure AI Studio

## Visão Geral do Desafio

Este projeto foi desenvolvido como parte de um desafio prático da Digital Innovation One (DIO), visando explorar conceitos de IA generativa, embeddings e busca vetorial para criar um chatbot capaz de responder perguntas com base em conteúdo específico de arquivos PDF.

A ideia central é construir um sistema de assistência virtual que utilize um conjunto de documentos proprietários (como artigos de TCC) como fonte de conhecimento, em vez de depender unicamente do conhecimento geral de modelos pré-treinados.

## Cenário do Projeto

Seguindo o cenário proposto, imaginei a necessidade de um estudante de Engenharia de Software que precisa revisar e correlacionar diversos artigos científicos para seu Trabalho de Conclusão de Curso (TCC). A dificuldade em gerenciar e extrair informações relevantes de múltiplos documentos levou à busca por uma solução baseada em inteligência artificial para facilitar a pesquisa e organização das informações.

## Objetivos (Propostos e Alcançados)

Os objetivos originais do desafio eram:

✅ Carregar arquivos PDF contendo informações relevantes.
✅ Implementar um sistema de busca vetorial para indexar e recuperar informações dos PDFs.
✅ Utilizar inteligência artificial (modelo de linguagem) para gerar respostas baseadas no conteúdo dos documentos carregados.
✅ Desenvolver um chat interativo para realizar perguntas e obter respostas contextuais fundamentadas nos arquivos.

Durante a execução do projeto, consegui configurar o ambiente e os componentes necessários para a abordagem de RAG (Retrieval-Augmented Generation), mas encontrei desafios na etapa de ingestão e indexação dos dados. Portanto, os objetivos de ter um chatbot plenamente funcional respondendo com base nos *meus* PDFs não foi totalmente alcançado neste momento, mas a maior parte da infraestrutura foi configurada e o processo de RAG compreendido e implementado até o ponto da falha.

## Abordagem Técnica: Azure AI Studio e Padrão RAG

Para este projeto, utilizei a plataforma **Azure AI Foundry**, que oferece ferramentas integradas para o ciclo de vida de IA generativa, incluindo a configuração de RAG.

A abordagem seguiu o padrão RAG:

1.  **Fonte de Dados:** Meus arquivos PDF (destinados a serem a base de conhecimento).
2.  **Indexação e Busca Vetorial:** Utilização do **Azure AI Search** para criar um índice vetorial dos embeddings gerados a partir do conteúdo dos PDFs.
3.  **Modelo de Embedding:** Um modelo específico (como `text-embedding-3-large` ou `text-embedding-ada-002`) para converter o texto dos documentos em vetores numéricos.
4.  **Modelo de Linguagem (LLM):** Um modelo poderoso (como `gpt-4` ou `gpt-35-turbo`) para gerar as respostas.
5.  **Orquestração (RAG):** Configuração no **Playground de Chat** do Azure AI Studio para que, ao receber uma pergunta, o sistema primeiro busque no índice as informações relevantes dos PDFs e inclua essas informações como contexto no prompt enviado ao LLM para gerar a resposta final.

## Passos Executados no Azure AI Studio

Segui os seguintes passos para configurar o ambiente e a solução:

1.  **Criação do Hub e Projeto de IA do Azure:** Configurei um novo Hub de IA e, dentro dele, um Projeto de IA (Ex: `proj-dio-dois`) para organizar os recursos.
2.  **Criação do Serviço Azure AI Search:** Criei uma instância do serviço Azure AI Search (Ex: `diosearchfabuloso`) no Portal do Azure para ser o back-end do meu índice vetorial.
3.  **Implantação dos Modelos (Embedding e LLM):** Implantei os modelos necessários para gerar embeddings e para a conclusão de chat no meu Recurso de IA conectado (Azure OpenAI). Modelos como `text-embedding-3-large` (ou similar) e `gpt-4.1` (ou similar) foram implantados.
4.  **Tentativa de Adicionar Dados e Indexar PDFs:** Dentro do Projeto de IA, acessei a seção "Dados + índices" para carregar meus arquivos PDF e criar o índice no serviço Azure AI Search. Configurei a conexão com o modelo de embedding para a busca vetorial.

## Desafios Encontrados: Falha na Ingestão dos Dados

Durante a etapa de adição e indexação dos dados (passo 4), enfrentei falhas persistentes.

Inicialmente, deparei-me com um erro relacionado à extensão do arquivo:
`Exception: None of the provided file extensions are supported. List of supported file extensions is ['.txt', '.md', '.html', '.htm', '.py', '.pdf', ...] Skipped extensions: { "": 1 }`
Isso indicava que o arquivo carregado não estava com a extensão `.pdf` corretamente identificada.

Após corrigir a extensão do arquivo e tentar novamente, encontrei um novo erro:
`Job failed... Error: {"Error":{"Code":"UserError","Message":"...Request timeout|...Connection failed when trying to access the stream..."}}`
Este log sugere um problema ao acessar o arquivo, possivelmente um timeout ou falha de conexão ao tentar ler o conteúdo do PDF a partir da fonte de dados durante o processo de ingestão. Apesar de tentar novamente, não consegui superar este problema de acesso aos dados para que a indexação fosse concluída com sucesso.

![image (6)](https://github.com/user-attachments/assets/60ee94b0-b289-4eef-8034-ad612d7b25b5)


Como resultado da falha na ingestão, o índice vetorial com o conteúdo dos meus PDFs não foi populado corretamente. Ao configurar e testar o Playground de Chat com a opção de "Fundamentação" (Grounding) usando este índice, o modelo de linguagem (GPT) não conseguiu encontrar o conteúdo dos meus documentos para responder, como demonstrado na imagem abaixo:

![image](https://github.com/user-attachments/assets/e4ca042e-1e13-4290-8e16-93f587ed03f1)

## Atendendo aos Requisitos Específicos do Desafio DIO

Apesar do desafio técnico na ingestão dos dados, os seguintes requisitos do desafio da DIO foram atendidos ou abordados:

*   **Crie um novo repositório no github:** Este repositório atual (`[Link para o seu repositório]`) foi criado para a entrega.
*   **Crie uma pasta chamada 'inputs' e crie um documento de texto com algumas sentenças:** A pasta `inputs` foi criada e contém um arquivo `sample.txt` com algumas sentenças, conforme solicitado. (Note: Embora o foco principal fosse PDF, esta instrução específica foi cumprida).
*   **Crie um arquivo chamado readme.md:** Este arquivo.
*   **Deixe alguns prints:** Capturas de tela do processo no Azure AI Studio foram/serão incluídas.
*   **Descreva o processo:** O processo de configuração no Azure AI Studio e a abordagem RAG foram descritos acima.
*   **Alguns insights e possibilidades:** Incluídos na seção abaixo.

## Insights e Aprendizados

Este projeto prático, mesmo encontrando um obstáculo técnico, proporcionou aprendizados valiosos:

*   **Compreensão do Padrão RAG:** Aprofundei o entendimento sobre como funciona o padrão RAG para "fundamentar" modelos de linguagem com dados externos, e a importância de cada componente (fonte de dados, indexação, embeddings, LLM e orquestração).
*   **Exploração do Azure AI Studio:** Ganhei familiaridade com a interface e os recursos do Azure AI Studio para criar projetos de IA generativa, implantar modelos e configurar fluxos de RAG.
*   **Importância da Preparação e Acesso aos Dados:** O problema encontrado na ingestão reforçou a criticidade da etapa de preparação dos dados e, principalmente, de garantir que o serviço de ingestão tenha as permissões e o acesso de rede adequados para ler os arquivos da fonte configurada. Problemas na origem dos dados podem impedir todo o fluxo de RAG.
*   **Diagnóstico de Erros:** A necessidade de analisar logs de erro para identificar a causa raiz dos problemas é uma habilidade fundamental no desenvolvimento de sistemas.

## Possibilidades Futuras

Para evoluir este projeto, as próximas etapas incluiriam:

1.  Diagnosticar e resolver o problema de "Request timeout" na ingestão dos dados para que o índice no Azure AI Search seja populado com sucesso.
2.  Refinar a mensagem do sistema e os parâmetros no Playground de Chat para otimizar as respostas do modelo com base nos PDFs.
3.  Explorar a integração deste chat com uma interface web ou aplicativo para um uso mais amigável (por exemplo, utilizando o recurso "Aplicativos Web" do Azure AI Studio, se disponível para este tipo de cenário).
4.  Indexar outros tipos de documentos além de PDFs.
5.  Implementar métricas de avaliação (automatizadas e/ou manuais) para medir a qualidade das respostas do chatbot.

Este projeto foi um passo importante na jornada de aprendizado sobre IA generativa e a utilização de plataformas MLOps para construí-la.

---

## Autor

Fábio Jorge de Nazaré Ferreira
---
