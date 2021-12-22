                        SUPER MARKET BILLING SYSTEM 

Source code:-

#include<iostream>
#include<windows.h>
#include<conio.h>
#include<fstream>
#include<cstring>
#include<cstdio>
#include<cstdlib>
#include<iomanip>
using namespace std;
int k=7,r=0,flag=0;
COORD coord = {0, 0};
void gotoxy(int x, int y)
{
 COORD coord;
 coord.X = x;
 coord.Y = y;
 SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE), coord);
}
struct date
{int mm,dd,yy;};
ofstream fout;
ifstream fin;
class item
{
int itemno;
char name[25];
date d;
public:
void add()
{
cout<<"\n\n\tItem No: ";
cin>>itemno;
cout<<"\n\n\tName of the item: ";
cin>>name;
cout<<"\n\n\tManufacturing Date(dd-mm-yy): ";
cin>>d.mm>>d.dd>>d.yy;
}
void show()
{
cout<<"\n\tItem No: ";
cout<<itemno;
cout<<"\n\n\tName of the item: ";
cout<<name;
cout<<"\n\n\tDate : ";
cout<<d.mm<<"-"<<d.dd<<"-"<<d.yy;
}
void report()
{
gotoxy(3,k);
cout<<itemno;
gotoxy(13,k);
puts(name);
}
int retno()
{
    return(itemno);
}
};
class amount: public item
{
float price,qty,tax,gross,dis,netamt;
public:
void add();
void show();
void report();
void calculate();
void pay();
float retnetamt()
{
    return(netamt);
}
}amt;
void amount::add()
{
item::add();
cout<<"\n\n\tPrice: ";
cin>>price;
cout<<"\n\n\tQuantity: ";
cin>>qty;
cout<<"\n\n\tTax percent: ";
cin>>tax;
cout<<"\n\n\tDiscount percent: ";
cin>>dis;
calculate();
fout.write((char *)&amt,sizeof(amt));
fout.close();
}
void amount::calculate()
{gross=price+(price*(tax/100));
netamt=qty*(gross-(gross*(dis/100)));
}
void amount::show()
{fin.open("itemstore.dat",ios::binary);
fin.read((char*)&amt,sizeof(amt));
item::show();
cout<<"\n\n\tNet amount: ";
cout<<netamt;
fin.close();
}
void amount::report()
{item::report();
gotoxy(23,k);
cout<<price;
gotoxy(33,k);
cout<<qty;
gotoxy(44,k);
cout<<tax;
gotoxy(52,k);
cout<<dis;
gotoxy(64,k);
cout<<netamt;
k=k+1;
if(k==50)
{gotoxy(25,50);
cout<<"PRESS ANY KEY TO CONTINUE...";
getch();
k=7;
system("cls");
gotoxy(30,3);
cout<<" ITEM DETAILS ";
gotoxy(3,5);
cout<<"NUMBER";
gotoxy(13,5);
cout<<"NAME";
gotoxy(23,5);
cout<<"PRICE";
gotoxy(33,5);
cout<<"QUANTITY";
gotoxy(44,5);
cout<<"TAX";
gotoxy(52,5);
cout<<"DEDUCTION";
gotoxy(64,5);
cout<<"NET AMOUNT";
}
}
void amount::pay()
{show();
cout<<"\n\n\n\t\t*********************************************";
cout<<"\n\t\t                 DETAILS                  ";
cout<<"\n\t\t*********************************************";
cout<<"\n\n\t\tPRICE                     :"<<price;
cout<<"\n\n\t\tQUANTITY                  :"<<qty;
cout<<"\n\t\tTAX PERCENTAGE              :"<<tax;
cout<<"\n\t\tDISCOUNT PERCENTAGE         :"<<dis;
cout<<"\n\n\n\t\tNET AMOUNT              :Rs."<<netamt;
cout<<"\n\t\t*********************************************";
}
int main()
{
cout.setf(ios::fixed);
cout.setf(ios::showpoint);
cout<<setprecision(2);
fstream tmp("temp.dat",ios::binary|ios::out);
menu:
system("cls");
gotoxy(25,2);
cout<<"Super Market Billing ";
gotoxy(25,3);
cout<<"===========================\n\n";
cout<<"\n\t\t1.Bill Report\n\n";
cout<<"\t\t2.Add/Remove/Edit Item\n\n";
cout<<"\t\t3.Show Item Details\n\n";
cout<<"\t\t4.Exit\n\n";
cout<<"\t\tPlease Enter Required Option: ";
int ch,ff;
float gtotal;
cin>>ch;
switch(ch)
{case 1:ss:
system("cls");
gotoxy(25,2);
cout<<"Bill Details";
gotoxy(25,3);
cout<<"================\n\n";
cout<<"\n\t\t1.All Items\n\n";
cout<<"\t\t2.Back to Main menu\n\n";
cout<<"\t\tPlease Enter Required Option: ";
int cho;
cin>>cho;
if(cho==1)
{system("cls");
gotoxy(30,3);
cout<<" BILL DETAILS ";
gotoxy(3,5);
cout<<"ITEM NO";
gotoxy(13,5);
cout<<"NAME";
gotoxy(23,5);
cout<<"PRICE";
gotoxy(33,5);
cout<<"QUANTITY";
gotoxy(44,5);
cout<<"TAX %";
gotoxy(52,5);
cout<<"DISCOUNT %";
gotoxy(64,5);
cout<<"NET AMOUNT";
fin.open("itemstore.dat",ios::binary);
if(!fin)
{cout<<"\n\nFile Not Found...";
goto menu;}
fin.seekg(0);
gtotal=0;
while(!fin.eof())
{fin.read((char*)&amt,sizeof(amt));
if(!fin.eof())
{amt.report();
gtotal+=amt.retnetamt();
ff=0;}
if(ff!=0) gtotal=0;
}gotoxy(17,k);
cout<<"\n\n\n\t\t\tGrand Total="<<gtotal;
getch();
fin.close();
}
if(cho==2)
{goto menu;}
goto ss;
case 2:
db:
system("cls");
gotoxy(25,2);
cout<<"Bill Editor";
gotoxy(25,3);
cout<<"=================\n\n";
cout<<"\n\t\t1.Add Item Details\n\n";
cout<<"\t\t2.Edit Item Details\n\n";
cout<<"\t\t3.Delete Item Details\n\n";
cout<<"\t\t4.Back to Main Menu ";
int apc;
cin>>apc;
switch(apc)
{
case 1:fout.open("itemstore.dat",ios::binary|ios::app);
amt.add();
cout<<"\n\t\tItem Added Successfully!";
getch();
goto db;
case 2:
int ino;
flag=0;
cout<<"\n\n\tEnter Item Number to be Edited :";
cin>>ino;
fin.open("itemstore.dat",ios::binary);
fout.open("itemstore.dat",ios::binary|ios::app);
if(!fin)
{cout<<"\n\nFile Not Found...";
goto menu;
}
fin.seekg(0);
r=0;
while(!fin.eof())
{fin.read((char*)&amt,sizeof(amt));
if(!fin.eof())
{int x=amt.item::retno();
if(x==ino)
{flag=1;
fout.seekp(r*sizeof(amt));
system("cls");
cout<<"\n\t\tCurrent Details are\n";
amt.show();
cout<<"\n\n\t\tEnter New Details\n";
amt.add();
cout<<"\n\t\tItem Details editted";
}
}r++;
}
if(flag==0)
{
cout<<"\n\t\tItem No does not exist...Please Retry!";
getch();
goto db;
}
fin.close();
getch();
goto db;
case 3:flag=0;
cout<<"\n\n\tEnter Item Number to be deleted :";
cin>>ino;
fin.open("itemstore.dat",ios::binary);
if(!fin)
{cout<<"\n\nFile Not Found...";
goto menu;
}
fin.seekg(0);
while(fin.read((char*)&amt, sizeof(amt)))
{int x=amt.item::retno();
if(x!=ino)
tmp.write((char*)&amt,sizeof(amt));
else
{flag=1;}
}
fin.close();
tmp.close();
fout.open("itemstore.dat",ios::trunc|ios::binary);
fout.seekp(0);
tmp.open("temp.dat",ios::binary|ios::in);
if(!tmp)
{cout<<"Error in File";
goto db;
}
while(tmp.read((char*)&amt,sizeof(amt)))
fout.write((char*)&amt,sizeof(amt));
tmp.close();
fout.close();
if(flag==1)
cout<<"\n\t\tItem Succesfully Deleted";
else if (flag==0)
cout<<"\n\t\tItem does not Exist! Please Retry";
getch();
goto db;
case 4:
goto menu;
default: cout<<"\n\n\t\tWrong Choice!!! Retry";
getch();
goto db;
}
case 3:system("cls");
flag=0;
int ino;
cout<<"\n\n\t\tEnter Item Number :";
cin>>ino;
fin.open("itemstore.dat",ios::binary);
if(!fin)
{cout<<"\n\nFile Not Found...\nProgram Terminated!";
goto menu;
}
fin.seekg(0);
while(fin.read((char*)&amt,sizeof(amt)))
{int x=amt.item::retno();
if(x==ino)
{amt.pay();
flag=1;
break;
}
}
if(flag==0)
cout<<"\n\t\tItem does not exist....Please Retry!";
getch();
fin.close();
goto menu;
case 4:system("cls");
gotoxy(20,20);
cout<<"ARE YOU SURE, YOU WANT TO EXIT (Y/N)?";
char yn;
cin>>yn;
if((yn=='Y')||(yn=='y'))
{gotoxy(12,20);
system("cls");
cout<<"************************** THANKS **************************************";
getch();
exit(0);
}
else if((yn=='N')||(yn=='n'))
goto menu;
else{goto menu;}
default:cout<<"\n\n\t\tWrong Choice....Please Retry!";
getch();
goto menu;
}
return 0;
}






