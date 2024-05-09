                       Python Programming with Data Science
PART-A
1)Build a program called “GuessMyNumber”. The computer will generate a random number between 1 and 10. The user types in a number and the computer replies “lower” if the random number is lower than the guess, “higher” if the random number is higher , and “correct!” if the guess is correct. The player can continue guessing until the guess is right.

import random
rnum=random.randint(1, 10)
while True:
    guess=int(input("Enter the guess number\n"))
    if guess==rnum:
        print("CORRECT!")
        break
    elif guess<rnum:
        print("Guess is lower")
    else:
        print("The guess is higher")
print("Game over!!!")        
       
Output:
Enter the guess number
9
The guess is higher
Enter the guess number
6
Guess is lower
Enter the guess number
8
CORRECT!
Game over!!!

2)Write a Python program to generate first ‘n ’Fibonacci numbers.
n = int(input("Enter n:"))
n1,n2 = 0,1

if n<=0:
    print("Please enter a positive integer")
elif n==1:
    print("Fibonacci sequence upto",n,":")
    print(n1)
else:
    print("Fibonacci series:")
    for i in range(n):
        print(n1,end=" ")
        n3 = n1+n2
        n1=n2
        n2=n3

3)Write a program to count the number of each vowel in a given string.

ipstr=input("Enter the string :")

acount=ecount=icount=ocount=ucount=0

for ch in ipstr :
    if ch=='a':
        acount+=1
    if ch=='e':
        ecount+=1
    if ch=='i':
        icount+=1
    if ch=='o':
        ocount+=1
    if ch=='u':
        ucount+=1
    
print("Total number of a= ",acount)
    
print("Total number of e=",ecount)
    
print("Total number of i",icount)
    
print("Total number of o=",ocount)
    
print("Total number of u=",ucount)

Output:
Enter the string :volvo car is the safest car in the world
Total number of a=  3
Total number of e= 3
Total number of i 2
Total number of o= 3
Total number of u= 0

4)Write a program to count the number of capital letters and display the position of each capital letter in a user entered string via keyboard.
ipstr=input("Enter the string :")
count=0
for i in range(len(ipstr)):
    if ipstr[i].isupper():
        count+=1
        print("The charachter",ipstr[i],"is at position ",i+1)
print("The total count of apital letters in the string is :",count)

ouput:
Enter the string :Hello World!
The charachter H is at position  1
The charachter W is at position  7
The total count of apital letters in the string is : 2

5)Write a program to remove all punctuations like “’!()-[]{};:’’’,\,<>,/,?,@,#,$, %^&*_~” from the string provided by the user
punctuation_str = "&%#$@![]{}?<>^:;()"

mystr = input("Enter your string:\n")

punctuation_str = "&%#$@![]{}?<>^:;()"

optstr = ""
for ch in mystr:
    if(ch not in punctuation_str):
        optstr = optstr + ch
        
print("output string is:",optstr)
        
Output:
Enter your string:
Hello ! how are you
output string is: Hello  how are you



6)Consider two strings, String1 and String2 and display the merged string as output. The merged string should be the capital letters from both the strings in the order they appear. 
Sample Input: String1:ILikeC 
String2:MaryLikesPython
 Merged string should be ILCMLP

str1=input("Enter the string 1: ")
str2=input("Enter the string 2: ")
opstr=""

str3=str1+str2

for ch in str3:
    if ch.isupper():
        opstr=opstr+ch

print("Concatinated string is : ",opstr)

Output:
Enter the string 1: Uttara 
Enter the string 2: Kannada
Concatinated string is :  UK




7) Write a python program to create two separate lists of integers. List elements of both the list shall be read from keyboard input. Program should display “Lists are symmetrical” if both the lists contain equal number of even and odd numbers.
def read(lst,n):
    print("Enter the ",n,"elemnt into the list :")
    for i in range(n):
        ele=int(input())
        lst.append(ele)
        
def findcount(lst):
    ecount=ocount=0
    for i in lst:
        if i%2==0 :
            ecount+=1 
        else:
            ocount+=1 
    return ecount,ocount

lst1=[]
lst2= []

n1=int(input("Enter the size of the list 1 :\n"))
read(lst1,n1)


