# bankpj_and_finding_bugs

#include "stdio.h"
#include "time.h"
#include "stdlib.h"


#define SIZE 1000

struct trans{

    char note[200];
};

struct data{

    unsigned int id;
    char name[30];
    char nrc[50];
    char email[50];
    char password[50];
    char pOrb[10];//personal or business
    char loan_s[20];//loan status  //should loan or not
    unsigned int monthly_income;
    unsigned int loan_amount;//how much you take loan
    float loan_rate;
    char acc_s[10];//account status;
    int account_level;
    unsigned long long int phNumber;
    unsigned int current_amount;
    char address[100];
   unsigned int tranAmolimitPerday;//transationAmountLimitPerDay minimize for our project 5minutes
    struct trans trc[100];

};
struct data db[SIZE];


void loadingAlldataFromFile();
void printingAlldatafromFile();
void welcome();
void useractionsector();
int charcounting(char toCount[50]);
void emailvalidation(char tovalidate[50]);
void registration();
void StrCmp(char userInputchar[50]);
void comparingarray(char first[50],char second[50]);
void nrc_validatation(char nrc_check[50]);
void myStrongPassword(char strongpassword[50]);
void phonevalidation( unsigned long long int phone_tovalidate);
void myStringcopy(char first[50],char second[50]);
void recordingAlldatatoFile();
void login_form();
void finding_phone_number(unsigned int tofindphone);
void transfer_money(int transfer,int receiver,unsigned int amount_transfer);
void trancsation_record(int transfer,int receiver,char who,unsigned int amount);
void integer_to_char(unsigned int value);
void space_counter();
void get_time();
void get_limit_amount(int user_index);
unsigned int charArray_to_unsignedint_fun(char charArray[]);
void current_data_to_transfer(int current_amount_to_transfer);
unsigned int calculate_amount_same_days(int to_calculate_index);


int globalusers=0;
int gvalid=-1;
int emailExit=-1;
int two_charArray=-1;
int nrcvalid=-1;
int strongpass=-1;
int phonevalid=-1;
int phone_found=-1;
int useraction=-1;
int space_array[30];
char int_to_char[10];
int trans_limit=0;
unsigned long long int phone_check=959423000221;
unsigned int current_day_to_transfer=0;
unsigned int current_amount_toTransfer=0;




char month[3];
char day[2];
char year[4];


struct mytime{

    char c_time[25];
};

struct mytime Ctime[1];


int main(){

 current_data_to_transfer(3000);
//integer_to_char(1000);
space_counter();
calculate_amount_same_days(0);
get_limit_amount(0);
printf("Get limit amount: %d\n",trans_limit);
loadingAlldataFromFile();
printingAlldatafromFile();

welcome();
while(useraction!=1){
    useractionsector();
}
    return 0;
}

void welcome(){

    int option=0;

    printf("\nWelcome to our bank!\n");
    printf("Press 1 to login\nPress 2 to register!\nPress 3 to exit!\n");
    scanf("%d",&option);

    if(option==1){

        login_form();

    }else if(option==2){


        registration();


    }else if(option==3){

        recordingAlldatatoFile();
        exit(1);


    }else{

        printf("This is invalid option.");
        welcome();
    }
}

