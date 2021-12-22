                                     BANK MANAGEMENT SYSTEM

Source Code:-

#include<iostream>
#include<fstream>
#include<cctype>
#include<iomanip>
using namespace std;
class account
{
    int acno;
    char name[50];
    int deposit;
    char type;
public:
    void create_account();  
    void show_account() const;  
    void modify();  
    void dep(int);  
    void draw(int); 
    void report() const;    
    int retacno() const;    
    int retdeposit() const; 
    char rettype() const;  
};         
void account::create_account()
{
    cout<<"\nEnter The account No. :";
    cin>>acno;
    cout<<"\n\nEnter The Name of The account Holder : ";
    cin.ignore();
    cin.getline(name,50);
    cout<<"\nEnter Type of The account (C/S) : ";
    cin>>type;
    type=toupper(type);
    cout<<"\nEnter The Initial amount(>=500 for Saving and >=1000 for current ) : ";
    cin>>deposit;
    cout<<"\n\n\nAccount Created..";
}
void account::show_account() const
{
    cout<<"\nAccount No. : "<<acno;
    cout<<"\nAccount Holder Name : ";
    cout<<name;
    cout<<"\nType of Account : "<<type;
    cout<<"\nBalance amount : "<<deposit;
}
void account::modify()
{
    cout<<"\nAccount No. : "<<acno;
    cout<<"\nModify Account Holder Name : ";
    cin.ignore();
    cin.getline(name,50);
    cout<<"\nModify Type of Account : ";
    cin>>type;
    type=toupper(type);
    cout<<"\nModify Balance amount : ";
    cin>>deposit;
}
void account::dep(int x)
{
    deposit+=x;
}
void account::draw(int x)
{
    deposit-=x;
}
void account::report() const
{
    cout<<acno<<setw(10)<<" "<<name<<setw(10)<<" "<<type<<setw(6)<<deposit<<endl;
}
int account::retacno() const
{
    return acno;
}
int account::retdeposit() const
{
    return deposit;
}
char account::rettype() const
{
    return type;
}
void write_account();   
void display_sp(int);   
void modify_account(int);   
void delete_account(int);   
void display_all();     
void deposit_withdraw(int, int); 
void intro();   
int main()
{
    char ch;
    int num;
    intro();
    do
    {
        system("cls");
        cout<<"\n\n\n\t\t\t\t\t\t\t\t\t\t\t\tMAIN MENU";
        cout<<"\n\n\t\t\t\t\t\t\t\t\t\t\t\t01. NEW ACCOUNT";
        cout<<"\n\n\t\t\t\t\t\t\t\t\t\t\t\t02. DEPOSIT AMOUNT";
        cout<<"\n\n\t\t\t\t\t\t\t\t\t\t\t\t03. WITHDRAW AMOUNT";
        cout<<"\n\n\t\t\t\t\t\t\t\t\t\t\t\t04. BALANCE ENQUIRY";
        cout<<"\n\n\t\t\t\t\t\t\t\t\t\t\t\t05. ALL ACCOUNT HOLDER LIST";
        cout<<"\n\n\t\t\t\t\t\t\t\t\t\t\t\t06. CLOSE AN ACCOUNT";
        cout<<"\n\n\t\t\t\t\t\t\t\t\t\t\t\t07. MODIFY AN ACCOUNT";
        cout<<"\n\n\t\t\t\t\t\t\t\t\t\t\t\t08. EXIT";
        cout<<"\n\n\t\t\t\t\t\t\t\t\t\t\t\tSelect Your Option (1-8) ";
        cin>>ch;
        system("cls");
        switch(ch)
        {
        case '1':
            write_account();
            break;
        case '2':
            cout<<"\n\n\tEnter The account No. : "; cin>>num;
            deposit_withdraw(num, 1);
            break;
        case '3':
            cout<<"\n\n\tEnter The account No. : "; cin>>num;
            deposit_withdraw(num, 2);
            break;
        case '4':
            cout<<"\n\n\tEnter The account No. : "; cin>>num;
            display_sp(num);
            break;
        case '5':
            display_all();
            break;
        case '6':
            cout<<"\n\n\tEnter The account No. : "; cin>>num;
            delete_account(num);
            break;
         case '7':
            cout<<"\n\n\tEnter The account No. : "; cin>>num;
            modify_account(num);
            break;
         case '8':
            cout<<"\n\n\tThanks for using bank managemnt system";
            break;
         default :cout<<"a";
        }
        cin.ignore();
        cin.get();
    }while(ch!='8');
    return 0;
}
void write_account()
{
    account ac;
    ofstream outFile;
    outFile.open("account.dat",ios::binary|ios::app);
    ac.create_account();
    outFile.write(reinterpret_cast<char *> (&ac), sizeof(account));
    outFile.close();
}
void display_sp(int n)
{
    account ac;
    bool flag=false;
    ifstream inFile;
    inFile.open("account.dat",ios::binary);
    if(!inFile)
    {
        cout<<"File could not be open !! Press any Key...";
        return;
    }
    cout<<"\nBALANCE DETAILSn";
        while(inFile.read(reinterpret_cast<char *> (&ac), sizeof(account)))
    {
        if(ac.retacno()==n)
        {
            ac.show_account();
            flag=true;
        }
    }
    inFile.close();
    if(flag==false)
        cout<<"\n\nAccount number does not exist";
}
void modify_account(int n)
{
    bool found=false;
    account ac;
    fstream File;
    File.open("account.dat",ios::binary|ios::in|ios::out);
    if(!File)
    {
        cout<<"File could not be open !! Press any Key...";
        return;
    }
    while(!File.eof() && found==false)
    {
        File.read(reinterpret_cast<char *> (&ac), sizeof(account));
        if(ac.retacno()==n)
        {
            ac.show_account();
            cout<<"\n\nEnter The New Details of account"<<endl;
            ac.modify();
            int pos=(-1)*static_cast<int>(sizeof(account));
            File.seekp(pos,ios::cur);
            File.write(reinterpret_cast<char *> (&ac), sizeof(account));
            cout<<"\n\n\t Record Updated";
            found=true;
          }
    }
    File.close();
    if(found==false)
        cout<<"\n\n Record Not Found ";
}
void delete_account(int n)
{
    account ac;
    ifstream inFile;
    ofstream outFile;
    inFile.open("account.dat",ios::binary);
    if(!inFile)
    {
        cout<<"File could not be open !! Press any Key...";
        return;
    }
    outFile.open("Temp.dat",ios::binary);
    inFile.seekg(0,ios::beg);
    while(inFile.read(reinterpret_cast<char *> (&ac), sizeof(account)))
    {
        if(ac.retacno()!=n)
        {
            outFile.write(reinterpret_cast<char *> (&ac), sizeof(account));
        }
    }
    inFile.close();
    outFile.close();
    remove("account.dat");
    rename("Temp.dat","account.dat");
    cout<<"\n\n\tRecord Deleted ..";
}
void display_all()
{
    account ac;
    ifstream inFile;
    inFile.open("account.dat",ios::binary);
    if(!inFile)
    {
        cout<<"File could not be open !! Press any Key...";
        return;
    }
    cout<<"\n\n\t\tACCOUNT HOLDER LIST\n";
    cout<<"\t\t-------------------\n";
    cout<<"\n\n\n\nA/c no.         NAME            Type Balance\n";
    cout<<"-------        ------           ---- -------\n";
    while(inFile.read(reinterpret_cast<char *> (&ac), sizeof(account)))
    {
        ac.report();
    }
    inFile.close();
}
void deposit_withdraw(int n, int option)
{
    int amt;
    bool found=false;
    account ac;
    fstream File;
    File.open("account.dat", ios::binary|ios::in|ios::out);
    if(!File)
    {
        cout<<"File could not be open !! Press any Key...";
        return;
    }
    while(!File.eof() && found==false)
    {
        File.read(reinterpret_cast<char *> (&ac), sizeof(account));
        if(ac.retacno()==n)
        {
            ac.show_account();
            if(option==1)
            {
                cout<<"\n\n\tTO DEPOSITE AMOUNT ";
                cout<<"n\n\Enter The amount to be deposited";
                cin>>amt;
                ac.dep(amt);
            }
            if(option==2)
            {
                cout<<"\n\n\tTO WITHDRAW AMOUNT ";
                cout<<"\n\nEnter The amount to be withdraw";
                cin>>amt;
                int bal=ac.retdeposit()-amt;
                if((bal<500 && ac.rettype()=='S') || (bal<1000 && ac.rettype()=='C'))
                    cout<<"Insufficience balance";
                else
                    ac.draw(amt);
            }
            int pos=(-1)*static_cast<int>(sizeof(ac));
            File.seekp(pos,ios::cur);
            File.write(reinterpret_cast<char *> (&ac), sizeof(account));
            cout<<"\n\n\t Record Updated";
            found=true;
           }
         }
    File.close();
    if(found==false)
        cout<<"\n\n Record Not Found ";
}
void intro()
{
    cout<<"\n\n\n\t\t\t\t\t\t\t\t\t\t\t\tBANK MANAGEMENT SYSTEM";
    cout<<"\n\n\n\n\t\t\t\t\t\t\t\t\t\t\t\tMADE BY : Shivam Choudhary";
    cin.get();
}