n2=int(input("Enter the size of the list 2 :\n"))
read(lst2,n2)

e1count,o1count=findcount(lst1)
e2count,o2count=findcount(lst2)

if e1count==e2count and o1count==o2count:
    print("Lists are symmetrical")
    
else:
    print("Lists are not  symmetrical")
   
Output: 
1)
Enter the size of the list 1 :
4
Enter the  4 elemnt into the list :
1
2
3
4
Enter the size of the list 2 :
4
Enter the  4 elemnt into the list :
4
3
2
1
Lists are symmetrical
2)
Enter the size of the list 1 :
4
Enter the  4 elemnt into the list :
2
4
6
7
Enter the size of the list 2 :
4
Enter the  4 elemnt into the list :
3
4
6
7
Lists are not  symmetrical




8)Write a function that returns the index of the smallest element in a list of integers. If the number of such elements is greater than 1, return the smallest index. 
Use the following header: def indexOfSmallestElement(lst):
 Write a program that prompts the user to enter a list of numbers, invokes this function to return the index of the smallest element and displays the index.
def indexOfSmallestElement(lst):
    small=min(lst)
    index=lst.index(small)
    return small,index

lst=[]
n=int(input("Enter the size of the list :"))
print("Enter ",n," elements to the list :")
for i in range(n):
    ele=int(input())
    lst.append(ele)

if len(lst) >1:
    small,index=indexOfSmallestElement(lst)
    print("The smallest integer is ",small,"is in the index ",index)
else:
    print("Invalid number of inputs")

Output:
Enter the size of the list :5
Enter  5  elements to the list :
23
75
34
76
98
The smallest integer is  23 is in the index  0

9)Write a binary search function which searches an item in a sorted list. The function should return the index of element to be searched in the list.def binsearch(lst,key):
def binsearch(lst,key):
      low=0
      high=len(lst)-1
      while low<=high:
          mid=(low+high)//2
          if key==lst[mid]:
              return mid
          elif key<lst[mid]:
              high=mid-1
          else:
              low=mid+1
      return -1
 
lst=[]
n=int(input("Enter the size of n\n"))
print("Enter",n,"elements")
for i in range(n):
    ele=int(input())
    lst.append(ele)
lst.sort()
print("Sorted lis is:",lst)
key=int(input("Enter the key to be searched \n"))
pos=binsearch(lst,key)
if pos==-1:
    print("Key not found")
else:
    print("Key found at position",pos+1)

OUTPUT:
Enter the size of n
5
Enter 5 elements
6
9
2
1
5
Sorted lis is: [1, 2, 5, 6, 9]
Enter the key to be searched
5
Key found at position 3


Enter the size of n
5
Enter 5 elements
9
8
7
6
4
Sorted lis is: [4, 6, 7, 8, 9]
Enter the key to be searched
1
Key not found

10)Given below is the list of marks scored by students. Find top three scorers for the course and also display the average marks scored by all students. Implement the solution using Python Programming
Student Name
Marks
John
86.5
Jack
91.5
Jill
84.5
Harry
72.1
Joe
80.5


student={
    "John":86.5,
    "Jack":91.2,
    "Jill":84.5,
    "Harry":72.1,
    "Joe":80.5
    }
mlist=list(student.values())
mlist.sort(reverse=True)
mydct={}
for mark in mlist:
    for key in student:
        if student[key]==mark:
            mydct[key]=mark
count=1
for key,value in mydct.items():
    if count>3:
        break
    print(key,value)
    count+=1   
sum=0
for value in mlist:
    sum=sum+value
print("Average marks=",sum/len(mydct))
Output :
Jack 91.2
John 86.5
Jill 84.5
Average marks= 82.96

11)Write a program that will count the number of characters, words, and lines in a file. Words are separated by a white-space character. Your program should prompt the user to enter a filename.

ipfile=input("Enter the filename\n")
f1=open(ipfile)
lcount=wcount=ccount=0
for line in f1:
     lst=line.split()
     wcount=wcount+len(lst)
     for word in lst:
         ccount=ccount+len(word)
     lcount=lcount+1
print("Total lines=",lcount)
print("Total words=",wcount)
print("Total characters=",ccount)


