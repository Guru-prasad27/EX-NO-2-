## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

## NAME: GURU PRASAD D.R.
## Reg.No:212225040104

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

char matrix[5][5];

void createMatrix(char key[])
{
    int used[26] = {0};
    int r = 0, c = 0;
    
    // Insert key characters
    for (int i = 0; key[i] != '\0'; i++)
    {
        char ch = tolower(key[i]);
        if (ch >= 'a' && ch <= 'z' && ch != 'j')
        {
            if (!used[ch - 'a'])
            {
                matrix[r][c++] = ch;
                used[ch - 'a'] = 1;
                if (c == 5) { c = 0; r++; }
            }
        }
    }
    
    // Fill remaining letters
    for (char ch = 'a'; ch <= 'z'; ch++)
    {
        if (ch != 'j' && !used[ch - 'a'])
        {
            matrix[r][c++] = ch;
            if (c == 5) { c = 0; r++; }
        }
    }
}

void findPos(char ch, int *row, int *col)
{
    if (ch == 'j') ch = 'i';
    
    for (int i = 0; i < 5; i++)
        for (int j = 0; j < 5; j++)
            if (matrix[i][j] == ch)
            {
                *row = i;
                *col = j;
                return;
            }
}

void prepareText(char text[], char prepared[])
{
    int len = 0;
    
    for (int i = 0; text[i] != '\0'; i++)
    {
        if (isalpha(text[i]))
        {
            char ch = tolower(text[i]);
            if (ch == 'j') ch = 'i';
            prepared[len++] = ch;
        }
    }
    
    // Add 'x' between repeated letters
    char temp[200];
    int t = 0;
    for (int i = 0; i < len; i++)
    {
        temp[t++] = prepared[i];
        if (i + 1 < len && prepared[i] == prepared[i + 1])
            temp[t++] = 'x';
    }
    
    // Make even length
    if (t % 2 != 0) temp[t++] = 'x';
    temp[t] = '\0';
    
    strcpy(prepared, temp);
}

void encrypt(char text[], char cipher[])
{
    char prepared[200];
    prepareText(text, prepared);
    
    int k = 0;
    for (int i = 0; prepared[i] != '\0'; i += 2)
    {
        int r1, c1, r2, c2;
        findPos(prepared[i], &r1, &c1);
        findPos(prepared[i + 1], &r2, &c2);
        
        if (r1 == r2)  // Same row
        {
            cipher[k++] = matrix[r1][(c1 + 1) % 5];
            cipher[k++] = matrix[r2][(c2 + 1) % 5];
        }
        else if (c1 == c2)  // Same column
        {
            cipher[k++] = matrix[(r1 + 1) % 5][c1];
            cipher[k++] = matrix[(r2 + 1) % 5][c2];
        }
        else  // Rectangle
        {
            cipher[k++] = matrix[r1][c2];
            cipher[k++] = matrix[r2][c1];
        }
    }
    cipher[k] = '\0';
}

void decrypt(char cipher[], char plain[])
{
    int k = 0;
    for (int i = 0; cipher[i] != '\0'; i += 2)
    {
        int r1, c1, r2, c2;
        findPos(cipher[i], &r1, &c1);
        findPos(cipher[i + 1], &r2, &c2);
        
        if (r1 == r2)
        {
            plain[k++] = matrix[r1][(c1 + 4) % 5];
            plain[k++] = matrix[r2][(c2 + 4) % 5];
        }
        else if (c1 == c2)
        {
            plain[k++] = matrix[(r1 + 4) % 5][c1];
            plain[k++] = matrix[(r2 + 4) % 5][c2];
        }
        else
        {
            plain[k++] = matrix[r1][c2];
            plain[k++] = matrix[r2][c1];
        }
    }
    plain[k] = '\0';
}

int main()
{
    char key[50], plaintext[100], ciphertext[200], decrypted[200];
    
    printf("Enter keyword: ");
    scanf("%s", key);
    printf("Enter plaintext: ");
    scanf("%s", plaintext);
    
    createMatrix(key);
    
    printf("\nKey Matrix:\n");
    for (int i = 0; i < 5; i++)
    {
        for (int j = 0; j < 5; j++)
            printf("%c ", matrix[i][j]);
        printf("\n");
    }
    
    encrypt(plaintext, ciphertext);
    printf("\nEncrypted: %s\n", ciphertext);
    
    decrypt(ciphertext, decrypted);
    printf("Decrypted: %s\n", decrypted);
    
    return 0;
}

```

Output:
<img width="1838" height="686" alt="image" src="https://github.com/user-attachments/assets/a53bce70-170f-44ce-bff0-d47e33972021" />


## RESULT:
Thus the program was executed successfully.
