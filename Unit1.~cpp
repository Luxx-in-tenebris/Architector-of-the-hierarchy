//---------------------------------------------------------------------------

#include <vcl.h>
#pragma hdrstop
#include <vector>
#include "Unit1.h"
//---------------------------------------------------------------------------
#pragma package(smart_init)
#pragma resource "*.dfm"
TForm1 *Form1;
class main_struct{    //струтура информации по человеку и его задачам
public:
  String adr;//адрес
  String tel;//телефон
  String zp;//зарплата
  String zd;//задачи
  int n;   //номер

  main_struct(String s2, String s3, String s4,String z,int i){
    adr=s2;
    tel=s3;
    zp=s4;
    zd=z;
    n = i;
  };
};

std::vector <main_struct> lst; //список информации
int n_click; //номер выделенного элемента
AnsiString Str_children; //строка с именами дочерних элементов

//---------------------------------------------------------------------------
//функция подсчета количества всех дочерних элементов у TTreeNode* TN
int Count_children(TTreeNode* TN)
   {
   //функция count подсчитывает количество дочерних узлов, но только на одном уровне
   //чтобы охватить все уровни используется рекурсия
   //т.е. мы рекурсивно пробегаемся по каждому элементу и считаем его предков
    int count=TN->Count;
    for(int i=0; i<TN->Count; i++)
    {
      Str_children+=", " +TN->Item[i]->Text;
      count+=Count_children(TN->Item[i]);
    }

    return count;
   }
//---------------------------------------------------------------------------
__fastcall TForm1::TForm1(TComponent* Owner)
        : TForm(Owner)
{
}
//---------------------------------------------------------------------------
//обработчик кнопки "Новый элемент"
void __fastcall TForm1::Button1Click(TObject *Sender)
{
String s = InputBox("Введите название нового элемента ","","Имя");
//создаем новый элемент в качестве дочернего по отношению к текущему
if (s!="")
   TreeView1->Items->AddChild(TreeView1->Selected,s);
else
   ShowMessage("Название элемента не может быть пустым");

   TreeView1->FullExpand();//раскрыть дерево
}
//---------------------------------------------------------------------------
//обработчик кнопки "В файл"
void __fastcall TForm1::Button5Click(TObject *Sender)
{
TStringList *ts = new TStringList();
TreeView1->SaveToFile("data.txt");
for (int i=0;i<lst.size();i++)               {
                           ts->Add(lst[i].adr);
                           ts->Add(lst[i].tel);
                           ts->Add(lst[i].zp);
                           ts->Add(lst[i].zd);
                           ts->Add(lst[i].n);
                           }
ts->SaveToFile("data2.txt");
TreeView1->FullExpand();  //раскрыть дерево
}
//---------------------------------------------------------------------------
//обработчик кнопки "Из файла"
void __fastcall TForm1::Button6Click(TObject *Sender)
{
if(FileExists("data.txt"))
   TreeView1->LoadFromFile("data.txt");  //открыть файл структуры
else
   ShowMessage("Файл data.txt не был найден");

if(FileExists("data2.txt"))
{
  TStringList *ts = new TStringList();
  ts->LoadFromFile("data2.txt"); //открыть файл
  for(int i=0;i<ts->Count/5;i=i+5)
  { //по строкам создать запись и прочитать ее в список
     lst.push_back(main_struct(ts->Strings[i],ts->Strings[i+1],ts->Strings[i+2],ts->Strings[i+3],ts->Strings[i+4].ToInt()));
  }
}
else
   ShowMessage("Файл data2.txt не был найден");
//открыть дерево
TreeView1->FullExpand();
}
//---------------------------------------------------------------------------
//обработчик кнопки "Удалить" 
void __fastcall TForm1::Button3Click(TObject *Sender)
{
if(TreeView1->Selected==TreeView1->TopItem)
{
   ShowMessage("Нельзя удалить предприятие");
}
else
  if(TreeView1->Selected->HasChildren==false)
  {
    TreeView1->Selected->Delete();
  }
  else
  {
      Str_children="";
      //вызываем функцию подсчета количества предков
      int count= Count_children(TreeView1->Selected);
      //переводим число в строку
      AnsiString str_count =""+IntToStr(count);

      if(count==1)
      {
         ShowMessage("Нельзя удалить узел, т.к. имеется 1 дочерний элемент"
         +Str_children);
      }
      else
      {
         //выводим сообщение, о том что есть дочерние элементы, также выводим их число и имена через запятую
        ShowMessage("Нельзя удалить узел, т.к. имеется "+str_count+" дочерних объектов"
        +Str_children);
      }


  }
TreeView1->FullExpand();
}
//---------------------------------------------------------------------------
// обработчик кнопки "Правка" 
void __fastcall TForm1::Button4Click(TObject *Sender)
{
String s = InputBox("Введите название организации","",TreeView1->Selected->Text);
if (s!="")
   TreeView1->Selected->Text=s;
else
   ShowMessage("Название предприятия не может быть пустым");

}
//---------------------------------------------------------------------------
//обработчик кнопки "Записать данные" 
void __fastcall TForm1::Button2Click(TObject *Sender)
{
//создать новый элемент списка
lst.push_back(main_struct(Edit2->Text,Edit3->Text,Edit4->Text,Memo1->Text,TreeView1->Selected->AbsoluteIndex));
}

