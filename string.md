# string
1. 求字符串长度
strlen
长度不受限制的字符串函数
strcpy 
strcat
strcmp
``` c
int strcmp(const char *str1, const char *str2){
assert(str1 && str2);
while(*str1 == *str2){
if(*str1 == '\0')
return 0;
str1++;
str2++;
}
} 
```
长度受限制的字符串函数介绍
strncpy
它的功能是把src所指向的字符串复制到dest，最多复制n个字符。当src的长度小于n时，dest的剩余部分将用空字节填充。
这个函数的优点是可以防止目标数组溢出，因为它只复制指定的字符数。但是它也有一些缺点，比如：
如果src的长度大于等于n，那么dest不会以空字符结尾，可能导致字符串操作错误。
如果src的长度小于n，那么dest的剩余部分会被填充很多空字符，浪费空间和时间。
如果n的值不合适，可能会截断src中的有效字符，造成信息丢失。
示例代码1:
```c
char*cdecl strncpy(char * dest,const char *source,size_t count){
char *start = dest;
while (count && (*dest++ = *source++))/* copy string */count--;
if (count)while (--count)*dest++ =' 0';
/* pad out with zeroes */
return(start);}
示例代码2:
char* my_strncpy(char*dest,const char*src,int n)
{
    //assert(dest&&src);
    char*ret=dest;
    while(n--)
    {
        *dest++=*src++;
    }
    return ret;
}
```
strncat
它的功能是把src所指向的字符串追加到dest所指向的字符串的结尾，直到n个字符长度为止。

使用strncat函数时，需要注意以下事项：

dest指向的数组必须足够大，能够容纳追加后的字符串，包括额外的空字符。
src指向的字符串必须以空字符结尾，否则可能会造成缓冲区溢出。
n指定的字符数不能超过src字符串的长度，否则可能会追加多余的空字符。
strncat函数返回一个指向最终的目标字符串dest的指针。
```c
char*cdecl strncat(char * dest,const char *source,size_t count){
char *start = dest;
while (*dest)dest++;/* find end of dest string */
while (count-- && (*dest++ = *source++))/* copy string */ ;
*dest = ' 0';/* null terminate destination string */
return(start);}
strncmp
int strncmp(const char *str1, const char *str2, size_t n){
assert(str1 && str2);
while(n-- && *str1 == *str2){
if(*str1 == '\0')
return 0;
str1++;
str2++;
}
}
```
2. 字符串查找
strstr
C语言strstr函数是一个C库函数，它在一个字符串haystack中查找一个子字符串needle的第一次出现。它返回一个指向第一个匹配的指针，如果没有找到匹配则返回NULL。
strtok
strtok函数的作用是用指定的分隔符分解字符串。

strtok函数的一般形式如下：

char *strtok(char *str, const char *delim)

其中，str是要被分解的字符串，delim是包含分隔符的字符串。

strtok函数返回被分解的第一个子字符串的指针，如果没有可检索的字符串，则返回一个空指针。
# 内存操作函数
1. memcpy函数
memcpy函数是一个内存拷贝函数，它的功能是从源内存地址的起始位置开始拷贝若干个字节到目标内存地址中。1 它的函数原型是 void *memcpy(void *destin, void *source, unsigned n)，其中destin是目标内存地址，source是源内存地址，n是要拷贝的字节数。
```c
void *my_memcpy(void *dest, const void *src, size_t n){
assert(dest && src);
void *ret = dest;
while(n--){
*(char *)dest = *(char *)src;
dest = (char *)dest + 1;
src = (char *)src + 1;
}
return ret;
}
```
2. memmove函数
memmove是一个C语言库函数，它可以从一个内存位置复制指定数量的字节到另一个内存位置。它比memcpy更安全，当源区域和目标区域有重叠时，因为它可以保证在源字节被覆盖之前将它们复制到目标区域。函数的声明是：
```c
void *memmove (void *dest, const void *src, size_t n);
其中dest是一个指向目标内存位置的指针，src是一个指向源内存位置的指针，n是要复制的字节数。

要实现memmove，一种可能的方法是检查目标是在源之前还是之后，然后相应地向前或向后复制字节。下面是一个可能的实现示例：

void *_memmove (void *dest, const void *src, size_t n) {
  // 将指针转换为char类型
  char *d = (char *)dest;
  char *s = (char *)src;
  // 检查源和目标是否重叠
  if (d < s || d >= s + n) {
    // 没有重叠，使用memcpy
    while (n--) {
      *d++ = *s++;
    }
  } else {
    // 有重叠，从末尾开始复制
    d += n - 1;
    s += n - 1;
    while (n--) {
      *d-- = *s--;
    }
  }
  // 返回目标指针
  return dest;
}
```
3. memcmp函数
memcmp函数是一个C库函数，用于比较两个内存区域的前n个字节，返回它们之间的差异。

比较的方式是把每个字节都当作无符号字符来处理。

如果返回值小于0，表示第一个内存区域小于第二个内存区域。

如果返回值等于0，表示两个内存区域相等。

如果返回值大于0，表示第一个内存区域大于第二个内存区域。

memcmp函数的声明如下：
```c
int memcmp(const void *str1, const void *str2, size_t n);

其中，str1和str2是指向要比较的内存区域的指针，n是要比较的字节数
memset
memset函数是一个C库函数，用于给一段内存空间填充一个指定的值。

memset函数的声明如下：

void *memset(void *str, int c, size_t n);

其中，str是指向要填充的内存空间的指针，c是要填充的值，n是要填充的字节数。

c是一个int类型的参数，但是函数在填充内存时是使用该值的无符号字符形式。

memset函数的返回值是一个指向str的指针。

memset函数通常用于给数组或结构体进行初始化，尤其是清零操作。

例如，如果要给一个char类型的数组str赋值为0，可以使用以下语句：

memset(str, 0, sizeof(str));
```
这样就可以把str数组中的每个元素都设置为0。
# 结构体
1. 结构体类型说明
t对方无色