Output:-

                                      Super Market Billing
                         ===========================


               1.Bill Report
               2.Add/Remove/Edit Item
               3.Show Item Details
              4.Exit

               Please Enter Required Option: 2


                         Bill Editor
               =================


                1.Add Item Details
                2.Edit Item Details
                3.Delete Item Details
                4.Back to Main Menu 1


        Item No: 1
       Name of the item: pepsi
       Manufacturing Date(dd-mm-yy): 16-10-2022
       Price: 35
       Quantity: 10
      Tax percent: 5
      Discount percent: 2

      Item Added Successfully!



                                 Bill Edit
                     =================


                1.Add Item Details
               2.Edit Item Details
               3.Delete Item Details
               4.Back to Main Menu 1


        Item No: 2


        Name of the item: perfume
        Manufacturing Date(dd-mm-yy): 10-12-2023
        Price: 185
        Quantity: 5
        Tax percent: 6
        Discount percent: 3

        Item Added Successfully!



                                    Bill Editor
                         =================


                1.Add Item Details
                2.Edit Item Details
                3.Delete Item Details
                4.Back to Main Menu 1
                   Item No: 3


        Name of the item: maggie
        Manufacturing Date(dd-mm-yy): 13-02-2024
        Price: 10
        Quantity: 12
        Tax percent: 4
        Discount percent: 2

        Item Added Successfully!




                                    Bill Editor
                         =================


                1.Add Item Details
                2.Edit Item Details
                3.Delete Item Details
                4.Back to Main Menu