test.txt
The sun shines.
Birds chirp loudly.
Nature's beauty surrounds.

OUTPUT :

Enter the filename
test.txt
Total lines= 3
Total words= 9
Total characters= 54

12)Suppose that a text file contains marks for 6 courses for a student in a line. Each course marks is separated by space as delimiter. File contains marks for ‘n’ number of students in separate lines. Write a program that reads the marks from the file for each student and displays the total and average. Your program should prompt the user to enter a filename.

ipfile=input("Enter the filename\n")
f1=open(ipfile)
count=1
print("---------------------------------")
for line in f1:
    lst=line.split()
    sum=0
    for mark in lst:
        sum=sum+int(mark)
    avg=sum/len(lst)
    print("Student",count,"marks:")
    print("Total marks=",sum)
    print("Avearge=",avg)
    count+=1
    print("-------------------------------")

OUTPUT
Enter the filename
marks.txt
---------------------------------
Student 1 marks:
Total marks= 415
Avearge= 69.16666666666667
-------------------------------
Student 2 marks:
Total marks= 370
Avearge= 61.666666666666664
-------------------------------
Student 3 marks:
Total marks= 496
Avearge= 82.66666666666667
-------------------------------
Student 4 marks:
Total marks= 373
Avearge= 62.166666666666664
-------------------------------
Student 5 marks:
Total marks= 340
Avearge= 56.666666666666664
-------------------------------
Student 6 marks:
Total marks= 382
Avearge= 63.666666666666664
-------------------------------











PART- B

13) Design a class named Account that contains: A private int data field named accountno for the account. A private float data field named balance for the account. A constructor that creates an account with the specified accountno and Initial balance (default 100). A method named withdraws that withdraws a specified amount from the account. A minimum balance of 100 should be maintained for each account. A method named deposit that deposits a specified amount to the specific account. 
Write a program that maintains ‘n’ number of Account objects with unique accountno and supports following operations.
 a) New account creation 
 b) deposit operation for a given account no
 c) withdraw operation for a given account no 
 d) Display account no with highest balance
code:

class Account:
    def __init__(self,number):
        self.__number=number
        self.__balance=100
    def deposit(self,amount):
        self.__balance+=amount
        print("Account Balance is:",self.__balance)
    def withdraw(self,amount):
        if self.__balance-amount<100:
            print("Low balance for withdrawal")
        else:
            self.__balance-=amount
            print("Account Balance is:",self.__balance)
    def getbalance(self):
        return self.__balance
acclist=[]
accnolist=[]
while True:
    print("\nEnter")
    print("1:Account creation\n2:Deposit\n3:Withdraw")
    print("4:Get highest balance\n5:Exit")
    choice=int(input("Enter the choice: "))
    if choice==1:
        number=int(input("Enter the account number: "))
        if number in accnolist:
            print("Account already exists")
        else:
              new=Account(number)
              acclist.append(new)
              accnolist.append(number)
    elif choice==2:
        number=int(input("Enter the account number: "))
        if number in accnolist:
            amount=float(input("Enter the amount for deposit: "))
            index=accnolist.index(number)
            acclist[index].deposit(amount)
        else:
            print("Account not found")
    elif choice==3:
       number=int(input("Enter the account number: "))
       if number in accnolist:
           amount=float(input("Enter the amount for withdrawal: "))
           index=accnolist.index(number)
           acclist[index].withdraw(amount)
       else:
           print("Account not found")
    elif choice==4:
        ballist=[]
        for object in acclist:
            ballist.append(object.getbalance())
        if len(ballist)==0:
            print("No account exists")
        else:
            high=max(ballist)
            index=ballist.index(high)
            print("Highest balance value:",high)
            print("Account number is:",accnolist[index])
    else:
        break

Output :

Enter
1:Account creation
2:Deposit
3:Withdraw
4:Get highest balance
5:Exit
Enter the choice: 1
Enter the account number: 123

Enter
1:Account creation
2:Deposit
3:Withdraw
4:Get highest balance
5:Exit
Enter the choice: 4
Highest balance value: 100
Account number is: 123

Enter
1:Account creation
2:Deposit
3:Withdraw
4:Get highest balance
5:Exit
Enter the choice: 2
Enter the account number: 123
Enter the amount for deposit: 1000
Account Balance is: 1100.0

