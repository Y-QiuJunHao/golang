- [10 What is a Raw String Literal?](#10)
- [11 How to get the length of a utf-8 string?](#11)
- [12 Example: Banger: Yell it back!](#12)
- [13 STRINGS EXERCISES](#13)
- [14 Constants and iota](#14)
- [15 IOTA EXERCISES](#15)
- [16 Print Formatted Output](#16)
- [23 24 If Statement Else and Else If](#23)
- [25 練習](#25)
- [26 Tiny Challenge: Validate a single user](#26)
- [28 Tiny Challenge: Validate multiple users](#28)
- [31 What is a nil value?](#31)
- [32 What is an error value?](#32)
- [35 Solution: Feet to Meter](#35)
- [36 What is a Simple Statement?](#36)
# golang學習(10-21節)
## <div id='10' />10 What is a Raw String Literal?
### 印出文字使用方法
1. 使用""包起文字時,文字只能使用一行tab等需使用\t\n等處理,要顯示"時需使用\\"

       "\"我\n要\n測\n試\n\""

2. 使用\`包起文字時,文字可以收錄多行,且tab等依照輸入內容呈現.

    如下:

       `"我
       要
       測
       試"`
3. 延伸:coding路徑時也可以使用``包起較為方便
    ex:

       fmt.Println("c:\\a\\b\\c")
       fmt.Pringln(`c:\a\b\c`)

---
## <div id='11' />11 How to get the length of a utf-8 string?
### utf8文字計算問題
1. String計算使用func:

       len("XXX")

2. utf8計算:

       import("unicode/utf8")
       utf8.RuneCountInString("XXX")

使用len計算時會將文字照byte的方式計算出結果,中文會有4byte
而使用RuneCountInString時會把中文一個字算成1

---
## <div id='12' />12 Example: Banger: Yell it back!
1. 重複某個詞
   EX:輸入abc產出abc!!!

       import("strings")
       strings.Repeat("!",次數)
2. 大寫

       import("strings")
       strings.ToUpper("xxx")


    練習範例: 使用repeat功能1次但要呈現前後都加驚嘆號且改大寫

       msg := strings.ToUpper(os.Args[1])
       r := strings.Repeat("!", utf8.RuneCountInString(msg))
       s := r + msg + r
       fmt.Println(s)
---
## <div id='13' />13 STRINGS EXERCISES
1. Windows Path

       path := `c:\program files\duper super\fun.txt
       c:\program files\really\funny.png`
       fmt.Println(path)
2. Print JSON

       json := `{
           "Items": [{
               "Item": {
                   "name": "Teddy Bear"
               }
           }]
       }`
       fmt.Println(json)
3. Raw Concat

       msg := `hi ` + os.Args[1] + ` CONCATENATE-NAME-VARIABLE-HERE!
       how are you?`
       fmt.Println(msg)
4. Count the Chars

       length := len(os.Args[1])
       length := utf8.RuneCountInString(os.Args[1])
       fmt.Println(length)
5. Improved Banger

       msg := strings.ToUpper(os.Args[1])
       r := strings.Repeat("!", utf8.RuneCountInString(msg))
       s := r + msg + r
       fmt.Println(s)
6. ToLowercase

       s := strings.ToLower(os.Args[1])
       fmt.Println(s)
7. Trim It
    **strings.TrimSpace**

       msg := `

       The weather looks good.
       I should go and play.
       `

       fmt.Println(strings.TrimSpace(msg))
8. Right Trim It
    **strings.TrimRight**

       name := "inanç           "
       name = strings.TrimRight(name, " ")
       l := utf8.RuneCountInString(name)
       fmt.Println(l)
---
## <div id='14' />14 Constants and iota
### 注意:
### 1. iota只能用在宣告常數(const)時
### 2. const在定義時若第二行跟第一行變數量一樣,第二行後可省略後方文字

    const (
        a = iota
        b
        c
    )
    fmt.Println(a, b, c)

    ->0 1 2



    const (
        a = 1
        b
        c
    )
    fmt.Println(a, b, c)

    -> 1 1 1
### 3. iota在換行時才會遞增,同一行宣告兩個變數時會使用同一個

    const (
        a, a2 = 1 - iota, 2 - iota
        b = iota
        c
    )
    fmt.Println(a, a2, b, c)

    ->a a2 b c
    ->1 2 1 2


a2在a同行所以還是維持0的狀態
b已換行,所以iota跑到1

### 4. iota在再次宣告時會從0開始計

    const (
        a = iota
        b
        c
    )
    const d = iota
    fmt.Println(a, b, c, d)

    ->0 1 2 0

### 5. iota 來當狀態
可以用二位元紀錄status的方式

    type Allergen int

    const (
        IgEggs Allergen = 1 << iota // 1 << 0 which is 00000001
        IgChocolate                 // 1 << 1 which is 00000010
        IgNuts                      // 1 << 2 which is 00000100
        IgStrawberries              // 1 << 3 which is 00001000
        IgShellfish                 // 1 << 4 which is 00010000
    )

    fmt.Println(IgEggs | IgChocolate | IgShellfish)

    // output:
    // 19
    // 00010011
---
## <div id='15' />15 IOTA EXERCISES
1. Iota Months

       const (
           Nov = 11 - iota
           Oct
           Sep
       )
       fmt.Println(Sep, Oct, Nov)
2. Iota Months #2

       const (
           _ = iota
           Jan
           Feb
           Mar
       )
       fmt.Println(Jan, Feb, Mar)
3. Iota Seasons

       const (
           Spring = 3 * (1 + iota)
           Summer
           Fall
           Winter
       )

       fmt.Println(Winter, Spring, Summer, Fall)



---
## <div id='16' />16 Print Formatted Output
### 1. Println 跟 Printf差異

**Println是直接印出對應東西,且在參數前後會增加空白分隔.**

    fmt.Println("textA:",a,",textB:",b)

    output: textA: XXX ,textB: YYY

**Printf是經過定義的型態印出**

    fmt.Printf("textA: %d ,textB:%d",a,b)

    output: textA: 123 ,textB: 456

### 2. Printf使用的代碼
##### 不可將不正確型態的變數做輸出
##### EX:123abc以%d產出(X)

| 參數 | 呈現內容 | 呈現結果 | 呈現結果 |
| :--: | :------ | :------ | :--- |
| %d | 以數字呈現 | 123 | 123 |
| %s | 以字串呈現 | abctext | abctext |
| %t | true or false | true | true |
| %T | 顯示形態 | 1.23 | float64 |
| %q | 字串加上雙引號 | Google | "Google" |
| %f | 浮點數 | 1.23456 | 1.234560 |
| %.2f | 浮點數(含位數) | 1.23456 | 1.23 |
| %v | 直接將變數呈現 | abc1.23 | abc1.23 |
|  |  | flase | false |
|  |  | 12 | 12 |
| %[2]v | 取得第幾個變數顯示 | 123, 456 | 456 |

### 3. 練習(重點)




------------------------------------------

## <div id='23' />23 24 If Statement Else and Else If
使用方法

    A := 10
    B := 20
    C := true

    if A < B && C {
        fmt.Printf("?")
    } else {
        fmt.Printf("!")
    }

    if A < B && !C {
        fmt.Printf("?")
    } else if A > B {
        fmt.Printf("!")
    } else {
        fmt.Printf("*")
    }

## <div id='25' />25 練習
1. Age Seasons

       const (
           old = "Getting older"
           wis = "Getting wiser"
           adu = "Adulthood"
           blo = "Young blood"
           boo = "Booting up"
       )
       age := 22

       if age > 60 {
           fmt.Printf(old)
       } else if age > 30 {
           fmt.Printf(wis)
       } else if age > 20 {
           fmt.Printf(adu)
       } else if age > 10 {
           fmt.Printf(blo)
       } else {
           fmt.Printf(boo)
       }

2. Simplify It

       isSphere, radius := true, 200
   
       if isSphere && radius >= 200 {
           fmt.Println("It's a big sphere.")
       } else {
           fmt.Println("I don't know.")
       }
3. Arg Count

       const (
           ArgsErr = "Give me args"
           ArgsG5  = "There are %d arguments"
       )
   
       if len(os.Args) < 2 {
           fmt.Printf(ArgsErr)
       } else if len(os.Args) == 2 {
           fmt.Printf("%s", os.Args[1])
       } else if len(os.Args) == 3 {
           fmt.Printf("%s %s", os.Args[1], os.Args[2])
       } else {
           fmt.Printf(ArgsG5, len(os.Args)-1)
       }
4. Vowel or Consonant
母音:aeiou
疑似母音:wy

       const (
           ArgsErr = "Give me args"
       )
   
       if len(os.Args) < 2 || len(os.Args[1]) > 1 {
           fmt.Printf(ArgsErr)
           return
       }
       s := os.Args[1]
       if strings.IndexAny(s, "aeiou") != -1 {
           fmt.Printf("%q is a vowel.\n", s)
       } else if s == "w" || s == "y" {
           fmt.Printf("%q is sometimes a vowel, sometimes not.\n", s)
       } else {
           fmt.Printf("%q is a consonant.\n", s)
       }

---
## <div id='26' />26 Tiny Challenge: Validate a single user
驗證身分

    const (
        ArgsErr = "Usage: [username] [password]\n"
        UserErr = "Access denied for %q\n"
        PassErr = "Invalid password for %q\n"
        UserAcc = "Access granted to %q\n"

        user1 = "jack"
        pass1 = "1888"
    )

    if len(os.Args) != 3 {
        fmt.Printf(ArgsErr)
    } else {
        user, pass := os.Args[1], os.Args[2]
        if user != user1 && user != user2 {
            fmt.Printf(UserErr, user)
        } else if user == user1 && pass == pass1 {
            fmt.Printf(UserAcc, user)
        } else {
            fmt.Printf(PassErr, user)
        }
    }

---
## <div id='28' />28 Tiny Challenge: Validate multiple users

    const (
        ArgsErr = "Usage: [username] [password]\n"
        UserErr = "Access denied for %q\n"
        PassErr = "Invalid password for %q\n"
        UserAcc = "Access granted to %q\n"

        user1, user2 = "jack", "inc"
        pass1, pass2 = "1888", "1288"
    )

    if len(os.Args) != 3 {
        fmt.Printf(ArgsErr)
    } else {
        user, pass := os.Args[1], os.Args[2]
        if user != user1 && user != user2 {
            fmt.Printf(UserErr, user)
        } else if user == user1 && pass == pass1 || user == user2 && pass == pass2 {
            fmt.Printf(UserAcc, user)
        } else {
            fmt.Printf(PassErr, user)
        }
    }
---

## <div id='31' />31 What is a nil value?
**nil是go版的null**

    a = do()
    if a != nil {
        fmt.Println("error")
        return
    }
    fmt.Println("access")

---
## <div id='32' />32 What is an error value?
strconv.Atoi -> 將string轉成int
回傳為: 變數, 錯誤訊息

    tmpstr := "22307646"
    tmpint := 22307
    stoi, _ := strconv.Atoi(tmpstr)
    itos := strconv.Itoa(tmpint)
    // stoi := strconv.iota()
    fmt.Printf("stoi(%T) : %[1]d\n", stoi)
    fmt.Printf("itos(%T) : %[1]s\n", itos)
    // fmt.Printf("string(%T) :%[1]s\n", int(tmpstr))
    fmt.Printf("string(%T) :%[1]s\n", string(tmpint))

### 額外學習
strconv.Itoa -> 將int轉string
#### 不可使用強制轉型方式轉型態因上述範例輸出為以下:

    stoi(int) : 22307646
    itos(string) : 22307
    string(string) :圣
使用Itoa時是正常轉出,但string強制轉型時會變成中文(甚至是亂碼)

---
## <div id='32' />32 What is an error value?
像strconv.Atoi()會有error的回傳值,如以下

    n, error := strconv.Atoi(os.Args[1])

    fmt.Println("convert number  :", n)
    fmt.Println("error           :", err)


---
## <div id='35' />35 Solution: Feet to Meter
**學習重點**: strconv.ParseFloat(arg, 64), %g(顯示非零的浮點數)

    if len(os.Args) < 2 {
        fmt.Printf("請輸入參數\n")
        return
    }
    num := os.Args[1]
    fnum, err := strconv.ParseFloat(num, 64)
    if err != nil {
        fmt.Printf("error: %q is not number.\n", num)
        // fmt.Printf("%s", err)
        return
    }
    fmt.Printf("%g feet is %g meters", fnum, fnum * 0.3048)

---
## <div id='36' />36 What is a Simple Statement?
短判斷宣告方式:
原型:

    fnum, err := strconv.ParseFloat(num, 64)
    if err == nil {
        fmt.Printfln("num :%g ",fnum)
    }
短宣告:

    if fnum, err := strconv.ParseFloat(num, 64); err == nil {
        fmt.Printf("num :%g ",fnum)
    }
將設定參數直接塞到if跟條件式中間
這是判斷前先定義值