Output:-



                                     BANK MANAGEMENT SYSTEM

                                     MADE BY : Shivam Choudhary


                                     MAIN MENU

                                    01. NEW ACCOUNT
                                    02. DEPOSIT AMOUNT
                                    03. WITHDRAW AMOUNT
                                    04. BALANCE ENQUIRY
                                    05. ALL ACCOUNT HOLDER LIST
                                    06. CLOSE AN ACCOUNT
                                    07. MODIFY AN ACCOUNT
                                    08. EXIT
                                    Select Your Option (1-8) 1

For New Account:- press 1
Enter The account No. :3210
Enter The Name of The account Holder : Suraj Sen
Enter Type of The account (C/S) : S
Enter The Initial amount(>=500 for Saving and >=1000 for current ) : 4000

Account Created..


For Balance Enqury:- press 4



                                     MAIN MENU

                                    01. NEW ACCOUNT
                                    02. DEPOSIT AMOUNT
                                    03. WITHDRAW AMOUNT
                                    04. BALANCE ENQUIRY
                                    05. ALL ACCOUNT HOLDER LIST
                                    06. CLOSE AN ACCOUNT
                                    07. MODIFY AN ACCOUNT
                                    08. EXIT
                                    Select Your Option (1-8) 4



        Enter The account No. : 3210

