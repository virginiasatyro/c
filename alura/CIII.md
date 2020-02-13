# Alura

## C III: Recursos avançados da linguagem

### Aula 1 Matrizes

#### Introdução

#### Entendendo as Matrizes


O primeiro passo para a implementação do Foge-Foge é pensar no mapa que nosso herói andará. Nele, precisamos representar tudo o que pode existir por lá, como paredes, monstros, o próprio herói, e lugares pela qual ele pode passar.

Podemos representar um mapa por meio de várias linhas e colunas. E, em cada posição, teríamos um caractere que representaria o que tem ali. Veja o exemplo do mapa abaixo, onde o ponto (.) significa "caminho livre", o arroba (@) representa nosso herói e os caracteres | e - representam paredes:

  0123456789
0 |--------|
1 |...|..-.|
2 |..-|.@..|
3 |......-.|
4 |--------|

Nosso herói que está na linha 2, coluna 6, pode andar para esquerda, direita, cima ou baixo. Mas não pode andar duas vezes seguidas para a esquerda, por exemplo, pois há uma parede ali.

Podemos representar cada linha desse mapa por meio de um array de caracteres. Como cada linha tem 10 posições, um array de 10 posições resolveria. E como temos 5 linhas, precisamos de 5 desses arrays para representar todo o mapa:

char linha1[10];
char linha2[10];
char linha3[10];
char linha4[10];
char linha5[10];

Claramente essa não é uma boa solução. Imagine se tivéssemos um mapa com 50 linhas? Precisaríamos ter 50 variáveis dessas. E manipulá-las não seria fácil. A solução para isso é deixar de usar vetores e passar a usar matrizes. Matrizes, assim como na matemática, é uma estrutura retangular de dados, ou seja, nosso "linhas e colunas".

Se quisermos uma matriz de caracteres de 5x10, declaramos uma variável para tal. Para declararmos matrizes em C, basta repetirmos duas vezes o conjunto de [], um para linhas e outro para colunas, respectivamente:

char mapa[5][10];

Acessar e alterar valores das posições é análogo ao uso de vetores, com a diferença que agora passamos ambas as posições:

// armazenando na posicao 0x0
mapa[0][0] = '|';

// imprimindo a posição 2x3
printf("%c", mapa[2][3]);


#### Declarando Matrizes
Declare uma matriz de char, chamada mapa que tenha 10 x 5 de tamanho.

A sintaxe é similar ao do array. Basta passar mais uma dimensão.

char mapa[10][5];

#### Salvando e lendo dados de uma matriz

Para que nosso jogo fique interessante, vamos lê-lo de um arquivo. Nosso programa deve ler linha a linha e salvá-las na matriz mapa. Vamos começar abrindo o arquivo, do jeito que já conhecemos:

FILE* f;

f = fopen("mapa.txt", "r");
if(f == 0) {
    printf("Erro na leitura do mapa");
    exit(1);
}

Agora precisamos mudar a declaração do nosso mapa. Apesar do mapa acima ter 10 caracteres na linha, precisamos lembrar que quando usarmos o fscanf(), passando uma string como máscara, ele colocará um \n ao final. Podemos também usar as funções de leitura que vimos no final do capítulo de I/O, como a fread(), e ler byte a byte, mas isso nos daria um trabalho desnecessário. Vamos declarar o array com uma posição a mais em cada linha, apenas para guardar o enter:

char mapa[5][10+1];

Com isso em mãos, podemos agora fazer o loop para capturar as linhas do arquivo. Apontamos mapa[i] como o lugar onde os caracteres devem ser colocados:

for(int i = 0; i < 5; i++) {
    fscanf(f, "%s", mapa[i]);
}

Podemos imprimir o mapa, de maneira simples por enquanto:

for(int i = 0; i < 5; i++) {
    printf("%s\n", mapa[i]);
}

Por fim, precisamos fechar o arquivo:

fclose(f);

Pronto. Já estamos lendo a matriz de mapa de um arquivo. Mas podemos melhorar ainda mais.

#### Imprimindo uma matriz


Imprima a matriz de inteiros abaixo:

int numeros[20][10];

Para isso, use dois fors.

O código ficará assim:

for(int i = 0; i < 20; i++) {
    for(int j = 0; j < 10; j++) {
        printf("%d ", numeros[i][j]);
    }
    printf("\n");
}


#### Para saber mais: Ponteiros de ponteiros

