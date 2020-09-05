# C++高精度

1. **高精度算法简介**
   在C++中当你用int、long long，甚至是unsigned long long 都无法处理的超级巨大数字，你会感到无比痛苦甚至到绝望，那么我们此时就只有一种方法了——高精度算法。

   高精度算法：属于处理大数字的数学计算方法，是用计算机对于超大数据的一种模拟加，减，乘，除等运算。对于非常庞大的数字无法在计算机中正常存储，于是，将这个数字拆开，拆成一位一位的存储到一个数组中， 用一个数组去表示一个数字，这样这个数字就被称为是高精度数。
2. **高精度的作用**
    正如上面所说的，高精度的作用就是对于一些异常之大的数字进行加减乘除乘方阶乘开方等运算。比如给你一道a+b的题目，读入a和b，让你输出它们的和，但a和b的范围都是小于等于10的6666次方，这个时候你就只能用高精度了。
3. **高精度读入处理数据**
   当一个数据很大的时候，我们用一个整数类型是存不下的，所以我们可以先用一个字符串输入，这样就可以输入很长的数，然后再利用字符串函数和操作运算，将每一位数取出，存入一个数组里，我们用数组里的每一位表示这个数的每一个数位。

   例如：998244353用数组储存下来，a{3,5,3,4,4,2,8,9,9}，一般是倒着存（从低位到高位，因为整数没有除个位以下的数位，但你的最高位还可以进位，那么你就又要开一个位置来存这个新的最高位）
   
   高精度读入code
   ```C++
   char s[6666];
   int a[6666];
   
   int main(){
      scanf("%s",s+1);//用字符串读入
      len=strlen(s+1);//这个数的长度为len
      for(int i=1;i<=len;i++){
         a[i]=s[len-i+1]-'0';//倒叙储存，每一位存一个数
      }
      return 0;
   }
   ```
   
   高精度输出code
   ```C++
   int a[6666]
   void write(int a[])
   {
      for(int i=lena;i>0;i--)
      {
         printf("%d",a[i]);   //一位一位地输出
      }
   }
   ```
4. **高精度比较大小**
   
   所以，高精度比较大小的步骤大致如下：

   1、比较两个数的长度，长度更长的数越大。

   2、如果两个数长度相等，那么就从高位到低位一位一位比较，如果某一位数字不同时，较大的数大。否则继续比较下一位。

   3、如果比到最后都没有比出谁大谁小，就说明这两个数一样大。

   ```C++
   //比较a和b的大小，如果a>b则返回真，否则返回假
   int a[6666],b[6666];
   
   int compare(){
      if(lena>lenb) return 1;//lena表示a这个数的长度，lenb则表示b的长度
      if(lenb>lena) return 0;//步骤1
      for(int i=lena;i>0;i--){//从高位到底位一位一位比较
         if(a[i]>b[i]) return 1;
         if(b[i]>a[i]) return 0;
      }//步骤2
      return 0;//步骤3，a=b，即a不大于b
   }
   ```
5. **高精度处理进位与借位**

   **进位**

   按照实际运算时候的进位方法，不过这里注意一下，一般储存各位数至数组时，倒着储存，防止出现10+90时，数组全往右移位的尴尬。
   ```C++
   
   int a[6666],b[6666],c[6666];
   int lena,lenb,lenc;
   
   int main(){
      lenc=max(lena,lenb);
      for(int i=1;i<=lenc;i++){
         c[i]=c[i]+a[i]+b[i];
         c[i+1]=c[i]/10;
         c[i]=c[i]%10;
      }
      while(c[lenc+1]>0) lenc++;//答案的长度有时还会增加
   }

   ```

   **借位**

   1、将当前位置的数向减。

   2、如果结果大于或等于0就直接作为当前位的答案。

   3、否则将结果加10作为当前位的答案，在将高位的数-1即可
   ```C++
   int a[6666],b[6666],c[6666];
   int lena,lenb,lenc;
   
   int main(){
      lenc=max(lena,lenb);
      for(int i=1;i<=lenc;i++){
         c[i]=c[i]+a[i]-b[i];
         if(c[i]<0) c[i]=c[i]+10,c[i+1]=-1;
      }
      while(c[lenc]==0&&lenc>1) lenc--;//细节，如果a-b结果为0，那么也要输出一个0
   }
   ```
