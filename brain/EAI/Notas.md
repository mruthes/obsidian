Senha: OPAZ
I/O Definitions: IDEF
System Setting: CFGR
Zone - > galpão
Group -> pode ser um conjunto de sensores, exaustores, inlets


GROUP.C

// Base Reference
fValue = ScaleUnsignedToFloat (zPtr->brood_table[0].cool_target, devt);
SetCtrlVal (panel, GROUPCFGR_BASE_REF, fValue);

Base_ref é zPtr->brood_table[0].cool_target

// Control Target
if (MACRO_TEST_BIT (oPtr->flags, 0) == COOL)
	fValue = ScaleUnsignedToFloat (zPtr->brood_table[0].cool_target - zPtr->b_CoolOffsetDown + zPtr->b_CoolOffsetUp, devt);
else
	fValue = ScaleUnsignedToFloat (zPtr->brood_table[0].heat_target - zPtr->b_HeatOffsetDown + zPtr->b_HeatOffsetUp, devt);
SetCtrlVal (panel, GROUPCFGR_CONTROL_TGT, fValue);

O Target depende do oPtr->flags ??


o que é oPtr->flags??
if (MACRO_TEST_BIT (oPtr->flags, 0) == COOL && dataVal <= dataVal1)
	valid = FALSE;
if (MACRO_TEST_BIT (oPtr->flags, 0) == HEAT && dataVal >= dataVal1)
	valid = FALSE;


ScaleFloatToSigned  - converte de float para inteiro sinalizado

oPtr->on_temp = (unsigned short)dataVal; --> 850
oPtr->off_temp = (unsigned short)dataVal1; --> 750

o que é oPtr??

call back para GroupCfgrProcess quando altera qualquer valor.



Zone.c
função ZoneCnfgControl 

