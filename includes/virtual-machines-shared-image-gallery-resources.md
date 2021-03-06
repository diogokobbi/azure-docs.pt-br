---
title: incluir arquivo
description: incluir arquivo
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 05/04/2020
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 433e909563602a2ef32b7986959b428c9afaf9f4
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82788972"
---
| Recurso | Descrição|
|----------|------------|
| **Origem da imagem** | Este é um recurso que pode ser usado sozinho ou para criar uma **versão da imagem** em uma galeria de imagens. Uma origem de imagem pode ser uma VM do Azure existente [generalizada ou especializada](../articles/virtual-machines/windows/shared-image-galleries.md#generalized-and-specialized-images), uma imagem gerenciada, um instantâneo ou uma versão de imagem em outra galeria de imagens. |
| **Galeria de imagens** | Como o Azure Marketplace, uma **galeria de imagens** é um repositório para gerenciar e compartilhar imagens, mas você controla quem tem acesso. |
| **Definição da imagem** | As definições de imagem são criadas dentro de uma galeria e transportam informações sobre a imagem e os requisitos para usá-la internamente. Isso inclui se a imagem é Windows ou Linux, notas sobre a versão e requisitos mínimos e máximos de memória. É uma definição de um tipo de imagem. |
| **Versão da imagem** | Uma **versão da imagem** é usada para criar uma VM ao usar uma galeria. Você pode ter diversas versões de uma imagem conforme necessário para seu ambiente. Como uma imagem gerenciada, quando você usa uma **versão da imagem** para criar uma VM, a versão da imagem é usada para criar novos discos para a VM. Versões de imagem podem ser usadas várias vezes. |