No curso anterior, discutimos que um array/vetor é simplesmente um ponteiro que aponta para uma posição da memória que tem espaços vazios para guardar as N posições declaradas.

Uma matriz é uma abstração ainda maior. Ela é um ponteiro que aponta para uma lista de arrays. Como arrays são ponteiros, então uma matriz é um ponteiro que aponta para uma lista de ponteiros.

Ou seja, mapa é um ponteiro que aponta para outros ponteiros. Isso significa que mapa[0] é um ponteiro que aponta para um array, mapa[1] é um ponteiro que aponta para outro array, e assim por diante. Veja na imagem abaixo:

Ponteiros de ponteiros

Em código, nada nos impede de declararmos uma matriz e depois declarar ponteiros que apontam para cada um dos vetores dentro dessa matriz. Veja o código abaixo. linha0[0] é igual a numeros[0][0], afinal tanto linha0 quanto numeros[0] apontam para o mesmo endereço de memória.

int numeros[5][10];

int* linha0 = numeros[0];
int* linha1 = numeros[1];
int* linha2 = numeros[2];
int* linha3 = numeros[3];
int* linha4 = numeros[4];

// os numeros serao iguais, afinal
// ambos ponteiros apontam para o mesmo
// endereço de memória
printf("%d %d", linha0, numeros[0]);

Ou seja, nesse loop que faremos, guardaremos a primeira linha no array mapa[0], a segunda linha no array mapa[1] e assim por diante. Veja que no código acima, declaramos int* para representar cada array que está dentro da matriz. Mas e se quiséssemos um novo ponteiro para apontar para a matriz como um todo? Como ela é um ponteiro que aponta para outros ponteiros de inteiro, a declaração é int**, ou seja, duas estrelas para representar "ponteiro de ponteiro":

int numeros[5][10];

// o ponteiro copia é idêntico ao
// ponteiro numeros... ambos apontam
// para uma lista de ponteiros de inteiros
int** copia = numeros;

// as duas operações abaixo são idênticas
numeros[0][0] = 10;
copia[0][0] = 10;

Ou seja, enquanto um array é um ponteiro para uma lista de posições de memória que estão livres para guardar o tipo escolhido, uma matriz é um ponteiro que aponta para uma lista de ponteiros, que por sua vez, apontam para posições de memória livres para guardar algo.

#### Alocação dinâmica de memória


Mas e se quisermos mapas com tamanhos diferentes, para, por exemplo, diferenciar o nível do jogo? Quanto mais difícil, maior o mapa. O grande problema é: como variar o tamanho da matriz?

Uma possível estratégia seria ter uma matriz gigante, que suportasse o maior mapa possível. Por exemplo, uma matriz mapa[100][100]. Nosso programa então olharia apenas para a quantidade de linhas e colunas daquele mapa, em particular, que será sempre menor que 100x100.

Aqui, tomaremos outra decisão: a de gerar um array (ou matrizes) de tamanhos aleatórios. Leremos o arquivo, descobriremos o tamanho do mapa, e então, declararemos uma matriz com o tamanho ideal.

Para fazer isso, precisamos mudar a maneira de declararmos nosso array, alocando a memória necessária em tempo de execução. Para isso, usaremos a função malloc(). Ela recebe como parâmetro a quantidade de bytes que precisa ser alocado, e nos devolve um ponteiro para o primeiro byte alocado.

O código abaixo, por exemplo, aloca um único byte. Como ele nos devolve um ponteiro, o seu retorno pode ser armazenado em um ponteiro de char:
```
char* letra = malloc(1);
*letra = 'M';
```
Se quiséssemos guardar um int precisaríamos de mais de 1 byte, afinal inteiros são representados por 4 bytes. O código então seria algo como:
```
int* numero = malloc(4);
*numero = 20;
printf("%d", *numero);
```
Mas como a quantidade de bytes que um inteiro ocupa pode variar de plataforma para plataforma, é melhor dizermos ao compilador para usar o tamanho do inteiro daquela plataforma. Para isso, usamos a instrução sizeof(int), que nos retorna o tamanho do inteiro correto:
```
int* numero = malloc(sizeof(int));
*numero = 20;
printf("%d", *numero);
```
Como o malloc() simplesmente aloca a quantidade de bytes desejada e nos devolve um ponteiro, para declararmos um array de maneira dinâmica, basta apenas passarmos a quantidade de bytes corretos. Por exemplo, se quisermos um array de inteiros de 10 posições, precisamos passar 40 para ele (4 bytes por inteiro vezes 10 inteiros). Com esse número, sabemos que podemos manipular um ponteiro como se fosse um array:
```
// calcula o tamanho de bytes
int colunas = 10;
int memoria = sizeof(int) * colunas;

// aloca memoria suficiente para o array
int* numeros = malloc(memoria);

// usa o ponteiro como se fosse um array
numeros[2] = 10;
printf("%d", numeros[2]);
```
Já para uma matriz, temos mais de uma abordagem. Imagine uma matriz de 5 linhas por 10 colunas. Precisamos primeiro alocar um array para guardar 5 ponteiros de inteiros. Depois, alocar dez arrays de 5 arrays de 10 posições, e guardar cada um desses arrays nos ponteiros de inteiros declarados anteriormente.

