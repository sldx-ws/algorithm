# 版本1：next[0] = -1

```c
#include <stdio.h>
#include <string.h>

void getNext(char* pat, int* next, int len)
{
    int i = 0;  // 指向后缀末尾
    int j = -1; // 指向前缀末尾

    // 默认 next[0] = -1
    next[0] = -1;

    while (i < len)
    {
        if (j == -1 || pat[i] == pat[j])
        {
            ++i;
            ++j;
            next[i] = j;  // j指向前缀末尾
        }
        else
        {
            j = next[j];
        }
    }  
}

void getNextval(char* pat, int* nextval, int len)
{
    getNext(pat, nextval, len);
    
    for (int i = 1; i < len; ++i)
    {
        if (pat[i] == pat[nextval[i]])
            nextval[i] = nextval[nextval[i]];
    }
}

int KMP(char* txt, char* pat)
{
    int n = strlen(txt);
    int m = strlen(pat);
    int nextval[m + 1];  // 注意是m+1，getNext中会用到nextval[m]

    // getNext(pat, nextval, m);
    getNextval(pat, nextval, m);

    int i = 0, j = 0;
    while (i < n && j < m)
    {
        if (j == -1 || txt[i] == pat[j])
        {
            ++i;
            ++j;
        }
        else 
        {
            j = nextval[j];
        }
    }

    if (j == m)
        return i - j;
    else 
        return -1;
}

int main() 
{
    char text[] = "ababcabcacbab";
    char pattern[] = "abcac";

    int pos = KMP(text, pattern);

    if (pos != -1)
        printf("匹配成功，起始位置：%d\n", pos);
    else
        printf("未匹配到\n");

    return 0;
}
```



# 版本2：next[0] = 0

```c
#include <stdio.h>
#include <string.h>

void getNext(char* pat, int* next, int len)
{
    int i = 1;  // 从1开始（标准next数组）
    int j = 0;  // 前缀长度

    // 默认 next[0] = 0
    next[0] = 0;

    while (i < len)
    {
        if (pat[i] == pat[j])
        {
            ++j;
            next[i] = j;
            ++i;
        }
        else
        {
            if (j > 0)
            {
                j = next[j - 1];
            }
            else 
            {
                next[i] = 0;
                ++i;
            }
        }
    }
}

int KMP(char* txt, char* pat)
{
    int n = strlen(txt);
    int m = strlen(pat);
    int next[m];

    getNext(pat, next, m);

    int i = 0, j = 0;
    while (i < n && j < m)
    {
        if (txt[i] == pat[j])
        {
            ++i;
            ++j;
        }
        else 
        {
            if (j > 0)
                j = next[j - 1];
            else 
                ++i;
        }
    }

    if (j == m)
        return i - j;
    else 
        return -1;
}

int main() 
{
    char text[] = "ababcabcacbab";
    char pattern[] = "abcac";

    int pos = KMP(text, pattern);

    if (pos != -1)
        printf("匹配成功，起始位置：%d\n", pos);
    else
        printf("未匹配到\n");

    return 0;
}
```

