# Example: Why the loop variable should be of signed type

### Code
    
    
    program project1;
    
    uses Classes;
    
    procedure Test_SignedType(AList: TStrings);
    var
      i, Last_AsShortInt: ShortInt;
    begin
      WriteLn('Test with signed type');
      WriteLn('=====================');
      WriteLn;
    
      // Test with 5 items (signed)
    
      AList.CommaText := 'One,Two,Three,Four,Five';
      Last_AsShortInt := AList.Count-1;
    
      WriteLn('  5 items:');
      WriteLn('  for i:=0 to ', Last_AsShortInt);
      WriteLn('  -------------');
    
      for i:=0 to AList.Count-1 do
        WriteLn(i:3, ':  ', AList[i]);
    
      WriteLn('  -------------');
      WriteLn;
    
      // Test with empty list (signed)
    
      AList.Clear;
      Last_AsShortInt := AList.Count-1;
    
      WriteLn('  Empty list:');
      WriteLn('  for i:=0 to ', Last_AsShortInt);
      WriteLn('  --------------');
    
      for i:=0 to AList.Count-1 do
        WriteLn(i:3, ':  ', AList[i]);
    
      WriteLn('  --------------');
    end;
    
    
    procedure Test_UnsignedType(AList: TStrings);
    var
      i, Last_AsByte: Byte;
    begin
      WriteLn('Test with unsigned type');
      WriteLn('=======================');
      WriteLn;
    
      // Test with 5 items (unsigned)
    
      AList.CommaText := 'One,Two,Three,Four,Five';
      Last_AsByte := AList.Count-1;
    
      WriteLn('  5 items:');
      WriteLn('  for i:=0 to ', Last_AsByte);
      WriteLn('  -------------');
    
      for i:=0 to AList.Count-1 do
        WriteLn(i:3, ':  ', AList[i]);
    
      WriteLn('  -------------');
      WriteLn;
    
      // Test with empty list (unsigned)
    
      AList.Clear;
      Last_AsByte := AList.Count-1;
    
      WriteLn('  Empty list:');
      WriteLn('  for i:=0 to ', Last_AsByte);
      WriteLn('  ---------------');
    
      for i:=0 to AList.Count-1 do
        WriteLn(i:3, ':  '{, AList[i]});  // exception if uncomment
    
      WriteLn('-----------------');
    end;
    
    
    procedure Pause;
    begin
      WriteLn;
      Write('...');
      ReadLn;
      WriteLn;
    end;
    
    
    var
      List: TStringList;
    
    begin
      List := TStringList.Create;
      try
        Test_SignedType(List);
        Pause;
        Test_UnsignedType(List);;
        Pause;
      finally
        List.Free;
      end;
    end.
    

### Output
    
    
    Test with signed type
    =====================
    
      5 items:
      for i:=0 to 4
      -------------
      0:  One
      1:  Two
      2:  Three
      3:  Four
      4:  Five
      -------------
    
      Empty list:
      for i:=0 to -1
      --------------
      --------------
    
    ...
    
    Test with unsigned type
    =======================
    
      5 items:
      for i:=0 to 4
      -------------
      0:  One
      1:  Two
      2:  Three
      3:  Four
      4:  Five
      -------------
    
      Empty list:
      for i:=0 to 255
      ---------------
      0:
      1:
      2:
      3:
      4:
      5:
      6:
      7:
      8:
      9:
     10:
     11:
     12:
     13:
     14:
     15:
     16:
     17:
     18:
     19:
     20:
     21:
     22:
     23:
     24:
     25:
     26:
     27:
     28:
     29:
     30:
     31:
     32:
     33:
     34:
     35:
     36:
     37:
     38:
     39:
     40:
     41:
     42:
     43:
     44:
     45:
     46:
     47:
     48:
     49:
     50:
     51:
     52:
     53:
     54:
     55:
     56:
     57:
     58:
     59:
     60:
     61:
     62:
     63:
     64:
     65:
     66:
     67:
     68:
     69:
     70:
     71:
     72:
     73:
     74:
     75:
     76:
     77:
     78:
     79:
     80:
     81:
     82:
     83:
     84:
     85:
     86:
     87:
     88:
     89:
     90:
     91:
     92:
     93:
     94:
     95:
     96:
     97:
     98:
     99:
    100:
    101:
    102:
    103:
    104:
    105:
    106:
    107:
    108:
    109:
    110:
    111:
    112:
    113:
    114:
    115:
    116:
    117:
    118:
    119:
    120:
    121:
    122:
    123:
    124:
    125:
    126:
    127:
    128:
    129:
    130:
    131:
    132:
    133:
    134:
    135:
    136:
    137:
    138:
    139:
    140:
    141:
    142:
    143:
    144:
    145:
    146:
    147:
    148:
    149:
    150:
    151:
    152:
    153:
    154:
    155:
    156:
    157:
    158:
    159:
    160:
    161:
    162:
    163:
    164:
    165:
    166:
    167:
    168:
    169:
    170:
    171:
    172:
    173:
    174:
    175:
    176:
    177:
    178:
    179:
    180:
    181:
    182:
    183:
    184:
    185:
    186:
    187:
    188:
    189:
    190:
    191:
    192:
    193:
    194:
    195:
    196:
    197:
    198:
    199:
    200:
    201:
    202:
    203:
    204:
    205:
    206:
    207:
    208:
    209:
    210:
    211:
    212:
    213:
    214:
    215:
    216:
    217:
    218:
    219:
    220:
    221:
    222:
    223:
    224:
    225:
    226:
    227:
    228:
    229:
    230:
    231:
    232:
    233:
    234:
    235:
    236:
    237:
    238:
    239:
    240:
    241:
    242:
    243:
    244:
    245:
    246:
    247:
    248:
    249:
    250:
    251:
    252:
    253:
    254:
    255:
    -----------------
    
    ...
    

## see also

  * [`{$rangechecks}` compiler directive](</index.php?title=sRangechecks&action=edit&redlink=1> "sRangechecks \(page does not exist\)")

---

_Source: [https://wiki.freepascal.org/Example%3A_Why_the_loop_variable_should_be_of_signed_type](https://web.archive.org/web/20250114160037/https://wiki.freepascal.org/Example%3A_Why_the_loop_variable_should_be_of_signed_type)_
