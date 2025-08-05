NMBR_DGP			253

no InitSystem
le um arquivo NVdata.bin
carrega em uma estrutura nvStorage.

NVStorage:
![[Pasted image 20250805100058.png]]

dgpPtr
![[Pasted image 20250805100404.png]]



A velocidade para o modbus master  é de 38400


No process que roda o codigo, segue a sequencia da leitura do modbus
![[Pasted image 20250805161230.png]]



No modbusRTU temos no maximo 2 sessoes

![[Pasted image 20250805170315.png]]

na estrutura "ne" tem as informações da lista dos slaves, endereços para leitura e escrita, estado de maquina de cada slave
![[Pasted image 20250805170751.png]]

Source: [[code]]
Related:
Tags: #IntraE 