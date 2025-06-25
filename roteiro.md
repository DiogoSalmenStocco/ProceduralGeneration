# Roteiro do vídeo

## Introdução
- Apresentação (Pessoal e Tema do vídeo) + tópicos
```
Olá! Meu nome é Diogo, sou estudante calouro do curso Bacharelado em Ciências da
Computação da USP de São Carlos. Nesse vídeo eu falarei um pouco sobre Geração Procedural,
o que é, quais propósitos, como é feita e exemplos. 
```

## Definição
- O que é?
```
Geração procedural é definida como um método de criação de dados ou estruturas por meio
de algoritmos e regras, geralmente com certa variabilidade - definida manualmente ou por
aleatoriedade.
```
- Pra quê serve?
```
E pra quê serve isso? Seu propósito é evitar trabalho manual, fazendo trabalhos cíclicos
enquanto garante naturalidade e aleatoriedade controlada, podendo, consequentemente,
gerar simulações do mundo real, com suas perfeitas imperfeições.
```
- Como é feita? falo de PRNG
```
Além de precisar de um determinado algoritmo direcionado para o propósito de uma geração
qualquer, é necessário encontrar um número aleatório que, de preferência, seja encontrado
novamente a fim de repetir certo resultado. Para isso, usaremos um Gerador de Número
Pseudo Aleatório, um PRNG (Pseudo Random Number Generator).
```

## PRNG
- O que é? (exemplo com LCG)
```
Um número pseudo aleatório não é aleatório de verdade, e para isso ele vem acompanhado de
uma semente - seed, no inglês. Por exemplo: Com a seed 0, tenho retorno 1. Com 5, 3. Com
7, 2. Enfim, com uma seed aleatória, eu tenho um valor ((pseudo)) aleatório. Nesse caso
didático, vamos limitar a seed e o retorno com valores de 0 a 8.
A conta por trás desse caso se chama LCG - Gerador Linear Congruente - e é relativamente
simples: calculamos a seed vezes 4 mais 1 e o resto desse valor dividido por 9 (de 0 a 8,
lembra?) ((visual: (SEED x 4 + 1) mod 9)). Esse caso é cíclico, conforme mostra a tabela
que eu coloquei aí pra vocês. Demonstrando rapidamente: 5 vezes 4 é 20, mais 1 dá 21, 
dividido por 9 resulta 18 com resto 3, a terceira linha da tabela. Usando a resposta como 
seed, você passa por todos valores.
```
| Fórmula                          | Resultado |
|----------------------------------|-----------|
| (0 x 4 + 1) mod 9                | 1         |
| (1 x 4 + 1) mod 9                | 5         |
| (5 x 4 + 1) mod 9 = 21 mod 9     | 3         |
| (3 x 4 + 1) mod 9 = 13 mod 9     | 4         |
| (4 x 4 + 1) mod 9 = 17 mod 9     | 8         |
| (8 x 4 + 1) mod 9 = 33 mod 9     | 6         |
| (6 x 4 + 1) mod 9 = 25 mod 9     | 7         |
| (7 x 4 + 1) mod 9 = 29 mod 9     | 2         |
| (2 x 4 + 1) mod 9 = 9 mod 9      | 0         |
| (0 x 4 + 1) mod 9                | 1 (repete)|

