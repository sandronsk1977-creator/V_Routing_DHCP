# Manual do Aluno • Laboratório de VLAN, Inter-VLAN Routing e DHCP

Este manual explica como usar o laboratório virtual, o que fazer em cada etapa e
o que significa cada parte da tela. Ele não entrega a resposta: o laboratório
tem dicas em três níveis e um gabarito protegido por senha, que o professor
libera quando quiser.

---

## 1. O que você vai aprender

Ao final, você terá configurado uma rede real de três departamentos em uma
empresa fictícia chamada SF Academia Cyber:

| Departamento | VLAN | Rede | Gateway |
|:---|:---:|:---|:---|
| TI | 10 | 172.16.10.0/24 | 172.16.10.1 |
| RH | 20 | 192.168.20.0/24 | 192.168.20.1 |
| Marketing | 30 | 192.168.30.0/24 | 192.168.30.1 |

A ideia é segmentar a rede com VLANs, ligar os switches com troncos, criar o
roteamento entre as VLANs no roteador e entregar endereços automaticamente por
DHCP.

---

## 2. Antes de começar: leia a Aula

Na primeira vez que você abre o laboratório, aparece automaticamente a
**Aula Challenge • Antes de iniciar**. Leia antes de configurar qualquer coisa.

São 9 seções, do básico ao avançado: o problema do mundo real, o que é VLAN,
access x trunk, por que o switch não roteia entre VLANs, DHCP e o processo
DORA, as vantagens do cenário, o mapa de ataque, o glossário e como mexer no
laboratório.

**Termo em azul tracejado é clicável.** Clique e a definição aparece na hora,
sem sair do texto. Termos que aparecem muito: gateway, sub-interface, trunk,
pool, lease, Discover, Offer, Request e Ack.

Quando terminar, clique em **Começar o laboratório**. Para reler depois, use o
botão **Aula** no alto da página.

---

## 3. Conhecendo a tela

A página tem duas colunas.

**Coluna da esquerda**

| Cartão | Para que serve |
|:---|:---|
| Topologia do laboratório | A rede desenhada. É o seu mapa. |
| Testes de conectividade | Pings entre os hosts, com o motivo de cada falha. |
| Equipamento selecionado | As portas do equipamento que você clicou. |
| Endereçamento de hosts | Onde você digita o IP dos PCs ou pede por DHCP. |

**Coluna da direita**

| Cartão | Para que serve |
|:---|:---|
| Objetivo | O resumo do desafio. |
| Trilha de aprendizagem | As 4 etapas, suas competências e seus erros. |
| Tarefas do desafio | As 12 tarefas, agrupadas por etapa. |
| Console | Onde você digita os comandos. |
| Escopo DHCP | A faixa de endereços que o servidor DHCP entrega. |

Todos os cartões podem ser recolhidos e abertos de novo. O botão de
rótulos na topologia liga e desliga os nomes das conexões.

---

## 4. Os quatro gestos básicos

**Ver a configuração de um equipamento**: passe o mouse por cima dele na
topologia. Aparece um balão com o que está configurado agora.

**Abrir o console**: dê um duplo clique no equipamento. O console simula o
Cisco IOS, então os comandos são os de verdade:

```
enable
configure terminal
show running-config
```

Complete comandos com **Tab**, veja as opções de um comando com **?** e limpe a
tela com **clear**.

**Configurar um PC**: na tabela de endereçamento de hosts, clique no botão de
edição da linha e digite IP, máscara e gateway. Para PC5 e PC6 existe também o
botão **DHCP**, que pede o endereço ao roteador.

**Testar**: no cartão de testes, escolha a origem e o destino e clique em
ping. O pacote é desenhado atravessando a topologia e o resultado mostra o
caminho real e a causa da falha.

---

## 5. As quatro etapas

A trilha funciona como um roteiro. Você pode pular etapas, mas o painel e o
certificado vão mostrar o que ficou para trás.

### Etapa 1 • Fundamentos de segmentação

**Objetivo:** criar as três VLANs, colocar as portas dos PCs em access e ligar
os switches com trunk.

**Tarefas 1 a 4.**

O que você vai fazer, na ordem:

1. Criar e nomear a VLAN 10 no SW1, a 20 no SW2 e a 30 no SW3.
2. Em cada switch, colocar as duas portas dos PCs em access, cada uma na VLAN
   do seu departamento.
3. Transformar as ligações entre switches em trunk e liberar as três VLANs.

Quando essa etapa fechar, os três switches passam a se enxergar, mas ainda
ninguém consegue falar com o outro departamento. Isso é proposital.

### Etapa 2 • Roteamento entre VLANs

