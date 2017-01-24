# Base64
## Rule
1. 將3 bytes的資料分為一組，按其ASCII值填入24 bit的緩衝區
1. 每次取6 bits，按「A-Za-z0-9+/」(共64項)取值做為輸出
1. 若原資料不是3的倍數時，不足1個補「=」，不足2個補「==」

## Flow
將「GPU」使用base64加密
<table>
  <tr>
    <th>Char</th>
    <td colspan="8" align="center">G</td>
    <td colspan="8" align="center">P</td>
    <td colspan="8" align="center">U</td>
  </tr><tr>
    <th>ASCII</th>
    <td colspan="8" align="center">0x47</td>
    <td colspan="8" align="center">0x50</td>
    <td colspan="8" align="center">0x55</td>
  </tr><tr>
    <th>Bit</th>
    <td>0</td><td>1</td><td>0</td><td>0</td><td>0</td><td>1</td><td>1</td><td>1</td>
    <td>0</td><td>1</td><td>0</td><td>1</td><td>0</td><td>0</td><td>0</td><td>0</td>
    <td>0</td><td>1</td><td>0</td><td>1</td><td>0</td><td>1</td><td>0</td><td>1</td>
  </tr><tr>
    <th>Index</th>
    <td colspan="6" align="center">17</td>
    <td colspan="6" align="center">53</td>
    <td colspan="6" align="center">1</td>
    <td colspan="6" align="center">21</td>
  </tr><tr>
    <th>Base64</th>
    <td colspan="6" align="center">R</td>
    <td colspan="6" align="center">1</td>
    <td colspan="6" align="center">B</td>
    <td colspan="6" align="center">V</td>
  </tr>
</table>

## Example
OpenSSL參數
* -in/-out: 輸入檔案/輸出檔案
* -a = -base64
* -d: decrypt，解密
```console
# echo -n "GPU" > plain.txt
# openssl enc -a -in plain.txt -out encrypt.txt
# openssl enc -d -a -in encrypt.txt -out decrypt.txt
# cat encrypt.txt
R1BV
# diff plain.txt decrypt.txt
# 
```
可以再驗證G、GP的編碼
```console
# echo -n "G" > plain.txt
# openssl enc -a -in plain.txt -out encrypt.txt
# cat encrypt.txt
Rw==
# echo -n "GP" > plain.txt
# openssl enc -a -in plain.txt -out encrypt.txt
# cat encrypt.txt
R1A=
```
