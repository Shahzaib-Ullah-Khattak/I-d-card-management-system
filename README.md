

// author Shahzaib khattak
# I-d-card-management-system
It automatically update the I'd card of the students for the upcoming semester upon completion. Also store the students data in data base that the admin acces through CMS and semester wise



#include<cstdlib>    	// random no geration
#include<iomanip> 		// for seting lines wedth
#include <string>		// for string class
#include<ctime>			// change the random no at each run
using namespace std;

struct date{			// for dates like regestration, valiodity and current date
    int day;
    int month;
    int year;
};

struct dataBase{		// main students data base
    string name;
    string cms;
    string email;
    string department;
    char section;
    int semester;
    string address;
    date regDate;
    date validityDate;
    string barcode;
    bool isUpdated ;
};

dataBase students[51]={				// struct arry 
    { "Muhammad Safdar",	"023-25-0169",	"safdar@gmail.com",		"CS Specilization in AI",'C',1,"Nashpa, Karak",	{11,8,2025},{11,2,2026},"KPK123",false},
    { "Muhammad Naeem",		"023-25-0168",	"naeem@gmail.com",		"CS Specilization in AI",'C',1,"wah cant",		{11,8,2025},{11,2,2026},"KPK124",false},
    { "Durkhanai",			"023-25-0167",	"durkhanai@gmail.com",	"CS Specilization in AI",'C',1,"waziristan",	{11,8,2025},{11,2,2026},"KPK125",false},
    { "Gul Khan",			"023-25-0166",	"khan@gmail.com",		"CS Specilization in AI",'C',1,"waziristan",	{11,8,2025},{11,2,2026},"KPK126",false},
    { "Gul Lakhtha",		"023-25-0180",	"gul@gmail.com",		"CS Specilization in AI",'C',1,"Thor Ghar",		{11,8,2025},{11,2,2026},"KPK127",false},
    { "Gulalai",			"023-25-0182",	"gulalai@gmail.com",	"CS Specilization in AI",'C',1,"Swat",			{11,8,2025},{11,2,2026},"KPK128",false},
    { "Sharjeel Ahmad",		"023-25-0184",	"sharjeel@gmail.com",	"CS Specilization in AI",'C',1,"boot bazar",	{11,8,2025},{11,2,2026},"KPK129",false},
    { "Thorpakhai",			"023-25-0185",	"pakhai@gmail.com",		"CS Specilization in AI",'C',1,"Chitral",		{11,8,2025},{11,2,2026},"KPK130",false},
    { "Shahzaib Khattak",	"023-25-0187",	"shahzaib@gmail.com",	"CS Specilization in AI",'C',1,"BDS, Karak",	{11,8,2025},{11,2,2026},"KPK131",false},
    { "Yasir Ali",			"023-25-0188",	"yasir@gmail.com",		"CS Specilization in AI",'C',1,"Rahim yar khan",{11,8,2025},{11,2,2026},"KPK132",false},
    { "Zohaib Ahmad",		"023-25-0191",	"zohaib@gmail.com",		"CS Specilization in AI",'C',1,"Tunsa Shareef",	{11,8,2025},{11,2,2026},"KPK133",false}
};


struct accounts{				// new acount created structs
    string name;
    string email;
    string password;
};

accounts detail[51];		// accounts created accounts array
int  totalAccounts =0;

accounts reserved[100];		// if the account array is full the new accounts will be stored in reserved array
struct admin{
    string name;
    string email;
    string password;
    string secretID;
};

admin secretId[2]={						// admins that can access only admin portal pre defined no other creation accounts
    {"Abdullah Hakro","abdullah@gmail.com","ab1","KP12KP"},
    {"Ma'am Nimra Mughal","nimra@gmail.com","ab5","KP13KP"}
};

int sizeAccount=51;


/* ===========================
   SMALL HELPER FUNCTIONS
   1.  readAccount
   2.  printStudentDetails (small)
   3.  loginAccount
   4.  findEmptyStudentIndex
   5.  registerStudent (small)
   6. findStudentByBarcode
   7. generateAndVerifyOTP (used by update)
   8. updateID print
   9. updateing ID Process (small)
   10 createStudentAccount (small for case 2)
   11. adminVerify (small)
   12   month differnce
  
   =========================== */
   
   // barcode generation
