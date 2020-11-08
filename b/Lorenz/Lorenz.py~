#This code is licensed under the creative commons attribution share-alike license 4.0.
#CC-BY-SA-4.0
baudot = ["?","T","<","O"," ","H","N","M",">","L","R","G","I","P","C","V","E","Z","D","B","S","Y","F","X","A","W","J","^","U","Q","K","!"]
wheelPop = [43,47,51,53,59,37,61,41,31,29,26,23]#The number of pins on each wheel, coprime within each set of wheels (PPPPPMMXXXXX).
#this 126 didgit hex value holds the config of the machine
key = 0xcda8564cd29e4bc7d72ac08c43cfa0852281369b91c2d586a95c95283567f64225436788ffb87bc992addc4276223daac55c5c3ede3cf3052617b4a7a898ea
pinCount = sum(wheelPop)
maxKey = 2**pinCount
import time

global wheels
global wheelPos; wheelPos = [0]*len(wheelPop)
def encodeString(string):
    plainText = list(string.upper())
    cypherText = ""
    for x in plainText:
        #print("Letter", x)
        cypherText = cypherText + encodeLetter(x)
    #print(string)
    return "".join(cypherText)

def encodeLetter(letter):
    # generate cypher-tool
    psi = []
    for p in range(-1,4):
        psi = psi + [wheels[p][wheelPos[p]-1]]
    chi = []
    for c in range(6,11):
        chi = chi + [wheels[c][wheelPos[c]-1]]
    out = [False]*5
    l = fromBaudot(letter)
    #print(l)
    for t in range(0,5):
        #print(t)
        out[t] = chi[t] ^ psi[t] ^ l[t] #xor the chi, psi and plaintext bits
    #printWheels([tool])
    tickwheels()# increment wheels by one
    return toBaudot(out)

#print(wheels)
#as pincount = 501, 125.25 didgits of hex are needed, as such 126 will be used and the remainder ignored
def getWheels(key):
    wheels = []
    for p in wheelPop:
        wheels = wheels + [True]*p
    keyList = list(bin(key))[2:] #list of binary didgits of key
    p = 0
    for p in range(0,len(wheels)):
        if keyList[p] == "1":
            wheels[p] = True
        else:
            wheels[p] = False
    #return wheels
    #generate cumulative wheelPop:
    p = 0
    cWheelPop = []
    for p in range(0,len(wheelPop)):
        cWheelPop = cWheelPop + [sum(wheelPop[:p])]
    cWheelPop = cWheelPop + [sum(wheelPop)] #adds final value
    p = 0
    output = []
    for p in range(0,len(cWheelPop)-1):
        output = output + [wheels[cWheelPop[p]:cWheelPop[p+1]]]
    return output

def tickwheels(): # if the program is modified to a different bit-length this will not work at all
    global wheelPos
    for wheel in [6,7,8,9,10,11]: # tick the Chi Wheels and muA wheel
        if wheelPos[wheel] == wheelPop[wheel]:
            wheelPos[wheel] = 0
        else:
            wheelPos[wheel] = wheelPos[wheel] + 1 # tick the muB wheel
    if wheels[6][wheelPos[6]-1] == True: 
        if wheelPos[5] == wheelPop[5]:
            wheelPos[5] = 0
        else:
            wheelPos[5] = wheelPos[5] + 1
    if wheels[5][wheelPos[5]-1] == True: # tick the Phi wheels
        for wheel in [0,1,2,3,4]:
            if wheelPos[wheel] == wheelPop[wheel]:
                wheelPos[wheel] = 0
            else:
                wheelPos[wheel] = wheelPos[wheel] + 1
#    print(wheelPos)

def printWheels(wheels):
    print("Current wheel settings generated from key:\n1 represents a raised pin and 0 represents a lowered pin.\nThe wheels are shown with the first coloumn representing the position [00000,00,00000]")
    pos = 0
    names = ["P1[","P2[","P3[","P4[","P5[","MA[","MB[","C1[","C2[",'C3[','C4[','C5[']
    for x in wheels:
        a = []
        for p in x:
            a = a + [str(int(p))]
        print(names[pos],"".join(a),']')
        pos = pos + 1
    input("PRESS ENTER")
    
def fromBaudot(letter):
    boolList = [False]*5
    bits = list(bin(baudot.index(letter)))[2:] #take a list of chars
    bits = ["0"]*(5-len(bits)) + bits #make it 5bits
    for b in range(0,5):
        if bits[b] == "0":
            boolList[b] = False
        else:
            boolList[b] =True
    return boolList

def toBaudot(boolList):
    #print(boolList)
    binStr = "0b"
    for bit in boolList:
        if bit == True:
            binStr = binStr + "1"
        else:
            binStr = binStr + "0"
    #print(binStr)
    return baudot[eval(binStr)]
        

wheels = getWheels(key) #generate wheels and write to global variable

################## EVERYTHING BELOW THIS LINE IS PURELY USER INTERFACE
def main():
    global wheelPos
    end = False
    while end == False:
        print("""To demonstrate the cypher the following modes are available:
1 - Encyrpt strings
2 - Print the curent settings on the wheels
3 - Print the key (hardcoded)
4 - Reset wheels to starting position, necesary for decryption
Q - End program
Enter the choice you want to select:""")
        choice = input().upper()        
        if choice == "1":
            #wheelPos = [0]*len(wheelPop)<< used to reset wheels
            stringWise()
        elif choice == "2":
            printWheels(wheels)
        elif choice == "3":
            global key; print("The current key is:\n",key,"\n")
            time.sleep(1)
        elif choice == "4":
            wheelPos = [0]*len(wheelPop)
            print("done")
        elif choice == "Q":
            end = True
        else:
            print("Invalid input.")
            time.sleep(0.5)
            
def stringWise():
    string = input("Enter a string to be encoded. Only letters in the baudot code can be used, ie letters, and spaces:\n")
    valid = True
    for char in list(string):
        if char.upper() not in baudot:
            valid = False
    if valid == True:
        print(encodeString(string))
    else:
        print("Invalid input. Does not contain only baudot characters.")
    input("\nPRESS ENTER")

main()



