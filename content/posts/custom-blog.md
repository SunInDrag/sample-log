---
title: 自定義更改紀錄
tags:
  - Hugo
categories: hugo
date: 2025-5-17 23:24:57
draft: false
---
- [ ] **最後上傳時間**
   一直不能成功...我之後再試試看吧

- [x] archieve 不需要index.md即可使用
   
- [x] code block 會被縮進、代碼高亮

   ```python
   def choose(choice):
       try:
           function = actions[choice]
           try:
               function()
           except Exception as e:
               print(str(e))
       except KeyError as e:
           print(strings.PROMPT_INVALID_ACTION)
   ```

   ```javascript
   loop(food = 0; foodNeeded = 10) {
     if (food = foodNeeded) {
       exit loop;
       // We have enough food; let's go home
     } else {
       food += 2; // Spend an hour collecting 2 more food
       // loop will then run again
     }
   }
   ```

   ```c#
   using System;
   using System.Collections.Generic;
   using System.Linq;
   using System.Windows.Forms;
   
   namespace PDFQFZ
   {
       static class Program
       {
           [STAThread]
           static void Main(string[] args)
           {
               Application.EnableVisualStyles();
               Application.SetCompatibleTextRenderingDefault(false);
               Application.Run(new Form1(args));
           }
       }
   }
   ```

   