**Objetivo:** criar as três sub-interfaces no roteador, configurar os
endereços de PC1 a PC4 e provar que as VLANs se comunicam.

**Tarefas 5, 6 e 9.**

O que você vai fazer:

1. Criar as sub-interfaces Gi0/0.10, Gi0/0.20 e Gi0/0.30, cada uma com o
   encapsulation da VLAN, o endereço do gateway e o no shutdown.
2. Digitar o endereço manual de PC1, PC2 (TI) e PC3, PC4 (RH), com máscara
   255.255.255.0 e o gateway da VLAN de cada um.
3. Rodar o teste de comunicação entre as VLANs.

### Etapa 3 • Serviço DHCP

**Objetivo:** criar o pool no roteador e fazer PC5 e PC6 receberem endereço
sozinhos.

**Tarefas 7 e 8.**

O que você vai fazer:

1. Criar o pool DHCP com a rede 192.168.30.0, a máscara 255.255.255.0 e o
   gateway 192.168.30.1.
2. Na tabela de hosts, clicar em **DHCP** nas linhas de PC5 e PC6.

Ao clicar, o cartão **Escopo DHCP** mostra a animação do processo DORA:
Discover, Offer, Request e Ack. Leia as quatro linhas, porque é nelas que você
entende o que o servidor entregou.

**O escopo**: por padrão o servidor entrega endereços de 192.168.30.100 a
192.168.30.150. Você pode mudar essa faixa no cartão Escopo DHCP e reservar
endereços com exclusões, para que nenhum PC receba um IP que você quer usar em
outro equipamento. PC5 e PC6 vão receber o primeiro e o segundo endereço
livres do escopo.

### Etapa 4 • Validação e entrega

**Objetivo:** rodar todos os testes, corrigir o que falhar, salvar e exportar.

**Tarefas 10, 11 e 12.**

O que você vai fazer:

1. Rodar o ping dos seis hosts contra o gateway da própria VLAN.
2. Rodar os testes cruzados entre os departamentos.
3. Confirmar as 12 tarefas.

---

## 6. A checagem de compreensão

Cada etapa tem três perguntas que **não contam nota**. A ideia é você conferir
se entendeu o conceito, e não só se acertou o comando.

Se você errar, a tela mostra a alternativa correta e a explicação do porquê,
com um botão que leva direto para a seção da Aula que trata do assunto. A
etapa fica marcada como "revisar" até você refazer.

Acertou as três, aparece "Compreensão confirmada" e a etapa segue para as
tarefas.

---

## 7. Quando travar: use as dicas

Cada tarefa tem o botão **Pedir dica**, que revela três níveis, um de cada
vez:

| Nível | O que mostra |
|:---:|:---|
| 1 | A ideia: o que fazer e por quê. |
| 2 | Os comandos-chave. |
| 3 | A sequência completa, linha por linha. |

Depois de mostradas, o botão vira **Ocultar dicas**. Você pode pedir de novo
quantas vezes quiser, sem perder progresso.

Não tem vergonha de usar. O laboratório registra as 12 tarefas de qualquer
forma. Usar a dica nível 3 e entender vale mais do que acertar sozinho e não
saber por quê.

---

## 8. Seus erros viram matéria de estudo

O cartão **Meus erros** mostra os erros que você mais repete, quantas vezes
cada um aconteceu e a explicação de cada um.

Ele reconhece oito tipos de erro comuns, entre eles esquecer o no shutdown,
faltar encapsulation dot1Q, deixar de digitar o ip address do gateway, criar
porta access antes da VLAN existir e criar o trunk sem liberar as VLANs.

Use esse cartão como lista de revisão antes de tentar de novo. Corrigir o erro
que mais se repete destrava várias tarefas de uma vez.

---

## 9. Modo prática: erre sem medo

O botão **Modo prática**, no cartão da trilha, desliga a contagem da nota e
libera o gabarito. As tarefas continuam sendo validadas normalmente, então
você pode testar, errar, usar o gabarito e refazer quantas vezes quiser.

Depois é só clicar de novo para voltar ao modo avaliação.

Sugestão de uso: ligue o modo prática, monte a rede do seu jeito, compare com o
gabarito e desligue antes de começar a validação de verdade.

---

## 10. Ao terminar

### 10.1 Salve a configuração

Em cada um dos 5 equipamentos (R1, SW1, SW2, SW3 e SW4), entre no console e
execute:

```
enable
copy running-config startup-config
```

Sem isso a configuração fica só na memória do equipamento e é perdida se a
aba for fechada.

### 10.2 Confira o resultado

Os comandos de verificação não são todos do mesmo equipamento:

