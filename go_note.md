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