barcodeGeneration(int idx){

srand(time(0));

		int barcode = rand() % 1000;     // random number 0–999
		
		stringstream ss;
		ss << barcode;                   // convert int to string
		string bar = ss.str();          // result string
		
		students[idx].barcode = "KPK" + bar;   // final CMS code
}


cmsGeneration(int idx){

srand(time(0));

		int lCmsNum = rand() % 500;     // random number 0–500
		
		stringstream ss;
		ss << lCmsNum;                   // convert int to string
		string lCms = ss.str();          // result string
		
		students[idx].cms = "023-25-0" + lCms;   // final CMS code
}


void readAccount(){				// fun 1  read already created accounts from file
	    ifstream read("Create Account.txt");
    if (!read) {
        cout << "Sorry, file not found." << endl;
        return;
    }

    int r = 0;
    while (r < 51 && getline(read, detail[r].name)) {
		getline(read, detail[r].email);
        getline(read, detail[r].password);
        r++;
    }

    totalAccounts = r;
    read.close();
}


/* Function 2:
   printStudentDetails - prints details of students[i] */
void printStudentDetails(ostream &out,int i){
    out<<"   " << left << setw(25) <<"Name: "<< left << setw(15)<<students[i].name<<endl;
    out<<"   " << left << setw(25) <<"CMS ID: "<< left << setw(15)<<students[i].cms<<endl;
    out<<"   " << left << setw(25) <<"Email: "<< left << setw(15)<<students[i].email<<endl;
    out<<"   " << left << setw(25) <<"Department: "<< left << setw(15)<<students[i].department<<endl;
    out<<"   " << left << setw(25) <<"Section: "<< left << setw(15)<<students[i].section<<endl;
    out<<"   " << left << setw(25) <<"Semester: "<< left << setw(15)<<students[i].semester<<endl;
    out<<"   " << left << setw(25) <<"Address: "<< left << setw(15)<<students[i].address<<endl;
    out<<"   " << left << setw(25) <<"Reg. Date: "<<students[i].regDate.day<<"-"<<students[i].regDate.month<<"-"<<students[i].regDate.year<<endl;
    out<<"   " << left << setw(25) <<"Valid upto: "<<students[i].validityDate.day<<"-"<<students[i].validityDate.month<<"-"<<students[i].validityDate.year<<endl;
    out<<"   " << left << setw(25) <<"Barcode: "<< left << setw(15)<<students[i].barcode<<endl;
    out<<"------------------------------------------------"<<endl;
}
/* Function 3:
   loginAccount - checks detail[] for matching email & password
   returns 1 if found, 0 otherwise  */
int login(string email, string passward, int sizeAccount){
    for(int i=0; i<sizeAccount; i++){
        if(email==detail[i].email  && passward==detail[i].password){
            return 1 ;
        }
    }
    return 0;
}

/* Function 4:
   findEmptyStudentIndex - returns first index in students[] where name == ""
   returns -1 if none */
int findEmptyStudentIndex(){
    for(int i=0;i<51;i++){
        if(students[i].name==""){
            return i;
        }
    }
    return -1;
}

/* Function 5:
   registerStudent - fills a new students[idx] with user input 
   If no empty slot, prints "Database is full!" */
void registerStudent(){
  cout<<"-------------------------------------"<<endl;
  cout<<"|         Register Yourself         |"<<endl;
  cout<<"-------------------------------------"<<endl<<endl; 

  
    int idx = findEmptyStudentIndex();
    if (idx == -1) {
        cout << "Database is full!" << endl;
        return;
    }

    // INPUT VIA CONSOLE
    cout << "\tEnter your name : ";
    getline(cin, students[idx].name);

    cout << "\tEnter your email account : ";
    getline(cin, students[idx].email);
// Cms geration all function
    cmsGeneration(idx);

    cout << "\tEnter your Department: ";
    getline(cin, students[idx].department);

    cout << "\tEnter your section: ";
    cin >> students[idx].section;
    cin.ignore();

    cout << "\tEnter your semester number : ";
    cin >> students[idx].semester;
    cin.ignore();

    cout << "\tEnter your Permanent Address: ";
    getline(cin, students[idx].address);

    cout << "\tEnter Registration date (dd mm yyyy) : ";
    cin >> students[idx].regDate.day
        >> students[idx].regDate.month
        >> students[idx].regDate.year;
    cin.ignore();

    cout << "\tEnter Validity Date (dd mm yyyy) : ";
    cin >> students[idx].validityDate.day
        >> students[idx].validityDate.month
        >> students[idx].validityDate.year;
    cin.ignore();

// bar code generation
   		barcodeGeneration(idx);


//    getline(cin, students[idx].barcode);
    cout<<"\n\n================================================="<<endl;
	cout<<"\tRegesteration Details"<<endl;
    // OUTPUT GOES TO "out"
    ofstream write("Register Student.txt", ios::app);
    printStudentDetails(write,idx);
    printStudentDetails(cout,idx);
    write.close();
    cout<<"\tYou are successfully regestered "<<endl;
	cout<<"================================================\n"<<endl;
}
/* Function 6:
   findStudentByBarcode - returns index of student with matching barcode, -1 if not found */
