# Avaliação Prática — Monitoramento de Geração Solar com Arduino

**Disciplina:** Soluções em Energias Renováveis e Sustentáveis  
**Curso:** Ciências da Computação — Turmas 1CC  
**Formato:** Individual, dupla ou trio  
**Modalidade:** Presencial — válido somente para alunos presentes

---

## Circuito base no Tinkercad

**[Acesse o circuito aqui → LINK_DO_TINKERCAD](https://www.tinkercad.com/things/28QEgg4Pb1I-checkpoint-02-sers-1cc-2026-1sem?sharecode=Bxc4rRv9Vy0ou-7XMgv0fatFfWdxCSP0TECy5HelR5A)**  

---

## Configurações Básicas do Arduino IDE

- Abra o Arduino IDE. Clique no menu File ---> Preferences:
- **Tamanho da Fonte**: escolha um tamanho que facilite a leitura em sala (ex: 14 ou 16)
- Escolha o tema de sua preferência.
- Habilite a opção "Editor Quicks Suggestions".

## Descrição do circuito

O circuito disponibilizado simula uma instalação de geração solar de pequeno porte. Antes de escrever qualquer linha de código, **analise o diagrama no Tinkercad** e identifique a ligação de cada componente ao Arduino. O mapeamento dos pinos faz parte da avaliação.

O circuito conta com:

- **Arduino Uno**
- **LED vermelho**
- **LED amarelo**
- **LED verde**
- **String Solar 1** — 3 módulos em série (36V por string) com divisor de tensão pré-calculado
- **String Solar 2** — 3 módulos em série (36V por string) com divisor de tensão pré-calculado

Os divisores de tensão já estão calculados e montados no circuito. As entradas analógicas recebem valores (0–1023) proporcionais à tensão de cada string.

---

## Código base

Utilize a estrutura abaixo. Cada exercício deve ser implementado em sua própria função. No `loop()`, comente todas as chamadas exceto a do exercício que está testando no momento.

```cpp
// Mapeie os pinos a partir da analise do diagrama no Tinkercad
// Declare aqui suas constantes de pinos e quaisquer variaveis globais

void setup() {
  Serial.begin(9600);

  // Configure os modos dos pinos identificados

  // Adicione qualquer outra configuracao que seu exercicio precisar
}

void loop() {
  // ex1();
  // ex2();
  // ex3();
  // ex4();
  // ex5();
}

void ex1() {
  // Exercicio 1
}

void ex2() {
  // Exercicio 2
}

void ex3() {
  // Exercicio 3
}

void ex4() {
  // Exercicio 4
}

void ex5() {
  // Exercicio 5
}
```

> Descomente apenas a chamada do exercício que está sendo testado. Deixe todas as implementações no arquivo — o professor irá descomentar cada função durante a correção.

---

## Situação-problema

A empresa **SolarGrid Soluções em Energia**, especializada em instalação e monitoramento de sistemas fotovoltaicos de pequeno e médio porte, firmou contrato com sua equipe de desenvolvimento para criar um **sistema embarcado de monitoramento e controle** para uma instalação piloto.

O sistema conta com duas strings solares independentes, cada uma composta por três módulos em série, totalizando tensão nominal de **36V por string**. A equipe de engenharia elétrica já dimensionou e montou os divisores de tensão para adequar os sinais ao Arduino. Sua responsabilidade é **analisar o diagrama, mapear o circuito e desenvolver o firmware** que transforma esses dados brutos em informação útil para o operador da planta.

Todas as saídas devem ser exibidas pelo **Serial Monitor**. Os LEDs sinalizam o estado do sistema de forma visual.

---

## Exercícios

### Exercício 1 — Painel de leitura em tempo real

A equipe de campo precisa monitorar continuamente a tensão de cada string durante a operação. Implemente a função `ex1()` com:

- Leitura da tensão das duas strings a cada segundo
- Exibição no Serial Monitor a cada ciclo:
  - `S1: XX.X V`
  - `S2: XX.X V`
- Enquanto a tensão da String 1 for maior que a da String 2, mantenha o LED amarelo aceso; caso contrário, mantenha o LED verde aceso
- Se as tensões forem iguais, acenda ambos

---

### Exercício 2 — Sistema de alarme de subtensão

A engenharia definiu que qualquer string abaixo de **20V** indica condição anormal — sombreamento, módulo com defeito ou falha de conexão. Implemente a função `ex2()` com:

- Monitoramento contínuo das duas strings
- Se **ambas** estiverem acima de 20V: LED verde aceso, Serial Monitor exibe `Sistema OK`
- Se **qualquer uma** estiver entre 10V e 20V: LED amarelo aceso, Serial Monitor exibe `Atencao: S[1 ou 2] = XX.X V`
- Se **qualquer uma** cair abaixo de 10V: LED vermelho aceso, Serial Monitor exibe `FALHA: S[1 ou 2] = XX.X V`
- Apenas um LED deve ficar aceso por vez; prioridade: vermelho > amarelo > verde

---

### Exercício 3 — Cálculo de potência estimada e eficiência relativa

A equipe de monitoramento precisa estimar a potência gerada por cada string. Considerando corrente nominal de **8A por string**, implemente a função `ex3()` com:

- Cálculo de potência estimada para cada string: `P = V x I`
- Exibição no Serial Monitor a cada ciclo:
  - `P1: XXX W`
  - `P2: XXX W`
  - `Total: XXX W`
- Cálculo da eficiência relativa entre as strings: `Ef = (menor / maior) x 100%`
  - `Eficiencia: XX.X %`
- Se eficiência relativa for **>= 90%**: LED verde
- Se entre **70% e 89%**: LED amarelo
- Se **< 70%**: LED vermelho — desbalanceamento crítico

---

### Exercício 4 — Registro de mudanças de estado

A equipe de engenharia precisa acompanhar a evolução do estado operacional de cada string para identificar padrões de falha. Implemente a função `ex4()` com:

- Monitoramento contínuo com três faixas de operação para cada string:
  - **Normal:** V >= 25V
  - **Alerta:** 15V <= V < 25V
  - **Critico:** V < 15V
- A cada ciclo, exibir no Serial Monitor o estado atual das duas strings:
  - `S1: NORMAL | S2: ALERTA`
- Sempre que qualquer string mudar de faixa, registrar a transição separadamente. Exemplo: `S1: Normal -> Alerta (22.4V)`
- Os LEDs refletem o estado mais grave entre as duas strings, usando o mesmo critério de prioridade do Exercício 2

---

### Exercício 5 — Sistema de decisão para proteção da carga

A SolarGrid precisa de um módulo de decisão automática que simule o comportamento de um controlador de carga simples, protegendo os equipamentos conectados contra subtensão. Implemente a função `ex5()` com:

- Leitura contínua das duas strings e cálculo da **tensão média**
- Lógica de decisão baseada na tensão média:
  - **>= 30V**: carga conectada — LED verde, Serial Monitor exibe `Carga: ON | Vm: XX.X V`
  - **Entre 20V e 29.9V**: carga em standby — LED amarelo, Serial Monitor exibe `Standby | Vm: XX.X V`
  - **< 20V**: carga desconectada — LED vermelho, Serial Monitor exibe `Carga: OFF | Vm: XX.X V`
- Implementar **histerese de 2V**: uma vez em estado de corte (< 20V), o sistema só retorna ao standby quando a tensão média superar **22V**, evitando oscilações rápidas
- Sempre que o estado mudar, registrar no Serial Monitor a transição e o valor de tensão que a provocou

---

## Entregável

Cada equipe deve entregar dois itens.

**1. Link do Tinkercad**

Antes de copiar o link, é necessário tornar o projeto acessível. Siga os passos abaixo:

1. Conclua o circuito e volte para a tela inicial do Tinkercad
2. Clique no seu projeto e, em seguida, em **Alterar visibilidade**
3. No campo **Privacidade**, selecione a opção **Compartilhar link** — visível para qualquer pessoa com o link
4. Volte à tela anterior e clique em **Copiar link**
5. Cole o link como anexo da tarefa no Teams

> O circuito deve estar funcional e com o código de todos os exercícios implementados. Descomente apenas a chamada do exercício do seu grupo antes de entregar.

**2. Arquivo `integrantes.txt`**

Crie um arquivo de texto simples com o seguinte formato:

```
Atividade: Checkpoint 02 - SERS
Turma: 1CC[letra]

Integrante 1: Nome Completo - RM XXXXX
Integrante 2: Nome Completo - RM XXXXX
Integrante 3: Nome Completo - RM XXXXX
```

---

## Observações

- Esta avaliação é válida somente para alunos presentes na aula
- O link do Tinkercad deve ser testado antes de ser entregue — links inválidos ou sem o código implementado não serão considerados
- Não é necessário criar um repositório GitHub — entregue apenas o link do Tinkercad e o arquivo `.txt`
- Dúvidas durante a atividade devem ser direcionadas ao professor presente

---

*Disciplina de Soluções em Energias Renováveis e Sustentáveis — FIAP*
