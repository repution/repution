#include <conio.h>
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

#define TOTAL 100

void DispMenu();
int load(struct Book *head[]);
void addBook(struct Book *head[], int * total);
void displayBooks(struct Book * head[], int total);
void delBook(struct Book * head[], int * total);
void saveBooks(struct Book *head[], int total);
void searchBook(struct Book *head[], int total);


struct Book
{
   char no[10];    /*书的编号*/
   char * pub;     /*出版社*/
   char * name;    /*书名*/
   int  price;     /*价格*/
};


/*******************************************************/
/* 函数功能：显示主菜单，选择菜单 */
/* 输入参数：无 */
/* 函数输出：选择的菜单序号 */
/*******************************************************/
int menu_select()
{
   int cn=0;
   DispMenu();
   for(; ;) 
   {
      cn=getch()-'0';
      if (cn>=0&&cn<=5) return cn;
   }
}

/****************************************************************************/
/* 函数功能：主程序，显示主菜单，根据用户的选择调用相应的函数完成各自的功能 */
/* 输入参数：无                                                             */
/* 函数输出：选择的菜单序号                                                 */
/****************************************************************************/
int main()
{
   struct Book * Books[TOTAL];      /* Books数组用于保存所有的图书。*/
                                    /* 注意：Books数组中的每个元素是一个指针，指向一个Book结构体变量，*/
                                    /* 该变量才真正存放每本书的信息。*/
   int count=load(Books);           /* count变量指示Books数组中已经保存了多少本书*/

   for(; ;)
   {
	  system("cls");
      switch(menu_select())
      {
         case 1:
         addBook(Books,&count);
         break;
         case 2:
         displayBooks(Books,count);
         break;
         case 3:
         searchBook(Books,count);
         break;
         case 4:
         delBook(Books,&count);
         break;
         case 5:
         saveBooks(Books,count);
         break;
         case 0:
            printf("谢谢使用！\n");
            return 0;
      }
   }
} 

//*****************************************************************************
//* 函数功能：添加一本新书
//* 输入参数：head：main函数中Books数组的首地址（亦即main函数中Books数组名的值）
//*           total：Books数组中已存书的数量                                                
//* 函数输出：无                                                 
//* 函数设计：1、先判断Books数组中是否还有空余空间，否，输出提示信息并退出，否则，total指向的变量值+1，
//*              然后继续执行以下步骤；
//*           2、用malloc函数创建一个Book变量用于保存需要添加的新书信息，并将该变量的地址（即malloc的返回值，
//*              用malloc创建的变量没有变量名，只有该变量的首地址）保存到Books数组中；
//*           3、从键盘上输入添加的新书信息并保存到新创建的Book变量中 
//*****************************************************************************
void addBook(struct Book *head[], int * total)
{
	if(*total>=100)
	{
		printf("图书管理系统已满,无法添加新书!\n");
		return;
	}
	struct Book *newBook=(struct Book*)malloc(sizeof(struct Book));
	if(newBook==NULL)
	{
		printf("为新书分配内存失败!\n");
		return;
	}
	printf("请输入书的编号:");
	scanf("%s",newBook->no);
	printf("请输入书的出版社:");
	char pub[100];
	scanf("%s",pub);
	newBook->pub=(char *)malloc(strlen(pub)+1);
	strcpy(newBook->pub,pub);
	printf("请输入书名:");
	char name[100];
	scanf("%s",name);
	newBook->name=(char *)malloc(strlen(name)+1);
	strcpy(newBook->name,name);
	printf("请输入书的价格:");
	scanf("%d",&newBook->price);
	head[*total]=newBook;
	(*total)++;
	printf("已成功添加该图书!\n");
	printf("按任意键继续...");
	getch();
}

//*****************************************************************************
//* 函数功能：在屏幕上显示所有图书的信息
//* 输入参数：head：main函数中Books数组的首地址（亦即main函数中Books数组名的值）
//*           total：Books数组中已存书的数量                                                
//* 函数输出：无                                                 
//************************************************************************/
void displayBooks(struct Book *head[], int total)
{
	if(total==0)
	{
		printf("图书管理系统为空!\n");
		getch();
		return;
	}
	printf("所有图书的信息如下:\n");
	for(int i=0;i<total;i++)
	{
		printf("书的编号:%s\n",head[i]->no);
        printf("出版社:%s\n",head[i]->pub);
        printf("书名:%s\n",head[i]->name);
        printf("价格:%d\n",head[i]->price);
        printf("\n");
	}
	printf("按任意键继续...");
	getch();
}