int findStudentByBarcode(string barcode){
    for(int j=0;j<51;j++){
        if(students[j].barcode==barcode){
            return j;
        }
    }
    return -1;
}



/* Function 7:
   verifyOTP - small helper  */
int verifyOTP(int otp, int vCode){
    if( otp==vCode ){
	return 1;
	}
	return -1;
	
	
}


// fun 8 print updated id card to cansole and file
void printIDCard(ostream &out,int i) {
			out<<endl<<endl;
  			out << "\n\t\t+---------------------------------------+\n";
			out << "\t\t|           STUDENT ID CARD (Front)     |\n";
			out << "\t\t+---------------------------------------+\n";
			out << "\t\t|        SUKKUR IBA UNIVERSITY        	|\n";
			out << "\t\t|     _______                           |\n";
			out << "\t\t|    |       |                          |\n";
			out << "\t\t|    | photo |                          |\n";
			out << "\t\t|    |_______|                          |\n";
			out << "\t\t| Name     : " << setw(25) << left << students[i].name  << "  |\n";
			out << "\t\t| CMS ID   : " << setw(25) << left << students[i].cms   << "  |\n";
			out << "\t\t| Email    : " << setw(25) << left << students[i].email << "  |\n";
			out << "\t\t+---------------------------------------+\n\n";
			
			// BACK SIDE OF ID CARD
			out << "\t\t+---------------------------------------+\n";
			out << "\t\t|           STUDENT ID CARD (Back)      |\n";
			out << "\t\t+---------------------------------------+\n";
			out << "\t\t| Department : " << setw(21) << left << students[i].department << "   |\n";
			out << "\t\t| Section    : " << setw(21) << left << students[i].section 	<< "    |\n";
			out << "\t\t| Semester   : " << setw(21) << left << students[i].semester	<< "    |\n";
			out << "\t\t| Address    : " << setw(21) << left << students[i].address 	<< "    |\n";
			out << "\t\t| Reg. Date  : " 
			     << students[i].regDate.day << "-" 
			     << students[i].regDate.month << "-" 
			     << students[i].regDate.year 
			     << setw(8) << " " << "        |\n";
			out << "\t\t| Valid Upto : " 
			     << students[i].validityDate.day << "-" 
			     << students[i].validityDate.month << "-" 
			     << students[i].validityDate.year 
			     << setw(8) << " " << "        |\n";
			out << "\t\t| Barcode    : " << setw(21) << left << students[i].barcode << "    |\n";
			out << "\t\t+---------------------------------------+\n";
}




//Function 9:
//   updateIDProcess - handles OTP generation & updating student's semester and validity date 
void updateIDProcess(int i){
    srand(time(0));
    int vCode = rand()%10000;
    cout<<"\tYour Verification code for Updating ID Card is "<<vCode<<endl;

    while(true){
        int otp;
        cout<<"\tEnter Your Verification code: ";
        cin>>otp;

        if(verifyOTP(otp,vCode)==1){
            students[i].isUpdated=true;
            students[i].semester++;

            students[i].validityDate.month+=6;
            if(students[i].validityDate.month>12){
                students[i].validityDate.month-=12;
                students[i].validityDate.year++;
            }

            cout<<"\tValidity date has been extended by 6 months."<<endl;
            cout<<"\tValidity Date: "<<students[i].validityDate.day<<"-"<<students[i].validityDate.month<<"-"<<students[i].validityDate.year<<endl;

          // FRONT SIDE OF ID CARD
	

    ofstream card("ID Card.txt");
		printIDCard(card,i);
		printIDCard(cout,i);
		
	card.close();
            // consume newline left by numeric input (if any) before returning to higher menus
            cin.ignore();
            break;
        }
        else{
            cout<<"\tYou entered wrong otp try again"<<endl;
        }
    }
}

