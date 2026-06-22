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
NAT:used to share the HOST ip addresss meaing it will create a spreate network using the Host network adabter (if we have three virual machine each one will havr differnt networks and they will use host ip to access internet for example virual1 will have 192.168.0.1 virual2 will have 192.168.1.1 virual3 will have 192.168.3.1) 
Host-only: a priavt network shared with the host (each virual machine can comuncate with the host and others viruals machines that set to host-only ) the is good for our project
LAN Segment:A provate network shared with other standed VMs (they can talk to other virual machine but not to the Host and you configure each virual matchine give its ip address)this is the best option why 