void useractionsector(){

    int userOption=0;

       unsigned int phone=0;

     unsigned  int amount_to_transfer=0;



             printf("This is useraction sector!\n");

             printf("Press 1 to Transfer money!\nPress 2 to Withdraw!\nPress 3 to Update your information!\nPress 4 to Cash in!\nPress 5 to Take loan!\nPress 6 to View your history!\nPress 7 to Exit!\nEnter your option: ");

             scanf(" %d", &userOption);



    if(userOption==1){

        useraction=1;

        printf("This is transfer option!");

       phone_found=-1;
       while(phone_found==-1){

           printf("Enter receive phone number: ");

           scanf("%d",&phone);

           finding_phone_number(phone);

           if(phone_found==-1){

               printf("Your phone number doesn't belong to any name!");
           }


       }

       printf("Are you sure you want to send for %s email %s\n",db[phone_found].name,db[phone_found].email);



       while(amount_to_transfer<db[emailExit].current_amount){

           printf("Enter your amount to transfer: ");

           scanf("%u",&amount_to_transfer);


           if(db[emailExit].current_amount-1000>amount_to_transfer){

               break;
           }else{
               printf("No sufficient balance!");
               amount_to_transfer=0;
           }




       }

      two_charArray=-1;

       while(two_charArray==-1){

           char tran_pass[50];


            for(int count=0;count<3;count++){


                printf("Enter your password to proceed your trancsation: ");

                scanf(" %[^\n]",&tran_pass[0]);

                comparingarray(db[emailExit].password,tran_pass);


                if(two_charArray==-1){

                    printf("Wrong credential password!\n");



                }else{


                    transfer_money(emailExit, phone_found, amount_to_transfer);

                    useractionsector();

                    break;

                }
               if(count==2){

                   useractionsector();
               }


            }

       }


    }else if(userOption==7){
        welcome();

    }else{

        printf("Invalid option!");
    }
}

void transfer_money(int transfer,int receiver,unsigned int amount){

    printf("Loading to transfer......");

    unsigned int total_amount=calculate_amount_same_days(transfer);

    printf("Name: %s : Total amount: %u\n",db[transfer].name,total_amount);

    get_limit_amount(transfer);

    total_amount=total_amount+amount;//amount is user transfer amount

    printf("Total amount: %u\n",total_amount);

    if(total_amount>=trans_limit){

        printf("Exceeded limit amount!");

        printf("You can transfer amount: %u",total_amount-trans_limit);

        useractionsector();

    }else{

        db[transfer].current_amount=db[transfer].current_amount-amount;

        db[receiver].current_amount=db[receiver].current_amount+amount;

        printf("Transaction complete!\n");

        trancsation_record(transfer,receiver,'t',amount);

        trancsation_record(transfer,receiver,'r',amount);

        printingAlldatafromFile();


    }


}
void trancsation_record(int transfer,int receiver,char who,unsigned int amount){

    int trans_name_counter=charcounting(db[transfer].name);

    int receive_name_counter=charcounting(db[receiver].name);


  int amount_counter=  charcounting(int_to_char);



    char from[5]={'F','r','o','m','-'};

    char to[4]={'-','t','o','-'};

    if(who=='t'){

        int index_point=0;

        for(int i=0;i<5;i++){

            db[transfer].trc[space_array[transfer]-15].note[i]=from[i];

            index_point++;
        }

        for(int a=0;a<trans_name_counter;a++){

            db[transfer].trc[space_array[transfer]-15].note[index_point]=db[transfer].name[a];

            index_point++;

        }

        for(int j=0;j<4;j++){

            db[transfer].trc[space_array[transfer]-15].note[index_point]=to[j];

            index_point++;
        }
        for(int aaa=0;aaa<receive_name_counter;++aaa){

            db[transfer].trc[space_array[transfer]-15].note[index_point]=db[receiver].name[aaa];

            index_point++;
        }
        db[transfer].trc[space_array[transfer]-15].note[index_point]='$';

        index_point++;

        for(int x=0;x<amount_counter;++x){

            db[transfer].trc[space_array[transfer]-15].note[index_point]=int_to_char[x];

            index_point++;
        }

        get_time();

        for(int i=0;i<25;i++){

            db[transfer].trc[space_array[transfer]-15].note[index_point]=Ctime[0].c_time[i];

            index_point++;
        }

        space_array[transfer]+=1;

    }else{

        char receive[14]={'-','R','e','c','e','i','v','e','-','F','r','o','m','-'};

        int index_point=0;
        for(int i=0;i<receive_name_counter;i++){

            db[receiver].trc[space_array[receiver]-15].note[index_point]=db[receiver].name[i];

            index_point++;
        }
        for(int a=0;a<14;a++){

            db[receiver].trc[space_array[receiver]-15].note[index_point]=receive[a];

            index_point++;
        }
        for(int aaa=0;aaa<trans_name_counter;++aaa){

            db[receiver].trc[space_array[receiver]-15].note[index_point]=db[transfer].name[aaa];

            index_point++;
        }

        db[receiver].trc[space_array[receiver]-15].note[index_point]='$';

        index_point++;

        for(int j=0;j<amount_counter;++j){

            db[receiver].trc[space_array[receiver]-15].note[index_point]=int_to_char[j];

            index_point++;
        }

        get_time();

        for(int i=0;i<25;i++){

            db[receiver].trc[space_array[receiver]-15].note[index_point]=Ctime[0].c_time[i];

            index_point++;
        }

        space_array[receiver]+=1;

    }


}
void loadingAlldataFromFile(){

    FILE *fptr = fopen("bankpj.txt","r");

    if(fptr==NULL){

        printf("Error at loading all data from file!");

    }else{

        for(int ncc=0;ncc<SIZE;ncc++){

            fscanf(fptr, " %u%s%s%s%s%s%s%u%u%f%s%d%u%u%s%u\n",&db[ncc].id, &db[ncc].name, &db[ncc].nrc,&db[ncc].email,&db[ncc].password,&db[ncc].pOrb,&db[ncc].loan_s,&db[ncc].monthly_income,&db[ncc].loan_amount,&db[ncc].loan_rate,&db[ncc].acc_s,&db[ncc].account_level,&db[ncc].phNumber,&db[ncc].current_amount,&db[ncc].address,&db[ncc].tranAmolimitPerday);

            for(int gcc=0;gcc<space_array[ncc]-15;gcc++){

               fscanf(fptr,"%s",&db[ncc].trc[gcc].note[0]);
            }


            if(db[ncc].email[0]=='\0'){

                break;
            }
            globalusers++;
        }
        fclose(fptr);

    }

}

