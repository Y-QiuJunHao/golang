# golang學習(10-21節)
## 10 What is a Raw String Literal?
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
## 11 How to get the length of a utf-8 string?
### utf8文字計算問題
1. String計算使用func:

       len("XXX")

2. utf8計算:

       import("unicode/utf8")
       utf8.RuneCountInString("XXX")

使用len計算時會將文字照byte的方式計算出結果,中文會有4byte
而使用RuneCountInString時會把中文一個字算成1

---
## 12 Example: Banger: Yell it back!
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
## 13 STRINGS EXERCISES
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
## 14 Constants and iota
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
## 15 IOTA EXERCISES
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
## 16 Print Formatted Output
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