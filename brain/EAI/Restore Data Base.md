Arquivo egdat.job  
 - nome do galpao e tipo da comunicação
	 - 0 - memory shared
	 - 1-254 -comm
	 - 255 - tcp
 - 


restore();		/* get the old data base from the disk */

egdat.200 - arquivo binario, na leitura aponta para fPtr??

void get_data (void) é a função que le byte a byte o egdata.200


OutputTempVsFans ("tmpvsfan.dat");	// Write out database info for fans
o arquivo tmpvsfan.dat
// Outputs a list of temperatues (from 65 to 85 F) showing how many fans
// would be on if the temperature were increasing in a zone.  Decreasing
// temperatures would generate a different list since the "off_temp" field
// of the "output" data structure would be used to make the decision to turn
// a fan off.