void printingAlldatafromFile(){

    for(int ncc=0;ncc<globalusers;ncc++){

        printf("  %u-%s-%s-%s-%s-%s-%s-%u-%u-%f-%s-%d-%u-%u-%s-%u", db[ncc].id,db[ncc].name,db[ncc].nrc,db[ncc].email,db[ncc].password,db[ncc].pOrb,db[ncc].loan_s,db[ncc].monthly_income,db[ncc].loan_amount,db[ncc].loan_rate,db[ncc].acc_s,db[ncc].account_level,db[ncc].phNumber,db[ncc].current_amount,db[ncc].address,db[ncc].tranAmolimitPerday);

        for(int gcc=0;gcc<space_array[ncc]-15;gcc++){

            printf("-%s",&db[ncc].trc[gcc].note[0]);
        }
        printf("\n");

    }

}

void space_counter(){

    FILE *fptr=fopen("bankpj.txt","r");

    if(fptr==NULL){

        printf("Error at space_counter!");
    }else{

    char c=  fgetc(fptr);

    int index=0;

    while(!feof(fptr)){
         if(c!='\n'){

             if(c==' '){

                 space_array[index]+=1;
             }
             c=fgetc(fptr);
         }else{

             index++;
             c=fgetc(fptr);
         }

    }
    for(int aaa=0;aaa<30;aaa++){

        printf("%d",space_array[aaa]);

        printf(" ");
    }
    printf("\n");

    }
}
void registration(){

    char reEmail[50];

    char reName[30];

    char reNrc[50];

    char rePassword[50];

    char re_Address[100];

   unsigned long long int re_Phone;

    unsigned int Re_current_amount;

    printf("This is Bank Registration!\n");

    printf("Enter your email to register: ");

    scanf(" %[^\n]",&reEmail);

    gvalid=-1;

    emailvalidation(reEmail);

    if(gvalid!=-1){

        emailExit=-1;

        StrCmp(reEmail);

        if(emailExit==-1){

            printf("Your email is valid!\n");

            printf("Enter your name: ");

            scanf(" %[^\n]",&reName[0]);

            nrcvalid=-1;

            while(nrcvalid==-1) {



                printf("Enter your NRC number: ");

                scanf(" %[^\n]",&reNrc[0]);

                nrcvalid=-1;

                nrc_validatation(reNrc);

              if(nrcvalid==-1){

                  printf("Your NRC is not valid now!");

              }


            }


               printf("Your NRC is passed in!");
            strongpass=-1;

             while(strongpass==-1){

                 printf("Enter your password!");

                 scanf(" %[^\n]",&rePassword[0]);


                 strongpass=-1;
                 myStrongPassword(rePassword);

                 if(strongpass==-1){
                     printf("Your password is not valid!");
                 }

             }

             printf("Your password is valid and saved!");


             phonevalid=-1;

             while(phonevalid==-1){

                 printf("Enter your phone number: ");

                 scanf("%llu",&re_Phone);

                 phonevalidation(re_Phone);


                  if(phonevalid==-1){

                      printf("Your phone number is not valid or is already exit!");
                  }



             }
             if(re_Phone==12 && phonevalid==1) {

                 printf("Your phone number is saved!\n");
             }

             printf("Enter your current amount: ");

             scanf(" %u",&Re_current_amount);

             printf("Enter your monthly income: ");

             scanf(" %u",&db[globalusers].monthly_income);

             printf("Enter your address: ");

             scanf(" %[^\n]",&re_Address[0]);

             printf("Enter your trc note: ");

             scanf(" %[^\n]",&db[globalusers].trc[0].note[0]);

             myStringcopy(db[globalusers].email,reEmail);

             myStringcopy(db[globalusers].name,reName);

             myStringcopy(db[globalusers].nrc,reNrc);

             myStringcopy(db[globalusers].password,rePassword);


             myStringcopy(db[globalusers].pOrb,db[2].pOrb);

             myStringcopy(db[globalusers].loan_s,db[2].loan_s);

             myStringcopy(db[globalusers].acc_s,db[2].acc_s);


             db[globalusers].loan_amount=db[2].loan_amount;

             db[globalusers].loan_rate=db[2].loan_rate;

             db[globalusers].account_level=db[2].account_level;

             db[globalusers].tranAmolimitPerday=db[2].tranAmolimitPerday;

             db[globalusers].id=globalusers+1;

             db[globalusers].phNumber=re_Phone;

            myStringcopy(db[globalusers].address,re_Address);

            db[globalusers].current_amount=Re_current_amount;

             globalusers++;

             printingAlldatafromFile();

             welcome();


        }else{

            printf("Your email was already exit!");



        }


    }else{

        printf("Your gmail is not valid!\n");
        registration();
    }


}

