# Avaliação Prática — Monitoramento de Geração Solar com Arduino
**Disciplina:** Soluções em Energias Renováveis e Sustentáveis  
**Curso:** Ciências da Computação — Turmas 1CC  
**Formato:** Individual, dupla ou trio  
**Modalidade:** Presencial — válido somente para alunos presentes
-----
## Circuito base no Tinkercad
****  

-----
## Descrição do circuito
O circuito disponibilizado simula uma instalação de geração solar de pequeno porte e conta com os seguintes componentes:
- **Arduino Uno**
- **Display LCD 16x2 com módulo I2C**
- **LED vermelho** — pino digital **2**
- **LED amarelo** — pino digital **4**
- **LED verde** — pino digital **6**
- **String Solar 1** — 3 módulos em série (36V por string) — divisor de tensão pré-calculado — pino **A2**
- **String Solar 2** — 3 módulos em série (36V por string) — divisor de tensão pré-calculado — pino **A3**
Os divisores de tensão já estão calculados e montados no circuito. Os pinos A2 e A3 recebem valores analógicos (0–1023) proporcionais à tensão de cada string.
-----

-----
## Situação-problema
A empresa **SolarGrid Soluções em Energia**, especializada em instalação e monitoramento de sistemas fotovoltaicos de pequeno e médio porte, firmou contrato com sua equipe de desenvolvimento para criar um **sistema embarcado de monitoramento e controle** para uma instalação piloto.
O sistema conta com duas strings solares independentes, cada uma composta por três módulos em série, totalizando tensão nominal de **36V por string**. A equipe de engenharia elétrica já dimensionou e montou os divisores de tensão para adequar os sinais ao Arduino. Sua responsabilidade é **desenvolver o firmware** que transforma esses dados brutos em informação útil para o operador da planta.
Cada exercício representa uma demanda real levantada pela equipe de engenharia. Resolva o exercício indicado pelo seu professor.
-----
## Exercícios
### Exercício 1 — Painel de leitura em tempo real
A equipe de campo precisa de uma forma rápida de conferir a tensão de cada string durante a operação, sem necessidade de equipamentos adicionais. Implemente um sistema que:
- Leia continuamente a tensão das duas strings solares (A2 e A3)
- Exiba no LCD, alternando a cada 2 segundos:
 - Linha 1: `S1: XX.X V`
 - Linha 2: `S2: XX.X V`
- Enquanto exibe a leitura da String 1, mantenha o LED amarelo aceso; enquanto exibe a da String 2, mantenha o LED verde aceso
- Exiba os valores também no Serial Monitor, formatados com unidade
-----
### Exercício 2 — Sistema de alarme de subtensão
A engenharia definiu que qualquer string abaixo de **20V** indica condição anormal — sombreamento, módulo com defeito ou falha de conexão. Implemente:
- Monitoramento contínuo das duas strings
- Se **ambas** estiverem acima de 20V: LED verde aceso, LCD exibe `Sistema OK`
- Se **qualquer uma** estiver entre 10V e 20V: LED amarelo aceso, LCD exibe `Atencao: S[1 ou 2]` e o valor da tensão
- Se **qualquer uma** cair abaixo de 10V: LED vermelho aceso, LCD exibe `FALHA: S[1 ou 2]` e o valor da tensão
- Apenas um LED deve ficar aceso por vez; prioridade: vermelho > amarelo > verde
-----
### Exercício 3 — Cálculo de potência estimada e eficiência relativa
A equipe de monitoramento precisa estimar a potência gerada por cada string. Considerando corrente nominal de **8A por string**, implemente:
- Cálculo de potência estimada para cada string: `P = V x I`
- Exibição no LCD em alternância de 2 segundos cada:
 - `P1: XXX W`
 - `P2: XXX W`
 - `Total: XXX W`
- Cálculo da eficiência relativa entre as strings: `Ef = (menor / maior) x 100%`
- Se eficiência relativa for **>= 90%**: LED verde
- Se entre **70% e 89%**: LED amarelo
- Se **< 70%**: LED vermelho — desbalanceamento crítico
- Exibir no Serial Monitor as tensões, potências e eficiência relativa a cada ciclo
-----
### Exercício 4 — Registro de estados no Serial Monitor
A equipe de engenharia precisa acompanhar a evolução do estado operacional de cada string para identificar padrões de falha. Implemente:
- Monitoramento contínuo com três faixas de operação para cada string:
 - **Normal:** V >= 25V
 - **Alerta:** 15V <= V < 25V
 - **Critico:** V < 15V
- O estado atual de cada string deve ser exibido no LCD simultaneamente:
 - Linha 1: `S1: NORMAL` ou `S1: ALERTA` ou `S1: CRITICO`
 - Linha 2: `S2: NORMAL` ou `S2: ALERTA` ou `S2: CRITICO`
- Sempre que qualquer string mudar de faixa, registrar no Serial Monitor a mudança e o valor de tensão que a provocou. Exemplo: `S1: Normal -> Alerta (22.4V)`
- Os LEDs refletem o estado mais grave entre as duas strings, usando o mesmo critério de prioridade do Exercício 2
-----
### Exercício 5 — Sistema de decisão para proteção da carga
A SolarGrid precisa de um módulo de decisão automática que simule o comportamento de um controlador de carga simples, protegendo os equipamentos conectados contra subtensão. Implemente:
- Leitura contínua das duas strings e cálculo da **tensão média**
- Lógica de decisão baseada na tensão média:
 - **>= 30V**: carga conectada — LED verde, LCD linha 1: `Carga: ON`, linha 2: `Vm: XX.X V`
 - **Entre 20V e 29.9V**: carga em standby — LED amarelo, LCD linha 1: `Standby`, linha 2: `Vm: XX.X V`
 - **< 20V**: carga desconectada — LED vermelho, LCD linha 1: `Carga: OFF`, linha 2: `Vm: XX.X V`
- Implementar **histerese de 2V**: uma vez em estado de corte (< 20V), o sistema só retorna ao standby quando a tensão média superar **22V**, evitando oscilações rápidas
- Registrar no Serial Monitor cada mudança de estado com o valor de tensão que provocou a transição
-----
## Entregável
Cada equipe deve entregar dois itens.
**1. Link do Tinkercad**
- Acesse o circuito base, clique em **Share** e copie o link
- O circuito deve estar funcional e com o código do exercício implementado
- Certifique-se de que o link está configurado como público ou acessível por link
**2. Arquivo `integrantes.txt`**
Crie um arquivo de texto simples com o seguinte formato:
```
Exercicio: [número]
Turma: 1CC[letra]
Integrante 1: Nome Completo - RM XXXXX
Integrante 2: Nome Completo - RM XXXXX
Integrante 3: Nome Completo - RM XXXXX
```
-----
## Observações
- Esta avaliação é válida somente para alunos presentes na aula
- O link do Tinkercad deve ser testado antes de ser entregue — links inválidos ou sem o código implementado não serão considerados
- Não é necessário criar um repositório GitHub — entregue apenas o link do Tinkercad e o arquivo `.txt`
- Dúvidas durante a atividade devem ser direcionadas ao professor presente
-----
*Disciplina de Soluções em Energias Renováveis e Sustentáveis — FIAP*