- Pra quê serve?
```
Assim, eu posso conseguir um número aleatoriamente - a seed - e, a partir dela, gerar
outro valor (ou valores) também aleatórios ((agora de mentira))
```
- Em criptografia
```
Curiosamente, esse conceito se encaixa muito bem em criptografia: a seed seria sua senha
e vários cálculos retornariam a mensagem encriptada. Obviamente, usos práticos são
complexos demais pra esse vídeo quando se trata de evitar hackers persistentes.
```
- Em jogos ou simulação (PRNG de Mersenne Twister = 2 ^ 19.937 ou 4,32 · 10 ^ 6001)
```
Para jogos e simulações como as que farei daqui a pouco, é utilizado o PRNG de Mersenne
Twister, cujos cálculos não vem ao caso mas que garante uma geração menos cíclica que a
anterior e atinge valores entre 0 e 2^19.937, considerando o uso de 19.937 bits. Isso
equivale a um valor qualquer entre 0 e 4 3 2 zero zero zero, 6 mil zeros! Isso ultrapassa
qualquer valor comum, bilhões têm 9 zeros, isso tem 6 mil!
```
- Versão mais simples (shaders ou jogo 3D multiplayer online)
```
Enfim, para jogos 3D multiplayers online e para shaders, a fim de evitar excesso de 
processamento, PRNGs como o LCG utilizado anteriormente são leves e  extremamente
convenientes, pontanto não devem ser descartados!
```

## L-Systems
- Algoritmo de geração procedural Sistemas Lindenmayer
```
Agora que temos a um gerador de números (pseudo) aleatórios, vamos gerar algo procedural
legal o suficiente pra esse vídeo. Utilizarei os Sistemas de Lindenmayer, ou L-Systems,
algoritmo que pode ser usado para vários propósitos, no caso: geração de árvores.
```
- Exemplo das árvores
```
Olha essa árvore, que coisa linda. Para gerar ela, utilizei parâmetros como tamanho inicial
, definhamento mínimo e máximo, chance de bifurcação, ângulo mínimo e máximo, profundidade
e seed. Perceba que alterando apenas a seed, temos árvores semelhantes mas nunca iguais.
Nesses casos mantive a chance de ramificação igual a 100%. Diminuindo pra 90%, e comparando
os resultados, repare como se trata da mesma árvore com galhos a menos, algumas com mais
azar, outras com menos. Voltando pra 100% e aumentando a profundidade, tenho árvores com
mais e com menos folhas, podendo simular uma possível primavera ou inverno sem trocar a
árvore inteira. Diferentes biomas teriam árvores naturalmente mais secas, mais redondas,
enfim, basta ser criativo com os parâmetros.
Visando economizar processamento, uma estratégia comum é gerar árvores em jogos apenas
quando o jogador se aproxima, e tudo que ele precisará ter salvo é a seed e a posição.
Assim, uma floresta inteira pode ser gerada e regerada sem alterações. Detalhe: as
posições das árvores bem como suas seeds também podem ser geradas proceduralmente, ou
seja, um único valor retornaria uma floresta inteira!
```

## Perlin Noise
- Explicação sintetizada
```
Outro algoritmo de geração procedural bastante familiar é o Perlin Noise e sua versão
simplificada, o Simplex Noise. Originalmente, esse método utiliza conceitos de Geometria
Analítica - para operações com vetores - e de Cálculo universitário - derivadas, no caso -
então vamos focar no seu propósito e retorno.
O principal uso evidente desse ruído é voltado para definição de terrenos, como no exemplo
gerado a partir de uma simulação do Perlin Noite. Mudando a seed é possível obter 
resultados bem distintos, e com aplicações mais precisas, o uso de parâmetros para criação
e segmentação de biomas com vegetação e relevo próprio se torna viável.
```

## Jogos de exemplo
```
O primeiro jogo relevante com geração procedural se chama Rogue, de 1980, que se preocupava
com masmorras aleatórias, garantindo que os jogadores não enjoassem facilmente. A inovação
resultou em um gênero completamente baseado nisso, os jogos Roguelike (como Rogue), por
exemplo Hades, Hollow Knight, Risk of Rain 2, The Binding of Isaac, Balatro, entre outros.
A tecnologia também dominou a geração de terrenos, muito notável nos jogos Minecraft, 
Civilization 6, No Man's Sky com seus 18 quintilhões de planetas num multiplayer de tempo
real e Elite Dangerous, que simula as 400 bilhões de estrelas da Via Láctea.
Esse último tem um feito incrível: esse usuário do Reddit comentou sobre quando os
desevolvedores de Elite Dangerous foram adicionar o Sistema planetário TRAPPIST-1, 
e, para a surpresa dos devs, ele já existia.
```
- Rogue (1980)
- Minecraft (ideia dos biomas)
- Civilization 6 (geração fractal)
- Elite Dangerous (400 bilhões de estrelas da Via Láctea)
- No Man’s Sky (18 quintilhões de planetas diferentes num multiplayer em tempo real)