6. **高精度加法**
```C++  
#include<cstdio>
#include<cstring>
#include<string>
#include<cmath>
#include<algorithm>
using namespace std;
int a[6666],b[6666],c[6666];
int lena,lenb,lenc;
char s1[6666],s2[6666];
 
int main(){
    scanf("%s %s",s1+1,s2+1);
    lena=strlen(s1+1);
    lenb=strlen(s2+1);
    for(int i=1;i<=lena;i++) a[i]=s1[lena-i+1]-'0';
    for(int i=1;i<=lenb;i++) b[i]=s2[lenb-i+1]-'0';
    lenc=max(lena,lenb);
    for(int i=1;i<=lenc;i++){
        c[i]=c[i]+a[i]+b[i];
        c[i+1]=c[i]/10;
        c[i]=c[i]%10;
    }
    while(c[lenc+1]>0) lenc++;
    for(int i=lenc;i>0;i--) printf("%d",c[i]);
}
```

7. **高精度减法**
```C++
#include<cstdio>
#include<cstring>
#include<string>
#include<cmath>
#include<algorithm>
using namespace std;
int a[6666],b[6666],c[6666];
int lena,lenb,lenc;
char s1[6666],s2[6666];
 
int main(){
    scanf("%s%s",s1+1,s2+1);
    lena=strlen(s1+1);
    lenb=strlen(s2+1);
    if(lenb>lena||(lena==lenb&&s2>s1)){//如果第二个数比第一个数大，那么结果是负数。
//这里比较字符串大小不能这样比较，需要再打一个函数一位一位比较，这里只是为了理解，详细比较可以看后面的代码或自己补充
    	printf("-");
    	swap(s1,s2);//swap是C++自带函数可以直接调用
    	swap(lena,lenb);//别忘了交换长度
    }
    for(int i=1;i<=lena;i++) a[i]=s1[lena-i+1]-'0';
    for(int i=1;i<=lenb;i++) b[i]=s2[lenb-i+1]-'0';
    lenc=max(lena,lenb);
    for(int i=1;i<=lenc;i++){
        c[i]=c[i]+a[i]-b[i];
        if(c[i]<0) c[i]=c[i]+10,c[i+1]=-1;
    }
    while(c[lenc]==0&&lenc>1) lenc--;
    for(int i=lenc;i>0;i--) printf("%d",c[i]);
}
```

8. **高精度乘法**
   1. **高精度乘以单精度**
      
      ```C++
      #include<cstdio>
      #include<cstring>
      #include<string>
      #include<cmath>
      #include<algorithm>
      using namespace std;
      int a[6666],b;
      int lena;
      char s1[6666];
      
      int main(){
         scanf("%s%d",s1+1,&b);
         lena=strlen(s1+1);
         for(int i=1;i<=lena;i++) a[i]=s1[lena-i+1]-'0';
         for(int i=1;i<=lena;i++) a[i]*=b;
         for(int i=1;i<=lena;i++){
            a[i+1]+=a[i]/10;
            a[i]%=10;
         }
         while(a[lena+1]>0) lena++;
         for(int i=lena;i>0;i--) printf("%d",a[i]);
      }

      ```
   2. **高精度乘以高精度**

     ```C++
      #include<cstdio>
      #include<cstring>
      #include<string>
      #include<cmath>
      #include<algorithm>
      using namespace std;
      int a[6666],b[6666],c[6666];
      int lena,lenb,lenc;
      char s1[6666],s2[6666];
      
      int main(){
         scanf("%s%s",s1+1,s2+1);
         lena=strlen(s1+1);
         lenb=strlen(s2+1);
         for(int i=1;i<=lena;i++) a[i]=s1[lena-i+1]-'0';
         for(int i=1;i<=lenb;i++) b[i]=s2[lenb-i+1]-'0';
         lenc=lena+lenb-1;
         for(int i=1;i<=lena;i++){
            for(int j=1;j<=lenb;j++){
               c[i+j-1]+=a[i]*b[j];
               c[i+j]+=c[i+j-1]/10;
               c[i+j-1]%=10;
            }
         }
         while(c[lenc+1]>0) lenc++;
         for(int i=lenc;i>0;i--) printf("%d",c[i]);
      }

     ```
