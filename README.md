# Projeto 1 — Redes Neurais para reconhecimento de dígitos

Projeto desenvolvido para a disciplina **MS571**. Este repositório contém a implementação da **Parte 1 do Projeto 1**, cujo objetivo é construir e analisar uma rede neural regularizada para reconhecer dígitos manuscritos.

A solução segue o algoritmo apresentado no material didático. As etapas centrais da rede — propagação direta, função de custo, retropropagação e descida do gradiente — foram implementadas explicitamente, sem o uso de classificadores prontos. O `scipy.optimize.minimize` é utilizado apenas no experimento com gradiente conjugado, conforme permitido pelo enunciado.

## Integrantes

- Jhuan Marcos Nunes Solis Silva - 266349
- Melissa Schiavon Pastene - 270324
- Pedro Rocha Teixeira - 253670

## Situação atual do projeto

A Parte 1 está implementada no notebook [`Parte1_Projeto1_Redes_Neurais.ipynb`](Parte1_Projeto1_Redes_Neurais.ipynb). Até o momento, foram desenvolvidas as seguintes etapas:

- leitura dos dados a partir de `ex3data1.mat` ou dos arquivos CSV;
- inspeção das dimensões, classes e distribuição dos dados;
- visualização de uma amostra das imagens;
- codificação *one-hot* dos rótulos;
- inicialização aleatória dos pesos;
- propagação direta (*forward propagation*);
- função de custo multiclasse regularizada;
- retropropagação (*backpropagation*);
- regularização dos pesos sem penalizar os termos de bias;
- empacotamento e desempacotamento dos parâmetros para arquiteturas genéricas;
- checagem numérica do gradiente por diferenças centrais;
- treinamento por descida do gradiente em lote;
- treinamento por gradiente conjugado;
- comparação de diferentes números de iterações e valores de regularização;
- análise das imagens classificadas incorretamente;
- visualização dos 25 filtros aprendidos pela camada escondida;
- discussão crítica dos métodos, resultados e limitações;
- fundamentação teórica e bibliografia.

## Dados e arquitetura

O conjunto contém **5.000 imagens** de dígitos manuscritos. Cada imagem possui dimensão `20 × 20` e é representada por um vetor de **400 atributos**.

A arquitetura principal utilizada é:

```text
400 entradas → 25 unidades escondidas → 10 unidades de saída
```

Os rótulos internos variam de `1` a `10`. No conjunto original, o rótulo `10` representa visualmente o dígito zero.

O código aceita tanto:

- `ex3data1.mat`; ou
- o par `imageMNIST.csv` e `labelMNIST.csv`.

Como esses arquivos representam o mesmo conjunto de dados, recomenda-se manter apenas `ex3data1.mat` no repositório para evitar duplicação.

## Resultados de referência da Parte 1

Na execução completa usada para validar o notebook, foram obtidos os seguintes resultados:

| Verificação ou método | Configuração | Resultado de referência |
|---|---|---:|
| Checagem do gradiente | Rede pequena `3 → 5 → 3`, `λ = 0,7` | Diferença relativa `2,216 × 10⁻¹⁰` |
| Descida do gradiente | `λ = 1`, `α = 0,8`, 300 iterações | Acurácia de ajuste `91,92%` |
| Gradiente conjugado | `λ = 1`, limite de 400 iterações | Acurácia de ajuste `99,52%` |

A diferença relativa da checagem ficou muito abaixo do limite adotado de `10⁻⁷`, fornecendo evidência numérica de que o gradiente analítico calculado pelo backpropagation está correto.

Essas acurácias foram calculadas sobre os mesmos exemplos utilizados no treinamento. Portanto, são **acurácias de ajuste**, e não estimativas de desempenho em dados novos. A separação entre treino, validação e teste, assim como a seleção adequada de `λ`, pertence à Parte 2 do projeto.

Pequenas diferenças numéricas podem ocorrer em outra máquina ou versão das bibliotecas. O tempo de execução também depende do processador utilizado.

## Estrutura recomendada

```text
Projeto1/
├── Parte1_Projeto1_Redes_Neurais.ipynb
├── ex3data1.mat
├── requirements.txt
├── README.md
└── .gitignore
```

A pasta `.venv` não deve ser enviada ao GitHub. Cada integrante deve criar seu próprio ambiente virtual.

## Requisitos

- Python 3.10 ou superior;
- VS Code;
- extensões **Python** e **Jupyter** da Microsoft;
- NumPy;
- Pandas;
- Matplotlib;
- SciPy;
- Jupyter e ipykernel.

As dependências estão listadas em `requirements.txt`.

## Como reproduzir os resultados no Windows e VS Code

### 1. Obtenha o projeto

Para baixar pelo Git, use no PowerShell:

```powershell
git clone URL_DO_REPOSITORIO
cd Projeto1
```

Substitua `URL_DO_REPOSITORIO` pela URL exibida no botão **Code** do GitHub. Outra opção é baixar o repositório como ZIP e extrair os arquivos.

### 2. Crie o ambiente virtual

Dentro da pasta do projeto, execute:

```powershell
py -m venv .venv
```

Se o comando `py` não estiver disponível, tente:

```powershell
python -m venv .venv
```

### 3. Ative o ambiente

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\.venv\Scripts\Activate.ps1
```

Quando a ativação funcionar, o terminal começará com `(.venv)`.

A ativação é conveniente, mas não é obrigatória. Os comandos das etapas seguintes também podem ser executados diretamente com `.\.venv\Scripts\python.exe`.

### 4. Instale as dependências

Com o ambiente ativado:

```powershell
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

