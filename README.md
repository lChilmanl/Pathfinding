# Pathfinding
Primeira tentativa de algoritmo de Pathfinding na Unity, utilizando A* e otimização com Heap

*Boa parte do código está comentado e o projeto foi feito para que não fosse muito difícil de etender o que está acontecendo, mas para sanar qualquer dúvida aqui tem uma explicação mais abstrata de o que está acontecendo reunida em um lugar só.*


## Sobre o projeto Unity

![Unity_zaTsWrCkUD](https://user-images.githubusercontent.com/64097254/134823056-5732323c-d74a-4d64-be4c-93b4a990de00.png)
Primeiramente é bom lembrar que como já estava funcionando no ambiente do labirinto resolvi fazer a mudança para um ambiente mais orgânico e que possibilitasse movimentos diagonais, então não é mais algo tão reto que nem o labirinto inicial.

  ### VISUALIZAÇÃO
   - Toda visualização dos caminhos encontrados pelo algoritmo assim como da identificação de paredes e caminhos difíceis é feita por Gizmos, então só é possível ver no editor durante a execução
   - Por padrão é possível ver apenas os caminhos encontrados que as entidades seguem  
      ![Unity_1321oqwy12](https://user-images.githubusercontent.com/64097254/134823166-c8b2300b-1364-4507-9ee3-22c375e37653.png)
      + Cada entidade usa uma cor aleatória para representar seu caminho
   - Caso deseje ver também a grid que identifica paredes e caminhos difíceis basta habilitar a opção "Show Gizmos" no objeto "Grid"
      ![Unity_kgqV2wRGqA](https://user-images.githubusercontent.com/64097254/134823246-26f5bf79-c20b-49b4-8a89-1c78d1ff58a8.png)
      + Caminhos normais são representados em cor neutra
      + Caminhos difíceis (seria uma espécie de grama alta) são representados em verde
      + Paredes (não caminháveis) são representados em vermelho

  ### ALGORITMO
   - Todas entidades mandam pedidos para um gerenciador para que esse retorne os caminhos que devem seguir, uma fila de pedidos é criada e atendida por "ordem de chegada"
   - O algoritmo primeiramente cria uma matriz de nodos já identificando qual o tipo desse nodo:
      + Caminho normal - custo 1
      + Caminho difícil - custo 2
      + Parede (não caminhável)
   - Encontrando o caminho:
      + O algoritmo atua com um conjunto de nodos abertos e fechados (abertos é uma Heap<>, fechados é uma HashSet<>)
      + Começando com o nodo referente à posição da entidade sendo o único no conjunto de abertos
      + Então o loop que verifica todos nodos necessários se inicia:
        + É selecionado o nodo do conjunto aberto com menor custo F (geralmente é só a soma da distância à entidade com a distância ao objetivo final, mas aqui possui uma adição do custo daquele tipo de caminho, "normal ou difícil")
          + Esse nodo é identificado utilizando uma estrutura de Heap<>, que deixa o processo muito mais otimizado que uma List<>
        + Esse nodo é retirado do conjunto aberto e colocado no conjunto fechado
        + Caso esse nodo seja o mesmo referente à posição do objetivo o loop se encerra pois o caminho foi encontrado
        + Para cada nodo vizinho é feito uma séria de ações:
          + Se o vizinho está no conjunto fechado ou não é caminhável ele é ignorado e passa para o próximo
          + Um novo custo G é calculado de acordo com o caminho que foi tomado para chegar até esse vizinho, caso seja menor que o que já está registrado ou ainda não possua nenhum então o novo custo é registrado nesse vizinho
          + O nodo atual é registrado como o nodo pai desse vizinho
          + Se o nodo vizinho ainda não está no conjunto aberto então é colocado lá