9. **高精度除法**
   1. **高精度除以单精度**
      ```C++
      #include<cstdio>
      #include<cstring>
      #include<string>
      #include<cmath>
      #include<algorithm>
      using namespace std;
      int a[6666],b,r;
      int lena;
      char s1[6666];
      
      int main(){
         scanf("%s %d",s1+1,&b);
         lena=strlen(s1+1);
         for(int i=1;i<=lena;i++) a[i]=s1[lena-i+1]-'0';
         for(int i=lena;i>0;i--){
            r=r*10+a[i];
            a[i]=r/b;
            r=r%b;
         }
         while(a[lena]==0&&lena>1) lena--;
         for(int i=lena;i>0;i--) printf("%d",a[i]);
      }
      ```
   2. **高精度除以高精度**
      ```C++
      #include<cstdio>
      #include<cstring>
      #include<string>
      #include<cmath>
      #include<algorithm>
      using namespace std;
      int a[6666],b[6666],ans[6666],t[6666],l[6666],r[6666],mid[6666];
      char s1[6666],s2[6666];
      
      int compare(int a[],int b[]){
         if(a[0]>b[0]) return 0;
         if(a[0]<b[0]) return 1;
         for(int i=a[0];i>0;i--){
            if(a[i]>b[i]) return 0;
            if(a[i]<b[i]) return 1;
         }
         return 1;
      }
      
      void add(){
         mid[0]=max(l[0],r[0]);
         for(int i=1;i<=mid[0];i++) mid[i]=l[i]+r[i];
         while(mid[mid[0]+1]>0) mid[0]++;
         for(int i=1;i<=mid[0];i++){
            mid[i+1]+=mid[i]/10;
            mid[i]%=10;
         }
         while(mid[mid[0]+1]>0) mid[0]++;
      }
      
      void times(){
         memset(t,0,sizeof(t));
         t[0]=b[0]+mid[0]-1;
         for(int i=1;i<=b[0];i++){
            for(int j=1;j<=mid[0];j++){
                  t[i+j-1]+=b[i]*mid[j];
                  t[i+j]+=t[i+j-1]/10;
                  t[i+j-1]%=10;
            }
         }
         while(t[t[0]+1]>0) t[0]++;
      }
      
      void div(){
         int r=0;
         for(int i=mid[0];i>0;i--){
            r=r*10+mid[i];
            mid[i]=r/2;
            r%=2;
         }
         while(mid[mid[0]]==0) mid[0]--;
      }
      
      void left(){
         for(int i=0;i<=mid[0];i++) l[i]=ans[i]=mid[i];
         l[1]++;
         for(int i=1;i<=l[0];i++){
            l[i+1]+=l[i]/10;
            l[i]%=10;
         }
      }
      
      void right(){
         for(int i=0;i<=mid[0];i++) r[i]=mid[i];
         r[1]--;
         for(int i=1;i<=r[0];i++){
            if(r[i]<0){
                  r[i+1]--;
                  r[i]+=10;
            }
         }
      }
      
      int main(){
         scanf("%s %s",s1+1,s2+1);
         a[0]=r[0]=strlen(s1+1);
         b[0]=strlen(s2+1);
         for(int i=1;i<=a[0];i++) a[i]=r[i]=s1[a[0]-i+1]-'0';
         for(int i=1;i<=b[0];i++) b[i]=s2[b[0]-i+1]-'0';
         l[0]=ans[0]=1;
         while(compare(l,r)){
            add();
            div();
            times();
            if(compare(t,a)) left();
            else right();
         }
         for(int i=ans[0];i>0;i--) printf("%d",ans[i]);
         return 0;
      }
      ```
