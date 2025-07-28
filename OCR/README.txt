- Azure AI Vision - OCR - Leitura de texto em imagens

O reconhecimento óptico de caracteres (OCR) é um subconjunto da visão computacional que lida com a leitura de texto em imagens e documentos. Usaremos o serviço de Análise de Imagens do Azure AI Vision através de uma API fornecida para fazer a leitura de texto.

Será criado:

1. Recurso do Azure AI Vision
2. Aplicativo de leitura de texto em imagens com o Azure AI Vision SDK

- Como usar: Passo a passo

1. Acesse o Portal do Azure Estúdio
   [https://azure.microsoft.com]

2. Provisionando recurso no Azure AI Vision
   Selecione "+Criar Recurso"
   Na barra de pesquisa, busque por "Computer Vision"e crie um recurso
   Preencha: Assinatura, Grupo de Recursos, Região, Nome e Nível de preço
   Clique em "Criar e revisar" e após verificação, em "Criar"
   Após a criação, acesse o recurso e na aba lateral "Gerenciamento de Recursos" e copie  

3. Desenvolvendo um aplicativo de extração de texto com o Azure AI Vision SDK
   Crie um novo Cloud Shell no portal do Azure, selecionando o ambiente do PowerShell sem armazenamento na sua assinatura. 
   Na barra de ferramentas do Cloud Shell, em configurações, selecione "ir para a versão clássica
   Clone o repositório do GitHub que contém os arquivos de código
   "rm -r mslearn-ai-vision -f
    git clone https://github.com/MicrosoftLearning/mslearn-ai-vision"
   Visualize a pasta com os arquivos clonados
   "cd mslearn-ai-vision/Labfiles/ocr/python/read-text
    ls -a -l"
   Instale o pacote Azure AI Vision SDK e outros pacotes necessários
   "python -m venv labenv
   ./labenv/bin/Activate.ps1
   pip install -r requirements.txt azure-ai-vision-imageanalysis==1.0.0"
   Edite o arquivo de configuração
   "Code .env"
   O arquivo é aberto em um editor de código
   Copie o endpoint e a chave nos lugares solicitados
   Use CTRL+S para salvar e CTRL+Q para sair
   
4. Adicionando código para ler o texto de uma imagem
   Na linha de comando do Cloud Shell abra o arquivo de código do cliente
   "code read-text.py"
   Encontre o comentário Import namespaces e adicione o seguinte código para importar os namespaces que você precisará para usar o Azure AI Vision SDK:
   "from azure.ai.vision.imageanalysis import ImageAnalysisClient
   from azure.ai.vision.imageanalysis.models import VisualFeatures
   from azure.core.credentials import AzureKeyCredential"
   Localize o comentário "Authenticate Azure AI Vision client" e adicione o seguinte código:
    "cv_client = ImageAnalysisClient(
          endpoint=ai_endpoint,
          credential=AzureKeyCredential(ai_key))"
    Encontre o comentário "Ler texto na imagem" e adicione o seguinte código:
    "with open(image_file, "rb") as f:
          image_data = f.read()
    print (f"\nReading text in {image_file}")

    result = cv_client.analyze(
         image_data=image_data,
         visual_features=[VisualFeatures.READ])"
    Encontre o comentário "Imprima o texto", adicione o seguinte código:
    "if result.read is not None:
          print("\nText:")
    
          for line in result.read.blocks[0].lines:
              print(f" {line.text}")        
          # Annotate the text in the image
          annotate_lines(image_file, result.read)

          # Find individual words in each line
    Salve
    Digite no Shell "python read-text.py images/Lincoln.jpg"para executar o programa
    Será criada uma imagem ("lines.jpg")na pasta read-text. Para baixa-la, digite o comendo: download lines.jpg
    
5. Adicionando código para retornar a posição de palavras individuais
   "print ("\nIndividual words:")
    for line in result.read.blocks[0].lines:
        for word in line.words:
         print(f"  {word.text} (Confidence: {word.confidence:.2f}%)")
   # Annotate the words in the image
   annotate_words(image_file, result.read)
   Salve
   Execute novamente o programa
   Faca download da nova imagem gerada
   
   
- Tecnologias Usadas

 1. Microsoft Azure - (https://azure.microsoft.com/)
 2. Azure AI Vision - (https://azure.microsoft.com/)
 3. ARM Templates - (https://learn.microsoft.com/azure/azure-resource-manager/templates/overview)
 4. Markdown para documentação

- Autor
  Dannuza Martins Cardoso Silva
 
 