| Console | Comandos |
|:---|:---|
| SW1, SW2 e SW3 | `show vlan brief` • `show interfaces trunk` |
| SW4 | `show interfaces trunk` (não tem VLAN, é o núcleo) |
| R1 | `show ip interface brief` • `show ip dhcp pool` • `show ip dhcp binding` |

### 10.3 Exporte a entrega

Clique em **Exportar .cfg**. O arquivo reúne o running-config dos 5
equipamentos, o endereçamento dos hosts e a lista de tarefas.

### 10.4 Guarde o certificado

O botão **Concluir** mostra o certificado com a nota, o percurso das 4 etapas,
as competências e os pontos para revisar. Use Ctrl+P ou o menu do navegador
para imprimir ou salvar em PDF.

### 10.5 Envie

Envie o arquivo `.cfg` e o certificado na plataforma que o professor indicar.

---

## 11. Perguntas frequentes

**Posso fechar o laboratório e continuar depois?**
Pode. Tudo é salvo sozinho no navegador. Abrindo de novo, você volta exatamente
onde parou. Use **Reiniciar** para zerar e começar do zero.

**A topologia mostra a linha tracejada. Isso é erro?**
Não. Tracejado cinza significa que o enlace está caído, ou seja, falta
configurar trunk em uma das pontas. Laranja tracejado significa que o tronco
está ativo mas sem todas as VLANs liberadas. Passe o mouse sobre o enlace para
ver o motivo exato.

**Por que o switch não liga os departamentos?**
Porque switch não roteia entre VLANs. Isso é proteção, não defeito. Quem faz
esse caminho é o roteador, com uma interface dentro de cada VLAN.

**Por que o ping falha mesmo com tudo configurado?**
Na maioria das vezes é sub-interface sem no shutdown, ou host com o gateway
errado. O cartão **Meus erros** mostra o que mais se repete no seu caso.

**Posso usar endereços diferentes dos da tabela?**
Pode, desde que cada PC esteja na mesma rede do gateway da VLAN dele, com
máscara coerente. O validador do professor aceita qualquer endereçamento
válido.

**O DHCP atende todas as VLANs?**
Não, e isso é de propósito. O pool foi criado dentro da sub-interface da
VLAN 30, e o DHCP só atende quem está na mesma VLAN. É assim que o desafio
compara o endereçamento automático com o manual.

**Onde eu vejo o gabarito?**
No botão **Gabarito**, no alto da lista de tarefas. O professor informa a
senha. No modo prática ele já vem liberado.

**Para que serve o botão Validar .cfg?**
É do professor, não seu. Ele importa o arquivo que você entregou, reconstrói a
configuração do zero e roda as 12 tarefas de novo para conferir se a entrega
está correta. Não mexa nesse botão.

---

## 12. O que é DORA

DORA não é um protocolo. É o nome dado às quatro mensagens que o PC e o
servidor DHCP trocam para um computador sair daRede sem endereço nenhum. Cada
letra do nome é a inicial de uma mensagem, nesta ordem.

| Letra | Mensagem | Quem envia | O que acontece |
|:---:|:---|:---|:---|
| **D** | Discover | o PC | "existe algum servidor DHCP aqui?" O PC que não tem IP dispara um broadcast pedindo ajuda. |
| **O** | Offer | o servidor | "tenho o 192.168.30.100 livre, com máscara, gateway e DNS". |
| **R** | Request | o PC | "quero esse mesmo". O PC precisa confirmar, porque pode haver mais de um servidor na rede. |
| **A** | Ack | o servidor | "confirmado". O PC salva IP, máscara, gateway e DNS e considera a rede pronta. |

**Por que são quatro mensagens e não duas?** Porque a rede pode ter mais de um
servidor DHCP. Se dois servidores oferecerem o mesmo endereço ao mesmo tempo,
duas máquinas receberiam o mesmo IP, e o resultado seria um conflito de
endereço. A mensagem Request diz exatamente qual endereço foi aceito, e o Ack
fecha a negociação garantindo que só um servidor forneça aquele endereço.

**O que você vê no laboratório**: ao clicar em **DHCP** na linha de PC5 ou
PC6, o cartão Escopo DHCP mostra as quatro linhas aparecendo uma a uma, com o
caminho percorrido na topologia. O endereço entregue é o primeiro livre do
escopo, por isso PC5 recebe o primeiro da faixa e PC6 o seguinte.

**Lease**: o endereço não é definitivo. O servidor empresta por um tempo
determinado, chamado de lease. Passado o prazo, o endereço volta para o pool,
caso o PC não peça para renovar. Em uma rede real, o lease costuma durar de 24
horas a uma semana.

