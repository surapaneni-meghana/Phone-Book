#include<stdio.h>
#include<stdlib.h>
#include<stdbool.h>
#include<string.h>
#define size 26

typedef struct trie
{
struct trie *node[size];
bool val;
}trie;

typedef struct phone
{
  char name[30];
  char num[15];
  struct phone *next;
}phone;

phone *q,*p,*t;
trie *root,*temp;

void enter(char n[30],char num[15])
{
	t=(phone *)malloc(sizeof(phone));
	strcpy(t->name,n);
	strcpy(t->num,num);
	if(p==NULL)
	 p=t;
	else{
		t->next=p;
		p=t;
	}	
}

void display()
{
	char n[30];
	printf("Enter name :\n");
	scanf("%s",n);
	t=p;
	int f=0;
	while(t!=NULL)
	{
	  if(!(strcmp(t->name,n))){
	    printf("%s\n",t->num);
	    f=1;
		break;
	  }
	  t=t->next;
	}
	if(f==0)
	  printf("No contact found!!\n");
}

trie* newnode()
{
  int i;
  temp=(trie*)malloc(sizeof(trie));
  for(i=0;i<size;i++)
  temp->node[i]=NULL;
  temp->val=false;
  return temp;
}

void insert(trie *t)
{
  int i,index,len;
  char str[30],num[15];
  printf("Enter contact name :\n");
  scanf("%s",str);
  bool val=true;
  while(val){
  	printf("Enter number :\n");
    scanf("%s",num);
    if(strlen(num)!=10){
    	printf("Please! Enter Valid Number\n");
    	val=true;
    }
    else
       val=false;
  }
  
  enter(str,num);
  len=strlen(str);
  for(i=0;i<len;i++)
  {
   	index=str[i]-'a';
   	if(!t->node[index])
   	 t->node[index]=newnode();
   	 t=t->node[index];
  }
  t->val=true;
}

bool isLastNode(trie* root) 
{ 
   int i;
    for (i = 0; i < size; i++) 
        if (root->node[i]) 
            return 0; 
    return 1; 
} 

void suggestions(trie* root,char prefix[10]) 
{ 
    static int f=0;
    if (root->val) 
    { 
        printf("%s\n",prefix);
        f++;
    } 
    if (isLastNode(root)) {
    	
        return;
    }
        
    int i,len;
    char c;
    for (i = 0; i < size; i++) 
    { 
        if (root->node[i]) 
        { 
            c=97+i;
           strncat(prefix,&c,1);
           len=strlen(prefix);
            suggestions(root->node[i],prefix);
            prefix[len-1]='\0';
            len=len-1;     
        } 
    }
} 

int search(trie* root,char str[10]) 
{ 
    trie *pnode = root; 
    int i;
    int n = strlen(str);
    for (i = 0; i < n; i++) 
    { 
        int index =str[i]-'a';
        if (!pnode->node[index]) 
            return 0; 
        pnode = pnode->node[index]; 
    } 
    bool isWord = (pnode->val == true); 
    bool isLast = isLastNode(pnode);
    if (isWord && isLast) 
    { 
        printf("%s\n",str);
        return -1; 
    } 
    if (!isLast) 
    { 
        char prefix[10];
        strcpy(prefix,str);
       int len=strlen(prefix);
        suggestions(pnode, prefix); 
        return 1; 
    } 
    
} 

int main()
{
	int i,ch;
	char str[10];
	root=newnode();

	printf("***********************************************************************\n");
	printf("-----------------------| PHONE BOOK DIRECTORY |------------------------\n");
	printf("***********************************************************************\n");

	while(1)
	{
	  printf("\n1.save contact \n2.search contact\n3.get details\n");
  	  scanf("%d",&ch);
	  switch(ch)
	  {
  		case 1:
  		printf("------------------------------------------\n");
  		  insert(root);
  		  printf("------------------------------------------\n");
  		  break;
	    case 2:
	    printf("------------------------------------------\n");
	     scanf("%s",str);
	      search(root,str);
	      printf("------------------------------------------\n");
	      break;
        case 3:
        printf("------------------------------------------\n");
          display();
          printf("------------------------------------------\n");
          break;
        case 4:
          exit(1);
          break;
	  }
  	}
}
		
