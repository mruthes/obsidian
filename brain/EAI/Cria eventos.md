// Create an event
									switch (bData[3]) {
										case 0:	// Read
											switch (bData[5]) {
												case 0:	// DO
													srcReg = kMsgDO;
													break;
												case 1:	// DI
													srcReg = kMsgDI;
													break;
												case 3:	// AI
													srcReg = kMsgAI;
													break;
												case 4:	// AO
													srcReg = kMsgAO;
													break;
											}
											switch (bData[7]) {
												case 0:	// DO
													if (NE_EventCyclicCreate (nsPtr, (unsigned short)bData[2], kEventActionRead, (uint16_t)bData[4],
														srcReg, (tRegIdx)bData[6],
														DO_BANK, (tRegIdx)bData[8],
														3) != NULL)
														printf ("DO read:%d,%d\n", bData[6], bData[8]);
													else
														printf ("Error creating DO read:%d,%d\n", bData[6], bData[8]);
													break;
												case 1:	// DI
													if (NE_EventCyclicCreate (nsPtr, (unsigned short)bData[2], kEventActionRead, (uint16_t)bData[4],
														srcReg, (tRegIdx)bData[6],
														DI_BANK, (tRegIdx)bData[8],
														4) != NULL)
														printf ("DI read:%d,%d\n", bData[6], bData[8]);
													else
														printf ("Error creating DI read:%d,%d\n", bData[6], bData[8]);
													break;
												case 3:	// AI
													if (NE_EventCyclicCreate (nsPtr, (tNetAddr)bData[2], kEventActionRead, (uint16_t)bData[4],
														srcReg, (tRegIdx)bData[6],
														AI_BANK, (tRegIdx)bData[8],
														5) != NULL)
														printf ("AI read:%d,%d\n", bData[6], bData[8]);
													else
														printf ("Error creating AI read:%d,%d\n", bData[6], bData[8]);
													break;
												case 4:	// AO
													switch (bData[2]) {
														case 112:
														case 113:
														case 114:
														case 115:
														case 116:
														case 117:
														case 118:
														case 119:
														case 120:
														case 121:
														case 122:
														case 123:
														case 124:
														case 125:
														case 126:
														case 127:
														case 144:	// 2nd bank of bird scales
														case 145:
														case 146:
														case 147:
														case 148:
														case 149:
														case 150:
														case 151:
														case 152:
														case 153:
														case 154:
														case 155:
														case 156:
														case 157:
														case 158:
														case 159:
														case 160:	// 3rd bank of bird scales
														case 161:
														case 162:
														case 163:
														case 164:
														case 165:
														case 166:
														case 167:
														case 168:
														case 169:
														case 170:
														case 171:
														case 172:
														case 173:
														case 174:
														case 175:
															// Read bird scale every 10 minutes (bData[9])
															ne = NE_EventTimerCreate (nsPtr, (tNetAddr)bData[2], kEventActionRead, (uint16_t)bData[4],
																srcReg, (tRegIdx)bData[6],
																AO_BANK, (tRegIdx)bData[8],
																(uint16_t)bData[9]);
															break;
														default:
															ne = NE_EventCyclicCreate (nsPtr, (tNetAddr)bData[2], kEventActionRead, (uint16_t)bData[4],
																srcReg, (tRegIdx)bData[6],
																AO_BANK, (tRegIdx)bData[8],
																17);
															break;
													}
													if (ne != NULL) {
														NE_Trigger (ne);	// Force an immediate read
														printf ("AO read:%d,%d\n", bData[6], bData[8]);
													}
													else
														printf ("Error creating AO read:%d,%d\n", bData[6], bData[8]);
													break;
												default:
													printf ("Nothing:%d,%d,%d,%d\n", bData[5], bData[6], bData[7], bData[8]);
													break;
											}
											break;




typedef enum
{
	kMsgDI = 30000,
	kMsgDO = 30001,
	kMsgAI = 30002,
	kMsgAO = 30003,

	// these are used for range checking
	kMsgFirst = kMsgDI,
	kMsgLast  = kMsgAO,
} tMsgType;