---
title: Notas de versão – serviço de fala
titleSuffix: Azure Cognitive Services
description: Um log em execução de versões de recursos do Speech Service, melhorias, correções de bugs e problemas conhecidos.
services: cognitive-services
author: oliversc
manager: jhakulin
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 08/17/2020
ms.author: oliversc
ms.custom: seodec18
ms.openlocfilehash: 94947499452c7f1b8515fee56996b13120232f34
ms.sourcegitcommit: 4a7a4af09f881f38fcb4875d89881e4b808b369b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89462370"
---
# <a name="speech-service-release-notes"></a>Notas de versão do Serviço de Fala

## <a name="text-to-speech-2020-august-release"></a>Conversão de texto em fala 2020 – versão de agosto

### <a name="new-features"></a>Novos recursos

* **TTS de neural: novo estilo de fala para `en-US` Voz do Aria**. AriaNeural pode parecer um Newscaster ao ler notícias. O estilo ' newscast-formal ' parece mais sério, enquanto o estilo ' newscast-casual ' é mais relaxado e informal. Veja [como usar os estilos de fala em SSML](speech-synthesis-markup.md).

* **Voz personalizada: um novo recurso é liberado para verificar automaticamente a qualidade dos dados de treinamento**. Quando você carregar seus dados, o sistema examinará vários aspectos de seus dados de áudio e transcrição e corrigirá ou filtrará automaticamente os problemas para melhorar a qualidade do modelo de voz. Isso abrange o volume de áudio, o nível de ruído, a precisão da pronúncia da fala, o alinhamento da fala com o texto normalizado, silêncio no áudio, além do formato de áudio e de script. 

* **Criação de conteúdo de áudio: um conjunto de novos recursos para habilitar o ajuste de voz mais poderoso e recursos de gerenciamento de áudio**.

    * Pronúncia: o recurso de ajuste de pronúncia é atualizado para o conjunto mais recente de fonema. Você pode escolher o elemento fonema correto da biblioteca e refinar a pronúncia das palavras selecionadas. 

    * Download: o recurso de "download"/"exportação" de áudio é aprimorado para dar suporte à geração de áudio por parágrafo. Você pode editar o conteúdo no mesmo arquivo/SSML, enquanto gera várias saídas de áudio. A estrutura de arquivos de "download" também é refinada. Agora, você pode facilmente obter todos os áudios em uma pasta. 

    * Status da tarefa: a experiência de exportação de vários arquivos foi aprimorada. Quando você exportar vários arquivos no passado, se um dos arquivos falhar, a tarefa inteira falhará. Mas, agora, todos os outros arquivos serão exportados com êxito. O relatório de tarefas é aprimorado com informações mais detalhadas e estruturadas. Você pode verificar os logs de todos os arquivos com falha e sentenças agora com o relatório. 

    * Documentação do SSML: vinculada ao documento SSML para ajudá-lo a verificar as regras de como usar todos os recursos de ajuste.

* **A API de lista de voz é atualizada para incluir um nome de exibição amigável do usuário e os estilos de fala com suporte para vozes neurais**.

### <a name="general-tts-voice-quality-improvements"></a>Melhorias gerais de qualidade de voz TTS

* Redução do erro de pronúncia no nível de palavra% para `ru-RU` (erros reduzidos por 56%) e `sv-SE` (erros reduzidos por 49%)

* Melhoria da leitura de palavras Polyphony em `en-US` vozes neurais por 40%. Exemplos de palavras polyphonys incluem "Read", "Live", "content", "Record", "Object" etc. 

* Melhoria da naturalidade do Tom da pergunta no `fr-FR` . MOS (Pontuação média de opinião) ganho: + 0,28

* Atualização do vocoders para as seguintes vozes, com melhorias de fidelidade e velocidade geral de desempenho em 40%.

    | Localidade | Voz |
    |---|---|    
    | `en-GB` | Mia |
    | `es-MX` | Dalia |
    | `fr-CA` | Sylvie |
    | `fr-FR` | Denise |
    | `ja-JP` | Nanami |
    | `ko-KR` | Sol-Hi |

### <a name="bug-fixes"></a>Correções de bug

* Correção de vários bugs com a ferramenta de criação de conteúdo de áudio 
    * Corrigido o problema com a atualização automática. 
    * Correção de problemas com estilos de voz em zh-CN na região do Sul Ásia Oriental.
    * Correção do problema de estabilidade, incluindo um erro de exportação com a marca ' break ' e erros em pontuações.    

## <a name="new-speech-to-text-locales-2020-august-release"></a>Novas localidades de fala em texto: 2020 – versão de agosto
A conversão de fala em texto lançou 26 novas localidades em agosto: 2 idiomas europeus `cs-CZ` e `hu-HU` 5 localidades em inglês e 19 localidades do espanhol que abrangem a maioria dos países da América do Sul. Abaixo está uma lista das novas localidades. Consulte a lista completa de idiomas [aqui](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support).

| Local  | Linguagem                          |
|---------|-----------------------------------|
| `cs-CZ` | Tcheco (República Tcheca)            | 
| `en-HK` | Inglês (Hong Kong)               | 
| `en-IE` | Inglês (Irlanda)                 | 
| `en-PH` | Inglês (Filipinas)             | 
| `en-SG` | Inglês (Singapura)               | 
| `en-ZA` | Inglês (África do Sul)            | 
| `es-AR` | Espanhol (Argentina)               | 
| `es-BO` | Espanhol (Bolívia)                 | 
| `es-CL` | Espanhol (Chile)                   | 
| `es-CO` | Espanhol (Colômbia)                | 
| `es-CR` | Espanhol (Costa Rica)              | 
| `es-CU` | Espanhol (Cuba)                    | 
| `es-DO` | Espanhol (República Dominicana)      | 
| `es-EC` | Espanhol (Equador)                 | 
| `es-GT` | Espanhol (Guatemala)               | 
| `es-HN` | Espanhol (Honduras)                | 
| `es-NI` | Espanhol (Nicarágua)               | 
| `es-PA` | Espanhol (Panamá)                  | 
| `es-PE` | Espanhol (Peru)                    | 
| `es-PR` | Espanhol (Porto Rico)             | 
| `es-PY` | Espanhol (Paraguai)                | 
| `es-SV` | Espanhol (El Salvador)             | 
| `es-US` | Espanhol (EUA)                     | 
| `es-UY` | Espanhol (Uruguai)                 | 
| `es-VE` | Espanhol (Venezuela)               | 
| `hu-HU` | Húngaro (Hungria)               | 


## <a name="speech-sdk-1130-2020-july-release"></a>SDK de fala 1.13.0:2020 – versão de julho

**Observação**: o SDK do Speech no Windows depende do Microsoft Visual C++ compartilhado redistribuível para o Visual Studio 2015, 2017 e 2019. Baixe-o e instale-o [aqui](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads).

