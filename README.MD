**PROJETO E APLICAÇÃO EM MINERAÇÃO DE DADOS - CURSO CIÊNCIA DE DADOS**

Aluno: Tiago Augusto Ferreira

**Projeto: Classificação dos diários oficiais da cidade de Guaratiguetá/SP**

**Objetivo**

Tal projeto tem por objetivo realizar uma análise exploratória nos textos contidos nos diários oficiais dos dias 01/10/2020 a 19/11/2020 da cidade de Guaratinguetá visando classificá-los dentro dos seguintes grupos:

- Tipos:
  - licitação, pregão, concorrência, inexigibilidade, leilão, carta-convite, lei, orçamento, decreto, projeto, concurso, edital;
- Áreas:
  - educação, saúde, finanças, esporte, lazer, segurança, ambiente, social, mobilidade.

**Introdução**

A ideia surgiu devido ao projeto Querido Diário que tem por objetivo &quot;atualizar o Diário Oficial para a era digital, centralizando as informações atualmente disponíveis apenas em fontes distintas.&quot; ([https://github.com/okfn-brasil/querido-diario](https://github.com/okfn-brasil/querido-diario)). Neste projeto uma das etapas é a extração dos arquivos dos Diários Oficiais de todos os municípios brasileiros, e para isto conta com a ajuda de colaboradores no modelo Open Source.

A segunda etapa é organizar as informações destes documentos para que seja mais fácil encontrar informações relevantes dentro destas publicações.

**Metodologia**

O projeto consistirá das seguintes etapas:

![](RackMultipart20201202-4-1qffy1u_html_ba22d9951b401838.gif) ![](RackMultipart20201202-4-1qffy1u_html_ba22d9951b401838.gif) ![](RackMultipart20201202-4-1qffy1u_html_ba22d9951b401838.gif) ![](RackMultipart20201202-4-1qffy1u_html_ba22d9951b401838.gif) ![](RackMultipart20201202-4-1qffy1u_html_ba22d9951b401838.gif)

1- Extração dos PDFs de Outubro para a cidade de Guaratinguetá/SP

2 - Extração das imagens digitalizadas dos PDFs

3 - Conversão das imagens digitalizadas em textos e criação dos documentos txt

4 - Análise dos documentos com Python e SpaCy

5 - Documentos Classificados por área e tipo

**Etapa 01 – Extração dos PDFs**

Para extração dos PDFs foi utilizada a biblioteca Python Scrapy ([https://scrapy.org/](https://scrapy.org/)) bem como a estrutura já construída pelo projeto Querido Diário (para saber como contribuir acesse: [https://github.com/okfn-brasil/querido-diario/blob/main/CONTRIBUTING.md](https://github.com/okfn-brasil/querido-diario/blob/main/CONTRIBUTING.md)). Não entrarei em detalhes desta construção, porém, o código para a extração encontra-se neste link ([https://github.com/tiagofer/querido-diario/blob/main/data\_collection/gazette/spiders/sp\_guaratingueta.py](https://github.com/tiagofer/querido-diario/blob/main/data_collection/gazette/spiders/sp_guaratingueta.py)).

**Etapa 02 – Extração das Imagens digitalizadas dos PDFs**

Todos os arquivos obtidos da Prefeitura foram criados por meio da digitalização de documentos e não digitados, desta forma, fez-se necessário o uso de recursos um pouco mais sofisticados para a obtenção dos arquivos em texto para as análises. Para isto foram utilizadas as seguintes bibliotecas:

- PymuPDF: para ler os arquivos .pdf gerados pelo processo de mineração no site da prefeitura e salvar as imagens contidas neles;
- Pillow e OpenCV: para manipular as imagens extraídas;

Infelizmente, alguns documentos não retornam as imagens corretamente, o que fará com que nem todos os dias do mês possam ser tagueados. Uma melhoria no pré-processamento deverá ser efetuada em trabalhos futuros.

**Etapa 03 – Extração dos textos das imagens**

Nesta etapa do processo, as imagens contidas dentro do PDF já foram extraídas e precisam ser convertidas em texto. Para isto foi utilizada a biblioteca Python pytesseract ([https://pypi.org/project/pytesseract/](https://pypi.org/project/pytesseract/)) que é uma abstração para o pacote Tesseract ([https://github.com/tesseract-ocr/tesseract](https://github.com/tesseract-ocr/tesseract)).

Tal pacote consegue, por meio de visão computacional e deep learning, extrair textos contidos em imagens, algo primordial para o nosso estudo. O output deste processo é um arquivo com extensão _txt_ contendo todo o texto não tratado das imagens.

**Etapa 04 – Análise dos documentos com Spacy**

O Spacy é uma biblioteca poderosíssima para processamento de linguagem natural (NLP) e disponível no Python. Com ela, conseguimos qualificar elementos do nosso texto de acordo com a sua classe gramatical, estrutura hierárquica dentro da frase, dentre muitas outras possibilidades. Para o nosso caso, a aplicação do Spacy será para comparar se um conjunto de palavras chave pré definidas estará presente ou não dentro do texto. Neste processo ambos serão separados em _tokens_ e uma comparação será feita para avaliar a aderência dos tipos e áreas aos textos dos diários oficiais. Em trabalhos futuros, pretende-se aplicar a análise de sentenças maiores, visando identificar contextos mais amplos nos conteúdos.

**Etapa 05 – Documentos Classificados**

Ao final do processo, a aplicação entrega um conjunto de tags para cada diário oficial analisado, facilitando a busca por informação para a população em geral. Para a classificação, foi utilizado o método _similarity_ da biblioteca Spacy, utilizando-se como limiar o valor de 0.9, ou seja, quando a similaridade é maior ou igual a tal quantia, considera-se que existe convergência entre a tag e o token pesquisado, então esta tag é adicionada à listagem da categoria correspondente.

Como resultado, observamos que o sistema conseguiu classificar bem os diários, trazendo as tags corretas para 100% dos casos que obtiveram a correta conversão de textos para imagem. Abaixo, uma visão geral da quantidade de tags por categoria do período analisado.

![](RackMultipart20201202-4-1qffy1u_html_94e9bdbef2263e09.gif)

![](RackMultipart20201202-4-1qffy1u_html_8eaba62de929b46d.png)
_Figura 1: Total de Tags por Categoria_

**Conclusão**

Tal estudo teve como objetivo a utilização de técnicas de processamento de imagens e deep learning para classificar os diários oficiais da cidade de Guaratinguetá conforme um grupo de categorias previamente definidos. O projeto ainda carece de melhorias, principalmente no tocante à conversão das imagens em texto, para este caso, duas publicações não foram identificadas e algumas apresentaram problemas em páginas do documento, porém, tais problemas não comprometeram o objetivo final do estudo.

Como uma segunda evolução, tal sistema poderia estar integrado a alguma plataforma web, onde toda a população da cidade pudesse realizar buscas dentro destes documentos, ou realizando filtros pelas categorias geradas, algo que não é possível no site da prefeitura.

**Bibliografia**

PYTESSERACT. [_S. l._]. Disponível em: https://pypi.org/project/pytesseract/. Acesso em: 30 nov. 2020.

Biblioteca Spacy. [_S. l._]. Disponível em: [https://spacy.io/usage/linguistic-features](https://spacy.io/usage/linguistic-features). Acesso em: 30 nov. 2020.

Tratamento de Imagens. [_S. l._]. Disponível em: [https://nanonets.com/blog/ocr-with-tesseract/](https://nanonets.com/blog/ocr-with-tesseract/). Acesso em: 30 nov. 2020.

Sumarização de Textos. [_S. l._]. Disponível em: [https://jcharistech.wordpress.com/2018/12/31/text-summarization-using-spacy-and-python/](https://jcharistech.wordpress.com/2018/12/31/text-summarization-using-spacy-and-python/). Acesso em: 30 nov. 2020.

Querido Diário. [_S. l._]. Disponível em: [https://github.com/okfn-brasil/querido-diario](https://github.com/okfn-brasil/querido-diario). Acesso em: 30 nov. 2020.

DATA Science para Negócios:: O que você precisa saber sobre mineração de dados e pensamento analítico de dados. 1. ed. [_S. l._:_s. n._], 2016.