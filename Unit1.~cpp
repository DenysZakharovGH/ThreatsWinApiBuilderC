//---------------------------------------------------------------------------

#include <vcl.h>
#pragma hdrstop

#include "windows.h"
#include "iostream.h"
#include <ctime>
#include <iostream>
#include <string>
#include <fstream>
#include <math.h>
#include <vector>
#include "Unit1.h"
#include "Unit2.h"
#include "Unit3.h"
#include "Unit4.h"
#define Size 10
#define true false
//---------------------------------------------------------------------------
#pragma package(smart_init)
#pragma resource "*.dfm"
using namespace std;
TForm1 *Form1;

int array1[Size][Size];
int array2[Size][Size];
int array3[Size][Size];

AnsiString Rez1,Rez2,Rez3,Ind;

int i,j =0;
int a,b,c,d;

CRITICAL_SECTION g_cs;
CRITICAL_SECTION g_cs2;

//---------------------------------------------------------------------------

DWORD WINAPI ThreadFunction1(LPVOID){
        InitializeCriticalSection(&g_cs);
        TDateTime Time1 = Time();

        if (Form1->CheckBox4->Checked)
                Sleep(a*1000);

        Form1->Memo4->Lines->Add("1 Process Work");
        Rez1 ="";
        std::fstream in1("Input.txt"); // окрываем файл для чтения
        
        if (in1.is_open())
        {
       EnterCriticalSection(&g_cs);
        for(i=0; i<Size; i++){
                for(j=0; j<Size; j++){
                        in1 >> array1[i][j];
                        Rez1=Rez1 + "  " +array1[i][j];

                }
              Rez1=Rez1+"\r\n";
        }
       LeaveCriticalSection(&g_cs);

        in1.close();
        }
        Form1->Memo1->Clear();
        Form1->Memo1->Text =Rez1;
        Form1->Edit4->Text=Time() - Time1;

}

//---------------------------------------------------------------------------
DWORD WINAPI ThreadFunction2(LPVOID){
        TDateTime Time2 = Time();
        InitializeCriticalSection(&g_cs2);

        int iSum_under_Diagonal;

        if (Form1->CheckBox4->Checked)
                Sleep(b*1000);

        Form1->Memo4->Lines->Add("2 Process Work");
        const int N = 20;
        int arr[N][Size][Size];
                for (int i = 0; i < N; i++) {
                        for (int j = 0; j < Size; j++) {
                                for (int k = 0; k < Size; k++) {
                                        arr[i][j][k] = rand()%10;

                }
                        }
                                }


    int Max_value = 0 ;

    for (int i = 0; i < N; i++) {
    iSum_under_Diagonal =0;
         for (int j = 0; j < Size; j++) {
              for (int k = 0; k < j; k++) {
                iSum_under_Diagonal += arr[i][j][k];
                Sleep(10);
        }
  }
  if (iSum_under_Diagonal > Max_value)
              Max_value = iSum_under_Diagonal;

}

EnterCriticalSection(&g_cs2);
    std::ofstream out;
    out.open("Output.txt");
    if (out.is_open())
    {
        out << Max_value <<endl;
    }
LeaveCriticalSection(&g_cs2);

Form1->Edit5->Text= Time() - Time2;
}


DWORD WINAPI ThreadFunction3(LPVOID){
        TDateTime Time3 = Time();

        if (Form1->CheckBox4->Checked)
                Sleep(c*1000);

        Form1->Memo4->Lines->Add("3 Process Work");
        Rez3 ="";

        EnterCriticalSection(&g_cs);
        for(i=0; i<Size; i++){
                for(j=0; j<Size; j++)
                {
                        array1[i][j] += 6;
                        Rez3 = Rez3 + "  "+array1[i][j];
                        Sleep(10);
                }
        }
              Rez3 = Rez3+"\r\n";
        LeaveCriticalSection(&g_cs);


  Form1->Memo2->Clear();
  Form1->Memo2->Text = Rez3;
  Form1->Edit6->Text=Time() - Time3;

}

//---------------------------------------------------------------------------
DWORD WINAPI ThreadFunction4(LPVOID){
        TDateTime Time4 = Time();
        if (Form1->CheckBox4->Checked)
                Sleep(d*1000);

                string line;
        Form1->Memo4->Lines->Add("4 Process Work");
        std::ifstream in1("Output.txt"); // окрываем файл для чтения
        EnterCriticalSection(&g_cs2);
        if (in1.is_open())
        {
                while (getline(in1, line))
                {
                        Sleep(10);
                        Form1->Memo3->Lines->Add(line.c_str());

                }
        }
        LeaveCriticalSection(&g_cs2);
        in1.close();
  Form1->Edit8->Text= Time() - Time4;

}
//---------------------------------------------------------------------------