/* Function 10:
   createStudentAccount - handles case 2 (account creation)  */
void createStudentAccount() {

    // Open file for writing account details
    ofstream accounts("Accounts Details.txt"); 
    //  (does not overwrite old data)

    cout << "\n=== Create Your New Account ===\n";

    string name, gmail, password, cPassword;
 
    // ---- USER INPUT ----
    cout << "Name: ";
   
    getline(cin, name);
    cout << "Email: ";
    getline(cin, gmail);

    // Password + Confirm Password
    do {
        cout << "Password: ";
        getline(cin, password);

        cout << "Confirm Password: ";
        getline(cin, cPassword);

        if (password != cPassword) {
            cout << "\tPasswords do not match! Try again.\n";
        }

    } while (password != cPassword);


    // ---- STORE IN accounts array----
    bool created = false;

    for (int i = 0; i < 51; i++) {
        if (detail[i].email == "" && detail[i].name == "" && detail[i].password == "") {

            detail[i].name = name;
            detail[i].email = gmail;
            detail[i].password = password;
            created = true;
            break;
        }
    }

    // ---- IF MAIN ARRAY FULL, USE reserved[] ----
    if (!created) {
        for (int m = 0; m < (sizeof(reserved) / sizeof(reserved[0])); m++) {

            if (reserved[m].email == "") {

                reserved[m].name = name;
                reserved[m].email = gmail;
                reserved[m].password = password;
                created = true;
                break;
            }
        }
    }

    // ---- DISPLAY RESULT ----
    if (created) {
        cout << "\nYour Account is Successfully Created.\n";

        // Write to file
        accounts << "\tName: " << name << "\n";
        accounts << "\tEmail: " << gmail << "\n";
        accounts << "\tPassword: " << password << "\n";
        accounts << "-------------------------------------\n";

    } else {
        cout << "\tUnable to create account (database full).\n";
    }

    accounts.close();
}

/* Function 11:
   adminVerify - checks admin credentials  */
bool adminVerify(string email,  string password){
    for(int i=0;i<2;i++){
        if(email==secretId[i].email && password==secretId[i].password){
            return true;
        }
    }
    return false;
}

// fun 12  monthDiff kept as a small helper  
int monthDiff(int day, int month, int year, date reg){
    int monthDifference= (year-reg.year)*12+(month-reg.month);
    return monthDifference;
}

/* ===========================
   LARGE FUNCTIONS (Portals)
   
   (A) studentPortalLarge
   (B) adminPortalLarge
   (C) mainMenu (calls A and B)
   =========================== */

