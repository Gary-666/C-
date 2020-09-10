# 质数的求解方法

### 质数的思路

1. **直接试除**  
   
   很挫的方法，直接用循环全部来搞!

2. **一点优化**

   除2之外不需要试除偶数

   ```C++
    bool is_prime2(unsigned long long n) { //middle
        long long stop = sqrt(n) + 1;
        if (n == 2) {
            return 1;
        }
        if (n % 2 == 0) {
            return 0;
        }
        for (int i = 3; i <= stop; i += 2) {
            if (n % i == 0) {
                return 0;
            }
        }
        return 1;
    }

   ```
3. **6n+1**
  
   如果n是一个质数(不为2或3),那么:
     
     n=6n+1

   那么n%2!=0且n%3!=0

 ```C++
    bool is_prime(unsigned long long n) { //quick
        unsigned long long stop = n / 6 + 1, Tstop = sqrt(n) + 5;
        if (n == 2 || n == 3 || n == 5 || n == 7 || n == 11) {
            return 1;
        }
        if (n % 2 == 0 || n % 3 == 0 || n % 5 == 0 || n == 1) {
            return 0;
        }
        for (unsigned long long i = 1; i <= stop; i++) {
            if (i * 6 >= Tstop) {break;}
            if ((n % (i * 6 + 1) == 0) || (n % (i * 6 + 5) == 0)) {
                return 0;
            }
        }
        return 1;
    }
 ```