Veja, em código:
```
int** matriz;

int linhas = 5;
int colunas = 10;

// alocando espaço para as linhas,
// que guardam ponteiro de inteiro.
matriz = malloc(sizeof(int*) * linhas);

// agora, para cada linha, alocamos
// espaço para um array com 10 ("colunas") posições.
for(int i = 0; i < linhas; i++) {
    matriz[i] = malloc(sizeof(int) * colunas);
}
```
// agora podemos usar 'matriz' como uma matriz
matriz[2][3] = 10;

É isso que faremos então.

#### Alocando matrizes dinamicamente

As dimensões de uma matriz estão declaradas nas variáveis abaixo:

int linhas = 5;
int colunas = 10;

Aloque essa matriz de maneira dinâmica.

O código ficará assim:
```
// alocando espaço para as linhas,
// que guardam ponteiro de inteiro.
matriz = malloc(sizeof(int*) * linhas);

// agora, para cada linha, alocamos
for(int i = 0; i < linhas; i++) {
    matriz[i] = malloc(sizeof(int) * colunas);
}
```

#### Separando o código em funções

Vamos recomeçar nosso código já o separando em funções (código organizado sempre). Vamos declarar algumas variáveis globais, que serão importantes ao longo do jogo: o tamanho do mapa e o mapa em si:
```
#include <stdio.h>
#include <stdlib.h>

char** mapa;
int linhas;
int colunas;
```
Nosso mapa.txt também terá um formato diferente. A primeira linha terá dois inteiros, indicando o tamanho do mapa que virá a seguir. No mapa atual, a primeira linha seria "5 10". E ler isso é fácil, dado que sabemos usar bem a função fscanf(). Usaremos esses números para então alocar o tamanho do mapa de maneira dinâmica, que será feito na função alocamapa():
```
void lemapa() {
    FILE* f;
    f = fopen("mapa.txt", "r");
    if(f == 0) {
        printf("Erro na leitura do mapa");
        exit(1);
    }

    fscanf(f, "%d %d", &linhas, &colunas);
    alocamapa();

    for(int i = 0; i < linhas; i++) {
        fscanf(f, "%s", mapa[i]);
    }

    fclose(f);
}
```
A função alocamapa() contém um código bastante similar ao que usamos acima para entender alocação dinâmica. A diferença é que agora temos ponteiros para char. Apesar de sabermos que char é armazenado sempre em 1 bytes, vamos usar sizeof() para nunca termos problema. Lembre-se que precisamos adicionar mais 1 byte, para guardar o enter que é salvo pelo fscanf():
```
void alocamapa() {
    mapa = malloc(sizeof(char*) * linhas);

    for(int i = 0; i < linhas; i++) {
        mapa[i] = malloc(sizeof(char) * colunas + 1);
    }
}
```
Nossa main por enquanto apenas lerá o mapa e o imprimirá, para termos a certeza que tudo está funcionando como gostaríamos:
```
int main() {

    lemapa();

    for(int i = 0; i < linhas; i++) {
        printf("%s\n", mapa[i]);
    }
}
```
Para também não termos problemas com ordem da declaração das funções, vamos desde já colocar todas as assinaturas dentro do fogefoge.h, que é importado no começo do nosso código-fonte:
```
// fogefoge.h
void alocamapa();
void lemapa();
```
Pronto. Nosso jogo agora é capaz de ler mapas de qualquer tamanho. Mas ainda temos um problema. Você se lembra que no capítulo de I/O, discutimos que todo recurso aberto deve ser fechado. Ou seja, se abrimos um arquivo, precisamos fechá-lo.

