---
title: Design base de dados de conhecimento-QnA Maker
description: Saiba como criar uma base de dados de conhecimento-QnA Maker.
ms.topic: conceptual
ms.date: 01/27/2020
ms.openlocfilehash: 86e176a378d02d294e0123aba0d2e08d99b044fe
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91330648"
---
# <a name="question-and-answer-pair-concepts"></a>Conceitos do par de perguntas e respostas

Uma base de dados de conhecimento consiste em pares de perguntas e respostas (QnA).  Cada par tem uma resposta e um par contém todas as informações associadas a essa _resposta_. Uma resposta pode se assemelhar livremente a uma linha de banco de dados ou a uma instância de estrutura de dado.

## <a name="question-and-answer-pairs"></a>Pares de pergunta e resposta

As configurações **necessárias** em um par de QnA (perguntas e respostas) são:

* um texto de **pergunta** da consulta do usuário, usado para o aprendizado de máquina de QnA Maker, para se alinhar com o texto da pergunta do usuário com palavras diferentes, mas a mesma resposta
* a **resposta-a resposta do** par é a resposta retornada quando uma consulta de usuário é correspondida com a pergunta associada

Cada par é representado por uma **ID**.

As configurações **opcionais** para um par incluem:

* **Formas alternativas da pergunta** – isso ajuda a QnA Maker retornar a resposta correta para uma variedade maior de frases de pergunta
* **Metadados**: os metadados são marcas associadas a um par de QnA e são representados como pares de chave-valor. As marcas de metadados são usadas para filtrar pares QnA e limitar o conjunto sobre o qual a correspondência da consulta será executada.
* **Prompts de múltipla ativação**, usados para continuar uma conversa com vários folheios

![Bases de conhecimento do QnA Maker](../media/qnamaker-concepts-knowledgebase/knowledgebase.png)

## <a name="editorially-add-to-knowledge-base"></a>Adicionar de modo editorial à base de dados de conhecimento

Se você não tiver conteúdo pré-existente para preencher a base de dados de conhecimento, poderá adicionar pares de QnA editorial no portal de QnA Maker. Saiba como atualizar sua base de dados de conhecimento [aqui](../How-To/edit-knowledge-base.md).

## <a name="editing-your-knowledge-base-locally"></a>Como editar sua base de dados de conhecimento localmente

Após criar uma base de conhecimento, é recomendável que você faça as edições no texto da base de conhecimento no [portal QnA Maker](https://qnamaker.ai), em vez de exportar e reimportar por meio de arquivos locais. No entanto, pode haver ocasiões em que você precisa editar uma base de conhecimento localmente.

Exporte a base de conhecimento na página **Configurações** e, em seguida, edite-a com o Microsoft Excel. Se você optar por usar outro aplicativo para editar o arquivo exportado, o aplicativo poderá introduzir erros de sintaxe porque não é compatível totalmente com TSV. Em geral, os arquivos TSV do Microsoft Excel não introduzem nenhum erro de formatação.

Depois de concluir as edições, reimporte o arquivo TSV na página **Configurações**. Isso substitui totalmente a base de conhecimento atual pela base de conhecimento importada.

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Ciclo de vida da base de dados de conhecimento no QnA Maker](./development-lifecycle-knowledge-base.md)