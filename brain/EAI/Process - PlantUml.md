@startuml
start

partition "process()" {
    repeat
        if (count == 2?) then (yes)
            :ProcessSystemEvents();
            :count++;
        elseif (count == 3?) then (yes)
            :GetWeatherData();
            :count++;
        elseif (count >= 4?) then (yes)
            :ProcessTimedEvents();
            :ModbusProcess();
            
            if (TOUCH defined?) then (yes)
                :SetCtrlVal(mainPanel, PANEL_DATA_LED, goodData);
                :addr.Type = TOUCH_ZONES/10000;
                :addr.Index = TOUCH_ZONES%10000;
                :ModbusRead(&numHouses, &addr, 0);
            endif
            
            if (Timer() - backupTimer > 3600?) then (yes)
                :BackupData();
                :backupTimer = Timer();
            endif
            
            if (pTime > 0?) then (yes)
                if (Timer() - passTimer > 60?) then (yes)
                    :pTime--;
                    :passTimer = Timer();
                endif
            endif
            
            if (pcPort == 255?) then (yes)
                if (g_connected[0] < 0?) then (yes)
                    :SetupTCPConnection(0, hostStr);
                endif
            endif
            
            :count = 0;
            
            if (TOUCH defined?) then (yes)
                :SetCtrlVal(mainPanel, PANEL_DATE_HEADER, DateStr());
                :SetCtrlVal(mainPanel, PANEL_TIME_HEADER, TimeStr());
            endif
        else (no)
            :count++;
            :ConsoleInteraction();
        endif
    repeat while (continue?)
    stop
}

@enduml