O mesmo acontece com memória que é alocada dinamicamente. Sempre que usamos malloc(), o sistema operacional nos reserva um pedaço da memória e não permite com que nenhum outro programa encoste nele. No entanto, nosso programa é o responsável também por liberar essa memória de volta ao SO. Fazemos isso por meio da função free(). Ou seja, para cada malloc(), precisamos de um free() ao final.

Veja a função liberamapa(), que navega pela matriz, liberando cada um dos arrays alocados dinamicamente. Ao final, libera também a matriz como um todo:
```
void liberamapa() {
    for(int i = 0; i < linhas; i++) {
        free(mapa[i]);
    }

    free(mapa);
}
```
Basta agora a invocarmos ao final do nosso programa, quando temos a certeza de que o mapa não será mais utilizado:
```
int main() {

    lemapa();

    for(int i = 0; i < linhas; i++) {
        printf("%s\n", mapa[i]);
    }

    liberamapa();
}
```
Agora, ao rodarmos o programa, temos o mapa como saída:
```
|--------|
|...|..-.|
|..-|.@..|
|......-.|
|--------|
```

#### Começando o jogo

Escreva todo o código do Foge-Foge mostrado pelo instrutor até o fim do vídeo.
https://gist.github.com/mauricioaniche/83a9b17ad588b37d9431

#### O que aprendemos?

Nesta aula, aprendemos:

- A declarar e manipular matrizes
- O que são ponteiros de ponteiros
- Que matrizes são, no fim, ponteiros de ponteiros
- A declarar memória dinamicamente, usando malloc()
- Que a instrução sizeof() nos devolve o tamanho em bytes de um tipo específico naquela plataforma
- Que devemos liberar a memória alocada dinamicamente ao fim, com free()

### Aula 2 - Structs

#### Fazendo o herói andar

Nosso próximo passo é fazer nosso herói andar. Como todo jogo, usaremos a combinação de teclas A, S, D, W. A ideia será a seguinte: exibiremos o mapa, leremos uma entrada do teclado, faremos as atualizações necessárias no mapa, e repetiremos tudo de novo, até que o jogador ganhe ou perca.

Vamos começar com nosso loop principal, que será parecido com o do nosso jogo de forca. Usaremos a função scanf(), do jeito que você já está acostumado. Lembre-se do espaço em branco para que o enter seja ignorado:

int main() {

    lemapa();

    do {

        imprimemapa();

        char comando;
        scanf(" %c", &comando);
        move(comando);

    } while (!acabou());

    liberamapa();
}

A função acabou(), como o seu próprio nome diz, nos informará se o jogo acabou ou não. Por enquanto, vamos fazer uma função "falsa", que sempre nos retorna "0". Ou seja, por enquanto, o jogo nunca acaba:

int acabou() {
    return 0;
}

A função imprimemapa() é também bastante simples. Ela apenas imprime o mapa, do jeito que ele está na matriz:

void imprimemapa() {
    for(int i = 0; i < linhas; i++) {
        printf("%s\n", mapa[i]);
    }
}

Com o comando em mãos, podemos mover o herói para a direção que o jogador pediu. O primeiro passo do algoritmo deve ser localizar o herói. Para isso, não temos como fugir de varrer toda a matriz, usando dois loops encadeados:

void move(char direcao) {
    int x;
    int y;

    for(int i = 0; i < linhas; i++) {
        for(int j = 0; j < colunas; j++) {
            if(mapa[i][j] == '@') {
                x = i;
                y = j;
                break;
            }
        }
    }

}

Em seguida, modifica o mapa de acordo com a direção escolhida: A vai para a esquerda, D para a direita, S para baixo e W para cima. Se o jogador andou pra esquerda, devemos deslocar o herói para lá, ou seja, para a linha, coluna-1). Se ele andou para direita, o herói vai para linha, coluna+1. Se andou para cima, vai para linha-1, coluna, e se andou para baixo, vai para linha+1, coluna. Ao final, devemos colocar como vazio a posição que o herói estava antes. Resolvemos isso com um switch:

void move(char direcao) {
    // os fors aqui...

    switch(direcao) {
        case 'a':
            mapa[x][y-1] = '@';
            break;
        case 'w':
            mapa[x-1][y] = '@';
            break;
        case 's':
            mapa[x+1][y] = '@';
            break;
        case 'd':
            mapa[x][y+1] = '@';
            break;
    }

    mapa[x][y] = '.';
}

