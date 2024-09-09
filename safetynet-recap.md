# Congnitive Complexity
```c
// ............................ABCDEFGHIJKLMNOPQRSTUVWXYZ
static const char* mappings = "01230120022455012623010202";

```

# Good Spread Of  Functionality
https://github.com/tts-tcq-2024/build-safety-net-in-cpp-mani195/blob/main/Soundex.h
https://github.com/tts-tcq-2024/build-safety-net-in-java-SindhuRdk

# Fat Interface 
https://github.com/tts-tcq-2024/build-safety-net-in-cpp-mani195/blob/main/Soundex.h

# Good Spread of TestCases (Optimized)
https://github.com/tts-tcq-2024/build-safety-net-in-cpp-Harish-C-15/blob/main/Soundex.Tests.cpp

# Value Parameetrized Test
https://github.com/tts-tcq-2024/build-safety-net-in-cpp-JayanthAkash/blob/main/Soundex.Tests.cpp

# Switch Case Reduction
```C++
 static const std::unordered_map<char, char> soundexCodes {
        {'B', '1'}, {'F', '1'}, {'P', '1'}, {'V', '1'},
        {'C', '2'}, {'G', '2'}, {'J', '2'}, {'K', '2'}, {'Q', '2'},
        {'S', '2'}, {'X', '2'}, {'Z', '2'},
        {'D', '3'}, {'T', '3'},
        {'L', '4'},
        {'M', '5'}, {'N', '5'},
        {'R', '6'}
    };
```
```c
char getSoundexCode(char c) {
    c = toupper(c);
    static const char lookupTable[26] = {
        '0', '1', '2', '3', '0', '1', '2', '0', // A, B, C, D, E, F, G, H
        '0', '2', '2', '4', '5', '5', '0', '1', // I, J, K, L, M, N, O, P
        '2', '6', '2', '3', '0', '1', '0', '2', // Q, R, S, T, U, V, W, X
        '0', '2'                                // Y, Z
    };

    
    return lookupTable[c-65]; 
}


```
## Chain Of Responsibility

``` C
char R(char c){
    if(c=='R'){
        return '6';
    }
    return '0';
}

char MN(char c){
    if(c=='N' || c=='M'){
        return '5';
    }
    return R(c);
}

char L(char c){
    if(c=='L'){
        return'4';
    }
   return MN(c);
}

char DT(char c){
    if(c=='D' || c=='T'){
        return '3';
    }
    return L(c);
}

char CGKKQSXZ(char c){
    char ch[] = {'C','G','J','K','Q','S','X','Z'};
    for(int i=0; i<8;i++){
        if(c==ch[i]){
            return '2';
        }
    }
    return  DT(c);
}

char BFPV(char c){
    char ch[]={'B','F','P','V'};
    for(int i=0;i<4;i++){
        if(c==ch[i]){
            return '1';
        }
    }
    return CGKKQSXZ(c);
}

```
