
---

### **1. Especificações das placas DGP**

> _“DGP boards have 8 analog inputs, 8 counters, 8 relay outputs and optional analog outputs or RS232 channel.”_

**Tradução e explicação:**  
As placas DGP têm:

- 8 entradas analógicas,
    
- 8 contadores,
    
- 8 saídas a relé,
    
- E podem opcionalmente ter saídas analógicas ou uma porta RS232 (comunicação serial).
    

> _“The new hardware can have many different combinations, but not any configuration similar to a DGP.”_

Ou seja, **o novo hardware é mais flexível**, mas **não consegue replicar exatamente o que uma DGP faz**.

---

### **2. Configuração do "brick"**

> _“The brick supports 4 cards that can be 16 analog inputs, 16 discrete inputs/outputs, or a Combo card with 4 analog inputs, 2 analog outputs, 4 discrete inputs and 4 relays.”_

**Tradução e explicação:**  
O dispositivo chamado "brick" pode ter até 4 placas, sendo que cada uma pode ser:

- 16 entradas analógicas,
    
- 16 entradas/saídas digitais (discretas),
    
- ou uma placa **Combo** com:
    
    - 4 entradas analógicas,
        
    - 2 saídas analógicas,
        
    - 4 entradas digitais,
        
    - 4 relés.
        

---

### **3. Especificações do controlador EtherLogic Ultima**

> _“The EtherLogic Ultima controller has a fixed number of I/O with 8 analog inputs, 4 analog outputs, 12 relays, and up to 20 discrete inputs.”_

Este controlador tem uma configuração fixa de:

- 8 entradas analógicas,
    
- 4 saídas analógicas,
    
- 12 relés,
    
- Até 20 entradas digitais.
    

---

### **4. Conectividade adicional via Modbus**

> _“Any other I/O that can communicate via Modbus can also be connected, such as the Intra+ card, Micro/Pico Bricks or a PLC supporting Modbus.”_

**Ou seja:** outros dispositivos com suporte ao protocolo **Modbus** também podem ser conectados, como:

- A placa Intra+,
    
- Micro ou Pico Bricks,
    
- Um CLP (PLC) que suporte Modbus.
    

---

### **5. Arquivo de configuração**

> _“A configuration file is installed on the controller that the program reads on startup to arrange the channels into the DGP channels desired and scales the signals to match the DGP range.”_

Há um **arquivo de configuração** instalado no controlador. Na inicialização:

- O programa lê esse arquivo,
    
- Organiza os canais para que correspondam aos canais esperados de uma placa DGP,
    
- Faz o **ajuste de escala (scaling)** dos sinais para combinar com os valores esperados por um DGP.
    

---

### **6. Comunicação com o programa Integra**

> _“The controller will send information to the Integra program only for those DGP boards that it has assignments for.”_

O controlador **só envia dados para o programa Integra** se ele tiver uma atribuição (mapeamento) para aquela placa DGP.

> _“Only one channel needs to be assigned for the program to respond to all requests for that DGP board. It will respond with a value of zero for undefined channels.”_

Se pelo menos **um canal da placa DGP estiver atribuído**, o Integra responderá a todas as requisições dessa placa. Para canais não definidos, responderá com **zero**.

---

### **7. Troca de arquivos de configuração por endereço**

> _“The EtherLogic controller has an address switch that can be used to select from multiple configuration files.”_

O controlador tem uma **chave de endereço (address switch)** que permite selecionar entre diferentes arquivos de configuração.

> _“Thus, each controller can have the files for all the houses where changing of the address switch will select the house it is installed in.”_

Assim, **um mesmo controlador pode ser usado em várias casas**: ele armazena os arquivos de todas elas e, ao mudar a chave de endereço, ele passa a usar o arquivo correspondente à casa onde foi instalado.

---

### **Resumo geral:**

O sistema envolve diferentes tipos de controladores e placas de I/O. O novo hardware é mais flexível, mas não imita exatamente as placas DGP. Para que o programa Integra funcione corretamente, é necessário um **arquivo de configuração** que faz o mapeamento dos canais físicos para os canais esperados da DGP. Há ainda **compatibilidade com dispositivos Modbus** e uma chave física no controlador permite alternar entre configurações para diferentes instalações (casas).


Detalhes do arquivo:



[Ports]

!Port,Protocol,SlaveAddr,Action,BlockSize,mType,mStart,cType,cStart
!5,          1,            1,            0,        96,           1,         1,       1,        65   -> port 5 modbus address 1 with 96 digital inputs  mapped to address 65 (Intra+ card)
!5,          1,            1,            1,        16,           0,         1,       0,        65   -> port 5 modbus address 1 with 16 outputs starting after the DIO cards at address 65 (Intra+ card)
!5,           1,           1,            0,       112,          3,         1,       3,        65   -> port 5 modbus address 1 with 112 analog inputs mapped to address 65 (Intra+ card)
5,            1,           1,            0,         96,          4,         1,       4,        65  -> port 5 modbus address 1 with 96 holding registers (counts) mapped to address 65 (Intra+ card)
5,            1,           2,            1,           8,          4,         1,       4,     162   -> port 5 modbus address 2  with 8 holding registers (counts)  starting address **40162** (PicoBrick)

5,           1,            3,            0,         84,          1,       2049,    1,    162   -> port 5 modbus address 3 with

5,1,3,1,80,0,2049,0,81

5,1,3,0,16,4,833,3,178

5,1,3,0,84,4,641,4,170

5,1,3,0,84,4,737,3,194
1,           0,            1
4,           0,            2

!Port= RS232 ports on the controller.
Protocol: 0 = DGP, 1 = Modbus Master, 2 - Modbus Slave 
Action:  0=read and 1=write (modbus master)
BlockSize = number of values
mType: for module??
cType: 0=DI (digital input??) 1= DO (digital output??) 3= AI (analog input) e 4 = AO (analog output)
mStart: starting location for the values
cStart: starting location for the values







