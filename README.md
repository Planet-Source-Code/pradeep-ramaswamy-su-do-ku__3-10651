<div align="center">

## SU DO KU


</div>

### Description

It solves sudoku puzzle,and frames you new sudoku puzzles.it enables the user to fix the values so that the user can solve it in PC rather than scribbling in a paper.
 
### More Info
 
sudoku puzzle.

solution for the given puzzle.

give a puzzle such that a solution exists.


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[pradeep ramaswamy](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/pradeep-ramaswamy.md)
**Level**          |Intermediate
**User Rating**    |4.7 (28 globes from 6 users)
**Compatibility**  |C\+\+ \(general\)
**Category**       |[Math](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/math__3-12.md)
**World**          |[C / C\+\+](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/c-c.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/pradeep-ramaswamy-su-do-ku__3-10651/archive/master.zip)





### Source Code

```
#include<process.h>
#include<stdlib.h>
#include<stdio.h>
#include<dos.h>
#include<iostream.h>
#include<graphics.h>
#include<conio.h>
#include<time.h>
void place(int,int,int);
void back(int,int,int);
void print();
union REGS i,o;
void initmp();
void resmp();
void find();
void check();
void create();
void solve();
void disp();
void fix();
void getmp(int *,int *,int *);
void timfix();
void tim();
void hiscore();
int a[10][10],over=0,save[10][30],v[35],e,f,g,m,wrong=0,cop[10][10],v1[35],fixed=0,tm=0,ts=0,min,sec;
char *c,*ch;
char *t_m,*t_s;
void main()
{
clrscr();
int gd=DETECT,gm;
initgraph(&gd,&gm,"c:\\tc\\bgi");
find();
getch();
cleardevice();
closegraph();
}
void place(int n,int i,int j)
 {
 for(int x=1;x<=9 ;x++)
 {
 if(a[i][x]==100)
  a[i][x]=0;
 }
 for(x=1;x<=9;x++)
 {
 if(a[x][j]==100)
 a[x][j]=0;
 }
int k,l;
k=j/3;
if(j%3==0)
 k--;
k=k*3+1;
l=i/3;
if(i%3==0)
 l--;
l=l*3+1;
int m,o;
m=l+3;
o=k+3;
n=n;
for(i=l;i<m;i++)
 for(j=k;j<o;j++)
  if(a[i][j]==100)
   a[i][j]=0;
}
void back(int n,int i, int j)
 {
 int x,y,s;
 for(int k=1;k<=9;k++)
  {
  if(a[i-1][k]==n)
  {
  x=i-1;
  y=k;
  break;
  }
  }
 for(i=1;i<=9;i++)
 {
 for(j=1;j<=9;j++)
 {
 if(a[i][j]!=0 || (i==x)&&(j==y))
 continue;
 a[i][j]=100;
 }
 a[x][y]=-1;
 }
 for(i=1;i<=9;i++)
 {
 if(i<=x)
  continue;
 for(j=1;j<=9;j++)
  {
  if(a[i][j]==-1)
  a[i][j]=100;
  }
 }
 for(i=1;i<=9;i++)
 for(j=1;j<=9;j++)
  if(a[i][j]==n)
  {
  place(n,i,j);
  break;
  }
 }
 /************************** PRINT************************************/
void print()
{
int x1,y1,x2,y2,x,r;
  fflush(stdin);
 ostream& flush();
 settextstyle(0,0,0);
 setcolor(8);
 for(int i=0;i<=8;i++)
 {
 for(int j=0;j<=8;j++)
  {
  if(a[i+1][j+1]==100)
  continue;
  setfillstyle(1,BLACK);
  *ch=' ';
  x1=182,y1=162,x2=198,y2=178;
  for(r=1;v[r]!=0;r++)
  {
  m=v[r];
  g=m%10;
  m/=10;
  f=m%10;
  m/=10;
  e=m;
  if(i+1==f && j+1==g)
   {
   setfillstyle(1,8);
   setcolor(BLACK);
   }
  }
  x1=x1+j*20;
  y1=y1+i*20;
  x2=x2+j*20;
  y2=y2+i*20;
  *ch=a[i+1][j+1]+48;
  bar(x1,y1,x2,y2);
  outtextxy(x1+5,y1+5,ch);
  for(r=1;v[r]!=0;r++)
  {
  m=v[r];
  g=m%10;
  m/=10;
  f=m%10;
  m/=10;
  e=m;
  if(i+1==f && j+1==g)
   {
   setcolor(8);
   setfillstyle(1,BLACK);
   }
  }
  }
 }
}
void initmp()
 {
 i.x.ax=0;
 int86(0x33,&i,&o);
 resmp();
 i.x.ax=1;
 int86(0x33,&i,&o);
 }
void resmp()
 {
 int xx=getmaxx(),xy=getmaxy();
 i.x.ax=7;
 i.x.cx=400;
 i.x.dx=xx-11;
 int86(0x33,&i,&o);
 i.x.ax=8;
 i.x.cx=65;
 i.x.dx=xy-60;
 int86(0x33,&i,&o);
 }
void getmp(int *but,int *x,int *y)
 {
 i.x.ax=3;
 int86(0x33,&i,&o);
 *but=o.x.bx;
 *x=o.x.cx;
 *y=o.x.dx;
 }
/********************************* FIND *********************************/
void find()
 {
 int x,y,n,p,q,but,option;
 for(int i=1;i<=9;i++)
 for(int j=1;j<=9;j++)
 {
 a[i][j]=100;
 save[i][j]=0;
 }
for(i=0;i<=35;i++)
 v[i]=0;
 int xx=getmaxx(),xy=getmaxy();
 rectangle(1,65,xx-1,xy-1-50);
  setcolor(8);
 settextstyle(4,1,7);
 outtextxy(5,80,"SU DO KU");
  setcolor(8);
 settextstyle(1,0,1);
 for(int k=0;k<=3 ;k++)
 {
 if(k==2)
  continue;
 rectangle(470,180+k*35,570,200+k*35);
 }
 outtextxy(497,178,"New");
 outtextxy(495,213,"Solve");
/* outtextxy(480,248,"Hi Scores");*/
 outtextxy(497,283,"Quit");
 settextstyle(0,0,0);
 setcolor(8);
 for(i=0;i<=7;i++)
 {
 line(180,180+i*20,360,180+i*20);
 line(200+i*20,160,200+i*20,340);
 }
  setcolor(WHITE);
 settextstyle(2,0,0);
 for(i=1;i<=9;i++)
 {
 *c=i+48;
 outtextxy(183+(i-1)*20,140,c);
 }
 for(i=1;i<=9;i++)
 {
 *c=i+48;
 outtextxy(160,163+(i-1)*20,c);
 }
 for(i=0;i<=7;i++)
 {
 if(i==2 || i==5)
 {
 line(180,180+i*20,360,180+i*20);
 line(200+i*20,160,200+i*20,340);
 }
 }
 setcolor(8);
 delay(2500);
 int x1=430,y1=-90,x2=610,y2=90;
 int xx1=-70,yy1=410,xx2=110,yy2=590;
 setcolor(BLACK);
 for(i=0;i<=250;i+=5)
 {
 line(x1,160,x2,160);
 line(xx1,340,xx2,340);
 line(180,y1,180,y2);
 line(360,yy1,360,yy2);
 x1=430,y1=-90,x2=610,y2=90;
 xx1=-70,yy1=410,xx2=110,yy2=590;
 x1=x1-i*1;
 x2=x2-i*1;
 xx1=xx1+i*1;
 xx2=xx2+i*1;
 y1=y1+i*1;
 y2=y2+i*1;
 yy1=yy1-i*1;
 yy2=yy2-i*1;
 setcolor(WHITE);
 line(180,y1,180,y2);
 line(xx1,340,xx2,340);
 line(x1,160,x2,160);
 line(360,yy1,360,yy2);
 setcolor(BLACK);
  delay(10);
 }
 settextstyle(2,0,0);
 setcolor(15);
 outtextxy(185,350,"FILL IN THE GRID WITH DIGITS");
 outtextxy(185,365,"IN SUCH A MANNER THAT EVERY ");
 outtextxy(185,380,"ROW,EVERY COLUMN,EVERY 3X3 BOX");
 outtextxy(185,395,"ACCOMODATES THE DIGITS 1-9");
 setcolor(8);
 rectangle(390,83,620,160);
 setcolor(WHITE);
 rectangle(388,81,622,162);
 line(180,340,360,340);
 line(360,160,360,340);
 setcolor(15);
 initmp();
  settextstyle(1,0,1);
 getmp(&but,&x,&y);
 int point=0;
 while(1)
 {
 while(1)
 {
 settextstyle(1,0,1);
 getmp(&but,&x,&y);
 if(x>=470 && x<=570 && y>=180 && y<=200)
  {
  rectangle(469,179,571,201);
  outtextxy(497,178,"New");
  settextstyle(0,0,0);
  setfillstyle(1,BLACK);
  if(point!=1)
  {
  bar(391,84,619,159);
  outtextxy(395,87,"THIS PLACES A NEW PUZZLE");
  outtextxy(395,97,"TRY SOLVING IT!!!");
  outtextxy(395,117,"PRESS RETURN FOR THE");
  outtextxy(395,127,"SOLUTION");
  }
  point=1;
  }
 else
 if(x>=470 && x<=570 && y>=215 && y<=235)
  {
  rectangle(469,214,571,236);
  outtextxy(495,213,"Solve");
  settextstyle(0,0,0);
  setfillstyle(1,BLACK);
  if(point!=2)
  {
  bar(391,84,619,159);
  outtextxy(395,87,"ENTER THE PUZZLE.");
  outtextxy(395,107,"PRESS RETURN FOR THE");
  outtextxy(395,117,"SOLUTION.");
  outtextxy(395,137,"PRESS ALT+F TO FIX THE");
  outtextxy(395,147,"PUZZLE.");
  }
  point=2;
  }
/* else
 if(x>=470 && x<=570 && y>=250 && y<=270)
  {
  rectangle(469,249,571,271);
  outtextxy(480,248,"Hi Scores");
  }*/
 else
 if(x>=470 && x<=570 && y>=285 && y<=305)
  {
  rectangle(469,284,571,306);
  outtextxy(497,283,"Quit");
  settextstyle(0,0,0);
  setfillstyle(1,BLACK);
  if(point!=3)
  {
  bar(391,84,619,159);
  outtextxy(395,87,"QUIT");
  }
  point=3;
  }
 else
  {
  setcolor(BLACK);
  rectangle(469,179,571,201);
  rectangle(469,214,571,236);
  rectangle(469,249,571,271);
  rectangle(469,284,571,306);
  setcolor(8);
  outtextxy(497,178,"New");
  outtextxy(495,213,"Solve");
/*  outtextxy(480,248,"Hi Scores");*/
  outtextxy(497,283,"Quit");
  setcolor(15);
  }
 if((but&1)==1)
  {
  if(x>=470 && x<=570 && y>=180 && y<=200)
   {
   option=1;
   break;
   }
  else
  if(x>=470 && x<=570 && y>=215 && y<=235)
   {
   option=2;
   break;
   }
  else
  if(x>=470 && x<=570 && y>=250 && y<=270)
   {
   option=3;
   break;
   }
  else
  if(x>=470 && x<=570 && y>=285 && y<=305)
  {
  option=4;
  break;
  }
  }
 }
 setfillstyle(1,BLACK);
 bar(160,140,380,360);
 setcolor(8);
 for(i=-1;i<=8;i++)
 {
 line(180,180+i*20,360,180+i*20);
 line(200+i*20,160,200+i*20,340);
 }
  setcolor(WHITE);
 for(i=-1;i<=8;i++)
 {
 if(i==2 || i==5 || i==-1 ||i==8)
 {
 line(180,180+i*20,360,180+i*20);
 line(200+i*20,160,200+i*20,340);
 }
 }
 for(int i=1;i<=9;i++)
 for(int j=1;j<=9;j++)
 {
 a[i][j]=100;
 save[i][j]=0;
 }
for(i=0;i<=35;i++)
 v[i]=0;
 if(option==1)
  {
  create();
  }
 else
 if(option==2)
 {
 settextstyle(0,0,0);
 disp();
 if(fixed!=1)
  {
  solve();
  print();
  }
  fixed=0;
 }
else
if(option==3)
 {}
else
if(option==4)
  exit(0);
}
 }
/********************************* DISP ***********************************/
void disp()
 {
 int x,y,n,p,q,x1,x2,y1,y2;
 setcolor(8);
 x1=182;y1=162;x2=198;y2=178;
 x=1;
 y=1;
 rectangle(x1,y1,x2,y2);
 setcolor(WHITE);
 setfillstyle(1,BLACK);
 bar(180,80,340,140);
 setcolor(WHITE);
 settextstyle(2,0,0);
 for(int i=1;i<=9;i++)
 {
 *c=i+48;
 outtextxy(183+(i-1)*20,140,c);
 }
 for(i=1;i<=9;i++)
 {
 *c=i+48;
 outtextxy(160,163+(i-1)*20,c);
 }
 settextstyle(0,0,0);
 *c=' ';
 for(i=0;;i++)
 {
 *c=getch();
 bar(391,84,619,159);
 if(*c==0)
 {
 *c=getch();
 if(*c==77)
  {
  if(x2+20>360)
  continue;
  setcolor(BLACK);
  rectangle(x1,y1,x2,y2);
  x1+=20;
  x2+=20;
  y++;
  setcolor(8);
  rectangle(x1,y1,x2,y2);
  setcolor(WHITE);
  }
 else
 if(*c==75)
  {
  if(x1-20<180)
  continue;
  setcolor(BLACK);
  rectangle(x1,y1,x2,y2);
  x1-=20;
  x2-=20;
  y--;
  setcolor(8);
  rectangle(x1,y1,x2,y2);
  setcolor(WHITE);
  }
 else
 if(*c==80)
  {
  if(y2+20>340)
  continue;
  setcolor(BLACK);
  rectangle(x1,y1,x2,y2);
  y1+=20;
  y2+=20;
  x++;
  setcolor(8);
  rectangle(x1,y1,x2,y2);
  setcolor(WHITE);
  }
 else
 if(*c==72)
  {
  if(y1-20<160)
  continue;
  setcolor(BLACK);
  rectangle(x1,y1,x2,y2);
  y1-=20;
  y2-=20;
  x--;
  setcolor(8);
  rectangle(x1,y1,x2,y2);
  setcolor(WHITE);
  }
  else
  if(*c==83)
   {
   a[x][y]=100;
   setfillstyle(1,BLACK);
   bar(x1+1,y1+1,x2-1,y2-1);
   }
  else
  if(*c==33)
   {
   for(i=1;i<=9;i++)
   for(int j=1;j<=9;j++)
    {
    x1=182,y1=162,x2=198,y2=178;
    cop[i][j]=a[i][j];
    y1+=((i-1)*20);
    y2+=((i-1)*20);
    x1+=((j-1)*20);
    x2+=((j-1)*20);
    setcolor(BLACK);
    rectangle(x1,y1,x2,y2);
    if(a[i][j]==100)
	continue;
    *ch=' ';
    *ch=a[i][j]+48;
    setfillstyle(1,8);
    bar(x1,y1,x2,y2);
    outtextxy(x1+5,y1+5,ch);
    }
   solve();
   fix();
   fixed=1;
   break;
   }
  }
  else
  {
  if(*c>=48 && *c<=57)
   {
   a[x][y]=*c-48;
   setfillstyle(1,BLACK);
   bar(x1+1,y1+1,x2-1,y2-1);
   outtextxy(x1+5,y1+5,c);
   for(i=0;i<=35;i++)
   v[i]=0;
   int yo=1;
  for(i=9;i>0;i--)
   for(int j=9;j>0;j--)
   {
   if(a[i][j]==100)
   continue;
    v[yo]=(((a[i][j]*10)+i)*10)+j;
   check();
   if(wrong==1)
	 {
	 wrong=0;
	 break;
	 }
    m++;
   }
   }
   else
   if(*c==13)
   break;
  }
  }
  for(i=0;i<=35;i++)
   v[i]=0;
}
/*************************** CHECK *****************************/
void check()
 {
 int x=0;
 char cool[40];
 for(int n=1;n<=9;n++)
 {
 for(int r=1;v[r]!=0;r++)
  {
  m=v[r];
  g=m%10;
  m/=10;
  f=m%10;
  m/=10;
  e=m;
 if(n==e)
 {
 for(int x=1;x<=9 ;x++)
  if(a[f][x]==n && x!=g)
  {
  bar(391,84,619,159);
  sprintf(cool,"More than one %d is placed in",n);
  outtextxy(395,87,cool);
  sprintf(cool,"the ROW %d",f);
  outtextxy(395,97,cool);
  sprintf(cool,"Delete one of the %ds",n);
  outtextxy(395,107,cool);
  wrong=1;
  fflush(stdin);
  return;
  }
 for(x=1;x<=9;x++)
  if(a[x][g]==n && x!=f)
  {
  bar(391,84,619,159);
  sprintf(cool,"More than one %d is placed in",n);
  outtextxy(395,87,cool);
  sprintf(cool,"the COLUMN %d",g);
  outtextxy(395,97,cool);
  sprintf(cool,"Delete one of the %ds",n);
  outtextxy(395,107,cool);
  wrong=1;
  fflush(stdin);
  return;
  }
 int k,l;
 k=g/3;
 if(g%3==0)
 k--;
 k=k*3+1;
 l=f/3;
 if(f%3==0)
 l--;
 l=l*3+1;
 int m,o;
 m=l+3;
 o=k+3;
 for(int i=l;i<m;i++)
  for(int j=k;j<o;j++)
  if(a[i][j]==n && i!=f && j!=g)
   {
   bar(391,84,619,159);
   sprintf(cool,"More than one %d is placed in",n);
   outtextxy(395,87,cool);
   sprintf(cool,"the BOX %c",' ');
   outtextxy(395,97,cool);
   sprintf(cool,"Delete one of the %ds",n);
   outtextxy(395,107,cool);
   wrong=1;
   fflush(stdin);
   return;
   }
  }
  }
 }
 x=x;
 }
/**************************************** SOLVE **************************/
void solve()
 {
  int m,x,y;
   m=1;
  for(int i=9;i>0;i--)
   for(int j=9;j>0;j--)
   {
   if(a[i][j]==100)
   continue;
   v[m]=(((a[i][j]*10)+i)*10)+j;
   m++;
   }
for(int n=1;n<=9;n++)
{
for(i=1;i<=9;i++)
 {
 for(int r=1;v[r]!=0;r++)
  {
  m=v[r];
  g=m%10;
  m/=10;
  f=m%10;
  m/=10;
  e=m;
  if(n==e && f==i)
   {
   place(e,f,g);
   over=1;
   break;
   }
  if(i==1 && n==e)
   {
   place(e,f,g);
   }
  }
if(over==1)
 {
 over=0;
 continue;
 }
 for(j=1;j<=9;j++)
 {
 if(a[i][j]==100)
  {
  a[i][j]=n;
  place(n,i,j);
  break;
  }
 }
 for(r=1;v[r]!=0;r++)
  {
  m=v[r];
  g=m%10;
  m/=10;
  f=m%10;
  m/=10;
  e=m;
 if(n==e && i==f+1 && j==10)
  {
  i--;
  }
  }
 if(i==1 && j==10)
  {
  for(x=1;x<=9;x++)
  for(y=1;y<=9;y++)
  if(a[x][y]==-1 || a[x][y]==100)
   a[x][y]=0;
  n--;
  for(j=1;save[n][j]!=0;j++)
  {
  y=save[n][j]%10;
  save[n][j]/=10;
  x=save[n][j];
  a[x][y]=-1;
  }
  i=10;
  for(r=1;v[r]!=0;r++)
  {
  m=v[r];
  g=m%10;
  m/=10;
  f=m%10;
  m/=10;
  e=m;
 if(n==e && i==f+1)
  i--;
  }
  back(n,i,j);
  i-=2;
  }
 else
 if(j==10)
  {
  back(n,i,j);
  i-=2;
  }
 }
 int h=1;
 for(x=1;x<=9;x++)
 for(y=1;y<=9;y++)
  if(a[x][y]==-1)
  {
  save[n][h]=x*10+y;
  h++;
  }
 for(x=1;x<=9;x++)
 for(y=1;y<=9;y++)
  if(a[x][y]==0 || a[x][y]==-1)
  a[x][y]=100;
}
}
/********************************* CREATE ****************************/
void create()
 {
 int i,j,k,val;
 for(i=1;i<=9;i++)
  for(j=1;j<=9;j++)
  cop[i][j]=100;
 m=1;
 time_t t;
 srand((unsigned) time(&t));
 for(val=1;val<=9;val++)
 {
 i=rand()%9+1;
 j=rand()%9+1;
 a[i][j]=val;
 v[m]=(val*10+i)*10+j;
 m++;
 }
 solve();
   setcolor(WHITE);
 settextstyle(2,0,0);
 for(i=1;i<=9;i++)
 {
 *c=i+48;
 outtextxy(183+(i-1)*20,140,c);
 }
 for(i=1;i<=9;i++)
 {
 *c=i+48;
 outtextxy(160,163+(i-1)*20,c);
 }
 int ii,jj;
 int x1,y1,x2,y2,x,r,f=0,g=0,con=0,con1=0;
 for(val=1;val<=27;val++)
  {
  ii=rand()%3+1;
  jj=rand()%3+1;
  ii+=g;
  jj+=f;
  con++;
  if(con==3)
  {
  con1++;
  con=0;
  f+=3;
  }
  if(con1==3)
  {
  con1=0;
  g+=3;
  f=0;
  }
  x1=182,y1=162,x2=198,y2=178;
  setfillstyle(1,8);
  settextstyle(0,0,0);
  setcolor(BLACK);
  x1=x1+(jj-1)*20;
  y1=y1+(ii-1)*20;
  x2=x2+(jj-1)*20;
  y2=y2+(ii-1)*20;
  cop[ii][jj]=a[ii][jj];
  *ch=a[ii][jj]+48;
  bar(x1,y1,x2,y2);
  outtextxy(x1+5,y1+5,ch);
  }
  fix();
}
/*************************************** FIX *****************/
void fix()
 {
 timfix();
 setfillstyle(1,BLACK);
 bar(180,100,340,140);
 settextstyle(2,0,0);
 setcolor(15);
 outtextxy(185,350,"FILL IN THE GRID WITH DIGITS");
 outtextxy(185,365,"IN SUCH A MANNER THAT EVERY ");
 outtextxy(185,380,"ROW,EVERY COLUMN,EVERY 3X3 BOX");
 outtextxy(185,395,"ACCOMODATES THE DIGITS 1-9");
 setfillstyle(1,8);
 int i,j,k,val;
 int x1,y1,x2,y2,x,r;
 for(j=1;j<=35;j++)
  v1[j]=0;
  m=1;
  for(i=1;i<=9;i++)
	   for(j=1;j<=9;j++)
	   {
	   if(cop[i][j]==100)
	   continue;
	   v1[m]=(((cop[i][j]*10)+i)*10)+j;
	   m++;
	   }
  for(i=1;i<=9;i++)
  for(j=1;j<=9;j++)
  {
  k=cop[i][j];
  cop[i][j]=a[i][j];
  a[i][j]=k;
  }
  for(j=1;j<=35;j++)
   v[j]=0;
  m=1;
  for(i=1;i<=9;i++)
   for(j=1;j<=9;j++)
   {
   if(a[i][j]==100)
   continue;
   v[m]=(((a[i][j]*10)+i)*10)+j;
   m++;
   }
 int p,q;
 setcolor(8);
 x1=182;y1=162;x2=198;y2=178;
 int z=0;
 int xp=1;
 int yp=1;
 for(r=1;v[r]!=0;r++)
  {
  m=v1[r];
  g=m%10;
  m/=10;
  f=m%10;
  m/=10;
  e=m;
  if(f==xp && g==yp)
   {
   yp++;
   x1+=20;
   x2+=20;
   }
  }
 rectangle(x1,y1,x2,y2);
 setcolor(WHITE);
 setfillstyle(1,BLACK);
 *c=' ';
 for(i=0;;i++)
 {
 int hisc=0;
 for(i=1;i<=9;i++)
  for(j=1;j<=9;j++)
  {
  if(a[i][j]!=100)
   hisc++;
  }
  if(hisc==81)
  {
  wrong=0;
  check();
  cout<<wrong<<endl;
  if(wrong==0)
  {
  hiscore();
  hisc=100;
  break;
  }
  wrong=0;
  }
 while (!kbhit())
  tim();
 *c=getch();
  bar(391,84,619,159);
 if(*c==0)
 {
 *c=getch();
 if(*c==77)
  {
  yp++;
  for(r=1;v1[r]!=0;r++)
  {
  m=v1[r];
  g=m%10;
  m/=10;
  f=m%10;
  m/=10;
  e=m;
  if(f==xp && g==yp)
   {
   z++;
   yp++;
   }
  }
  z++;
  if(x2+(z*20)>360)
  {
  yp=yp-z;
  z=0;
  continue;
  }
  setcolor(BLACK);
  rectangle(x1,y1,x2,y2);
  x1+=(z*20);
  x2+=(z*20);
  z=0;
  setcolor(8);
  rectangle(x1,y1,x2,y2);
  setcolor(WHITE);
  }
 else
 if(*c==75)
 {
  yp--;
  for(r=35;r>0;r--)
  {
  m=v1[r];
  g=m%10;
  m/=10;
  f=m%10;
  m/=10;
  e=m;
  if(f==xp && g==yp)
   {
   z++;
   yp--;
   }
  }
  z++;
  if(x1-(z*20)<180)
  {
  yp=yp+z;
  z=0;
  continue;
  }
  setcolor(BLACK);
  rectangle(x1,y1,x2,y2);
  x1-=(z*20);
  x2-=(z*20);
  z=0;
  setcolor(8);
  rectangle(x1,y1,x2,y2);
  setcolor(WHITE);
  }
 else
 if(*c==80)
  {
  xp++;
  for(r=1;v1[r]!=0;r++)
  {
  m=v1[r];
  g=m%10;
  m/=10;
  f=m%10;
  m/=10;
  e=m;
  if(f==xp && g==yp)
   {
   z++;
   xp++;
   }
  }
  z++;
  if(y2+(z*20)>340)
  {
  xp=xp-z;
  z=0;
  continue;
  }
  setcolor(BLACK);
  rectangle(x1,y1,x2,y2);
  y1+=(z*20);
  y2+=(z*20);
  z=0;
  setcolor(8);
  rectangle(x1,y1,x2,y2);
  setcolor(WHITE);
  }
 else
 if(*c==72)
  {
  xp--;
  for(r=35;r>0;r--)
  {
  m=v1[r];
  g=m%10;
  m/=10;
  f=m%10;
  m/=10;
  e=m;
  if(f==xp && g==yp)
   {
   z++;
   xp--;
   }
  }
  z++;
  if(y1-(z*20)<160)
  {
  xp=xp+z;
  z=0;
  continue;
  }
  setcolor(BLACK);
  rectangle(x1,y1,x2,y2);
  y1-=(z*20);
  y2-=(z*20);
  z=0;
  setcolor(8);
  rectangle(x1,y1,x2,y2);
  setcolor(WHITE);
  }
  else
  if(*c==83)
   {
   a[xp][yp]=100;
   setfillstyle(1,BLACK);
   bar(x1+1,y1+1,x2-1,y2-1);
   }
  }
  else
  {
  if(*c>=48 && *c<=57)
   {
   a[xp][yp]=*c-48;
   setfillstyle(1,BLACK);
   bar(x1+1,y1+1,x2-1,y2-1);
   outtextxy(x1+5,y1+5,c);
   for(i=0;i<=35;i++)
   v[i]=0;
  int yo=1;
  for(i=9;i>0;i--)
   for(int j=9;j>0;j--)
   {
   if(a[i][j]==100)
   continue;
    v[yo]=(((a[i][j]*10)+i)*10)+j;
   check();
   if(wrong==1)
	 {
	 wrong=0;
	 break;
	 }
    m++;
   }
   }
   else
   if(*c==13)
   {
    for(i=1;i<=9;i++)
	for(j=1;j<=9;j++)
	 a[i][j]=cop[i][j];
    for(j=1;j<=35;j++)
	v[j]=v1[j];
    for(j=1;j<=35;j++)
   print();
   break;
   }
  }
  if(hisc==100)
  {
  break;
  }
  }
 }
void timfix()
 {
 struct dostime_t t;
 _dos_gettime(&t);
 tm=t.minute;
 ts=t.second;
 }
void tim()
 {
 int temp=0,temp1=0;
 struct dostime_t t;
 _dos_gettime(&t);
 temp=t.second;
 temp1=t.minute;
 if(temp<ts)
 {
 temp+=60;
 temp1-=1;
 }
 if(temp1<tm)
 {
 temp1+=60;
 }
 temp1=temp1-tm;
 temp=temp-ts;
 delay(100);
 char *cool;
 if(temp>=10 && temp1>=10)
 sprintf(cool,"%d : %d",temp1,temp);
 else
 if(temp<10 && temp1<10)
 sprintf(cool,"0%d : 0%d",temp1,temp);
 else
 if(temp>=10 && temp1<10)
  sprintf(cool,"0%d : %d",temp1,temp);
 else
  sprintf(cool,"%d : 0%d",temp1,temp);
 bar(180,80,340,140);
 setcolor(WHITE);
 settextstyle(4,0,4);
 outtextxy(210,80,cool);
 settextstyle(0,0,0);
 min=temp1;
 sec=temp;
 }
void hiscore()
 {
  setcolor(15);
  bar(391,84,619,159);
  outtextxy(395,87,"YOU ARE VICTORIOUS");
  outtextxy(395,97,"Press any key to continue.");
  getch();
 }
```

