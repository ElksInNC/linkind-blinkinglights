Rule1
ON System#Boot DO br load('blerry.be') ENDON
ON power1#boot do backlog var5 %value%; var6 %value%; var3 0; var9 %value%; event setlt ENDON
ON mqtt#connected DO Subscribe LED, stat/kitchen/automation/ledstate ENDON
ON event#LED do backlog Var5=(%value%)%2; Var6=(%value%-Var5)/2; var7=var3; ENDON
ON var7#state==0 do backlog var3 0; event setlt; event pubout; ENDON
ON var7#state!=0 do backlog var8=(var5+1)%2; var9=(var6+1)%2; ledpower1 %var8%; ledpower2 %var9%; delay 2; ledpower1 %var5%; ledpower2 %var6%; delay 2; var7=var7-1 ENDON

Rule2
ON button2#state=10 do backlog power1 1; var9=1; var3 0; var4=%value%+10 ENDON 
ON button1#state=10 do backlog power1 0; var9=0; var3 0; var4=%value% ENDON
ON event#setlt do backlog ledpower1 %var5%; ledpower2 %Var6% ENDON 
ON power1#state do backlog ledpower1 %value%; ledpower2 %value% ENDON

Rule3
ON button2#state!=10 do backlog var3=(%value%)%10; var4=%value%+10; var7=var3 ENDON
ON button1#state!=10 do backlog var3=(%value%)%10; var4=%value%; var7=var3 ENDON
ON var4#state do event pubout=%value% ENDON
ON event#pubout do publish stat/kitchen/automation/status {"State": %var9%, "Button": %value%, "Blink": %var3%, "Via":"LINKIND-07"} ENDON