//---------------------------------------------------------------------------
//обработчик события выбора элемента дерева на форме
void __fastcall TForm1::TreeView1Click(TObject *Sender)
{
//найти связанную с элементом информацию
Edit2->Text = "";
Edit3->Text = "";
Edit4->Text = "";
Memo1->Clear();
int i = TreeView1->Selected->AbsoluteIndex;   //поиск по абсолютному индексу
n_click = i;//запомнили номер выделенного элемента
for (int j=0;j<lst.size();j++)
 if (lst[j].n==i)
 {
    Edit2->Text = lst[j].adr;
      Edit3->Text = lst[j].tel;
        Edit4->Text = lst[j].zp;
        Memo1->Text=lst[j].zd;
 }


}


//---------------------------------------------------------------------------
// обработчик события перетаскивания элемента
void __fastcall TForm1::TreeView1DragOver(TObject *Sender, TObject *Source,
      int X, int Y, TDragState State, bool &Accept)
{
//запрещаем двигать самый верхний элемент
if(TreeView1->Selected!=TreeView1->TopItem )
{
   if(TreeView1->GetNodeAt(X,Y)!=0)
   {
       if(TreeView1->GetNodeAt(X,Y)==TreeView1->TopItem)
            TreeView1->TopItem=TreeView1->TopItem->GetPrev();
       else
            if (TreeView1->GetNodeAt(X,Y)->GetNext()!=0)
                TreeView1->GetNodeAt(X,Y)->GetNext()->MakeVisible();
   }
}
}
//---------------------------------------------------------------------------

//обработчик события конца перетаскивания элемента 
void __fastcall TForm1::TreeView1EndDrag(TObject *Sender, TObject *Target,
      int X, int Y)
{
//провеяем является ли элемент на который хотим переместить наш верхушкой дерева
if(TreeView1->GetNodeAt(X,Y)==TreeView1->TopItem)
{
   ShowMessage("Нельзя поставить элемент выше предприятия");
}
else
{
  TreeView1->Selected->MoveTo(TreeView1->GetNodeAt(X,Y),naInsert);
  //меняем номер элемента в списке в соответствии с перемещеием
  for (int j=0;j<lst.size();j++)
  if (lst[j].n==n_click)
  {
     //ShowMessage(TreeView1->Selected->AbsoluteIndex);
     lst[j].n=TreeView1->Selected->AbsoluteIndex;
  }
}
}
//---------------------------------------------------------------------------