BALANCE DETAILS
Account No. : 3210
Account Holder Name : Suraj Sen
Type of Account : S
Balance amount : 4000



For  ALL ACCOUNT HOLDER LIST:- Press 5


                                     MAIN MENU

                                    01. NEW ACCOUNT
                                    02. DEPOSIT AMOUNT
                                    03. WITHDRAW AMOUNT
                                    04. BALANCE ENQUIRY
                                    05. ALL ACCOUNT HOLDER LIST
                                    06. CLOSE AN ACCOUNT
                                    07. MODIFY AN ACCOUNT
                                    08. EXIT
                                    Select Your Option (1-8) 5



       ACCOUNT HOLDER LIST
      -----------------------------------
A/c no.         NAME            Type Balance
-------            --------            ------ ----------
3765          Sonai Sen          S 25000
4956          Rupam Sen        S 15000
6792          Chiku Sen          S 8000
4567          Raj Kumar         S 20000
3210          Suraj Sen           S  4000



For DEPOSIT AMOUNT:- Press 2


                                     MAIN MENU

                                    01. NEW ACCOUNT
                                    02. DEPOSIT AMOUNT
                                    03. WITHDRAW AMOUNT
                                    04. BALANCE ENQUIRY
                                    05. ALL ACCOUNT HOLDER LIST
                                    06. CLOSE AN ACCOUNT
                                    07. MODIFY AN ACCOUNT
                                    08. EXIT
                                    Select Your Option (1-8) 2



        Enter The account No. : 6792

