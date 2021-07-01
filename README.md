- 👋 Hi, I’m @webliert
- 👀 I’m interested in study
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
webliert/webliert is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
#include<stdio.h>
#include<conio.h>
#include<stdlib.h>
int score;
int px,py;
int i,j;
	int map[10][13]=
	{
		{2,2,2,2,2,2,2,2,2,2,2,2,2},
		{2,0,0,1,0,0,0,0,0,1,0,0,2},
		{2,0,1,0,1,0,0,0,1,0,1,0,2},
		{2,1,0,0,0,1,1,1,0,0,0,1,2},
		{2,1,0,4,0,0,3,0,0,6,0,1,2},
		{2,0,1,0,0,0,4,0,0,0,1,0,2},
		{2,0,0,1,0,0,3,0,0,1,0,0,2},
		{2,0,0,0,1,0,0,0,1,0,0,0,2},
		{2,0,0,0,0,1,0,1,0,0,0,0,2},
		{2,2,2,2,2,2,1,2,2,2,2,2,2},
	};//定义地图
void printfgametip()//打印游戏说明
{
	printf("*************************************************************************\n");
	printf("\n\t符号说明：墙：▇ 箱子：卍 空地：□ 人物：♀ 目的地：囍\n");
	printf("\n\t游戏说明：利用你的智慧控制人物将所有箱子推到目的地即过关，如果游戏陷入僵局，按q以退出\n");
	printf("\n\t按键说明：w(↑),s(↓),a(←),d(→)\n");
	printf("*************************************************************************\n");
}
void huaditu()//画地图函数（绘制游戏地图）
{
	for(i=0;i<10;i++)
	{
		printf("\t\t\t");//将游戏画面定格为中间，打印每一行时都打印三个退格符
		for(j=0;j<13;j++)
		{
			switch(map[i][j])
			{
			case 0:
				printf("□");//数字0为空地
				break;
			case 1:
				printf("▇");//数字1为墙壁
				break;
			case 2:
				printf("○");//数字2为游戏边框的空白部分
				break;
			case 3:
				printf("囍");//数字3为目的地
				break;
			case 4:
				printf("卍");//数字4代表箱子
				break;
			case 6:
				printf("♀");//数字6代表人
				break;
			case 7:
				printf("¤");//数字7代表箱子进入目的地
				break;
			case 9:
				printf("♀");//数字9代表人进入目的地
				break;
			}
		}
		printf("\n");
	}
}
void zaoweizhi()//找到人当前的位置
{
	for(i=0;i<10;i++)
	{
		for(j=0;j<13;j++)
		{
			if(map[i][j]==6 || map[i][j]==9)
			{
				px = i;
				py = j;
			}
		}
	}
}
void renyidong(int x,int y)//接受键盘指令，控制人的移动
{
	if(map[px+x][py+y]==0)//1////////当要移动的位置为空地时
	{
		map[px+x][py+y]=6+0;//空地直接值加即可
		if(map[px][py]==9)
			map[px][py]=3;//如果人在目的地时，则将目的地复原为3
		else
			map[px][py]=0;//本来在空地则将原来位置复原为0
	}
	else if(map[px+x][py+y]==1  || map[px+x][py+y]==2)
		printf("不可移动！");
	else if(map[px+x][py+y]==4)//2////////当要移动的位置为箱子时
	{
		if(map[px+2*x][py+2*y]==0)//如果箱子前面时是空地
		{
			map[px+2*x][py+2*y]=4;//箱子前进一步
			if(map[px][py]==7)//进一步判断箱子原来的位置,如过之前在目的地则为7
				map[px][py]=9;//人占空位后应该变为9
			else
				map[px+x][py+y]=6;//如果之前为空地则变为6，因为人占位
			if(map[px][py]==9)//判断人之前的位置
				map[px][py]=3;
			else
				map[px][py]=0;
		}
		else if(map[px+2*x][py+2*y]==3)//如果箱子前面是目的地
		{
			map[px+2*x][py+2*y]=7;//箱子进入目的地
			map[px+x][py+y]=6;
			map[px][py]=0;
			score++;
		}
		else if(map[px+2*x][py+2*y]==1 || map[px+2*x][py+2*y]==2)//箱子前面是墙或者边界
		{
			printf("不能动了！！！！！");
		}
	}
	else if(map[px+x][py+y]==7)//3////////当要移动的位置为已经在目的地的箱子时
	{
		if(map[px+2*x][py+2*y]==0)//箱子前面是空地
		{
			score--;
			map[px+2*x][py+2*y]=4;//箱子到了空地上变为4
			map[px+x][py+y]=9;//人站在了原来的目的地上变为9
			if(map[px][py]==9)//对人的位置进行判断
				map[px][py]=3;
			else
				map[px][py]=0;
		}
		else if(map[px+2*x][py+2*y]==3)//箱子前面是另一个目的地
		{
			map[px+2*x][py+2*y]=7;
			map[px+x][py+y]=9;//人站在了原来的目的地上变为9
			if(map[px][py]==9)//对人的位置进行判断
				map[px][py]=3;
			else
				map[px][py]=0;
		}
	}
	else if(map[px+x][py+y]==3)//4////////当要移动的位置为目的地时
	{
		map[px+x][py+y]=9;//目的地加人为9
		if(map[px][py]==9)
			map[px][py]=3;//如果人在目的地时，则将目的地复原为3
		else
			map[px][py]=0;//本来在空地则将原来位置复原为0
	}
}
int play()//玩游戏函数
{
	char input=' ';//接受用户移动指令变量input，定义初始值空格
	while(1)
	{
		system("cls");//通过一次次重新打印来体现游戏的变化
		printfgametip();
		huaditu();//绘制地图
		zaoweizhi();//寻找人物位置函数
		input=getch();//getch函数：从控制台读取一个字符，但不显示在屏幕上 
		switch(input)
		{
		case 'w':
			renyidong(-1,0);//输入w，坐标变换，实际上是数组行列的变换，向上行减一，列不变，所以返回-1，0
			break;
		case 's':
			renyidong(1,0);//输入s，坐标变换，实际上是数组行列的变换，向下行加一，列不变，所以返回1，0
			break;
		case 'a':
			renyidong(0,-1);//输入a，坐标变换，实际上是数组行列的变换，向左行不变，列减一，所以返回0，-1
			break;
		case 'd':
			renyidong(0,1);//输入d，坐标变换，实际上是数组行列的变换，向右行不变，列加一，所以返回0，1
			break;
		}
		if(input=='q')
		{
			char a;
			printf("就输了，这么菜？再按y退出");
			a=getchar();
			if(a=='y')
			{
			break;
			}
		}
		if(jieshuhanshu())
	{
		break;
	}
	}
}
int jieshuhanshu()//结束函数，判断是否完成任务，完成返回1，否则返回0
{
	if(score==2)
		return 1;
	else
		return 0;
}
int main()
{
	int a=0;
	printf("你即将进入推箱子小游戏，进入密码20101\n");
	scanf("%d",&a);
	if(a==20101)
	{
		play();
	}
	else 
		printf("进入失败！！！");
	if(jieshuhanshu())
		printf("\t\t\t竟然让你赢了！！！！！");
	return 0;
}