**Novos recursos**
- **C#**: suporte adicionado para transcrição de conversa assíncrona. Consulte a documentação [aqui](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-async-conversation-transcription).  
- **JavaScript**: adicionado suporte a reconhecimento do locutor para o [navegador](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript/browser/speaker-recognition) e [node.js](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript/node/speaker-recognition).
- **JavaScript**: suporte adicionado para detecção automática de idioma/ID de idioma. Consulte a documentação [aqui](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-automatic-language-detection?pivots=programming-language-javascript).
- **Objective-C**: suporte adicionado para [conversa com vários dispositivos](https://docs.microsoft.com/azure/cognitive-services/speech-service/multi-device-conversation) e [transcrição de conversa](https://docs.microsoft.com/azure/cognitive-services/speech-service/conversation-transcription). 
- **Python**: suporte de áudio compactado adicionado para Python no Windows e Linux. Consulte a documentação [aqui](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-use-codec-compressed-audio-input-streams). 

**Correções de bug**
- **Tudo**: Corrigido um problema que fazia com que o KeywordRecognizer não avançasse os fluxos após um reconhecimento.
- **Todos**: Corrigido um problema que fazia com que o fluxo fosse obtido de um KeywordRecognitionResult para não conter a palavra-chave.
- **Tudo**: Corrigido um problema em que o SendMessageAsync não envia realmente a mensagem pela conexão depois que os usuários terminam de esperar.
- **Todos**: correção de uma falha em APIs reconhecimento do locutor quando os usuários chamam o método VoiceProfileClient:: SpeakerRecEnrollProfileAsync várias vezes e não aguardaram que as chamadas sejam concluídas.
- **Todos**: habilitar o registro em log de arquivo fixo em classes VoiceProfileClient e SpeakerRecognizer.
- **JavaScript**: Corrigido um [problema](https://github.com/microsoft/cognitive-services-speech-sdk-js/issues/74) com a limitação quando o navegador é minimizado.
- **JavaScript**: Corrigido um [problema](https://github.com/microsoft/cognitive-services-speech-sdk-js/issues/78) com um vazamento de memória em fluxos.
- **JavaScript**: cache adicionado para respostas de OCSP de NodeJS.
- **Java**: Corrigido um problema que estava fazendo com que os campos BigInteger sempre retornassem 0.
- **Ios**: Corrigido um [problema](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/702) com a publicação de aplicativos baseados no SDK da fala na iOS App Store.

**Amostras**
- **C++**: foi adicionado um código de exemplo para reconhecimento do locutor [aqui](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/cpp/windows/console/samples/speaker_recognition_samples.cpp).

**Teste do COVID-19 resumida:** Devido ao trabalho remoto nas últimas semanas, não poderíamos fazer tantos testes de verificação manual como normalmente. Não fizemos nenhuma alteração que achamos que poderia ter quebrado alguma coisa e nossos testes automatizados passaram. No caso improvável de não ter perdido algo, informe-nos no [GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues?q=is%3Aissue+is%3Aopen).<br>
Mantenha-se íntegro!

## <a name="text-to-speech-2020-july-release"></a>Conversão de texto em fala 2020-versão de julho

### <a name="new-features"></a>Novos recursos

* **TTS de neural, 15 novas vozes neurais**: as novas vozes adicionadas ao portfólio de TTS do neural são Salma em `ar-EG` árabe (Egito), Zariyah em `ar-SA` árabe (Arábia Saudita), Alba in `ca-ES` Catalão (Espanha), Christel em `da-DK` dinamarquês (Dinamarca), Neerja em `es-IN` Inglês (Índia), Noora em `fi-FI` finlandês (Finlândia), swara em `hi-IN` híndi (Índia), Colette em `nl-NL` holandês (Holanda), Zofia em `pl-PL` polonês (Polônia), Fernanda em `pt-PT` Português (Portugal), Dariya em `ru-RU` russo (Rússia), Hillevi em `sv-SE` sueco (Suécia), achara em `th-TH` tailandês (Tailândia), HiuGaai em `zh-HK` chinês (Cantonês, tradicional) e HsiaoYu em `zh-TW` chinês (dólar taiwanês mandarim). Verifique todos os [idiomas com suporte](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#neural-voices).  

* **Voz personalizada, testes de voz simplificados com o fluxo de treinamento para simplificar a experiência do usuário**: com o novo recurso de teste, cada voz será testada automaticamente com um conjunto de teste predefinido otimizado para cada idioma para cobrir cenários de assistente de voz e geral. Esses conjuntos de testes são cuidadosamente selecionados e testados para incluir casos de uso típicos e fonemas na linguagem. Além disso, os usuários ainda podem optar por carregar seus próprios scripts de teste ao treinar um modelo.

* **Criação de conteúdo de áudio: um conjunto de novos recursos é lançado para permitir um ajuste de voz mais poderoso e recursos de gerenciamento de áudio**

    * `Pitch`, `rate` e `volume` são aprimorados para dar suporte ao ajuste com um valor predefinido, como lento, médio e rápido. Agora, é simples para os usuários escolherem um valor "constante" para a edição de áudio.

    ![Ajuste de áudio](media/release-notes/audio-tuning.png)

    * Os usuários agora podem examinar o `Audio history` seu arquivo de trabalho. Com esse recurso, os usuários podem facilmente acompanhar todo o áudio gerado relacionado a um arquivo de trabalho. Eles podem verificar a versão do histórico e comparar a qualidade durante o ajuste ao mesmo tempo. 

    ![Histórico de áudio](media/release-notes/audio-history.png)

    * Agora, o `Clear` recurso é mais flexível. Os usuários podem limpar um parâmetro de ajuste específico e manter outros parâmetros disponíveis para o conteúdo selecionado.  

    * Um vídeo do tutorial foi adicionado na [página de aterrissagem](https://speech.microsoft.com/audiocontentcreation) para ajudar os usuários a começar rapidamente com o gerenciamento de áudio e o ajuste de voz TTS. 

### <a name="general-tts-voice-quality-improvements"></a>Melhorias gerais de qualidade de voz TTS

* Vocoder de TTS aprimorados em para maior fidelidade e menor latência.

    * Atualização de Elsa em `it-IT` para um novo vocoder que obteve o ganho de + 0,464 CMOS (Pontuação de opinião média comparativa) em qualidade de voz, 40% mais rápido em síntese e 30% de redução na latência do primeiro byte. 
    * Xiaoxiao atualizado `zh-CN` para o novo vocoder com o CMOS de + 0148 para o domínio geral, + 0,348 para o estilo newscast e + 0,195 para o estilo Lyrical. 

* `de-DE`Modelos de `ja-JP` voz e atualizados para tornar a saída de TTS mais natural.
    
    * Katja atualizado `de-DE` com o método de modelagem Prosody mais recente, o ganho (Pontuação média de opinião) é + 0,13. 
    * Atualizado o Nanami em `ja-JP` com um novo modelo Prosody de ênfase de Tom, o ganho (Pontuação média de opinião) é + 0,19;  

* Precisão de pronúncia de nível de palavra aprimorada em 5 idiomas.

    | Idioma | Redução de erros de pronúncia |
    |---|---|
    | `en-GB` | 51% |
    | `ko-KR` | 17% |
    | `pt-BR` | 39% |
    | `pt-PT` | 77% |
    | `id-ID` | 46% |

### <a name="bug-fixes"></a>Correções de bug

* Leitura de moeda
    * Corrigido o problema com a leitura de moeda para `es-ES` e `es-MX`
     
    | Idioma | Entrada | Leitura após aperfeiçoamento |
    |---|---|---|
    | `es-MX` | $1.58 | un peso Cincuenta y Ocho centavos |
    | `es-ES` | $1.58 | Dólar Cincuenta y Ocho centavos |

    * Suporte para moeda negativa (como "-$325") nas seguintes localidades: `en-US` ,,,,, `en-GB` `fr-FR` `it-IT` `en-AU` `en-CA` .

* Leitura de endereço aprimorada no `pt-PT` .
* Correção `en-AU` dos problemas de pronúncia Natasha () e Libby ( `en-UK` ) na palavra "for" e "quatro".  
* Correção de bugs na ferramenta de criação de conteúdo de áudio
    * A pausa adicional e inesperada depois que o segundo parágrafo é corrigido.  
    * O recurso ' no break ' é adicionado novamente de um bug de regressão. 
    * O problema de atualização aleatória do Speech Studio é fixo.  

### <a name="samplessdk"></a>Exemplos/SDK

* JavaScript: corrige o problema de reprodução no Firefox e no Safari no macOS e no iOS. 

## <a name="speech-sdk-1121-2020-june-release"></a>SDK de fala 1.12.1:2020 – versão de junho
**CLI de fala (também conhecida como SPX)**
-   Adicionados recursos de pesquisa na ajuda da CLI:
    -   `spx help find --text TEXT`
    -   `spx help find --topic NAME`
-   Atualizado para trabalhar com as APIs de Fala Personalizada e lote do v 3.0 implantadas recentemente:
    -   `spx help batch examples`
    -   `spx help csr examples`

**Novos recursos**
-   **C \# , C++**: reconhecimento do locutor Preview: esse recurso habilita a identificação do orador (que está falando?) e a verificação do palestrante (é isso que afirma ser?). Comece com uma [visão geral](https://docs.microsoft.com/azure/cognitive-services/Speech-Service/speaker-recognition-overview), leia o [artigo reconhecimento do locutor noções básicas](https://docs.microsoft.com/azure/cognitive-services/speech-service/speaker-recognition-basics)ou os [documentos de referência de API](https://docs.microsoft.com/rest/api/speakerrecognition/).

**Correções de bug**
-   **C \# , C++**: a gravação fixa do microfone não estava funcionando em 1,12 no reconhecimento do viva-voz.
-   **JavaScript**: correções de conversão de texto em fala no Firefox e Safari no MacOS e Ios.
-   Correção da falha na violação de acesso do verificador de aplicativos do Windows na transcrição de conversa ao usar o fluxo de 8 canais.
-   Correção da falha na violação de acesso do verificador de aplicativos do Windows na tradução de conversa de vários dispositivos.

**Amostras**
-   **C#**: [exemplo de código](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/csharp/dotnet/speaker-recognition) para reconhecimento de palestrante.
-   **C++**: [exemplo de código](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/cpp/windows/speaker-recognition) para reconhecimento de palestrante.
-   **Java**: [exemplo de código](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/java/android/intent-recognition) para reconhecimento de intenção no Android. 

**Teste do COVID-19 resumida:** Devido ao trabalho remoto nas últimas semanas, não poderíamos fazer tantos testes de verificação manual como normalmente. Não fizemos nenhuma alteração que achamos que poderia ter quebrado alguma coisa e nossos testes automatizados passaram. No caso improvável de não ter perdido algo, informe-nos no [GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues?q=is%3Aissue+is%3Aopen).<br>
Mantenha-se íntegro!


## <a name="speech-sdk-1120-2020-may-release"></a>SDK de fala 1.12.0:2020 – lançamento de maio
**CLI de fala (também conhecido como SPX)**
- O **SPX** é uma nova ferramenta de linha de comando que permite executar reconhecimento, síntese, tradução, transcrição de lote e gerenciamento de fala personalizado na linha de comando. Use-o para testar o serviço de fala ou para gerar script das tarefas de serviço de fala que você precisa executar. Baixe a ferramenta e leia a documentação [aqui](https://docs.microsoft.com/azure/cognitive-services/speech-service/spx-overview).

**Novos recursos**
- **Go**: suporte ao novo idioma do Go para [reconhecimento de fala](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/speech-to-text-from-microphone?pivots=programming-language-go) e [Assistente de voz personalizado](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/voice-assistants?pivots=programming-language-go). Configure seu ambiente de desenvolvimento [aqui](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?pivots=programming-language-go). Para obter o código de exemplo, consulte a seção de exemplos abaixo. 
- **JavaScript**: suporte adicionado ao navegador para conversão de texto em fala. Consulte a documentação [aqui](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/text-to-speech-audio-file?pivots=programming-language-JavaScript).
- **C++, C#, Java**: novo `KeywordRecognizer` objeto e APIs com suporte em plataformas Windows, Android, Linux & Ios. Leia a documentação [aqui](https://docs.microsoft.com/azure/cognitive-services/speech-service/custom-keyword-overview). Para obter o código de exemplo, consulte a seção de exemplos abaixo. 
- **Java**: conversa de vários dispositivos adicionada com suporte à tradução. Consulte o documento de referência [aqui](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.transcription).

**Melhorias & otimizações**
- **JavaScript**: implementação do microfone do navegador otimizada melhorando a precisão do reconhecimento de fala.
- **Java**: associações Refatorada usando implementação de JNI direta sem Swig. Isso reduz 10 vezes o tamanho das associações para todos os pacotes Java usados para Windows, Android, Linux e Mac e facilita o desenvolvimento adicional da implementação Java do SDK de fala.
- **Linux**: [documentação](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk?tabs=linux) de suporte atualizada com as últimas observações específicas do RHEL 7.
- Lógica de conexão aprimorada para tentar conectar várias vezes em caso de erros de serviço e de rede.
- Atualização da página de início rápido do [Portal.Azure.com](https://portal.azure.com) Speech para ajudar os desenvolvedores a executarem a próxima etapa na jornada de fala do Azure.

**Correções de bug**
- **C#, Java**: Corrigido um [problema](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/587) com o carregamento de bibliotecas do SDK no ARM do Linux (32 e 64 bits).
- **C#**: descarte explícito fixo de identificadores nativos para objetos TranslationRecognizer, IntentRecognizer e Connection.
- **C#**: gerenciamento de tempo de vida de entrada de áudio corrigido para o objeto ConversationTranscriber.
- Corrigido um problema em que o `IntentRecognizer` motivo do resultado não foi definido corretamente ao reconhecer tentativas de frases simples.
- Corrigido um problema em que o `SpeechRecognitionEventArgs` deslocamento de resultado não foi definido corretamente.
- Correção de uma condição de corrida em que o SDK estava tentando enviar uma mensagem de rede antes de abrir a conexão WebSocket. O was é reproduzível `TranslationRecognizer` durante a adição de participantes.
- Correção de vazamentos de memória no mecanismo de reconhecimento de palavra-chave.

**Amostras**
- **Go**: foram adicionados guias de início rápido para [reconhecimento de fala](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/speech-to-text-from-microphone?pivots=programming-language-go) e [Assistente de voz personalizado](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/voice-assistants?pivots=programming-language-go). Encontre o código de exemplo [aqui](https://github.com/microsoft/cognitive-services-speech-sdk-go/tree/master/samples). 
- **JavaScript**: foram adicionados guias de início rápido para conversão de [texto em fala](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/text-to-speech?pivots=programming-language-javascript), [tradução](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started-speech-translation?tabs=script&pivots=programming-language-csharp)e [reconhecimento de intenção](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/intent-recognition?pivots=programming-language-javascript).
- Exemplos de reconhecimento de palavra-chave para [C \# ](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/csharp/uwp/keyword-recognizer) e [Java](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/java/android/keyword-recognizer) (Android).  

**Teste do COVID-19 resumida:** Devido ao trabalho remoto nas últimas semanas, não poderíamos fazer tantos testes de verificação manual como normalmente. Não fizemos nenhuma alteração que achamos que poderia ter quebrado alguma coisa e nossos testes automatizados passaram. No caso improvável de não ter perdido algo, informe-nos no [GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues?q=is%3Aissue+is%3Aopen).<br>
Mantenha-se íntegro!

## <a name="speech-sdk-1110-2020-march-release"></a>SDK de fala 1.11.0:2020 – versão de março
**Novos recursos**
- Linux: suporte adicionado para Red Hat Enterprise Linux (RHEL)/CentOS 7 x64 com [instruções](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-configure-rhel-centos-7) sobre como configurar o sistema para o SDK de fala.
- Linux: suporte adicionado para .NET Core C# no Linux ARM32 e ARM64. Leia mais [aqui](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk?tabs=linux). 
- C#, C++: adicionado `UtteranceId` em `ConversationTranscriptionResult` , uma ID consistente em todos os intermediários e o resultado do reconhecimento de fala final. Detalhes de [C#](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.transcription.conversationtranscriptionresult?view=azure-dotnet), [C++](https://docs.microsoft.com/cpp/cognitive-services/speech/transcription-conversationtranscriptionresult).
- Python: suporte adicionado para `Language ID` . Consulte speech_sample. py no [repositório GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/python/console).
- Windows: adicionado suporte ao formato de entrada de áudio compactado na plataforma Windows para todos os aplicativos de console do Win32. Detalhes [aqui](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-use-codec-compressed-audio-input-streams). 
- JavaScript: suporte à síntese de fala (conversão de texto em fala) no NodeJS. Saiba mais [aqui](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript/node/text-to-speech). 
- JavaScript: adicione novas APIs para habilitar a inspeção de todas as mensagens de envio e recebidas. Saiba mais [aqui](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript). 
        
**Correções de bug**
- C#, C++: Corrigido um problema; `SendMessageAsync` agora, envia uma mensagem binária como um tipo binário. Detalhes de [C#](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.connection.sendmessageasync?view=azure-dotnet#Microsoft_CognitiveServices_Speech_Connection_SendMessageAsync_System_String_System_Byte___System_UInt32_), [C++](https://docs.microsoft.com/cpp/cognitive-services/speech/connection).
- C#, C++: Corrigido um problema em que o uso do `Connection MessageReceived` evento pode causar falha se `Recognizer` for descartado antes do `Connection` objeto. Detalhes de [C#](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.connection.messagereceived?view=azure-dotnet), [C++](https://docs.microsoft.com/cpp/cognitive-services/speech/connection#messagereceived).
- Android: o tamanho do buffer de áudio do microfone diminuiu de 800ms para 100 ms para melhorar a latência.
- Android: Corrigido um [problema](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/563) com o emulador Android x86 no Android Studio.
- JavaScript: suporte adicionado para regiões na China com a `fromSubscription` API. Detalhes [aqui](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?view=azure-node-latest#fromsubscription-string--string-). 
- JavaScript: Adicione mais informações de erro para falhas de conexão de NodeJS.
        
**Amostras**
- Unity: o exemplo público de reconhecimento de intenção é fixo, em que a importação de JSON LUIS estava falhando. Detalhes [aqui](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/369).
- Python: exemplo adicionado para `Language ID` . Detalhes [aqui](https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/python/console/speech_sample.py).
    
**Teste do Covid19 resumida:** Devido ao trabalho remoto nas últimas semanas, não pudemos fazer o teste manual de verificação do dispositivo como normalmente. Um exemplo disso é testar a entrada do microfone e a saída do orador no Linux, iOS e macOS. Não fizemos nenhuma alteração que achamos que poderia ter quebrado alguma coisa nessas plataformas, e nossos testes automatizados passaram. No caso improvável de não ter perdido algo, informe-nos no [GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues?q=is%3Aissue+is%3Aopen).<br> Obrigado pelo seu suporte contínuo. Como sempre, poste perguntas ou comentários no [GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues?q=is%3Aissue+is%3Aopen) ou [Stack Overflow](https://stackoverflow.microsoft.com/questions/tagged/731).<br>
Mantenha-se íntegro!

## <a name="speech-sdk-1100-2020-february-release"></a>SDK de fala 1.10.0:2020 – versão de fevereiro

**Novos recursos**

 - Pacotes python adicionados para dar suporte à nova versão 3,8 do Python.
 - Red Hat Enterprise Linux (RHEL) suporte a/CentOS 8 x64 (C++, C#, Java, Python).
   > [!NOTE] 
   > Os clientes devem configurar o OpenSSL de acordo com [estas instruções](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-configure-openssl-linux).
 - Suporte do Linux ARM32 para Debian e Ubuntu.
 - O DialogServiceConnector agora dá suporte a um parâmetro opcional "ID do bot" em BotFrameworkConfig. Esse parâmetro permite o uso de vários bots de fala de linha direta com um único recurso de fala do Azure. Sem o parâmetro especificado, o bot padrão (conforme determinado pela página de configuração de canal da Direct line Speech) será usado.
 - DialogServiceConnector agora tem uma propriedade SpeechActivityTemplate. O conteúdo dessa cadeia de caracteres JSON será usado pela Direct line Speech para preencher previamente uma grande variedade de campos com suporte em todas as atividades que atingem um bot de fala de linha direta, incluindo atividades geradas automaticamente em resposta a eventos como reconhecimento de fala.
 - A TTS agora usa a chave de assinatura para autenticação, reduzindo a latência de primeiro byte do primeiro resultado de síntese após a criação de um sintetizador.
 - Modelos de reconhecimento de fala atualizados para 19 localidades para uma redução média da taxa de erros do Word de 18,6% (es-ES, es-MX, fr-CA, fr-FR, it-IT, ja-JP, ko-KR, pt-BR, zh-CN, ZH-HK, NB-NO, Fi-FL, ru-RU, pl-PL, CA-ES, zh-TW, th-ÉSIMO, pt-PT, TR-TR). Os novos modelos trazem melhorias significativas em vários domínios, incluindo ditado, transcrição de Call Center e cenários de indexação de vídeo.

**Correções de bug**

 - Corrigido o bug em que o transcrevedor de conversa não aguardou corretamente em APIs JAVA 
 - Correção do emulador do Android x86 para o problema do Xamarin [GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/363)
 - Adição ausente (Get | Set) métodos de propriedade para AudioConfig
 - Corrigir um bug de TTS em que o audioDataStream não pôde ser interrompido quando a conexão falha
 - Usar um ponto de extremidade sem uma região causaria falhas de USP para o tradutor de conversa
 - A geração de ID em aplicativos universais do Windows agora usa um algoritmo GUID exclusivo apropriado; Ele, anteriormente, de forma não intencional, um padrão de implementação de fragmentado que frequentemente produziu colisões em grandes conjuntos de interações.
 
 **Amostras**
 
 - Exemplo de Unity para usar o SDK de fala com o [microfone do Unity e streaming do modo Push](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/unity/from-unitymicrophone)

**Outras alterações**

 - [Documentação de configuração do OpenSSL atualizada para Linux](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-configure-openssl-linux)

## <a name="speech-sdk-190-2020-january-release"></a>SDK de fala 1.9.0:2020 – versão de janeiro

**Novos recursos**

- Conversa com vários dispositivos: Conecte vários dispositivos à mesma fala ou conversa baseada em texto e, opcionalmente, traduza as mensagens enviadas entre elas. Saiba mais neste [artigo](multi-device-conversation.md). 
- Suporte de reconhecimento de palavra-chave adicionado ao pacote Android. aar e adicionado suporte para tipos x86 e x64. 
- Objective-C: `SendMessage` e `SetMessageProperty` métodos adicionados ao `Connection` objeto. Consulte a documentação [aqui](https://docs.microsoft.com/objectivec/cognitive-services/speech/spxconnection).
- A API do TTS C++ agora dá suporte `std::wstring` como entrada de texto de síntese, removendo a necessidade de converter um wstring em cadeia de caracteres antes de passá-lo para o SDK. Veja os detalhes [aqui](https://docs.microsoft.com/cpp/cognitive-services/speech/speechsynthesizer#speaktextasync). 
- C#: a [ID do idioma](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-automatic-language-detection?pivots=programming-language-csharp) e a configuração do idioma de [origem](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-specify-source-language?pivots=programming-language-csharp) agora estão disponíveis.
- JavaScript: adicionou um recurso ao `Connection` objeto para passar mensagens personalizadas do serviço de fala como um retorno de chamada `receivedServiceMessage` .
- JavaScript: suporte adicionado para `FromHost API` para facilitar o uso com contêineres locais e nuvens soberanas. Consulte a documentação [aqui](speech-container-howto.md).
- JavaScript: agora respeitamos `NODE_TLS_REJECT_UNAUTHORIZED` graças a uma contribuição do [orgads](https://github.com/orgads). Veja os detalhes [aqui](https://github.com/microsoft/cognitive-services-speech-sdk-js/pull/75).

**Alterações da falha**

- `OpenSSL` foi atualizado para a versão 1.1.1 b e é vinculado estaticamente à biblioteca principal do SDK de fala para Linux. Isso poderá causar uma interrupção se a caixa de entrada `OpenSSL` não tiver sido instalada `/usr/lib/ssl` no diretório no sistema. Consulte [nossa documentação](how-to-configure-openssl-linux.md) em documentos do SDK de fala para contornar o problema.
- Alteramos o tipo de dados retornado para C# `WordLevelTimingResult.Offset` de `int` para `long` para permitir o acesso ao `WordLevelTimingResults` quando os dados de fala forem maiores do que 2 minutos.
- `PushAudioInputStream` e `PullAudioInputStream` agora envie informações de cabeçalho WAV para o serviço de fala com base em `AudioStreamFormat` , opcionalmente especificado quando elas foram criadas. Os clientes agora devem usar o [formato de entrada de áudio com suporte](how-to-use-audio-input-streams.md). Qualquer outro formato terá resultados de reconhecimento abaixo do ideal ou poderá causar outros problemas. 

**Correções de bug**

- Consulte a `OpenSSL` atualização em alterações significativas acima. Corrigimos uma falha intermitente e um problema de desempenho (contenção de bloqueio sob alta carga) no Linux e no Java. 
- Java: fez melhorias no fechamento de objetos em cenários de alta simultaneidade.
- Reestruturado nosso pacote NuGet. Removemos as três cópias de `Microsoft.CognitiveServices.Speech.core.dll` e `Microsoft.CognitiveServices.Speech.extension.kws.dll` em pastas lib, tornando o pacote NuGet menor e mais rápido para baixar e adicionamos os cabeçalhos necessários para compilar alguns aplicativos nativos do C++.
- Corrigimos amostras de início rápido [aqui](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/cpp). Eles estavam saindo sem a exibição da exceção "microfone não encontrado" no Linux, macOS, Windows.
- Correção de falha do SDK com longos resultados de reconhecimento de fala em determinados caminhos de código como [Este exemplo](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/csharp/uwp/speechtotext-uwp).
- Corrigido o erro de implantação do SDK no ambiente de aplicativo Web do Azure para resolver [esse problema do cliente](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/396).
- Correção de um erro de TTS ao usar `<voice>` marcação múltipla ou `<audio>` marca para resolver [esse problema do cliente](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/433). 
- Foi corrigido um erro de TTS 401 quando o SDK é recuperado de suspenso.
- JavaScript: Corrigido uma importação circular de dados de áudio graças a uma contribuição de [euirim](https://github.com/euirim). 
- JavaScript: suporte adicionado para definir propriedades de serviço, conforme adicionado em 1,7.
- JavaScript: Corrigido um problema em que um erro de conexão pode resultar em tentativas de reconexão de WebSocket contínuas e sem êxito.

**Amostras**

- Exemplo adicional de reconhecimento de palavra-chave para Android [aqui](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/java/android/sdkdemo).
- Adição de exemplo de TTS para o cenário de servidor [aqui](https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/csharp/sharedcontent/console/speech_synthesis_server_scenario_sample.cs).
- Foram adicionados inícios rápidos de conversa com vários dispositivos para C# e C++ [aqui](quickstarts/multi-device-conversation.md).

**Outras alterações**

- Tamanho otimizado da biblioteca principal do SDK no Android.
- O SDK no 1.9.0 e em diante dá suporte `int` a ambos os `string` tipos e no campo versão da assinatura de voz para o Assinante de conversa.

## <a name="speech-sdk-180-2019-november-release"></a>SDK de fala 1.8.0:2019 – versão de novembro

**Novos recursos**

- Adicionada uma `FromHost()` API para facilitar o uso com contêineres locais e nuvens soberanas.
- Detecção de Idioma de origem automática adicionada para reconhecimento de fala (em Java e C++)
- `SourceLanguageConfig`Objeto adicionado para reconhecimento de fala, usado para especificar os idiomas de origem esperados (em Java e C++)
- `KeywordRecognizer`Suporte adicionado no Windows (UWP), Android e Ios por meio dos pacotes NuGet e Unity
- API Java de conversa remota adicionada para fazer a transcrição de conversa em lotes assíncronos.

**Alterações da falha**

- Funcionalidades de transistores de conversa movidas no namespace `Microsoft.CognitiveServices.Speech.Transcription` .
- Parte dos métodos de transistores de conversa são movidos para a nova `Conversation` classe.
- Suporte removido para iOS de 32 bits (ARMv7 e x86)

**Correções de bug**

- Correção de falha se `KeywordRecognizer` o local for usado sem uma chave de assinatura de serviço de fala válida

**Amostras**

- Exemplo do Xamarin para `KeywordRecognizer`
- Exemplo de Unity para `KeywordRecognizer`
- Exemplos de C++ e Java para Detecção de Idioma de origem automática.

## <a name="speech-sdk-170-2019-september-release"></a>SDK de fala 1.7.0:2019 – versão de setembro

**Novos recursos**

- Adicionado suporte beta para Xamarin em Plataforma Universal do Windows (UWP), Android e iOS
- Adicionado suporte do iOS para o Unity
- Adição `Compressed` de suporte de entrada para alaw, mulaw, FLAC no Android, Ios e Linux
- Adicionado `SendMessageAsync` `Connection` à classe para enviar uma mensagem para o serviço
- Adicionado `SetMessageProperty` à `Connection` classe para definir a propriedade de uma mensagem
- Ligações adicionadas a TTS para Java (JRE e Android), Python, Swift e Objective-C
- Suporte à reprodução de TTS adicionado para macOS, iOS e Android.
- Informações de "limite de palavras" adicionadas para TTS.

**Correções de bug**

- Correção do problema de compilação do IL2CPP no Unity 2019 para Android
- Corrigido o problema com cabeçalhos malformados na entrada do arquivo WAV que está sendo processada incorretamente
- Corrigido o problema com UUIDs que não são exclusivos em algumas propriedades de conexão
- Correção de alguns avisos sobre especificadores de nulidade nas associações Swift (pode exigir pequenas alterações de código)
- Correção de um bug que fazia com que as conexões WebSocket fosse fechadas de acordo com a carga de rede
- Correção de um problema no Android que às vezes resulta em IDs de impressão duplicadas usadas pelo `DialogServiceConnector`
- Melhorias na estabilidade de conexões entre interativações de várias transformações e o relatório de falhas (por meio de `Canceled` eventos) quando ocorrem com `DialogServiceConnector`
- `DialogServiceConnector` a sessão inicia agora fornecerá eventos, inclusive ao chamar `ListenOnceAsync()` durante um ativo `StartKeywordRecognitionAsync()`
- Endereçado uma falha associada às `DialogServiceConnector` atividades que estão sendo recebidas

**Amostras**

- Início rápido para Xamarin
- Atualizado o início rápido do CPP com as informações de ARM64 do Linux
- Guia de início rápido do Unity atualizado com informações do iOS

## <a name="speech-sdk-160-2019-june-release"></a>SDK de fala 1.6.0:2019 – versão de junho

**Amostras**

- Exemplos de início rápido para texto para fala sobre UWP e Unity
- Exemplo de início rápido para Swift no iOS
- Amostras de Unity para & de fala Reconhecimento de intenção e tradução
- Exemplos de guia de início rápido atualizados para `DialogServiceConnector`

**Melhorias/Alterações**

- Namespace de caixa de diálogo:
  - `SpeechBotConnector` foi renomeado para `DialogServiceConnector`
  - `BotConfig` foi renomeado para `DialogServiceConfig`
  - `BotConfig::FromChannelSecret()` foi remapeado para `DialogServiceConfig::FromBotSecret()`
  - Todos os clientes de fala de linha direta existentes continuam com suporte após a renomeação
- Atualizar o adaptador REST TTS para dar suporte a proxy, conexão persistente
- Melhorar a mensagem de erro quando uma região inválida é passada
- Swift/Objective-C:
  - Melhoria no relatório de erros: os métodos que podem resultar em um erro agora estão presentes em duas versões: uma que expõe um `NSError` objeto para tratamento de erros e outra que gera uma exceção. O primeiro é exposto a Swift. Essa alteração requer adaptações para o código Swift existente.
  - Manipulação de eventos aprimorada

**Correções de bug**

- Correção para TTS: em que o `SpeakTextAsync` futuro retornou sem aguardar até que o áudio tenha concluído a renderização
- Correção para o marshaling de cadeias de caracteres em C# para habilitar o suporte a idioma completo
- Correção do problema do aplicativo .NET Core para carregar a biblioteca principal com a estrutura de destino net461 em exemplos
- Correção de problemas ocasionais para implantar bibliotecas nativas na pasta de saída em exemplos
- Correção de fechamento de soquete da Web confiável
- Correção de uma possível falha ao abrir uma conexão sob carga muito pesada no Linux
- Correção de metadados ausentes no pacote de estrutura para macOS
- Correção para problemas com o `pip install --user` no Windows

## <a name="speech-sdk-151"></a>1.5.1 SDK de fala

Essa é uma liberação de correção de bug e afeta apenas o SDK nativo/gerenciado. Ele não está afetando a versão JavaScript do SDK.

**Correções de bug**

- Corrigir o FromSubscription quando usado com a transcrição de conversa.
- Correção de bug na palavra-chave para assistentes de voz.

## <a name="speech-sdk-150-2019-may-release"></a>Speech SDK 1.5.0:2019-maio de lançamento

**Novos recursos**

- A palavra-chave (KWS) está disponível agora para Windows e Linux. A funcionalidade do KWS pode funcionar com qualquer tipo de microfone, o suporte oficial do KWS, no entanto, está limitado atualmente às matrizes de microfone encontradas no hardware do Azure Kinect DK ou no SDK dos dispositivos de fala.
- A funcionalidade de dica de frase está disponível por meio do SDK. Para saber mais, clique [aqui](how-to-phrase-lists.md).
- A funcionalidade de transcrição de conversa está disponível por meio do SDK. Consulte [aqui](conversation-transcription-service.md).
- Adicione suporte para assistentes de voz usando o canal de fala de linha direta.

**Amostras**

- Foram adicionadas amostras para novos recursos ou novos serviços com suporte no SDK.

**Melhorias/Alterações**

- Adicionada várias propriedades de reconhecedor para ajustar o comportamento do serviço ou os resultados do serviço (como mascarar profanação e outros).
- Agora você pode configurar o reconhecedor por meio das propriedades de configuração padrão, mesmo que você tenha criado o reconhecedor `FromEndpoint` .
- Objective-C: `OutputFormat` a propriedade foi adicionada a `SPXSpeechConfiguration` .
- O SDK agora dá suporte a Debian 9 como uma distribuição do Linux.

**Correções de bug**

- Corrigido um problema em que o recurso do orador foi destruido muito cedo em texto para fala.

## <a name="speech-sdk-142"></a>SDK 1.4.2 de fala

Essa é uma liberação de correção de bug e afeta apenas o SDK nativo/gerenciado. Ele não está afetando a versão JavaScript do SDK.

## <a name="speech-sdk-141"></a>1.4.1 SDK de fala

Esta é uma versão somente em JavaScript. Nenhum recurso foi adicionado. Foram feitas as seguintes correções:

- Impedir que o pacote da Web carregue HTTPS-proxy-Agent.

## <a name="speech-sdk-140-2019-april-release"></a>SDK de fala 1.4.0:2019 – versão de abril

**Novos recursos**

- O SDK agora dá suporte ao serviço de conversão de texto em fala como uma versão beta. Há suporte na área de trabalho do Windows e do Linux do C++ e do C#. Para obter mais informações, consulte a [visão geral de conversão de texto em fala](text-to-speech.md#get-started).
- O SDK agora dá suporte a arquivos de áudio MP3 e Opus/OGG como arquivos de entrada de fluxo. Esse recurso está disponível apenas no Linux em C++ e em C# e, no momento, está em beta (mais detalhes [aqui](how-to-use-codec-compressed-audio-input-streams.md)).
- O SDK de fala para Java, .NET Core, C++ e Objective-C ganhou suporte para macOS. O suporte a Objective-C para macOS está atualmente em beta.
- iOS: o SDK de fala para iOS (Objective-C) agora também é publicado como um CocoaPod.
- JavaScript: suporte para microfone não padrão como um dispositivo de entrada.
- JavaScript: suporte a proxy para Node.js.

**Amostras**

- Foram adicionados exemplos de uso do SDK de fala com C++ e com o Objective-C no macOS.
- Exemplos que demonstram o uso do serviço de conversão de texto em fala foram adicionados.

**Melhorias/Alterações**

- Python: as propriedades adicionais dos resultados de reconhecimento agora são expostas por meio da `properties` propriedade.
- Para obter suporte adicional para desenvolvimento e depuração, você pode redirecionar as informações de log e diagnóstico do SDK para um arquivo de log (mais detalhes [aqui](how-to-use-logging.md)).
- JavaScript: melhorar o desempenho de processamento de áudio.

**Correções de bug**

- Mac/iOS: um bug que levou a uma longa espera quando uma conexão com o serviço de fala não pôde ser estabelecida foi corrigida.
- Python: melhorar o tratamento de erros para argumentos em retornos de chamada do Python.
- JavaScript: o relatório de estado incorreto corrigido para a fala terminou em RequestSession.

## <a name="speech-sdk-131-2019-february-refresh"></a>SDK de fala 1.3.1:2019 – atualização de fevereiro

Essa é uma liberação de correção de bug e afeta apenas o SDK nativo/gerenciado. Ele não está afetando a versão JavaScript do SDK.

**Correção de bug**

- Foi corrigido um vazamento de memória ao usar a entrada do microfone. Baseada em fluxo ou entrada de arquivo não é afetada.

## <a name="speech-sdk-130-2019-february-release"></a>SDK de fala 1.3.0:2019 – versão de fevereiro

**Novos recursos**

- O SDK de fala dá suporte à seleção do microfone de entrada por meio da `AudioConfig` classe. Isso permite que você transmita dados de áudio para o serviço de fala de um microfone não padrão. Para obter mais informações, consulte a documentação que descreve a [seleção de dispositivo de entrada de áudio](how-to-select-audio-input-devices.md). Esse recurso ainda não está disponível no JavaScript.
- O Speech SDK agora dá suporte ao Unity em uma versão beta. Forneça comentários na seção de problemas no [repositório de exemplo do GitHub](https://aka.ms/csspeech/samples). Essa versão dá suporte ao Unity no Windows x86 e x64 (área de trabalho ou aplicativos da Plataforma Universal do Windows) e Android (ARM32/64, x86). Mais informações estão disponíveis em nosso [início rápido do Unity](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=unity).
- O arquivo `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` (fornecido em versões anteriores) não é mais necessário. A funcionalidade agora está integrada ao SDK do Core.

**Amostras**

O novo conteúdo a seguir está disponível no nosso [repositório de exemplo](https://aka.ms/csspeech/samples):

- Exemplos adicionais para o `AudioConfig.FromMicrophoneInput` .
- Mais exemplos do Python para conversão e reconhecimento de intenção.
- Exemplos adicionais para usar o `Connection` objeto no Ios.
- Exemplos adicionais de Java para tradução com saída de áudio.
- Novo exemplo para usar a [API REST de Transcrição de Lote](batch-transcription.md).

**Melhorias/Alterações**

- Python
  - Verificação de parâmetro e mensagens de erro aprimoradas no `SpeechConfig` .
  - Adicione suporte para o `Connection` objeto.
  - Suporte para Python de 32 bits (x86) no Windows.
  - O Speech SDK para Python está fora do beta.
- iOS
  - O SDK agora é construído com relação ao SDK versão 12.1 do iOS.
  - O SDK agora dá suporte a iOS versões 9.2 e posteriores.
  - Melhore a documentação de referência e conserte vários nomes de propriedade.
- JavaScript
  - Adicione suporte para o `Connection` objeto.
  - Adicione arquivos de definição de tipo para JavaScript agrupado
  - Suporte inicial e implementação para dicas de frase.
  - Retornar a coleção de propriedades com o serviço de JSON para reconhecimento
- DLLs do Windows agora contêm um recurso de versão.
- Se você criar um reconhecedor `FromEndpoint` , poderá adicionar parâmetros diretamente à URL do ponto de extremidade. Usando `FromEndpoint` o, você não pode configurar o reconhecedor por meio das propriedades de configuração padrão.

**Correções de bug**

- Nome de usuário de proxy e senha de proxy vazios não foram tratados corretamente. Com esta versão, se você definir o nome de usuário de proxy e a senha de proxy como uma cadeia de caracteres vazia, eles não serão enviados ao conectarem-se ao proxy.
- As SessionIds criadas pelo SDK nem sempre eram realmente aleatórias para alguns idiomas&nbsp;/ambientes. Adicionada a inicialização aleatória do gerador para corrigir esse problema.
- Melhore o tratamento do token de autorização. Se você quiser usar um token de autorização, especifique em `SpeechConfig` e deixe a chave de assinatura vazia. Em seguida, crie o reconhecedor como de costume.
- Em alguns casos, o `Connection` objeto não foi liberado corretamente. Esse problema foi corrigido.
- O exemplo de JavaScript foi corrigido para dar suporte para saída de áudio para síntese de conversão também no Safari.

## <a name="speech-sdk-121"></a>Speech SDK 1.2.1

Esta é uma versão somente em JavaScript. Nenhum recurso foi adicionado. Foram feitas as seguintes correções:

- Acionar o final do fluxo no turn.end, não no speech.end.
- Consertar um bug na bomba de áudio que não agendou o próximo envio em caso de falha do envio atual.
- Consertar reconhecimento contínuo com o token de autenticação.
- Correção de bug para diferentes reconhecedores/pontos de extremidade.
- Melhorias na documentação.

## <a name="speech-sdk-120-2018-december-release"></a>SDK de fala 1.2.0:2018 – versão de dezembro

**Novos recursos**

- Python
  - A versão Beta do suporte do Python (3.5 e posterior) está disponível com esta versão. Para obter mais informações, consulte aqui] (início rápido-python.md).
- JavaScript
  - O SDK de Fala para o JavaScript tem sido livre. O código-fonte está disponível no [GitHub](https://github.com/Microsoft/cognitive-services-speech-sdk-js).
  - Agora damos suporte a node. js, mais informações podem ser encontradas [aqui](quickstart-js-node.md).
  - A restrição de comprimento para sessões de áudio foi removida, a reconexão ocorrerá automaticamente sob a tampa.
- Objeto `Connection`
  - No `Recognizer` , você pode acessar um `Connection` objeto. Esse objeto permite iniciar a conexão de serviço e inscrever-se para se conectar e desconectar de eventos explicitamente.
    (Esse recurso ainda não está disponível no JavaScript e no Python.)
- Suporte para Ubuntu 18.04.
- Android
  - Suporte do ProGuard habilitado durante a geração de APK.

**Aprimoramentos**

- Melhorias no uso de thread interno, reduzindo o número de threads, bloqueios e exclusões mútuas.
- Relatório/informações de erros aprimorados. Em vários casos, as mensagens de erro não foram propagadas até o fim.
- As dependências de desenvolvimento atualizadas do JavaScript para usar módulos atualizados.

**Correções de bug**

- Correção de vazamentos de memória devido a uma incompatibilidade de tipo em `RecognizeAsync` .
- Em alguns casos, as exceções foram sendo vazadas.
- Corrigindo o vazamento de memória em argumentos de evento de tradução.
- Corrigido um problema de bloqueio na reconexão longa de sessões em execução.
- Correção de um problema que poderia levar a um resultado final ausente para traduções com falha.
- C#: se uma `async` operação não foi aguardada no thread principal, era possível que o reconhecedor pudesse ser descartado antes da conclusão da tarefa assíncrona.
- Java: Corrigido um problema que resulta em uma falha da VM Java.
- Objective-C: mapeamento de enumeração fixo; RecognizedIntent foi retornado em vez de `RecognizingIntent` .
- JavaScript: defina o formato de saída padrão como "simples" em `SpeechConfig` .
- JavaScript: removendo a inconsistência entre as propriedades no objeto de configuração em JavaScript e em outras linguagens.

**Amostras**

- Atualização e correção de vários exemplos (por exemplo, vozes de saída para tradução, etc.).
- Adicionados exemplos do Node. js no [repositório de exemplo](https://aka.ms/csspeech/samples).

## <a name="speech-sdk-110"></a>SDK de Fala 1.1.0

**Novos recursos**

- Suporte para Android x86/x64.
- Suporte a proxy: no `SpeechConfig` objeto, agora você pode chamar uma função para definir as informações de proxy (nome do host, porta, nome de usuário e senha). Este recurso ainda não está disponível no iOS.
- Melhor código de erro e mensagens. Se um reconhecimento retornou um erro, isso já definiu `Reason` (no evento cancelado) ou `CancellationDetails` (no resultado do reconhecimento) para `Error`. O evento cancelado agora contém dois membros adicionais, `ErrorCode` e `ErrorDetails`. Se o servidor retornou informações de erro adicionais com o erro relatado, agora ele estará disponível nos novos membros.

**Aprimoramentos**

- Adicionada verificação adicional na configuração do reconhecedor e adicionada outra mensagem de erro.
- Manipulação aprimorada de silêncio de longa duração no meio de um arquivo de áudio.
- Pacote NuGet: para projetos do .NET Framework, ele impede a construção com a configuração AnyCPU.

**Correções de bug**

- Corrigido várias exceções encontradas em reconhecedores. Além disso, as exceções são capturadas e convertidas em `Canceled` evento.
- Corrigir um vazamento de memória no gerenciamento de propriedades.
- Corrigido o erro no qual um arquivo de entrada de áudio poderia travar o reconhecedor.
- Corrigido um bug no qual os eventos podiam ser recebidos após um evento de parada da sessão.
- Corrigidas algumas condições de corrida no threading.
- Corrigido um problema de compatibilidade do iOS que poderia resultar em uma falha.
- Melhorias de estabilidade para suporte de microfone Android.
- Corrigido um erro em que um reconhecedor em JavaScript ignoraria o idioma de reconhecimento.
- Correção de um bug que impede a definição de `EndpointId` (em alguns casos) em JavaScript.
- A ordem dos parâmetros foi alterada em addpropósito em JavaScript e adicionou uma `AddIntent` assinatura JavaScript ausente.

**Amostras**

- Foram adicionados exemplos de C++ e C# para uso de fluxo de pull e Push no [repositório de exemplo](https://aka.ms/csspeech/samples).

## <a name="speech-sdk-101"></a>SDK de Fala 1.0.1

Melhorias na confiabilidade e correções de bugs:

- Corrigido erro fatal potencial devido à condição de corrida no reconhecedor de descarte
- Corrigido erro fatal potencial no caso de propriedades não definidas.
- Adicionado erro adicional e verificação de parâmetros.
- Objective-C: corrigido possível erro fatal causado por substituição de nome em NSString.
- Objective-C: visibilidade ajustada da API
- JavaScript: corrigido em relação a eventos e cargas.
- Melhorias na documentação.

Em nosso [repositório de exemplos](https://aka.ms/csspeech/samples), um novo exemplo para JavaScript foi adicionado.

## <a name="cognitive-services-speech-sdk-100-2018-september-release"></a>SDK de Fala dos Serviços Cognitivos 1.0.0: versão de setembro de 2018

**Novos recursos**

- Suporte para Objective-C no iOS. Confira nosso [Início Rápido do Objective-C para iOS](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone-langs/objectivec-ios.md).
- Suporte para JavaScript no navegador. Confira nosso [Início Rápido do JavaScript](quickstart-js-browser.md).

**Alterações da falha**

- Com esta versão, várias alterações significativas são introduzidas.
  Confira [esta página](https://aka.ms/csspeech/breakingchanges_1_0_0) para obter detalhes.

## <a name="cognitive-services-speech-sdk-060-2018-august-release"></a>SDK de Fala dos Serviços Cognitivos 0.6.0: versão de agosto de 2018

**Novos recursos**

- Os aplicativos UWP criados com o SDK de Fala agora podem ser aprovados pelo WACK (Kit de Certificação de Aplicativos Windows).
  Confira o [Início Rápido do UWP](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-chsarp&tabs=uwp).
- Suporte para .NET Standard 2.0 no Linux (Ubuntu 16.04 x64).
- Experimental: dê suporte Java 8 no Windows (64 bits) e no Linux (Ubuntu 16.04 x64).
  Confira o guia de [início rápido do Java Runtime Environment](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-java&tabs=jre).

**Alteração funcional**

- Expor informações de detalhe de erro adicionais sobre erros de conexão.

**Alterações da falha**

- No Java (Android), a função `SpeechFactory.configureNativePlatformBindingWithDefaultCertificate` não requer mais um parâmetro de caminho. Agora, o caminho é detectado automaticamente em todas as plataformas com suporte.
- O get-accessor da propriedade `EndpointUrl` em Java e C# foi removido.

**Correções de bug**

- Em Java, o resultado da síntese de áudio no reconhecedor de tradução agora está implementado.
- Foi corrigido um bug que podia causar threads inativos e um grande número de soquetes abertos e não usados.
- Foi corrigido um problema em que o reconhecimento de execução longa podia terminar no meio da transmissão.
- Corrigida uma condição de corrida no desligamento do reconhecedor.

## <a name="cognitive-services-speech-sdk-050-2018-july-release"></a>SDK de Fala dos Serviços Cognitivos 0.5.0: versão de julho de 2018

**Novos recursos**

- Suporte a plataforma Android (API 23: Android 6.0 Marshmallow ou superior). Confira o [Início Rápido para Android](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-java&tabs=android).
- Suporte para .NET Standard 2.0 no Windows. Confira o [Início Rápido para .NET Core](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnetcore).
- Experimental: Suporte a UWP no Windows (versão 1709 ou posterior).
  - Confira o [Início Rápido do UWP](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=uwp).
  - Observação: os aplicativos UWP compilados com o SDK de Fala ainda não são aprovados pelo WACK (Kit de Certificação de Aplicativos Windows).
- Suporte ao reconhecimento de execução longa com reconexão automática.

**Alterações funcionais**

- O `StartContinuousRecognitionAsync()` dá suporte ao reconhecimento de execução longa.
- O resultado do reconhecimento contém mais campos. Eles são deslocados do início do áudio e da duração (ambos em tiques) do texto reconhecido e dos valores adicionais que representam o status de reconhecimento, por exemplo, `InitialSilenceTimeout` e `InitialBabbleTimeout`.
- Suporte para AuthorizationToken para criar instâncias de fábrica.

**Alterações da falha**

- Eventos de reconhecimento: `NoMatch` o tipo de evento foi mesclado no `Error` evento.
- SpeechOutputFormat em C# foi renomeado para `OutputFormat` para se manter alinhado com o C++.
- O tipo de retorno de alguns métodos da interface `AudioInputStream` foi um pouco alterado:
  - Em Java, o método `read` agora retorna `long` em vez de `int`.
  - Em C#, o método `Read` agora retorna `uint` em vez de `int`.
  - Em C++, os métodos `Read` e `GetFormat` agora retornam `size_t` em vez de `int`.
- C++: as instâncias de fluxos de entrada de áudio agora podem ser passadas apenas como um `shared_ptr`.

**Correções de bug**

- Foram corrigidos os valores retornados incorretos no resultado quando `RecognizeAsync()` atinge o tempo limite.
- A dependência das bibliotecas do Media Foundation no Windows foi removida. O SDK agora usa as APIs Core Audio.
- Correção da documentação: uma página de [regiões](regions.md) foi adicionada para descrever as regiões com suporte.

**Problema conhecido**

- O SDK de Fala para Android não relata os resultados da síntese de fala para tradução. Esse problema será corrigido na próxima versão.

## <a name="cognitive-services-speech-sdk-040-2018-june-release"></a>SDK de Fala de Serviços Cognitivos 0.4.0: versão de junho de 2018

**Alterações funcionais**

- AudioInputStream

  Agora, um reconhecedor pode consumir um fluxo como a fonte de áudio. Para obter mais detalhes, confira o [guia de instruções](how-to-use-audio-input-streams.md) relacionado.

- Formato de saída detalhado

  Ao criar um `SpeechRecognizer`, você pode solicitar o formato de saída `Detailed` ou `Simple`. O `DetailedSpeechRecognitionResult` contém uma pontuação de confiança, texto reconhecido, forma léxica bruta, forma normalizada e forma normalizada com obscenidades mascaradas.

**Alteração significativa**

- Alterado para `SpeechRecognitionResult.Text` de `SpeechRecognitionResult.RecognizedText` em C#.

**Correções de bug**

- Foi corrigido um possível problema de retorno de chamada na camada USP durante o desligamento.
- Se um reconhecedor consumir um arquivo de entrada de áudio, ele manteve o identificador de arquivo por mais tempo do que o necessário.
- Foram removidos vários deadlocks entre a bomba de mensagens e o reconhecedor.
- Dispare um resultado `NoMatch` quando o tempo de resposta do serviço esgotar.
- As bibliotecas do Media Foundation no Windows são carregadas com atraso. Essa biblioteca é necessária apenas para entrada do microfone.
- A velocidade de carregamento de dados de áudio é limitada a duas vezes a velocidade do áudio original.
- No Windows, agora os assemblies .NET em C# têm nomes fortes.
- Correção de documentação: `Region` são as informações necessárias para criar um reconhecedor.

Mais exemplos foram adicionados e são atualizados constantemente. Para obter o último conjunto de exemplos, confira o [Repositório GitHub de exemplos do SDK de Fala](https://aka.ms/csspeech/samples).

## <a name="cognitive-services-speech-sdk-0212733-2018-may-release"></a>SDK de Fala de Serviços Cognitivos 0.2.12733: versão de maio de 2018

Esta versão é a primeira versão prévia pública do SDK de Fala dos Serviços Cognitivos.