/* Large Function A:
   studentPortalLarge - called after successful student login (case 1) in main function
   Calls small functions: registerStudent(), findStudentByBarcode(), printStudentDetails(), updateIDProcess()
*/
void studentPortalLarge(){
    bool studentMenu=true;

    while(studentMenu){
        int select;
        cout<<"\nEnter "<<endl
            <<" \t 1. Regesteration "<<endl
            <<" \t 2. Updated ID Card Printing Portal"<<endl
            <<" \t 3. Log out"<<endl;

        cout<<"Select Your Opertion to perform: ";
        cin>>select;
        cin.ignore();

        switch(select){
            case 1:{
                // registration (small function call)
                
                registerStudent();  // call function 5 
                 
                 
                break;
            }

            case 2:{
           		cout<<"----------------------------------------"<<endl;
                cout<<"| \t Student ID card Printing Portal |"<<endl;
              	cout<<"----------------------------------------"<<endl<<endl; 
              	
                cout<<" \t\t Insert your card "<<endl<<endl;
                cout<<" \t\t     Inserting...."<<endl<<endl;
                cout<<" \t\tScanning for details......"<<endl<<endl;

                string barcode;
	                cout<<"Enter your bar code: ";
	                cin>>barcode;
	                cin.ignore();

                int i = findStudentByBarcode(barcode);       // call function 6

                if(i==-1){
                    cout<<"Sorry Your bar code does not match"<<endl;
                    break;
                }
				else{
                // function 2
                printStudentDetails(cout,i);// diplay the match students details
				}
                int day,month,year;
	                cout<<"\tEnter your current date: ";
	                cin>>day>>month>>year;
	                cin.ignore();
//call function 12 inside if 
                if(monthDiff(day,month,year,students[i].regDate)>=6){
                    cout<<"\nYour Semester is Completed !\n"<<endl;
                    cout<<"\nDo you want to update your card and promote to next semester"<<endl;

                    int update;
                    cout<<"Press \n\t1. Yes\n\t2. No"<<endl;
                    cout<<"Enter your choice for updateing:";
                    cin>>update;
                    cin.ignore();

                    switch(update){
                        case 1:{
                            // call function 9 
                            updateIDProcess(i);
                            break;
                        }

                        case 2:
                            cout<<"\tOk You do not want to update your card "<<endl;
                            break;

                        default:
                            cout<<"\tInvalid choice"<<endl;
                            break;
                    }
                }
                else{
                    cout<<"\tSorry Your semester is not completed yet"<<endl;
                }

                break;
            }

            case 3:{
            	cout<<"--------------------------------------------\n";
                cout<<" Your are successfully Logout Your account"<<endl<<endl;
                studentMenu=false; // exit student menu
                break;
            }

            default:
                cout<<"\tInvalid choice"<<endl;
        }
    }
}
//function file handling 1
void printStudentLine(ostream &out, string name, string cms) {
    out << "   "
        << left << setw(25) << name
        << left << setw(15) << cms
        << endl;
}
// function file handling 2
void printStudentPromoted(ostream &out, string name, int sem) {
    out << "   "
        << left << setw(25) << name
        << left << setw(15) << sem
        << endl;
}


/* Large Function B:
   adminPortalLarge - handles admin sign-in and admin menu operations
   Calls small functions: promotion listing & semester-wise listing */
   
