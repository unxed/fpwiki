# Dialog Examples

│ **[Deutsch (de)](</Dialog_Examples/de> "Dialog Examples/de")** │  **[English (en)](<../en/Dialog_Examples.md> "Dialog Examples")** │  **[español (es)](</Dialog_Examples/es> "Dialog Examples/es")** │  **[suomi (fi)](</Dialog_Examples/fi> "Dialog Examples/fi")** │  **[français (fr)](</Dialog_Examples/fr> "Dialog Examples/fr")** │  **[日本語 (ja)](</Dialog_Examples/ja> "Dialog Examples/ja")** │  **[polski (pl)](</Dialog_Examples/pl> "Dialog Examples/pl")** │  **русский (ru)** │  **[slovenčina (sk)](</Dialog_Examples/sk> "Dialog Examples/sk")** │  **[中文（中国大陆） (zh_CN)](</Dialog_Examples/zh_CN> "Dialog Examples/zh CN")** │    
****

## Contents

  * 1 Несколько полезных диалогов
    * 1.1 Диалоги сообщений
      * 1.1.1 ShowMessage
      * 1.1.2 MessageBox
      * 1.1.3 MessageDLG
      * 1.1.4 QuestionDlg
    * 1.2 Диалоги ввода текста
      * 1.2.1 InputBox
      * 1.2.2 InputQuery
      * 1.2.3 PasswordBox
    * 1.3 Константы и типы, используемые в диалогах сообщений



# Несколько полезных диалогов

Вот некоторые полезные диалоги, которых нет в [палитре компонентов](<Component_Palette.md> "Component Palette/ru"): 

  * `procedure ShowMessage (const Msg: string);`
  * `function MessageBox (Text, Caption : PChar; Flags: Word): Integer;`
  * `function MessageDlg (const Msg: string; AType: TMsgDlgType; AButtons: TMsgDlgButtons; HelpCtx: LongInt): Word;`
  * `function InputBox (const ACaption, APrompt, ADefault: string); string;`
  * `function InputQuery (const ACaption, APrompt: string; var Value: string): Boolean;`
  * `function PasswordBox(const ACaption, APrompt : String) : String;`



Каждый из этих компонентов приводит к отображению небольшого всплывающего окна, которое содержит некоторую информацию и требует ответа пользователя: либо нажатия кнопки, либо ввода текста, либо и то и другое. Программист практически не контролирует формат, размер или положение этих всплывающих окон, но может влиять на их текстовое содержимое. 

Причина, по которой часто существует несколько очень похожих альтернатив, заключается в том, что разные методы могут вызывать компонент и получать данные обратно из процедуры или функции. Эти диалоги могут быть «платформозависимыми», то есть они могут отображаться по-разному. Например, строки, которые полностью отображаются в Windows XP, могут быть усечены в Windows 7. 

## Диалоги сообщений

Диалоги сообщений отображают сообщение и ждут реакции [пользователя в виде] нажатия клавиши или щелчка мыши. 

### ShowMessage
    
    
    procedure ShowMessage(const Msg: string);
    

{ Определено в Dialogs.pp } 

Простейшее диалоговое окно сообщения: принимает в качестве параметра простую строку, отображает ее в стереотипном поле и ожидает события щелчка мышью или клавиши ввода, прежде чем вернуться к вызывающей [подпрограмме](<Routine.md> "Routine/ru") или [программе](<Program.md> "Program/ru"). 

Это модальный вызов процедуры, то есть окно отображается, получает фокус и не оставляет фокус, пока не будет нажата кнопка ОК или не выбрано что-либо иное. 

Пример: 
    
    
    program LazMessage;
    uses 
      Dialogs;
    begin
      ShowMessage('Это сообщение от Lazarus');
    end.
    

У вас есть возможность создавать многострочные сообщения, используя следующие разделители строк, все они будут работать: 

  * sLineBreak
  * LineEnding
  * или символьный код: #13#10(Windows)/#10(Linux)/#13(macOS)



**Пример из нескольких строк:**
    
    
    program LazMessage;
    uses 
      Dialogs;
    begin
      ShowMessage('Это - многострочное' + sLineBreak + 'сообщение!' );
    end.
    

