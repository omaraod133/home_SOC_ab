# home_SOC_ab

step by step
i will create home lab for SOC analsy

i face mluitble assue one of them are the hardwear limition(12GB of RAM) and the other is windwos problem of concuming a lot of RAM minimum 4GB RAM without you do do anying + splunk need about 4GB this is about 8+GB

so i descied to use link as the main host opreating and i use EndeavourOS distribution which is light and conusming about 1GB Of RAM

so our lab will be consist of 3 vrusal miching

<img width="1611" height="837" alt="Screenshot From 2026-06-22 06-35-31" src="https://github.com/user-attachments/assets/01a2ae24-85a0-4394-a429-1d078625ddaf" />

attacker victem analyst 

attacker will attack the victem machin 
<video src="https://github.com/user-attachments/assets/4435e20a-703e-44a5-9ba2-e4db5f2e1602" width="100%" controls preload="none"></video>


victem machine will send logs to analyst SIME 

<video src="https://github.com/user-attachments/assets/a63ee215-3918-4870-bf3b-a83177d8ac62" width="100%" controls preload="none"></video>

so let start confiuger each machine 
<img width="1770" height="1030" alt="Screenshot_٢٠٢٦٠٦٢٢_٠٧٤٥١٤" src="https://github.com/user-attachments/assets/426cc2cc-da40-4b3b-bcd3-29b558a6345f" />

i setup each machine in the Vmwear and now i will configure it so it can comuncate each vitual machine without haveing connected to the HOST (to reduce risk)

lets start with the analyst vriual machine

<img width="1920" height="1035" alt="Screenshot_٢٠٢٦٠٦٢٢_١٤٣٣٥٥" src="https://github.com/user-attachments/assets/8d50177b-57a8-4103-b10e-b468dc6b8392" />


here we see many options
Bridged:connected directly to the physical netwok ( vruail machines will be in the same network like they are new device connect to the router, virual1 will be 192.168.0.1 virual2=192.168.0.2 virual3=192.168.0.3) (since they are at the same network dont excte malware in this option)
NAT:used to share the HOST ip addresss meaing it will create a spreate network using the Host network adabter (if we have three virual machine each one will havr differnt network then the host and they will use host ip to access internet for example host=10.10.10.1 virual1 will have 192.168.0.1 virual2 will have 192.168.0.2 virual3 will have 192.168.0.3) 
Host-only: a priavt network shared with the host (each virual machine can comuncate with the host and others viruals machines that set to host-only ) the is good for our project
LAN Segment:A provate network shared with other standed VMs (they can talk to other virual machine but not to the Host and you configure each virual matchine give its ip address)this is the best option why becase it give you full controal over the network and it is isolated you have no access to HOST so you can test without get warryed


now let is create our new  virtual network and we will name it LAN10

<img width="1920" height="1035" alt="Screenshot_٢٠٢٦٠٦٢٢_١٥١٣٣٥" src="https://github.com/user-attachments/assets/8b6206d8-f242-4755-9cc6-b699edd580a3" />
t
Click Save and we Done

now we will configure each virtual matchine and make them set our LAN 10 virtual network

<img width="1920" height="1035" alt="Screenshot_٢٠٢٦٠٦٢٢_١٥١٨٥٣" src="https://github.com/user-attachments/assets/4aa347b7-8ca9-45de-999e-3c6dcdce0b7a" />
click done to set the victim virual machine up 

and we will do the same with the attacker virual machine


now let start configure and give each machine his ip address and we will satrt with analsy machine 

<img width="1897" height="885" alt="Screenshot From 2026-06-23 06-32-29" src="https://github.com/user-attachments/assets/4244398c-7ba5-4134-9a48-f18c40011b66" />

then we will configure victm 

<img width="1439" height="895" alt="Annotation 2026-06-22 201352" src="https://github.com/user-attachments/assets/74ebd322-fc3a-4c2a-86f7-51adc5bebe2f" />

let test there connction and see is there any connction betwean them, so will use ping and if we do everythings right we will see reply from analst macthine

<img width="1421" height="877" alt="Annotation 2026-06-22 205406" src="https://github.com/user-attachments/assets/f802462d-a07f-4e54-ac7c-11b205a12a8d" />

greate we aew in the right path, now let confiuer the attacker(kali) machone 

<img width="1906" height="903" alt="Screenshot_2026-06-23_00_34_03" src="https://github.com/user-attachments/assets/f4297c0f-e524-43b9-b9c8-aad1a1eba67f" />

if we do it right we show see reply in our ping test

<img width="1440" height="860" alt="Annotation 2026-06-22 214656" src="https://github.com/user-attachments/assets/a0e1c519-ee3f-426e-b0b0-434e92ba3368" />

as we see we configure all the machine very well 

next we need to install symon and make windows redirct its logs to splunk in analyst machine

let start with ownloading sysmon and configur it
