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
- [38 Famous Shadowing Gotcha](#38)
- [41 42 43 44 Learn the Switch Statement](#41)
- [45 46 47 Learn the Switch Statement 2](#45)
- [49 If vs Switch: Which one to use?](#49)
- [50 ★ SWITCH EXERCISES ★](#50)
- [52. 53. 54. 55. Create a multiplication table](#55)
- [56. 57. For Range: Learn the easy way!](#57)
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

---
## <div id='38' />38 Famous Shadowing Gotcha

if條件中定義賦予值的參數為 **區域變數** ,條件終止後不會有效

    if fnum, err := strconv.ParseFloat(num, 64); err != nil {
        fmt.Printf("error")
    }
    fmt.Printf("%f", fnum)

    output:
    undefined: fnum
但若有先宣告並且不是使用宣告並賦予(:=),而只有賦予值(=), 則值會被存下

    var (
        fnum float64
        err  error
    )
    if fnum, err = strconv.ParseFloat(num, 64); err != nil {
        fmt.Printf("error")
    }
    fmt.Printf("%f", fnum)

    output:
    11.000000

---
## <div id='38' />38 Famous Shadowing Gotcha
### movie ratings

    if len(os.Args) != 2 {
        fmt.Printf("Arg need")
    } else if n, err := strconv.Atoi(os.Args[1]); err != nil {
        fmt.Printf("Requires age")
    } else if n < 0 {
        fmt.Printf("Wrong age: %q", os.Args[1])
    } else if n > 17 {
        fmt.Println("R-Rated")
    } else if n < 13 {
        fmt.Println("PG-Rated")
    } else {
        fmt.Println("PG-13")
    }

解答:

    if len(os.Args) != 2 {
        fmt.Println("Requires age")
        return
    }

    age, err := strconv.Atoi(os.Args[1])

    if err != nil || age < 0 {
        fmt.Printf(`Wrong age: %q`+"\n", os.Args[1])
        return
    } else if age > 17 {
        fmt.Println("R-Rated")
    } else if age >= 13 && age <= 17 {
        fmt.Println("PG-13")
    } else if age < 13 {
        fmt.Println("PG-Rated")
    }
解答2:

    if len(os.Args) != 2 {
        fmt.Println("Requires age")
        return
    } else if age, err := strconv.Atoi(os.Args[1]); err != nil || age < 0 {
        fmt.Printf(`Wrong age: %q`+"\n", os.Args[1])
        return
    } else if age > 17 {
        fmt.Println("R-Rated")
    } else if age >= 13 && age <= 17 {
        fmt.Println("PG-13")
    } else if age < 13 {
        fmt.Println("PG-Rated")
    }

### Odd or Even

    const (
        non  = "Pick a number"
        num  = "%q is not a number"
        even = "%d is an even number"
        odd  = "%d is an odd number"
        gb8  = "%d is an even number and it's divisible by 8"
    )
    var (
        number int
        err    error
    )
    if len(os.Args) != 2 {
        fmt.Printf(non)
        return
    } else if number, err = strconv.Atoi(os.Args[1]); err != nil {
        fmt.Printf(num, os.Args[1])
        return
    }
    if number%8 == 0 {
        fmt.Printf(gb8, number)
    } else if number%2 == 0 {
        fmt.Printf(even, number)
    } else {
        fmt.Printf(odd, number)
    }

解答:

    if len(os.Args) != 2 {
        fmt.Println("Pick a number")
        return
    }

    n, err := strconv.Atoi(os.Args[1])
    if err != nil {
        fmt.Printf("%q is not a number\n", os.Args[1])
        return
    }

    if n%8 == 0 {
        fmt.Printf("%d is an even number and it's divisible by 8\n", n)
    } else if n%2 == 0 {
        fmt.Printf("%d is an even number\n", n)
    } else {
        fmt.Printf("%d is an odd number\n", n)
    }

### leap-year
閏年規則:
每四年一潤,但整除100不算,或整除四百閏年
- 西元年份除以4不可整除，為平年。
- 西元年份除以4可整除，且除以100不可整除，為閏年。
- 西元年份除以100可整除，且除以400不可整除，為平年
- 西元年份除以400可整除，為閏年。

所以規則如下

    const (
        non  = "Give me a year number"
        num  = "%q is not a valid year."
        year = "%d is%s a leap year."
    )
    if len(os.Args) != 2 {
        fmt.Printf(non)
        return
    }
    if number, err := strconv.Atoi(os.Args[1]); err != nil {
        fmt.Printf(num, os.Args[1])
    } else if number%4 == 0 && number%100 != 0 || number%400 == 0 {
        fmt.Printf(year, number, "")
    } else {
        fmt.Printf(year, number, " not")
    }

解答:

    if len(os.Args) != 2 {
        fmt.Println("Give me a year number")
        return
    }

    year, err := strconv.Atoi(os.Args[1])
    if err != nil {
        fmt.Printf("%q is not a valid year.\n", os.Args[1])
        return
    }

    // Notice that:
    // I've intentionally created this solution as verbose
    // as I can.
    //
    // See the next exercise.

    var leap bool
    if year%400 == 0 {
        leap = true
    } else if year%100 == 0 {
        leap = false
    } else if year%4 == 0 {
        leap = true
    }

    if leap {
        fmt.Printf("%d is a leap year.\n", year)
    } else {
        fmt.Printf("%d is not a leap year.\n", year)
    }
### Days in a Month

    const (
        day = "%q has %d days."
    )
    year := time.Now().Year()
    mon2 := 28
    if year%4 == 0 && year%100 != 0 || year%400 == 0 {
        mon2 = 29
    }
    if len(os.Args) != 2 {
        fmt.Printf("Give me a month name")
        return
    }
    mon := strings.ToLower(os.Args[1])
    if mon != "january" && mon != "february" && mon != "march" && mon != "april" && mon != "may" && mon != "june" && mon != "july" && mon != "august" && mon != "september" && mon != "october" && mon != "november" && mon != "december" {
        fmt.Printf("%q is not a month.", mon)
        return
    }
    if mon == "january" ||
        mon == "march" ||
        mon == "may" ||
        mon == "july" ||
        mon == "august" ||
        mon == "october" ||
        mon == "december" {
        fmt.Printf(day, os.Args[1], 31)
    } else if mon == "february" {
        fmt.Printf(day, os.Args[1], mon2)
    } else {
        fmt.Printf(day, os.Args[1], 30)
    }

解答:

    if len(os.Args) != 2 {
        fmt.Println("Give me a month name")
        return
    }

    // get the current year and find out whether it's a leap year
    year := time.Now().Year()
    leap := year%4 == 0 && (year%100 != 0 || year%400 == 0)

    // setting it to 28, saves me typing it below again
    days := 28

    month := os.Args[1]

    // case insensitive
    if m := strings.ToLower(month); m == "april" ||
        m == "june" ||
        m == "september" ||
        m == "november" {
        days = 30
    } else if m == "january" ||
        m == "march" ||
        m == "may" ||
        m == "july" ||
        m == "august" ||
        m == "october" ||
        m == "december" {
        days = 31
    } else if m == "february" {
        // try a "logical and operator" above.
        // like: `else if m == "february" && leap`
        if leap {
            days = 29
        }
    } else {
        fmt.Printf("%q is not a month.\n", month)
        return
    }

    fmt.Printf("%q has %d days.\n", month, days)

---
## <div id='41' />41 42 43 44 Learn the Switch Statement 1
1. 不用書寫break
2. 若判斷式在case中switch條件可寫為true(或省略)
3. 若不是條件式中則用default

以下程式


    T := time.Now().Hour()
    switch {
    case T >= 18:
        fmt.Printf("good evening")
    case T >= 12:
        fmt.Printf("good afternoon")
    case T >= 6:
        fmt.Printf("good morning")
    default:
        fmt.Printf("good night")
    }

---
## <div id='45' />45 46 47 Learn the Switch Statement 2
1. 同樣可使用短宣告寫在switch中
2. 若要繼續執行(不使用原break功能),使用fallthrough

以下程式fallthrough範例

    switch i := 10; {
    case i > 100:
        fmt.Printf("big ")
        fallthrough
    case i > 0:
        fmt.Printf("positive ")
        fallthrough
    default:
        fmt.Printf("number")
    }

    output:
    i=124
    big positive number
    
    i=80
    positive number
    
    i=0
    number

以下挑戰項目

    switch T := time.Now().Hour(); {
    case T > 17:
        fmt.Printf("good evening")
    case T > 12:
        fmt.Printf("good afternoon")
    case T > 5:
        fmt.Printf("good morning")
    case T > 0:
        fmt.Printf("good night")
    }

答案:

    switch T := time.Now().Hour(); {
    case T >= 18:
        fmt.Printf("good evening")
    case T >= 12:
        fmt.Printf("good afternoon")
    case T >= 6:
        fmt.Printf("good morning")
    default:
        fmt.Printf("good night")
    }

---
## <div id='49' />49 If vs Switch: Which one to use?

當if else條件偏多時,可考慮使用switch case做,程式碼會比較少

    if m:=os.Args[1];m=="Dec" || m=="Jan" || m=="Feb" {
        fmt.Println("Winter")
    } else if m=="Mar" || m=="Apr" || m=="May" {
        fmt.Println("Spring")
    } else if m=="Jun" || m=="Jul" || m=="Aug" {
        fmt.Println("Summer")
    } else if m=="Sep" || m=="Oct" || m=="Nov" {
        fmt.Println("Fall")
    } else {
        fmt.Printf("%q is not a month\n", m)
    }

switch版

    switch m:=os.Args[1];m {
    case "Dec", "Jan", "Feb":
        fmt.Println("Winter")
    case "Mar", "Apr", "May":
        fmt.Println("Spring")
    case "Jun", "Jul", "Aug":
        fmt.Println("Summer")
    case "Sep", "Oct", "Nov":
        fmt.Println("Fall")
    default:
        fmt.Printf("%q is not a month\n", m)
    }

---
## <div id='50' />50 ★ SWITCH EXERCISES ★

### Richter Scale
    if len(os.Args) != 2 {
        fmt.Println("Give me the magnitude of the earthquake.")
        return
    }
    n, err := strconv.ParseFloat(os.Args[1], 64)
    if err != nil {
        fmt.Println("I couldn't get that, sorry.")
        return
    }
    switch {
    case n < 2.0:
        fmt.Printf("%.2f is micro")
    case n >= 2.0 && n < 3.0:
        fmt.Printf("%.2f is very minor", n)
    case n >= 3.0 && n < 4.0:
        fmt.Printf("%.2f is minor", n)
    case n >= 4.0 && n < 5.0:
        fmt.Printf("%.2f is light", n)
    case n >= 5.0 && n < 6.0:
        fmt.Printf("%.2f is moderate", n)
    case n >= 6.0 && n < 7.0:
        fmt.Printf("%.2f is strong", n)
    case n >= 7.0 && n < 8.0:
        fmt.Printf("%.2f is major", n)
    case n >= 8.0 && n < 10.0:
        fmt.Printf("%.2f is great", n)
    case n >= 10.0:
        fmt.Printf("%.2f is massive", n)
    }

解答:

    args := os.Args
    if len(args) != 2 {
        fmt.Println("Give me the magnitude of the earthquake.")
        return
    }

    richter, err := strconv.ParseFloat(args[1], 64)
    if err != nil {
        fmt.Println("I couldn't get that, sorry.")
        return
    }

    var desc string

    switch r := richter; {
    case r < 2:
        desc = "micro"
    case r >= 2 && r < 3:
        desc = "very minor"
    case r >= 3 && r < 4:
        desc = "minor"
    case r >= 4 && r < 5:
        desc = "light"
    case r >= 5 && r < 6:
        desc = "moderate"
    case r >= 6 && r < 7:
        desc = "strong"
    case r >= 7 && r < 8:
        desc = "major"
    case r >= 8 && r < 10:
        desc = "great"
    default:
        desc = "massive"
    }

    fmt.Printf("%.2f is %s\n", richter, desc)

### Richter Scale #2

    if len(os.Args) != 2 {
        fmt.Println("Tell me the magnitude of the earthquake in human terms.")
        return
    }
    var word string
    n := strings.ToLower(os.Args[1])
    switch n {
    case "micro":
        word = "%s's richter scale is less than 2.0"
    case "very minor":
        word = "%s's richter scale is 2 - 2.9"
    case "minor":
        word = "%s's richter scale is 3 - 3.9"
    case "light":
        word = "%s's richter scale is 4 - 4.9"
    case "moderate":
        word = "%s's richter scale is 5 - 5.9"
    case "strong":
        word = "%s's richter scale is 6 - 6.9"
    case "major":
        word = "%s's richter scale is 7 - 7.9"
    case "great":
        word = "%s's richter scale is 8 - 9.9"
    case "massive":
        word = "%s's richter scale is 10+"
    default:
        word = "%s's richter scale is unknown"
    }
    fmt.Printf(word, n)

解答:

    args := os.Args
    if len(args) != 2 {
        fmt.Println("Tell me the magnitude of the earthquake in human terms.")
        return
    }

    var richter string

    desc := args[1]

    switch desc {
    case "micro":
        richter = "less than 2.0"
    case "very minor":
        richter = "2 - 2.9"
    case "minor":
        richter = "3 - 3.9"
    case "light":
        richter = "4 - 4.9"
    case "moderate":
        richter = "5 - 5.9"
    case "strong":
        richter = "6 - 6.9"
    case "major":
        richter = "7 - 7.9"
    case "great":
        richter = "8 - 9.9"
    case "massive":
        richter = "10+"
    default:
        richter = "unknown"
    }

    fmt.Printf(
        "%s's richter scale is %s\n",
        desc, richter,
    )

### Convert

    const (
        usage       = "Usage: [username] [password]"
        errUser     = "Access denied for %q.\n"
        errPwd      = "Invalid password for %q.\n"
        accessOK    = "Access granted to %q.\n"
        user, user2 = "jack", "inanc"
        pass, pass2 = "1888", "1879"
    )

    args := os.Args

    if len(args) != 3 {
        fmt.Println(usage)
        return
    }

    u, p := args[1], args[2]

    //
    // REFACTOR THIS TO A SWITCH
    //

    switch {
    case u != user && u != user2:
        fmt.Printf(errUser, u)
    case u == user && p == pass, u == user2 && p == pass2:
        fmt.Printf(accessOK, u)
    default:
        fmt.Printf(errPwd, u)
    }

解答:

    const (
        usage       = "Usage: [username] [password]"
        errUser     = "Access denied for %q.\n"
        errPwd      = "Invalid password for %q.\n"
        accessOK    = "Access granted to %q.\n"
        user, user2 = "jack", "inanc"
        pass, pass2 = "1888", "1879"
    )

    args := os.Args

    if len(args) != 3 {
        fmt.Println(usage)
        return
    }

    u, p := args[1], args[2]

    //
    // REFACTOR THIS TO A SWITCH
    //

    switch {
    case u != user && u != user2:
        fmt.Printf(errUser, u)
    case u == user && p == pass:
        // notice this one (no more duplication)
        fallthrough
    case u != user2 && p == pass2:
        fmt.Printf(accessOK, u)
    default:
        fmt.Printf(errPwd, u)
    }

### String Manipulator

    if len(os.Args) != 3 {
        fmt.Println("[command] [string]\n\nAvailable commands: lower, upper and title")
        return
    }
    s := os.Args[2]
    switch t := os.Args[1]; t {
    case "lower":
        s = strings.ToLower(s)
        fmt.Println(s)
    case "upper":
        s = strings.ToUpper(s)
        fmt.Println(s)
    case "title":
        s = strings.Title(s)
        fmt.Println(s)
    default:
        fmt.Printf("Unknown command: %q", t)

    }

解答:

    const usage = `[command] [string]

    Available commands: lower, upper and title`

    args := os.Args
    if len(args) != 3 {
        fmt.Println(usage)
        return
    }

    cmd, str := args[1], args[2]
    switch cmd {
    case "lower":
        fmt.Println(strings.ToLower(str))
    case "upper":
        fmt.Println(strings.ToUpper(str))
    case "title":
        fmt.Println(strings.Title(str))
    default:
        fmt.Printf("Unknown command: %q\n", cmd)
    }

### Days in a Month

解答


    if len(os.Args) != 2 {
        fmt.Println("Give me a month name")
        return
    }

    year := time.Now().Year()
    leap := year%4 == 0 && (year%100 != 0 || year%400 == 0)

    days, month := 28, os.Args[1]

    switch strings.ToLower(month) {
    case "april", "june", "september", "november":
        days = 30
    case "january", "march", "may", "july",
        "august", "october", "december":
        days = 31
    case "february":
        if leap {
            days = 29
        }
    default:
        fmt.Printf("%q is not a month.\n", month)
        return
    }

    fmt.Printf("%q has %d days.\n", month, days)

---
## <div id='55' />52. 53. 54. 55. Create a multiplication table

for判斷式:

    for i := 1; i <= max; i++ {
        XXX
    }

預設值 ; 判斷值 ; 累進值
或寫作while方式

    for {
        XXX
    }

跳出:

    break

繼續:

    continue

小測驗:

    if len(os.Args) != 2 {
        fmt.Println("[number]")
    }
    max, err := strconv.Atoi(os.Args[1])
    if err != nil {
        fmt.Println("error number")
    }
    fmt.Printf("%5s", "X")
    for i := 1; i <= max; i++ {
        fmt.Printf("%5d", i)
    }
    fmt.Println("")
    for i := 1; i <= max; i++ {
        fmt.Printf("%5d", i)
        for j := 1; j <= max; j++ {
            fmt.Printf("%5d", i*j)
        }
        fmt.Println("")
    }

---
## <div id='57' />56. 57. For Range: Learn the easy way!

strings.Fields(string)可以直接將字串分成陣列(英文格式,以空白分割)
且從0開始編號

    words:=strings.Fields(os.Args[1])
    for i:=0;i<len(words);i++ {
        fmt.Printf("%s", words[i])
    }

range方式取值回傳index跟值

    for i, v:= range os.Args {

    }

os.Args[1:]建議是這樣寫
從1開始(0是程式位置,1是值)