### MessageBox
    
    
    { Определено в Forms.pp как часть TApplication; следовательно, должно вызываться как Application.Messagebox() 
    или с использованием конструкции 'with Application do ...'}
    
    function Application.MessageBox(Text, Caption: PChar; Flags: LongInt): Integer;
    

Параметры включают 

  * Text: строка, которая отображается как приглашение или инструкция в поле;
  * Caption: строка заголовка в верхней части окна сообщения;
  * Flags: longint - целое число, построенное путем сложения различных констант, для определения содержимого и поведения блока, например, MB_ABORTRETRYIGNORE + MR_ICONQUESTION заставит приложение отображать значок запроса (?) в поле с тремя кнопками: ABORT RETRY IGNORE.



Функция возвращает целочисленное значение, соответствующее нажатой кнопке; его значение может быть определено с помощью констант[IDOK..IDHELP] Она может быть вызвана как вызов процедуры (то есть как оператор 'MessageBox()', а не как вызов функции 'Variable:= MessageBox()' - см. Пример ниже) 

Пример: 
    
    
    uses 
      Forms, Dialogs, LCLType;
     
    procedure DisplayMessageBox;
    var 
      Reply, BoxStyle: Integer;
    begin
      BoxStyle := MB_ICONQUESTION + MB_YESNO;
      Reply := Application.MessageBox('Press either button', 'MessageBoxDemo', BoxStyle);
      if Reply = IDYES then Application.MessageBox('Yes       ', 'Reply',MB_ICONINFORMATION)
        else Application.MessageBox('No         ', 'Reply', MB_ICONHAND);
    end;
    

Обратите внимание, что в этом примере строки 'Yes' и 'No' были дополнены пробелами; в противном случае поле не будет достаточно широким, чтобы правильно отображать заголовок 