Enter
1:Account creation
2:Deposit
3:Withdraw
4:Get highest balance
5:Exit
Enter the choice: 3
Enter the account number: 123
Enter the amount for withdrawal: 500
Account Balance is: 600.0

Enter
1:Account creation
2:Deposit
3:Withdraw
4:Get highest balance
5:Exit
Enter the choice: 4
Highest balance value: 600.0
Account number is: 123

Enter
1:Account creation
2:Deposit
3:Withdraw
4:Get highest balance
5:Exit
Enter the choice: 5





14) Write a program to create a list to maintain country names, respective capital and its population. The program should support following operations 
a) To enter country name, capital and its population.
 b) To accept the name of a country as an input and print the corresponding capital name and population as output. Otherwise, the program should print an appropriate message if the country is not found in the list. 
c) To display the country name with highest population

code:
class Country:
    def __init__(self,name,capital,population):
        self.__name=name
        self.__capital=capital
        self.__population=population
    def display(self):
        print("Capital:",self.__capital)
        print("Population: ",self.__population)
clist=[]
cname=[]
plist=[]
while True:
    print("\nEnter")
    print("1:Enter Details\n2:Get Capital")
    print("3:Highest population\n4:Exit\n")
    choice=int(input("Enter your choice: "))
   
    if choice==1:
        name=input("Enter the country name: ")
        if name in cname:
            print("Country already exists")
        else:
            capital=input("Enter the capital: ")
            population=int(input("Enter the population: "))
            new=Country(name,capital,population)
            clist.append(new)
            cname.append(name)
            plist.append(population)
    elif choice==2:
        name=input("Enter the country name: ")
        if name in cname:
            print("Details:")
            index=cname.index(name)
            clist[index].display()
        else:
            print("Country info does not exist\n")
    elif choice==3:
        if len(plist)==0:
            print("No data exist")
        else:
            high=max(plist)
            index=plist.index(high)
            print("Highest population is:",high)
            print("Country name is:",cname[index])
    else:
        break
Output:
Enter
1:Enter Details
2:Get Capital
3:Highest population
4:Exit

Enter your choice: 1
Enter the country name: USA
Enter the capital: Washington D.C.
Enter the population: 331000000

Enter
1:Enter Details
2:Get Capital
3:Highest population
4:Exit

Enter your choice: 1
Enter the country name: China
Enter the capital: Beijing
Enter the population: 1444000000

Enter
1:Enter Details
2:Get Capital
3:Highest population
4:Exit

Enter your choice: 2
Enter the country name: USA
Details:
Capital: Washington D.C.
Population:  331000000

Enter
1:Enter Details
2:Get Capital
3:Highest population
4:Exit

Enter your choice: 3
Highest population is: 1444000000
Country name is: China

Enter
1:Enter Details
2:Get Capital
3:Highest population
4:Exit

Enter your choice: 4



  





15)


import matplotlib.pyplot as plt

f1=open("keys.txt")
line=f1.readline()
key=line.split()
f1.close()

f1=open("marks1.txt")
mark=[]
name=[]
print("Total marks of each student is")
for line in f1:
    ans=line.split()
    total=0
    for i in range(1,len(ans)):
        if ans[i]==key[i-1]:
            total+=1
    print(ans[0],total)
    mark.append(total)
    name.append(ans[0])
high=max(mark)
index=mark.index(high)
print("Top Score:",high)
print("Top Scorer:",name[index])

#Computing the range
r1=r2=r3=0
for value in mark:
    if value>=0 and value<=2:
        r1+=1
    elif value>=3 and value<=6:
        r2+=1
    else:
        r3+=1
#plot the graph
x=["0-2","3-6","7-10"]
y=[r1,r2,r3]
w=0.1
plt.bar(x,y,width=w,color=['r','g','b'])
plt.title("Students Performance in MCQ")
plt.xlabel("Score Range")
plt.ylabel("No.of Students")
plt.show()


OUTPUT:

Total marks of each student is
Student0 7
Student1 10
Student2 5
Student3 3
Student4 8
Student5 7
Student6 7
Student7 7
Student8 2
Student9 5
Top Score: 10
Top Scorer: Student1