10. **高精度开平方**
    ```C++
   #include<cstdio>
   #include<cstring>
   #include<string>
   #include<cmath>
   #include<algorithm>
   using namespace std;
   int a[6666],b[6666],ans[6666],t[6666],l[6666],r[6666],mid[6666];
   char s1[6666],s2[6666];
   
   int compare(int a[],int b[]){
      if(a[0]>b[0]) return 0;
      if(a[0]<b[0]) return 1;
      for(int i=a[0];i>0;i--){
         if(a[i]>b[i]) return 0;
         if(a[i]<b[i]) return 1;
      }
      return 1;
   }
   
   void add(){
      mid[0]=max(l[0],r[0]);
      for(int i=1;i<=mid[0];i++) mid[i]=l[i]+r[i];
      while(mid[mid[0]+1]>0) mid[0]++;
      for(int i=1;i<=mid[0];i++){
         mid[i+1]+=mid[i]/10;
         mid[i]%=10;
      }
      while(mid[mid[0]+1]>0) mid[0]++;
   }
   
   void times(){
      memset(t,0,sizeof(t));
      t[0]=mid[0]+mid[0]-1;
      for(int i=1;i<=mid[0];i++){
         for(int j=1;j<=mid[0];j++){
               t[i+j-1]+=mid[i]*mid[j];
               t[i+j]+=t[i+j-1]/10;
               t[i+j-1]%=10;
         }
      }
      while(t[t[0]+1]>0) t[0]++;
   }
   
   void div(){
      int r=0;
      for(int i=mid[0];i>0;i--){
         r=r*10+mid[i];
         mid[i]=r/2;
         r%=2;
      }
      while(mid[mid[0]]==0) mid[0]--;
   }
   
   void left(){
      for(int i=0;i<=mid[0];i++) l[i]=ans[i]=mid[i];
      l[1]++;
      for(int i=1;i<=l[0];i++){
         l[i+1]+=l[i]/10;
         l[i]%=10;
      }
   }
   
   void right(){
      for(int i=0;i<=mid[0];i++) r[i]=mid[i];
      r[1]--;
      for(int i=1;i<=r[0];i++){
         if(r[i]<0){
               r[i+1]--;
               r[i]+=10;
         }
      }
   }
   
   int main(){
      scanf("%s",s1+1);
      a[0]=r[0]=strlen(s1+1);
      for(int i=1;i<=a[0];i++) a[i]=r[i]=s1[a[0]-i+1]-'0';
      l[0]=ans[0]=1;
      while(compare(l,r)){
         add();
         div();
         times();
         if(compare(t,a)) left();
         else right();
      }
      for(int i=ans[0];i>0;i--) printf("%d",ans[i]);
      return 0;
   }
    ```
11. **高精度开n次方**
    ```C++
   #include<cstdio>
   #include<cstring>
   #include<string>
   #include<cmath>
   #include<algorithm>
   using namespace std;
   int n,a[6666],b[6666],ans[6666],t[6666],l[6666],r[6666],mid[6666];
   char s1[6666];
   
   int compare(int a[],int b[]){
      if(a[0]>b[0]) return 0;
      if(a[0]<b[0]) return 1;
      for(int i=a[0];i>0;i--){
         if(a[i]>b[i]) return 0;
         if(a[i]<b[i]) return 1;
      }
      return 1;
   }
   
   void add(){
      mid[0]=max(l[0],r[0]);
      for(int i=1;i<=mid[0];i++) mid[i]=l[i]+r[i];
      while(mid[mid[0]+1]>0) mid[0]++;
      for(int i=1;i<=mid[0];i++){
         mid[i+1]+=mid[i]/10;
         mid[i]%=10;
      }
      while(mid[mid[0]+1]>0) mid[0]++;
   }
   
   void times(){
      for(int i=0;i<=mid[0];i++) b[i]=mid[i];
      for(int k=1;k<=n-1;k++){
         memset(t,0,sizeof(t));
         t[0]=b[0]+mid[0]-1;
         for(int i=1;i<=b[0];i++){
               for(int j=1;j<=mid[0];j++){
                  t[i+j-1]+=b[i]*mid[j];
                  t[i+j]+=t[i+j-1]/10;
                  t[i+j-1]%=10;
               }
         }
         while(t[t[0]+1]>0) t[0]++;
         for(int i=0;i<=t[0];i++) b[i]=t[i];
      }
   }
   
   void div(){
      int r=0;
      for(int i=mid[0];i>0;i--){
         r=r*10+mid[i];
         mid[i]=r/2;
         r%=2;
      }
      while(mid[mid[0]]==0) mid[0]--;
   }
   
   void left(){
      for(int i=0;i<=mid[0];i++) l[i]=ans[i]=mid[i];
      l[1]++;
      for(int i=1;i<=l[0];i++){
         l[i+1]+=l[i]/10;
         l[i]%=10;
      }
   }
   
   void right(){
      for(int i=0;i<=mid[0];i++) r[i]=mid[i];
      r[1]--;
      for(int i=1;i<=r[0];i++){
         if(r[i]<0){
               r[i+1]--;			
               r[i]+=10;
         }
      }
   }
   
   int main(){
      scanf("%d\n",&n);
      scanf("%s",s1+1);
      a[0]=r[0]=strlen(s1+1);
      for(int i=1;i<=a[0];i++) a[i]=r[i]=s1[a[0]-i+1]-'0';
      l[0]=ans[0]=1;
      while(compare(l,r)){
         add();
         div();
         times();
         if(compare(t,a)) left();
         else right();
      }
      for(int i=ans[0];i>0;i--) printf("%d",ans[i]);
      return 0;
   }
    ```