## Outras utilizações
```
Além de jogos, a geração produral foi muito utilizada em grandes produções do cinema.
Um exemplo notável é o uso do algoritmo MASSIVE, usado desde o primeiro filme de O Senhor
dos Anéis, que garante individualidade e naturalidade em exércitos digitalmente gerados.
Outros filmes com exércitos ou uma grande massa de seres que posteriormente também 
utilizaram MASSIVE são: <lista abaixo>.
```
- Exemplos de filmes com MASSIVE
  - O Senhor dos Anéis, exigência do diretor Peter Jackson
  - Planeta dos Macacos
  - Avatar
  - As Crônicas de Nárnia
  - King Kong
  - Wall-E
  - Up: Altas Aventuras
  - As Aventuras de Pi
  - O Hobbit
  - Game Of Thrones
  - Pantera Negra
  - Aquaman
  - Vingadores: Ultimato

## Conclusão
- Resumo e tchau
```
Resumo da ópera: aprendemos o que é um número pseudo aleatório e seu uso para geração
procedural recuperável, bem como diferentes algoritmos com diferentes propósitos.
Também vimos diversos exemplos interessantíssimos de jogos e filmes que arrasaram com uma
mãozinha dessa tecnologia que é tão incrível.
Eu realmente espero que você tenha gostado do vídeo! Se puder responder ao formulário de
satisfação, que você encontra na descrição e no comentário fixada, eu serei eternamente
grato! Um grande abraço, e tchau tchau!
```
---
---

```
import matplotlib.pyplot as plt
import math
import random

def draw_tree(x, y, angle, length, depth, params, lines, rng):
    if depth == 0 or length < params["min_length"]:
        return

    rad = math.radians(angle)
    x2 = x + length * math.cos(rad)
    y2 = y + length * math.sin(rad)

    lines.append(((x, y), (x2, y2)))

    # Reduz comprimento com variação
    new_length = length * rng.uniform(params["shrink_min"], params["shrink_max"])

    # Ângulo base + variação
    delta = rng.uniform(params["angle_min"], params["angle_max"])

    # Ramificação para esquerda
    if rng.random() < params["branch_chance"]:
        draw_tree(x2, y2, angle - delta, new_length, depth - 1, params, lines, rng)

    # Ramificação para direita
    if rng.random() < params["branch_chance"]:
        draw_tree(x2, y2, angle + delta, new_length, depth - 1, params, lines, rng)

# --------------------------------------------
# PARÂMETROS CONTROLÁVEIS (você pode ajustar!)
# --------------------------------------------

params = {
    "initial_length": 200,
    "min_length": 2,
    "shrink_min": 0.7,
    "shrink_max": 0.8,
    "angle_min": 35,
    "angle_max": 50,
    "branch_chance": 1.0,
    "max_depth": 14,
    "seed": 13  # Altere para gerar outra árvore, ou mantenha para repetir
}

# --------------------------------------------
# EXECUÇÃO
# --------------------------------------------

rng = random.Random(params["seed"])  # PRNG com seed fixa

lines = []
draw_tree(0, 0, 90, params["initial_length"], params["max_depth"], params, lines, rng)

# Visualização
plt.figure(figsize=(8, 10))
for (x1, y1), (x2, y2) in lines:
    plt.plot([x1, x2], [y1, y2], color='green')

plt.title(f"Árvore Procedural (seed={params['seed']})")
plt.axis('equal')
plt.axis('off')
plt.show()

```


