## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 

## AIM:
 

 

To write a C program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.
## EXAMPLE:
![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

 

## ALGORITHM:

STEP-1: Read the plain text from the user.
STEP-2: Read the keyword from the user.
STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
STEP-5: Display the obtained cipher text.




Program:
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

char t[5][5]; int u[26];

void table(char k[]){
    int i,r=0,c=0;
    for(i=0;k[i];i++){
        char ch=tolower(k[i]);
        if(ch=='j') ch='i';
        if(ch>='a'&&ch<='z'&&!u[ch-'a']){
            u[ch-'a']=1; t[r][c++]=ch;
            if(c==5) r++,c=0;
        }
    }
    for(char ch='a';ch<='z';ch++){
        if(ch=='j'||u[ch-'a']) continue;
        t[r][c++]=ch;
        if(c==5) r++,c=0;
    }
}

void pos(char ch,int *r,int *c){
    if(ch=='j') ch='i';
    for(int i=0;i<5;i++)
        for(int j=0;j<5;j++)
            if(t[i][j]==ch){*r=i;*c=j;}
}

void enc(char p[],char c[]){
    int i,r1,c1,r2,c2,k=0;
    for(i=0;p[i]&&p[i+1];i+=2){
        pos(p[i],&r1,&c1); pos(p[i+1],&r2,&c2);
        if(r1==r2){
            c[k++]=t[r1][(c1+1)%5];
            c[k++]=t[r2][(c2+1)%5];
        } else if(c1==c2){
            c[k++]=t[(r1+1)%5][c1];
            c[k++]=t[(r2+1)%5][c2];
        } else {
            c[k++]=t[r1][c2];
            c[k++]=t[r2][c1];
        }
    }
    c[k]=0;
}

int main(){
    char p[50],k[50],c[50];

    printf("Enter Plain Text: ");
    fgets(p,50,stdin);

    printf("Enter Key: ");
    fgets(k,50,stdin);

    for(int i=0;p[i];i++)
        if(p[i]!=' ') p[i]=tolower(p[i]);

    table(k); enc(p,c);

    printf("\nPlain Text : %s",p);
    printf("Key : %s",k);

    printf("\nKey Table:\n");
    for(int i=0;i<5;i++){
        for(int j=0;j<5;j++)
            printf("%c ",t[i][j]);
        printf("\n");
    }

    printf("\nEncrypted Text : %s",c);
}
```

Output:

<img width="575" height="600" alt="image" src="https://github.com/user-attachments/assets/9841d0ec-ffd2-4e11-a7e8-c2e750adcc52" />
