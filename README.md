# generation-ai-script-nutris-food-recognition
Este repositório tem como objetivo armazenar os códigos e reproduzir o mesmo resultado obtido durante o desenvolvimento do script de geração do modelo do projeto Nutris.
## Autores

- [@arthur-cabral](https://www.github.com/arthur-cabral)
- [@giovanna-oliveira](https://www.github.com/giovanna-oliveira)
- [@mateus-gomes](https://www.github.com/mateus-gomes)
## Requisitos

Para replicação do resultado obtido, é necessário a criação e alteração de alguns diretórios:

O diretório raiz que foi utilizado para o script, foi o `/V1`, então será necessário alterar de acordo com a pasta raiz que será utilizada.

Dentro desse diretório raiz, deve conter 2 pastas:

- runs
- datasets

A pasta runs tem como funcionalidade armazenar o modelo e informações sobre métricas após sua execução. Já a pasta de datasets, tem como objetivo dividir as imagens em porcentagens de treino, validação e teste, além de armazenar as imagens e labels pelo padrão do YOLO.

A pasta de datasets deve conter 3 pastas, sendo elas:

- train
- valid
- test

E dentro de cada uma delas, devem conter as pastas `images` e `labels`, seguindo o padrão do YOLO, onde contém as imagens e a posição que elas estão demarcadas na imagem em questão. 

E também dentro do diretório raiz, devem conter 3 arquivos:

- O arquivo do script principal `Modelo_Nutris_v1.ipynb`
- O arquivo do script secundário `script_model_with_taco.ipynb`
- `data.yaml`

O arquivo `data.yaml`, deve estar no seguinte formato: 

```
train: ./datasets/train/images
val: ./datasets/valid/images
test: ./datasets/test/images

nc: 6
names: ['arroz', 'carne', 'feijao', 'frango', 'ovo', 'salada']
```

No qual, estão setados os diretórios contendo as imagens de treino, validação e teste (`train`, `val`, `test`), além do número de classes (`nc`), e os nomes das classes em si (`names`). 
## Rodando

Utilize uma instância do Google Colab, e altere o ambiente de execução para T4 GPU, visando um melhor desempenho do ambiente.

Execute linha por linha até o comando abaixo:

```
model.train(data="/content/drive/MyDrive/nutris yolo cnn/V1/data.yaml", epochs = 60, project=save_dir)
```

Ajuste suas épocas conforme o necessário.

Após realizar os ajustes, execute o comando abaixo para validar o modelo e descobrir métricas:

```
model = YOLO('/content/drive/MyDrive/nutris yolo cnn/V1/runs/train7/weights/best.pt')
metrics = model.val(save_json = True, project=save_dir)  # no arguments needed, dataset and settings remembered
metrics.box.map    # map50-95
metrics.box.map50  # map50
metrics.box.map75  # map75
metrics.box.maps   # a list contains map50-95 of each category
``` 

Por último, com o modelo já criado e validado, basta executar o comando abaixo, para realizar testes com imagens inseridas no diretório `/datasets/test`:

```
model.predict(save_txt=True,  conf=0.5, project=save_dir, source='/content/drive/MyDrive/nutris yolo cnn/V1/datasets/test/images', save=True)
```
## Resultados

Todos os resultados obtidos podem ser visualizados dentro do diretório `/runs`, o qual contém matrizes de confusão, curvas de F1, resultados em gráfico, além do próprio modelo que poderá ser utilizado em qualquer aplicação.

Matriz de confusão:
![confusion_matrix_normalized](https://github.com/arthur-cabral/generation-ai-script-nutris-food-recognition/assets/61799587/8017b366-588d-4700-a703-466266464831)

Curva de F1:
![F1_curve](https://github.com/arthur-cabral/generation-ai-script-nutris-food-recognition/assets/61799587/68270292-e14d-450d-b0b3-7ef55414ad25)

Lote de treino:
![val_batch0_labels](https://github.com/arthur-cabral/generation-ai-script-nutris-food-recognition/assets/61799587/86af7a50-660b-4c18-9364-fc8ae62a1a1c)