void adminPortalLarge(){
    	cout<<"----------------------------------------"<<endl;
        cout<<"|              Admin Access            |"<<endl;
        cout<<"----------------------------------------"<<endl<<endl; 

	string email, password;
	
		cout<<"Enter your email: ";
		getline(cin, email);
		cout<<"Enter your password: ";
		getline(cin, password);
	
	bool matched = false;
	
		for(int i=0; i<2; i++){
	    	if(email == secretId[i].email && password == secretId[i].password){
	       			matched = true;
					int choose;
	       		while(choose!=4) {  // LOOP START
	
		            cout<<"Enter "<<endl
		                <<"\t 1. Promotion portal"<<endl
		                <<"\t 2. Semester wise Students Details "<<endl
		                <<"\t 3. Search student by CMS ID "<<endl
		                <<"\t 4. logout"<<endl;
	                
	
		            cout<<"Enter your Choice: ";
		            cin >> choose;
		            cin.ignore();
		
	            	switch(choose){
	
		                case 1:{
		                    	cout << "----------------------------------------" << endl;
								cout << "|             Promotion Portal         |" << endl;
								cout << "----------------------------------------" << endl << endl;
								
								bool found = false;
								
								cout << "   " << left << setw(25) << "Name"
						         	 << left << setw(15) << "Semester"
						        	 << endl;
								for (int j = 0; j < 51; j++) {
								    if (students[j].isUpdated == true) {
								        cout <<"   " << left << setw(25) << students[j].name << left << setw(15) << students[j].semester << endl;
								        found = true;  // mark as found
								    }
								}
								cout<<"----------------------------------------------"<<endl;
								if (!found) {
								    cout << "\tSorry, no student has updated their card yet." << endl;
								}
								
								break;
								}
		
		                case 2:{
		                
						
						    int sem;
						    cout << "Enter semester: ";
						    cin >> sem;
						    cin.ignore();
						
						    ofstream file("Semester wise Students.txt");
							   
							
							    file << "----------------------------------------" << endl;
							    file << "|      Semester wise Students Details  |" << endl;
							    file << "----------------------------------------" << endl << endl;
								
								
								
								file << "Semester :"<<sem<<endl;
								
							    file << "   " << left << setw(25) << "Name"
							         << left << setw(15) << "CMS ID"
							         << endl;
								
							    
						    bool found = false;
						
						    // Print header to console 
						    cout << "----------------------------------------" << endl;
							cout << "|      Semester wise Students Details  |" << endl;
							cout << "----------------------------------------" << endl << endl;
						    cout << "Semester :"<<sem<<endl;
							cout << "   " << left << setw(25) << "Name"
							     << left << setw(15) << "CMS ID"
							     << endl;
						    // Loop through all students
						    for (int i = 0; i < 51; i++) {
						        if (students[i].semester == sem) {
						            found = true;
						// call function file handling 1
						            // Print details to console
						            printStudentLine(cout, students[i].name, students[i].cms);
						
						            // Print details to file
						            printStudentLine(file, students[i].name, students[i].cms);
						        }
						    }
						
						    // If no student found
						    if (!found) {
						        cout << "\tSorry, no student found in this semester\n";
						        file << "\tSorry, no student found in this semester\n";
						    }
						
						    cout << "\n----------------------------------------\n";
						    file << "\n----------------------------------------\n";
						
						    file.close();
						    break;
						}

			            
		                case 3:{
		                    string cms;
		                    int match = -1;
		
		                    	cout<<"----------------------------------------"<<endl;
                				cout<<"|      Searching Students By CMS ID    |"<<endl;
              					cout<<"----------------------------------------"<<endl<<endl; 
		                    do {
		                        cout<<"\tEnter CMS ID: ";
		                        cin >> cms;
		
		                        for(int x=0; x < 51; x++){
		                            if( students[x].cms==cms){
		                            	int y=x;
		                                printStudentDetails(cout,y);  // call function file handling 2 passing index of match cms student 
		                                match = x;
		                                break;
		                            }
		                        }
		
		                        if(match == -1)
		                            cout<<"\tSorry, CMS ID not found! Try again.\n";
		
		                    } while(match == -1);
		
		                    break;
		                }
		
		                default:
		                    cout<<"\t Your Successfully Log out your email"<<endl;
		            }
		
		        } // end while loop
		
		        break;
		    }
		}
		
		if(!matched)
		    cout<<"\tInvalid admin login"<<endl;
		
}

// end admin access function

int main(){

    cout<<"\n\t\t******************************************************************"<<endl;
    cout<<"\t\t|                     ID CARD PRINT OUT SYSTEM                   |"<<endl;
    cout<<"\t\t|                         Infinite Loopers                       |"<<endl;
    cout<<"\t\t******************************************************************"<<endl;
	
	readAccount();													// reads accounts deatalis and stored in the details array
   
    int chooseSignUp;

    while(chooseSignUp!=4){
        cout<<"\nEnter"<<endl;
        cout<<" \t 1. Student Sign in / Login "<<endl
            <<" \t 2. Create Student Account"<<endl
            <<" \t 3. Admin Sign in / Login"<<endl
            <<" \t 4. exit "<<endl;

        cout<<"Press The Respective Key to Proceed: ";
        cin>>chooseSignUp;
        cin.ignore();

        switch(chooseSignUp){

            case 1:{
                cout<<"---------------------------------------------"<<endl;
                cout<<"|      Sign in or Log in to your account    |"<<endl;
             	cout<<"---------------------------------------------"<<endl<<endl; 
                string email, password;

                	cout<<"\nEnter Email or Username: ";
                	getline(cin,email);
                	cout<<"Enter Password: ";
                	getline(cin,password);
                    cout<<"\n-------------------------------------------\n";
                if(login(email,password,51)==1){
                    cout<<"\tYour successfully login into the system "<<endl;
                    // call big student portal function ( function A) which uses small functions internally 
                    studentPortalLarge();
                } 
				else{
                    cout<<"\tYour password or User name is incorrect "<<endl;
                }

                	break;
            }

            case 2:{
                createStudentAccount();				//call small function to create account  function 10
                break;
            }

            case 3:{
                adminPortalLarge();					// call big admin portal fuction B
                break;
            }

            default:
                cout<<"\tyou are exit the program "<<endl;
        }
    }

    return 0;
}

