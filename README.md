# EcoGrid CEV
---

# Integrantes

| Nome                        | RM       |
| --------------------------- | -------- |
| Leonardo Gabriel Sá Duarte  | RM569029 |
| Enzo de Nadai               | RM569985 |
| João Pedro Conturbia Santos | RM569788 |
| Eduardo Oliveira da Silva   | RM570374 |
| Bruno Albuquerque Aguiar    | RM569035 |
| Timóteo De Andrade Romano   | RM569711 |

---

## Visão Geral

Este projeto implementa um sistema de carregamento de bateria que utiliza a energia elétrica local como fonte principal, com suporte opcional à energia solar armazenada para acelerar e reduzir o custo do processo de carga. O sistema foi desenvolvido e simulado no Tinkercad e é dividido em dois módulos funcionais independentes que atuam de forma complementar.

A ideia central é que o sistema de energia do local realize o carregamento normalmente. Quando há energia solar disponível e armazenada na bateria auxiliar, ela pode ser somada à fonte principal em série, elevando a tensão total e tornando o carregamento mais rápido e econômico, pois aproveita energia limpa já capturada pelo painel solar em vez de consumir mais energia da rede elétrica. Essa ativação não é obrigatória; o sistema opera perfeitamente sem ela, mas representa uma excelente opção para quem busca economia e menor tempo de recarga.

---

# Arquitetura do Sistema

## Parte 1 — Módulo de Carregamento Solar

O painel solar é responsável por captar energia e carregá-la na bateria auxiliar do sistema. O carregamento só ocorre quando duas condições são atendidas simultaneamente:

* O painel está gerando tensão suficiente, indicando que há incidência de luz solar;
* O switch de carregamento está ativado, representando um sensor que autoriza a entrada de corrente na bateria.

Quando ambas as condições são satisfeitas, o LED do circuito acende, indicando que a bateria auxiliar está sendo carregada pelo painel solar.

---

## Parte 2 — Módulo de Alternância de Fonte

Este módulo controla a fonte de tensão ativa no processo de carregamento por meio de um relé gerenciado por um Arduino Uno.

A fonte principal é uma fonte de bancada de 12V, que representa o sistema de energia elétrica do local onde o carregador está instalado. No estado padrão, com o relé desligado, o carregamento ocorre apenas com essa fonte. Nessa condição, a corrente medida é de aproximadamente 10,0 mA e a tensão no circuito é de 1,98 V.

Quando o relé é ativado, a bateria carregada pelo painel solar, representada por uma bateria de 9V, entra em série com a fonte de 12V, resultando em uma tensão combinada de 21V. Com isso, a corrente sobe para aproximadamente 18,9 mA e a tensão medida chega a 2,07 V.

O sistema passa a carregar utilizando energia parcialmente proveniente do sol, de forma mais rápida e com menor custo, já que parte da energia utilizada foi captada gratuitamente pelo painel solar.

Quando o usuário decide utilizar a bateria em conjunto com o sistema (decisão representada pelo botão), o Arduino ativa o relé para que essa mudança ocorra automaticamente.

---

# Justificativas Técnicas

O relé foi escolhido por garantir isolamento elétrico entre o circuito de controle do Arduino, que opera a 5V, e o circuito de potência, que opera entre 12V e 21V. Isso permite o chaveamento seguro sem a necessidade de transistores de potência externos para as correntes envolvidas.

A estratégia de colocar a bateria solar em série com a fonte principal é fundamentada na eletricidade básica: quando dois elementos são conectados em série, suas tensões se somam enquanto a capacidade de corrente permanece constante. Assim, os 12V da fonte de bancada somados aos 9V da bateria carregada pelo sistema solar resultam em 21V disponíveis para o carregamento.

O aumento da tensão produz diretamente um aumento da corrente para uma resistência constante, conforme a Lei de Ohm. Isso se reflete nos dados medidos na simulação: ao ativar o relé, a corrente sobe de 10,0 mA para 18,9 mA, um aumento de aproximadamente 89%, acelerando proporcionalmente a transferência de energia e reduzindo o tempo total de carregamento.

O modo de 21V não é obrigatório, pois a fonte de bancada já é suficiente para realizar o carregamento. A bateria solar entra como um recurso adicional que, quando disponível, agrega valor ao sistema, reduzindo o tempo de carga e aproveitando energia que já foi captada gratuitamente.

---

# Princípios de Energia Renovável e Sustentabilidade

O papel do painel solar não é apenas fornecer uma fonte alternativa de energia; ele funciona como um acumulador de energia limpa que é utilizada no momento do carregamento.

Em vez de depender exclusivamente da rede elétrica convencional, o sistema aproveita a energia solar captada durante o dia para reduzir a demanda sobre essa rede quando o carregamento ocorre.

O uso da bateria solar em série com a fonte principal representa uma forma eficiente de integrar energia renovável a um sistema já existente sem substituí-lo completamente. A rede convencional permanece como fonte principal, enquanto a energia solar atua como complemento que reduz custos e emissões associadas ao consumo elétrico.

Ao aumentar a tensão disponível com energia já captada gratuitamente pelo painel solar, o sistema reduz o tempo total de carregamento e, consequentemente, o tempo de consumo da rede convencional.

---

# Instruções de Uso

## Simulação do Módulo Solar

1. Abra o projeto no Tinkercad.
2. Certifique-se de que o switch está na posição ligada.
3. Inicie a simulação.
4. Observe o LED:

   * Se estiver aceso, a bateria auxiliar está sendo carregada pelo painel solar.
   * Se o switch for desligado, o carregamento é interrompido e o LED apaga imediatamente.

## Simulação do Módulo de Alternância

1. Abra o segundo circuito no Tinkercad.
2. Inicie a simulação.
3. Pressione o botão para simular a escolha do usuário de utilizar a fonte principal em conjunto com a bateria auxiliar.
4. Observe os valores exibidos no multímetro e no medidor de corrente.

### Relé desativado

* Fonte utilizada: 12V
* Corrente aproximada: 10,0 mA
* Tensão medida: 1,98 V

### Relé ativado

* Fonte utilizada: 12V + 9V em série
* Corrente aproximada: 18,9 mA
* Tensão medida: 2,07 V

O código de controle do Arduino está disponível neste repositório.

---

# Limitações da Simulação no Tinkercad

O Tinkercad não possui suporte à função de carregamento real de baterias, o que impossibilita simular o estado de carga ao longo do tempo.

Para contornar essa limitação, uma bateria de 9V é utilizada como elemento representativo da bateria previamente carregada pelo painel solar, enquanto o LED atua como indicador funcional do estado de carregamento ativo.

As células solares simuladas na plataforma também não variam seu comportamento de acordo com o ângulo de incidência da luz ou com a intensidade luminosa, simplificando o comportamento real do painel solar.
