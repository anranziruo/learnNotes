### 最长回文字符串

### 定义
```
"回文串”是一个正读和反读都一样的字符串
```
暴力破解法
```
func longestPalindrome(str string) string {
    maxLen:=0
    maxStr:=""
    for i:=0;i<len(str);i++{
        for j:=i;j<len(str);j++{
            huiStr,isHui:=checkH(str[i:j+1])
            if isHui && len(huiStr)>maxLen{
                maxLen = len(huiStr)
                maxStr = huiStr
            }
        }
    }
    return maxStr
}

func checkH(subStr string) (string,bool){
    newStr:=""
    for i:=0;i<len(subStr);i++{
        newStr=string(subStr[i])+newStr
    }
    return subStr,newStr==subStr
}
```