1


        Item No: 4


        Name of the item: chocolate
        Manufacturing Date(dd-mm-yy): 15-05-2023
        Price: 5
       Quantity: 50
       Tax percent: 5
       Discount percent: 2

       Item Added Successfully!




                                     Bill Editor
                         =================


                1.Add Item Details
                2.Edit Item Details
               3.Delete Item Details
               4.Back to Main Menu 1


        Item No: 5


        Name of the item: soap
        Manufacturing Date(dd-mm-yy): 15-07-2024
        Price: 12


        Quantity: 12
       Tax percent: 4
       Discount percent: 2

       Item Added Successfully!





                                      Super Market Billing
                         ===========================


                1.Bill Report
                2.Add/Remove/Edit Item
                3.Show Item Details
               4.Exit

                Please Enter Required Option: 3



         Enter Item Number :1
        Item No: 1
        Name of the item: pepsi
        Date : 16—10--2022
       Net amount: 352.23

                *********************************************
                                                 DETAILS    
               *********************************************

                PRICE                     :35.00
                QUANTITY                  :10.00
                TAX PERCENTAGE              :5.00
                DISCOUNT PERCENTAGE         :2.00


                NET AMOUNT              :Rs.352.23
                *********************************************




         Enter Item Number :2
        Item No: 2
        Name of the item: perfume
        Date : 10--12--2023

        Net amount: 927.47


                *********************************************
                                                   DETAILS
                *********************************************

                PRICE                     :185.00
                QUANTITY                  :5.00
                TAX PERCENTAGE              :6.00
                DISCOUNT PERCENTAGE         :3.00

                NET AMOUNT              :Rs.927.47
                *********************************************



        Enter Item Number :3
        Item No: 3
        Name of the item: maggie
        Date : 13--02--2024

        Net amount: 120.00


                *********************************************
                                                 DETAILS
                *********************************************

                PRICE                     :10.00
                QUANTITY                  :12.00
                TAX PERCENTAGE              :4.00
                DISCOUNT PERCENTAGE         :2.00


                NET AMOUNT              :Rs.120.00
                *********************************************









        Enter Item Number :4
        Item No: 4
        Name of the item: chocolate
        Date : 15--5--2023

        Net amount: 252.35


                *********************************************
                                                 DETAILS
                *********************************************

                PRICE                     :5.00
                QUANTITY                  :50.00
                TAX PERCENTAGE              :3.00
                DISCOUNT PERCENTAGE         :2.00


                NET AMOUNT              :Rs.252.35
                *********************************************





                Enter Item Number :5
                Item No: 5
                Name of the item: soap
               Date : 15--7--2024
               Net amount: 146.76


                *********************************************
                                                  DETAILS
                *********************************************

                PRICE                     :12.00
                QUANTITY                  :12.00
                TAX PERCENTAGE              :4.00
                DISCOUNT PERCENTAGE         :2.00


                NET AMOUNT              :Rs.146.76
                *********************************************











                                      Super Market Billing
                         ===========================


                1.Bill Report
                2.Add/Remove/Edit Item
                3.Show Item Details
                4.Exit
                Please Enter Required Option: 4

                               ARE YOU SURE, YOU WANT TO EXIT (Y/N)?y
************************** THANKS **************************************
--------------------------------
Process exited after 573.7 seconds with return value 0
Press any key to continue . . .