__fastcall TForm1::TForm1(TComponent* Owner)
        : TForm(Owner)
{
Form1->Memo1->Clear();
}
//---------------------------------------------------------------------------


void __fastcall TForm1::N4Click(TObject *Sender)
{
        Close();
}


//---------------------------------------------------------------------------
void __fastcall TForm1::Button1Click(TObject *Sender){
         Form1->Memo1->Clear();
         Form1->Memo2->Clear();
         Form1->Memo3->Clear();
         Form1->Memo4->Clear();
         
if (Form1->CheckBox1->Checked || Form1->CheckBox2->Checked || Form1->CheckBox3->Checked){
         DWORD WINAPI ThreadFunction1(LPVOID);
         DWORD WINAPI ThreadFunction2(LPVOID);
         DWORD WINAPI ThreadFunction3(LPVOID);
         DWORD WINAPI ThreadFunction4(LPVOID);

         HANDLE hThread1;
         HANDLE hThread2;
         HANDLE hThread3;
         HANDLE hThread4;


         for(i=0;i<Size;i++){
                for(j=0;j<Size;j++){
                        array1[i][j] = 0;
                        }
                        }


        if (Form1->CheckBox1->Checked){ // если нажат ЧЕКБОКС порвоого процесса
                HANDLE hThread1,hThread2,hThread3,hThread4;
                a = StrToInt((const AnsiString)Form1->Edit1->Text);
                DWORD dwThreadID1, dwThreadParam1 = 0;

                DWORD dwPriorityClass = THREAD_PRIORITY_NORMAL;


                hThread1 = CreateThread(NULL, 0, ThreadFunction1, &dwThreadParam1, 0, &dwThreadID1);

                        // вибираем приоритет виполнения
                        /*THREAD_PRIORITY_IDLE
                        THREAD_PRIORITY_LOWEST
                        THREAD_PRIORITY_BELOW_NORMAL
                        THREAD_PRIORITY_NORMAL
                        THREAD_PRIORITY_ABOVE_NORMAL
                        THREAD_PRIORITY_HIGHEST
                        THREAD_PRIORITY_TIME_CRITICAL*/
                        switch(Form1->ComboBox1->ItemIndex)
                        {
                                case 0:
                                     dwPriorityClass = THREAD_PRIORITY_IDLE;
                                     break;
                                case 1:
                                     dwPriorityClass = THREAD_PRIORITY_LOWEST;
                                     break;
                                case 2:
                                    dwPriorityClass = THREAD_PRIORITY_BELOW_NORMAL;
                                     break;
                                case 4:
                                    dwPriorityClass = THREAD_PRIORITY_ABOVE_NORMAL;
                                     break;
                                case 5:
                                    dwPriorityClass = THREAD_PRIORITY_HIGHEST;
                                     break;
                                case 6:
                                    dwPriorityClass = THREAD_PRIORITY_TIME_CRITICAL;
                                     break;
                                default:
                                    dwPriorityClass = THREAD_PRIORITY_NORMAL; // если приоритет не менялся или в листе вибрано один что =
                                     break;  // НОрмальному тогда и дефолт чтоби не писать два нормальних приоритетов
                        }
               SetThreadPriority(hThread1,dwPriorityClass);// меняем приоритет



        }
//-------------------------------------------------------------------------------------------------------------------------------

          if (Form1->CheckBox2->Checked){

          b = StrToInt((const AnsiString)Form1->Edit2->Text);

                DWORD dwThreadID2, dwThreadParam2 = 0;
	        //HANDLE hThread2;
                DWORD dwPriorityClass = THREAD_PRIORITY_NORMAL;


                hThread2 = CreateThread(NULL, 0, ThreadFunction2, &dwThreadParam2, 0, &dwThreadID2);

                        // вибираем приоритет виполнения
                        switch(Form1->ComboBox2->ItemIndex)
                        {
                                case 0:
                                     dwPriorityClass = THREAD_PRIORITY_IDLE;
                                     break;
                                case 1:
                                     dwPriorityClass = THREAD_PRIORITY_LOWEST;
                                     break;
                                case 2:
                                    dwPriorityClass = THREAD_PRIORITY_BELOW_NORMAL;
                                     break;
                                case 4:
                                    dwPriorityClass = THREAD_PRIORITY_ABOVE_NORMAL;
                                     break;
                                case 5:
                                    dwPriorityClass = THREAD_PRIORITY_HIGHEST;
                                     break;
                                case 6:
                                    dwPriorityClass = THREAD_PRIORITY_TIME_CRITICAL;
                                     break;
                                default:
                                    dwPriorityClass = THREAD_PRIORITY_NORMAL; // если приоритет не менялся или в листе вибрано один что =
                                     break;  // НОрмальному тогда и дефолт чтоби не писать два нормальних приоритетов
                        }
               SetThreadPriority(hThread2,dwPriorityClass);// меняем приоритет


//-------------------------------------------------------------------------------------------------------------------------------

         if (Form1->CheckBox3->Checked){

                c = StrToInt((const AnsiString)Form1->Edit3->Text);

                DWORD dwThreadID3, dwThreadParam3 = 0;
	       // HANDLE hThread3;
                DWORD dwPriorityClass = THREAD_PRIORITY_NORMAL;


                hThread3 = CreateThread(NULL, 0, ThreadFunction3, &dwThreadParam3, 0, &dwThreadID3);

                        // вибираем приоритет виполнения
                        switch(Form1->ComboBox3->ItemIndex)
                        {
                                case 0:
                                     dwPriorityClass = THREAD_PRIORITY_IDLE;
                                     break;
                                case 1:
                                     dwPriorityClass = THREAD_PRIORITY_LOWEST;
                                     break;
                                case 2:
                                    dwPriorityClass = THREAD_PRIORITY_BELOW_NORMAL;
                                     break;
                                case 4:
                                    dwPriorityClass = THREAD_PRIORITY_ABOVE_NORMAL;
                                     break;
                                case 5:
                                    dwPriorityClass = THREAD_PRIORITY_HIGHEST;
                                     break;
                                case 6:
                                    dwPriorityClass = THREAD_PRIORITY_TIME_CRITICAL;
                                     break;
                                default:
                                    dwPriorityClass = THREAD_PRIORITY_NORMAL; // если приоритет не менялся или в листе вибрано один что =
                                     break;  // НОрмальному тогда и дефолт чтоби не писать два нормальних приоритетов
                        }
               SetThreadPriority(hThread3,dwPriorityClass);// меняем приоритет

        }

}
        if (Form1->CheckBox5->Checked){

                d = StrToInt((const AnsiString)Form1->Edit7->Text);

                DWORD dwThreadID4, dwThreadParam4 = 0;
	       // HANDLE hThread3;
                DWORD dwPriorityClass = THREAD_PRIORITY_NORMAL;


                hThread3 = CreateThread(NULL, 0, ThreadFunction4, &dwThreadParam4, 0, &dwThreadID4);

                        // вибираем приоритет виполнения
                        switch(Form1->ComboBox4->ItemIndex)
                        {
                                case 0:
                                     dwPriorityClass = THREAD_PRIORITY_IDLE;
                                     break;
                                case 1:
                                     dwPriorityClass = THREAD_PRIORITY_LOWEST;
                                     break;
                                case 2:
                                    dwPriorityClass = THREAD_PRIORITY_BELOW_NORMAL;
                                     break;
                                case 4:
                                    dwPriorityClass = THREAD_PRIORITY_ABOVE_NORMAL;
                                     break;
                                case 5:
                                    dwPriorityClass = THREAD_PRIORITY_HIGHEST;
                                     break;
                                case 6:
                                    dwPriorityClass = THREAD_PRIORITY_TIME_CRITICAL;
                                     break;
                                default:
                                    dwPriorityClass = THREAD_PRIORITY_NORMAL; // если приоритет не менялся или в листе вибрано один что =
                                     break;  // НОрмальному тогда и дефолт чтоби не писать два нормальних приоритетов
                        }
               SetThreadPriority(hThread4,dwPriorityClass);// меняем приоритет

        }


 


}// if (Form1->CheckBox1->Checked || Form1->CheckBox2->Checked || Form1->CheckBox3->Checked){
else{Memo1->Text = "<------Оберіть який потік виконувати";}

}
//---------------------------------------------------------------------------

void __fastcall TForm1::N1Click(TObject *Sender)
{
 Form3->Show();
}
//---------------------------------------------------------------------------

/*// считаваем 3 матрици и виводим в мемо
        string line;
        Form2->Show();
        std::ifstream in1("C:/Users/Денис/source/repos/Firstprocess/Debug/Result1.txt"); // окрываем файл для чтения
        if (in1.is_open())
        {
                while (getline(in1, line))
                {
                        Form2->Memo1->Lines->Add(line.c_str());
                }
        }
        in1.close();     // закрываем файл


        std::ifstream in2("C:/Users/Денис/source/repos/Firstprocess/Debug/Result2.txt"); // окрываем файл для чтения
        if (in2.is_open())
        {
                while (getline(in2, line))
                {
                        Form2->Memo2->Lines->Add(line.c_str());
                }
        }
        in2.close();     // закрываем файл


        Form2->Show();
        std::ifstream in3("C:/Users/Денис/source/repos/Firstprocess/Debug/Result3.txt"); // окрываем файл для чтения
        if (in3.is_open())
        {
                while (getline(in3, line))
                {
                        Form2->Memo3->Lines->Add(line.c_str());
                }
        }
        in3.close();     // закрываем файл
*/









