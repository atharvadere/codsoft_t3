#include<iostream>

using namespace std;

class solution
{   public:
    char arr[100][100];
    int tictactoe(string p1,string p2);
    void displayboard();
    bool checkwinlose(int i,int j);
    void initialize();
    void mainmenu();
};

static int siz=3;

int solution :: tictactoe(string p1,string p2)
{
    int m,i=0;
    int flag=0;

    cout<<"\nWelcome "<<p1<<" [o]  and "<<p2<<" [x] :\n\n";
    displayboard();
    while(true)
    {   cout<<endl;
        if(i%2==0)
            cout<<p1<<" [o]";
        else
            cout<<p2<<" [x]";
        cout<<" Enter your move :";
        invalidmove:
        cin>>m;
        int tmp=arr[(m-1)/siz][(m-1)%siz];
        if(m<1 || m>siz*siz || tmp=='x' || tmp=='o')
        {
            cout<<"Error ! invalid move !\nPlease renter move :";
            goto invalidmove;
        }
        if(i%2==0)
            arr[(m-1)/siz][(m-1)%siz]='o';
        else
            arr[(m-1)/siz][(m-1)%siz]='x';
        displayboard();
        if(i>siz-2)
        {
            flag=checkwinlose((m-1)/siz,(m-1)%siz);
            if(flag)
            {
                if(i%2==0)
                    cout<<p1<<" Won !";
                else
                    cout<<p2<<" Won !";
                return i%2+1;
            }
            else if(i==siz*siz-1)
            {
                cout<<"Match Draw !";
                return 0;
            }
        }    
        i++;
    }
}

void solution :: displayboard()
{   int k,m;
    for(int i=1;i<siz*4;i++)
    {   
        for(int j=1;j<siz*4;j++)
        {   
            if(i%4==0 && j%4==0)
            {
                cout<<"+";
                continue;
            }
            if(j%4==0)
            {
                cout<<"|";
                continue;
            }
            if((j-i)%4==0 && j%2==0)
            {   
                k=(i-2)/4;
                m=(j-2)/4;
                if(arr[k][m]=='1')
                {
                    cout<<k*siz+m+1;
                    if(k*siz+m+1>=10)
                        j++;
                }    
                else
                    cout<<arr[k][m];
                continue;
            }
            if(i%4==0)
                cout<<"-";
            else
                cout<<" ";
        }
        cout<<endl;
    }
}

bool solution :: checkwinlose(int i,int j)
{
    int flg=1;
    char cmpval=arr[i][j];
    for(int k=0;k<siz;k++)
    {
        if(arr[i][k]!=cmpval)
        {
            flg=0;
            break;
        }
    }
    if(flg)
        return 1;
    flg=1;
    for(int m=0;m<siz-1;m++)
    {
        if(arr[m][j]!=arr[m+1][j])
        {
            flg=0;
            break;
        }
    }
    if(flg)
        return 1;
    
    if(i==j)
    {      
        flg=1;
        for(int n=0;n<siz-1;n++)
        {
            if(arr[n][n]!=arr[n+1][n+1])
            {
                flg=0;
                break;
            }
        }
        if(flg)
            return 1;
    }
    if(i+j==siz-1)
    {
        flg=1;
        int temp;
        for(int p=0;p<siz;p++)
        {
            temp=siz-p-1;
            if(arr[p][temp]!=cmpval)
            {
                flg=0;
                break;
            }
        }
        if(flg)
            return 1;
    }
    if(flg)
        return 1;
    else
        return 0;
}

void solution :: mainmenu()
{   int ch,res;
    bool swapflg=false;
    int resar[3]={0,0,0};
    string p1,p2,temp;
    cout<<"\n\nWelcome to Tic Tac Toe Game \n";
    cout<<"\nEnter Player 1 name [o]:";
    cin>>p1;
    cout<<"\nEnter Player 2 name [x]:";
    cin>>p2;
    while(true)
    {
    cout<<"\n\n1.Enter Game\n2.Matrix Size [default 3]\n3.Switch Players\n4.Display Result\n5.Exit\n";
    cout<<"Enter your choice :";
    invalidch:
    cin>>ch;
    if(ch<1 || ch>5)
    {
        cout<<"Please enter valid choice !\nRe-enter your choice :";
        goto invalidch;
    }
    switch(ch){
        case 1:
        initialize();
        res=tictactoe(p1,p2);
        if(!swapflg || res==0)
            resar[res]++;
        else
        {
            resar[(res%2)+1]++;
        }
        break;

        case 2:
        cout<<"\nEnter matrix size for game [1<size<10]:";
        cin>>siz;
        if(siz>1 && siz<10)
            cout<<"\nMatrix size set to "<<siz<<"\n";
        else
        {
            siz=3;
            cout<<"\nInvalid matrix size !\nMatrix size set to default [3]\n";
        }
        break;

        case 3:
        temp=p1;
        p1=p2;
        p2=temp;
        swapflg=!swapflg;
        cout<<"Players switched successfully !";
        cout<<"\n"<<p1<<"-> [o]\n"<<p2<<"-> [x]";
        break;

        case 4 :
        cout<<"\n---Result---\nTotal Matches :";
        cout<<resar[0]+resar[1]+resar[2];
        cout<<"\nDraw :"<<resar[0];
        cout<<"\nWon by "<<p1<<" :"<<resar[1+swapflg];
        cout<<"\nWon by "<<p2<<" :"<<resar[2-swapflg];
        break;

        case 5:
        cout<<"\nThank You !\n";
        return;
    }
    }
}

void solution::initialize()
{
        for(int i=0;i<siz;i++)
        {   
            for(int j=0;j<siz;j++)
            { 
                arr[i][j]='1';
            }    
        }
}

int main()
{   
    solution obj;
    obj.mainmenu();
    return 0;
}