//*****************************************************************************
//* 函数功能：给定图书名称，查找并显示该书的相关信息
//* 输入参数：head：main函数中Books数组的首地址（亦即main函数中Books数组名的值）
//*           total：Books数组中已存书的数量                                                
//* 函数输出：无                                                 */
//************************************************************************/
void searchBook(struct Book *head[], int total)
{
	if(total==0)
	{
		printf("图书管理系统为空!\n");
		getch();
		return;
	}
	printf("请输入想要查询的书名:");
	char search_name[100];
	scanf("%s",search_name);
	int found=0;
	for(int i=0;i<total;i++)
	{
		if(strcmp(head[i]->name,search_name)==0)
		{
			printf("查询结果如下:\n");
			printf("书的编号:%s\n",head[i]->no);
            printf("出版社:%s\n",head[i]->pub);
            printf("书名:%s\n",head[i]->name);
            printf("价格:%d\n",head[i]->price);
            printf("\n");
            found=1;
		}
	}
	if(found!=1)
	{
		printf("未找到匹配的图书信息!\n");
	}
	printf("按任意键继续...");
	getch();
}

//*****************************************************************************
//* 函数功能：给定图书名称，删除该本图书的信息
//* 输入参数：head：main函数中Books数组的首地址（亦即main函数中Books数组名的值）
//*           total：Books数组中已存书的数量                                                
//* 函数输出：无                                                 
//* 函数设计：1、从键盘上输入需要删除的图书名称；
//*           2、依次查找Books数组中每个元素所指向的Book变量，检查其中保存的图书名称是否需要删除的图书
//*              名称相同，是，执行以下步骤；
//*           3、用malloc函数创建一个Book变量用于保存需要添加的新书信息，并将该变量的地址（即malloc的返回值，
//*              用malloc创建的变量没有变量名，只有该变量的首地址）保存到Books数组中；
//*           3、从键盘上输入添加的新书信息并保存到新创建的Book变量中 
//*****************************************************************************
void delBook(struct Book *head[], int * total)
{
	if(*total==0)
	{
		printf("图书管理系统为空!\n");
		getch();
		return;
	}
	printf("请输入想要删除的书名:");
	char del_name[100];
	scanf("%s",del_name);
	int found=0;
	for(int i=0;i<*total;i++)
	{
		if(strcmp(head[i]->name,del_name)==0)
		{
			free(head[i]);
			free(head[i]->pub);
			free(head[i]->name);
			for(int j=i;j<*total-1;j++)
			{
				head[j]=head[j+1];
			}
			(*total)--;
			found=1;
			printf("删除图书成功!\n");
			break;
		}
	}
	if(found!=1)
	{
		printf("未找到匹配的图书信息!\n");
	}
	printf("按任意键继续...");
	getch();
}

/*******************************************************/
/* 函数功能：将图书信息保存到文件中 */
/* 输入参数：链表头指针 */
/* 函数输出：无 */
/*******************************************************/
void saveBooks(struct Book *head[], int total)
{
	if(total==0)
	{
		printf("图书管理系统为空!\n");
		getch();
		return;
	}
	FILE *fp;
	fp=fopen("books.txt","wb");
	if(fp==NULL)
	{
		printf("无法打开写入的文件!\n");
		getch();
		return;
	}
	for(int i=0;i<total;i++)
	{
		fprintf(fp,"%s,%s,%s,%d\n",head[i]->no,head[i]->pub,head[i]->name,head[i]->price);
	}
	fclose(fp);
	printf("图书信息已保存到文件中!\n");
	printf("按任意键继续...");
	getch();
}

/*****************************************************************************/
/* 函数功能：从文件中读入数据，创建Books数组                                 */
/* 输入参数：head：main函数中Books数组的首地址（亦即main函数中Books数组名的值*/
/* 函数输出：Books数组的长度                                                 */
/*****************************************************************************/
int load(struct Book *head[])
{
	FILE *fp;
	fp=fopen("books.txt","r");
	if(fp==NULL)
	{
		printf("无法打开要读取的文件!\n");
		getch();
		return 0;
	}
	int count=0;
	char  line[100];
	while(fgets(line,sizeof(line),fp)!=NULL)
	{
		struct Book *newBook=(struct Book*)malloc(sizeof(struct Book));
		if(newBook==NULL)
		{
			printf("分配内存失败,无法加载图书信息!\n");
			fclose(fp);
			return count;
		}
		sscanf(line,"%s,%s,%s,%d",newBook->no,newBook->pub,newBook->name,&newBook->price);
		head[count]=newBook;
		count++;
	}
	fclose(fp);
	printf("成功加载%d本图书信息!\n",count);
	printf("按任意键继续...");
	getch();
	return count;
	return 0;
}

void DispMenu()
{
	puts("* * * * * * * * * * * * * * * * * * * * * * * *");
	puts("*　                                           *");
	puts("*　        欢迎使用图书管理系统V1.0           *");
	puts("*          1.添加新书信息                     *");
	puts("*          2.显示图书信息                     *");
	puts("*          3.查询图书信息                     *");
	puts("*          4.删除图书信息                     *");
	puts("*          5.保存图书信息                     *");
	puts("*          0.退出系统                         *");
	puts("*　                                           *");
	puts("* * * * * * * * * * * * * * * * * * * * * * * *");
}



