* 單元測試

  測試函數、方法等最小單元的測試。這個等級的測試能明確看到輸入和輸出，所以測試內容往往就是函數或方法的設計方案本身。該部分要利用mock 或dummy，把測試對象的處理單獨拿出來執行，看結果是否達到預期。
  
* 組件集成測試

  這是集成多個函數或方法的輸入輸出的測試，測試時需要將多個測試對象組合在一起。由單個測試對象構成的流程已在單元測試中測試完畢，所以不參與這一步測試。對象的前後處理與單元測試一樣要使用mock 或dummy。
  
* 功能單元測試

  測試用戶能看得到的功能。此時用戶的輸入項目以及數據庫等外部系統爲輸入的來源。輸出則是向用戶顯示的結果、向數據庫保存的內容以及對外部系統的調用。系統內部不使用mock 和dummy，而是全部使用正式的代碼。不過，在對付某些異步調用之類的難以自動測試的部分時，需要進行一定程度的置換。外部系統方面，要準備好虛擬的SMTP服務器或Web API服務器，用以執行應用內的通信。
  
* 功能集成測試

  集成各功能之間輸入輸出的測試。這裏要盡可能不去直接查看數據庫內的數據，比如可以用引用類功能來顯示更新類功能生成的數據。另外在外部系統的聯動方面，要借助開發專用的API等模擬出正式運行時的結構，然後再進行測試。這部分測試要依賴與數據庫以及外部服務等諸多環境，難以自動執行，所以屬於偏手動的測試。
  
* 系統測試

  對需求的測試。測試成品是否最終滿足了所有需求。在客戶驗收項目時進行。
  
* 非功能測試

  對性能、安全等非功能方面進行的測試。借助壓力測試軟件進行正常/高峯/極限情況的測試，通過XSS、CSRF以及注入式等模擬攻擊來驗證系統的安全性及可靠性。