int charcounting(char toCount[50]){

    int charcount=0;

    for(int i=0;i<50;i++){

        if(toCount[i]=='\0'){

            break;
        }else{

            charcount++;

        }
    }
    return charcount;
}

void emailvalidation(char tovalidate[50]){

    int count=0;

  int validate=  charcounting(tovalidate);

  char form[10]={'@','g','m','a','i','l','.','c','o','m'};

  for(int i=0;i<validate;i++){

      if(tovalidate[i]==' '){

          break;
      }else if(tovalidate[i]=='@'){
          break;
      }else{

          count++;
      }



  }

  int check=0;

  for(int ncc=0;ncc<10;ncc++){

      if(tovalidate[count]!=form[ncc]){

          break;
      }else{

          check++;
          count++;
      }
  }
  if(check==10){
      gvalid=1;
  }


}

void StrCmp(char userInputchar[50]){

    int samecount=0;


    int second=charcounting(userInputchar);

    for(int j=0;j<globalusers;j++){

        int first=charcounting(db[j].email);

        if(first==second){

            for(int gcc=0;gcc<first;gcc++){

                if(db[j].email[gcc]==userInputchar[gcc]){

                    samecount++;

                }else{

                    break;
                }
            }

        }
        if(second==samecount){

            emailExit=j;

            break;
        }



    }



}

