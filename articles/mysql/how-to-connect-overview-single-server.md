---
title: Conexão e consulta-servidor único MySQL
description: Links para os guias de início rápido do banco de dados SQL do Azure mostrando como se conectar ao servidor e executar consultas.
services: mysql
ms.service: mysql
ms.topic: how-to
author: mksuni
ms.author: sumuth
ms.date: 09/22/2020
ms.openlocfilehash: 229011f11ad6898555f59b063910d80a679070e0
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90933742"
---
# <a name="connect-and-query-overview-for-azure-database-for-mysql--single-server"></a>Visão geral de conexão e consulta do banco de dados do Azure para MySQL-servidor único
O documento a seguir inclui links para exemplos que mostram como se conectar e consultar com o banco de dados do Azure para MySQL servidor único. Este guia também inclui as recomendações e bibliotecas de TLS que você pode usar para se conectar ao servidor em idiomas com suporte abaixo.

## <a name="quickstarts"></a>Inícios rápidos

| Guia de Início Rápido | Descrição |
|---|---|
|[MySQL Workbench](connect-workbench.md)|Este guia de início rápido demonstra como usar o cliente MySQL Workbench para se conectar a um banco de dados. Em seguida, você pode usar instruções do MySQL para consultar, inserir, atualizar e excluir dados no banco de dado.|
|[Azure Cloud Shell](https://docs.microsoft.com/azure/mysql/quickstart-create-mysql-server-database-using-azure-cli#connect-to-azure-database-for-mysql-server-using-mysql-command-line-client)|Este artigo mostra como executar **mysql.exe** no [Azure cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) para se conectar ao servidor e, em seguida, executar instruções para consultar, inserir, atualizar e excluir dados no banco de dado.|
|[MySQL com Visual Studio](https://www.mysql.com/why-mysql/windows/visualstudio)|Você pode usar o MySQL para Visual Studio para se conectar ao servidor MySQL. O MySQL para Visual Studio se integra diretamente ao Gerenciador de Servidores facilitando a configuração de novas conexões e o trabalho com objetos de banco de dados.|
|[PHP](connect-php.md)|Este guia de início rápido demonstra como usar o PHP para criar um programa a fim de se conectar a um banco de dados e usar instruções do MySQL para consultar os dados.|
|[Java](connect-java.md)|Este guia de início rápido demonstra como usar o Java para se conectar a um banco de dados e, em seguida, usar instruções do MySQL para consultar os dados.|
|[Node.js](connect-nodejs.md)|Este guia de início rápido demonstra como usar Node.js para criar um programa a fim de se conectar a um banco de dados e usar instruções do MySQL para fazer a consulta.|
|[.NET (C#)](connect-csharp.md)|Este guia de início rápido demonstra como use.NET (C#) para criar um programa em C# a fim de se conectar a um banco de dados e usar instruções do MySQL para consultar os dados.|
|[Go](connect-go.md)|Este guia de início rápido demonstra como usar o Go para se conectar a um banco de dados. As instruções Transact-SQL para consultar e modificar dados também são demonstradas.|
|[Python](connect-python.md)|Este guia de início rápido demonstra como usar o Python para conectar-se a um banco de dados e usar instruções do MySQL para consultas de consulta. |
|[Ruby](connect-ruby.md)|Este guia de início rápido demonstra como usar o Ruby para criar um programa a fim de se conectar a um banco de dados e usar instruções do MySQL para fazer consultas de dado.|
|[C++](connect-cpp.md)|Este guia de início rápido demonstra como usar o C++ + para criar um programa para se conectar a um banco de dados e usar o Query Data.|


## <a name="tls-considerations-for-database-connectivity"></a>Considerações sobre o TLS para a conectividade de banco de dados

O protocolo TLS é usado por todos os drivers que a Microsoft fornece ou dá suporte para conectar-se a bancos de dados no Azure Database para MySQL. Nenhuma configuração especial é necessária, mas impor o TLS 1,2 para servidores recém-criados. Recomendamos se você estiver usando o TLS 1,0 e 1,1 e atualizar a versão do TLS para seus servidores. Consulte [ como configurar o TLS](howto-tls-configurations.md)


## <a name="libraries"></a>Bibliotecas

O Banco de Dados do Azure para MySQL usa a edição de comunidade mais popular do mundo do banco de dados MySQL. Portanto, ele é compatível com uma ampla variedade de linguagens de programação e drivers. A meta é dar suporte às três versões mais recentes de drivers MySQL e os esforços com autores da comunidade de software livre para melhorar constantemente a funcionalidade e usabilidade dos drivers MySQL continuam.

Veja quais [drivers](concepts-compatibility.md) são compatíveis com o banco de dados do Azure para MySQL servidor único. 


## <a name="next-steps"></a>Próximas etapas 
- [Migrar dados usando despejo e restauração](concepts-migrate-dump-restore.md)
- [Migrar dados usando importação e exportação](concepts-migrate-import-export.md)