17)CIE and SEE marks of Ten students for 3rd semester is stored in a CSV file. Marks details are stored in a worksheet and the format of marks details is shown below. The worksheet provides CIE and SEE marks for the courses CS301 TO CS306. Perform data analysis using numpy, pandas, matplotlib python libraries for the following scenarios. 
a) Display the subject code with highest avg score in the semester 
b) Visualize each student’s avg CIE and avg SEE for 3 rd sem using a bar chart graph.
import pandas as pd
import pandas as pd
import matplotlib.pyplot as plt

df1=pd.read_csv("semcie.csv")

avgsrs=pd.Series()                               #initially empty

for i in range(1,len(df1.columns),2):
    code=df1.columns[i][:5]                      #to get the sub code without CIE letters
    avg=(df1.iloc[::,i].mean(0)+df1.iloc[::,i+1].mean(0))/2
    avgsrs[code]=avg                              #stores in series the sub code and its avg
print("Highest average=",avgsrs.max())
print("Subject code with highest average=",avgsrs.idxmax())


avgcie=df1.iloc[::,1::2].mean(1)
avgsee=df1.iloc[::,2::2].mean(1)

#plotting operation
x1=[]     #ticker values for red bars
x2=[]     #ticker values for green bars
ticks=[]  #ticker marks for labels
w=0.2

usn=df1['USN']
for i in range(len(usn)):
    x1.append(i)
    x2.append(i+w)
    ticks.append(i+w/2)
plt.figure(figsize=(10,5))    #to set the graph size to prevent overlapping(width,height)
plt.bar(x1,avgcie,width=w,color='r',label="CIE")
plt.bar(x2,avgsee,width=w,color='g',label="SEE")
plt.xticks(ticks,usn)        #to add usn label on x axis
plt.xlabel("Student USNs")
plt.ylabel("CIE vs SEE score")
plt.title("Student performance Average CIE vs SEE")
plt.legend()
plt.show()



ouput:
Highest average= 38.95
Subject code with highest average= CS301


18)Placement data distribution across various branches and companies is shown in the table below for the academic year 2020.Perform data analysis using numpy, pandas, matplotlib python libraries for the following scenarios. 
a) Display the branch name with highest no of total placements across all the companies. 
b) Display the company name with highest no of total placements across all the branches. 
c) Display branch name and company name having highest no of placements.(e.g,’CSE’-‘DXC’) 
d) Draw a pie chart showing distribution of total placements across all the branches.
 e) Draw a pie chart showing distribution of total placements across all the companies.

Code:

import pandas as pd
import matplotlib.pyplot as plt

df1=pd.read_csv("placement.csv")

branchtotal=df1.iloc[::,1::].sum(1)   #sum is horizontally
branchmax=branchtotal.max()
maxidx=branchtotal.idxmax()
print("Highest placement branchwise=",branchmax)
print("Branch name=",df1.iloc[maxidx,0])

comptotal=df1.iloc[::,1::].sum(0)    #sum is performed vertically
print("Highest Placement companywise",comptotal.max())
print("Company name=",comptotal.idxmax())

branch=df1['Branch']   #to get branch name in Branch column
number=df1.iloc[::,1::].max(1)  #max no of placement in branch
company=df1.iloc[::,1::].idxmax(1) #get company names for number which is index
df2=pd.DataFrame(data={'Branch':branch,'Company':company,'Number':number})
print(df2)

#plotting pie chart

plt.figure(figsize=(6,6))
plt.pie(branchtotal,labels=branch)
plt.show()


plt.figure(figsize=(6,6))
plt.pie(comptotal,labels=df1.columns[1:],rotatelabels=True)
plt.show()



output:

Highest placement branchwise= 279.0
Branch name= CSE
Highest Placement companywise 137.0
Company name= DXC
  Branch        Company 		 Number
0  Civil            	DXC               2.0
1  Mech.  	Infosys (CRP)    	15.0
2    E&E       TCS Ninja     		9.0
3    E&C      Capgemini   		39.0
4    CSE        DXC    		76.0
5    ISE        Mindtree   		39.0
6     BT       Infosys (CRP)     	4.0
7    MCA    Capgemini     	9.0
                    



