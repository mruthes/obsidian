

A base é a referencial de temperatura na tabela, é a entrada 0 da brood_table, a partir dela que são calculados os targets. Para calcular o target, são aplicados offsets, estes offsets são calculados conforme o valor desejado da idade subtraindo da base.

O target (Control Target) é tipo uma desejada, é calculada:

GROUPCFGR_CONTROL_TGT=  zPtr->brood_table[0].cool_target - zPtr->b_CoolOffsetDown + zPtr->b_CoolOffsetUp

No caso dos outputs on e off, o valor armazenado é sempre o valor configurado pelo usuario, durante a mudanças de desejadas é aplicado o valor do offset para compensar o liga e desliga, portanto não é alterado.

**Targets for primary on and off**

OUTCONTROL_TGT_PRIMARY_ON =  oPtr->on_temp + gPtr->currentOffsetCool - zPtr->b_CoolOffsetDown + zPtr->b_CoolOffsetUp

OUTCONTROL_TGT_PRIMARY_OFF= oPtr->off_temp + gPtr->currentOffsetCool - zPtr->b_CoolOffsetDown + zPtr->b_CoolOffsetUp
  

**Offsets primary onf and off**

GROUPCFGR_OUTPUT_PRIMARY_ON =   oPtr->on_temp - zPtr->brood_table[0].cool_target;

GROUPCFGR_OUTPUT_PRIMARY_OFF =  oPtr->off_temp - zPtr->brood_table[0].cool_target;