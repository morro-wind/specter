**解釋器**：每次處理一小部分程序，交替讀入代碼行並進行運算。
**編譯器**：高級語言程序成爲源代碼，編譯而成的程序成爲目標代碼或可執行代碼。
解釋執行的兩種方式：**交互模式**和 **腳本模式**。

**程序**是指一組定義如何進行計算的指令的集合。
3種類型錯誤： **語法錯誤**、 **運行時錯誤**和 **語義錯誤**。
**語法**指的是程序的結構以及此結構的規則。
運行時錯誤，這種錯誤只有在程序運行後才會出現。常被稱爲異常。
語義錯誤，不產生錯誤信息，不執行正確的邏輯。問題在於寫出的代碼和想要寫的代碼並不一致。

夏洛克·福爾摩斯所說：“當你排除掉所有的可能性，那麼剩下的，不管多麼不可能，必定是真相。”（柯南·道爾《四籤名》）

編程正是不斷調試修改直到程序達到設計目的的過程。這種想法的要旨是，應該從一個能做某些事的程序開始，然後做一點點修改，並調試修改，如此迭代，以確保總是有一個可以運行的程序。

自然語言是指人們所說的語言。自然演化而來。
形式語言則是人們設計出來用於特別用途的語言。
編程語言是人們設計出來用來表達計算過程的形式語言。
形式語言傾向於對語法作出嚴格的限制。
語法規則有兩種，分別適用於**記號(token)**和 **結構**。記號是語言的基本元素，比如詞、數字和化學元素。


語法分析：弄清句子的結構是什麼。

## 術語表
問題解決(problem solving): 總結問題、尋找解決方案以及表達解決方案的過程。
高級語言(high-level language): 設計來方便人們讀寫的編程語言。
低級語言(low-level language): 設計來方便計算機執行的編程語言，也被成爲“機器語言”或“匯編語言”。
可移植性(portability): 程序的一種屬性：可以在多種類型的計算機上運行。
解釋(interpret): 按照一行一行解釋翻譯的方式來執行高級語言編寫的程序。
編譯(compile): 一次性將一個高級語言編寫的程序翻譯爲低級語言程序，之後可以單獨運行。
源代碼(source code): 使用高級語言編寫的程序，在編譯之前稱爲源代碼。
目標代碼(object code): 編譯器輸出的程序。
可執行文件(executable): 目標代碼的另一個名字，表示它可以直接被執行。
提示符(prompt): 解釋器顯示出來的文字，用來表示它準備好接收用戶新的輸入。
腳本(script): 保存在文件中的程序（用於被解釋器解釋執行）。
交互模式(interactive mode): 使用Python 解釋器的一種方式。在解釋器的提示處輸入命令和表達式。
腳本模式(script mode): 使用Python 解釋器的另一種方式。讀取並執行一個腳本文件中的代碼。
程序(program): 一系列代碼指令的集合，指定一種運算。
算法(algorithm): 解決某一類問題的通用運算流程。
bug: 程序中的一個錯誤。
調試(debugging): 發現和解決程序中出現的3類錯誤的過程。
語法(syntax): 程序的結構。
語法錯誤(syntax error): 程序中的一種錯誤，導致它無法進行語法解析(因此也無法被解釋器執行)。
異常(exception): 程序運行中發現的錯誤。
語義(semantics): 程序表達的意義。
語義錯誤(semantic error): 程序中的一種錯誤，導致它運行所做的事情和程序員設想的不同。
自然語言(natural language): 自然演化而來的人們所說的語言。
形式語言(formal language): 人們設計來用於某些特定目的的語言，例如表達數學概念或者計算機程序。所有的編程語言都屬於形式語言。
記號(token): 程序的語法結構的最基本單位，類似於自然語言中的詞。
語法分析(parse): 檢查程序並分析其語法結構。
print語句(print statement): 一個指令，可以通知Python 解釋器在屏幕上輸出一個值。