**Por que o DHCP só atende a VLAN 30 neste laboratório?** O pool foi criado
dentro da sub-interface Gi0/0.30, e o DHCP só entrega endereço para quem está
na mesma VLAN. É uma limitação proposital do desafio: assim você pode
comparar o endereçamento automático, no Marketing, com o manual, no TI e no
RH, na mesma rede.

## 13. Glossário

Estes são os termos que você vai encontrar no enunciado, no console e nas
dicas. Todos eles também são clicáveis na Aula, e o glossário completo da
página tem 33 entradas.

### Rede

| Termo | O que é |
|:---|:---|
| VLAN | Rede lógica criada dentro de um switch físico. Cada VLAN é separada das outras. |
| Domínio de broadcast | Conjunto de equipamentos que escutam os mesmos broadcasts. A VLAN é a fronteira que define esse conjunto. |
| Broadcast | Pacote enviado para todos da rede. É o que o ARP usa, e por isso precisa ficar preso dentro da VLAN. |
| ARP | Protocolo que traduz endereço IP em endereço MAC. |
| Endereço MAC | Identificador físico da placa de rede, 12 dígitos hexadecimais. Ele não muda conforme a rede. |
| access | Porta que pertence a uma única VLAN, usada por PCs, impressoras e servidores. |
| trunk | Porta que transporta várias VLANs ao mesmo tempo, cada uma com sua etiqueta. |
| 802.1Q | Padrão IEEE que define a etiqueta de 4 bytes usada para identificar a VLAN de um quadro. |
| Etiqueta (tag) | Campo inserido no quadro Ethernet com o número da VLAN. |
| Switch | Equipamento de camada 2 que encaminha quadros pelo endereço MAC. Cria e separa VLANs, mas não roteia entre elas. |
| Switch núcleo | Switch central que concentra os troncos. Aqui é o SW4. |
| VLAN 1 | VLAN padrão, criada sozinha, usada por portas access sem VLAN configurada. Existe por compatibilidade. |
| Cabo de rede | Ligação física entre dois equipamentos. O tipo, de cobre ou fibra, não define a velocidade aqui: quem manda é o switch e o roteador. |

### Roteamento entre VLANs

| Termo | O que é |
|:---|:---|
| Roteador | Equipamento de camada 3 que decide o caminho entre redes diferentes, olhando o endereço IP. |
| Gateway | Endereço IP do roteador dentro da sua VLAN. É a porta de saída que o PC usa. |
| Sub-interface | Interface virtual criada em cima de uma interface física, cada uma atendendo uma VLAN. |
| encapsulation dot1Q | Comando que define qual VLAN uma sub-interface atende. |
| router-on-a-stick | Técnica de rotear entre VLANs com uma única interface física do roteador, fatiada em sub-interfaces. |
| Inter-VLAN Routing | Permitir que máquinas de VLANs diferentes conversem, o que só acontece com um roteador. |

### Endereçamento

| Termo | O que é |
|:---|:---|
| Endereço IP | Identificador lógico de cada equipamento na rede, como 192.168.30.10. |
| Máscara de sub-rede | Separa a parte rede da parte host do endereço. |
| Sub-rede /24 | Fatia com 254 posições, de .0 a .255. São as três redes deste laboratório. |
| DNS | Serviço que traduz nomes em endereços IP. |
| Interface | Porta física ou virtual de um equipamento, configurada com "interface gi0/0". |

### DHCP

| Termo | O que é |
|:---|:---|
| DHCP | Serviço que entrega IP, máscara, gateway e DNS automaticamente. |
| DORA | Nome das quatro mensagens do DHCP: Discover, Offer, Request e Ack. |
| Discover | Primeira mensagem. O PC sem IP pergunta se existe servidor DHCP. |
| Offer | O servidor oferece um endereço livre. |
| Request | O PC confirma que quer aquele endereço. |
| Ack | O servidor confirma e o PC passa a usar o endereço. |
| Pool (escopo) | Faixa de endereços que o servidor DHCP pode entregar. |
| Exclusão | Endereço retirado do pool para não ser entregue a ninguém. |
| Lease | Tempo pelo qual o endereço fica emprestado ao PC. |

### Comandos

| Termo | O que é |
|:---|:---|
| shutdown | Estado desligado de uma interface. No Cisco IOS, toda sub-interface nasce assim. |
| no shutdown | Comando que liga a interface. Sem ele o gateway não responde. |
| show | Família de comandos que mostra o estado e a configuração do equipamento. |
| running-config | Configuração que está valendo agora, na memória do equipamento. |
| startup-config | Configuração salva, que sobrevive a um reinício. |

---

Bom trabalho. Se travar, a aula, as dicas e o cartão de erros estão aí
justamente para isso.