!DGP Addr,   DGP Ch, Offset,  Mult,   Div,    AI Addr, DO Addr, AO Addr,    Count Addr, Rate Addr

1,                      1,       1900,     10,       42,      30033,     00049,      40162,        40081,        30089

1,                      2,       1900,     10,       42,      30034,     00050,      00000,        40082,        30090

1,                      3,       1900,     10,       42,      30035,     00051,      00000,        40083,        30091

1,                      4,       1900,     10,      42,       30036,     00052,      00000,        40084,       30092

1,                      5,       1900,     10,      42,       30037,     00053,      00000,        40085,        30093

1,                      6,       1900,     10,      42,       30038,     00054,      00000,        40086,        30094

1,                      7,       1900,     10,      42,       30039,     00055,      00000,        40087,        30095

1,                      8,       1900,     10,      42,       30040,     00056,      00000,        40088,        30096



### **Visão Geral**

- **Seção:** `[Ports]`
    
- Essa seção mapeia **portas seriais RS232** no controlador.
    
- A coluna `Protocol` define o tipo de comunicação:
    
    - `0` → **DGP para PC** (protocolo proprietário)
        
    - `1` → **Modbus Master**
        
    - `2` → **Modbus Slave**
        
- Os demais parâmetros (como `Action`, `BlockSize`, `mType`, etc.) são relevantes apenas para o **Modbus Master (1)**.
    

---

### 📋 **Formato da linha**

Cada linha (exceto a primeira com `!Port`) segue este formato:

plaintext

CopiarEditar

`Port,Protocol,SlaveAddr,Action,BlockSize,mType,mStart,cType,cStart`

- `Port`: número da porta RS232
    
- `Protocol`: tipo de protocolo (0 = DGP, 1 = Modbus Master, 2 = Modbus Slave)
    
- `SlaveAddr`: endereço do dispositivo escravo Modbus (ou baud rate para DGP)
    
- `Action`: 0 = leitura, 1 = escrita
    
- `BlockSize`: quantidade de registros
    
- `mType`: tipo de dado no módulo (0=DI, 1=DO, 3+=AI, 4=AO)
    
- `mStart`: endereço inicial no módulo
    
- `cType`: tipo de dado no controlador
    
- `cStart`: endereço inicial no controlador
    

---

### 🔍 **Análise das Linhas**

#### **Porta 3**

plaintext

CopiarEditar

`3,0,19200`

- Porta 3 configurada como **DGP** (protocolo 0) com baud rate **19200**
    
- Ligada ao **PC**, usada para comunicação direta.
    

---

#### **Porta 5 – Modbus Master para 3 dispositivos**

##### 📌 Escravo 1 (Provavelmente um Intra+ card)

plaintext

CopiarEditar

`5,1,1,0,96,1,1,1,65      # 96 entradas digitais 5,1,1,1,16,0,1,0,65      # 16 saídas digitais 5,1,1,0,112,3,1.3,65     # 112 entradas analógicas 5,1,1,0,96,4,1,4,65      # 96 registradores de contagem (holding registers)`

- Tudo começa no endereço 65 para deixar espaço para até 64 canais.
    
- `mType` 1 e `cType` 1 → entradas digitais (DI)
    
- `mType` 0 e `cType` 0 → saídas digitais (DO)
    
- `mType` 3 e `cType` 3 → entradas analógicas (AI)
    
- `mType` 4 e `cType` 4 → holding registers (contadores)
    

---

##### 📌 Escravo 2 (provavelmente um AO PicoBrick)

plaintext

CopiarEditar

`5,1,2,1,8,4,1,4,162`

- Porta 5 → comunica com escravo 2 via Modbus
    
- Escreve (1) 8 valores analógicos de saída (AO)
    
- Começa no endereço 162 para **não conflitar com registros anteriores**
    

---

##### 📌 Escravo 3 (um PLC DL-06)

plaintext

CopiarEditar

`5,1,3,0,84,1,2049,1,162     # 84 DI (entradas digitais) 5,1,3,1,80,0,2049,0,81      # 80 DO (saídas digitais) 5,1,3,0,16,4,833,3,178      # 16 AO lidos como AI (entradas analógicas) 5,1,3,0,84,4,641,4,170      # 84 contadores 5,1,3,0,84,4,737,3,194      # 84 taxas de contadores lidos como AI`

- **Os endereços começam nos registros internos do PLC:**
    
    - Entradas e saídas digitais: `2049`
        
    - AO: `833`, `641`, `737`
        
- Os valores são remapeados no controlador a partir de:
    
    - DI → 162
        
    - DO → 81
        
    - AO → 178 (AI), 170 (contador), 194 (taxa)
        
- Obs: o PLC tem **limitação física de slots**, mas o programa interno permite representar isso logicamente, com apenas 1 AI card obrigatório na 1ª posição.
    

---

#### **Porta 1**

plaintext

CopiarEditar

`1,0,1`

- Porta 1 configurada como DGP para **DGP 001**
    

---

#### **Porta 4**

plaintext

CopiarEditar

`4,0,2`

- Porta 4 configurada como DGP para **DGP 002**
    

---

### ✅ **Resumo Final**

| Porta | Tipo          | Dispositivo               | Observação                    |
| ----- | ------------- | ------------------------- | ----------------------------- |
| 1     | DGP           | DGP 001                   | Saída serial                  |
| 3     | DGP           | PC                        | Comunicação PC                |
| 4     | DGP           | DGP 002                   | Saída serial                  |
| 5     | Modbus Master | Vários escravos (1, 2, 3) | Integração com módulos e PLCs |