# 简单的通讯录address_list————版本一

### 题目

实现一个通讯录；
通讯录可以用来存储1000个人的信息，每个人的信息包括：
姓名、性别、年龄、电话、住址
提供方法：

1. 添加联系人信息
2. 删除指定联系人信息
3. 查找指定联系人信息
4. 修改指定联系人信息
5. 显示所有联系人信息
6. 清空所有联系人
7. 以名字排序所有联系人
8. 保存联系人到文件
9. 加载联系人



**<u>address_list.h</u>**  

```c
#define _CRT_SECURE_NO_DEPRECATE 1

#ifndef __address_list_H_
#define __address_list_H_
#include<stdio.h>
#include<string.h>
typedef struct address_list
{
	char name[10];
	char sex[6];
	char age[3];
	char tel[12];
	char address[30];
} address_list;
#define SZ 1000
void add(address_list* addr, int sz);
void del(address_list* addr, char *p);
int find(address_list* addr, char* p);
void modify(address_list* addr, char* p);
void display(address_list* addr);
void empty(address_list*  addr);
void sort(address_list*  addr);
void sever(address_list *addr);

#endif

```





**<u>test.c</u>**

```c
#define _CRT_SECURE_NO_DEPRECATE 1
//
//2.实现一个通讯录；
//通讯录可以用来存储1000个人的信息，每个人的信息包括：
//姓名、性别、年龄、电话、住址
//提供方法：
//1. 添加联系人信息
//2. 删除指定联系人信息
//3. 查找指定联系人信息
//4. 修改指定联系人信息
//5. 显示所有联系人信息
//6. 清空所有联系人
//7. 以名字排序所有联系人
//8. 保存联系人到文件
//9. 加载联系人


#include"address_list.h"


void menu()
{
	printf("****************************************\n");
	printf("*********** 1.add      2.del  **********\n");
	printf("*********** 3.find     4.modify **********\n");
	printf("*********** 5.display  6.empty **********\n");
	printf("*********** 7.sort     8.sever **********\n");
	printf("*********** 9.loading  0.exit **********\n");
}

int main()
{
	address_list addr[SZ] = { {0},{0},0,{0},{0} };
	int input = 0, n = 0;
	do //用do--while循环，至少执行一次
	{
		menu();
		printf("please select\n");
		scanf("%d", &input);
		if (input < 0 && input>9)
		{
			printf("enter error Please enter again\n");

		}
		else
		{
			switch (input)
			{
			case 1:
			{
				printf("请输入要录入的人数\n");
				scanf("%d", &n);
				add(addr, n);
				break;
			}
			case 2:
			{
				char p[6] = { 0 };
				printf("请输入要删除人的名字\n");
				scanf("%s", p);
				del(addr,p);
				break;
			}
			case 3:
			{
				char p[6] = { 0 };
				printf("请输入要查找联系人的名字\n");
				scanf("%s", p);
				if (find(addr, p) >= 0)
				{
					printf("%d\n", find(addr, p));
				}
				else
				{
					printf("没有该联系人的信息\n");
				}
				break;
			}
			case 4:
			{
				char p[6] = { 0 };
				printf("请输入要修改联系人的名字\n");
				scanf("%s", p);
				modify(addr, p);
				printf("修改联系人的的位置\n");
				printf("%d\n", find(addr, p));
				break;

			}
			case 5:
			{
				display(addr);
				break;
			}
			case 6:
			{
				empty(addr);
				break;
			}
			case 7:
			{
				sort(addr);
				break;
			}
			case 8:
			{
				sever(addr);
				break;
			}
			}
		}
		}while (input);

		return 0;
	}
```

address_list.c

```
#include"address_list.h"
static int flag = 0;
void add(address_list* addr, int sz)//添加联系人信息
{
	int i = 0;
	int a = 0;
	if (flag < SZ)
	{
		for (i = flag; i < sz; i++)
		{
			printf("name:\n");
			scanf("%s", (addr + i)->name);
			printf("sex:\n");
			scanf("%s", (addr + i)->sex);
			printf("age:\n");
			scanf("%s", (addr + i)->age);
			printf("tel:\n");
			scanf("%s", (addr + i)->tel);
			printf("address:\n");
			scanf("%s", (addr + i)->address);
			flag++;
		}
	}
	else
		printf("通讯录已满\n");

}
void del(address_list* addr,char *p)// 首先查找联系人，然后删除；
{
	if (flag == 0)
	{
		printf("当前通信录为空\n");
	}
	else
	{
		
		int i = find(addr, p);
		memset(addr + i, '*', sizeof(address_list));
		memmove(addr + i, addr + i + 1, sizeof(address_list)*(SZ - i - 1));
		flag--;
	}
}
int find(address_list* addr, char* str)//查找联系人，  长度一样， 每个字符一样
{
	char* p = str;
	int i = 0,j=0;
	for (i = 0; i < flag; i++)
	{
		if (strcmp((addr + i)->name, p) == 0)
			return i;
		else
			return -1;
	}

}

void modify(address_list* addr, char* p)// 修改指定联系人信息
{
	int i = find(addr, p);
	printf("modify name:\n");
	scanf("%s", (addr + i)->name);
	printf("modify sex:\n");
	scanf("%s", (addr + i)->sex);
	printf("modify age:\n");
	scanf("%s", (addr + i)->age);
	printf("modify tel:\n");
	scanf("%s", (addr + i)->tel);
	printf("modify address:\n");
	scanf("%s", (addr + i)->address);
	
}

void display(address_list* addr)
{
	int i = 0;
	printf("name	sex		age		tel		address\n");
	for (i = 0; i < flag; i++)
	{
		
		printf("name: %s\n", ((addr + i)->name));
		printf("sex: %s\n", ((addr + i)->sex));
		printf("age: %s\n", ((addr + i)->age));
		printf("tel: %s\n", ((addr + i)->tel));
		printf("address: %s\n", ((addr + i)->address));
		printf("\n");
	}

}
void empty(address_list*  addr)
{
	int i = 0;
	for (i = 0; i < flag; i++)
	{
		memset(addr + i, "0", sizeof(address_list));
	}
	flag = 0;
}
void sort(address_list*  addr)
{
	int i = 0; int n = 0;
	for (i = 0; i < flag-1; i++)
	{
		int j = 0;
		for (j = 0; j < flag - 1 - i; j++)
		{
			address_list tmp;
			if (strcmp((addr + j)->name, (addr + j + 1)->name))
			{
				memcpy(&tmp,addr+j,sizeof(address_list));
				memcpy(addr + j,addr+j+1,sizeof(address_list));
				memcpy( addr + j+1, &tmp,sizeof(address_list));
				n = 1;
			}
			if (n == 0)
				break;
		}
	}
}
void sever(address_list *addr)
{

	FILE *pf = fopen("address_list.txt","w+");
	int i = 0;
	if (pf != NULL)
	{
		for (i = 0; i < flag; i++)
		{
			fputs(addr+i, pf);
			fclose(pf);
		}
	}
}
```