struct nrc_region{

    char nrc_regioncheck[10];
};

struct nrc_region nrcRegionchecking[3];

void nrc_validatation(char nrc_check[50]){



   int nrcCount=  charcounting(nrc_check);

   int nrc_char=0;

   for(int i=0;i<nrcCount;i++){

       if(nrc_check[i]==')'){

           break;
       }
       nrc_char++;
   }
   for(int gcc=0;gcc<3;gcc++){

       two_charArray=-1;

       comparingarray(nrc_check,db[gcc].nrc);

       if(two_charArray==1){

           nrcvalid=1;

           break;
       }
   }



}

void comparingarray(char first[50],char second[50]){

int firstCount=charcounting(first);

int secondCount=charcounting(second);

int nrc_Count=0;

if(firstCount==secondCount){

    for(int ncc=0;ncc<firstCount;ncc++){

        if(first[ncc]!=second[ncc]){

            break;
        }
        nrc_Count++;
    }

    if(nrc_Count==secondCount){

        two_charArray=1;


    }
}





}

void myStrongPassword(char strongpassword[50]){

    int i=0;

    int strongpassone=0;

    int strongpasstwo=0;

    int strongpassthree=0;

    int strongpassfour=0;

  int pass_counter=  charcounting(strongpassword);

   if(pass_counter==7){

       while(strongpass==-1){//strongpass is global


               if(i==pass_counter){

                   strongpass=-1;

                   break;
               }

               if(strongpassword[i]>=33 && strongpassword[i]<=42){

                   strongpassone++;
               }else if(strongpassword[i]>=42 && strongpassword[i]<=57){

                   strongpasstwo++;
               }else if(strongpassword[i]>=65 && strongpassword[i]<=90){

                   strongpassthree++;
               }else if(strongpassword[i]>=97 && strongpassword[i]<=122){

                   strongpassfour++;
               }
               i++;


               if(strongpassone>0 && strongpasstwo>0 && strongpassthree>0 && strongpassfour>0){

                   strongpass=1;
               }


       }





   }else{

       printf("We need at least 7 characters!");
       strongpass=-1;
   }


}

void phonevalidation(unsigned long long int phone_tovalidate){


               if (phone_tovalidate / 1000000000 == 959) {
                   phonevalid = 1;
               } else {
                   phonevalid = -1;
               }



}

void myStringcopy(char first[50],char second[50]){

  int copy_counter=  charcounting(second);

  for(int i=0;i<50;i++){

      first[i]='\0';
  }
  for(int j=0;j<copy_counter;j++){

      first[j]=second[j];
  }
}
void recordingAlldatatoFile() {

    FILE *fptr = fopen("bankpj.txt", "w");

    if (fptr == NULL) {

        printf("Error at recording all data to file!");
    } else {

        for (int ncc = 0; ncc < globalusers; ncc++) {
            fprintf(fptr, "%u%c%s%c%s%c%s%c%s%c%s%c%s%c%u%c%u%c%f%c%s%c%d%c%u%c%u%c%s%c%u", db[ncc].id, ' ',
                    db[ncc].name, ' ', db[ncc].nrc, ' ', db[ncc].email,
                    ' ', db[ncc].password, ' ', db[ncc].pOrb, ' ', db[ncc].loan_s, ' ', db[ncc].monthly_income, ' ',
                    db[ncc].loan_amount, ' ', db[ncc].loan_rate, ' ', db[ncc].acc_s, ' ', db[ncc].account_level, ' ',
                    db[ncc].phNumber, ' ', db[ncc].current_amount, ' ', db[ncc].address, ' ',
                    db[ncc].tranAmolimitPerday);

           for(int gcc=0;gcc<space_array[ncc]-15;gcc++){

                fprintf(fptr," %s",&db[ncc].trc[gcc].note[0]);
            }
            fprintf(fptr,"%c",'\n');//(to notice)not double quoute in \n


        }
    }
    printf("Recording all data to bankpj.txt is complete!");

    fclose(fptr);
}

