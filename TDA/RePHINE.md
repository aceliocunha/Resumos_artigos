# Resumo do Artigo: Going beyond persistent homology using persistent homology
Autores do artigo: Johanna Emilia Immonen, Amauri H Souza, and Vikas Garg
---

## 📄 Introdução à Homologia Persistente (PH) em Grafos

A **Homologia Persistente (PH)** é uma técnica de topologia computacional utilizada para analisar a "forma" dos dados em diversas escalas. Em grafos, ela rastreia características topológicas (como componentes conexas e ciclos) à medida que o grafo se transforma através de uma **filtração**.

* Uma **filtração** é uma sequência aninhada de subgrafos ($G_1 \subseteq G_2 \subseteq \dots \subseteq G$). O artigo foca em filtrações baseadas em **cores**:
    * **Filtração por cor de vértice:** Subgrafos são formados adicionando vértices gradualmente, seguindo a ordem de suas cores.
    * **Filtração por cor de aresta:** Subgrafos são formados adicionando arestas gradualmente, seguindo a ordem de suas cores.
* O resultado da PH é um **diagrama de persistência**, um multiconjunto de pares $(b, d)$, onde '$b$' é o valor da filtração onde uma característica topológica **nasce** e '$d$' é onde ela **desaparece**. A diferença $d-b$ indica a "vida" da característica. O foco do artigo são as características 0-dimensionais ($\beta_0$), que representam **componentes conexas**.

---

## 🚫 Limitações da PH Baseada em Cores

O artigo investiga o poder expressivo da PH com filtrações baseadas em cores, apontando limitações:

* **Inconsistência:** Filtrações com funções injetivas (que atribuem valores únicos a cada vértice) podem gerar diagramas diferentes para grafos isomórficos (**Lema 1**). Para resolver isso, são necessárias funções de filtro equivariantes à permutação (como as baseadas em cores ou atributos de GNN).
* **Poder Limitado:** Mesmo com filtrações baseadas em cores, a PH tem limites para distinguir grafos:
    * A PH por cor de vértice está ligada à capacidade de encontrar **conjuntos separadores de cor** (**Teorema 1**).
    * A PH por cor de aresta está ligada à capacidade de encontrar **conjuntos desconectores de cor** (**Teorema 2**).
    * Nenhuma das abordagens (vértice ou aresta) é estritamente mais poderosa que a outra (**Teorema 3**). Existem grafos que uma pode distinguir e a outra não, e vice-versa.
    * Existem grafos simples, não isomórficos, que **nenhuma** filtração baseada em cores consegue distinguir (Figura 4c do artigo).

---

## ✨ RePHINE: Indo Além da PH Tradicional

Para superar as limitações, o artigo propõe o **RePHINE**.

* **Ideia Central:** O RePHINE **refina** os diagramas de persistência gerados por filtrações de **cor de aresta** adicionando informações de **cor de vértice**.
* **Diagramas Aumentados:** Em vez de apenas $(b, d)$, o RePHINE usa tuplas $(b, d, \alpha, \gamma)$:
    * $b$ e $d$: Tempos de nascimento e morte, como na PH de aresta padrão.
    * $\alpha$: Para "buracos" que nascem em $b=0$ (componentes conexas), $\alpha$ codifica a cor do vértice "representante" dessa componente.
    * $\gamma$: Para buracos que nascem em $b=0$, $\gamma$ codifica informações sobre as cores das arestas vizinhas do vértice representante.
    * **"Buracos Faltantes" ($b=1$):** Representam arestas que formam ciclos, mas não causam a morte de componentes 0-dimensionais. Para eles, $\alpha$ e $\gamma$ são valores não informativos (ex: 0).
* **Benefícios do RePHINE:**
    * **Invariante a Isomorfismo:** Produz diagramas idênticos para grafos isomórficos (**Teorema 4**).
    * **Mais Expressivo:** É estritamente mais expressivo que a PH baseada em cores (vértice ou aresta), inclusive incorporando o poder das características 1-dimensionais (ciclos) através dos "buracos faltantes" (**Teorema 5**).

---

## 🤝 Integração com GNNs e Experimentos

Os diagramas RePHINE podem ser usados em conjunto com **Redes Neurais Gráficas (GNNs)**. As tuplas do RePHINE são processadas (por exemplo, via DeepSets), agrupadas e concatenadas com o *embedding* de nível de grafo da GNN para tarefas como classificação.

**Experimentos** em dados sintéticos e reais (benchmarks de classificação de grafos) confirmam a teoria:

* Em dados sintéticos, o RePHINE demonstra maior expressividade e capacidade de aprendizado.
* Em dados reais, o RePHINE combinado com GCN ou GIN consistentemente supera a PH padrão.

---

## 🎯 Conclusão

O artigo argumenta que a Homologia Persistente padrão em grafos tem limitações. Ele propõe o **RePHINE**, um método que aprimora a PH baseada em cores de aresta, adicionando informações de cor de vértice e o conceito de "buracos faltantes". O RePHINE é **teoricamente e experimentalmente mais expressivo** do que as abordagens baseadas em cores isoladas, além de ser invariante a isomorfismo, tornando-o uma ferramenta mais robusta para representação de grafos, especialmente quando combinado com GNNs.