Se você rodar o jogo, verá que ele já começa a se parecer com o que queremos. Conseguimos movimentar nosso personagem para todas as direções. Mas ainda estamos longe de terminá-lo: se digitamos uma letra inválida, o herói desaparece. Ele também atravessa paredes. Temos muito a corrigir.

#### Definindo uma struct

Repare que sempre que usamos o mapa, usamos também as variáveis linhas e colunas. Afinal, sempre precisamos delas para navegar na matriz. Como essas variáveis são globais, não temos problemas para usá-las. Mas e se precisássemos passá-las como parâmetros para alguma função? Seria trabalhoso; teríamos muitos parâmetros.

Pela primeira vez temos um conjunto de variáveis que "não fazem sentido separadas". A matriz só faz sentido se tiver uma linha e coluna junto. Precisamos agrupá-las, e garantir que elas sempre serão "transportadas" juntas. Para tal, usaremos uma struct. Estruturas (ou, em C, struct) são uma maneira que temos para agrupar variáveis. Ou seja, definiremos que a estrutura "mapa" contém uma matriz (o ponteiro de ponteiro de char), uma variável para guardar a quantidade de linhas (int) e outra para guardar a quantidade de colunas (também int). Com essa estrutura definida, podemos declarar variáveis que a seguem. Dessa forma, garantiremos sempre que mapas tem uma matriz, e a quantidade de linhas e colunas.

Veja a declaração da struct abaixo, que colocamos dentro do nosso fogefoge.h. Dentro dele, declaramos a matriz e os dois inteiros para guardar as suas dimensões:
```
struct mapa {
    char** matriz;
    int linhas;
    int colunas;
};
```
Como dissemos anteriormente, com essa struct em mãos, podemos fazer uso dela e declarar variáveis desse tipo. Mas diferente de um inteiro ou um char que usamos até então, ela tem variáveis dentro dela. Para acessá-las, usamos ponto (.). Veja o trecho de código abaixo, por exemplo, onde definimos um mapa e setamos os valores para linhas e colunas. Repare na palavra struct no começo da declaração:
```
struct mapa facil;

char tabuleiro[10][20];
facil.matriz = &tabuleiro;
facil.linhas = 10;
facil.colunas = 20;
```
printf("O mapa tem tamanho %d x %d", facil.linhas, facil.colunas);

Vamos fazer as devidas alterações no código, usando agora a struct que definimos. A começar pelas variáveis globais, que são substituídas:
```
struct mapa m;
```
Em todo lugar que fazia uso de linhas ou colunas, precisamos trocar para m.linhas e m.colunas. Em todo lugar que tínhamos a matriz mapa, agora precisamos usar m.matriz.

Um detalhe importante é perceber precedência de operadores. Por exemplo, dentro da função lemapa(), temos o fscanf(), que agora ficou parecido com o código abaixo:
```
fscanf(f, "%d %d", &m.linhas, &m.colunas);
```
A instrução &m.linhas é processada corretamente. Passamos o endereço da variável linhas que está dentro da struct m. Uma outra interpretação dessa instrução seria que o & é para a variável m, ou seja, gostaríamos do endereço de memória da estrutura m como um todo. Para evitar essa possível ambiguidade, é comum fazermos uso de parênteses para deixar claro a ordem de precedência:
```
fscanf(f, "%d %d", &(m.linhas), &(m.colunas));
```
A função move(), com todas as alterações, fica como a abaixo:
```
void move(char direcao) {
    int x;
    int y;

    for(int i = 0; i < m.linhas; i++) {
        for(int j = 0; j < m.colunas; j++) {
            if(m.matriz[i][j] == '@') {
                x = i;
                y = j;
                break;
            }
        }
    }

    switch(direcao) {
        case 'a':
            m.matriz[x][y-1] = '@';
            break;
        case 'w':
            m.matriz[x-1][y] = '@';
            break;
        case 's':
            m.matriz[x+1][y] = '@';
            break;
        case 'd':
            m.matriz[x][y+1] = '@';
            break;
    }

    m.matriz[x][y] = '.';
}
```
Apesar da sensação de que agora temos muito mais código escrito, usar structs para agrupar dados que devem ficar perto é uma boa prática. Você verá que, em breve, deixaremos de usar variáveis globais, e começaremos a passar essas structs pra lá e pra cá. Mas, como elas agrupam tudo o que precisamos, fica fácil: a função receberá apenas um ponteiro para a estrutura.