void login_form(){
    char lEmail[50];

    char lPassword[50];

    emailExit=-1;

    two_charArray=-1;

  while(emailExit==-1 || two_charArray==-1){

      printf("Enter your email to login: ");

      scanf(" %[^\n]",&lEmail[0]);

      printf("Enter your password: ");

      scanf(" %[^\n]",&lPassword[0]);

      StrCmp(lEmail);

      comparingarray(db[emailExit].password,lPassword);


      if(emailExit==-1 || two_charArray==-1){

          emailExit=-1;

          two_charArray=-1;

          printf("Your login credential was wrong!");
      }

  }
  printf(" Welcome Mr/Mrs! %s",db[emailExit].name);

  useractionsector();




}
void finding_phone_number(unsigned int tofindphone){

    for(int i=0;i<globalusers;i++){

        if(db[i].phNumber==tofindphone){

            phone_found=i;
            break;
        }
    }
}

void integer_to_char(unsigned int value){


    FILE *fptr=fopen("value.txt","w");

    if(fptr==NULL){

        printf("Error at integer to char!");
    }else{

        fprintf(fptr,"%u",value);
    }

    fclose(fptr);

    FILE *fptr2=fopen("value.txt","r");

    fscanf(fptr2,"%s",&int_to_char[0]);


    fclose(fptr2);


}



void current_data_to_transfer(int current_amount_to_transfer){//function to get current data to transfer for transition amount limit per day

    char get_current_day[2];

    get_time();

    printf("Current info: %s\n",Ctime[0].c_time);

    get_current_day[0]=Ctime[0].c_time[9];

    get_current_day[1]=Ctime[0].c_time[10];

//get_current_day is char
 unsigned int current_day= charArray_to_unsignedint_fun(get_current_day);

 printf("\ncurrent day: %u\n current amount to tranfer: %u\n",current_day,current_amount_to_transfer);

     current_day_to_transfer=current_day;

    //current_amount_toTransfer variable is global


}


unsigned int calculate_amount_same_days( int to_calculate_index){

    int record_counter=space_array[to_calculate_index]-15;

    char amount_char_array[10];

           char day_char_array[3];

           unsigned int total_amount_for_same_day=0;

           int index_counter=0;

           for( int i=record_counter-1;i>=0;i--){//counting from last record

              int current_record_counter=charcounting(db[to_calculate_index].trc[i].note);
             //to get $
             for(int a=0;a<current_record_counter;a++) {

                 if (db[to_calculate_index].trc[i].note[a] == '$') {

                     break;

                 }
                 index_counter++;

             }

             //to get -

             int quantity_of_amount=0;
            for(int aa=index_counter;aa<current_record_counter;aa++) {

                if (db[to_calculate_index].trc[i].note[aa] =='-') {

                    break;

                }
                index_counter++;

            }



             for(int x=0;x<quantity_of_amount-1;x++) {

                 amount_char_array[x]=db[to_calculate_index].trc[i].note[index_counter];

                 index_counter++;
             }

            unsigned int current_record_amount= charArray_to_unsignedint_fun(amount_char_array);

               printf("Current record amount: %u\n",current_record_amount);




               for(int xx=index_counter;xx<current_record_counter;xx++){

                   if(db[to_calculate_index].trc[i].note[xx]=='!'){

                       break;
                   }

                   index_counter++;
               }
               day_char_array[0]=db[to_calculate_index].trc[i].note[index_counter+5];

               day_char_array[1]=db[to_calculate_index].trc[i].note[index_counter+6];

        unsigned int current_record_day= charArray_to_unsignedint_fun(day_char_array);

               printf("Current record day: %u\n",current_record_day);

           /*  if(current_record_day!=current_day_to_transfer){
                 break;
             }*/

             total_amount_for_same_day=total_amount_for_same_day+current_record_amount;


           }
           printf("Total amount for same day: %u",total_amount_for_same_day);


  }