Sem ativar o ambiente:

```powershell
.\.venv\Scripts\python.exe -m pip install --upgrade pip
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

### 5. Selecione o kernel no VS Code

1. Abra `Parte1_Projeto1_Redes_Neurais.ipynb`.
2. Clique em **Select Kernel** ou **Selecionar Kernel**, no canto superior direito.
3. Escolha **Python Environments**.
4. Selecione o interpretador:

```text
.venv\Scripts\python.exe
```

Se ele não aparecer, pressione `Ctrl + Shift + P`, escolha **Python: Select Interpreter**, clique em **Enter interpreter path** e selecione manualmente o arquivo acima.

### 6. Execute o notebook completo

Confirme que `ex3data1.mat` está na mesma pasta do notebook ou dentro de uma subpasta chamada `dados`. Depois, no notebook, clique em:

```text
Run All / Executar Tudo
```

Por padrão, a célula de configuração utiliza:

```python
SEED = 2026
FAST_MODE = False
```

Na implementação, `FAST_MODE` é obtido da variável de ambiente `NN_FAST_MODE`. Sem essa variável, o modo completo é usado automaticamente. Ele executa os experimentos com os 5.000 exemplos e inclui o gradiente conjugado com limite de até 400 iterações.

### 7. Faça primeiro um teste rápido, se necessário

Para verificar a instalação sem executar toda a grade experimental, altere temporariamente na célula de configuração:

```python
FAST_MODE = True
```

Execute todas as células. Nesse modo, o notebook usa até 1.000 exemplos e uma grade reduzida de experimentos. Para produzir os resultados finais, restaure a linha original:

```python
FAST_MODE = os.getenv("NN_FAST_MODE", "0") == "1"
```

e execute novamente com `FAST_MODE = False` ou sem definir `NN_FAST_MODE`.

## Ordem dos experimentos

Ao executar todas as células, o notebook realiza:

1. leitura e validação dos dados;
2. exibição da distribuição das classes e de 25 imagens;
3. definição das rotinas matemáticas da rede;
4. checagem numérica do gradiente em uma rede pequena;
5. treinamento da arquitetura `400 → 25 → 10` por descida do gradiente;
6. gráfico da evolução da função de custo;
7. apresentação dos erros de classificação;
8. treinamento por gradiente conjugado para diferentes configurações;
9. geração das tabelas e dos gráficos comparativos;
10. visualização dos 25 filtros da primeira camada;
11. síntese automática e discussão crítica dos resultados.

## Problemas comuns

### `FileNotFoundError`

Verifique se pelo menos uma destas opções está disponível na pasta do notebook ou em `dados/`:

```text
ex3data1.mat
```

ou:

```text
imageMNIST.csv
labelMNIST.csv
```

### O ambiente `.venv` não aparece como kernel

Registre o kernel manualmente:

```powershell
.\.venv\Scripts\python.exe -m ipykernel install --user --name projeto1-venv --display-name "Python (Projeto1 .venv)"
```

Depois pressione `Ctrl + Shift + P`, execute **Developer: Reload Window** e selecione **Python (Projeto1 .venv)**.

### A execução está demorando

Isso é esperado no modo completo, sobretudo nos experimentos de gradiente conjugado com até 400 iterações. Utilize temporariamente `FAST_MODE = True` apenas para verificar se o ambiente e o código estão funcionando.

## Colaboração pelo GitHub

Antes de começar a trabalhar:

```powershell
git pull
```

Para desenvolver uma tarefa isoladamente:

```powershell
git switch -c nome-da-tarefa
```

Depois das alterações:

```powershell
git add .
git commit -m "Descrição objetiva da alteração"
git push -u origin nome-da-tarefa
```

Em seguida, abra um *Pull Request* no GitHub para que outro integrante revise a alteração antes de integrá-la à branch principal.

Evite que duas pessoas editem simultaneamente o mesmo notebook, pois arquivos `.ipynb` são documentos JSON e podem gerar conflitos difíceis de resolver. Antes dos commits intermediários, é útil limpar as saídas do notebook. Para a entrega final, uma pessoa deve executar todas as células na ordem e salvar o notebook com as saídas definitivas.

## Próxima etapa

A Parte 2 deverá reutilizar as funções e a arquitetura implementadas aqui, mas deverá reinicializar os pesos e treinar novamente após dividir os dados em treino, validação e teste. Os pesos ajustados com todas as 5.000 imagens na Parte 1 não devem ser usados na avaliação da Parte 2, pois isso causaria vazamento de dados.

## Referências principais

- Notas de aula da disciplina MS571, capítulos sobre regressão logística, redes neurais, retropropagação, regularização e avaliação de modelos.
- Enunciado do Projeto 1 — Parte I: Redes Neurais.
- Notebook-base `neural_networks.ipynb` fornecido com o projeto.
- GLOROT, X.; BENGIO, Y. *Understanding the difficulty of training deep feedforward neural networks*. AISTATS, 2010.
- LECUN, Y.; BOTTOU, L.; BENGIO, Y.; HAFFNER, P. *Gradient-based learning applied to document recognition*. Proceedings of the IEEE, 1998.
- NOCEDAL, J.; WRIGHT, S. J. *Numerical Optimization*. 2. ed. Springer, 2006.
- RUMELHART, D. E.; HINTON, G. E.; WILLIAMS, R. J. *Learning representations by back-propagating errors*. Nature, 1986.