[![MessageBoxDemo.png](https://wiki.freepascal.org/images/c/c2/MessageBoxDemo.png)](</File:MessageBoxDemo.png>) [![ReplyYes.png](https://wiki.freepascal.org/images/0/06/ReplyYes.png)](</File:ReplyYes.png>)

### MessageDLG
    
    
    function MessageDlg(const aMsg: string; DlgType: TMsgDlgType; Buttons: TMsgDlgButtons; HelpCtx: Longint): Integer;
    function MessageDlg(const aCaption, aMsg: string; DlgType: TMsgDlgType; Buttons: TMsgDlgButtons; HelpCtx: Longint): Integer;
    

Две версии этой функции, то есть первый параметр 'Caption' является необязательным; если пропущено, заголовок отсутствует в поле 

Это наиболее полный и продуманный диалог сообщений, который позволяет программисту значительно контролировать внешний вид диалогового окна. Параметры, определяющие тип поля и его значка, являются типами, а не целочисленными константами, и кнопки можно указывать в виде набора в квадратных скобках, например [mbRetry, mbIgnore, mbAbort, mbCancel]. Параметр HelpCtx в настоящее время не реализован и должен быть установлен в ноль. Возвращаемое значение из функции - это идентификатор нажатой кнопки, выраженный в виде целого числа (см. определения ниже, [mrNone..mrAll]). 

Пример: 
    
    
    uses 
      Forms, Dialogs, LCLType, Controls;
     
    procedure TryMessageDlg;
    begin
      if MessageDlg('Question', 'Do you wish to Execute?', mtConfirmation, [mbYes, mbNo, mbIgnore],0) = mrYes
      then { выполнить оставшуюся часть программы };
    end;
    

[![Question.png](https://wiki.freepascal.org/images/3/30/Question.png)](</File:Question.png>)

### QuestionDlg

Диалог вопросов позволяет изменять заголовки кнопок и устанавливать кнопки по умолчанию и отмены. 

Пример: 
    
    
        case QuestionDlg ('Caption','Message',mtCustom,[mrYes,'Positive', mrNo, 'Negative', 'IsDefault'],'') of
            mrYes: QuestionDlg ('Caption','So you mean „Yes“',mtCustom,[mrOK,'That is right'],'');
            mrNo:  QuestionDlg ('Caption','Oh, you mean „No“',mtCustom,[mrOK,'Exactly'],'');
            mrCancel: QuestionDlg ('Caption','You canceled the dialog with ESC or close button.',mtCustom,[mrOK,'Exactly'],'');
        end;
    

[![QuestionDlg.png](https://wiki.freepascal.org/images/a/ac/QuestionDlg.png)](</File:QuestionDlg.png>)

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Если пользователь отменяет диалог, и кнопка 'IsCancel' не определена, возвращается mrCancel! Если вы хотите перенаправить ESC/закрыть на определенную кнопку, используйте параметр 'IsCancel' сразу после кнопки отмены

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Использование букв местного алфавита в типе UnicodeString в качестве заголовков кнопок может привести к ошибкам (замечено в V1.6.4).

Расширенные примеры для кнопок (из файла promptdialog.inc) 
    
    
        [mrOk,mrCancel,'Cancel now',mrIgnore,300,'Do it','IsDefault']
    

Это приведет к отображению 4 кнопок: 

  * 'Ok' возвращает mrOk
  * 'Cancel now' возвращает mrCancel
  * 'Ignore' возвращает mrIgnore
  * 'Do it' возвращает 300. Это будет кнопка по умолчанию (в фокусе)



* * *

[Прим.переводчика:](</User:Zoltanleo> "User:Zoltanleo") для Darwin имеет значение порядок указания кнопок диалога. Этот код: 
    
    
      QuestionDlg('Caption',
                  'Message',
                  mtInformation,
                  [mrOK,
                  mrCancel,
                  mrYes,
                  mrNo],
                  0);
    

даст такое расположение кнопок диалога 

[![scr 084.png](https://wiki.freepascal.org/images/b/bc/scr_084.png)](</File:scr_084.png>)

а вот эти изменения кода 
    
    
      QuestionDlg('Caption',
                  'Message',
                  mtInformation,
                  [mrCancel,
                  mrOK,
                  mrNo,
                  mrYes],
                  0);
    

поменяют порядок расположения на более привычный 

[![scr 085.png](https://wiki.freepascal.org/images/0/0c/scr_085.png)](</File:scr_085.png>)

* * *

Обычно кнопки в диалогах Lazarus имеют значки. Для предотвращения отображения значков можно использовать нестандартизированные значения для результатов сообщения. В настоящее время наибольшее стандартизированное значение равно 11. Например: 
    
    
      case QuestionDlg ('Caption','Message',mtCustom,[20,'Positive', 21, 'Negative',22,'I do not know','IsCancel'],'') of
         20: QuestionDlg ('Caption','So you mean „Yes“',mtCustom,[20,'That is right'],'');
         21:  QuestionDlg ('Caption','Oh, you mean „No“',mtCustom,[21,'Exactly'],'');
         22:  QuestionDlg ('Caption','So, please find out!',mtCustom,[22,'Maybe'],'');
      end;
    

Для облегчения работы можно определить константы. Например: 
    
    
    const
      mrNoneNI=      20;
      mrOkNI=        mrNoneNI+1;
      mrCancelNI=    mrNoneNI+2;
      mrAbortNI=     mrNoneNI+3;
      mrRetryNI=     mrNoneNI+4;
      mrIgnoreNI=    mrNoneNI+5;
      mrYesNI=       mrNoneNI+6;
      mrNoNI=        mrNoneNI+7;
      mrAllNI=       mrNoneNI+8;
      mrYesToAllNI=  mrNoneNI+10;
      mrCloseNI=     mrNoneNI+11;
      mrLastNI=      mrCloseNI; 
    begin
      case QuestionDlg ('Caption','Message',mtCustom,[mrYesNI,'Positive', mrNoNI, 'Negative',mrCancelNI,'I do not know','IsCancel'],'') of
        mrYesNI: QuestionDlg ('Caption','So you mean „Yes“',mtCustom,[mrYesNI,'That is right'],'');
        mrNoNI:  QuestionDlg ('Caption','Oh, you mean „No“',mtCustom,[mrNoNI,'Exactly'],'');
        mrCancelNI:  QuestionDlg ('Caption','So, please find out!',mtCustom,[mrCancelNI,'Maybe'],'');
      end; //case
    end;
    

[![QuestionDlgNoButIcons.png](https://wiki.freepascal.org/images/1/15/QuestionDlgNoButIcons.png)](</File:QuestionDlgNoButIcons.png>)

* * *

[Прим.переводчика:](</User:Zoltanleo> "User:Zoltanleo") Отображение значков на кнопках диалоговых окон зависит от системных настроек. Можно попытаться ими управлять из своего приложения, не меняя глобальных настроек, при помощи свойства [TApplicationShowGlyphs](<https://dsiders.gitlab.io/lazdocsnext/lcl/forms/tapplicationshowglyphs.html>) компонента [TApplicationProperties](<TApplicationProperties.md> "TApplicationProperties/ru"). Но, как говорится, "есть нюансы" :) 

  * Windows 
    * для Win XP отображение значков _**зависит от**_ значений этого свойства
    * для Win 7 и Win 10 значки _**не отображаются никогда**_ вне зависимости от значений этого свойства
    * для Win 11 не проверял


  * Linux (проверялось для Debian GTK2 и Qt5) 
    * значки _**отображаются всегда**_ вне зависимости от значений этого свойства



[![scr 086.png](https://wiki.freepascal.org/images/c/c5/scr_086.png)](</File:scr_086.png>)

[![scr 087.png](https://wiki.freepascal.org/images/3/3a/scr_087.png)](</File:scr_087.png>)

  * Darwin 
    * отображение значков _**зависит от**_ значений этого свойства



[![scr 081.png](https://wiki.freepascal.org/images/0/0e/scr_081.png)](</File:scr_081.png>)

[![scr 082.png](https://wiki.freepascal.org/images/a/a7/scr_082.png)](</File:scr_082.png>)

[![scr 083.png](https://wiki.freepascal.org/images/2/22/scr_083.png)](</File:scr_083.png>)

## Диалоги ввода текста

### InputBox

Диалоги ввода текста: отображение сообщения и ожидание ввода текста пользователем 
    
    
    function InputBox(const ACaption, APrompt, ADefault: String): String;
    

Отображает поле с определенным заголовком и приглашением, и ожидает ввода пользователя в текстовом поле. По умолчанию строка может быть отображена в текстовом поле. Введенная пользователем строка или строка по умолчанию возвращается как результат функции. 

Пример: 
    
    
    uses 
      Forms, LCLType, Dialogs, Controls;
     
    procedure TryInputBox;
    var 
      UserString: string;
    begin
      UserString := InputBox('Get some text input','Please type in some information', 'Some sample text');
      ShowMessage(UserString)
    end;
    

### InputQuery
    
    
    function InputQuery(const ACaption, APrompt : String; MaskInput : Boolean; var Value : String) : Boolean;
    function InputQuery(const ACaption, APrompt : String; var Value : String) : Boolean;
    

Две версии этой функции, которая отображает подсказку и ожидает ввода пользователем текстовых данных; первый включает в себя логический параметр MaskInput, который определяет, маскируется ли пользовательский ввод звездочками в поле ввода текста (например, при вводе пароля), а второй пропускает это свойство. Текст, введенный пользователем, возвращается в переменном параметре 'Value'(Значение); результатом функции является логическое значение, которое возвращает TRUE, если была нажата кнопка OK, или FALSE, если окно было закрыто любым другим механизмом (например, нажатием на значок 'Close'(Закрыть) в верхней строке заголовка). Пропуск параметра MaskInput эквивалентен установке его в значение FALSE. 

Пример: 
    
    
    uses 
      Forms, LCLType, Dialogs, Controls;
     
    procedure TryInputQuery;
    var 
      QueryResult: Boolean;
      UserString: string;
    begin
      if InputQuery('Question', 'Type in some data', TRUE, UserString)
      then ShowMessage(UserString)
      else 
      begin
        InputQuery('Don''t be silly', 'Please try again', UserString);
        ShowMessage(UserString);
      end
    end;
    

[![MessageDlgQuestion.png](https://wiki.freepascal.org/images/d/d0/MessageDlgQuestion.png)](</File:MessageDlgQuestion.png>) [![DontBeSillly.png](https://wiki.freepascal.org/images/c/ce/DontBeSillly.png)](</File:DontBeSillly.png>)

### PasswordBox
    
    
    Function PasswordBox(const ACaption, APrompt : String) : String;
    

Ведет себя очень похоже на функцию InputQuery с MaskInput = TRUE; разница в том, что введенный пароль возвращается как результат функции (как в InputBox). 

## Константы и типы, используемые в диалогах сообщений

Несколько констант и типов, относящихся к использованию с диалоговыми окнами, предварительно определены в библиотеке LCL: 

целочисленные константы для определения типов кнопок и иконки для отображения в MessageBox 
    
    
    const { Defined in LCLType.pp }
    
    MB_OK = $00000000;
    MB_OKCANCEL = $00000001;
    MB_ABORTRETRYIGNORE = $00000002;
    MB_YESNOCANCEL = $00000003;
    MB_YESNO = $00000004;
    MB_RETRYCANCEL = $00000005;
    
     
    MB_ICONHAND = $00000010;
    MB_ICONQUESTION = $00000020;
    MB_ICONEXCLAMATION = $00000030;
    MB_ICONASTERICK = $00000040;
    MB_ICONWARNING = MB_ICONEXCLAMATION;
    MB_ICONERROR = MB_ICONHAND;
    MB_ICONINFORMATION = MB_ICONASTERICK;
    

целочисленные константы, определяющие возвращаемое значение из MessageBox в зависимости от того, какая кнопка была нажата 
    
    
    IDOK = 1; 	ID_OK = IDOK;
    IDCANCEL = 2;	ID_CANCEL = IDCANCEL;
    IDABORT = 3;	ID_ABORT = IDABORT;
    IDRETRY = 4;	ID_RETRY = IDRETRY;
    IDIGNORE = 5;	ID_IGNORE = IDIGNORE;
    IDYES = 6;	ID_YES = IDYES;
    IDNO = 7;	ID_NO = IDNO;
    IDCLOSE = 8;	ID_CLOSE = IDCLOSE;
    IDHELP = 9;	ID_HELP = IDHELP;
    

определяет, будет ли первая, вторая или третья кнопка по умолчанию 
    
    
    MB_DEFBUTTON1 = $00000000;
    MB_DEFBUTTON2 = $00000100;
    MB_DEFBUTTON3 = $00000200;
    MB_DEFBUTTON4 = $00000300;
    

Параметр Flags MessageBox создается путем добавления константы кнопки [MB_OK..MB_RETRYCANCEL], необязательной константы значка [MB_ICONHAND..MB_ICONINFORMATION] и необязательной постоянной кнопки по умолчанию [MB_DEFBUTTON1..MB_DEFBUTTON3] 

Типы для использования в MessageDlg, для которого требуются параметры AType TMsgDlgType и AButtons of TMSgDlgButtons 
    
    
    { Defined in Dialogs.pp }
    type
      TMsgDlgType    = (mtWarning, mtError, mtInformation,  mtConfirmation,
                        mtCustom);
      TMsgDlgBtn     = (mbYes, mbNo, mbOK, mbCancel, mbAbort, mbRetry, mbIgnore,
                       mbAll, mbNoToAll, mbYesToAll, mbHelp, mbClose);
      TMsgDlgButtons = set of TMsgDlgBtn;
      
     
    const
      mbYesNoCancel = [mbYes, mbNo, mbCancel];
      mbOKCancel = [mbOK, mbCancel];
      mbAbortRetryIgnore = [mbAbort, mbRetry, mbIgnore];
      
     
      MsgDlgBtnToBitBtnKind: array[TMsgDlgBtn] of TBitBtnKind = (
       bkYes, bkNo, bkOK, bkCancel, bkAbort, bkRetry, bkIgnore,
        bkAll, bkNoToAll, bkYesToAll, bkHelp, bkClose
       );
     
     
      BitBtnKindToMsgDlgBtn: array[TBitBtnKind] of TMsgDlgBtn = (
        mbOk, mbOK, mbCancel, mbHelp, mbYes, mbNo,
        mbClose, mbAbort, mbRetry, mbIgnore, mbAll, mbNoToALl, mbYesToAll
        );
     
     
    { Defined in Controls.pp }
    const
      mrNone = 0;
      mrOK = mrNone + 1;
      mrCancel = mrNone + 2;
      mrAbort = mrNone + 3;
      mrRetry = mrNone + 4;
      mrIgnore = mrNone + 5;
      mrYes = mrNone + 6;
      mrNo = mrNone + 7;
      mrAll = mrNone + 8;
      mrNoToAll = mrNone + 9;
      mrYesToAll = mrNone + 10;
      mrLast = mrYesToAll;

---

_Source: [https://wiki.freepascal.org/Dialog_Examples/ru](https://web.archive.org/web/20250117085652/https://wiki.freepascal.org/Dialog_Examples/ru)_