unsigned int charArray_to_unsignedint_fun(char charArray[]){

     unsigned int data=0;
    FILE *fptr=fopen("value.txt","w");

    if(fptr==NULL){

        printf("Error at char to integer!");
    }else{

        fprintf(fptr,"%s",charArray);


    }
    fclose(fptr);

    FILE *fptr2=fopen("value.txt","r");

    fscanf(fptr2,"%u",&data);

    fclose(fptr2);

    return data;

}





/*void time_class(int user_index){

    time_class_last_record( user_index);


}

void time_class_last_record(int user_index){

    int last=0;

    int records=space_array[user_index]-15;

    for(int i=0;i<records;i++){

        last=i;

    }

    printf("User last record: %s\n",db[user_index].trc[last].note);
}

void time_class_get_date(char last_record[]){

    //to get month name,day,year

    int i=0;

   int last_record_counter= charcounting(last_record);

   for( i=0;i<last_record_counter;i++){

       if(last_record[i]=='!'){

           break;
       }
   }
   i++;

   for(int a=0;a<3;a++){

       month[a]=last_record[i];
       i++;

   }
   printf("get month: %s",month);

   day[0]=last_record[i+1];

   day[1]=last_record[i+2];

   unsigned int get_day=charArray_to_unsignedint_fun(day);

   printf("Get day: %u",get_day);
}*/


    


    void get_time() {

        time_t tm;

        time(&tm);


        FILE *fptr = fopen("time.txt", "w");

        if (fptr == NULL) {

            printf("Error at time file!");
        } else {

            fprintf(fptr, "%s", ctime(&tm));
        }

        fclose(fptr);

        int index = 0;
        Ctime[0].c_time[index] = '-';
        index++;

        FILE *fptr2 = fopen("time.txt", "r");

        char c = fgetc(fptr2);

        int time_space_counter = 0;

        while (!feof(fptr2)) {

            if (c == ' ') {

                time_space_counter++;//for finding a space

                if (time_space_counter == 1) {

                    Ctime[0].c_time[index] = '!';

                    c = fgetc(fptr2);

                    index++;

                } else if (time_space_counter == 4) {

                    Ctime[0].c_time[index] = '@';

                    c = fgetc(fptr2);

                    index++;
                } else {
                    Ctime[0].c_time[index] = '-';

                    c = fgetc(fptr2);

                    index++;
                }
            } else {

                Ctime[0].c_time[index] = c;

                c = fgetc(fptr2);

                index++;
            }
        }



    }

    void get_limit_amount(int user_index) {

        char account_level = db[user_index].account_level;

        char pOrb = db[user_index].pOrb[0];

        int p_Or_b = 0;

        if (pOrb == 'p') {


            p_Or_b = 1;
        } else {

            p_Or_b = 2;
        }

        switch (account_level) {

            case 1://high level account

                if (p_Or_b == 1) {

                    trans_limit = 100000;
                } else {

                    trans_limit = 1000000;
                }

                break;

            case 2:

                if (p_Or_b == 1) {

                    trans_limit = 50000;
                } else {

                    trans_limit = 500000;
                }
                break;

            case 3://low level account

                if (p_Or_b == 1) {

                    trans_limit = 10000;
                } else {
                    trans_limit = 100000;
                }
                break;

            default:

                break;
        }
    }