Account No. : 6792
Account Holder Name : Chiku Sen
Type of Account : S
Balance amount : 8000
TO DEPOSITE AMOUNT 
Enter the Amount:-2000
Record Updated



                ACCOUNT HOLDER LIST
                ----------------------------------
A/c no.         NAME            Type Balance
-------           ---------             ----  -----------
3765          Sonai Sen          S 25000
4956          Rupam Sen        S 15000
6792          Chiku Sen          S 10000
4567          Raj Kumar         S 20000
3210          Suraj Sen           S 4000


For WITHDRAW AMOUNT:- Press 3



                                     MAIN MENU

                                    01. NEW ACCOUNT
                                    02. DEPOSIT AMOUNT
                                    03. WITHDRAW AMOUNT
                                    04. BALANCE ENQUIRY
                                    05. ALL ACCOUNT HOLDER LIST
                                    06. CLOSE AN ACCOUNT
                                    07. MODIFY AN ACCOUNT
                                    08. EXIT
                                    Select Your Option (1-8) 3


        Enter The account No. : 4956

Account No. : 4956
Account Holder Name : Rupam Sen
Type of Account : S
Balance amount : 15000
TO WITHDRAW AMOUNT
Enter The amount to be withdraw 5000
Record Updated


                ACCOUNT HOLDER LIST
                ----------------------------------
A/c no.         NAME            Type Balance
-------           ---------             ------ ----------
3765          Sonai Sen          S 25000
4956          Rupam Sen        S 10000
6792          Chiku Sen          S 10000
4567          Raj Kumar         S 20000
3210          Suraj Sen           S 4000


For CLOSE AN ACCOUNT:- Press 6



                                     MAIN MENU

                                    01. NEW ACCOUNT
                                    02. DEPOSIT AMOUNT
                                    03. WITHDRAW AMOUNT
                                    04. BALANCE ENQUIRY
                                    05. ALL ACCOUNT HOLDER LIST
                                    06. CLOSE AN ACCOUNT
                                    07. MODIFY AN ACCOUNT
                                    08. EXIT
                                    Select Your Option (1-8) 6



        Enter The account No. : 3210
        Record Deleted ..


          ACCOUNT HOLDER LIST
         -----------------------------------
A/c no.         NAME            Type Balance
-------           ---------            ------ -----------
3765          Sonai Sen          S 25000
4956          Rupam Sen        S 10000
6792          Chiku Sen          S 10000
4567          Raj Kumar         S 20000




For MODIFY AN ACCOUNT:- Press 7




                                          MAIN MENU
                                    01. NEW ACCOUNT
                                    02. DEPOSIT AMOUNT
                                    03. WITHDRAW AMOUNT
                                    04. BALANCE ENQUIRY
                                    05. ALL ACCOUNT HOLDER LIST
                                    06. CLOSE AN ACCOUNT
                                    07. MODIFY AN ACCOUNT
                                    08. EXIT
                                    Select Your Option (1-8) 7

 Enter The account No. : 6792
Account No. : 6792
Account Holder Name : Chiku Sen
Type of Account : S
Balance amount : 10000
Enter The New Details of account
Account No. : 6792
Modify Account Holder Name : Joy  Sen
Modify Type of Account : 2976
Modify Balance amount :
         Record Updated


                ACCOUNT HOLDER LIST
                -----------------------------------


A/c no.         NAME            Type Balance
-------           ----------           ------  ----------
3765          Sonai Sen          S 25000
4956          Rupam Sen        S 10000
6792          Joy  Sen             S  2976
4567          Raj Kumar         S  20000

For EXIT:-Press 8



                                          MAIN MENU
                                    01. NEW ACCOUNT
                                    02. DEPOSIT AMOUNT
                                    03. WITHDRAW AMOUNT
                                    04. BALANCE ENQUIRY
                                    05. ALL ACCOUNT HOLDER LIST
                                    06. CLOSE AN ACCOUNT
                                    07. MODIFY AN ACCOUNT
                                    08. EXIT
                                    Select Your Option (1-8) 8




        Thanks for using bank managemnt system
-----------------------------------------------------------
Process exited after 1283 seconds with return value 0
Press any key to continue . . .



































