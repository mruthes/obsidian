
**Base Reference**  na tela sempre é o zPtr->brood_table[0].cool_target

O **Control Target** depende do tipo da saída (oPtr->flags - Bit0) , que são dois:

**Cool:**

**Control Target** = zPtr->brood_table[0].cool_target - zPtr->b_CoolOffsetDown + zPtr->b_CoolOffsetUp

**Heat:**

**Control Target** = zPtr->brood_table[0].heat_target - zPtr->b_HeatOffsetDown + zPtr->b_HeatOffsetUp

No código tem uma variável global **target[0]**:

target[0] = (unsigned short)(zone[z]->brood_table[0].cool_target + zone[z]->b_CoolOffsetUp - zone[z]->b_CoolOffsetDown);

A ***Target[0]*** é utilizada aparentemente para o controle dos fans (ver em *fanctrl.c* -> função ProcPidFans)


