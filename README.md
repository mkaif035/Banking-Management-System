#include <iostream>
#include<string.h>
#include<conio.h>
#include<stdio.h>
#include<fstream>
#include<stdlib.h>

using namespace std;

class Bank{
private:
int acno,balance;
char cname[20],actype[20];
void setBalance(){
if(strcmpi(actype,"saving")==0)
balance=5000;
else if(strcmpi(actype,"current")==0)
balance=10000;
}
public:
void getAccountDetail(){
cout<<"\n\nenter account no";
cin>>acno;
fflush(stdin);
cout<<"\n\nenter customer name";
gets(cname);
fflush(stdin);
cout<<"\n\nenter account type";
fflush(stdin);
gets(actype);
fflush(stdin);
setBalance();
}
void depositMoney(int amt)
{
balance=balance+amt;
cout<<"\n\namount deposited and your current balance is"<<balance;
}
void withdrawMoney(int amt)
{
if(balance>amt){
balance=balance-amt;
cout<<"\n\namount withdrawn and your current balance is"<<balance;
}
else{
cout<<"\n\nnot sufficient fund";
}
}
void enquiryAccount(){
cout<<"\n\nAccount no is\t"<<acno;
cout<<"\n\ncustomer name is\t"<<cname;
cout<<"\n\naccount type is\t"<<actype;
cout<<"\n\nbalance is\t"<<balance;
}
int getAcno(){
return acno;
}
};

void openAccount(){
ofstream f1("C:\\Users\\Pradeep Kumar\\Documents\\bankapplication\\mybank.dat",ios::binary|ios::app);
Bank cust;
cust.getAccountDetail();
f1.write((char *)&cust,sizeof(cust));
f1.close();
}

void displayAll(){
ifstream f1("C:\\Users\\Pradeep Kumar\\Documents\\bankapplication\\mybank.dat",ios::binary|ios::in);
Bank cust;
while(f1.read((char *)&cust,sizeof(cust)))
{
cust.enquiryAccount();
cout<<"\n=====================================\n";
}
f1.close();
}

void displayAccount(){
ifstream f1("C:\\Users\\Pradeep Kumar\\Documents\\bankapplication\\mybank.dat",ios::binary|ios::in);
Bank cust;
int f=0,acno;
cout<<"\n\nenter account no to be search";
cin>>acno;
while(f1.read((char *)&cust,sizeof(cust)))
{
if(cust.getAcno()==acno){
f=1;
break;
}
}
if(f==0)
cout<<"\n\naccount no not found";
else
cust.enquiryAccount();
f1.close();
}

void closeAccount(){
ifstream f1("C:\\Users\\Pradeep Kumar\\Documents\\bankapplication\\mybank.dat",ios::binary|ios::in);
ofstream f2("C:\\Users\\Pradeep Kumar\\Documents\\bankapplication\\mytemp.dat",ios::binary|ios::out);
Bank cust;
int f=0,acno;
cout<<"\n\nenter account no to be close";
cin>>acno;
while(f1.read((char *)&cust,sizeof(cust)))
{
if(cust.getAcno()==acno){
f=1;
continue;
}
else{
f2.write((char *)&cust,sizeof(cust));
}
}
if(f==0){
   cout<<"\n\naccount no not found";
}
else
{
f1.close();
f2.close();
remove("C:\\Users\\Pradeep Kumar\\Documents\\bankapplication\\mybank.dat");
rename("C:\\Users\\Pradeep Kumar\\Documents\\bankapplication\\mytemp.dat","C:\\Users\\Pradeep Kumar\\Documents\\bankapplication\\mybank.dat");
}
f1.close();
}

void depositAmount(){
fstream f1("C:\\Users\\Pradeep Kumar\\Documents\\bankapplication\\mybank.dat",ios::binary|ios::in|ios::out);
Bank cust;
int f=0,acno,amt;
cout<<"\n\nenter account no in which amount to be deposited";
cin>>acno;
cout<<"\n\nenter amount to be deposited";
cin>>amt;
while(f1.read((char *)&cust,sizeof(cust)))
{
if(cust.getAcno()==acno){
f=1;
break;
}
}
if(f==0)
cout<<"\n\naccount no not found";
else
{
int n=f1.tellp();
cust.depositMoney(amt);
f1.seekp(n-sizeof(cust),ios::beg);
f1.write((char *)&cust,sizeof(cust));
}
f1.close();
}

void withdrawAmount(){
fstream f1("C:\\Users\\Pradeep Kumar\\Documents\\bankapplication\\mybank.dat",ios::binary|ios::in|ios::out);
Bank cust;
int f=0,acno,amt;
cout<<"\n\nenter account no from which amount to be withdrawn";
cin>>acno;
cout<<"\n\nenter amount to be withdrawn";
cin>>amt;
while(f1.read((char *)&cust,sizeof(cust)))
{
if(cust.getAcno()==acno){
f=1;
break;
}
}
if(f==0)
cout<<"\n\naccount no not found";
else
{
int n=f1.tellp();
cust.withdrawMoney(amt);
f1.seekp(n-sizeof(cust),ios::beg);
f1.write((char *)&cust,sizeof(cust));
}
f1.close();
}

int main()
{
int choice,k=0;
do{
cout<<"\n\nBank Menu";
cout<<"\n\n1.Open Account";
cout<<"\n\n2. Deposit Money";
cout<<"\n\n3. Withdraw Money";
cout<<"\n\n4. Enquiry Account";
cout<<"\n\n5. Display All";
cout<<"\n\n6. Close Account";
cout<<"\n\n7. Transfer Of Money";
cout<<"\n\n8. Exit";
cout<<"\n\nEnter your choice";
cin>>choice;
switch(choice){
case 1:
openAccount();
break;
case 2:
depositAmount();
break;
case 3:
withdrawAmount();
break;
case 4:
displayAccount();
break;
case 5:
displayAll();
break;
case 6:
closeAccount();
break;
case 7:
break;
case 8:
exit(0);
default:
    cout<<"\n\ninvalid choice";
}
cout<<"\n\ndo u want to cont..press 1 for no";
cin>>k;
}while(k!=1);
return 0;
}
